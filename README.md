# ns8-flatnotes

A [NethServer 8](https://github.com/NethServer/ns8-core) module for [flatnotes](https://github.com/dullage/flatnotes), a self-hosted, database-less note-taking web app that stores notes as plain Markdown files.

## Features

- Notes are plain Markdown files, no database
- Full-text search and tags
- Single user login, with a generated password
- Published by Traefik, with optional Let's Encrypt certificate

## Install

Instantiate the module with:

```bash
add-module ghcr.io/geniusdynamics/flatnotes:latest 1
```

The output will return the instance name:

```json
{
  "module_id": "flatnotes1",
  "image_name": "flatnotes",
  "image_url": "ghcr.io/geniusdynamics/flatnotes:latest"
}
```

## Configure

Let's assume that the flatnotes instance is named `flatnotes1`.

Launch `configure-module`, by setting the following parameters:

- `host`: a fully qualified domain name for the application
- `http2https`: enable or disable HTTP to HTTPS redirection (true/false)
- `lets_encrypt`: enable or disable Let's Encrypt certificate (true/false)

Example:

```bash
api-cli run configure-module --agent module/flatnotes1 --data - <<EOF
{
  "host": "flatnotes.example.org",
  "http2https": true,
  "lets_encrypt": false
}
