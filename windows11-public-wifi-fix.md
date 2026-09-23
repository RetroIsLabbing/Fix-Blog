# Fix: Windows 11 Fails to Connect to Public Wi-Fi Networks

## Context

Windows 11 sometimes fails to authenticate on open/public Wi-Fi networks — airports, cafes, hotels, campus hotspots — even when the network shows as available and other devices connect without issue. The connection attempt either hangs, drops immediately, or shows "Can't connect to this network."

This usually isn't a driver or hardware problem. It's tied to how Windows negotiates TLS during EAP (Extensible Authentication Protocol) authentication through the Remote Access service (RasMan). Many public networks use a captive-portal or authentication handshake that depends on a specific range of supported TLS versions. If Windows has an outdated, restricted, or misconfigured `TlsVersion` bitmask for EAP method 13, that handshake fails silently, and the connection is refused before you even reach the login/portal page.

Fixing this means correcting the TLS version bitmask so Windows offers the right set of TLS versions during EAP negotiation.

## Symptom

- Windows 11 fails to connect to open/public Wi-Fi networks.
- The network appears in the list and shows full signal, but connection fails or times out.
- Private/WPA2-WPA3 networks connect normally — only open/public networks are affected.

## Fix

1. Open **Registry Editor** (`regedit`).
2. Navigate to:
   ```
   HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13
   ```
3. Locate (or create) the DWORD (32-bit) value:
   ```
   TlsVersion
   ```
4. Set its value data to:
   ```
   FC0 (Hexadecimal)
   ```
5. Close Registry Editor and restart your PC.
6. Reconnect to the public Wi-Fi network.

## Notes

- This edits the TLS version bitmask used for EAP authentication — back up the registry key (right-click → Export) before making changes.
- Requires administrator privileges to edit `HKEY_LOCAL_MACHINE`.
- If the `13` subkey doesn't exist under `EAP`, create it manually, then add the `TlsVersion` DWORD inside it.
- Always double check the exact path — editing the wrong key under `HKEY_LOCAL_MACHINE\SYSTEM` can affect other network services.
