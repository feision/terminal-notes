# Terminal Fix Guide

## Goal

Make Windows Terminal and PowerShell display Chinese text correctly, while keeping Kilo run scripts quiet and stable.

## Step 1. Set the terminal font

Open Windows Terminal settings and set the default font to `Cascadia Mono`.

Recommended path:

- Settings
- Profiles
- Defaults or Appearance
- Font face
- `Cascadia Mono`

## Step 2. Force UTF-8 in PowerShell

Use the PowerShell profile file:

`C:\Users\Administrator\Documents\Codex\rotesite\.kilo\PowerShell_profile.ps1`

It should contain:

```powershell
$env:LANG = 'zh_CN.UTF-8'
[Console]::InputEncoding = [System.Text.UTF8Encoding]::new($false)
[Console]::OutputEncoding = [System.Text.UTF8Encoding]::new($false)
$OutputEncoding = [System.Text.UTF8Encoding]::new($false)
chcp 65001 | Out-Null
```

## Step 3. Keep Kilo run scripts quiet

In `C:\Users\Administrator\Documents\Codex\rotesite\.kilo\run-script.ps1`, keep these settings near the top:

```powershell
$ProgressPreference = 'SilentlyContinue'
$env:CI = '1'
$env:FORCE_COLOR = '0'
$env:TERM = 'dumb'
```

## Step 4. Avoid the noisy wrapper in the run script

The Kilo run script launches Next.js directly through `node` so the terminal has one less wrapper layer.

## Step 5. Validate

Run these checks:

- Chinese text output test.
- VT / ANSI test script.
- Kilo run script startup test.

Expected result:

- Chinese text should render normally.
- VT sequences should not break the terminal session.
- The dev server should start and report a port URL.

## What not to do

- Do not use unstable font names that are not installed.
- Do not use long inline one-liners for complex shell commands.
- Do not rely on animated or progress-heavy output in Kilo run scripts.
