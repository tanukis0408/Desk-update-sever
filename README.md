# Update Server

## How to publish a new release

1. Build with signing:
```powershell
$env:TAURI_SIGNING_PRIVATE_KEY_PATH = "../updater-keys"
$env:TAURI_SIGNING_PRIVATE_KEY_PASSWORD = "desk-updater-key"
npx tauri build --bundles nsis
```

2. Upload `src-tauri/target/release/bundle/nsis/desk_<version>_x64-setup.exe` to server.

3. Generate signature for the installer:
```powershell
npx tauri signer sign -k "../updater-keys" -p "desk-updater-key" "../target/release/bundle/nsis/desk_<version>_x64-setup.exe"
```

4. Update `update.json` with new version, URL, signature, and release date.

## Files to host on `releases.codenamedesk.com`

```
/update.json          — Update manifest (served at https endpoint)
/download/            — Installer files
  desk_0.1.0_x64-setup.exe
```

## update.json format

```json
{
  "version": "0.2.0",
  "notes": "Что нового в этой версии",
  "pub_date": "2026-07-10T12:00:00Z",
  "platforms": {
    "windows-x86_64": {
      "signature": "dW50cnR...",
      "url": "https://releases.codenamedesk.com/download/desk_0.2.0_x64-setup.exe"
    }
  }
}
```

## Security

- Private key: `updater-keys` (DO NOT COMMIT, keep in secure storage)
- Public key: `updater-keys.pub` (safe to commit, embedded in app)
- Password: set during key generation
