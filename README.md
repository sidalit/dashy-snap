<div align="center">

<img src="snap/gui/dashy.png" alt="Dashy logo" width="128" />

# Dashy

**A self-hosted dashboard for your services**

[![dashy](https://snapcraft.io/dashy/badge.svg)](https://snapcraft.io/dashy)
[![Base: core26](https://img.shields.io/badge/Base-core26-2C8C3C?logo=canonical&logoColor=white)](https://snapcraft.io/core26)
![Confinement: strict](https://img.shields.io/badge/Confinement-strict-2C8C3C)
[![Maintainer: sidaliteir](https://img.shields.io/badge/Maintainer-sidaliteir-2C8C3C?logo=snapcraft&logoColor=white)](https://snapcraft.io/publisher/sidaliteir)
![Architectures: amd64 and arm64](https://img.shields.io/badge/Architectures-amd64%20%7C%20arm64-2C8C3C?logo=linux&logoColor=white)

[Getting Started](#getting-started) · [Configure](#configure) · [Limitations](#limitations) · [Build](#build) · [Support](#support) · [License](#license)

Unofficial snap packaging for [Dashy](https://github.com/lissy93/dashy), a self-hosted start page for your services and bookmarks.

</div>

---

## Getting Started

<a href="https://snapcraft.io/dashy"><img alt="Get it from the Snap Store" src="https://snapcraft.io/en/dark/install.svg" width="180"></a>

```bash
sudo snap install dashy
```

Open [http://127.0.0.1:4000](http://127.0.0.1:4000). The service starts automatically with upstream's example dashboard.

Edit the dashboard from the web interface, or by hand in `/var/snap/dashy/common/user-data/conf.yml` and reload the page. Sub-page configs and custom icons go in the same directory. Saving from the web interface first backs up the previous file to `/var/snap/dashy/common/user-data/config-backups`.

## Configure

| Option | Accepted value | Default |
| --- | --- | --- |
| `port` | Integer from `1` to `65535` | `4000` |
| `host` | IP address or host name to listen on | `127.0.0.1` |
| `ssl-port` | Integer from `1` to `65535` | `4001` |
| `http-auth` | `true` or `false` | `false` |

```bash
sudo snap set dashy host=0.0.0.0 port=4000
sudo snap get dashy
```

Changes restart an enabled or running service. Set `host=0.0.0.0` to reach the dashboard from other devices at `http://<computer-ip>:4000`.

Anyone who can reach the dashboard can read and save its config. Before opening it to your network, add users under `appConfig.auth.users` in `conf.yml` and set `http-auth=true`, so the server checks them on its own endpoints too (upstream's `ENABLE_HTTP_AUTH`); without it the login page is enforced only in the browser.

For HTTPS, place `dashy-pub.pem` and `dashy-priv.key` in `/var/snap/dashy/common/ssl` and run `sudo snap restart dashy`. HTTPS is served on `ssl-port`, and HTTP requests redirect to it.

Status checks over HTTP work out of the box. For ping checks (`pingCheckEnabled`), connect `sudo snap connect dashy:network-observe`; the service needs it to open raw ICMP sockets.

## Limitations

- Other upstream environment variables, such as `BASIC_AUTH_USERNAME`, `ENABLE_API` or `DISABLE_PROXY_ENDPOINTS`, are not exposed as snap options.
- The service reads files only from `/var/snap/dashy/common`; it cannot use icons or configs from your home directory.
- Remote access also requires the chosen port to be allowed by your firewall.

## Build

```bash
git clone https://github.com/sidalit/dashy-snap.git
cd dashy-snap
sudo snap install snapcraft --classic
snapcraft pack
```

## Support

- **Installation or snap issues:** [open a packaging issue](https://github.com/sidalit/dashy-snap/issues).
- **Application bugs:** [report them upstream](https://github.com/lissy93/dashy/issues).

## License

Snap packaging: [MIT](https://github.com/sidalit/dashy-snap/blob/main/LICENSE).
