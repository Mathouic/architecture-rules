# Référentiel d'architecture — définitions

L'implémentation de ces notions est décrite dans [architecture/ReferentialArchitectureModels.md](architecture/ReferentialArchitectureModels.md) (modèle), [architecture/ArchitectureDiagrams.md](architecture/ArchitectureDiagrams.md) (diagrammes) et [architecture/ReferentialArchitectureProfiles.md](architecture/ReferentialArchitectureProfiles.md) (profils).

## Modèle d'architecture

Un **modèle d'architecture** est une représentation de l'architecture d'un système.

Il est exprimé à l'aide de 5 méta-éléments :

| Méta-élément | Signification |
|---|---|
| **model** | La racine d'un modèle d'architecture |
| **folder** | Un regroupement d'éléments du modèle |
| **component** | Un élément d'architecture |
| **dependency** | Une relation d'un component vers un autre component, ou d'un folder vers un autre folder |
| **diagram** | Une vue du modèle d'architecture (cf. [Diagramme de modèle d'architecture](#diagramme-de-modèle-darchitecture)) |

Ses éléments, à l'exception de `model` et `diagram`, peuvent être typés par des stéréotypes, qui leur ajoutent des méta-informations.

> Un component peut être composé d'autres components, appelés components internes.

## Diagramme de modèle d'architecture

Un **diagramme de modèle d'architecture** est une vue partielle ou totale d'un modèle d'architecture sous la forme d'un dessin (en Mermaid par exemple).

- Un diagramme déclare les objets du modèle qu'il montre (folders, components, dependencies) ; aucun autre objet n'apparaît dans son dessin.
- Un diagramme ne peut pas être étendu par un stéréotype.

## Stéréotype

Un **stéréotype** est une extension d'un méta-élément `folder`, `component` ou `dependency`.

- Les méta-éléments `model` et `diagram` ne peuvent pas être étendus.
- Un stéréotype déclare les méta-informations que porte un élément stéréotypé.

## Profil

Un **profil** est un ensemble cohérent de stéréotypes pour un domaine donné.

## Méta-information d'un profil

Une **méta-information** est un attribut typé, déclaré par un stéréotype.

- Sa valeur est portée par l'élément du modèle qui porte ce stéréotype.
- Types possibles : chaîne de caractères, nombre, date, temps, ainsi que les listes de chacun de ces types.

## Nom qualifié

Le **nom qualifié** d'un élément est le nom qui l'identifie de façon unique dans son modèle d'architecture ou dans son référentiel de profils.

- Tout élément d'un modèle (model, folder, component, dependency, diagram) et d'un référentiel de profils (profil, stéréotype, méta-information, icône) a un nom qualifié.
- Faire référence à un élément, c'est le désigner par son nom qualifié.
- Les règles de construction des noms qualifiés sont décrites dans [architecture/ReferentialArchitectureModels.md](architecture/ReferentialArchitectureModels.md#7-nom-qualifié) (modèle) et [architecture/ReferentialArchitectureProfiles.md](architecture/ReferentialArchitectureProfiles.md#6-nom-qualifié) (profils).
