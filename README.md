# BE Station

BE Station is the graphical boot environment manager for GhostBSD. It provides a GTK interface over OpenZFS boot environments so you can create, rename, activate, mount, unmount, and delete them without using `bectl` by hand.

This project was previously named Backup Station. The command, gettext domain, and desktop entry are now `be-station`.

## Overview

A boot environment (BE) is a bootable clone of the system, usually stored as a ZFS dataset. GhostBSD uses boot environments so you can keep a known-good system, try an update, and roll back by activating an older BE and rebooting.

BE Station is a GTK 3 Python application that lists every boot environment and exposes the common `bectl` operations:

- Create a new boot environment from the selected source BE
- Rename a boot environment
- Activate a boot environment (used on the next reboot)
- Delete a boot environment (the currently active BE cannot be deleted)
- Mount a boot environment
- Unmount a boot environment

The list shows the BE name, active flag, mountpoint, space used, and creation date and time.

BE Station must run as root because boot environment changes affect the whole system. If you start it without root, it offers to relaunch with `sudo`. When it is started through `sudo`, it asks for the calling user's password before opening the main window.

## Requirements

- GhostBSD, or another FreeBSD-based system with OpenZFS boot environments
- Python 3
- GTK 3 and PyGObject
- [py-bectl](https://github.com/GhostBSD/pybectl)
- `bectl` (from the base system)
- `sudo`

## Installation

From a source checkout:

```bash
python setup.py install
```

On GhostBSD, BE Station is also available as a package once the port is updated.

The installer places:

- `/usr/local/bin/be-station`
- `/usr/local/share/applications/be-station.desktop`
- `/usr/local/etc/sudoers.d/be-station`
- gettext catalogs under `/usr/local/share/locale`

The sudoers drop-in lets members of `wheel` run `/usr/local/bin/be-station` with `sudo`.

## Usage

From the application menu, open **BE Station**. From a terminal:

```bash
sudo be-station
```

Select a boot environment in the list, then use the buttons:

| Action | What it does |
| --- | --- |
| Create BE | Clones the selected boot environment under a name you enter |
| Rename BE | Renames the selected boot environment |
| Delete BE | Destroys the selected boot environment after confirmation |
| Activate BE | Marks the selected boot environment to boot on the next restart |
| Mount BE | Mounts the selected boot environment |
| Unmount BE | Unmounts the selected boot environment |

Activating a boot environment does not switch the running system immediately. Reboot for the change to take effect.

## Managing translations

BE Station uses GNU gettext. Translation sources live in `po/`. Russian (`ru`) and Simplified Chinese (`zh_CN`) are included.

To merge new strings from the source into the `.pot` and `.po` files, and to build `.mo` catalogs:

```bash
python setup.py build_i18n -m
```

To start a new language, copy the template and edit the translations:

```bash
msginit --input=po/be-station.pot --locale=fr --output=po/fr.po
```

Replace `fr` with the locale you are adding.

## Contributing

Interested in contributing to BE Station or other GhostBSD tools?

- Join our Telegram channel: https://t.me/ghostbsd_dev
- Discuss on GitHub: https://github.com/orgs/ghostbsd/discussions/categories/tools-and-softwares

## License

BSD 2-Clause License. See [LICENSE](LICENSE).

## Links

- GitHub: https://github.com/GhostBSD/be-station/
- py-bectl: https://github.com/GhostBSD/pybectl
- GhostBSD: https://www.ghostbsd.org/
