# ns8-flatnotes

Self-hosted note-taking app that stores notes as plain Markdown files in a folder, with search and tags.

Module id: `flatnotes`. Images are pinned in `build-images.sh` and updated through reviewed changes.

## Install

```bash
add-module ghcr.io/geniusdynamics/flatnotes:latest 1
```

The command returns the instance id, for example `flatnotes1`. Use the id NS8 returns.

## Configure

```bash
api-cli run configure-module --agent module/flatnotes1 --data - <<'EOF'
{
  "host": "flatnotes.example.org",
  "http2https": true,
  "lets_encrypt": false
}
EOF
```

| Parameter | Required | Type | Description |
|---|---|---|---|
| `host` | yes | string | Host name for the application, like 'flatnotes.domain.org' |
| `lets_encrypt` | yes | boolean | Request a valid Let's Encrypt certificate. |
| `http2https` | yes | boolean | Redirect all the HTTP requests to HTTPS |

## Configuration output

`get-configuration` returns: `host`, `lets_encrypt`, `http2https`.

## Images

| Image | Version |
|---|---|
| `docker.io/dullage/flatnotes` | `v5.5.5` |

Requires NS8 core `3.12.4-0` or newer.

## Network and access

- Reserves 1 TCP port(s) on the node. The web application is published through Traefik on the configured host name.
- Runs rootless.
- Declares these NS8 authorizations: `traefik@node:routeadm`.

## Backup

The following paths are included in the NS8 backup (`imageroot/etc/state-include.conf`):

- `state/passwords.env`
- `volumes/data`

## Services

- `flatnotes.service`

## Get the configuration

```bash
api-cli run get-configuration --agent module/flatnotes1
```

## Update and uninstall

```bash
api-cli run update-module --data '{"module_url":"ghcr.io/geniusdynamics/flatnotes:latest","instances":["flatnotes1"],"force":true}'
remove-module --no-preserve flatnotes1
```

## Testing

Continuous integration:

- `Publish images` builds the module image on every push.
- `Test module` installs the image on a QEMU NS8 node and runs the Robot Framework suite in `tests/`. It runs after `Publish images` succeeds on `dev` and on `main`.
- `Build API JSON Schema docs` and the security scan run alongside.

The suite installs the module, configures a host name, checks that the web application answers through Traefik, takes UI screenshots and removes the module.

Run it locally with the NS8 standard infrastructure described in the [ns8-github-actions README](https://github.com/NethServer/ns8-github-actions/blob/v1/README.md#running-tests-locally).

## Upstream

Application documentation: https://github.com/dullage/flatnotes

## License

GPL-3.0-or-later. See [LICENSE](LICENSE).
