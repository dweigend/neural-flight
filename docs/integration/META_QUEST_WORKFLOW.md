# Meta Quest Workflow

> ADB, HTTPS und Testing-Workflow auf der Meta Quest 3

**Status:** 🔴 Draft
**Zuletzt aktualisiert:** Januar 2026

---

## Übersicht

Workflow für die Entwicklung und das Testen von WebXR-Anwendungen auf der Meta Quest 3, inklusive ADB-Debugging und HTTPS-Konfiguration.

## Voraussetzungen

- [ ] Meta Quest 3 im Developer Mode
- [ ] ADB installiert (via SideQuest oder Android SDK)
- [ ] USB-C Kabel für ADB-Verbindung

## Inhalt

### Developer Mode aktivieren

[Wird ergänzt: Meta Developer Account, Quest App Settings]

### ADB-Verbindung

```bash
# Quest per USB verbinden
adb devices

# Port-Forwarding für lokale Entwicklung
adb reverse tcp:5173 tcp:5173
adb reverse tcp:3000 tcp:3000
```

### Quest Browser Testing

[Wird ergänzt: URL-Eingabe, Zertifikat-Akzeptanz]

### Wireless ADB

[Wird ergänzt: Kabellose Debugging-Option]

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| "no devices" | USB-Debugging auf Quest erlauben |
| "unauthorized" | Quest abnehmen, USB neu verbinden, Dialog bestätigen |
| Zertifikat-Warnung | Manuell akzeptieren oder mkcert verwenden |

## Referenzen

- [Meta Quest Developer](https://developer.oculus.com)
- [ADB Platform Tools](https://developer.android.com/tools/releases/platform-tools)
- [SideQuest](https://sidequestvr.com)

---

*Teil des [Neural Flight](../README.md) Projekts*
