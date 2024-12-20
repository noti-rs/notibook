# Example scripts

## Volume Control

This script allows you to adjust the volume (increase, decrease, or mute) and displays notifications for each action.

<details style="cursor: pointer">
  <summary>Click to reveal</summary>

### Requirements:

- **pamixer**: To control the audio volume.

### Script:

```bash
#!/bin/bash

VOL_UP=$ICONS/volup.svg
VOL_DOWN=$ICONS/voldown.svg
VOL_MUTE=$ICONS/volmute.svg

URGENCY=low
APP_NAME=volume

ID_TMPFILE=/tmp/vol-noti-id

if [[ ! -f $ID_TMPFILE ]]; then
  touch $ID_TMPFILE
fi

ID=$(<$ID_TMPFILE)

send_notification() {
    local icon=$1
    local message=$2
    local urgency=${3:-$URGENCY}

    if [[ $ID ]]; then
        noti send -a "$APP_NAME" -u "$urgency" -i "$icon" -r "$ID" "$message"
    else
        noti send -a "$APP_NAME" -u "$urgency" -i "$icon" "$message" --print-id > "$ID_TMPFILE"
    fi
}

update_volume() {
    local change=$1
    pamixer $change 5 -u
    send_notification "$VOL_UP" "Volume: $(pamixer --get-volume)%"
}

toggle_mute() {
    pamixer -t
    if $(pamixer --get-mute); then
        send_notification "$VOL_MUTE" "Volume muted" "critical"
    else
        send_notification "$VOL_MUTE" "Volume unmuted" "critical"
    fi
}

case $1 in
up)
    update_volume "-i"
    ;;
down)
    update_volume "-d"
    ;;
mute)
    toggle_mute
    ;;
*)
    echo "Usage: $0 { 'up' | 'down' | 'mute' }"
    exit 1
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

## Brightness Control

This script allows you to adjust the screen brightness and displays notifications for each action.

<details style="cursor: pointer">
  <summary>Click to reveal</summary>

### Requirements:

- **brightnessctl**: To control the screen brightness.

### Script:

```bash
#!/bin/bash

ID_TMPFILE=/tmp/brightness-noti-id

if [[ ! -f $ID_TMPFILE ]]; then
  touch $ID_TMPFILE
fi

ID=$(<$ID_TMPFILE)

send_notification() {
    local value=$1

    if [[ $ID ]]; then
        noti send -a "brightness" -u low -i $ICONS/brightness.svg -r $ID "Brightness: $value%"
    else
        noti send -a "brightness" -u low -i $ICONS/brightness.svg "Brightness: $value%" > $ID_TMPFILE
    fi
}

calculate_percentage() {
    local value=$1

	percentage=$(echo "scale=0; $brightness * 100 / 1515" | bc -l)
	rounded_percentage=$(echo "($percentage + 5) / 10 * 10" | bc)

    echo $rounded_percentage
}

case $1 in
up)
	brightnessctl s +10%
	brightness=$(brightnessctl g)
    send_notification $(calculate_percentage brightness)
	;;
down)
	brightnessctl s 10%-
	brightness=$(brightnessctl g)
    send_notification $(calculate_percentage brightness)
	;;
*)
    echo "Usage: $0 {up|down}"
    exit 1
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

### Requirements:

- **hyprctl**: To manage devices and switch keyboard layouts.
- **kb-switcher**(optional): See [hyprland_kb_switcher](https://github.com/JarKz/hyprland_kb_switcher/)

### Script:

```bash

#!/bin/sh

ID_TMPFILE=/tmp/kb-noti-id

if [[ ! -f $ID_TMPFILE ]]; then
  touch $ID_TMPFILE
fi

ID=$(<$ID_TMPFILE)

KEYBOARD="your-keyboard-name" # use hyprctl devices to see all connected keyboards

# NOTE: kb is https://github.com/JarKz/hyprland_kb_switcher/
# but you can just use `hyprctl switchxkblayout $keyboard next`
kb switch

VALUE=$(hyprctl devices | grep -i $KEYBOARD -A 2 | tail -n1 | cut -f3,4 -d' ')

if [[ $ID ]]; then
    noti send -a "kb" -u low -i $ICONS/kb.svg -r $ID "$VALUE"
else
    noti send -a "kb" -u low -i $ICONS/kb.svg "$VALUE" --print-id > $ID_TMPFILE
fi

```

> **Note**:  
> Replace `keyboard` with your actual keyboard device name and `$ICONS/kb.svg` with the path to your keyboard icon.

</details>
