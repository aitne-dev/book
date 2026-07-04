# Getting Started

This chapter will guide you through installing the Aitne CLI toolchain, initializing your first project, and spinning up a local development server.

Before proceeding, ensure you have the [MoonBit toolchain](https://www.moonbitlang.com/) installed.

## Installation

Aitne provides a lightweight CLI tool to manage project lifecycles, code generation, and compilation tasks.

### macOS and Linux

You can install the Aitne CLI automatically by running the following command in your terminal:

```bash
curl -fsSL https://raw.githubusercontent.com/Arcelyth/aitne/main/scripts/install.sh | bash
```

> ⚠️ **Windows Support Note**
> The `aitne` CLI relies on moonbit async library that are currently not supported natively on Windows. If you are on Windows, we highly recommend using **WSL2 (Windows Subsystem for Linux)** to run the toolchain.

## Creating a New Project

Once the installation is complete, you can bootstrap a fresh Aitne project using the `init` subcommand. This will scaffold a standard project directory structure pre-configured for MoonBit and MBX development.

```bash
# Initialize a new project named "your_project"
aitne init your_project

cd your_project
```

## Building the Project

To build your project, execute:

```bash
aitne build
```

Under the hood, this command scans your `.mbx` templates, generates the corresponding MoonBit implementation files, and invokes the MoonBit backend compiler to output optimized JavaScript/WebAssembly artifacts.

## Running the Development Server

To see your reactive web application in action, start the built-in high-performance web server:

```bash
aitne run
```

By default, this spins up a local server hosting your application. Open your browser and navigate to: `http://localhost:8000`.

### Configuration via `eirene.toml`

The behavior of `aitne run` (such as the binding port, routing rules, and asset directories) is governed by the `eirene.toml` file generated in your project root. You can modify this file to customize your local development environment.
