# General Settings

General settings apply to the entire notification system and control global behavior.

| Property name    | Description                                                       | Type                  | Default value |
| :--------------- | :---------------------------------------------------------------- | :-------------------- | :------------ |
| `font`           | Font                                                              | `String`              | "Noto Sans"   |
| `width`          | Width of banner frame                                             | `u16`                 | 300           |
| `height`         | Height of banner frame                                            | `u16`                 | 150           |
| `anchor`         | Screen position where notifications appear                        | `String`              | "top right"   |
| `gap`            | Space size between two banners. Measures in px                    | `u8`                  | 10            |
| `offset`         | Distance from screen edges                                        | `[u8, u8]`            | [0, 0]        |
| `sorting`        | How notifications are ordered                                     | `String` or `Sorting` | "default"     |
| `idle_threshold` | Duration that pauses notification timeouts during user inactivity | `String`              | "5 sec"       |
| `limit`          | Maximum number of notifications on the screen                     | `u8`                  | 0             |

## Font

Accepts the font name, which may or may not be separated by spaces.

```toml
font = "JetBrainsMono Nerd Font"
```

> **Note**:
>
> The application can only use font names recognized by the `fc-list` command.
> Styled fonts like "Noto Sans Bold" can't be used directly, but the app may load the required styles internally.

## Width & Height

The `width` and `height` properties define the dimensions of the notification banner frame in pixels. These settings control the visual size of the individual notification banners on the screen.

- **`width`**: Sets the horizontal size of the banner.
- **`height`**: Sets the vertical size of the banner.

```toml
width = 400
height = 200
```

## Anchor

The anchor of the current monitor for the current window instance, which determines where the notification banners will appear. The possible values are:

|             |        |              |
| :---------- | :----: | -----------: |
| top-left    |  top   |    top-right |
| left        |        |        right |
| bottom-left | bottom | bottom-right |

```toml
anchor = "top"
```

> **Note:**
>
> Dash between words is optional.

## Gap

The `gap` property specifies the amount of space, measured in pixels, between two adjacent notification banners. This setting ensures that notifications do not overlap and maintains a consistent visual separation between them.

```toml
gap = 15
```

## Offset

The offset from the edges for the window instance. The first value represents the offset along the `x-axis`, and the second value represents the offset along the `y-axis`.

For example, if you choose the `"bottom-left"` anchor and set an offset of `[5, 10]`, the window instance will be positioned at the bottom-left edge of the current monitor, with a 5-pixel offset from the left edge and a 10-pixel offset from the bottom edge.

```toml
offset = [10, 10]
```

## Sorting

The property that determines the banner sorting rule. This is particularly useful when you want to position banners with critical urgency at the top or bottom.

You can define a string for ascending sorting by default.

```toml
# with default ordering
sorting = "urgency"
```

However, to sort in descending order, you must define a table:

Possible values of the `by` property name:

- **`"default"`** (alias to **`"time"`**)
- **`"time"`**
- **`"id"`**
- **`"urgency"`**

Possible values of the `ordering` property name:

- **`"ascending"`** (also possible short name "asc")
- **`"descending"`** (also possible short name "desc")

```toml
# with specific ordering
[general.sorting]
by = "id"
ordering = "descending"
```

## Idle Threshold

When `idle_threshold` is set, notifications will not be removed or expired while the user is idle beyond the configured threshold. Once the user is active again, the timeout resumes.
This setting accepts a human-readable duration format (e.g., `"15 minutes"`, `"30s"`).

If set to `"none"`, the idle timeout behavior is disabled.

```toml
idle_threshold = "5 min"
```

> **WARNING:**
>
> Changes to the `idle_threshold` setting cannot be applied via hot-reload.
> To apply a new value, a full restart of the application is required.

## Limit

The maximum number of notifications that can be displayed at once. Once this limit is reached, any new notifications will be queued and shown once the currently displayed ones either time out or are manually dismissed.

A value of `0` means there is no limit.

```toml
limit = 3
```
