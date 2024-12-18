# Themes

Define custom color schemes for different notification urgency levels:

The `Theme` table:

| Key      | Type     | Default value | Short description                        |
| :------- | :------- | :-----------: | :--------------------------------------- |
| low      | `Colors` |       -       | The colors for `low` urgency banner      |
| normal   | `Colors` |       -       | The colors for `normal` urgency banner   |
| critical | `Colors` |       -       | The colors for `critical` urgency banner |

The `Colors` table:

| Key        | Type    |               Default value               | Short description       |
| :--------- | :------ | :---------------------------------------: | :---------------------- |
| background | `Color` |                 "#FFFFFF"                 | The background color    |
| foreground | `Color` | "#000000" (but for `critical`: "#FF0000") | The foreground color    |
| border     | `Color` | "#000000" (but for `critical`: "#FF0000") | The border stroke color |

```toml
[[theme]]
name = "pastel"

[theme.normal]
background = "#1e1e2e"
foreground = "#99AEB3"
border = "#000"

[theme.critical]
background = "#EBA0AC"
foreground = "#1E1E2E"
border = "#000"
```
