# Getting Started with Noti

## Prerequisites

Before installing Noti, ensure you have the following:

- A Linux system with a Wayland compositor
- Rust toolchain (`rustc` and `cargo`)
- Basic understanding of terminal commands

## Installation

### Installing via Cargo (Recommended)

The simplest way to install Noti is using Cargo, Rust's package manager:

```bash
# Install directly from GitHub
cargo install --git https://github.com/noti-rs/noti/

# Or clone and install locally
git clone https://github.com/noti-rs/noti.git
cd noti
cargo install --path .
```

### Building from Source

```bash
# Clone the repository
git clone https://github.com/noti-rs/noti.git
cd noti

# Build in release mode
cargo build --release

# Install the binary
cargo install --path .
```

## Running

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

## Automatic Startup

### Session Startup Script

You can add noti run to your session startup script to ensure it starts automatically. For example, if you use Hyprland, include the following line in your Hyprland configuration:

```conf
exec-once = noti run
```

This is a simple way to start noti automatically when your session begins. However, note that this approach does not handle cases where the application crashes or encounters errors—it will not automatically restart `noti` in such situations.

### Advanced Autostart

For more robust and advanced autostart setups, refer to the [Autostart](./Autostart.md). This guide provides detailed instructions for setting up a reliable autostart configuration.
