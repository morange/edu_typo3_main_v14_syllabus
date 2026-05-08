# Change Log

Änderungsprotokoll für `edu_typo3_main_v14_syllabus` (TYPO3 v14 Syllabus-Installation
des TYPO3 Education-Teams). Neueste Einträge stehen oben.

## 2026-05-08

- `.gitignore` und `public/fileadmin/`: Tracking neu sortiert — Camino-
  Theme-Demobilder (`fileadmin/camino/`) wieder aus Git entfernt
  (Projektentscheidung: nicht im Schulungs-Repo); dafür die noch
  fehlenden Standard-Schutzdateien aus den TYPO3-Folder-Structure-
  Templates aufgenommen (`_temp_/.htaccess`, `_temp_/index.html`,
  `user_upload/index.html`, `user_upload/_temp_/index.html`).
- TYPO3-Install-Artefakte ergänzt: `config/system/settings.php` (Globale
  TYPO3-Konfiguration nach Install-Wizard) und `config/sites/camino/config.yaml`
  (Site-Konfiguration der Camino-Demo-Site).
- `public/fileadmin/`: Folder-Structure-Schutzdateien (`.htaccess`,
  `_temp_/`, `user_upload/_temp_/importexport/.htaccess` über Negation)
  sowie die Demo-Bilder des Camino-Themes (`fileadmin/camino/*.webp`)
  ergänzt.
- `.gitignore`: an die TYPO3-v14-Install-Struktur angepasst — `public/_assets_install/`,
  `public/typo3temp/` und `public/web.config` (IIS irrelevant) ergänzt; gezielte
  Negation für `public/fileadmin/user_upload/_temp_/importexport/.htaccess`,
  damit die Schutzdatei trackbar bleibt.
- `composer.json`: doppelten `config`-Block bereinigt, projektspezifische
  `name`/`description` (`typo3-edu/syllabus-v14`) gesetzt und
  `extra.typo3/cms.web-dir = "public"` ergänzt.
- `composer.lock`: Content-Hash an aktualisierte `composer.json` angeglichen.
- `ERROR.md`: Status-Kennzeichnungen in der Legende auf englische Begriffe
  (`OPEN`, `ANALYSE`, `BLOCKED`, `WORKAROUND`, `FIXED`, `WONTFIX`)
  vereinheitlicht.
- `ERROR.md`: zwei Einträge zur `.htaccess`-Situation ergänzt:
  - `[OFFEN]` Fehlende `public/typo3temp/var/.htaccess` aus dem Install-
    Template `vendor/typo3/cms-install/.../typo3temp-var-htaccess` — inkl.
    Übersicht aller Folder-Structure-Template-Quellen und Soll-Ziele sowie
    Lösung über Install-Tool / `install:fixfolderstructure`.
  - `[BEHOBEN]` `public/.htaccess` versehentlich im Doku-Commit `1137610`
    mitgewandert — Hintergrund zur Quelle der Datei (1:1 aus `root-htaccess`
    Install-Template) und Konvention, TYPO3-Setup-Artefakte zukünftig in
    einen eigenen, themenreinen Commit zu legen.

## 2026-05-07

- `ERROR.md`: Eintrag zum *Trusted Hosts Pattern*-Fehler bei der TYPO3-v14-
  Installation auf `edu-syllabus-v14.ddev.site` ergänzt — inkl. Ursache,
  Lösung über `config/system/additional.php` und Vergleichstabelle der
  Pattern-Varianten.
- Projektdokumentation initialisiert: `README.md` (Projektüberblick, DDEV-Setup,
  Komponentenliste) und `ERROR.md` (Vorlage zur Sammlung fehlerhafter Stellen)
  angelegt.
- `change_log.md` angelegt (Pflege gemäß globaler Konvention: deutscher
  Eintrag pro inhaltlichem Commit).