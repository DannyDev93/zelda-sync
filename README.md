# zelda-sync

Keeps Zelda save files in sync between a PC and a Steam Deck, through a private GitHub repo.

```
curl -fsSL https://raw.githubusercontent.com/DannyDev93/zelda-sync/main/zsync | bash
```

Try it without changing anything first:

```
curl -fsSL https://raw.githubusercontent.com/DannyDev93/zelda-sync/main/zsync | bash -s -- --dry-run
```

| Game | Port | Synced |
|---|---|---|
| Skyward Sword HD | Sky Recomp | `skyrecomp/save` |
| Ocarina of Time | Ship of Harkinian | `soh/Save` |
| Majora's Mask | Zelda64Recompiled | `Zelda64Recompiled/saves` |
| Twilight Princess | Dusklight | the `*-GZ2?-gczelda2.gci` memory card file only |
| Wind Waker HD | ZeldaWWHDRecomp | `wwhd/save/user` |

Only real save files are synced: no settings and no save states.

## What you see

Before changing anything it shows, for each game, what will be uploaded or downloaded: the files with their
sizes and dates, where they come from and where they go, and which current save gets backed up first. It then
asks `Go ahead? [y = yes / Enter = cancel]`. Afterwards it prints a summary: what was backed up, where it is now,
the version dates, and where any replaced save was kept.

## How it decides

For each game it compares this device, the cloud, and what this device had at its last sync:

- same everywhere: nothing to do
- only this device changed: upload
- only the cloud changed: download (the local save is backed up first)
- both changed: it shows both dates and asks which one to keep; the other one is backed up

It searches for the save files itself (home folder, Flatpak, Proton, EmuDeck, SD card, USB drives) by their
file names. If a game isn't found, is running, or has saves in more than one place, it says so and skips that
game; the summary at the end always says how many games were skipped.

Local backups: `~/.local/share/zelda-sync/backups/` (the newest 20 per game). The cloud repo keeps every version.

## Options

- `--dry-run` shows what would happen and changes nothing
- `--pick-again` forgets the remembered save locations and asks again
- `--yes` skips the confirmation question (for unattended use)
- `--diagnose` searches the device for anything related to these games and uploads a report to the private
  saves repo (`diagnostics/`). It touches no saves.

The GitHub token is read from `~/Games/Zelda/.sync/token` or `~/.config/zelda-sync/token`; on a new device it
asks for it once.
