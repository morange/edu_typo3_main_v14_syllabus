# ERROR.md — Bekannte Fehler & Auffälligkeiten

Diese Datei dokumentiert fehlerhafte, unklare oder noch zu prüfende Stellen
in der TYPO3-v14-Syllabus-Installation. Sie dient dem TYPO3 Education-Team
als laufende Sammelstelle, um Probleme nicht zu verlieren.

## Wie wird hier eingetragen?

Jeder Eintrag bekommt einen eigenen Abschnitt nach folgendem Muster:

```
## [STATUS] Kurzer Titel des Problems

- **Datum:**     YYYY-MM-DD
- **Bereich:**   z. B. Backend / Frontend / Install / Extension XYZ / DDEV
- **TYPO3:**     14.3.x
- **Umgebung:**  z. B. DDEV lokal, Staging
- **URL/Pfad:**  Wo tritt es auf?
- **Schritte:**  Wie reproduzierbar?
- **Erwartet:**  Was sollte passieren?
- **Tatsächlich:** Was passiert wirklich?
- **Logs:**      Auszüge aus var/log/, Browser-Konsole, ddev logs ...
- **Workaround:** falls vorhanden
- **Notiz:**     freie Anmerkungen, Verweise (Forge-Issue, GitHub-PR …)
```

### Status-Kennzeichnungen

| Status        | Bedeutung                                                       |
|---------------|-----------------------------------------------------------------|
| `OFFEN`       | Problem besteht, noch nicht analysiert                          |
| `ANALYSE`     | wird gerade untersucht                                          |
| `BLOCKIERT`   | wartet auf externe Klärung (Core-Bug, Drittextension, Infra)    |
| `WORKAROUND`  | umgangen, eigentliche Ursache bleibt offen                      |
| `BEHOBEN`     | gefixt — Eintrag bleibt zur Doku stehen                         |
| `WONTFIX`     | bewusst nicht behoben (mit Begründung)                          |

Behobene Einträge werden **nicht gelöscht**, sondern auf `BEHOBEN` umgesetzt
und mit Datum & Commit-/PR-Referenz ergänzt — so bleibt der Verlauf sichtbar.

---

## Offene Punkte

<!-- Hier neue Einträge oben einfügen. -->

_(noch keine Einträge)_

---

## Behoben

<!-- Erledigte Einträge hierhin verschieben, Status auf BEHOBEN setzen. -->

_(noch keine Einträge)_