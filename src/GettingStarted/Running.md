# Running

To confirm Noti is installed correctly:

```bash
# Check Noti version
noti --version

# Display help information
noti --help
```

After installation, you can start Noti manually:

```bash
# Start Noti daemon
noti run
```

# Automatic Startup

## Session Startup Script

You can add noti run to your session startup script to ensure it starts automatically. For example, if you use Hyprland, include the following line in your Hyprland configuration:

```conf
exec-once = noti run
```

This is a simple way to start noti automatically when your session begins. However, note that this approach does not handle cases where the application crashes or encounters errors—it will not automatically restart `noti` in such situations.

## Advanced Autostart

For more robust and advanced autostart setups, refer to the [Autostart](./Autostart.md). This guide provides detailed instructions for setting up a reliable autostart configuration.
