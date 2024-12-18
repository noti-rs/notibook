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
