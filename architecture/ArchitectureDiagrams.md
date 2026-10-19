# Implémentation d'un diagramme de modèle d'architecture

## 0. Conventions

Les conventions (variables `<...>`, types de méta-informations) sont décrites dans [Conventions.md](../Conventions.md).

## 1. Objet

L'objet de ce document est l'implémentation d'un diagramme de modèle d'architecture (cf. les [définitions](../Definitions.md#diagramme-de-modèle-darchitecture)).

## 2. Fichier

Un diagramme se matérialise par un fichier yaml `<DiagramName>.architecturediagram.yaml` placé dans un folder du modèle (cf. [ReferentialArchitectureModels.md](ReferentialArchitectureModels.md#4-folder)).

- Un diagramme n'est placé ni à la racine du modèle, ni dans un répertoire de components internes.
- Un fichier `<DiagramName>.architecturediagram.yaml` n'est pas un component, bien que son extension soit `.yaml`.
- Le méta-élément `diagram` ne peut pas être étendu par un stéréotype.

## 3. Contenu

Le fichier contient la définition du diagramme.

Template du fichier `<DiagramName>.architecturediagram.yaml` :

```yaml
objects:
  - component: <TreePath>.<ComponentName>
    icon: <ProfileName>.<TypeName>_<IconKey>
    visibleMetaInfos:
      - <ProfileName>.<TypeName>.<MInfoName>
      - <ProfileName>.<TypeName>.<MInfoName>
  - folder: <TreePath>
    icon: <ProfileName>.<TypeName>_<IconKey>
    visibleMetaInfos:
      - <ProfileName>.<TypeName>.<MInfoName>
  - dependency: <TreePath>.<ComponentName>.<DependencyName>
    visibleMetaInfos:
      - <ProfileName>.<TypeName>.<MInfoName>
  - dependency: <TreePath>.<DependencyName>
```

- `objects` liste les objets visibles dans le dessin. Chaque item désigne un objet par une clef qui donne son genre :
  - `component` : un component (ou un component interne), désigné par son nom qualifié ;
  - `folder` : un folder, désigné par son nom qualifié ;
  - `dependency` : une dependency de component ou de folder, désignée par son nom qualifié.
- `icon` est optionnelle et réservée aux components et aux folders : c'est le nom qualifié de l'icône d'un stéréotype appliqué à l'objet (cf. [ReferentialArchitectureProfiles.md § 4](ReferentialArchitectureProfiles.md#4-profil)).
- `visibleMetaInfos` est optionnelle : elle liste, par leurs noms qualifiés, les méta-informations des stéréotypes appliqués à l'objet qui sont affichées dans le dessin.

Les noms qualifiés sont décrits dans [ReferentialArchitectureModels.md § 7](ReferentialArchitectureModels.md#7-nom-qualifié) et [ReferentialArchitectureProfiles.md § 6](ReferentialArchitectureProfiles.md#6-nom-qualifié).

## 4. Dessin

Le dessin (en Mermaid par exemple) est produit à partir de la définition du diagramme.

- Aucun objet absent de `objects` ne doit apparaître dans le dessin.
- Une dependency n'est donc dessinable que si l'élément qui la porte (component ou folder) et son élément cible sont tous deux dans `objects`.

## 5. Exemple

Le fichier `applicative/UserToConcerto.architecturediagram.yaml` de l'[exemple](../samples/architectureModel/applicative/UserToConcerto.architecturediagram.yaml) :

```yaml
objects:
  - component: applicative.User
    icon: org.mathouic.modelia.architecture.profile.applicative.V01_00_000.User_100
  - component: applicative.Concerto
    icon: org.trainingorchestra.architecture.profile.applicative.V01_00_000.Concerto_100
    visibleMetaInfos:
      - org.trainingorchestra.architecture.profile.applicative.V01_00_000.Concerto.version
  - dependency: applicative.User.concerto
```

Le fichier `applicative/ApplicativeToTechnical.architecturediagram.yaml` de l'[exemple](../samples/architectureModel/applicative/ApplicativeToTechnical.architecturediagram.yaml) montre une dependency de folder, `technical`, portée par le `metainfo.yaml` du folder `applicative` :

```yaml
objects:
  - folder: applicative
  - folder: technical
  - dependency: applicative.technical
```
