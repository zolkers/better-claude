# Angular

Inherits from `_shared/conventions.md` and `typescript/typescript.md` (Angular is TypeScript-first).

## Detection

This guide applies when `@angular/core` is found in `package.json` dependencies or `angular.json` exists.

## Conventions

### Components

- Use standalone components (Angular 14+) -- avoid NgModules for new code.
- One component per file. File naming: `user-card.component.ts`, `user-card.component.html`, `user-card.component.scss`.
- Keep components under 200 lines -- extract sub-components when growing.
- Use `ChangeDetectionStrategy.OnPush` by default for better performance.
- Prefix component selectors with app or feature name: `app-user-card`, `auth-login-form`.

```typescript
@Component({
  selector: "app-user-card",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./user-card.component.html",
  changeDetection: ChangeDetectionStrategy.OnPush,
})
export class UserCardComponent {
  @Input({ required: true }) user!: User;
  @Output() selected = new EventEmitter<User>();
}
```

### Signals (Angular 16+)

- Use Signals for reactive state -- prefer over RxJS for component state.
- Use `signal()` for writable state, `computed()` for derived state.
- Use `effect()` sparingly -- prefer computed signals or template bindings.
- Use `input()` and `output()` signal-based APIs (Angular 17+) for cleaner syntax.

```typescript
// Modern signal-based component
@Component({
  selector: "app-counter",
  standalone: true,
  template: `
    <button (click)="increment()">{{ count() }} ({{ doubled() }})</button>
  `,
})
export class CounterComponent {
  count = signal(0);
  doubled = computed(() => this.count() * 2);

  increment() {
    this.count.update((c) => c + 1);
  }
}
```

### Templates

- Use control flow syntax (Angular 17+): `@if`, `@for`, `@switch` -- avoid `*ngIf`, `*ngFor`.
- Always use `track` with `@for` loops for performance.
- Use `@defer` for lazy loading heavy components.
- Avoid complex logic in templates -- move to computed signals or component methods.
- Use `ng-container` to group elements without adding DOM nodes.

```html
@if (user()) {
  <app-user-card [user]="user()" />
} @else {
  <app-skeleton />
}

@for (item of items(); track item.id) {
  <app-item [item]="item" />
} @empty {
  <p>No items found.</p>
}
```

### Services & Dependency Injection

- Use `@Injectable({ providedIn: 'root' })` for singleton services.
- Use `inject()` function instead of constructor injection (cleaner, works with standalone).
- Keep services focused -- one responsibility per service.
- Services handle business logic and data fetching, not components.

```typescript
@Injectable({ providedIn: "root" })
export class UserService {
  private http = inject(HttpClient);

  getUsers(): Observable<User[]> {
    return this.http.get<User[]>("/api/users");
  }
}
```

### RxJS

- Use RxJS for async streams (HTTP, WebSockets, complex event handling).
- Prefer Signals for simple component state -- RxJS for complex async flows.
- Always unsubscribe: use `takeUntilDestroyed()`, `async` pipe, or `DestroyRef`.
- Use `toSignal()` and `toObservable()` to bridge between Signals and RxJS.
- Prefer operators over nested subscribes: `switchMap`, `mergeMap`, `concatMap`.

```typescript
export class UserListComponent {
  private userService = inject(UserService);
  private destroyRef = inject(DestroyRef);

  users = toSignal(this.userService.getUsers(), { initialValue: [] });

  // Or with manual subscription
  ngOnInit() {
    this.userService
      .getUsers()
      .pipe(takeUntilDestroyed(this.destroyRef))
      .subscribe((users) => this.users.set(users));
  }
}
```

### Forms

- Use Reactive Forms for complex forms -- Template-driven for simple cases.
- Type your forms with `FormGroup<T>` (Angular 14+ typed forms).
- Validate on blur, show errors after touched.
- Extract form logic into reusable form classes or services for complex forms.

```typescript
interface LoginForm {
  email: FormControl<string>;
  password: FormControl<string>;
}

export class LoginComponent {
  form = new FormGroup<LoginForm>({
    email: new FormControl("", { nonNullable: true, validators: [Validators.required, Validators.email] }),
    password: new FormControl("", { nonNullable: true, validators: [Validators.required, Validators.minLength(8)] }),
  });

  onSubmit() {
    if (this.form.valid) {
      const { email, password } = this.form.getRawValue();
      // ...
    }
  }
}
```

### Routing

- Use lazy loading for feature routes: `loadComponent` or `loadChildren`.
- Use route guards for auth: `canActivate`, `canMatch`.
- Use resolvers sparingly -- prefer loading data in components with skeleton UI.
- Use `withComponentInputBinding()` to bind route params to component inputs.

```typescript
export const routes: Routes = [
  {
    path: "users",
    loadComponent: () => import("./users/user-list.component").then((m) => m.UserListComponent),
  },
  {
    path: "users/:id",
    loadComponent: () => import("./users/user-detail.component").then((m) => m.UserDetailComponent),
  },
];
```

### HTTP

- Use `HttpClient` with typed responses.
- Centralize API calls in services -- never call `HttpClient` directly from components.
- Use interceptors for auth headers, error handling, loading states.
- Handle errors with `catchError` -- show user-friendly messages.

### Performance

- Use `OnPush` change detection everywhere.
- Use `@defer` for below-the-fold content.
- Use `track` in all `@for` loops.
- Lazy load routes and heavy components.
- Use `NgOptimizedImage` for images.
- Avoid function calls in templates -- use computed signals or pipes.

### Testing

- Use `TestBed` for component integration tests.
- Use `jest` or `vitest` instead of Karma/Jasmine (faster).
- Mock services with `jest.fn()` or `jasmine.createSpyObj()`.
- Test component behavior, not implementation details.
- Use `spectator` or `testing-library/angular` for cleaner test syntax.

## Project Structure

```
src/
├── app/
│   ├── core/                    # Singleton services, guards, interceptors
│   │   ├── services/
│   │   │   └── auth.service.ts
│   │   ├── guards/
│   │   │   └── auth.guard.ts
│   │   └── interceptors/
│   │       └── auth.interceptor.ts
│   ├── shared/                  # Shared components, directives, pipes
│   │   ├── components/
│   │   │   ├── button/
│   │   │   └── modal/
│   │   ├── directives/
│   │   └── pipes/
│   ├── features/                # Feature modules/components
│   │   ├── auth/
│   │   │   ├── login/
│   │   │   │   ├── login.component.ts
│   │   │   │   ├── login.component.html
│   │   │   │   └── login.component.scss
│   │   │   └── auth.routes.ts
│   │   └── users/
│   │       ├── user-list/
│   │       ├── user-detail/
│   │       └── users.routes.ts
│   ├── app.component.ts
│   ├── app.config.ts
│   └── app.routes.ts
├── assets/
├── environments/
└── styles/
```

## Questions to Ask

- Angular version (16, 17, 18, 19)?
- Using standalone components or NgModules?
- State management: Signals only, NgRx, or RxJS subjects?
- Styling: SCSS, CSS, Tailwind?
- SSR needed: Angular Universal / SSR?
- Testing: Jest/Vitest or Karma/Jasmine?
- UI library: Angular Material, PrimeNG, Tailwind?
