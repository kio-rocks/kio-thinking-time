# Projektanweisung — Thinking-Time Feedback

> Für alle Nutzer:innen der Thinking-Time-Skills (Teams, KIO Champions, KIO Implementer): strukturiertes Feedback zu jedem Skill.

---

## Claude Desktop Setup

1. **Claude Desktop öffnen** — [claude.ai/download](https://claude.ai/download)
2. **Neues Projekt erstellen** — Klick auf „+ New Project" in der linken Sidebar. Name: „Thinking-Time Feedback".
3. **Diese Datei einfügen** — Inhalt dieser `projektanweisung.md` in das Feld „Project Instructions" kopieren.
4. **Skill hochladen** — ZIP aus `dist/` ins Projekt hochladen (enthält den `feedback`-Skill).

Dieses Projekt läuft **neben** deinen Workshop-Projekten (z.B. `strategy-canvas`). Wenn du während oder nach einer Sitzung Feedback geben willst, wechselst du in dieses Feedback-Projekt.

---

## Zweck

Dieses Meta-Projekt nimmt dein Feedback zu **jedem** Thinking-Time-Skill entgegen — strategy-canvas (kio-one-year-plan, kio-core-competency), und künftige Module. Du gibst die Beobachtung in natürlicher Sprache an, der Skill strukturiert (Modul / Skill / Phase / Kategorie / Beschreibung) und generiert einen **pre-filled GitHub-Issue-Link**.

Ein Klick, Submit im Browser — fertig.

---

## Trigger

| Du sagst … | Claude tut … |
|---|---|
| *"Feedback zu kio-one-year-plan Phase 2"* | → `feedback` → Link für `strategy-canvas/kio-one-year-plan` |
| *"Bug im Jahresplan-Hero-DOCX"* | → `feedback` → Link mit Kategorie `hero-output` |
| *"Idee für die Moderation der Kernkompetenz"* | → `feedback` → Link mit Kategorie `idee` |
| *"Die Frage 3 in Phase 1 ist unklar"* | → `feedback` → Link mit Kategorie `frage-formulierung` |

---

## Ablauf

Der Skill arbeitet in vier Phasen:

1. **Klassifikation** — Welches Modul / welcher Skill / welche Phase ist gemeint?
2. **Kategorie** — Bug, Frage-Formulierung, Synthesis-Qualität, Hero-Output, Moderation oder Idee?
3. **Strukturieren** — Was ist passiert, was wäre erwartet, welcher Kontext (Rolle, Gruppengröße, Branche)?
4. **Link** — Markdown-Link zum vorgefüllten GitHub-Issue-Formular.

---

## GitHub-Account

Für die finale Abgabe brauchst du einen GitHub-Account. Wenn du die Thinking-Time-Skills aus `kio-rocks/kio-thinking-time` selbst installiert hast, hast du bereits einen.

---

## Ziel-Repo

[`kio-rocks/kio-thinking-time`](https://github.com/kio-rocks/kio-thinking-time) (public). Issues mit den Labels `feedback`, `thinking-time`, `modul:{modul}`, `kategorie:{kategorie}` landen dort. Das KIO-Team liest regelmäßig mit und reagiert per Issue-Kommentar.
