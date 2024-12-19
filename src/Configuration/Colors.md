# Colors

## Fill

To declare fill color use a string in format `"#RGB"`, `"#RRGGBB"` or `"#RRGGBBAA"`:

```toml
[theme.normal]
background = "#FFF"
foreground = "#A2A2A2"
border = "#BEBEBEAA"
```

## Gradient

When defining gradients, use the same property as you would for a single color, but provide a table value instead.
All gradient configurations share these common properties:

| Key    | Type      | Default value | Short description                        |
| :----- | :-------- | :-----------: | :--------------------------------------- |
| mode   | `String`  |       -       | Specifies the gradient type              |
| degree | `u16`     |       -       | Angle of the gradient in degrees         |
| colors | `Color[]` |       -       | An array of color values (in hex format) |

### Supported gradient types

#### Linear Gradient

Linear gradients create a smooth transition between two or more colors along a straight line.background.
The transition direction is determined by the degree value, where 0° points upward and the angle increases clockwise.

```toml
[theme.normal]
background = { mode = "linear-gradient", degree = 30, colors = ["#F00", "#0F0", "#00F"] }]
```
