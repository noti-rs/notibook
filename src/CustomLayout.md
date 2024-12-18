# Custom Layout

The `Noti` application allows users to **customize the layout of notification banners**, giving you complete control over how your notifications are displayed. Whether you want to move an image to the right, swap the title and body, or pin the title to the top, `Noti` makes it possible through custom layout files.

To customize a layout, you define it in a `.noti` file and reference it in your main configuration file.

## Setting Up a Custom Layout

To enable a custom layout, specify its path in your configuration file:

```toml
display.layout = "path/to/your/layout/file.noti"
```

This can be set globally or for specific applications. Once specified, create the layout file and define your custom design.

> **WARNING**:
> When using a custom layout:
>
> - Border, padding, and margin settings are overridden
> - Only general configurations, colors, and non-styling settings remain active

---

## Layout Basics

Layouts in `Noti` are built using **widgets** and **type values**:

### Widgets

Widgets are the building blocks of layouts. They are graphic elements with customizable properties. The available widgets are:

- **FlexContainer**: A container that organizes its child widgets.
- **Text**: Displays text, such as the title or body.
- **Image**: Displays an image.

### Type Values

Type values are configuration options used to adjust widget behavior. Examples include:

- **[Alignment](#alignment)**: Controls horizontal and vertical alignment.
- **[Spacing](#spacing)**: Defines padding or margins.
- **[Border](./ConfigProperties#Border)**: Adds a border around widgets.

---

## Writing a Layout File

### General Syntax

Each layout file must have one root widget, typically a `FlexContainer`. Widgets and type values are declared using a constructor-like syntax:

```noti
WidgetName(
    property_name = value,
    another_property = true,
    nested_property = TypeValueName(
        type_value_property = 5,
    )
)
```

You can include trailing commas for readability. If a widget or type value has default properties, they can be omitted:

```noti
Image()
```

### Adding Child Widgets

To nest widgets, use the `children` property inside a `FlexContainer`:

```noti
FlexContainer() {
    Text(kind = title)
    Image()
}
```

Child widgets are enclosed in curly braces, with whitespace separating them.

---

## Widget Properties

### FlexContainer

The `FlexContainer` organizes its child widgets either horizontally or vertically.

| Property     | Description                                                              | Type        | Default                                               |
| ------------ | ------------------------------------------------------------------------ | ----------- | ----------------------------------------------------- |
| `direction`  | Layout direction (`horizontal` or `vertical`).                           | `Literal`   | -                                                     |
| `max_width`  | Maximum width of the container.                                          | `UInt`      | MAX                                                   |
| `max_height` | Maximum height of the container.                                         | `UInt`      | MAX                                                   |
| `border`     | Border around the container.                                             | `Border`    | Default [border](./ConfigProperties.md#border) values |
| `spacing`    | Spacing between child widgets.                                           | `Spacing`   | Default spacing values                                |
| `alignment`  | Horizontal and vertical alignment of content (`start`, `center`, `end`). | `Alignment` | -                                                     |

### Text

The `Text` widget is used for titles and body text. Specify the kind of text to display.

| Property | Description                                 | Type      | Default |
| -------- | ------------------------------------------- | --------- | ------- |
| `kind`   | Type of text (`title`/`summary` or `body`). | `Literal` | -       |

### Image

The `Image` widget displays an image. It uses properties defined in the main configuration.

---

## Supporting Type Values

### Spacing

Defines padding or margin offsets. Currently, individual values (e.g., `top`, `right`) must be set explicitly.

| Property | Description             | Type   | Default |
| -------- | ----------------------- | ------ | ------- |
| `top`    | Offset from the top.    | `UInt` | 0       |
| `right`  | Offset from the right.  | `UInt` | 0       |
| `bottom` | Offset from the bottom. | `UInt` | 0       |
| `left`   | Offset from the left.   | `UInt` | 0       |

### Alignment

Controls how content is aligned within a widget.

| Property     | Description                                                       | Type      | Default |
| ------------ | ----------------------------------------------------------------- | --------- | ------- |
| `horizontal` | Horizontal alignment (`start`, `end`, `center`, `space-between`). | `Literal` | -       |
| `vertical`   | Vertical alignment (`start`, `end`, `center`, `space-between`).   | `Literal` | -       |
