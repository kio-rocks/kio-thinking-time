# Strategy Canvas — Entwickler-Dokumentation

> Interne Doku für Build, Sync und Wartung des Strategy-Canvas-Moduls.
> Die `README.md` im selben Ordner wird automatisch aus `projekt.yaml` generiert
> und ist für Endnutzer (Download-Links, Setup-Anleitung) — nicht hier editieren.

---

## SSOT-Kette

```
docs/product/sources/thinking-time-skill-ecosystem/skills/{slug}/    (SSOT)
         ↓  scripts/build-thinking-time-strategy-canvas-skills.py
kio-thinking-time/strategy-canvas/skills/{slug}/                     (Gespiegelt)
         ↓  scripts/build-chat-skills.sh --target thinking-time
kio-thinking-time/strategy-canvas/dist/{slug}.skill                  (ZIP für Claude Desktop)
         ↓  scripts/build-chat-skills.sh --target thinking-time --sync
kio-rocks/kio-thinking-time (GitHub Distribution-Repo)
```

---

## Build-Befehle

### 1. Source-Mirror (SSOT → Modul)

```bash
# Dry-Run: welche Dateien würden gespiegelt?
python3 scripts/build-thinking-time-strategy-canvas-skills.py --dry-run

# Mirror + SKILL.md-Generierung (inkl. knowledge/faq.md → shared/faq.md)
python3 scripts/build-thinking-time-strategy-canvas-skills.py

# Verify: Drift-Check gegen SSOT
python3 scripts/build-thinking-time-strategy-canvas-skills.py --verify
```

**Was das Script tut:**

1. Spiegelt alle `.md`-Dateien aus `skills/{slug}/` (rekursiv) 1:1 ins Target.
2. Wendet Path-Rewrite an: `../../../../knowledge/` → `../../../../shared/` (Zielstruktur ist identisch, nur Ordner-Name unterscheidet sich).
3. Generiert eine `SKILL.md` pro Skill (Phasen-Router) aus dem YAML-Block in `process.md`.
4. Kopiert `knowledge/faq.md` → `shared/faq.md` (die anderen 3 shared-Files sind manuell gepflegt).
5. Stale-Cleanup: Entfernt Target-Files und -Skills die nicht mehr in Source existieren.

### 2. ZIP-Build + Export

```bash
# Verify ob dist/ aktuell ist
bash scripts/build-chat-skills.sh --target thinking-time --verify

# Baut ZIPs in dist/ und kopiert nach _export/
bash scripts/build-chat-skills.sh --target thinking-time --export --force
```

Output:
- `kio-thinking-time/strategy-canvas/dist/core-competency.skill`
- `kio-thinking-time/strategy-canvas/dist/annual-plan.skill`
- `kio-thinking-time/strategy-canvas/dist/strategy-canvas-alle-skills.zip` (Bundle)
- `kio-thinking-time/_export/*.skill` (Kopien für einfachen Claude-Desktop-Upload)

### 3. Sync zum Distribution-Repo

```bash
bash scripts/build-chat-skills.sh --target thinking-time --sync
```

Pushed zu `kio-rocks/kio-thinking-time` (siehe `DIST_REPO_SLUG` Target-Block in `scripts/build-chat-skills.sh`).

**Pre-Sync-Gates (intern im Script, hart):**

| Gate | Zeitpunkt | Verhalten bei Verstoß |
|------|-----------|----------------------|
| Visibility-Check | Vor dem Clone — `gh repo view` Abgleich mit `EXPECTED_VISIBILITY=PUBLIC` | Exit 2 |
| Klarnamen-Audit | Nach rsync in Clone-Dir — Grep gegen Blacklist aus `_raw/` Top-Level-Dirs | Exit 2 |
| Post-Sync-Verify | Nach `git push` — Remote-Tree via `gh api` vs. lokalem Push-Set | Warning (nicht-fatal) |

Die Klarnamen-Blacklist wird zur Laufzeit dynamisch aus `docs/product/sources/thinking-time-skill-ecosystem/_raw/` abgeleitet — wächst automatisch mit, sobald neue Legacy-Einträge dazukommen.

---

## End-to-End-Publish (empfohlener Pfad)

Kompletten Publish-Prozess nutzen statt Einzel-Befehle:

```bash
# Erstellt bei Bedarf Mirror, baut ZIPs, synct zu kio-rocks/kio-thinking-time
bash scripts/build-chat-skills.sh --target thinking-time --export --force  # Build
git add kio-thinking-time/strategy-canvas/dist/ kio-thinking-time/strategy-canvas/README.md kio-thinking-time/strategy-canvas/.versions.json kio-thinking-time/_export/
git commit -m "build(thinking-time): Skills neu gebaut"
git push origin main
bash scripts/build-chat-skills.sh --target thinking-time --sync
```

Skill-Wrapper: `.claude/skills/thinking-time-publishing/SKILL.md` (5-Phasen-Workflow mit Preflight).

---

## Neuen Skill hinzufügen

1. Source-Skill unter `docs/product/sources/thinking-time-skill-ecosystem/skills/{neuer-slug}/` anlegen (siehe `skill-folder-convention.md`).
2. `python3 scripts/build-thinking-time-strategy-canvas-skills.py --dry-run` — prüft welche Dateien gespiegelt würden.
3. `python3 scripts/build-thinking-time-strategy-canvas-skills.py` — spiegelt + generiert SKILL.md.
4. `build_description()` in `scripts/build-thinking-time-strategy-canvas-skills.py` um Trigger-Phrasen für den neuen Slug ergänzen (dict-Eintrag im `trigger_phrases` Dict).
5. `projekt.yaml` um Skill-Eintrag ergänzen.
6. `bash scripts/build-chat-skills.sh --target thinking-time --export --force` — ZIP-Build.

---

## Bekannte Issues

Aktuell keine.

**Gefixt (2026-04-20):** README-Generator nutzte hardcoded `kio-rocks/claude-chat` auch für `--target thinking-time`-Builds → 404 beim User-Download.
Fix: `scripts/lib/generate-chat-readme.py` verlangt jetzt Pflicht-Argument `--repo <owner/repo>` (kein Default); `scripts/build-chat-skills.sh` reicht `DIST_REPO_SLUG` aus dem Target-Block durch. Neue Targets schlagen beim Build explizit fehl, wenn der Slug vergessen wird — statt still mit falschen URLs zu publizieren.
