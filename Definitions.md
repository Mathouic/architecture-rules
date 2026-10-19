# Référentiel d'architecture — définitions

L'implémentation de ces notions est décrite dans [architecture/ReferentialArchitectureModels.md](architecture/ReferentialArchitectureModels.md) (modèle), [architecture/ArchitectureDiagrams.md](architecture/ArchitectureDiagrams.md) (diagrammes) et [architecture/ReferentialArchitectureProfiles.md](architecture/ReferentialArchitectureProfiles.md) (profils).

## Modèle d'architecture

Un **modèle d'architecture** est une représentation de l'architecture d'un système.

Il est exprimé à l'aide de 6 méta-éléments :

| Méta-élément | Signification |
|---|---|
| **model** | La racine d'un modèle d'architecture |
| **folder** | Un regroupement d'éléments du modèle |
| **component** | Un élément d'architecture |
| **dependency** | Une relation d'un component vers un autre component, ou d'un folder vers un autre folder |
| **diagram** | Une vue du modèle d'architecture (cf. [Diagramme de modèle d'architecture](#diagramme-de-modèle-darchitecture)) |
| **note** | Un commentaire attaché à un élément du modèle (cf. [Note](#note)) |

Ses éléments, à l'exception de `model` et `diagram`, peuvent être typés par des stéréotypes, qui leur ajoutent des méta-informations.

> Un component peut être composé d'autres components, appelés components internes.

## Typologie des components

Un component représente le plus souvent l'un des trois éléments suivants, chacun typé par les stéréotypes d'un profil dédié.

### Composant applicatif

Un **composant applicatif** est un élément logiciel qui rend un service applicatif et s'exécute dans un contexte applicatif.

Exemples : une base de données, un backend web, un royaume Keycloak, une file de messages.

### Contexte applicatif

Un **contexte applicatif** est un serveur logiciel sur lequel s'exécutent des composants applicatifs.

Exemples : MS SQL Server, sur lequel s'exécute une base de données ; IIS, sur lequel s'exécute une application web ; Keycloak, qui héberge des royaumes.

### Composant système

Un **composant système** est un élément d'infrastructure sur lequel s'exécutent des contextes applicatifs, ou qui assure leur communication.

Exemples : une machine virtuelle, un conteneur, un réseau, un load balancer.

### Hébergement

- Un composant système héberge des contextes applicatifs, qui hébergent des composants applicatifs.
- Un élément hébergé est un component interne de son hôte (cf. [architecture/ReferentialArchitectureModels.md](architecture/ReferentialArchitectureModels.md#51-components-internes)).

## Diagramme de modèle d'architecture

Un **diagramme de modèle d'architecture** est une vue partielle ou totale d'un modèle d'architecture sous la forme d'un dessin (en Mermaid par exemple).

- Un diagramme déclare les objets du modèle qu'il montre (folders, components, dependencies, notes) ; aucun autre objet n'apparaît dans son dessin.
- Un diagramme ne peut pas être étendu par un stéréotype.

## Note

Une **note** est un commentaire, rédigé en Markdown, attaché à un élément du modèle.

- Une note est toujours attachée à un seul élément : un folder, un component, une dependency ou un diagram.
- Une note peut être étendue par un stéréotype.

## Stéréotype

Un **stéréotype** est une extension d'un méta-élément `folder`, `component`, `dependency` ou `note`.

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

- Tout élément d'un modèle (model, folder, component, dependency, diagram, note) et d'un référentiel de profils (profil, stéréotype, méta-information, icône) a un nom qualifié.
- Faire référence à un élément, c'est le désigner par son nom qualifié.
- Les règles de construction des noms qualifiés sont décrites dans [architecture/ReferentialArchitectureModels.md](architecture/ReferentialArchitectureModels.md#8-nom-qualifié) (modèle) et [architecture/ReferentialArchitectureProfiles.md](architecture/ReferentialArchitectureProfiles.md#6-nom-qualifié) (profils).
