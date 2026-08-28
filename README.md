# antarctic-configs

Live preset feed for [AntarcticVPN](https://github.com/Vlad540/AntarcticVPN) —
self-hosted VPN for Android with one-tap server installation.

The app ships a bundled copy of this file and refreshes it from here, so new
transport + security combinations and updated reliability ratings reach users
without a new APK release.

- Feed URL: `https://raw.githubusercontent.com/Vlad540/antarctic-configs/main/presets/presets.json`
- The URL is configurable in the app: **Settings -> Preset feed**.

## Format

```jsonc
{
  "schemaVersion": 1,          // increased only on breaking changes
  "updated": "2026-08-28",
  "minAgentVersion": "1.0.0",  // presets are hidden for older server agents
  "presets": [
    {
      "id": "vless-reality-vision",       // stable, referenced by installed configs
      "name": { "ru": "...", "en": "..." },
      "protocol": "vless",                // vless | vmess | trojan | shadowsocks | hysteria2 | tuic | anytls
      "transport": "tcp",                 // tcp | ws | grpc | httpupgrade | none
      "security": "reality",              // reality | tls | shadowtls | none
      "rfReliability": 5,                 // 1-5, resilience against Russian DPI
      "requiresDomain": false,            // true = a domain with a certificate is needed
      "allowSelfSigned": false,           // true = works with a self-signed certificate
      "defaultPort": 443,
      "udp": false,                       // true = UDP based (QUIC)
      "handshakeCandidates": ["www.microsoft.com"],
      "method": "2022-blake3-aes-128-gcm", // Shadowsocks only
      "flow": "xtls-rprx-vision",          // VLESS only
      "tags": ["recommended", "no-domain"],
      "notes": { "ru": "...", "en": "..." }
    }
  ]
}
```

## Reliability ratings (rfReliability)

| Rating | Meaning |
| --- | --- |
| 5 | Works through aggressive DPI; indistinguishable from ordinary TLS |
| 4 | Reliable today, minor fingerprinting risk |
| 3 | Usually works, well-known pattern or UDP dependent |
| 2 | Detectable pattern, use as a fallback |
| 1 | Historical, likely blocked |

## Editing rules

1. Never change or reuse an existing `id` — installed client configs reference it.
2. Bump `updated` on every change.
3. Keep both `ru` and `en` strings for `name` and `notes`; the app has both locales.
4. Validate before pushing: `python3 -c "import json;json.load(open('presets/presets.json'))"`.
5. Raising `schemaVersion` hides the feed from older apps, which then fall back
   to their bundled copy — only do it for breaking changes.

## License

MIT
