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

## Types

The available property types are limited to a few:

- **Literal**: A continuous sequence of characters not enclosed in double quotes, always treated as a string.
- **UInt**: An unsigned integer.
- **[Type value](./LayoutBasics.md#type-value)**: Refer to the linked section for details.

The `Noti` application includes intelligent conversion for `Literal` types, allowing them to represent **boolean**, **color**, **type**, or other supported formats as needed.
