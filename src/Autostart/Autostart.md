# Autostart

This guide will help you configure `Noti` to start automatically.

## Understanding Environment Variables

Before proceeding, note that `Noti` respects the following environment variables:

| Variable           | Default Path     | Description                       |
| ------------------ | ---------------- | --------------------------------- |
| `$XDG_DATA_HOME`   | `~/.local/share` | User-specific data files          |
| `$XDG_CONFIG_HOME` | `~/.config`      | User-specific configuration files |

> **WARNING**:  
> Paths may vary depending on your distribution or desktop environment.
> Ensure the configuration file is readable by `Noti` if placed in a non-default location.

## Autostarting Noti

There are two ways to autostart the notification daemon:

1. **[Within a Wayland session](./WithinWaylandSession.md)** (Recommended)
   Simpler and integrates with your desktop session.

2. **[Without a Wayland session](./WithoutWaylandSession.md)** (Advanced)
   For headless/embedded setups or non-session environments.

> **NOTE**:  
> This documentation assumes a **systemd-based** Linux distribution.
> If you use OpenRC, SysV, or another init system, please adapt the examples or [contribute a guide](https://github.com/noti-rs/notibook/pulls).
