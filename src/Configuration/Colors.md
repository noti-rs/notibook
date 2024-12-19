# Colors

Noti supports various color formats and you can declare with a string which represents single color or a table with extended color configuration.
Here's a list of possible colors:

- [Fill](#fill)
- [LinearGradient](#linear-gradient)

## Fill

To declare fill color use a string in format `"#RGB"`, `"#RRGGBB"` or `"#RRGGBBAA"`:

```toml
[theme.normal]
background = "#FFF"
foreground = "#A2A2A2"
border = "#BEBEBEAA"
```

--- 

## Linear Gradient

Noti now supports gradients, allowing you to define smooth transitions between colors. Gradients are specified using the `mode`, `degree`, and `colors` attributes.

### Configuration

Gradients can be defined inline or using a nested table for clarity. The structure includes:

- **`mode`**: Specifies the gradient type. Currently, only `"linear-gradient"` is supported.
- **`degree`**: Sets the angle of the gradient in degrees.
- **`colors`**: An array of color values (in hex format) that define the gradient.

```toml
[theme.normal]
background = { mode = "linear-gradient", degree = 30, colors = ["#FFF", "#000"] }

[theme.normal.background]
mode = "linear-gradient"
degree = 40
colors = ["#FF00FF", "#00FFFF"]
```

--- 
