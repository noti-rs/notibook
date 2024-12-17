# 6. Tips and Tricks

## Blurred transparent background

If you're a Hyprland user, you can easily achieve a blurred transparent background effect by using specific layerrule configurations.
Add this somewhere to your Hyprland config:

```conf
layerrule = blur, noti
layerrule = blurpopups, noti
layerrule = xray, noti
layerrule = ignorealpha 0, noti
```

These layerrule options are specific to Hyprland, so they may not be available or work the same way in other compositors. Be sure to configure them in your Hyprland setup to achieve the desired effect!
