# WORKFLOW

> Maschinenlesbarer Workflow für Claude Code – Neural Flight Recherche-Projekt

---

## OVERVIEW

```
CHECKPOINT → RESEARCH → DOCUMENT → REVIEW → COMMIT
     ↑                                  │
     └──────────── (iterate) ───────────┘
```

Dieses Projekt ist in der **Recherche-Phase**. Es gibt keinen Code – nur Dokumentation.

---

## STEPS

### STEP: checkpoint

TRIGGER: Vor größeren Änderungen ODER Start einer neuen Recherche-Session
ACTION: `git add -A && git commit -m "checkpoint: before [description]"`
EXIT: Commit erstellt

---

### STEP: research

TRIGGER: Neues Thema recherchieren ODER Informationen sammeln

TOOL_PRIORITY:
1. `mcp__perplexity__research` → Tiefgehende Recherche
2. `mcp__perplexity__code_search` → Code-Beispiele, Libraries
3. `mcp__perplexity__science_search` → Wissenschaftliche Papers
4. `mcp__context7__` → Library-Dokumentation
5. `WebSearch` → Aktuelle Infos, Produkte
6. `WebFetch` → Spezifische Seiten lesen

SUBAGENT_STRATEGY:
```
IF multiple_topics:
  # Parallel Research (max 3 Agents)
  Task(subagent_type="Explore", prompt="Research [TOPIC_1]", model="haiku", run_in_background=true)
  Task(subagent_type="Explore", prompt="Research [TOPIC_2]", model="haiku", run_in_background=true)
  Task(subagent_type="Explore", prompt="Research [TOPIC_3]", model="haiku", run_in_background=true)
  # Wait for results
  TaskOutput(task_id=..., block=true)
```

OUTPUT: Notizen, Links, Erkenntnisse gesammelt
EXIT: Recherche abgeschlossen, bereit zum Dokumentieren

---

### STEP: document

TRIGGER: Nach research ODER wenn Informationen vorliegen

ACTIONS:
1. Zieldatei identifizieren (docs/[category]/[topic].md)
2. Bestehenden Inhalt lesen (falls vorhanden)
3. Neue Informationen strukturiert einarbeiten
4. Status-Badge aktualisieren (🔴 Draft → 🟡 In Progress → 🟢 Complete)
5. Referenzen und Links hinzufügen

QUALITY_CRITERIA:
- Klare Überschriften und Struktur
- Konkrete, actionable Informationen
- Links zu Quellen
- Troubleshooting-Tabelle wo sinnvoll
- Keine Duplikation mit anderen Dateien

EXIT: Dokumentation aktualisiert

---

### STEP: review

TRIGGER: Nach document ODER vor commit

PHASE_1_STRUCTURE:
```
1. Prüfe: Alle Links funktionieren (relative Pfade korrekt?)
2. Prüfe: Status-Badge aktuell?
3. Prüfe: Datum aktualisiert?
4. Prüfe: Keine TODO-Platzhalter vergessen?
```

PHASE_2_CONTENT:
```
IF significant_additions:
  # Review Agent für Qualitätsprüfung
  Task(
    subagent_type="general-purpose",
    model="haiku",
    prompt="""
    Review this documentation for quality.

    FILE: [PATH]
    CONTENT: [CONTENT]

    CHECK:
    1. Ist die Information korrekt und vollständig?
    2. Ist die Struktur klar und konsistent?
    3. Fehlen wichtige Aspekte?
    4. Gibt es Widersprüche zu anderen Dokumenten?

    RESPONSE:
    - "PASS" wenn alles gut
    - "IMPROVE: [konkrete Vorschläge]" wenn Verbesserungen nötig
    """
  )
```

MAX_ITERATIONS: 2
EXIT: Review bestanden ODER max_iterations erreicht

---

### STEP: commit

TRIGGER: Nach review
ACTIONS:
1. `git add -A`
2. `git commit -m "type: emoji description"`
3. Update: `dev/UEBERGABE.md` mit aktuellem Stand

COMMIT_TYPES:
| Type | Emoji | Wann? |
|------|-------|-------|
| docs | 📝 | Dokumentation hinzugefügt/aktualisiert |
| research | 🔬 | Recherche-Ergebnisse dokumentiert |
| structure | 📁 | Ordnerstruktur geändert |
| fix | 🐛 | Fehler in Dokumentation korrigiert |
| chore | 🔧 | Meta-Änderungen (Workflow, Config) |

EXIT: Commit erstellt, UEBERGABE.md aktuell

---

## SESSION_WORKFLOW

### Session Start
```
1. Read: dev/UEBERGABE.md (Was war der letzte Stand?)
2. Read: docs/ROADMAP.md (Was ist das nächste Ziel?)
3. Checkpoint erstellen
4. Mit Recherche/Dokumentation beginnen
```

### Session End
```
1. Letzte Änderungen committen
2. dev/UEBERGABE.md aktualisieren:
   - Was wurde gemacht?
   - Was ist der nächste Schritt?
   - Offene Fragen?
3. Final commit: "chore: 🔧 update handover docs"
```

---

## CONSTRAINTS

- NEVER: Code schreiben (wir sind in der Recherche-Phase)
- NEVER: AI/Claude in Commits erwähnen
- ALWAYS: Quellen angeben bei Recherche
- ALWAYS: UEBERGABE.md am Session-Ende aktualisieren
- PREFER: Kleine, thematisch fokussierte Commits
- PREFER: Parallele Recherche-Agents für mehrere Themen

---

## DOCUMENTATION_CATEGORIES

| Ordner | Themen |
|--------|--------|
| docs/hardware/ | ESP32, Sensoren, Netzwerk, Montage |
| docs/software/ | WebXR, Frameworks, Tools, Server |
| docs/integration/ | Architektur, Protokolle, Quest Workflow |
| docs/milestones/ | Meilenstein-Definitionen und Fortschritt |
| docs/research/ | Wissenschaft, Embodiment, Motion Sickness |
| docs/collaboration/ | Kooperationen, Contribution |

---

## RESEARCH_TEMPLATES

### Für Hardware-Recherche:
```
- Was ist das Produkt/die Komponente?
- Technische Spezifikationen
- Wo kaufen? (Links, Preise)
- Wie integrieren? (Pinout, Protokolle)
- Bekannte Probleme und Lösungen
```

### Für Software-Recherche:
```
- Was macht die Library/das Framework?
- Installation und Setup
- Basis-Beispiel
- Integration mit unserem Stack
- Alternativen und Trade-offs
```

### Für wissenschaftliche Recherche:
```
- Kernkonzept erklären
- Relevanz für unser Projekt
- Wichtige Papers/Quellen
- Praktische Implikationen
```
