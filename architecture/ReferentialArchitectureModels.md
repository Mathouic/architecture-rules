# Implémentation d'un modèle d'architecture

## 0. Conventions

Les conventions (variables `<...>`, types de méta-informations) sont décrites dans [Conventions.md](../Conventions.md).

## 1. Objet

L'objet de ce document est l'implémentation d'un modèle d'architecture.

## 2. Les 5 méta-éléments

| Méta-élément | Implémentation | Porteur des méta-informations |
|---|---|---|
| **model** | Le répertoire racine du modèle | Un fichier `metainfo.yaml` obligatoire dans ce répertoire |
| **folder** | Un sous-répertoire du modèle | Un fichier `metainfo.yaml` optionnel, présent uniquement si le folder est stéréotypé (un folder n'est pas obligatoirement stéréotypé) |
| **component** | Un fichier `<ComponentName>.yaml` | Le fichier lui-même |
| **dependency** | Un item de la clef `dependencies` du fichier component qui porte la dépendance | L'item lui-même |
| **diagram** | Un fichier `<DiagramName>.architecturediagram.yaml` dans un folder (cf. [ArchitectureDiagrams.md](ArchitectureDiagrams.md)) | Aucun : un diagram ne peut pas être stéréotypé |

Exemple de folder : `architectureModel/org/trainingorchestra/architecture/applicative/4_13/` et son fichier `metainfo.yaml`.

## 3. Racine du modèle

Template du fichier `metainfo.yaml` à la racine d'un modèle d'architecture :

```yaml
type: architecture.models
profiles:
  - profilePath: <ProfilePath>
    profileName: <ProfileName>
  - profilePath: <ProfilePath>
    profileName: <ProfileName>
```

- `profiles` liste les profils dont le modèle utilise les stéréotypes. Pour chaque profil :
  - `profilePath` est le chemin du référentiel de profils d'architecture qui contient le profil. Un chemin relatif est résolu depuis la racine du modèle ;
  - `profileName` est le nom du profil dans ce référentiel. Ce nom porte la version du profil utilisée par le modèle.

Exemple :

```yaml
type: architecture.models
profiles:
  - profilePath: ../architectureProfile
    profileName: org.mathouic.modelia.architecture.profile.applicative.V01_00_000
```

> Les méta-éléments `model` et `diagram` ne peuvent pas être étendus par un stéréotype.

## 4. Folder

Un folder non stéréotypé n'a pas de fichier `metainfo.yaml`.

Template du fichier `metainfo.yaml` d'un folder stéréotypé :

```yaml
type: architecture.folder
stereotypes:
  - name: <ProfileName>.<TypeName>
    <MInfoName>: <MInfoValue>
    <MInfoName>: <MInfoValue>
  - name: <ProfileName>.<TypeName>
    <MInfoName>: <MInfoValue>
    <MInfoName>: <MInfoValue>
```

## 5. Component

Le nom du component est le nom de son fichier `<ComponentName>.yaml`, sans l'extension.

Les fichiers `metainfo.yaml` et `<DiagramName>.architecturediagram.yaml` (diagrammes, cf. [ArchitectureDiagrams.md](ArchitectureDiagrams.md)) ne sont pas des components.

Template du fichier `<ComponentName>.yaml` :

```yaml
type: architecture.component
stereotypes:
  - name: <ProfileName>.<TypeName>
    <MInfoName>: <MInfoValue>
    <MInfoName>: <MInfoValue>
  - name: <ProfileName>.<TypeName>
    <MInfoName>: <MInfoValue>
    <MInfoName>: <MInfoValue>
dependencies:
  - name: <DependencyName>
    target: <TreePath>.<ComponentName>
    stereotypes:
      - name: <ProfileName>.<TypeName>
        <MInfoName>: <MInfoValue>
        <MInfoName>: <MInfoValue>
      - name: <ProfileName>.<TypeName>
        <MInfoName>: <MInfoValue>
        <MInfoName>: <MInfoValue>
innerComponents:
  - <ComponentName>
  - <ComponentName>
```

- `stereotypes` liste les stéréotypes appliqués au component. Pour chaque stéréotype :
  - `name` désigne le stéréotype par `<ProfileName>.<TypeName>` ;
  - les autres clefs portent les valeurs des méta-informations déclarées par le stéréotype.
- `dependencies` liste les dependencies portées par le component (cf. [§ 6. Dependency](#6-dependency)).
- `innerComponents` liste les noms des components internes (cf. [§ 5.1](#51-components-internes)).

### 5.1. Components internes

- Les components internes d'un component `<ComponentName>` sont stockés dans un sous-répertoire `<ComponentName>/`, placé à côté du fichier `<ComponentName>.yaml`.
- Chaque component interne est un fichier `<ComponentName>.yaml` de ce sous-répertoire, conforme au template du [§ 5](#5-component). Un component interne peut lui-même avoir des components internes.
- Ce sous-répertoire n'est pas un folder : il ne contient pas de fichier `metainfo.yaml` et n'apparaît pas dans le `<TreePath>`.

Exemple :

```
4_13/
├── metainfo.yaml
├── Concerto.yaml        # innerComponents: [API]
└── Concerto/
    └── API.yaml
```

### 5.2. Référence à un component

Une référence à un component est son nom qualifié (cf. [§ 7](#7-nom-qualifié)).

- Pour faire référence à un component dans un fichier yaml : `<TreePath>.<ComponentName>`.
  Exemple : `org.trainingorchestra.architecture.applicative.4_13.Concerto`.
- Pour faire référence à un component interne dans un fichier yaml : `<TreePath>.<ComponentName>.<ComponentName>`.
  Exemple : `org.trainingorchestra.architecture.applicative.4_13.Concerto.API`.

## 6. Dependency

Une dependency est un item de la clef `dependencies` du component qui la porte (cf. template du [§ 5](#5-component)) :

- `name` est le nom de la dependency ;
- `target` désigne le component cible par son nom qualifié, selon les règles du [§ 5.2](#52-référence-à-un-component) ;
- `stereotypes` liste les stéréotypes appliqués à la dependency, sur le même modèle que ceux du component.

## 7. Nom qualifié

Cf. la notion de nom qualifié dans les [définitions](../Definitions.md#nom-qualifié).

| Élément | Nom qualifié | Exemple |
|---|---|---|
| model | Le nom du répertoire racine du modèle | `architectureModel` |
| folder | `<TreePath>` | `org.trainingorchestra.architecture.applicative.4_13` |
| component | `<TreePath>.<ComponentName>` | `org.trainingorchestra.architecture.applicative.4_13.Concerto` |
| component interne | Le nom qualifié de son component suivi de `.<ComponentName>` | `org.trainingorchestra.architecture.applicative.4_13.Concerto.API` |
| dependency | Le nom qualifié du component qui la porte suivi de `.<DependencyName>` | `org.trainingorchestra.architecture.applicative.4_13.Concerto.database` |
| diagram | `<TreePath>.<DiagramName>` | `org.trainingorchestra.architecture.applicative.4_13.Overview` |

Un component interne et une dependency d'un même component ne peuvent donc pas porter le même nom (cf. règle `M-35` de [ValidationArchitecture.md](ValidationArchitecture.md#44-dependency)).

Les noms qualifiés des profils, stéréotypes, méta-informations et icônes sont décrits dans [ReferentialArchitectureProfiles.md](ReferentialArchitectureProfiles.md#6-nom-qualifié).
