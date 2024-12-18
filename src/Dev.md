# Developer Guide

Welcome to the Noti Developer Guide! This chapter provides essential information for developers who want to contribute to the Noti project or understand its architecture and codebase.

## Project Structure

The Noti project is organized into several crates, each serving a specific purpose:

- **app**: Contains the command-line interface and main application logic.
- **backend**: Handles the core functionality and runs as a daemon.
- **client**: Manages client-side operations and interactions.
- **dbus**: Implements D-Bus communication using the `zbus` crate.
- **render**: Provides rendering capabilities for notifications.
- **config**: Manages configuration settings and parsing.
- **macros**: Contains procedural macros to simplify code generation.
- **shared**: Includes shared utilities and types used across the project.

## Setting Up the Development Environment

To set up your development environment, follow these steps:

1. **Install Rust Toolchain**:
   Ensure you have the latest Rust toolchain installed. You can install it using [rustup](https://rustup.rs/).

2. **Clone the Repository**:

   ```bash
   git clone https://github.com/noti-rs/noti.git
   cd noti
   ```

3. **Run the Project**:
   Use Cargo to build the project:
   ```bash
   cargo run -- run
   ```

## Coding Guidelines

- **Rust Edition**: The project uses Rust 2021 edition. Ensure your code is compatible with this edition.
- **Formatting**: Use `rustfmt` to format your code. The configuration is specified in `rustfmt.toml`.
- **Linting**: Run `clippy` to catch common mistakes and improve code quality:
  ```bash
  cargo clippy
  ```

## Contributing

We welcome contributions from the community! Here’s how you can contribute:

1. **Fork the Repository**:
   Create a fork of the repository on GitHub.

2. **Create a Branch**:
   Create a new branch for your feature or bug fix:

   ```bash
   git checkout -b feat/my-feature
   ```

3. **Make Changes**:
   Implement your changes, ensuring you follow the coding guidelines.

4. **Commit and Push**:
   Commit your changes with a descriptive message and push to your fork:

   ```bash
   git commit -am "feat: add new feature"
   git push origin feature/my-feature
   ```

5. **Open a Pull Request**:
   Submit a pull request to the main repository. Provide a clear description of your changes and any relevant context.

## Commit messages

Write clear and concise commit messages. Use the following format:

```
type(scope): subject

body (optional)
```

For example:

```
feat(config): add support for custom themes

This feature allows users to define their own themes in the configuration file.
```
