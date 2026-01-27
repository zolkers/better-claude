# Turtle Graphics (Python)

## Conventions

Apply clean code principles to graphical programming. Focus on encapsulation, state management, and avoiding global variables. Use functional decomposition to break drawings into reusable parts.

### Structure & Complexity

- Max 3 nested statements (loops/if).
- Max 15 lines per drawing function.
- Max 4 parameters per function -- use a @dataclass or a dict for complex styles.
- **Rule of Return**: Every drawing function must return the turtle to its original starting position and heading (relative "home" for that shape) to ensure predictability.

### Setup & Performance

- Create specific instances: t = turtle.Turtle(). Avoid using global functions like turtle.forward().
- Use screen.tracer(0, 0) for complex or recursive drawings (fractals) to disable animation.
- Always call screen.update() manually when tracer is off.
- Use screen.exitonclick() to prevent the window from closing immediately.

### Naming Conventions

- Turtle instances: Descriptive names (path_finder, drawer, brush).
- Functions: Start with a verb (draw_star, move_to_corner).
- Constants: UPPER_SNAKE_CASE for screen dimensions, colors, and default speeds.

### State Management

- Always use t.penup() before moving to a new starting point.
- Use t.isdown() to check pen state if logic is complex.
- Use t.setheading() for absolute direction instead of t.left() / t.right() when precision is required over multiple steps.
- Restore the turtle's original color/speed at the end of a function if it was modified.

### Best Practices

- No Magic Numbers: Define constants for sizes, angles, and coordinates.
- Separate math: Put complex coordinate calculations into pure functions that return (x, y) tuples, then pass those to the turtle.
- Use if __name__ == "__main__": to wrap the screen setup and main loop.