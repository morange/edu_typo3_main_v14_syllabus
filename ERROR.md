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

## [BEHOBEN] Trusted Hosts Pattern blockiert Zugriff auf `edu-syllabus-v14.ddev.site`

- **Datum:**     2026-05-07
- **Bereich:**   Install / Backend (TYPO3 Core, `SYS`-Konfiguration)
- **TYPO3:**     14.3.x
- **Umgebung:**  DDEV lokal (`name: edu-syllabus-v14`)
- **URL/Pfad:**  `https://edu-syllabus-v14.ddev.site/`
- **Schritte:**  Erstinstallation per Install-Wizard / erster Aufruf von Frontend
                 oder Backend.
- **Erwartet:**  Seite wird ausgeliefert.
- **Tatsächlich:** TYPO3 antwortet mit:

  > The current host header value does not match the configured trusted hosts pattern!
  > Check the pattern defined in `$GLOBALS['TYPO3_CONF_VARS']['SYS']['trustedHostsPattern']`
  > and adapt it, if you want to allow the current host header `edu-syllabus-v14.ddev.site`
  > for your installation

- **Ursache:**   TYPO3 prüft eingehende `Host`-Header gegen ein konfigurierbares
                 Regex-Pattern (`SYS/trustedHostsPattern`). Standardmäßig matcht
                 dort nur die im Setup hinterlegte Hauptdomain — der DDEV-Host
                 `edu-syllabus-v14.ddev.site` ist (noch) nicht enthalten.
                 Hintergrund: Schutz vor *Host Header Injection*.

### Lösung

Konfigurations-Override in `config/system/additional.php` (wird vom Setup nicht
überschrieben, ideal für umgebungsspezifische Werte). Datei anlegen, falls
nicht vorhanden:

```php
<?php
// config/system/additional.php

// DDEV-Hostname und ggf. weitere Schulungs-/Staging-Hosts erlauben
$GLOBALS['TYPO3_CONF_VARS']['SYS']['trustedHostsPattern']
    = '.*\.ddev\.site|edu-syllabus-v14\.ddev\.site';
```

Alternative Varianten:

| Wert                             | Wirkung                                      | Empfehlung                |
|----------------------------------|----------------------------------------------|---------------------------|
| `SERVER_NAME` (Default)          | Matcht nur den Apache-`ServerName`           | Default, passt oft nicht  |
| `'.*\.ddev\.site'`               | Erlaubt jeden DDEV-Host                      | Für reine Dev-Umgebung OK |
| `'edu-syllabus-v14\.ddev\.site'` | Genau dieser Host                            | Strikt & explizit         |
| `.*`                             | Akzeptiert jeden Host (Sicherheitsrisiko!)   | **Nicht** in Produktion   |

Für Produktion bzw. Staging muss das Pattern jeweils auf die echten Domains
eingeengt werden — `.*` ist nur als Notnagel zulässig und sollte nie in einen
produktiven Branch gelangen.

**Hinweis:** Statt `additional.php` kann der Wert auch im Install-Tool unter
*Settings → Configure Installation-Wide Options → SYS → trustedHostsPattern*
gesetzt werden; dann landet er in `config/system/settings.php`. Für
umgebungsabhängige Werte ist `additional.php` aber sauberer, weil sie pro
Umgebung (Dev/Staging/Prod) abweichen kann.

- **Workaround:** —
- **Notiz:**     Doku:
                 <https://docs.typo3.org/m/typo3/reference-coreapi/main/en-us/Configuration/Typo3ConfVars/SYS.html#trustedHostsPattern>

---

## Behoben

<!-- Erledigte Einträge hierhin verschieben, Status auf BEHOBEN setzen. -->

_(noch keine Einträge)_