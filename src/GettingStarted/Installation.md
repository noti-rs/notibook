# Installation

## Installing via Cargo (Recommended)

The simplest way to install Noti is using Cargo, Rust's package manager:

```bash
# Install directly from GitHub
cargo install --git https://github.com/noti-rs/noti/

# Or clone and install locally
git clone https://github.com/noti-rs/noti.git
cd noti
cargo install --path .
```

## Building from Source

```bash
# Clone the repository
git clone https://github.com/noti-rs/noti.git
cd noti

# Build in release mode
cargo build --release

# Install the binary
cargo install --path .
```
