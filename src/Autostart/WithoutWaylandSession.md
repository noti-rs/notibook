# Without a Wayland Session

> **⚠️ WARNING**: This method is **not recommended** for normal desktop use.
> It bypasses proper session integration and will fail if:
>
> - No Wayland compositor is running
> - Environment variables aren't properly configured

---

## 1. Create the D-Bus Service File

Create `$XDG_DATA_HOME/dbus-1/services/org.noti_rs.noti.service` with:

```ini
[D-BUS Service]
Name=org.freedesktop.Notifications
Exec=/bin/false  # Forces systemd activation
SystemdService=noti.service
```

> **Why?** This makes D-Bus delegate service management to systemd while claiming the notification bus.

---

## 2. Create the systemd Service File

Use the same `noti.service` file as the [Wayland session guide](./WithinWaylandSession.md#create-the-systemd-service-file), but there is no requirement to enable service.

Also you may want to add the `Environment=XDG_CONFIG_HOME=%/.config` line because the environment variable won't be forwarded by session.

---

## 3. Restart D-Bus and Test

Restart the user D-Bus session to apply changes:

```bash
systemctl --user restart dbus.service
```

If issues persist, **reboot** for full D-Bus reinitialization.

Test activation:

```bash
noti server-info # Should trigger service start
```

> **Note**: The service will **start on first notification attempt**, but fail if no Wayland compositor exists.

---

## Troubleshooting

* **Service Not Found**

```bash
ls $XDG_DATA_HOME/dbus-1/services/  # Verify file exists
```

Or try `~/.local/share/dbus-1/services/` location instead of using `XDG_DATA_HOME` environment variable. If fails, then try `/usr/share/dbus-1/services/`, but only as a last resort.

* **Environment Issues**

```bash
# List all environment variables of noti.service process by `systemctl` and `strings`.
strings /proc/$(systemctl --user show --property MainPID --value noti.service)/environ
```

* **Manual Testing**

```bash
# Start the service
systemctl --user start noti.service

# Check status of the service
systemctl --user status noti.service

# Follow live service logs
journalctl --user --unit noti --follow
```

> Common Issues:
>
> - Ensure the executable path is correct.
> - Check file permissions.
> - Verify systemd configurations.
> - Confirm environment variables are set correctly.
