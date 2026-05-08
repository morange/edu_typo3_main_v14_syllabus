# Change Log

Änderungsprotokoll für `edu_typo3_main_v14_syllabus` (TYPO3 v14 Syllabus-Installation
des TYPO3 Education-Teams). Neueste Einträge stehen oben.

## 2026-05-08

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