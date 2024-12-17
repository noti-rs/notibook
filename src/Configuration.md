# 3. Configuration

## 3.1 Understanding Noti's Configuration

Noti looks for configuration files in the following locations (in order of priority):

1. `$XDG_CONFIG_HOME/noti/config.toml`
2. `$HOME/.config/noti/config.toml`

### Hot Reload

Noti supports hot-reloading most configuration changes. Simply save your config file, and Noti will automatically apply the new settings.

### Types

Before of all properties, need to understand a few primitive types. The complex types like array or table will be explained in place.

| Type     | Description                                                                                                                                                     |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `bool`   | A boolean value                                                                                                                                                 |
| `u8`     | An unsigned integer of 8 bit                                                                                                                                    |
| `u16`    | An unsigned integer of 16 bit                                                                                                                                   |
| `String` | A string,typically used as an enumeration variant                                                                                                               |
| `[..]`   | Array containing various type. Used as tuple                                                                                                                    |
| `Color`  | A hexadecimal value, which is started by hashtag (#) and wrapped by doubled quotes (defines as string). It can have 3, 6 or 8 symbols, which represents `RGBA`. |
| `Path`   | The path to particular file or directory                                                                                                                        |

### Configuration Structure

The Noti configuration is divided into four main property groups:

- [General](#32-general-settings)
- [Display](#33-display-configuration)
- [Themes](#34-themes)
- [Apps](#35-app-specific-configuration)

Each of them belongs to specific idea. So the reading order is not matter. But we recommend you
to go to through from the first one to the last one.

## 3.2 General Settings

General settings apply to the entire notification system and control global behavior.

| Property name    | Description                                                       | Type                  | Default value |
| :--------------- | :---------------------------------------------------------------- | :-------------------- | :-----------: |
| `font`           | Font                                                              | `String`              |  "Noto Sans"  |
| `width`          | Width of banner frame                                             | `u16`                 |      300      |
| `height`         | Height of banner frame                                            | `u16`                 |      150      |
| `anchor`         | Screen position where notifications appear                        | `String`              |  "top right"  |
| `gap`            | Space size between two banners. Measures in px                    | `u8`                  |      10       |
| `offset`         | Distance from screen edges                                        | `[u8, u8]`            |    [0, 0]     |
| `sorting`        | How notifications are ordered                                     | `String` or `Sorting` |   "default"   |
| `idle_threshold` | Duration that pauses notification timeouts during user inactivity | `String`              |    "5 sec"    |
| `limit`          | Maximum number of notifications on the screen                     | `u8`                  |       0       |

### Font

Accepts the font name, which may or may not be separated by spaces. The application can only use the font name as recognized by the `fc-list` command pattern. Therefore, styled fonts like "Noto Sans Bold" cannot be used directly. However, if possible, the application can internally load the font with the required styles.

Example:

```toml
font = "JetBrainsMono Nerd Font"
```

### Anchor

The anchor of the current monitor for the current window instance, which determines where the notification banners will appear. The possible values are:

```
top-left         top         top-right

left              +              right

bottom-left     bottom    bottom-right
```

Example:

```toml
anchor = "top-right"
```

### Offset

The offset from the edges for the window instance. The first value represents the offset along the `x-axis`, and the second value represents the offset along the `y-axis`.

For example, if you choose the `"bottom-left"` anchor and set an offset of `[5, 10]`, the window instance will be positioned at the bottom-left edge of the current monitor, with a 5-pixel offset from the left edge and a 10-pixel offset from the bottom edge.

Example:

```toml
offset = [10, 10]
```

### Sorting

The property that determines the banner sorting rule. This is particularly useful when you want to position banners with critical urgency at the top or bottom.

You can define a string for ascending sorting by default. However, to sort in descending order, you must define a table:

| Property name | Type     | Default value |
| :------------ | :------- | :-----------: |
| by            | `String` |   "default"   |
| ordering      | `String` |  "ascending"  |

Possible values of the `by` property name:

- "default" (alias to "time")
- "time"
- "id" (means the notification id, simple to "time", but in replacement case
  it stays in old place)
- "urgency"

Possible values of the `ordering` property name:

- "ascending" (also possible short name "asc")
- "descending" (also possible short name "desc")

Example:

```toml
[general.sorting]
by = "id"
ordering = "descending"
```

### Idle Threshold

When `idle_threshold` is set, notifications will not be removed or expired while the user is idle beyond the configured threshold. Once the user is active again, the timeout resumes.
This setting accepts a human-readable duration format (e.g., `"15 minutes"`, `"30s"`).
If set to `"none"`, the idle timeout behavior is disabled.

Example:

```toml
idle_threshold = "5 min"
```

> **WARNING:**
> Changes to the `idle_threshold` setting cannot be applied via hot-reload. To apply a new value, a full restart of the application is required.

### Limit

The maximum number of notifications that can be displayed at once. Once this limit is reached, any new notifications will be queued and shown once the currently displayed ones either time out or are manually dismissed. A value of `0` means there is no limit.

Example:

```toml
limit = 3
```

## 3.3 Display Configuration

The `display` section allows you to customize the visual aspects of notifications.

| Property name                  | Description                                     | Type                                                                    | Default value |
| :----------------------------- | :---------------------------------------------- | :---------------------------------------------------------------------- | :-----------: |
| [layout](./CustomLayout.md)    | Custom layout path                              | `Path`                                                                  |   "default"   |
| [theme](#34-themes)            | Theme name                                      | `String`                                                                |       -       |
| [image](#image)                | Image properties                                | `Image`                                                                 |       -       |
| [padding](#padding-and-margin) | Spacing from the banner's edge to inner content | `u8` or `[u8, u8]` or `[u8, u8, u8]` or `[u8, u8, u8, u8]` or `Spacing` |       0       |
| [border](#border)              | Border properties                               | `Border`                                                                |       -       |
| [text](#text)                  | Text properties (alias for `title` and `body`)  | `Text`                                                                  |       -       |
| [title](#text)                 | Title text properties                           | `Text`                                                                  |       -       |
| [body](#text)                  | Body text properties                            | `Text`                                                                  |       -       |
| [markup](#markup)              | Enables HTML markup                             | `bool`                                                                  |     true      |
| [timeout](#timeout)            | Banner timeout                                  | `u16` or `Timeout`                                                      |       0       |

The `Spacing` table:

| Key        | Short description                                                           | Type |
| :--------- | :-------------------------------------------------------------------------- | :--- |
| top        | Spacing from top                                                            | `u8` |
| right      | Spacing from right                                                          | `u8` |
| bottom     | Spacing from bottom                                                         | `u8` |
| left       | Spacing from left                                                           | `u8` |
| vertical   | Spacing from top and bottom together (incompatible with top or bottom keys) | `u8` |
| horizontal | Spacing from left and right together (incompatible with left or right keys) | `u8` |

Example:

```toml
[display]
[display.image]
max_size = 32,
rounding = 8,

padding = 8
border = { size = 2, radius = 10 }

[display.text]
wrap = true,
ellipsize_at = "end",

[display.timeout]
default = 3000
critical = 0
```

### Padding and Margin

In the context of this application, padding and margin serve distinct purposes, both relating to the spacing around and within elements.

- Padding refers to the space between the content of an element (such as text or an image) and the edges of the element itself. Essentially, padding is the internal spacing, shrinking the area available for the content to fit within the boundaries of the element.
- Margin, on the other hand, is the space between the outer edges of an element and the surrounding elements or the edges of the container. It provides external spacing that separates one element from another.

> **Note:**
> If you're facing an issue where the content, like an image or text, isn't displaying correctly within a banner, the problem may lie in the padding or margin values being set too high. Excessive padding or margin could reduce the space available for the content, causing it to overflow or not fit within the available area.

#### Declaring Padding and Margin Properties

##### CSS-like

If you familiar with CSS, you know that the padding or the margin can be applied in single row, in the TOML config file you can do it using array:

```toml
# Applies vertical and horizontal paddings respectively
padding = [0, 5]

# Applies top, horizontal and bottom paddings respectively
margin = [3, 2, 5]

# Applies top, right, bottom, left paddings respectively
padding = [1, 2, 3, 4]

# All-directional margin
margin = 3
```

##### Explicit

If you prefer a more detailed approach, you can use an explicit syntax where directions are specified as keys: top, bottom, right, and left. You can also use shorthand keys for vertical and horizontal margins or paddings.

```toml
# Sets only top padding
padding = { top = 3 }

# Sets only top and right padding
padding = { top = 5, right = 6 }

# Instead of
# padding = { top = 5, right = 6, bottom = 5 }
# You can write
padding = { vertical = 5, right = 6 }

# Conflicting values will result in an error due to ambiguity:
# padding = { top = 5, vertical = 6 } --- ERROR

# You can apply the same way for margin
margin = { top = 5, horizontal = 10 }

# For all-directional padding or margin
padding = 10
margin = 5
```

### Image

Typically, the notification includes an image or icon, which is displayed on the left side of the banner.

Here's a table of `Image` properties:

| Property name   | Description                                                                                                                                   | Type                                                                    | Default value |
| :-------------- | :-------------------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------- | :-----------: |
| max_size        | Sets the max size for image and resizes it when width or height of image exceeds this value                                                   | `u16`                                                                   |      64       |
| rounding        | Value used to round image corners                                                                                                             | `u16`                                                                   |       0       |
| margin          | Spacing around image. If there is no space for image, the image will be squished.                                                             | `u8` or `[u8, u8]` or `[u8, u8, u8]` or `[u8, u8, u8, u8]` or `Spacing` |       0       |
| resizing_method | Resizing method for image when it exceeds `max_size`. Possible values: `"gaussian"`, `"nearest"`, `"triandle"`, `"catmull-rom"`, `"lanczos3"` | `String`                                                                |  "gaussian"   |

### Border

You can apply border styles to the notification banner, including border size and border radius.

- Border size refers to the width of the stroke outlining the banner. It also reduces the available inner space within the rectangle.
- Border radius defines how rounded the corners of the banner will be.

> **Note:**
> You can find that the behavior of banner rounding is different from other applications.
> The rule is simple: the inner radius is calculated as $radius - size$.
> It means that inner rounding won't draws if border size exceeds the radius.

The `Border` table:

| Key    | Short description                                   | Type | Default value |
| :----- | :-------------------------------------------------- | :--- | :-----------: |
| size   | Width of stroke which is outlines around the banner | `u8` |       0       |
| radius | Border radius for corner rounding                   | `u8` |       0       |

### Text

Currently, the `Noti` application has only a **title** and **body**, but both are treated as `Text`, so they are covered here. Additionally, a new `text` property has been introduced, which can be used for both the title and body at the same time. The recommended approach is to use the `text` property when you want to set the same values for both the title and body. Otherwise, you can define separate values for the title or body. This means that the **title** and **body** properties **inherit** from the `text` property.

Property priority:

1. First, the `title` or `body` properties are used.
2. If any values are missing, the `text` property is checked, and its values are used instead.
3. If any values are still undefined, default values are applied.

The `Text` table:

| Key           | Short description                                                                                                                                                                                   | Type      | Default value |
| :------------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------- | :-----------: |
| wrap          | Sets possibility to line breaking when text overflows a line                                                                                                                                        | `bool`    |     true      |
| ellipsize_at  | Ellipsizes a text if it's totally overflows an area. Possible values: `"end"` and `"middle"`. `"end"` - put ellipsis at end of word, `"middle"` - cut word at some middle of word and puts ellipsis | `String`  |     "end"     |
| style         | Font style. Possible values: `"regular"`, `"bold"`, `"italic"`, `"bold italic"`                                                                                                                     | `String`  |   "regular"   |
| margin        | Text spacing from edges of remaining area                                                                                                                                                           | `Spacing` |       0       |
| justification | Text justification. Possible values: `"left"`, `"right"`, `"center"`, `"space-between"`                                                                                                             | `String`  |    "left"     |
| line_spacing  | Gap between wrapped text lines in `px`                                                                                                                                                              | `u8`      |       0       |
| font_size     | Font size in `px`                                                                                                                                                                                   | `u8`      |      12       |

Example:

```toml
[display.text]
justification = "left"
margin = { left = 15 }
wrap = false
ellipsize_at = "middle"
font_size = 18

[display.title]
style = "bold"

[display.body]
markup = false
```

### Markup

Enables text styling through HTML tags.

For the body, the `markup` property is applied, allowing the use of HTML-like tags, such as:

- `<b>` - bold style
- `<i>` - italic style
- `<u>` - underline style
- `<a href="https://google.com">` - a hyperlink
- `<img src="path/to/image" alt="image description">` - an image embedded in the text

You can disable the `markup` property by setting it to `false`.

### Timeout

The time in milliseconds after which the notification banner will automatically close due to expiration, starting from when it is created.

Additionally, there is an extended timeout configuration based on the urgency of the banners, defined in the `Timeout` table.

The `Timeout` table:

| Key      | Short description                                     | Type  |
| :------- | :---------------------------------------------------- | :---- |
| default  | Set default timeout for all urgency                   | `u16` |
| low      | Override timeout value for 'low' urgency banners      | `u16` |
| normal   | Override timeout value for 'normal' urgency banners   | `u16` |
| critical | Override timeout value for 'critical' urgency banners | `u16` |

> **Note:**
> The value `0` means will never expired.

## 3.4 Themes

Define custom color schemes for different notification urgency levels:

The `Theme` table:

| Key      | Type     | Default value | Short description                        |
| :------- | :------- | :-----------: | :--------------------------------------- |
| low      | `Colors` |       -       | The colors for `low` urgency banner      |
| normal   | `Colors` |       -       | The colors for `normal` urgency banner   |
| critical | `Colors` |       -       | The colors for `critical` urgency banner |

The `Colors` table:

| Key        | Type    |               Default value               | Short description                        |
| :--------- | :------ | :---------------------------------------: | :--------------------------------------- |
| background | `Color` |                 "#FFFFFF"                 | The background color of banner           |
| foreground | `Color` | "#000000" (but for `critical`: "#FF0000") | The foreground color which used for text |
| border     | `Color` | "#000000" (but for `critical`: "#FF0000") | The border stroke color                  |

Example:

```toml
[[theme]]
name = "dark-mode"

[theme.normal]
background = "#1E1E2E"
foreground = "#FFFFFF"
border = "#444444"

[theme.critical]
background = "#FF0000"
foreground = "#FFFFFF"
border = "#000000"
```

## 3.5 App-Specific Configuration

The Noti application includes an app-specific configuration feature, allowing you to override the display table for a particular application.

Here’s how the properties are selected:

- Check if the property is defined in the app-specific configuration.
- If not, use the general display property.
- If it’s still not defined in the general display property, use the default value.

The `App` table:

| Key     | Short description                                    | Type                                   |
| :------ | :--------------------------------------------------- | :------------------------------------- |
| name    | The name of application                              | `String`                               |
| display | The display configuration table for this application | [`Display`](#36-display-configuration) |

Example:

```toml
[[app]]
name = "Telegram Desktop"

[app.display]
theme = "telegram-theme"
padding = 10
border = { size = 2, radius = 12 }

[[app]]
name = "Spotify"

[app.display]
image = { max_size = 64 }
timeout = 2000
```

## 3.6 Importing

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
