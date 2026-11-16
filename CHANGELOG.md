# Changelog

All notable changes to this deployment definition are documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

## [0.0.1]

### Added

- Initial App-as-Code service definition for 2FAuth (image `2fauth/2fauth:9.0.0`).
- Redis 8.6.1 dependency for session and cache storage.
- Web interface on port 8000 with Traefik routing and automatic DNS.
- Optional OpenID Connect and WebAuthn settings.
- README with configuration reference and project icon.
