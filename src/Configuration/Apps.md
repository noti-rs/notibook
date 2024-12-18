# App-Specific Configuration

The Noti application includes an app-specific configuration feature, allowing you to override the display table for a particular application.

Here’s how the properties are selected:

- Check if the property is defined in the app-specific configuration.
- If not, use the general display property.
- If it’s still not defined in the general display property, use the default value.

The `App` table:

| Key     | Short description                                    | Type                      |
| :------ | :--------------------------------------------------- | :------------------------ |
| name    | The name of application                              | `String`                  |
| display | The display configuration table for this application | [`Display`](./Display.md) |

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
