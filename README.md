# Ragtech Supervise — Home Assistant Add-on

This repository packages the **Ragtech Supervise** Docker image from
[kriansa/ragtech-supervise](https://github.com/kriansa/ragtech-supervise) as a
Home Assistant add-on.

Ragtech Supervise is the Brazilian UPS monitoring software shipped by Ragtech.
The upstream project wraps it in a container so it can run on modern Linux
distributions. This add-on makes that image installable directly from the
Home Assistant add-on store.

## What it does

- Pulls and runs the upstream image `ghcr.io/kriansa/ragtech-supervise:latest`.
- Exposes the Ragtech Supervise **web interface on port `4470`**.
- Maps the UPS serial device **`/dev/ttyACM0`** into the container in
  **read-write mode** so Supervise can communicate with the UPS.

## Architecture support

The upstream Ragtech Supervise image is published **only for `amd64`**, so this
add-on declares `arch: [amd64]` and will only install on amd64 hosts.

## Installation

1. In Home Assistant, go to **Settings → Add-ons → Add-on store**.
2. Open the **⋮** menu (top right) → **Repositories** and add:
   ```
   https://github.com/carnei-ro/ragtech-supervise-ha-app
   ```
3. Find **Ragtech Supervise** in the store, install it, and start it.
4. Confirm your UPS is connected and exposed to the host as `/dev/ttyACM0`.
5. Open the web interface at `http://<home-assistant-host>:4470`.

## Configuration details

The relevant settings live in [`ragtech-supervise/config.yaml`](ragtech-supervise/config.yaml):

| Setting | Value | Notes |
| --- | --- | --- |
| `arch` | `amd64` | Upstream image is amd64-only. |
| `ports` | `4470/tcp: 4470` | Web interface, container `4470` → host `4470`. |
| `devices` | `/dev/ttyACM0` | UPS serial device, mapped read-write. |
| `init` | `false` | The upstream image ships its own init system. |
| `startup` | `services` | Long-running service. |

### Note on device read-write mode

The Home Assistant Supervisor maps any device listed under `devices` with
read + write (+ mknod) cgroup permissions by default. Listing `/dev/ttyACM0` is
therefore equivalent to the upstream `docker run --device /dev/ttyACM0:rw`
invocation — no extra suffix is required.

## Data persistence

Ragtech Supervise stores its SQLite database at `/data/monit.db`. Home Assistant
maps `/data` to a persistent volume automatically, so monitoring data survives
add-on restarts and updates.

## Credits

- Upstream image and software wrapper: [kriansa/ragtech-supervise](https://github.com/kriansa/ragtech-supervise) (Apache-2.0).
- Ragtech Supervise is a product of [Ragtech](https://www.ragtech.com.br/).
