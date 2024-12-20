# Configuration

## Understanding Noti's Configuration

Noti looks for configuration files in the following locations (in order of priority):

1. `$XDG_CONFIG_HOME/noti/config.toml`
2. `$HOME/.config/noti/config.toml`

### Hot Reload

Noti supports hot-reloading configuration changes. Simply save your config file, and Noti will automatically apply the new settings.

### Types

Before of all properties, need to understand a few primitive types. The complex types like array or table will be explained in place.

| Type                   | Description                                                                                                   |
| ---------------------- | ------------------------------------------------------------------------------------------------------------- |
| `bool`                 | A boolean value                                                                                               |
| `u8`                   | An unsigned integer of 8 bit                                                                                  |
| `u16`                  | An unsigned integer of 16 bit                                                                                 |
| `String`               | A string,typically used as an enumeration variant                                                             |
| `[...]`                | An array containing various type. Used as tuple                                                               |
| `[T; from..to]`        | An array with elements of type `T`, where the number of elements can vary between `from` and `to` (inclusive) |
| [`Color`](./Colors.md) | A color value. Either string or table                                                                         |
| `Path`                 | The path to particular file or directory                                                                      |

### Configuration Structure

The Noti configuration is divided into four main property groups:

- [General](./General.md)
- [Display](./Display.md)
- [Themes](./Themes.md)
- [Apps](./Apps.md)

Each of them belongs to specific idea. So the reading order is not matter. But we recommend you
to go to through from the first one to the last one.

## Importing

You can import configuration files from other files:

```toml
use = [
    "$XDG_CONFIG_HOME/noti/themes.toml",
    "~/layouts/layout.toml",
    "./apps/*"
]
```

> **Note:**
>
> - Current configuration file takes precedence
> - Avoid importing the same file multiple time
> - Avoid circular dependencies
> - Merge behavior is not guaranteed, so declare configurations carefully
