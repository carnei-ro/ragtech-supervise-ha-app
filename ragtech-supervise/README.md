# Ragtech Supervise

Packages the [kriansa/ragtech-supervise](https://github.com/kriansa/ragtech-supervise)
Docker image (`ghcr.io/kriansa/ragtech-supervise:v0.0.6`) as a Home Assistant
add-on for monitoring Ragtech UPS devices.

## Configuration

This add-on requires no options. It exposes:

- **Web interface** on port `4470`.
- The UPS serial device **`/dev/ttyACM0`**, mapped in **read-write mode**.

After starting, open `http://<home-assistant-host>:4470`.

> **Architecture:** the upstream image is published only for `amd64`.

See the [repository README](../README.md) for full installation instructions.
