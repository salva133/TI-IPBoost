# TI-IPBoost

Ein Mod für **Terra Invicta**, der zwei fraktionsexklusive Projekte für **Humanity First**
(`DestroyCouncil`) hinzufügt: eine wiederholbare Steigerung der Investment Points auf allen
nationalen Prioritäten und eine einmalige, drastische Verkürzung der Schiffsbauzeit.

| | |
|---|---|
| Titel | IPBoost |
| Autor | Steve |
| Version | 3.0.0 |
| Getestete Spielversion | 1.0.26 |
| Fraktion | Humanity First (`DestroyCouncil`) |

## Inhalt

### Projekte

#### Directed Investment Reform (`IPBoost_Project_DirectedInvestmentReform`)

Wiederholbares Projekt. Jeder Abschluss erhöht dauerhaft die Investment Points, die die
eigenen Control Points in **jede** nationale Priorität lenken. Der Bonus stapelt sich mit
jeder Wiederholung. Nur die eigene Fraktion profitiert.

* `techCategory`: `SocialScience`
* `researchCost`: 50
* `AI_techRole`: `Income`, `AI_projectRole`: `DevelopNation`
* `repeatable`: `true`, `oneTimeGlobally`: `false`
* Effekt: `IPBoost_Effect_AllPriorityBonus`

#### Advanced Shipyard Methods (`IPBoost_Project_AdvancedShipyardMethods`)

Einmaliges Projekt. Nach Abschluss bauen die eigenen Werften dauerhaft schneller.
Nur die eigene Fraktion profitiert.

* `techCategory`: `SpaceScience`
* `researchCost`: 50
* `AI_techRole`: `SpaceDevelopment`, `AI_projectRole`: `Fleet`
* `repeatable`: `false`, `oneTimeGlobally`: `false`
* Effekt: `IPBoost_Effect_ShipConstructionTimeReduction`

Beide Projekte teilen sich dieselben Freischaltbedingungen: `factionPrereq` und
`factionAlways` stehen auf `DestroyCouncil`, `factionAvailableChance`,
`initialUnlockChance` und `maxUnlockChance` auf 100, `deltaUnlockChance` auf 0.
`resourcesGranted` ist leer. Damit sind die Projekte für Humanity First immer
verfügbar und für alle anderen Fraktionen unsichtbar.

### Effekte

#### `IPBoost_Effect_AllPriorityBonus`

* `operation`: `Additive`, `value`: `2.0`
* `effectTarget`: `SourceFaction`
* `effectDuration`: `permanent`, `duration_months`: `-1`, `stackable`: `true`

Wirkt auf 17 Prioritäts-Kontexte:

`EconomyPriority`, `WelfarePriority`, `KnowledgePriority`, `UnityPriority`,
`GovernmentPriority`, `EnvironmentPriority`, `MilitaryPriority`, `OppressionPriority`,
`SpoilsPriority`, `LaunchFacilitiesPriority`, `MissionControlPriority`,
`SpaceflightPriority`, `BuildArmyPriority`, `UpgradeArmyPriority`,
`BuildSpaceDefensesPriority`, `BuildSTOSquadronPriority`, `BuildNuclearWeaponsPriority`

#### `IPBoost_Effect_ShipConstructionTimeReduction`

* `operation`: `Multiplicative`, `value`: `0.01`
* `effectTarget`: `SourceFaction`
* `effectDuration`: `permanent`, `duration_months`: `-1`, `stackable`: `true`
* Kontext: `ShipConstructionTime`

Der Multiplikator 0.01 ist bewusst extrem gewählt – die Bauzeit soll auf einen
Bruchteil des Originals fallen. Wie genau das Spiel den Wert auf den Kontext anwendet,
bestimmt Terra Invicta selbst; angepasst wird er über das Feld `value`.

## Dateien

| Datei | Zweck |
|---|---|
| `ModInfo.json` | Mod-Metadaten und Liste der zu konkatenierenden Templates |
| `TIProjectTemplate.json` | Definition der beiden Projekte |
| `TIEffectTemplate.json` | Definition der beiden Effekte |
| `TIProjectTemplate.en` | Englische Texte: Anzeigename, Summary, Beschreibung je Projekt |
| `TIEffectTemplate.en` | Englische Effektbeschreibungen |
| `.gitattributes` | Automatische LF-Normalisierung für Textdateien |

Alle vier Template-Dateien sind in `ModInfo.json` unter `TemplatesToConcatArrays`
eingetragen. Sie ersetzen die Vanilla-Daten also nicht, sondern werden an die
bestehenden Arrays angehängt – der Mod ist dadurch mit anderen Mods verträglich,
solange diese keine gleichnamigen `dataName`-Einträge verwenden.

In den `.en`-Dateien stehen `{3}` und `{18}` als Platzhalter, die das Spiel beim
Anzeigen der Effektbeschreibung mit dem formatierten Effektwert füllt.

## Installation

1. Ordner `IPBoost` unter dem Terra-Invicta-Mod-Verzeichnis anlegen:
   `Dokumente\My Games\TerraInvicta\Mods\IPBoost`
2. Den Inhalt dieses Repositories (mindestens `ModInfo.json` sowie die vier
   Template-Dateien) dort hineinkopieren.
3. Terra Invicta starten und den Mod im Mod-Menü aktivieren.

Die Projekte erscheinen erst in einem Spiel als **Humanity First**.

## Werte anpassen

Die Stärke steckt ausschließlich im Feld `value` der jeweiligen Effekte in
`TIEffectTemplate.json`:

* Prioritäten-Bonus: `value` von `IPBoost_Effect_AllPriorityBonus`
* Schiffsbauzeit: `value` von `IPBoost_Effect_ShipConstructionTimeReduction`

Forschungskosten lassen sich über `researchCost` in `TIProjectTemplate.json` ändern.

Seit Version 3.0.0 enthalten die `dataName`-Bezeichner **keine Magnituden mehr**
(vorher `…AllPriorityBonus200` und `…ShipConstructionTimeReduction20`). Dadurch
bleiben die Identifier beim Nachbalancieren stabil, und ein bestehender Speicherstand
wird durch geänderte Werte nicht ungültig.

## Balancing-Hinweis

Der Mod ist nicht am Vanilla-Balancing ausgerichtet. Ein wiederholbares Projekt für
50 Forschungspunkte, das +2.0 auf sämtliche Prioritäten gibt und beliebig oft
stapelbar ist, sowie eine Bauzeit von einem Hundertstel sind als deutliche
Erleichterung gedacht, nicht als ausgewogene Erweiterung.

## Versionsverlauf

* **3.0.0** – Magnituden aus den Effekt-`dataName`s entfernt (`IPBoost_Effect_AllPriorityBonus`,
  `IPBoost_Effect_ShipConstructionTimeReduction`) inklusive der zugehörigen Referenzen in
  Projekten und Lokalisierung; Mod-Beschreibung ohne feste Zahlenwerte formuliert.
* **2.2.0** – Zweites Projekt *Advanced Shipyard Methods* mit dem
  Schiffsbauzeit-Effekt ergänzt; Bauzeit-Multiplikator auf `0.01` gesetzt;
  Projekttexte erweitert.
* **Initial** – *Directed Investment Reform* mit dem Prioritäten-Effekt über alle
  17 Kontexte, Mod-Grundgerüst und Lokalisierung.
