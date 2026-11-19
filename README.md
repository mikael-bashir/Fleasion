> [!CAUTION]
> This is a personal fork of [Fleasion](https://github.com/fleasion/Fleasion). It is not affiliated with or endorsed by the Fleasion project. Use at your own risk. See the disclaimer below.

# Fleasion — Custom Patch

This fork applies macOS-specific fixes on top of upstream Fleasion. The original project and its docs live at [github.com/fleasion/Fleasion](https://github.com/fleasion/Fleasion).

## What changed

- **TLS fix (macOS, Apple Silicon):** Roblox Player 0.748+ reads startup FFlags from `~/Library/Roblox/ClientSettings/ClientAppSettings.json` as well as the app bundle path. This patch seeds `DFFlagHttpUseEmbeddedCaCerts=False` into both locations, so the proxy CA is trusted regardless of which path the client reads first. Without this, TLS verification fails and the proxy cannot intercept traffic.

- **Env Proxy + Account Manager (macOS):** The Launch button in the Account Manager (Miscellaneous tab) was calling the Roblox launcher directly, bypassing the Env Proxy lifecycle controller. The proxy environment was never injected, so traffic went unintercepted. This patch routes the launch through the lifecycle controller.

- **Live config reload:** Editing a config JSON in `configs/` with an external editor now immediately refreshes the Dashboard rule list. The original code only watched for file additions and removals, not in-place edits. This patch adds `fileChanged` watching to `folder_watcher.py`.

- **TLS fix (Windows):** Scans `%LocalAppData%\Roblox\Versions` and `C:\Program Files (x86)\Roblox\Versions` for installed Player version directories and seeds `DFFlagHttpUseEmbeddedCaCerts=False` into `ClientSettings\ClientAppSettings.json` alongside each executable. This runs in addition to the existing `flag_cache.dat` seeding, covering cases where `flag_cache.dat` does not yet exist (fresh install or post-update) or where the JSON file is read first.

- **Python 2 except syntax fix (all platforms):** 11 files contained `except TypeError, ValueError:` which in Python 3 silently catches only the first type. Fixed to `except (TypeError, ValueError):` throughout.

## Tested on

- macOS (Apple Silicon), Roblox Player 0.748+, Env Proxy mode, tested end-to-end
- Windows 11 ARM: patch logic verified; full end-to-end test pending VM setup

# Using Fleasion

- Delete your Roblox cache with the fleasion ui
- Log in to Roblox with a supported browser like Chrome
- Load your auth tokens with "import browser login" at "Miscellaneous", then click "switch to selected", then click "Launch" once you
have pasted a game link (you can find this for your game at the [Roblox home page](https://www.roblox.com/home), or from a private server owner, or launch without a link).

## Disclaimer

This fork is provided as-is for personal use. The author makes no guarantees about its correctness, safety, or fitness for any purpose. You are solely responsible for how you use this software and for any consequences that result. This is not official Fleasion software.
