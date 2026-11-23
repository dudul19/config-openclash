<p align="center">
  <img src="https://img.shields.io/badge/core-Clash%20Meta%20(mihomo)-ec185d" alt="core mihomo">
  <img src="https://img.shields.io/badge/OpenClash-555" alt="OpenClash">
  <img src="https://img.shields.io/badge/OpenWrt%20%2F%20ImmortalWrt-555" alt="OpenWrt / ImmortalWrt">
</p>

# Config OpenClash

A simple rule config for OpenClash with the Clash Meta (mihomo) core.
`dudul.yaml` holds the groups, `rule_provider/` holds the domain lists and
`proxy_provider/proxy.yaml` holds your VPN accounts. Moving to another server only means editing `proxy.yaml`; nothing
else has to be picked again.

## Files

| File | What it holds |
|---|---|
| `dudul.yaml` | DNS, sniffer, groups and the rule order |
| `rule_provider/*.yaml` | Domain lists, one file per group (8 files) |
| `proxy_provider/proxy.yaml` | Your VPN accounts (Trojan, VMess, VLESS examples inside) |
| `config/dudul.yaml` | Same file as `dudul.yaml`, kept for old links |

## Groups

| Group | Matches | Choices | Default |
|---|---|---|---|
| ⚡ Best Ping | All accounts in `proxy.yaml` | Automatic (lowest ping, checked every 5 minutes) | – |
| 🌍 Global | Everything not in a rule file | ⚡ Best Ping, DIRECT, any account | ⚡ Best Ping |
| 🏦 Bank | `bank.yaml`: local banks + e-wallets (DANA, GoPay, OVO, ShopeePay, ...) | 🌍 Global, ⚡ Best Ping, DIRECT | 🌍 Global |
| 💬 Sosmed | `sosmed.yaml`: WhatsApp, Instagram, Facebook, X, Telegram, TikTok, ... | 🌍 Global, ⚡ Best Ping, DIRECT | 🌍 Global |
| 🎬 Streaming | `streaming.yaml`: YouTube, Google, Netflix, Spotify, Vidio, HBO, ... | 🌍 Global, ⚡ Best Ping, DIRECT | 🌍 Global |
| 🎮 Games | `games.yaml`: Free Fire, Mobile Legends, PUBG, CoDM, ... |
| 📦 Lainnya | `lainnya.yaml`: AI (ChatGPT, Gemini, ...), meeting (Zoom, Meet, ...), marketplace (Shopee, Tokopedia, ...), Play Store updates | 🌍 Global, ⚡ Best Ping, DIRECT | 🌍 Global | 🌍 Global, ⚡ Best Ping, DIRECT | 🌍 Global |
| 🛡️ Ads Block | `adblock.yaml` (oisd big) + `reject.yaml` (trackers) | REJECT, DIRECT | REJECT |

`direct.yaml` (LAN and local IPs) always goes DIRECT.

Only 🌍 Global lists the accounts. The other groups point at 🌍 Global, so when
you change servers they follow it on their own.

## Install

1. In OpenClash, set the core to **Clash Meta (mihomo)**.
2. Config Manage: upload `dudul.yaml`.
3. Copy `proxy_provider/proxy.yaml` to `/etc/openclash/proxy_provider/proxy.yaml`
   (Config Editor or File Manager).
4. Copy the `rule_provider/` folder to `/etc/openclash/rule_provider/`.
5. Put your accounts in `proxy.yaml` (see below).
6. Commit settings, then Apply.

## Change server

1. Config Editor → `proxy_provider/proxy.yaml`.
2. Replace the example accounts with yours and delete the ones you do not use.
3. Apply. ⚡ Best Ping picks the fastest account by itself.

Account fields:

| Field | Value |
|---|---|
| `server` | VPN server domain/IP, or the bug host (depends on your inject method) |
| `sni` / `servername`, `Host` | VPN server domain, or the bug host |
| `password` / `uuid` | From your VPN account |
| `ws-opts.path` | WebSocket path, for example `/vless` |

## Add a rule

Add one line to the matching file in `rule_provider/`, for example `sosmed.yaml`:

```yaml
  - DOMAIN-SUFFIX,example.com
```

`adblock.yaml` uses the short form `- '+.example.com'`.

## Lower ping

The config already sets `unified-delay`, `tcp-concurrent` and fake-IP DNS. If
OpenClash overrides them, turn these on in LuCI → OpenClash → Overwrite Settings → Meta:

- Unified Delay: ON
- TCP Concurrent: ON
- Mode: Fake-IP

## Notes

- Quote `on`/`off`/`yes`/`no` values in YAML (for example `find-process-mode: "off"`),
  otherwise they turn into true/false and mihomo will not start.
- Rules match domain names. Game traffic that goes straight to an IP falls
  into 🌍 Global, which is the 🎮 Games default anyway.
- The ad list is small on purpose. The old 220k-domain oisd list was removed to
  keep the router light.

## Support

<a href="https://saweria.co/dudulrealnofek"><img src="https://img.shields.io/badge/Saweria-Donate-F47C20?style=for-the-badge&logo=ko-fi&logoColor=white" alt="Saweria"></a>

Made by [Mr Dudul](https://t.me/dudulrealnofek).
