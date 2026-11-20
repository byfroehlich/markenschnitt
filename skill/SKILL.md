---
name: markenschnitt
description: Schneidet meine Reels in meinem Markenstil, in Edits oder direkt als Videodatei. Nutzen, wenn ich „Schneide mein neues Reel“ sage, ein Video geschnitten haben will oder nach einem Schnitt „Merk dir das“ sage.
---
# Markenschnitt
Version: 2

Lies vor jedem Schnitt die Datei ~/.claude/skills/markenschnitt/stil.md. Ihre Stilangaben gelten vor den Stilvorgaben in dieser Datei, die Regeln unter „Gelernt“ vor dem Grundstil. Der Abschnitt „Datenschutz“ und das Postverbot aus Schritt 11 gelten immer. Beachte Wirkung, Ansprache und Hinweise aus der Beratung in stil.md bei Hook, Abschluss und allen Schnittentscheidungen.

## Welcher Weg
- Edits-Werkzeuge verfügbar: Weg A, Schnitt in Edits.
- Keine Edits-Werkzeuge, aber ~/.markenschnitt/venv vorhanden: Weg B, Schnitt mit ffmpeg und faster-whisper.
- Keins von beidem: Sag mir, dass Edits geöffnet und der Automation Server eingeschaltet sein muss und ich danach eine neue Sitzung starten soll.

## Wenn ich „Schneide mein neues Reel“ sage
0. Weg A: Fehlt meine Schrift in Edits, lade sie zuerst als Schrift für mein Konto hoch (Datei aus meinem Videoordner).
1. Nimm das neueste Video direkt in meinem Videoordner (siehe stil.md, nicht aus dem Unterordner fertig, keine Datei mit „_markenschnitt“ im Namen), außer ich nenne eine andere Datei. Liegt es woanders, kopier es zuerst dorthin. Kann Edits die Datei nicht öffnen, frag die Edits-Werkzeuge, aus welchem Ordner Edits lesen darf, und kopier sie dorthin.
2. Erstelle ein Transkript mit Zeitstempel für jedes Wort, bei Weg A mit den Edits-Werkzeugen, bei Weg B mit faster-whisper. Wende die Schreibweisen aus stil.md an. Prüf Namen und Fachbegriffe gegen das, was wirklich gesagt wird. Merk dir Wörter, bei denen du unsicher bist.
3. Grobschnitt: Schneide Pausen raus, die länger sind als die Pausengrenze in stil.md, dazu „äh“, „ähm“, Versprecher und abgebrochene Anläufe. Von einem doppelten Anlauf bleibt der letzte vollständige. Vor und nach jedem Satz bleibt etwa 0,15 Sekunden Luft, damit es nicht gehetzt klingt. Stell nie etwas um und lass nie Inhalt weg, außer Wiederholungen.
4. Bild im Format aus stil.md. Das Gesicht bleibt gut im Bild. Halte oben und unten etwa 12 % frei von wichtigen Inhalten, dort liegen die Bedienelemente von Instagram.
5. Zoom wie in stil.md. Immer als harter Schnitt, nie als Zoomfahrt. Ein Zoom gilt für einen ganzen Gedanken, danach zurück auf die normale Einstellung.
6. Untertitel wie in stil.md, in der Sprache des Videos, Wort für Wort hervorgehoben. Setz die Schrift als letzten Schritt, weil Edits sie beim Ändern der Farbe zurücksetzen kann.
7. Hook wie in stil.md: der Satz aus dem Video, der am meisten neugierig macht, meistens der erste. Höchstens 8 Wörter, oben im Bild, in den ersten Sekunden, in meiner Schrift.
8. Abschluss wie in stil.md: Steht dort ein Abschluss-Satz, blende ihn in den letzten 2 Sekunden ein, in Markenfarbe und meiner Schrift.
9. Look wie in stil.md, als Ebene direkt über dem Video und unter Hook und Untertiteln, damit diese ihre Farben behalten.
10. Ton: Stimme entrauschen, gleichmäßig laut (ungefähr −14 LUFS), nie übersteuern.
Weg A: Alles nur mit den Edits-Werkzeugen. Installiere dafür nichts zusätzlich. Was Edits nicht kann, lässt du weg und nennst es im Bericht.
11. Weg A: Nichts exportieren und nichts posten. Lass das Projekt in Edits offen, damit ich nachbessern kann. Weg B: Speichere das fertige Reel als MP4 (H.264, AAC) im Unterordner fertig meines Videoordners, Dateiname wie das Original mit „_markenschnitt“ (die Feed-Fassung mit „_markenschnitt_4x5“). Weg B postet nie.
12. Bericht in höchstens 5 Zeilen: Länge vorher und nachher, was du geschnitten hast, unsichere Wörter in den Untertiteln und wo ich am ehesten nachschauen sollte.

