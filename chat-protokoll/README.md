# Chat-Protokoll — Friction Documentation Tool

Isoliertes Werkzeug zum Export von Claude-Desktop-Friction-Sessions als strukturierte Markdown-Protokolle (Schema 2 — bias-getrennt). Buddy schreibt Roh-Daten und Buddy-Hypothese in getrennten Top-Level-Sektionen, damit Claude Code beim Konsum die Hypothese erst nach eigener Analyse als Sanity-Check liest. Tool-übergreifend einsetzbar (kio-one-year-plan, kio-core-competency, rocks, cfs, ...). Drop-Ziel der erzeugten Protokolle: kio-thinking-time/_protocols/.

## Download

**[Alle Skills herunterladen (1 Skills)](https://github.com/kio-rocks/kio-thinking-time/raw/main/chat-protokoll/dist/chat-protokoll-alle-skills.zip)**

Oder einzeln:

| Skill | Download |
|-------|----------|
| `chat-protokoll` | [chat-protokoll.skill](https://github.com/kio-rocks/kio-thinking-time/raw/main/chat-protokoll/dist/chat-protokoll.skill) |

## Einrichtung

1. In Claude ein neues Projekt anlegen
2. `projektanweisung.md` als Projektanweisung einfügen
3. Alle `.skill`-Dateien unter *Fähigkeit hochladen* hinzufügen

Claude führt ab hier durch den Rest.

## Skills

| Skill | Was er tut |
|-------|-----------|
| `chat-protokoll` | Erstellt strukturierte Chat-Protokolle nach KIO-Konvention für nachgelagerte Skill-Optimierung via Claude Code. Schema 2 trennt bias-freie Roh-Daten (Turn-Verlauf, Anhang) von bias-anfälliger Buddy-Hypothese (Diagnose, Patch-Vorschlag, Prüfpunkte) in getrennten Top-Level-Sektionen.
 |
