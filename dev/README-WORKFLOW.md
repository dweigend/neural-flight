# Workflow Guide

> Wie wir in diesem Projekt arbeiten – für Menschen erklärt.
> Die maschinenlesbare Version für Claude Code liegt in `WORKFLOW.md`.

---

## Überblick

```
┌─────────────┐    ┌──────────┐    ┌──────────┐    ┌────────┐    ┌────────┐
│ Checkpoint  │───▶│ Research │───▶│ Document │───▶│ Review │───▶│ Commit │
└─────────────┘    └──────────┘    └──────────┘    └────────┘    └────────┘
                        │               │               │
                   (wiederholen bei Bedarf)             │
                        └───────────────────────────────┘
```

**Wichtig:** Wir sind in der **Recherche-Phase**. Es wird noch kein Code geschrieben – nur Dokumentation.

---

## Die Schritte

### 1. Checkpoint

**Wann?** Vor größeren Änderungen oder am Anfang einer Session.

**Warum?** Sicherheitsnetz. Falls etwas schiefgeht, können wir zurückspringen.

```bash
git add -A && git commit -m "checkpoint: before researching sensors"
```

**Tipp:** Lieber ein Checkpoint zu viel als zu wenig.

---

### 2. Research

**Wann?** Wenn wir etwas Neues verstehen oder herausfinden müssen.

**Wie?**
1. Klares Ziel formulieren: "Was genau will ich wissen?"
2. Recherche-Tools nutzen (Perplexity, Web Search, etc.)
3. Notizen machen, Links sammeln
4. Erkenntnisse in der passenden Doku-Datei festhalten

**Tool-Priorität:**
| Tool | Wofür |
|------|-------|
| `mcp__perplexity__research` | Tiefgehende Fragen |
| `mcp__perplexity__code_search` | Code-Beispiele finden |
| `mcp__perplexity__science_search` | Papers, wissenschaftliche Quellen |
| `mcp__context7__` | Library-Dokumentation |
| `WebSearch` | Aktuelle Infos, Produkte |

**Beispiel-Recherchen:**
- "Welche WebXR-Frameworks gibt es und welches passt zu Svelte?"
- "Wie kalibriert man den BNO055 Sensor?"
- "Was ist Body Ownership Illusion und wie erzeugt man sie in VR?"

---

### 3. Document

**Wann?** Nach der Recherche, wenn Informationen vorliegen.

**Was tun?**
1. Richtige Datei finden (docs/hardware/, docs/software/, etc.)
2. Neue Infos strukturiert einarbeiten
3. Status-Badge aktualisieren:
   - 🔴 Draft → Noch Platzhalter, wenig Inhalt
   - 🟡 In Progress → Wird gerade bearbeitet
   - 🟢 Complete → Fertig und geprüft
4. Quellen und Links hinzufügen

**Qualitätskriterien:**
- Klare Struktur mit Überschriften
- Konkrete, nutzbare Informationen
- Links zu Quellen
- Troubleshooting-Tabelle wo sinnvoll

---

### 4. Review

**Wann?** Vor dem Commit, bei größeren Änderungen.

**Checkliste:**
- [ ] Alle relativen Links funktionieren?
- [ ] Status-Badge aktuell?
- [ ] Datum aktualisiert?
- [ ] Keine vergessenen Platzhalter ([TODO], [TBD])?
- [ ] Keine Widersprüche zu anderen Dokumenten?

**Bei größeren Änderungen:** Review Agent kann die Qualität prüfen.

---

### 5. Commit

**Wann?** Nach dem Review, wenn alles gut ist.

```bash
git add -A
git commit -m "docs: 📝 add BNO055 calibration guide"
```

**Commit-Typen:**

| Type | Emoji | Wann? |
|------|-------|-------|
| `docs:` | 📝 | Dokumentation hinzugefügt/aktualisiert |
| `research:` | 🔬 | Recherche-Ergebnisse dokumentiert |
| `structure:` | 📁 | Ordnerstruktur geändert |
| `fix:` | 🐛 | Fehler in Dokumentation korrigiert |
| `chore:` | 🔧 | Meta-Änderungen |

**Nicht vergessen:** `dev/UEBERGABE.md` aktualisieren!

---

## Session-Ablauf

### Am Anfang
1. `dev/UEBERGABE.md` lesen → Was war der letzte Stand?
2. `docs/ROADMAP.md` checken → Was ist das Ziel?
3. Checkpoint erstellen
4. Loslegen!

### Am Ende
1. Letzte Änderungen committen
2. `dev/UEBERGABE.md` aktualisieren:
   - Was wurde gemacht?
   - Was ist der nächste Schritt?
   - Offene Fragen?
3. Final commit

---

## Ordner-Struktur

| Ordner | Was gehört rein? |
|--------|------------------|
| `docs/hardware/` | ESP32, Sensoren, Netzwerk, Montage |
| `docs/software/` | WebXR, Frameworks, Tools, Server |
| `docs/integration/` | Architektur, Protokolle, Quest |
| `docs/milestones/` | Meilensteine, Fortschritt |
| `docs/research/` | Wissenschaft, Embodiment |
| `docs/collaboration/` | Kooperationen |
| `assets/diagrams/` | Mermaid-Diagramme |
| `assets/images/` | Screenshots, Fotos |
| `dev/` | Workflow, Übergabe, Guidelines |

---

## Goldene Regeln

1. **Quellen angeben** – Woher kommt die Information?
2. **Kleine Commits** – Ein Thema pro Commit
3. **UEBERGABE.md pflegen** – Immer am Session-Ende
4. **Kein Code** – Wir sind in der Recherche-Phase
5. **Nicht AI erwähnen** – Keine "Claude", "AI", "LLM" in Commits

---

## Tipps

- **Unsicher wo etwas hingehört?** → In die thematisch passendste Datei. Kann später verschoben werden.
- **Zu viel auf einmal?** → Aufteilen. Erst ein Thema, dann das nächste.
- **Recherche ergibt nichts?** → Andere Suchbegriffe probieren oder Frage umformulieren.
- **Widersprüchliche Infos gefunden?** → Beide dokumentieren und markieren.

---

*Dieses Dokument wächst mit dem Projekt.*
