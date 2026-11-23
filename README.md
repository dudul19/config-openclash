<p align="center">
  <img src="https://img.shields.io/badge/core-Clash%20Meta%20(mihomo)-ec185d" alt="core mihomo">
  <img src="https://img.shields.io/badge/OpenClash-555" alt="OpenClash">
  <img src="https://img.shields.io/badge/OpenWrt%20%2F%20ImmortalWrt-555" alt="OpenWrt / ImmortalWrt">
</p>

# Config OpenClash

A short rule config for OpenClash with the Clash Meta (mihomo) core. Two files:
`dudul.yaml` holds the groups and rules, `proxy_provider/proxy.yaml` holds your
VPN accounts. Moving to another server only means editing `proxy.yaml`; nothing
else has to be picked again.

## Files

| File | What it holds |
|---|---|
| `dudul.yaml` | DNS, sniffer, groups and every rule (about 85 lines of rules, no rule files) |
| `proxy_provider/proxy.yaml` | Your VPN accounts (Trojan, VMess, VLESS examples inside) |
| `config/dudul.yaml` | Same file as `dudul.yaml`, kept for old links |

## Groups

| Group | Matches | Choices | Default |
|---|---|---|---|
| ⚡ Best Ping | All accounts in `proxy.yaml` | Automatic (lowest ping, checked every 5 minutes) | – |
| 🌍 Global | Everything not listed below | ⚡ Best Ping, DIRECT, any account | ⚡ Best Ping |
| 🏦 Bank | BCA, BRI/BRImo, Mandiri, BNI, BSI, BTN, CIMB, Jago, SeaBank, Jenius, DANA, GoPay, OVO, LinkAja, Flip | 🌍 Global, ⚡ Best Ping, DIRECT | 🌍 Global |
| 💬 Sosmed | WhatsApp, Instagram, Facebook, Messenger, Threads, TikTok, X, Telegram | 🌍 Global, ⚡ Best Ping, DIRECT | 🌍 Global |
| 🎬 Streaming | YouTube, Netflix, Spotify, Vidio, Disney+ Hotstar | 🌍 Global, ⚡ Best Ping, DIRECT | 🌍 Global |
| 🎮 Games | Free Fire, Mobile Legends, PUBG, CoDM, Roblox, Genshin/HSR/ZZZ, Valorant, Steam | 🌍 Global, ⚡ Best Ping, DIRECT | 🌍 Global |
| 🛡️ Ads Block | Google ads, in-app ad networks (AppLovin, Unity, Vungle, InMobi, ...) | REJECT, DIRECT | REJECT |

LAN and local IPs always go DIRECT.

Only 🌍 Global lists the accounts. The other groups point at 🌍 Global, so when
you change servers they follow it on their own.

## Install

1. In OpenClash, set the core to **Clash Meta (mihomo)**.
2. Config Manage: upload `dudul.yaml`.
3. Copy `proxy_provider/proxy.yaml` to `/etc/openclash/proxy_provider/proxy.yaml`
   (Config Editor or File Manager).
4. Put your accounts in `proxy.yaml` (see below).
5. Commit settings, then Apply.

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

Add one line in `rules:` in `dudul.yaml`, above `MATCH`:

```yaml
  - DOMAIN-SUFFIX,example.com,💬 Sosmed
```

The last part is the group name, or `DIRECT` / `REJECT`.

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
