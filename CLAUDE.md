# architecture-rules

Les règles de modélisation **ModelIA** : définitions, conventions, implémentation et validation des référentiels de profils et des modèles d'architecture, destinées à être publiées en open source.

> Ce fichier décrit le dépôt `architecture-rules` lui-même. Dans un dépôt qui inclut ces règles comme sous-module (sous `.claude/rules/modelia/`), les consignes ci-dessous ne s'appliquent pas : les règles y sont en lecture seule.

## Organisation

| Chemin | Contenu |
|---|---|
| `Definitions.md` | Les notions : modèle d'architecture, typologie des components (composant applicatif, contexte applicatif, composant système), diagramme, note, stéréotype, profil, méta-information, nom qualifié |
| `Conventions.md` | Les variables `<...>`, les espaces de noms et les valeurs de `<MInfoType>` |
| `architecture/ReferentialArchitectureProfiles.md` | L'implémentation d'un référentiel de profils d'architecture |
| `architecture/ReferentialArchitectureModels.md` | L'implémentation d'un modèle d'architecture |
| `architecture/ArchitectureDiagrams.md` | L'implémentation d'un diagramme de modèle d'architecture |
| `architecture/ValidationArchitecture.md` | Les règles de validation, identifiées par `G-`, `P-`, `M-` et `X-` |
| `samples/` | Un exemple complet : un référentiel de profils (`architectureProfile/`) et un modèle (`architectureModel/`) |

## Utilisation

Ce dépôt est inclus comme sous-module `.claude/rules/modelia/` par :

- [standard-architecture-profil](https://github.com/Mathouic/standard-architecture-profil) ;
- `trainingorchestra-architecture`.

Après un commit poussé ici, on met à jour chaque dépôt utilisateur avec `git submodule update --remote .claude/rules/modelia`.

## Consignes

- Les documents sont rédigés en français ; les noms des méta-éléments (`model`, `folder`, `component`, `dependency`, `diagram`, `note`) et les clefs YAML restent en anglais.
- Une notion est définie dans `Definitions.md`, son implémentation dans `architecture/`. Un document renvoie aux autres par des liens relatifs avec ancre (ex. `[ValidationArchitecture.md](ValidationArchitecture.md#44-dependency)`) : renommer une section impose de mettre à jour les liens qui y pointent.
- Toute évolution d'un template (`ReferentialArchitecture*.md`, `ArchitectureDiagrams.md`) s'accompagne des règles de validation correspondantes dans `ValidationArchitecture.md`, et inversement.
- Les identifiants de règles (`G-01`, `P-20`, `M-35`, …) sont stables : une règle supprimée libère son identifiant sans renuméroter les autres, et une nouvelle règle prend un identifiant libre dans la plage de sa section.
- Une nouvelle clef fixée par un template est ajoutée à la liste de `G-03`.
- Les fichiers de `samples/` respectent toutes les règles de `ValidationArchitecture.md`, et les exemples cités dans les documents correspondent à ces fichiers.
- Rien de spécifique à Training Orchestra n'est introduit dans les règles ; `org.trainingorchestra` n'apparaît que comme exemple d'espace de noms.
