# OpenGGF AMP templates

[CubeCoders AMP](https://cubecoders.com/AMP) Generic-module templates for running
OpenGGF services. Add this repository in AMP under
**Configuration → Instance Deployment → Configuration Repositories** as
`OpenGGF/openggf-amp-templates:main`, then fetch the latest templates.

## OpenGGF Multiplayer Master (`openggf-multi`)

Runs the OpenGGF multiplayer master (room browser, relay rooms, recording
uploads) from the `racing/` tree of [OpenGGF](https://github.com/OpenGGF/OpenGGF).

| Release channel | Rolling prerelease tag | Public host |
|---|---|---|
| master | `multi-latest` | `multi.openggf.com` |
| develop | `multi-dev-latest` | `multi-dev.openggf.com` |
| next | `multi-next-latest` | `multi-next.openggf.com` |

- **Update** downloads Eclipse Temurin JRE 25 into the instance (skipped when the
  latest build is already installed), then replaces `server/` with
  `openggf-racing-server.zip` from the selected prerelease.
- **Start** runs `openggf-multi-prestart.sh`, which creates per-instance admin and
  verifier tokens under `secrets/` on first start and writes `master.yaml` from the
  AMP settings, then starts `racing-server master`.
- The master terminates TLS itself on the Master Port. The admin listener binds
  127.0.0.1 on Master Port + 1 and must not be exposed.
- TLS files are not part of the template. Install `tls/fullchain.pem` and
  `tls/privkey.pem` in the instance's `openggf-multi/` directory, readable by the
  AMP user, and restart after renewal. On the OpenGGF testbed a certbot deploy
  hook does this.
- Rooms, sessions and verification jobs live in memory: every restart or update
  disconnects players. `data/` (identities, recordings, audit log) persists.

Verifiers are not part of this template; they need operator-supplied ROMs.
See OpenGGF's `racing/deploy/README.md` for the service contract.
