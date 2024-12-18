# Example scripts

## Volume Control with Pamixer

This script allows you to adjust the volume (increase, decrease, or mute) and displays notifications for each action.

<details style="cursor: pointer">
  <summary>Click to reveal</summary>

### Requirements

- **pamixer**: To control the audio volume.

```bash
#!/bin/bash

VOL_UP=$ICONS/volup.svg
VOL_DOWN=$ICONS/voldown.svg
VOL_MUTE=$ICONS/volmute.svg

APP_NAME=volume
ID=9999

case $1 in
  up)
    pamixer -i 5 -u
    noti send -a $APP_NAME -u "low" -i $VOL_UP -r $ID "Volume: $(pamixer --get-volume)%"
    ;;
  down)
    pamixer -d 5 -u
    noti send -a $APP_NAME -u "low" -i $VOL_DOWN -r $ID "Volume: $(pamixer --get-volume)%"
    ;;
  mute)
    pamixer -t
    if $(pamixer --get-mute); then
      noti send -a $APP_NAME -u "critical" -i $VOL_MUTE -r $ID "Volume muted"
    else
      noti send -a $APP_NAME -u "critical" -i $VOL_UP -r $ID "Volume unmuted"
    fi
    ;;
esac
```

> **Note**:
> Replace the `$ICONS/volup.svg`, `$ICONS/voldown.svg`, and `$ICONS/volmute.svg` with the paths to your own icons for volume up, down, and mute.

### Usage

```bash
./volume.sh up     # Increase volume
./volume.sh down   # Decrease volume
./volume.sh mute   # Toggle mute
```

Bind these commands to keyboard shortcuts for easier access.

</details>

## Brightness Control with Brightnessctl

This script allows you to adjust the screen brightness and displays notifications for each action.

<details style="cursor: pointer">
  <summary>Click to reveal</summary>

### Requirements

- **brightnessctl**: To control the screen brightness.

```bash
#!/bin/bash

ID=9998

case $1 in
up)
	brightnessctl s +10%
	brightness=$(brightnessctl g)
	percentage=$(echo "scale=0; $brightness * 100 / 1515" | bc -l)
	echo $percentage
	rounded_percentage=$(echo "($percentage + 5) / 10 * 10" | bc)
	echo $rounded_percentage
	noti send -a "brightness" -u low -i $ICONS/brightness.svg -r $ID "Brightness: $rounded_percentage%"
	;;
down)
	brightnessctl s 10%-
	brightness=$(brightnessctl g)
	percentage=$(echo "scale=0; $brightness * 100 / 1515" | bc -l)
	echo $percentage
	rounded_percentage=$(echo "($percentage + 5) / 10 * 10" | bc)
	echo $rounded_percentage
	noti send -a "brightness" -u low -i $ICONS/brightness.svg -r $ID "Brightness: $rounded_percentage%"
	;;
esac
```

> **Note**:  
> Replace `$ICONS/brightness.svg` with the path to your own brightness icon.

### Usage

```bash
./brightness.sh up   # Increase brightness
./brightness.sh down # Decrease brightness
```

Bind these commands to keyboard shortcuts for quicker access.

</details>

## Keyboard Layout Switcher

This script switches between keyboard layouts and displays a notification showing the current layout.

<details style="cursor: pointer">
  <summary>Click to reveal</summary>

### Requirements

- **hyprctl**: To manage devices and switch keyboard layouts.
- **kb-switcher**(optional): See [hyprland_kb_switcher](https://github.com/JarKz/hyprland_kb_switcher/)

```bash
#!/bin/sh

ID=9997
keyboard="your-keyboard-name"

# NOTE: kb is https://github.com/JarKz/hyprland_kb_switcher/
# but you can just use `hyprctl switchxkblayout $keyboard next`
kb switch

value=$(hyprctl devices | grep -i $keyboard -A 2 | tail -n1 | cut -f3,4 -d' ')

noti send -a "kb" -u low -i $ICONS/kb.svg -r $ID "$value"
```

> **Note**:  
> Replace `keyboard` with your actual keyboard device name and `$ICONS/kb.svg` with the path to your keyboard icon.

</details>
