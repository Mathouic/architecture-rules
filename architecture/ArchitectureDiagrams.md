# Implémentation d'un diagramme de modèle d'architecture

## 0. Conventions

Les conventions (variables `<...>`, types de méta-informations) sont décrites dans [Conventions.md](../Conventions.md).

## 1. Objet

L'objet de ce document est l'implémentation d'un diagramme de modèle d'architecture (cf. les [définitions](../Definitions.md#diagramme-de-modèle-darchitecture)).

## 2. Fichier

Un diagramme se matérialise par deux fichiers de même nom, placés côte à côte dans un folder du modèle (cf. [ReferentialArchitectureModels.md](ReferentialArchitectureModels.md#4-folder)) :

- `<DiagramName>.architecturediagram.yaml`, obligatoire, qui contient la définition du diagramme (cf. [§ 3](#3-contenu)) ;
- `<DiagramName>.architecturediagram.mmd`, optionnel, qui contient le dessin du diagramme en Mermaid (cf. [§ 4](#4-dessin)). Il est absent tant que le dessin n'a pas été généré.

Exemple :

```
applicative/
├── UserToConcerto.architecturediagram.yaml
└── UserToConcerto.architecturediagram.mmd
```

- Un diagramme n'est placé ni à la racine du modèle, ni dans un répertoire de components internes.
- Un fichier `<DiagramName>.architecturediagram.mmd` n'existe pas sans le fichier `<DiagramName>.architecturediagram.yaml` de même nom.
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
  - note: <TreePath>.<NoteName>
    icon: <ProfileName>.<TypeName>_<IconKey>
    visibleMetaInfos:
      - <ProfileName>.<TypeName>.<MInfoName>
```

- `objects` liste les objets visibles dans le dessin. Chaque item désigne un objet par une clef qui donne son genre :
  - `component` : un component (ou un component interne), désigné par son nom qualifié ;
  - `folder` : un folder, désigné par son nom qualifié ;
  - `dependency` : une dependency de component ou de folder, désignée par son nom qualifié ;
  - `note` : une note, désignée par son nom qualifié (cf. [ReferentialArchitectureModels.md § 7](ReferentialArchitectureModels.md#7-note)).
- `icon` est optionnelle et réservée aux components, aux folders et aux notes : c'est le nom qualifié de l'icône d'un stéréotype appliqué à l'objet (cf. [ReferentialArchitectureProfiles.md § 4](ReferentialArchitectureProfiles.md#4-profil)).
- `visibleMetaInfos` est optionnelle : elle liste, par leurs noms qualifiés, les méta-informations des stéréotypes appliqués à l'objet qui sont affichées dans le dessin.

Les noms qualifiés sont décrits dans [ReferentialArchitectureModels.md § 8](ReferentialArchitectureModels.md#8-nom-qualifié) et [ReferentialArchitectureProfiles.md § 6](ReferentialArchitectureProfiles.md#6-nom-qualifié).

## 4. Dessin

Le dessin est le fichier `<DiagramName>.architecturediagram.mmd`, en Mermaid. Il est généré à partir de la définition du diagramme, le fichier `<DiagramName>.architecturediagram.yaml`, qui fait foi.

- Le dessin n'est pas modifié à la main : toute évolution passe par la définition, puis le dessin est généré à nouveau.
- Après une modification de la définition, le dessin est généré à nouveau ; sinon il ne reflète plus le diagramme.
- Aucun objet absent de `objects` ne doit apparaître dans le dessin.
- Une dependency n'est donc dessinable que si l'élément qui la porte (component ou folder) et son élément cible sont tous deux dans `objects`.
- Une note est dessinée reliée à l'élément auquel elle est attachée (son `target`) ; elle n'est donc dessinable que si cet élément est aussi dans `objects`.

### 4.1. Format du dessin

Le dessin est lisible par Mermaid 10 et par les versions suivantes : il n'utilise pas la syntaxe `@{ … }` des formes étendues, apparue avec Mermaid 11.3, qu'ignorent encore de nombreux outils de visualisation.

- La première ligne est le commentaire `%% Généré à partir de <DiagramName>.architecturediagram.yaml : ne pas modifier à la main.`, suivi de `flowchart LR`.
- L'identifiant Mermaid d'un objet est son nom qualifié, dont les `.` sont remplacés par des `_`.
- Un component est un nœud rectangulaire `<Id>["<Libellé>"]`. Si l'objet a une `icon`, le libellé commence par la balise `<img src='<Chemin>' width='100' height='100'/><br/>`, où `<Chemin>` est le chemin du fichier de l'icône, relatif au folder du dessin. Les attributs de la balise sont entre apostrophes, car le libellé est entre guillemets.
- Une note est un nœud rectangulaire de classe `note`, déclarée par `classDef note fill:#fff8c4,stroke:#c9b458,stroke-dasharray:4 2`, et reliée à son `target` par un lien pointillé `-.-`.
- Le libellé contient ensuite le nom de l'objet, puis une ligne `<br/><MInfoName> : <MInfoValue>` par méta-information visible ; les valeurs d'une liste sont séparées par des virgules. Les caractères `<`, `>` et `"` d'une valeur sont écrits `&lt;`, `&gt;` et `&quot;`.
- Une dependency est un lien `<IdSource> -->|<DependencyName>| <IdCible>`.

Si une icône est introuvable, seule l'image manque : le reste du dessin s'affiche.

## 5. Exemple

Le fichier `applicative/UserToConcerto.architecturediagram.yaml` de l'[exemple](../samples/architectureModel/applicative/UserToConcerto.architecturediagram.yaml) montre aussi la note `ConcertoHosting`, attachée au component `applicative.Concerto` :

```yaml
objects:
  - component: applicative.User
    icon: org.mathouic.modelia.architecture.profile.applicative.V01_00_000.User_100
  - component: applicative.Concerto
    icon: org.trainingorchestra.architecture.profile.applicative.V01_00_000.Concerto_100
    visibleMetaInfos:
      - org.trainingorchestra.architecture.profile.applicative.V01_00_000.Concerto.version
  - dependency: applicative.User.concerto
  - note: applicative.ConcertoHosting
    visibleMetaInfos:
      - org.mathouic.modelia.architecture.profile.applicative.V01_00_000.Decision.status
```

Son dessin, le fichier `applicative/UserToConcerto.architecturediagram.mmd` de l'[exemple](../samples/architectureModel/applicative/UserToConcerto.architecturediagram.mmd), a été généré à partir de cette définition :

```mermaid
%% Généré à partir de UserToConcerto.architecturediagram.yaml : ne pas modifier à la main.
flowchart LR
    classDef note fill:#fff8c4,stroke:#c9b458,stroke-dasharray:4 2

    applicative_User["<img src='../../architectureProfile/org.mathouic.modelia.architecture.profile.applicative.V01_00_000/icons/User_100.png' width='100' height='100'/><br/>User"]
    applicative_Concerto["<img src='../../architectureProfile/org.trainingorchestra.architecture.profile.applicative.V01_00_000/icons/Concerto_100.png' width='100' height='100'/><br/>Concerto<br/>version : 4.13.28"]
    applicative_ConcertoHosting["ConcertoHosting<br/>status : accepted"]:::note

    applicative_User -->|concerto| applicative_Concerto
    applicative_ConcertoHosting -.- applicative_Concerto
```

Le fichier `applicative/ApplicativeToTechnical.architecturediagram.yaml` de l'[exemple](../samples/architectureModel/applicative/ApplicativeToTechnical.architecturediagram.yaml) montre une dependency de folder, `technical`, portée par le `metainfo.yaml` du folder `applicative` :

```yaml
objects:
  - folder: applicative
  - folder: technical
  - dependency: applicative.technical
```

Son dessin n'a pas encore été généré : le fichier `applicative/ApplicativeToTechnical.architecturediagram.mmd` est absent.
