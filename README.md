# CoachLoggerReleases

Public release artifacts for [CoachLoggerApp](https://github.com/PEACH-INNOVATIONS/STMLoggerCoachDisplay)
(private source repo). The ESP32-S3 OTA updater ([ESPOTAUpdater](https://github.com/PEACH-INNOVATIONS/ESPOTAUpdater))
fetches `manifest.txt` over HTTPS to check for and download new firmware —
`raw.githubusercontent.com` always serves this file's current content on
`main`, so it acts as a "latest version" pointer with no ESP32-side change
needed per release.

## Cutting a release

1. Build `CoachLoggerApp` and produce a flat binary:
   ```
   arm-none-eabi-objcopy -O binary coachloggerapp_verify.elf CoachLoggerApp_vN.bin
   ```
2. Compute its size and CRC32 (must match `NvStore.c`/`ota_uart.c`'s
   IEEE 802.3/zlib CRC32 — Python's `zlib.crc32()` is the same algorithm):
   ```
   python3 -c "import zlib; d=open('CoachLoggerApp_vN.bin','rb').read(); print(len(d), hex(zlib.crc32(d)))"
   ```
3. Bump `APP_FW_VERSION` in `CoachLoggerApp/Core/Inc/Version.h` to match `N`.
4. Add `CoachLoggerApp_vN.bin` to this repo, update `manifest.txt`'s
   `version`/`url`/`size`/`crc32` to match, commit, push to `main`.
