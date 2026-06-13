# Was kann man in der POM updaten — und wie?

Überblick über die voneinander unabhängigen Dinge im POM, die jeweils ein
eigenes Goal haben (oder nur von Hand gehen). Werkzeug: `versions-maven-plugin`.

## Übersicht

| Was im POM | Anzeigen | Automatisch schreiben |
|---|---|---|
| **Dependency-Versionen** (`<dependencies>`, fest verdrahtet) | `display-dependency-updates` | `use-latest-releases` / `use-next-releases` |
| **Versions-Properties** (`<properties>`) | `display-property-updates` | `update-properties` (alle) / `update-property` (einzeln) |
| **Plugin-Versionen** (`<build><plugins>` + `pluginManagement`) | `display-plugin-updates` | ✗ — **nur von Hand** |
| **Parent-POM-Version** (`<parent>`) | (im plugin-updates-Lauf mit dabei) | `update-parent` |
| **Eigene Projekt-Version** (das `<version>` des Artefakts selbst) | — | `set -DnewVersion=…` |

## Einordnung

### Fremde Versionen (Libraries + Build-Werkzeuge)
Die ersten drei Zeilen = „Sachen, gegen die du baust".
- Dependencies + Properties: haben Auto-Schreiben.
- Plugins: **kein** Auto-Schreiben (bewusst — ein Plugin-Update kann das
  Build-Verhalten ändern: anderer Output, neue Defaults).

### Eigene Versionen
- `update-parent` → hebt die Version an, mit der ein Modul auf sein Eltern-POM
  zeigt (`<parent><version>…`).
- `set` → ändert die eigene Projekt-Version (z.B. `X.Y.local` → `X.Z.local`).
  Im Multi-Modul-Reactor setzt `set` die Version in ALLEN Modulen gleichzeitig
  konsistent.

### Multi-Modul-Spezial
- `versions:update-child-modules` → synchronisiert die Parent-Referenzen in den
  Modulen, falls sie auseinanderlaufen. Bei sauberem Reactor selten nötig.

## Was das versions-plugin NICHT anfasst
Struktur/Konfiguration, keine Versionen → immer manuell:
- die `<modules>`-Liste
- `<profiles>`
- hartcodierte Build-Pfade (besser ganz raus → relativ zu `${project.basedir}`)
- `finalName`

## Merksatz
- **Fremde Versionen** (Dependencies, Properties, Plugins)
  → `display-` / `use-` / `update-`-Goals
- **Eigene Versionen** (Projekt-Version, Parent)
  → `set` / `update-parent`
- **Struktur** → Handarbeit

## Befehle zum Anzeigen (ändern nichts) — vollständiger Überblick
```
mvn versions:display-dependency-updates    # Libraries (fest verdrahtet)
mvn versions:display-property-updates       # Libraries (über Properties)
mvn versions:display-plugin-updates         # Build-Plugins
```

## Tipp: nur stabile Versionen (RC/Milestone/Beta rausfiltern)
Im Plugin eine `<configuration><ruleSet>` mit `ignoreVersions` (Regex) hinterlegen
— gilt dann automatisch für alle Goals. Funktioniert inline ab Plugin-Version 2.15+.
