# Implémentation d'un modèle d'architecture

## 0. Conventions

Les conventions (variables `<...>`, types de méta-informations) sont décrites dans [Conventions.md](../Conventions.md).

## 1. Objet

L'objet de ce document est l'implémentation d'un modèle d'architecture.

## 2. Les 6 méta-éléments

| Méta-élément | Implémentation | Porteur des méta-informations |
|---|---|---|
| **model** | Le répertoire racine du modèle | Un fichier `metainfo.yaml` obligatoire dans ce répertoire |
| **folder** | Un sous-répertoire du modèle | Un fichier `metainfo.yaml` optionnel, présent uniquement si le folder est stéréotypé ou porte des dependencies (un folder n'est pas obligatoirement stéréotypé) |
| **component** | Un fichier `<ComponentName>.yaml` | Le fichier lui-même |
| **dependency** | Un item de la clef `dependencies` du fichier component ou du `metainfo.yaml` du folder qui porte la dépendance | L'item lui-même |
| **diagram** | Un fichier `<DiagramName>.architecturediagram.yaml` dans un folder (cf. [ArchitectureDiagrams.md](ArchitectureDiagrams.md)) | Aucun : un diagram ne peut pas être stéréotypé |
| **note** | Un fichier `<NoteName>.note.yaml` et un fichier `<NoteName>.note.md` dans un folder (cf. [§ 7](#7-note)) | Le fichier `<NoteName>.note.yaml` |

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

> Les méta-éléments `model` et `diagram` ne peuvent pas être étendus par un stéréotype ; `folder`, `component`, `dependency` et `note` peuvent l'être.

## 4. Folder

Un folder qui n'est pas stéréotypé et ne porte pas de dependency n'a pas de fichier `metainfo.yaml`.

Template du fichier `metainfo.yaml` d'un folder stéréotypé ou porteur de dependencies :

```yaml
type: architecture.folder
stereotypes:
  - name: <ProfileName>.<TypeName>
    <MInfoName>: <MInfoValue>
    <MInfoName>: <MInfoValue>
  - name: <ProfileName>.<TypeName>
    <MInfoName>: <MInfoValue>
    <MInfoName>: <MInfoValue>
dependencies:
  - name: <DependencyName>
    target: <TreePath>
    stereotypes:
      - name: <ProfileName>.<TypeName>
        <MInfoName>: <MInfoValue>
        <MInfoName>: <MInfoValue>
```

- `stereotypes` liste les stéréotypes appliqués au folder, sur le même modèle que ceux du component (cf. [§ 5](#5-component)).
- `dependencies` liste les dependencies portées par le folder vers d'autres folders (cf. [§ 6. Dependency](#6-dependency)).

### 4.1. Référence à un folder

Une référence à un folder est son nom qualifié (cf. [§ 8](#8-nom-qualifié)).

- Pour faire référence à un folder dans un fichier yaml : `<TreePath>`.
  Exemple : `org.trainingorchestra.architecture.applicative.4_13`.
- La racine du modèle n'est pas un folder : on ne peut pas y faire référence.

## 5. Component

Le nom du component est le nom de son fichier `<ComponentName>.yaml`, sans l'extension.

Les fichiers `metainfo.yaml`, `<DiagramName>.architecturediagram.yaml` (diagrammes, cf. [ArchitectureDiagrams.md](ArchitectureDiagrams.md)) et `<NoteName>.note.yaml` (notes, cf. [§ 7](#7-note)) ne sont pas des components.

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

Une référence à un component est son nom qualifié (cf. [§ 8](#8-nom-qualifié)).

- Pour faire référence à un component dans un fichier yaml : `<TreePath>.<ComponentName>`.
  Exemple : `org.trainingorchestra.architecture.applicative.4_13.Concerto`.
- Pour faire référence à un component interne dans un fichier yaml : `<TreePath>.<ComponentName>.<ComponentName>`.
  Exemple : `org.trainingorchestra.architecture.applicative.4_13.Concerto.API`.

## 6. Dependency

Une dependency relie deux éléments de même genre :

- une dependency de component, d'un component vers un autre component, est un item de la clef `dependencies` du component qui la porte (cf. template du [§ 5](#5-component)) ;
- une dependency de folder, d'un folder vers un autre folder, est un item de la clef `dependencies` du `metainfo.yaml` du folder qui la porte (cf. template du [§ 4](#4-folder)).

Dans les deux cas :

- `name` est le nom de la dependency ;
- `target` désigne l'élément cible par son nom qualifié : un component pour une dependency de component, selon les règles du [§ 5.2](#52-référence-à-un-component), un folder pour une dependency de folder, selon les règles du [§ 4.1](#41-référence-à-un-folder) ;
- `stereotypes` liste les stéréotypes appliqués à la dependency, sur le même modèle que ceux du component.

## 7. Note

Une note se matérialise par deux fichiers de même nom, placés côte à côte dans un folder du modèle :

- `<NoteName>.note.yaml`, qui attache la note à un élément et porte ses stéréotypes ;
- `<NoteName>.note.md`, qui contient le texte de la note, en Markdown libre.

Une note n'est placée ni à la racine du modèle, ni dans un répertoire de components internes.

Template du fichier `<NoteName>.note.yaml` :

```yaml
type: architecture.note
target: <QualifiedName>
stereotypes:
  - name: <ProfileName>.<TypeName>
    <MInfoName>: <MInfoValue>
    <MInfoName>: <MInfoValue>
```

- `target` est obligatoire : une note est toujours attachée à un élément. Il désigne cet élément par son nom qualifié (cf. [§ 8](#8-nom-qualifié)) : un folder, un component (interne ou non), une dependency (de component ou de folder) ou un diagram. Une note ne peut être attachée ni à la racine du modèle, ni à une autre note.
- `stereotypes` liste les stéréotypes appliqués à la note, sur le même modèle que ceux du component.

Exemple : la note `ConcertoHosting` du folder `applicative` de l'[exemple](../samples/architectureModel/applicative/ConcertoHosting.note.yaml), attachée au component `applicative.Concerto` :

```
applicative/
├── Concerto.yaml
├── ConcertoHosting.note.yaml
└── ConcertoHosting.note.md
```

```yaml
type: architecture.note
target: applicative.Concerto
stereotypes:
  - name: org.mathouic.modelia.architecture.profile.applicative.V01_00_000.Decision
    date: 2026-10-19
    status: accepted
```

## 8. Nom qualifié

Cf. la notion de nom qualifié dans les [définitions](../Definitions.md#nom-qualifié).

| Élément | Nom qualifié | Exemple |
|---|---|---|
| model | Le nom du répertoire racine du modèle | `architectureModel` |
| folder | `<TreePath>` | `org.trainingorchestra.architecture.applicative.4_13` |
| component | `<TreePath>.<ComponentName>` | `org.trainingorchestra.architecture.applicative.4_13.Concerto` |
| component interne | Le nom qualifié de son component suivi de `.<ComponentName>` | `org.trainingorchestra.architecture.applicative.4_13.Concerto.API` |
| dependency de component | Le nom qualifié du component qui la porte suivi de `.<DependencyName>` | `org.trainingorchestra.architecture.applicative.4_13.Concerto.database` |
| dependency de folder | Le nom qualifié du folder qui la porte suivi de `.<DependencyName>` | `org.trainingorchestra.architecture.applicative.4_13.technical` |
| diagram | `<TreePath>.<DiagramName>` | `org.trainingorchestra.architecture.applicative.4_13.Overview` |
| note | `<TreePath>.<NoteName>` | `org.trainingorchestra.architecture.applicative.4_13.ConcertoHosting` |

Un component interne et une dependency d'un même component ne peuvent donc pas porter le même nom (cf. règle `M-35` de [ValidationArchitecture.md](ValidationArchitecture.md#44-dependency)). De même, une dependency d'un folder ne porte le nom d'aucun component, sous-folder, diagramme ou note de ce folder (cf. règle `M-36`), et une note ne porte le nom d'aucun component, sous-folder, diagramme ou dependency de son folder (cf. règle `M-74` de [ValidationArchitecture.md](ValidationArchitecture.md#47-note)).

Les noms qualifiés des profils, stéréotypes, méta-informations et icônes sont décrits dans [ReferentialArchitectureProfiles.md](ReferentialArchitectureProfiles.md#6-nom-qualifié).
