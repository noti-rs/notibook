# Display Configuration

The `display` section allows you to customize the visual aspects of notifications.

| Property name                       | Description                                      | Type                              | Default value |
| :---------------------------------- | :----------------------------------------------- | :-------------------------------- | :-----------: |
| [padding](#padding-margin)          | Spacing from the banner's edge to inner content  | `u8` or `[u8; 1..4]` or `Spacing` |       0       |
| [margin](#padding-margin)           | Spacing from the banner's edge to inner content  | `u8` or `[u8; 1..4]` or `Spacing` |       0       |
| [image](#image)                     | Image properties                                 | `Image`                           |       -       |
| [icons](#icons)                     | Icons properties                                 | `Icons`                           |       -       |
| [border](#border)                   | Border properties                                | `Border`                          |       -       |
| [text](#text)                       | Text properties (alias for `summary` and `body`) | `Text`                            |       -       |
| [summary](#text)                    | Summary text properties                          | `Text`                            |       -       |
| [body](#text)                       | Body text properties                             | `Text`                            |       -       |
| [markup](#markup)                   | Enables HTML markup                              | `bool`                            |     true      |
| [timeout](#timeout)                 | Banner timeout                                   | `u16` or `Timeout`                |       0       |
| [layout](../Layout/CustomLayout.md) | Custom layout path                               | `Path`                            |   "default"   |
| [theme](./themes)                   | Theme name                                       | `String`                          |       -       |

## Padding & Margin

In this application, **padding** and **margin** control spacing in different ways:

- **Padding**: The space between an element's content (like text or an image) and its edges. It reduces the area available for the content inside the element.
- **Margin**: The space between an element's outer edges and other surrounding elements. It separates elements from each other.

### Declaring Padding and Margin Properties

#### CSS-like

If you familiar with CSS, you know that the padding or the margin can be applied in single row, in the TOML config file you can do it using array:

```toml
# Applies vertical and horizontal paddings respectively
padding = [0, 5]

# Applies top, horizontal and bottom paddings respectively
margin = [3, 2, 5]

# Applies top, right, bottom, left paddings respectively
padding = [1, 2, 3, 4]

# For all-directional padding or margin
padding = 10
margin = 5
```

#### Explicit

If you prefer a more detailed approach, you can use an explicit syntax with `Spacing` table:

| Key        | Short description                    | Type |
| :--------- | :----------------------------------- | :--- |
| top        | Spacing from top                     | `u8` |
| right      | Spacing from right                   | `u8` |
| bottom     | Spacing from bottom                  | `u8` |
| left       | Spacing from left                    | `u8` |
| vertical   | Spacing from top and bottom together | `u8` |
| horizontal | Spacing from left and right together | `u8` |

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

> **Warning:**
>
> - **horizontal** is incompatible with **left** or **right** keys
> - **vertical** is incompatible with **top** or **bottom** keys
>
> ---
>
> If content (like an image or text) isn't displaying correctly in a banner, check the padding or margin. Excessive padding or margin can reduce the space for content, causing it to overflow or not fit properly.

## Image

Typically, the notification includes an image or icon, which is displayed on the left side of the banner.

Here's a table of `Image` properties:

| Property name   | Description                                                                                                                                   | Type                              | Default value |
| :-------------- | :-------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------- | :-----------: |
| max_size        | Sets the max size for image and resizes it when width or height of image exceeds this value                                                   | `u16`                             |      64       |
| rounding        | Value used to round image corners                                                                                                             | `u16`                             |       0       |
| margin          | Spacing around image. If there is no space for image, the image will be squished.                                                             | `u8` or `[u8; 1..4]` or `Spacing` |       0       |
| resizing_method | Resizing method for image when it exceeds `max_size`. Possible values: `"gaussian"`, `"nearest"`, `"triandle"`, `"catmull-rom"`, `"lanczos3"` | `String`                          |  "gaussian"   |

```toml
[display.image]
max_size = 32
rounding = 10
```

## Icons

If application doesn't provide any image but app-icon, then it will be treated as image.

The `Icons` table:

| Property name | Description                                                                                                                               | Type         | Default value |
| :------------ | :---------------------------------------------------------------------------------------------------------------------------------------- | :----------- | :-----------: |
| theme         | Icon theme like Adwaita, Papirus...                                                                                                       | `Strng`      |   "Adwaita"   |
| size          | Concrete sizes of the icon. Will be picked the first one which found in system resources. Ordering matters - searches from first to last. | `[u16; 1..]` |   [64, 32]    |

```toml
[display.icons]
theme = "Papirus"
size = [64, 32, 16]
```

> **Note**: Since app-icon threated as image, the image properties will be applied to icon, even the `max_size`.

## Border

You can customize the notification banner with border styles, including border size and border radius:

- **Border size**: The width of the border around the banner, which also reduces the inner space.
- **Border radius**: How rounded the corners of the banner are.

The `Border` table:

| Key    | Short description                                   | Type | Default value |
| :----- | :-------------------------------------------------- | :--- | :-----------: |
| size   | Width of stroke which is outlines around the banner | `u8` |       0       |
| radius | Border radius for corner rounding                   | `u8` |       0       |

```toml
[display.border]
size = 2
radius = 12
margin = { right = 15 }
```

> **Note**: If the border size is larger than the radius, the inner corners won’t be rounded.

## Text

The application has **summary** and **body** properties, both treated as `Text`. A new `text` property can be used for both the summary and body at once. Use `text` when the same value applies to both, or set them separately if needed.

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

```toml
[display.text]
justification = "left"
wrap = false
ellipsize_at = "middle"
font_size = 18

[display.summary]
style = "bold"
```

> **Property priority**:
>
> 1. Use `summary` or `body` first.
> 2. If missing, use the `text` property.
> 3. If still missing, default values apply.

## Markup

Enables text styling through HTML tags.

For the body, the `markup` property is applied, allowing the use of HTML-like tags, such as:

- `<b>` - bold style
- `<i>` - italic style
- `<u>` - underline style
- `<a href="https://google.com">` - a hyperlink
- `<img src="path/to/image" alt="image description">` - an image embedded in the text

You can disable the `markup` property by setting it to `false`.

```toml
markup = false
```

## Timeout

The time in milliseconds after which the notification banner will automatically close due to expiration, starting from when it is created.

Additionally, there is an extended timeout configuration based on the urgency of the banners, defined in the `Timeout` table.

The `Timeout` table:

| Key      | Short description                                     | Type  |
| :------- | :---------------------------------------------------- | :---- |
| default  | Set default timeout for all urgency                   | `u16` |
| low      | Override timeout value for 'low' urgency banners      | `u16` |
| normal   | Override timeout value for 'normal' urgency banners   | `u16` |
| critical | Override timeout value for 'critical' urgency banners | `u16` |

```toml
timeout = 5000

# or

[display.timeout]
default = 5000
critical = 0
```

> **Note:**
> The value `0` means will never expired.
