> [!CAUTION]
> This is a personal fork of [Fleasion](https://github.com/fleasion/Fleasion). It is not affiliated with or endorsed by the Fleasion project. Use at your own risk. See the disclaimer below.

# Fleasion — Custom Patch

This fork applies macOS-specific fixes on top of upstream Fleasion. The original project and its docs live at [github.com/fleasion/Fleasion](https://github.com/fleasion/Fleasion).

## What changed

- **TLS fix (macOS, Apple Silicon):** Roblox Player 0.748+ reads startup FFlags from `~/Library/Roblox/ClientSettings/ClientAppSettings.json` as well as the app bundle path. This patch seeds `DFFlagHttpUseEmbeddedCaCerts=False` into both locations, so the proxy CA is trusted regardless of which path the client reads first. Without this, TLS verification fails and the proxy cannot intercept traffic.

- **Env Proxy + Account Manager (macOS):** The Launch button in the Account Manager (Miscellaneous tab) was calling the Roblox launcher directly, bypassing the Env Proxy lifecycle controller. The proxy environment was never injected, so traffic went unintercepted. This patch routes the launch through the lifecycle controller.

- **Live config reload:** Editing a config JSON in `configs/` with an external editor now immediately refreshes the Dashboard rule list. The original code only watched for file additions and removals, not in-place edits. This patch adds `fileChanged` watching to `folder_watcher.py`.

## Tested on

- macOS (Apple Silicon), Roblox Player 0.748+
- Env Proxy mode

## Disclaimer

This fork is provided as-is for personal use. The author makes no guarantees about its correctness, safety, or fitness for any purpose. You are solely responsible for how you use this software and for any consequences that result. This is not official Fleasion software.
