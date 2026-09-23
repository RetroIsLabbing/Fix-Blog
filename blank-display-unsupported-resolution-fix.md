# Fix: Blank/No Display After Setting an Unsupported Monitor Resolution

## Context

Manually changing a monitor's resolution to one it doesn't actually support can cause the screen to go blank immediately, with no way to see the display settings to undo the change. This typically happens when a resolution is applied that falls outside the monitor's native refresh rate or supported resolution range — the GPU sends a signal the monitor can't render, so there's nothing shown on screen.

Since you can't see the screen, the fix has to be done blind, using Windows' Safe Mode and Device Manager to force the graphics driver back to a default, universally supported state.

## Symptom

- Display goes black/blank immediately after changing screen resolution.
- Monitor shows no signal, or shows "no signal" / out-of-range message.
- PC is otherwise running (fans, lights, login sounds) — only the display output is affected.

## Fix

1. Boot the PC into **Safe Mode**.
2. Once in Safe Mode, open **Device Manager**:
   - Press `Windows key + R`
   - Type `devmgmt.msc` and press Enter
3. Expand **Display adapters**.
4. Right-click the listed graphics adapter and select **Uninstall device**.
5. Restart the PC normally (not into Safe Mode).
6. Allow some time on boot — Windows will detect the missing driver and automatically reinstall a generic/default display driver at a safe, supported resolution.

## Notes

- Uninstalling the display adapter in Device Manager doesn't remove the physical hardware — it forces Windows to rebuild the driver from scratch on next boot.
- The first boot after uninstalling may take longer than usual and may briefly flicker or go black again as Windows reinitializes the driver — this is expected.
- Once the display is back, you can go into **Settings > Display** and manually select a resolution the monitor actually supports before making any further changes.
- If Safe Mode itself doesn't produce a picture either, the issue is more likely a hardware/cable fault than a resolution/driver problem.
