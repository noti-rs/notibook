# Within a Wayland Session

## Verify the Wayland Session

First, confirm that your Wayland compositor (Sway, Hyprland, GNOME, etc.) is running as a systemd user service. Most compositors integrate with `graphical-session.target`, but some require binding to a specific service (e.g., `sway-session.target`).

Run this to check active sessions:

```bash
systemctl --user list-dependencies graphical-session.target  # or compositor-specific target
```

---

## Create the systemd Service File

Create `$XDG_CONFIG_HOME/systemd/user/noti.service` with:

```systemd
[Unit]
Description=Noti — Wayland notification daemon
PartOf=graphical-session.target
After=graphical-session.target

[Service]
Type=dbus
BusName=org.freedesktop.Notifications
Environment=NOTI_LOG=info
ExecCondition=/bin/sh -c '[ -n "$WAYLAND_DISPLAY" ]'  # Verify Wayland
ExecStart=%h/.cargo/bin/noti run
Restart=on-failure

[Install]
WantedBy=graphical-session.target
```

---

## Enable and Start the Service

```bash
systemctl --user daemon-reload
systemctl --user enable --now noti.service  # Enables and starts immediately
```

Verify it’s running:

```bash
systemctl --user status noti.service
```

---

## Binding to Compositor-Specific Targets

If your compositor uses a custom target (e.g., `sway-session.target`), bind `noti` to it:

```bash
systemctl --user add-wants sway-session.target noti.service  # Example for Sway
```

> **Tip**: Check your compositor’s documentation for the correct target name.
> Maintainers may recommend specific targets in their installation guides.

## Troubleshooting

```bash
# View service status
systemctl --user status noti

# Follow live service logs
journalctl --user --unit noti --follow
```

> Common Issues:
>
> - Ensure the executable path is correct.
> - Check file permissions.
> - Verify systemd configurations.
> - Confirm environment variables are set correctly.
