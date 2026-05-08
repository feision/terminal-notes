# Terminal Fix Postmortem

## Incident Summary

The terminal experience in Kilo and Windows Terminal was unstable when running shell commands that produced Chinese text, progress output, or ANSI / VT control sequences.

Symptoms included:

- Broken or unreadable Chinese characters.
- Visible control codes like `[555;80;74M`.
- Shell sessions appearing to exit unexpectedly.
- Run scripts failing because of PowerShell parameter conflicts.

## Root Causes

### 1. Terminal font mismatch

The default font choice was not ideal for mixed English and Chinese terminal output.

### 2. Encoding mismatch

PowerShell output was not consistently using UTF-8, which made Chinese text unreliable.

### 3. Noisy child-process output

The Kilo run path inherited output from child processes that could emit progress bars, color codes, and VT control sequences.

### 4. Script-level error

One version of the run script used an invalid `Start-Process` parameter combination: `-NoNewWindow` and `-WindowStyle` together.

## Fixes Applied

- Set Windows Terminal to `Cascadia Mono`.
- Forced UTF-8 in the PowerShell profile.
- Added quieting environment variables in the Kilo run script.
- Removed the invalid `Start-Process` parameter pairing.
- Reworked the dev-server launch path to use `node` directly against Next.js' CLI.

## Verification

- Chinese text printed correctly in PowerShell.
- A plain PowerShell VT test emitted control codes correctly.
- The run script started a dev server on a free port and reported the URL.

## Remaining Risks

- Some child processes can still emit noisy terminal UI depending on the command.
- Kilo may still show control codes if a tool prints them in a way that bypasses the expected terminal handling.

## Lessons Learned

- Avoid long inline shell commands when multiple parsing layers are involved.
- Prefer script files over one-liners for anything non-trivial.
- Keep Kilo run scripts minimal and quiet.
- Treat font selection and shell encoding as separate problems.
