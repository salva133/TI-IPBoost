# TI-IPBoost

A mod for **Terra Invicta** that adds two faction-private projects for **Humanity First**
(`DestroyCouncil`): a repeatable boost to the Investment Points directed into every national
priority, and a one-time, drastic reduction of ship construction time.

| | |
|---|---|
| Title | IPBoost |
| Author | Steve |
| Version | 3.0.0 |
| Target game version | 1.0.26 |
| Faction | Humanity First (`DestroyCouncil`) |

## Contents

### Projects

#### Directed Investment Reform (`IPBoost_Project_DirectedInvestmentReform`)

A repeatable project. Each completion permanently increases the Investment Points that your
control points direct into **every** national priority, and the bonus stacks with every
repeat. Only your own faction benefits.

* `techCategory`: `SocialScience`
* `researchCost`: 50
* `AI_techRole`: `Income`, `AI_projectRole`: `DevelopNation`
* `repeatable`: `true`, `oneTimeGlobally`: `false`
* Effect: `IPBoost_Effect_AllPriorityBonus`

#### Advanced Shipyard Methods (`IPBoost_Project_AdvancedShipyardMethods`)

A one-time project. On completion your shipyards permanently build ships faster.
Only your own faction benefits.

* `techCategory`: `SpaceScience`
* `researchCost`: 50
* `AI_techRole`: `SpaceDevelopment`, `AI_projectRole`: `Fleet`
* `repeatable`: `false`, `oneTimeGlobally`: `false`
* Effect: `IPBoost_Effect_ShipConstructionTimeReduction`

Both projects share the same unlock conditions: `factionPrereq` and `factionAlways` are set to
`DestroyCouncil`, `factionAvailableChance`, `initialUnlockChance` and `maxUnlockChance` are
100, and `deltaUnlockChance` is 0. `resourcesGranted` is empty. This makes the projects always
available to Humanity First and invisible to every other faction.

### Effects

#### `IPBoost_Effect_AllPriorityBonus`

* `operation`: `Additive`, `value`: `2.0`
* `effectTarget`: `SourceFaction`
* `effectDuration`: `permanent`, `duration_months`: `-1`, `stackable`: `true`

Applies to 17 priority contexts:

`EconomyPriority`, `WelfarePriority`, `KnowledgePriority`, `UnityPriority`,
`GovernmentPriority`, `EnvironmentPriority`, `MilitaryPriority`, `OppressionPriority`,
`SpoilsPriority`, `LaunchFacilitiesPriority`, `MissionControlPriority`,
`SpaceflightPriority`, `BuildArmyPriority`, `UpgradeArmyPriority`,
`BuildSpaceDefensesPriority`, `BuildSTOSquadronPriority`, `BuildNuclearWeaponsPriority`

#### `IPBoost_Effect_ShipConstructionTimeReduction`

* `operation`: `Multiplicative`, `value`: `0.01`
* `effectTarget`: `SourceFaction`
* `effectDuration`: `permanent`, `duration_months`: `-1`, `stackable`: `true`
* Context: `ShipConstructionTime`

The multiplier of 0.01 is deliberately extreme — construction time is meant to drop to a small
fraction of the original. Exactly how the game applies the value to the context is up to Terra
Invicta itself; tuning happens through the `value` field.

## Files

| File | Purpose |
|---|---|
| `ModInfo.json` | Mod metadata and the list of templates to concatenate |
| `TIProjectTemplate.json` | Definition of both projects |
| `TIEffectTemplate.json` | Definition of both effects |
| `TIProjectTemplate.en` | English strings: display name, summary and description per project |
| `TIEffectTemplate.en` | English effect descriptions |
| `.gitattributes` | Automatic LF normalization for text files |
| `.github/ti-validate.json` | Which identifier prefixes the checks hold the mod to |
| `.github/workflows/` | Validation on every push and pull request, release on a version bump |

All four template files are listed in `ModInfo.json` under `TemplatesToConcatArrays`. They do
not replace the vanilla data — they are appended to the existing arrays, which keeps the mod
compatible with other mods as long as those do not use the same `dataName` entries.

The `.en` files contain `{3}` and `{18}` as placeholders that the game fills with the formatted
effect value when it displays the effect description.

## Installation

1. Create an `IPBoost` folder inside the Terra Invicta mods directory:
   `Documents\My Games\TerraInvicta\Mods\IPBoost`
2. Copy the contents of this repository into it (at minimum `ModInfo.json` and the four
   template files).
3. Start Terra Invicta and enable the mod in the mod menu.

The projects only appear in a game played as **Humanity First**.

## Tuning the values

All of the strength lives in the `value` field of the respective effects in
`TIEffectTemplate.json`:

* Priority bonus: `value` of `IPBoost_Effect_AllPriorityBonus`
* Ship construction time: `value` of `IPBoost_Effect_ShipConstructionTimeReduction`

Research costs can be changed via `researchCost` in `TIProjectTemplate.json`.

Since version 3.0.0 the `dataName` identifiers **no longer carry magnitudes** (previously
`…AllPriorityBonus200` and `…ShipConstructionTimeReduction20`). This keeps the identifiers
stable while rebalancing, so changed values do not invalidate an existing save.

## Balance note

This mod is not aligned with vanilla balance. A repeatable project costing 50 research that
grants +2.0 to every priority and stacks indefinitely, together with construction time at one
hundredth, is intended as a significant leg up rather than a balanced expansion.

## Version history

* **3.0.0** – Removed magnitudes from the effect `dataName`s (`IPBoost_Effect_AllPriorityBonus`,
  `IPBoost_Effect_ShipConstructionTimeReduction`) along with the matching references in the
  projects and localization; reworded the mod description without hard-coded numbers.
* **2.2.0** – Added the second project *Advanced Shipyard Methods* with the ship construction
  time effect; set the construction time multiplier to `0.01`; expanded the project strings.
* **Initial** – *Directed Investment Reform* with the priority effect across all 17 contexts,
  the mod scaffolding and localization.

## Continuous integration

Every push and pull request runs the Terra Invicta mod validator from
[`salva133/My-Workflows`](https://github.com/salva133/My-Workflows). It reads the mod the way
the game does and reports what the game would swallow in silence:

* `ModInfo.json` parses, carries the fields the mod menu reads, and lists every template file
  present — a template missing from `TemplatesToConcatArrays` is a file the game never opens.
* Every template holds an array of records, each with its own `dataName`, and no `dataName` is
  defined twice.
* Every `IPBoost_` name a record points at resolves to a record the mod defines, so a typo in
  a project's `effects` list is caught rather than silently dropping the effect.
* Every localization key addresses a record that exists, no key is defined twice, and the
  `{n}` placeholders agree across languages.

A release is cut by bumping `Version` in `ModInfo.json` and pushing to `master`: the checks run
again, and a green run publishes `IPBoost-v<version>.zip` — an `IPBoost` folder ready to drop
into the mods directory — as a GitHub release tagged `v<version>`.
