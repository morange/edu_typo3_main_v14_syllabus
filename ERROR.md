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

| Status       | Bedeutung                                                       |
|--------------|-----------------------------------------------------------------|
| `OPEN`       | Problem besteht, noch nicht analysiert                          |
| `ANALYSE`    | wird gerade untersucht                                          |
| `BLOCKED`    | wartet auf externe Klärung (Core-Bug, Drittextension, Infra)    |
| `WORKAROUND` | umgangen, eigentliche Ursache bleibt offen                      |
| `FIXED`      | gefixt — Eintrag bleibt zur Doku stehen                         |
| `WONTFIX`    | bewusst nicht behoben (mit Begründung)                          |

Behobene Einträge werden **nicht gelöscht**, sondern auf `BEHOBEN` umgesetzt
und mit Datum & Commit-/PR-Referenz ergänzt — so bleibt der Verlauf sichtbar.

---

## Offene Punkte

<!-- Hier neue Einträge oben einfügen. -->

## [OFFEN] `public/typo3temp/var/.htaccess` aus Install-Template nicht angelegt

- **Datum:**     2026-05-07
- **Bereich:**   Install / Folder-Structure-Templates (`EXT:install`)
- **TYPO3:**     14.3.x
- **Umgebung:**  DDEV lokal (`name: edu-syllabus-v14`)
- **URL/Pfad:**  `public/typo3temp/var/.htaccess` (Soll-Pfad)
- **Schritte:**
  1. Frische Installation per Install-Wizard.
  2. Prüfen, welche `.htaccess`-Dateien unterhalb von `public/` platziert wurden:
     ```
     find public -name ".htaccess"
     ```
- **Erwartet:**  Alle in
                 `vendor/typo3/cms-install/Resources/Private/FolderStructureTemplateFiles/`
                 hinterlegten Templates sind in ihren Zielverzeichnissen
                 vorhanden — insbesondere
                 `typo3temp-var-htaccess` → `public/typo3temp/var/.htaccess`.
- **Tatsächlich:** `public/typo3temp/var/` existiert nach dem Setup noch nicht;
                 entsprechend fehlt auch die Schutz-`.htaccess` darin. Andere
                 Folder-Structure-Targets sind dagegen platziert
                 (`public/.htaccess`, `public/fileadmin/.htaccess`,
                 `public/fileadmin/_temp_/.htaccess`,
                 `public/fileadmin/user_upload/_temp_/importexport/.htaccess`).

### Hintergrund: `.htaccess` aus dem Install-Ordner

TYPO3 verwaltet Schutz-`.htaccess`-Dateien zentral als *Folder Structure
Templates*:

```
vendor/typo3/cms-install/Resources/Private/FolderStructureTemplateFiles/
  ├── root-htaccess                                 → public/.htaccess
  ├── root-web-config                               → public/web.config (IIS)
  ├── resources-root-htaccess                       → Resources Root
  ├── fileadmin-temp-htaccess                       → public/fileadmin/_temp_/.htaccess
  ├── fileadmin-temp-index.html                     → public/fileadmin/_temp_/index.html
  ├── fileadmin-user_upload-temp-importexport-htaccess
  │                                                 → public/fileadmin/user_upload/_temp_/importexport/.htaccess
  └── typo3temp-var-htaccess                        → public/typo3temp/var/.htaccess   ⟵ FEHLT
```

Die Platzierung erfolgt durch den *Folder Structure*-Check des Install-Tools
bzw. den CLI-Befehl
`vendor/bin/typo3 install:fixfolderstructure` (TYPO3 v14).

### Lösung / Workaround

1. Im Install-Tool unter
   *Maintenance → Folder Structure / Directory Structure*
   den „Fix“-Button ausführen — TYPO3 legt fehlende Verzeichnisse und
   Template-Dateien an.
2. Alternativ per CLI:
   ```bash
   ddev exec vendor/bin/typo3 install:fixfolderstructure
   ```
3. Manuell — falls beides nicht greift:
   ```bash
   mkdir -p public/typo3temp/var
   cp vendor/typo3/cms-install/Resources/Private/FolderStructureTemplateFiles/typo3temp-var-htaccess \
      public/typo3temp/var/.htaccess
   ```

- **Notiz:**     `public/typo3temp/var/` enthält Caches, Logs und kompilierte
                 Artefakte; die `.htaccess` sperrt direkten Web-Zugriff darauf.
                 Solange das Verzeichnis ungeschützt ist, sollten dort keine
                 sensiblen Daten liegen — der Fix ist also nicht rein
                 kosmetisch.

---

## [BEHOBEN] `public/.htaccess` versehentlich im falschen Commit gelandet

- **Datum:**     2026-05-07
- **Bereich:**   Repo-Hygiene / Initial-Setup
- **TYPO3:**     14.3.x
- **Umgebung:**  DDEV lokal
- **URL/Pfad:**  `public/.htaccess`
- **Schritte:**
  1. Install-Wizard ausführen → TYPO3 platziert `public/.htaccess`.
  2. Datei wurde manuell mit `git add` gestaged.
  3. Anschließender `git commit` mit eigentlich fokussiertem Inhalt
     (`[TASK] Document trustedHostsPattern issue in ERROR.md`)
     hat die `.htaccess` ungewollt mitgenommen
     (Commit `1137610`).
- **Erwartet:**  `public/.htaccess` als eigener Commit, themenrein.
- **Tatsächlich:** `.htaccess` (382 Zeilen) im selben Commit wie eine
                 reine Doku-Änderung — Commit-Granularität schlechter,
                 spätere Bisects / Reviews schwerer lesbar.

### Hintergrund / Quelle der Datei

`public/.htaccess` ist eine 1:1-Kopie des Install-Templates

```
vendor/typo3/cms-install/Resources/Private/FolderStructureTemplateFiles/root-htaccess
```

und enthält u. a.:

- Default-Regeln zur URL-Rewriting-Aktivierung (`RewriteEngine On`).
- Schutz von Konfigurations- und sensiblen Dateien
  (`composer.json`, `composer.lock`, `*.bak`, `.git*` …).
- Caching- und Compression-Header (`mod_deflate`, `mod_expires`).
- Zugriffsschutz für interne Pfade.

Die Datei muss versioniert sein, weil sie sicherheitsrelevant ist und
ein Deployment-Schritt sie nicht zwingend wieder herstellt.

### Lehre / Vorgehen ab jetzt

- **Vor jedem Commit `git status` prüfen** und gezielt mit
  `git add <datei>` einzelne Pfade stagen, statt `git add .` /
  `git add -A` zu verwenden.
- TYPO3-Setup-Artefakte (`public/.htaccess`, `public/index.php`,
  `config/system/*.php`, `public/fileadmin/.htaccess` …) gehören in
  einen eigenen, klar benannten Commit
  (z. B. `[TASK] Add TYPO3 install artifacts`), getrennt von Doku-
  oder Code-Änderungen.

- **Notiz:**     Da der Commit lokal noch nicht gepusht ist, könnte er
                 per `git reset HEAD~1` aufgesplittet werden. Aktuell
                 belassen — Eintrag dient als Referenz, falls künftig
                 ein vergleichbarer Mix-Commit auftritt.

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