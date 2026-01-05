# Network Infrastructure

> Router-Setup, Server-Konfiguration und Netzwerk-Topologie

**Status:** 🔴 Draft
**Zuletzt aktualisiert:** Januar 2026

---

## Übersicht

Das Neural Flight System verwendet ein dediziertes 5GHz WiFi-Netzwerk für niedrige Latenz und Isolation vom Futurium-Besuchernetzwerk.

## Voraussetzungen

- [ ] WiFi 6 Router (z.B. TP-Link Archer AX23)
- [ ] Ethernet-Kabel (Cat6)
- [ ] Server (Laptop oder Raspberry Pi 5)

## Inhalt

### Netzwerk-Topologie

```
SSID: ICAROS-Lab (5GHz, versteckt)
Subnetz: 192.168.10.0/24
Gateway: 192.168.10.1

Geräte:
- Router:     192.168.10.1
- ESP32 #1:   192.168.10.10
- ESP32 #2:   192.168.10.11
- Quest #1:   192.168.10.20
- Quest #2:   192.168.10.21
- Server:     192.168.10.100
```

### Router-Konfiguration

[Wird ergänzt: Schritt-für-Schritt Setup]

### DHCP-Reservierungen

[Wird ergänzt: MAC-Adressen und IP-Zuordnung]

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| Hohe Latenz | Festen 5GHz Kanal wählen (nicht Auto) |
| Verbindungsabbrüche | Router-Firmware aktualisieren |
| Quest findet SSID nicht | Versteckte SSID manuell eingeben |

## Referenzen

- [TP-Link Archer AX23 Manual](https://www.tp-link.com/de/support/download/archer-ax23/)

---

*Teil des [Neural Flight](../README.md) Projekts*
