# Markenschnitt

Claude schneidet Reels im Markenstil des Kunden. Entstanden mit Katja Schlecht (Brand Consulting) als Ergänzung ihrer Markenberatung.

## Aufbau

- `index.html` – Einstellseite, über GitHub Pages unter markenschnitt.byfroehlich.de. Schriften liegen selbst gehostet in `fonts/`. Erzeugt den Einrichtungstext für die Claude-App (Tab „Code“).
- `skill/SKILL.md` – die Schnittregeln. Wird beim Kunden als `~/.claude/skills/markenschnitt/SKILL.md` abgelegt.
- `skill/VERSION` – aktuelle Versionsnummer. Der Skill prüft sie höchstens einmal am Tag und fragt bei höherer Nummer den Kunden und lädt dann die neue `SKILL.md` (mit Prüfung von Kopf und Versionszeile).

Beim Kunden liegen zwei Schichten:

1. `SKILL.md` – unsere Regeln, werden per Update ersetzt.
2. `stil.md` – Stil des Kunden aus Einstellseite und Beratung, plus „Gelernt“ aus „Merk dir das“. Wird von Updates nie angefasst.

## Neue Fassung veröffentlichen

1. Regeln in `index.html` (Funktion `buildPrompt`, Teil zwischen SKILL und STIL) ändern.
2. `skill/SKILL.md` daraus neu erzeugen und die Version (ganze Zahl, Konstante `VERSION`) dort und in `skill/VERSION` erhöhen.
3. Auf `main` mergen. Kunden bekommen die Fassung beim nächsten Schnitt.

## Sicherheit

Wer auf `main` schreiben kann, ändert die Anweisungen bei allen Kunden. Deshalb: Zwei-Faktor-Anmeldung für das GitHub-Konto, Branch-Schutz auf `main`, Änderungen nur per Pull Request.

## Wege

- Mac mit Apple-Chip: Schnitt in Edits für Mac über dessen Automation Server (MCP).
- Andere Rechner: ffmpeg und faster-whisper auf dem Rechner, Ergebnis als MP4 (Testphase).

Videos bleiben auf dem Rechner des Kunden.
