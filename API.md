# HassIO Server

## HassIO REST API

Interface for HomeAssistant to control things from supervisor.

On error:
```json
{
    "result": "error",
    "message": ""
}
```

On success
```json
{
    "result": "ok",
    "data": { }
}
```

### HassIO

- GET `/supervisor/ping`

- GET `/supervisor/info`

The addons from `addons` are only installed one.

```json
{
    "version": "INSTALL_VERSION",
    "last_version": "LAST_VERSION",
    "arch": "armhf|aarch64|i386|amd64",
    "beta_channel": "true|false",
    "timezone": "TIMEZONE",
    "addons": [
        {
            "name": "xy bla",
            "slug": "xy",
            "description": "description",
            "arch": ["armhf", "aarch64", "i386", "amd64"],
            "repository": "12345678|null",
            "version": "LAST_VERSION",
            "installed": "INSTALL_VERSION",
            "detached": "bool",
            "build": "bool",
            "url": "null|url"
        }
    ],
    "addons_repositories": [
        "REPO_URL"
    ]
}
```

- GET `/supervisor/addons`

Get all available addons

```json
{
    "addons": [
        {
            "name": "xy bla",
            "slug": "xy",
            "description": "description",
            "arch": ["armhf", "aarch64", "i386", "amd64"],
            "repository": "core|local|REP_ID",
            "version": "LAST_VERSION",
            "installed": "none|INSTALL_VERSION",
            "detached": "bool",
            "build": "bool",
            "url": "null|url"
        }
    ],
    "repositories": [
        {
            "slug": "12345678",
            "name": "Repitory Name",
            "source": "URL_OF_REPOSITORY",
            "url": "null|WEBSITE",
            "maintainer": "null|BLA BLU <fla@dld.ch>"
        }
    ]
}
```

- POST `/supervisor/update`
Optional:
```json
{
    "version": "VERSION"
}
```

- POST `/supervisor/options`
```json
{
    "beta_channel": "true|false",
    "timezone": "TIMEZONE",
    "addons_repositories": [
        "REPO_URL"
    ]
}
```

- POST `/supervisor/reload`

Reload addons/version.

- GET `/supervisor/logs`

Output the raw docker log

### Security

- GET `/security/info`
```json
{
    "initialize": "bool",
    "totp": "bool"
}
```

- POST `/security/options`
```json
{
    "password": "xy"
}
```

- POST `/security/totp`
```json
{
    "password": "xy"
}
```

Return QR-Code

- POST `/security/session`
```json
{
    "password": "xy",
    "totp": "null|123456"
}
```

### Host

- POST `/host/shutdown`

- POST `/host/reboot`

- GET `/host/info`
See HostControl info command.
```json
{
    "type": "",
    "version": "",
    "last_version": "",
    "features": ["shutdown", "reboot", "update", "network_info", "network_control"],
    "hostname": "",
    "os": ""
}
```

- POST `/host/update`
Optional:
```json
{
    "version": "VERSION"
}
```

### Network

- GET `/network/info`
```json
{
    "hostname": ""
}
```

- POST `/network/options`
```json
{
    "hostname": "",
    "mode": "dhcp|fixed",
    "ssid": "",
    "ip": "",
    "netmask": "",
    "gateway": ""
}
```

### HomeAssistant

- GET `/homeassistant/info`

```json
{
    "version": "INSTALL_VERSION",
    "last_version": "LAST_VERSION",
    "devices": []
}
```

- POST `/homeassistant/update`
Optional:
```json
{
    "version": "VERSION"
}
```

- GET `/homeassistant/logs`

Output the raw docker log

- POST `/homeassistant/restart`

- POST `/homeassistant/options`
```json
{
    "devices": [],
}
```

### REST API addons

- GET `/addons/{addon}/info`
```json
{
    "name": "xy bla",
    "description": "description",
    "url": "null|url of addon",
    "detached": "bool",
    "repository": "12345678|null",
    "version": "VERSION",
    "last_version": "LAST_VERSION",
    "state": "started|stopped",
    "boot": "auto|manual",
    "build": "bool",
    "options": {},
}
```

- POST `/addons/{addon}/options`
```json
{
    "boot": "auto|manual",
    "options": {},
}
```

- POST `/addons/{addon}/start`

- POST `/addons/{addon}/stop`

- POST `/addons/{addon}/install`
Optional:
```json
{
    "version": "VERSION"
}
```

- POST `/addons/{addon}/uninstall`

- POST `/addons/{addon}/update`
Optional:
```json
{
    "version": "VERSION"
}
```

- GET `/addons/{addon}/logs`

Output the raw docker log

- POST `/addons/{addon}/restart`

## Host Control

Communicate over unix socket with a host daemon.

- commands
```
# info
-> {'type', 'version', 'last_version', 'features', 'hostname'}
# reboot
# shutdown
# host-update [v]

# hostname xy

# network info
-> {}
# network wlan ssd xy
# network wlan password xy
# network int ip xy
# network int netmask xy
# network int route xy
```

features:
- shutdown
- reboot
- update
- hostname
- network_info
- network_control

Answer:
```
{}|OK|ERROR|WRONG
```

- {}: json
- OK: call was successfully
- ERROR: error on call
- WRONG: not supported
