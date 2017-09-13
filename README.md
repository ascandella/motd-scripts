# Fancy MOTD

Note: I run Fedora 26. These scripts may or may not be useful on other distros.

![screenshot](https://raw.githubusercontent.com/sectioneight/motd-scripts/screenshots/img/motd.png)

## Backups

I use [Borg] to back up securely from my personal server to my Synology. This
README does not cover setting up Borg either on Fedora or Synology, but they
have great docs. Pro-tip: use the self-contained x64 binary to install on your
NAS (don't bother with Py3k).

[Borg]: https://borgbackup.readthedocs.io/en/stable/

## Installation

Since the creation of the MOTD text takes ~15 seconds, we obviously don't want
it running every time somebody logs in. So I run it on a cron, specifically with
this file in `/etc/cron.d/create-motd`

```bash
*/5 * * * * /etc/dynamic-motd/make-motd > /etc/motd2 && mv /etc/motd2 /etc/motd
```

`/etc/dynamic-motd/make-mod` is a symlink to a checkout of this repo. None of
the other scripts need to be symlinked in -- they all detect their installation
path via `readlink -f`.

## Borg Secrets

Note that, to use `borg` to query remote backups, you'll need a file (in my
case `/root/.borg` that exports the passphrase and the `$REPOSITORY`). Keep
this secure (`chmod 400`) and *never* put it in your scripts / on GitHub. Also
keep a backup of it, offsite, preferably GPG-encrypted.
