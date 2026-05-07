# edu_typo3_main_v14_syllabus

Frische TYPO3 v14 Installation des **TYPO3 Education-Teams** zur Vorbereitung des
Syllabus (Schulungs-Curriculum / offizielle TYPO3 Education-Inhalte).

Dieses Repository dient als Arbeitsumgebung, in der Schulungsbeispiele,
Referenzkonfigurationen und Inhalte für die TYPO3-Schulungen aufgebaut, geprüft
und dokumentiert werden.

## Projekt-Eckdaten

| Schlüssel        | Wert                                                  |
|------------------|-------------------------------------------------------|
| TYPO3-Version    | 14.3 (Meta-Paket `typo3/minimal:^14`)                 |
| Theme            | `typo3/theme-camino` (^14.3)                          |
| Projekt-Typ      | Composer-basiertes TYPO3                              |
| Lokale Umgebung  | DDEV (`name: edu-syllabus-v14`)                       |
| Webserver        | apache-fpm                                            |
| PHP              | 8.5                                                   |
| Datenbank        | MySQL 8.0                                             |
| Composer         | 2                                                     |
| Zeitzone         | Europe/Berlin                                         |
| Docroot          | `public/`                                             |

`.ddev/config.yaml` setzt zusätzlich die Umgebungsvariable
`TYPO3_AUTOLOGIN_USERNAME=Development`, sodass der Backend-Login in der lokalen
Schulungsumgebung beschleunigt wird.

## Projektstruktur

```
.
├── .ddev/            # DDEV-Konfiguration (Projekt edu-syllabus-v14)
├── packages/         # Lokale Site-/Schulungs-Extensions (aktuell leer)
├── public/           # Webroot (Docroot)
│   └── fileadmin/    # TYPO3 Fileadmin
├── vendor/           # Composer-Abhängigkeiten (nicht versioniert)
├── composer.json     # TYPO3 v14.3 Komponenten + minimal-Meta-Paket
├── README.md         # diese Datei
├── ERROR.md          # Sammlung bekannter Fehler / Auffälligkeiten
└── change_log.md     # Änderungsprotokoll (deutsch, pro Commit)
```

## Lokale Einrichtung (DDEV)

Voraussetzungen: [DDEV](https://ddev.com/) und Docker.

```bash
# Container starten
ddev start

# Abhängigkeiten installieren
ddev composer install

# TYPO3 Setup-Assistent öffnen (oder direkt das TYPO3 Backend)
ddev launch
```

Standard-URL nach `ddev start`:
`https://edu-syllabus-v14.ddev.site/`

Backend (nach Erstinstallation):
`https://edu-syllabus-v14.ddev.site/typo3/`

### Erstinstallation

Beim ersten Aufruf liegt im Webroot die Datei `public/FIRST_INSTALL`. Diese
triggert den TYPO3 Install-Wizard. Sie ist über `.gitignore` ausgeschlossen
und wird beim Setup automatisch entfernt.

## Verwendete TYPO3-Komponenten

Aus `composer.json` werden u. a. folgende Systemextensions installiert:

- Backend / Frontend / Install / Core / Extbase / Fluid
- Adminpanel, Belog, Beuser, Dashboard, Extensionmanager
- Felogin, Filelist, Filemetadata, Form
- Fluid Styled Content, Impexp, Indexed Search, Info
- Linkvalidator, Lowlevel, Opendocs, Reactions, Recycler
- Redirects, Reports, RTE CKEditor, Scheduler, SEO
- Sys-Note, TSTemplate, Viewpage, Webhooks, Workspaces
- Theme Camino

## Arbeitsweise / Konventionen

- **Schulungsbeispiele** und projektspezifische Erweiterungen kommen unter
  `packages/` und werden via `composer.json` (Pfad-Repositorien) eingebunden.
- **Inhalte / Seitenbäume** für den Syllabus werden als Datenbank-Snapshot
  oder über `EXT:impexp` reproduzierbar gehalten.
- **Fehler / Auffälligkeiten** werden in [`ERROR.md`](./ERROR.md) gesammelt.
- **Änderungen** werden in `change_log.md` protokolliert (deutsch, je Commit
  ein Eintrag).

## Ziel

Eine reproduzierbare, dokumentierte TYPO3-v14-Referenzinstallation, die als
Grundlage für die offiziellen TYPO3 Education-Schulungen dient — sauber,
nachvollziehbar und mit klar dokumentierten Fehlerstellen.