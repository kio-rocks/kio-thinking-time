# Projektanweisung — KIO Foundations (Shared Library)

> Universelle Read-Only-Reference-Skills, die in beliebigen Claude Desktop Projekten zusätzlich zu Workflow-Skills (kio-one-year-plan, kio-core-competency etc.) geladen werden, um die KIO-Foundations als Wissensbasis bereitzustellen.

---

## Was dieses Bundle ist

Dieses Bundle ist **kein eigenständiges Master-Projekt**. Es ist eine Sammlung von Reference-Skills, die als Foundation-Layer in jedes KIO-Thinking-Time-Projekt zusätzlich hochgeladen werden können.

Aktueller Inhalt (1 Skill):

| Skill | Zweck |
|-------|-------|
| `kio-foundations` | Spiegelt die drei KIO-Säulen-Indices (Modell, Prozess, Toolbox) und alle 20 Tool-Foundations als Read-Only-Wissensbasis |

---

## Setup

1. Im Ziel-Projekt (z.B. Strategy Canvas) zusätzlich zu den Workflow-Skills auch `kio-foundations.skill` aus `dist/` via „Fähigkeit hochladen" einfügen.
2. Im Projektchat ist die Foundation dann als implizite Wissensbasis verfügbar — Workflow-Skills greifen darauf zurück, statt eigene Foundations-Mirrors zu pflegen.

---

## Architektur

- **SSOT:** `knowledge/kio-modell/`, `knowledge/kio-prozess/`, `knowledge/kio-toolbox/` (im Repo)
- **Mirror:** `kio-foundations/knowledge.md` zwischen `<!-- BEGIN MIRROR -->` und `<!-- END MIRROR -->`
- **Sync:** `scripts/sync-knowledge-mirrors.py`
- **Build:** `bash scripts/build-chat-skills.sh _shared --target thinking-time`

---

## Pflege

1. SSOT-Änderungen passieren ausschließlich in `knowledge/`.
2. Sync-Script ausführen: `python3 scripts/sync-knowledge-mirrors.py`
3. Build-Script erzeugt aktualisiertes ZIP: `bash scripts/build-chat-skills.sh _shared --target thinking-time`
4. Im Ziel-Projekt das aktualisierte ZIP hochladen (alte Version vorher entfernen).

Manuelle Edits am Mirror-Block in `knowledge.md` sind verboten — sie werden beim nächsten Sync überschrieben.
