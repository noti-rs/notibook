# Tips and Tricks

## Blurred Transparent Background

If you're using Hyprland, you can create a blurred transparent background with the following steps:

1. Make the background transparent in your theme configuration:

```toml
[theme.normal]
background = "#ffffff00" # the last two symbols control opacity
```

2. Add these `layerrule` settings somewhere to your Hyprland config:

```conf
layerrule = blur, noti
layerrule = blurpopups, noti
layerrule = xray, noti
layerrule = ignorealpha 0, noti
```
