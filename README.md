# Zen Browser Workspace Switcher Dropdown

A [Zen Browser](https://zen-browser.app) mod that replaces the cramped row of workspace icons at the bottom of the sidebar with a single active-space button that expands into a list on hover.

## What it does

- **Collapsed:** only the active workspace is shown, with its emoji and name.
- **On hover:** every workspace appears in a vertical list above it, each with its emoji and name.
- **Scales with more spaces:** icons never shrink to fit, so it looks the same at 5 workspaces or 15.

Zen normally shrinks the workspace icons once they run out of room, and past a certain point it hides inactive icons behind small dots until you hover each one. This mod replaces that behavior.

## Compatibility

Last tested on **Zen 1.23** on Linux.

This mod is pure CSS, but it overrides Zen's own workspace styling, so a Zen update can break it. See [How it works](#how-it-works) for the internals it depends on. If it breaks after an update, please open an issue with your Zen version.

## Installation

There is currently no way to publish new mods to the Zen Mods store, so this is installed manually as a local mod.

1. **Find your profile folder.** Open `about:support` and click **Open Folder** next to *Profile Folder*. If you use the Flatpak build, it is under `~/.var/app/app.zen_browser.zen/.zen/`.
2. **Close Zen completely.** Zen rewrites `zen-themes.json` while running.
3. **Create Mod Folder.** Create this folder inside your profile:
   ```
   chrome/zen-themes/18ab34db-0d6d-4126-bbd1-06e9eae4b864
   ```
4. **Copy Mod in.** Copy `chrome.css` in this repo into the folder you just created
5. **Back up `zen-themes.json`** in your profile folder. If Zen finds invalid JSON in this file, it resets it to empty, which removes all your installed mods.
6. **Register the mod.** Add the entry from [`zen-themes-entry.json`](zen-themes-entry.json) at the root of this repo to the top-level object in `zen-themes.json`. With `jq`:
   ```bash
   cp zen-themes.json zen-themes.json.bak
   jq --slurpfile entry /path/to/zen-themes-entry.json '. + $entry[0]' \
     zen-themes.json.bak >! zen-themes.json
   ```
   Then check the result with `jq . zen-themes.json`. If it prints the file without an error, the JSON is valid.
7. **Start Zen**, open **Settings → Zen Mods**, and **toggle Workspace Dropdown off and then on again.**

   This step is required. On startup, Zen loads a previously generated stylesheet (`chrome/zen-themes.css`) rather than rebuilding it, so a hand-installed mod is not applied until something triggers a rebuild. Toggling the mod does that.

## Uninstalling

Remove it from **Settings → Zen Mods**. This deletes the mod's folder from your profile.

## Customizing

The variables at the top of `chrome.css` control the main sizes and colors:

| Variable | Default | Controls |
|---|---|---|
| `--wsd-row-height` | `30px` | Height of each row |
| `--wsd-list-width` | `170px` | Width of the expanded list |
| `--wsd-icon-font-size` | `14px` | Emoji size |
| `--wsd-name-font-size` | `13px` | Name text size |
| `--wsd-list-bg` | `var(--zen-colors-tertiary, Canvas)` | List background |
| `--wsd-hover-bg` | derived from the background | Hovered row background |

After editing `chrome.css` in your profile, **toggle the mod off and on in Settings** to apply the change. Restarting Zen alone does not rebuild the mod stylesheet.

For quicker experimenting, you can paste the CSS into `chrome/userChrome.css` instead and edit it live in the Browser Toolbox (Ctrl+Shift+Alt+I → Style Editor). Disable the mod while doing this so the rules don't apply twice.

## License

[MIT](LICENSE)
