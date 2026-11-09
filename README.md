# spltxt

**A tiny, customizable terminal splash text.** Inspired by the playful yellow splash messages shown when Minecraft starts.

`spltxt` is a small, local command-line utility. Its default command prints exactly one randomly selected sentence—no label, timestamp, counter, or decoration. Version **0.1R** targets **Kitty + Fish** ( Other terminal is considering) for automatic startup messages.

## Features

- One random sentence when you run `spltxt`.
- Optional Kitty-only automatic display when an interactive Fish shell starts.
- Add and delete sentences from the command line.
- Edit the collection with your `$VISUAL` or `$EDITOR`.
- Import UTF-8 `.txt` and `.docx` collections.
- Import asks whether to merge, replace, or cancel when a collection already exists.
- No network access, background service, third-party Python packages, or root privileges.
- Uninstalling preserves your personal message collection.

## Requirements

- Linux or another Unix-like system with Python 3.10+.
- Fish for automatic startup integration.
- Kitty for the automatic Kitty-only behaviour.
- Bash to run the installer.

The command itself is terminal-independent. The automatic integration checks Kitty's `KITTY_WINDOW_ID` environment variable and therefore does not print in other terminal emulators. Kitty documents this variable in its [environment variable glossary](https://sw.kovidgoyal.net/kitty/glossary/).

## Install

Clone or download this repository, then run:

```bash
chmod +x install.sh
./install.sh
```

The installer places the command in `~/.local/bin/spltxt`, the editable collection in `~/.local/share/spltxt/splash.txt`, and a Fish startup snippet in `~/.config/fish/conf.d/spltxt.fish` (respecting `XDG_DATA_HOME` and `XDG_CONFIG_HOME`). It does not use `sudo`.

Make sure `~/.local/bin` is on your `PATH`. In Fish, you can add it with:

```fish
fish_add_path ~/.local/bin
```

Open a new Kitty window to see the splash sentence. Existing Fish greeting customizations that define their own `fish_greeting` function may take precedence over Fish's default greeting variable; in that case, add this to your custom greeting function:

```fish
if set -q KITTY_WINDOW_ID
    command spltxt
end
```

## Commands

```text
spltxt                         Print one random sentence
spltxt add "TEXT"              Add a sentence
spltxt del "EXACT TEXT"         Delete an exact sentence
spltxt doc                     Open the collection in $VISUAL or $EDITOR
spltxt ver                     Show the version
spltxt install FILE            Import a .txt or .docx collection
spltxt uninstall               Uninstall the command and Fish integration
spltxt help                    Show help
```

Examples:

```bash
spltxt add "The penguin approves this session."
spltxt del "The penguin approves this session."
spltxt install ~/Downloads/my-splash-messages.docx
```

In Fish, set your preferred editor before opening the collection:

```fish
set -gx EDITOR nvim
spltxt doc
```

### Import behaviour

If a collection already exists, `spltxt install FILE` shows the number of current and incoming messages and asks:

- **M — Merge:** Keep existing messages and append imported messages that are not exact duplicates.
- **R — Replace:** Replace the current collection with the imported messages.
- **C — Cancel:** Leave the collection unchanged.

`.txt` files use one message per non-empty line. `.docx` files are read with Python's standard library. If paragraphs begin with numeric labels (for example, `001 Message`), the labels are removed and messages are sorted by their numeric labels. Otherwise, non-empty paragraphs are imported as messages.

## Files

```text
~/.local/bin/spltxt
~/.local/share/spltxt/splash.txt
~/.config/fish/conf.d/spltxt.fish
```

Edit `splash.txt` directly or run `spltxt doc`. Keep one message per line. Messages are UTF-8 and exact duplicates are ignored.

## Uninstall

```bash
spltxt uninstall
```

The command asks for confirmation, removes the installed executable and the managed Fish startup file, and **keeps** `~/.local/share/spltxt/splash.txt`. To remove personal messages too, delete that data directory yourself after uninstalling.

## Development / tests

Run the test suite with:

```bash
python -m unittest discover -s tests -v
```

## Licence
**Nope** (or not yet)
