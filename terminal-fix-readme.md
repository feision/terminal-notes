# Terminal Fix README

This note summarizes the terminal and shell fixes applied in the `Codex` workspace to make Windows Terminal, PowerShell, and Kilo behave more reliably with Chinese text and noisy output.

## Quick Summary

- Windows Terminal now uses `Cascadia Mono` as the default font.
- PowerShell is configured to use UTF-8 for input and output.
- The Kilo run script was simplified to reduce noisy output and avoid terminal breakage.
- The dev server is started through `node` directly instead of the extra `pnpm` wrapper in the Kilo run path.

## Why this matters

- Chinese text was not rendering cleanly on the English Windows system.
- Some commands caused Kilo sessions to appear to exit unexpectedly.
- Some output showed raw ANSI / VT control sequences such as `[555;80;74M`.

## Files changed

- `C:\Users\Administrator\AppData\Local\Packages\Microsoft.WindowsTerminal_8wekyb3d8bbwe\LocalState\settings.json`
- `C:\Users\Administrator\Documents\Codex\rotesite\.kilo\PowerShell_profile.ps1`
- `C:\Users\Administrator\Documents\Codex\rotesite\.kilo\run-script.ps1`

## Validation

- Verified PowerShell can print Chinese text after forcing UTF-8.
- Verified VT escape sequences can pass through a plain PowerShell script.
- Verified the Kilo run script starts the dev server successfully.

## Notes

- `pnpm` is still the normal package manager for day-to-day development.
- The Kilo run script only avoids the `pnpm` wrapper to keep output quieter.
