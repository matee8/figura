# Figura

**2D geometric primitives with backend-agnostic rendering.**

<img src="https://github.com/matee8/figura/raw/main/assets/heart-example.png" width="400" alt="Heart curve example">

# Overview

Figura is a Rust library designed for creating and manipulating 2D geometric
primitives. It provides a robust foundation for geometric calculations and
offers a flexible rendering system that is not tied to a specific backend, with
built-in support for SDL2.

# Features

-   **SDL2 Integration**: Built-in support for hardware-accelerated rendering
    via SDL2.
-   **Parametric Curves**: Facility to create complex and smooth shapes using
    mathematical functions (e.g., circles, epicycloids, heart curves).
-   **Geometric Primitives**:

    -   Circles, Polygons (with point containment checks), Hermite Arcs (for
        smooth interpolation).
    -   Line Segments with support for clipping and intersection detection.

-   **Extensible Architecture**: Core traits allow for the implementation of
    custom renderer backends and new shape types.
-   **Precision Math**: Leverages floating-point arithmetic with considerations
    for precision in geometric calculations.

# Getting Started

## Prerequisites

-   Rust programming language and Cargo (latest stable version recommended)
-   SDL2 development libraries (if using the `sdl2` feature for rendering)

    -   Installation instructions for SDL2 can be found on the
        [SDL2 website](https://wiki.libsdl.org/SDL2/Installation) or via your
        system's package manager (e.g., `apt-get install libsdl2-dev` on
        Debian/Ubuntu, `brew install sdl2` on macOS).

## Installation & Setup

As Figura is not currently published on crates.io, you can integrate it into
your project in one of the following ways:

1.  As a Path Dependency (Recommended for local development).

    If you have cloned the Figura repository locally, you can include it as a
    path dependency in your project's `Cargo.toml`.

    ```toml
    [dependencies]
    figura = { path = "../path/to/figura" } # Adjust the path accordingly
    ```

    If you need SDL2 rendering capabilities, enable the `sdl2` feature:

    ```toml
    [dependencies]
    figura = { path = "../path/to/figura", features = ["sdl2"] }
    ```

2.  As a Git Dependency.

    You can directly depend on the Git repository:

    ```toml
    [dependencies]
    figura = { git = "https://github.com/matee8/figura.git" } # Optionally specify a branch, tag, or commit
    ```

    And with features:

    ```toml
    [dependencies]
    figura = { git = "https://github.com/matee8/figura.git", features = ["sdl2"] }
    ```

3.  Copying Source Code (Not generally recommended for maintainability).

    Alternatively, you could copy the `src` directory of Figura into your own
    project's structure. This is less maintainable if Figura receives updates.

After configuring `Cargo.toml`, Cargo will fetch and compile Figura when you build your project:

```bash
cargo build
```

# Usage

Figura allows for the creation and rendering of various 2D shapes.

## Basic Curve Rendering

The `OneColorCurve` can be used to render parametric curves with a single
color. The following example demonstrates rendering a circle to an SDL2 canvas.

```rust
use figura::{Color, OneColorCurve, Renderable};

// Create a circle parametric curve
let circle = OneColorCurve::new_parametric(
    Color::RED,
    |t| 200.0 * t.cos() + 320.0,
    |t| 200.0 * t.sin() + 240.0,
    0.0,
    2.0 * std::f64::consts::PI,
    None,
)?;

// Render to SDL2 canvas
circle.render(&mut canvas)?;
```

*(Note: Full SDL2 canvas setup is omitted for brevity. Refer to SDL2 examples.)*

## Polygon Operations

The `Polygon` primitive allows for the creation of closed shapes and performing
operations like point containment checks.

```rust
use figura::{Polygon, Point};

// Create a quadrilateral
let polygon = Polygon::new(
    &[
        Point::new(100.0, 100.0),
        Point::new(100.0, 200.0),
        Point::new(200.0, 200.0),
        Point::new(200.0, 100.0),
    ],
    Color::BLUE,
)?;

// Check point containment
assert!(polygon.contains(Point::new(150.0, 150.0)));
```

## Running Examples

Figura comes with several examples demonstrating its capabilities. To run them,
clone the Figura repository and execute the following commands from the root
directory:

| Example       | Description                          | Command                                      |
|---------------|--------------------------------------|----------------------------------------------|
| Circle        | Basic parametric circle rendering    | `cargo run --features sdl2 --example circle` |
| Epicycloid    | Complex parametric curve             | `cargo run --features sdl2 --example epicycloid -- -a 5 -b 3` |
| Heart         | Romantic curve demonstration         | `cargo run --features sdl2 --example heart`  |

## API Overview

### Core Components

-   **`OneColorCurve`**: Represents a parametric curve defined by functions
    `x(t)` and `y(t)`, rendered with a single color.
-   **`Polygon`**: Defines a closed shape from a series of vertices, supporting
    operations like area calculation and point containment.
-   **`OneColorSegment`**: Represents a line segment between two points, with
    support for clipping algorithms.
-   **`HermiteArc`**: Implements a Hermite cubic spline for smooth curve
    interpolation between specified points and tangents.
-   **`Point`**: A fundamental 2D point with `x` and `y` coordinates.
-   **`Color`**: Represents an RGBA color.

### Key Traits

-   **`GeometricPrimitive`**: A base trait intended for all fundamental
    geometric shapes, providing common properties or methods.
-   **`Renderable<T>`**: A trait defining a unified interface for rendering
    shapes to a target `T` (e.g., an SDL2 canvas).
-   **`Shape`**: A trait associated primarily with `Polygon`, providing methods
    for geometric operations and properties (e.g., `contains`, `area`).

## License

This project is licensed under the [MIT License](LICENSE).
