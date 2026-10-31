# Graphite OS updates

Signed release builds of Graphite OS, a personal fork of
[GrapheneOS](https://grapheneos.org/) for the Pixel 4a (5G) (`bramble`).

Graphite OS is **not** GrapheneOS and isn't supported by the GrapheneOS
project. The Pixel 4a (5G) is end-of-life: it gets no firmware or driver
security updates, so these builds are a harm-reduction effort, not a fully
secure OS.

## How updates work

The Graphite OS Updater app fetches files from
`https://github.com/siesta5787/graphite-updates/releases/latest/download/`:

- `bramble-stable` (and the beta/alpha/testing channel files): latest build number and date
- `bramble-ota_update-<build>.zip`: full over-the-air update
- `bramble-incremental-<old>-<new>.zip` (when present): delta update

Updates are signed. The phone verifies them against its own keys, so this
repository can't push an update the phone hasn't been signed to accept.

## Verifying downloads

Verified boot key (the bootloader warning screen shows an ID derived from this SHA-256):

```
82e4d99ba1826e639f2bc54060b202f7db37a1e5dc40fdaeffa98974d21909de
```

Factory image signing key (`bramble-install-<build>.zip.sig`):

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIjLX/uX5JQKqXhJBWwr508zv61gyHAdCJbTDbH1SDiQ Graphite OS factory images
```

To verify an install zip:

```
echo "graphite ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIjLX/uX5JQKqXhJBWwr508zv61gyHAdCJbTDbH1SDiQ Graphite OS factory images" > allowed_signers
ssh-keygen -Y verify -f allowed_signers -I graphite -n "factory images" \
    -s bramble-install-<build>.zip.sig < bramble-install-<build>.zip
```