## Weg B im Detail
- Spracherkennung mit faster-whisper aus ~/.markenschnitt (Modell „small“, auf schnellen Rechnern „medium“), mit Zeitstempeln je Wort.
- Aus den Wortzeiten eine Schnittliste bauen und mit ffmpeg bildgenau schneiden. Danach aufs Format aus stil.md bringen.
- Untertitel als ASS-Datei mit Wort-Hervorhebung in der Markenfarbe und meiner Schrift aus ~/.markenschnitt/fonts (statische .ttf-Dateien, keine variable Schrift; fontsdir mit ausgeschriebenem Pfad), ins Bild einbrennen. Hook und Abschluss ebenfalls über die ASS-Datei.
- Zoom über Vergrößern und Zuschneiden an den Schnittstellen, Look über Farbfilter, Ton mit afftdn und loudnorm (I=-14).
- Schreib den Ablauf einmal als Skript nach ~/.markenschnitt/schnitt.py und nutze es danach wieder. Das Skript liest alle Stilwerte bei jedem Lauf aus stil.md und enthält selbst keine. Gibt es das Skript nicht, schreib es neu.

## Nachbessern
- Sage ich, was mir nicht gefällt, änderst du es im aktuellen Reel.
- Sage ich „Merk dir das“, trägst du die Änderung als kurze Regel mit Datum unter „Gelernt“ in stil.md ein. Diese Datei hier (SKILL.md) änderst du dafür nie.

## Neue Fassung
- Prüf höchstens einmal am Tag, vor dem ersten Schnitt, ob es eine neue Fassung gibt. Lies dazu mit curl -fsSL https://raw.githubusercontent.com/byfroehlich/markenschnitt/main/skill/VERSION (nicht mit WebFetch). Halte das Datum der Prüfung in ~/.markenschnitt/letzte-pruefung fest, auch wenn sie fehlschlägt (Ordner bei Bedarf anlegen).
- Ist dort eine ganze Zahl, die größer ist als die Version oben, frag mich: „Neue Fassung von Markenschnitt verfügbar. Jetzt aktualisieren?“ Nur bei Ja: Lade https://raw.githubusercontent.com/byfroehlich/markenschnitt/main/skill/SKILL.md mit curl -fsSL in eine temporäre Datei. Übernimm sie nur, wenn Zeile 1 „---“ ist, „name: markenschnitt“ darin steht und die Zeile „Version: <Zahl>“ genau zur gelesenen Zahl passt. Dann ersetze diese Datei damit und lies sie neu. Sonst verwirf sie und sag es mir in einem Satz.
- Ist die Adresse nicht erreichbar oder die Antwort keine ganze Zahl, schneide mit dieser Fassung weiter und sag nichts dazu.
- stil.md fasst eine neue Fassung nie an. Bekomme ich einen neuen Einrichtungstext, ersetzt er diese Datei, in stil.md nur den Grundstil, „Gelernt“ bleibt.

## Datenschutz
Meine Videos bleiben auf meinem Rechner. Lade sie außer an Edits nirgendwo hoch, nutze keine anderen Online-Dienste dafür.
