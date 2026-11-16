<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/icon-dark.svg">
    <img src="assets/icon.svg" alt="2FAuth logo" width="128">
  </picture>
</p>

<h1 align="center">aac-2fauth</h1>

<p align="center">Self-hosted deployment definition for 2FAuth, a web app for managing two-factor authentication accounts.</p>

<p align="center">
  <a href="https://github.com/Bubka/2FAuth"><img alt="Upstream" src="https://img.shields.io/badge/upstream-Bubka%2F2FAuth-blue?logo=github"></a>
  <img alt="Image" src="https://img.shields.io/badge/image-2fauth%2F2fauth%3A9.0.0-2496ED?logo=docker&logoColor=white">
  <a href="https://github.com/Bubka/2FAuth/blob/master/LICENSE"><img alt="License" src="https://img.shields.io/badge/license-AGPL--3.0-green"></a>
</p>

## About

2FAuth is a self-hosted web application that manages your two-factor authentication (2FA)
accounts and generates their security codes. It is a web-based alternative to mobile
authenticator apps, so the accounts can be accessed from any browser on your own network.

Key features (as provided by upstream):

- Store and manage TOTP, HOTP and other 2FA accounts
- Generate one-time security codes in the web interface
- Import and export accounts
- Multi-user support with a web-based login
- WebAuthn support and optional OpenID Connect login (this deployment is prepared for it)

## Upstream

- Project: <https://github.com/Bubka/2FAuth>
- Documentation: <https://docs.2fauth.app>
- Container image: `2fauth/2fauth:9.0.0`
- Dependency: `redis:8.6.1` (deployed as `aac-2fauth-redis`)
- License: AGPL-3.0 (upstream project)

## Quick start (Docker Compose)

The Compose file is rendered from `service.yml` by the App-as-Code template engine. For a
manual run, provide the values listed under "Configuration" first, then start the stack:

```bash
# Provide the values listed under "Configuration" in .env / stack.env
docker compose up -d
```

Secrets are not stored in this repository. Provide them through the deployment pipeline
or create the `.env` and `stack.env` files yourself (see below). Do not commit them.

## Configuration

Environment variables (defaults from `service.yml`; `CHANGE_ME` must be set per deployment):

| Variable | Default | Description |
|---|---|---|
| `APP_NAME` | `Familie 2FA` | Application name shown in the UI |
| `APP_ENV` | `local` | Laravel application environment |
| `APP_DEBUG` | `false` | Enable debug output (keep `false` in production) |
| `APP_KEY` | `CHANGE_ME` (secret) | Application encryption key |
| `APP_URL` | `CHANGE_ME` | Public URL of the instance |
| `SITE_OWNER` | `CHANGE_ME` | Site owner contact shown by the application |
| `IS_DEMO_APP` | `false` | Demo mode toggle |
| `MIX_ENV` | `local` | Build environment |
| `LOG_CHANNEL` | `daily` | Log channel |
| `LOG_LEVEL` | `notice` | Log level |
| `CACHE_DRIVER` | `file` | Cache backend |
| `SESSION_DRIVER` | `file` | Session backend |
| `SESSION_LIFETIME` | `120` | Session lifetime (minutes) |
| `BROADCAST_DRIVER` | `log` | Broadcast backend |
| `QUEUE_DRIVER` | `sync` | Queue backend |
| `MAIL_DRIVER` | `log` | Mail transport |
| `MAIL_HOST` | `CHANGE_ME` | SMTP host |
| `MAIL_PORT` | `465` | SMTP port |
| `MAIL_USERNAME` | `CHANGE_ME` | SMTP user |
| `MAIL_PASSWORD` | `CHANGE_ME` (secret) | SMTP password |
| `MAIL_ENCRYPTION` | `TLS` | SMTP encryption |
| `MAIL_FROM_NAME` | `Backend 2FA` | Sender name |
| `MAIL_FROM_ADDRESS` | `CHANGE_ME` | Sender address |
| `MAIL_VERIFY_SSL_PEER` | `true` | Verify the SMTP certificate |
| `THROTTLE_API` | `60` | API requests per minute |
| `LOGIN_THROTTLE` | `5` | Login attempts before throttling |
| `AUTHENTICATION_GUARD` | `web-guard` | Authentication guard |
| `WEBAUTHN_NAME` | `2FAuth` | WebAuthn relying party name |
| `WEBAUTHN_ID` | `null` | WebAuthn relying party ID |
| `WEBAUTHN_ICON` | `null` | WebAuthn relying party icon |
| `WEBAUTHN_USER_VERIFICATION` | `preferred` | WebAuthn user verification policy |
| `OPENID_AUTHORIZE_URL` | `CHANGE_ME` | OpenID Connect authorization endpoint |
| `OPENID_TOKEN_URL` | `CHANGE_ME` | OpenID Connect token endpoint |
| `OPENID_USERINFO_URL` | `CHANGE_ME` | OpenID Connect userinfo endpoint |
| `OPENID_CLIENT_ID` | `CHANGE_ME` | OpenID Connect client ID |
| `OPENID_CLIENT_SECRET` | `CHANGE_ME` (secret) | OpenID Connect client secret |
| `TRUSTED_PROXIES` | `*` | Trusted reverse proxies |
| `PROXY_FOR_OUTGOING_REQUESTS` | `null` | Outgoing HTTP proxy |
| `REDIS_HOST` | `CHANGE_ME` | Redis host (the dependency service is `aac-2fauth-redis`) |
| `REDIS_PORT` | `6379` | Redis port |
| `PUID` / `PGID` | `1000` | User and group ID the container runs as |
| `TZ` | `Europe/Berlin` | Time zone |

Ports:

| Port | Protocol | Purpose |
|---|---|---|
| `8000` | TCP | Web interface (`web`); external port `8001` |

Volumes:

| Mount | Description |
|---|---|
| `config` -> `/2fauth` | 2FAuth configuration and data |
| `redis-data` -> `/data` | Redis data (dependency service) |

## Deploy with App-as-Code

1. Edit `service.yml`. It is the single source of truth for image, ports, environment,
   volumes, dependencies and per-stage overrides. Do not edit the rendered output.
2. Pushing to `main` (or a configured stage branch) runs the service pipeline, which renders
   the manifests with `aac-template-engine` and triggers the deployment.
3. Releases of this definition are published under <https://github.com/app-as-code/aac-2fauth>.

## Credits and license

2FAuth is developed by the 2FAuth project and licensed under AGPL-3.0. This repository only
contains the deployment definition for it and is not a fork of the application. The logo is
provided by the upstream project and the dashboard icon collection; see
[`assets/ICON_SOURCE.md`](assets/ICON_SOURCE.md) for icon sources and licenses.
