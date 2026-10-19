# Validation d'un référentiel de profils et d'un modèle d'architecture

## 0. Conventions

Les conventions (variables `<...>`, types de méta-informations) sont décrites dans [Conventions.md](../Conventions.md).

## 1. Objet

L'objet de ce document est l'ensemble des règles qu'un référentiel de profils d'architecture et un modèle d'architecture doivent respecter pour être valides.

Les règles complètent :

- [ReferentialArchitectureProfiles.md](ReferentialArchitectureProfiles.md) pour le référentiel de profils ;
- [ReferentialArchitectureModels.md](ReferentialArchitectureModels.md) pour le modèle.

Chaque règle porte un identifiant :

| Préfixe | Portée |
|---|---|
| `P-` | Référentiel de profils, validable seul |
| `M-` | Modèle d'architecture, validable seul |
| `X-` | Modèle d'architecture confronté au référentiel de profils |

La violation d'une règle rend le référentiel ou le modèle invalide.

## 2. Règles générales

| Id | Règle |
|---|---|
| `G-01` | Tout fichier `.yaml` est un document YAML syntaxiquement valide. |
| `G-02` | Une même clef n'apparaît pas deux fois dans une même map YAML. |
| `G-03` | Les clefs fixées par les templates (`type`, `profiles`, `profilePath`, `profileName`, `metainfo`, `stereotypes`, `dependencies`, `innerComponents`, `name`, `target`, `objects`, `component`, `folder`, `dependency`, `note`, `icon`, `visibleMetaInfos`) sont écrites en respectant la casse. |

## 3. Référentiel de profils

### 3.1. Racine

| Id | Règle |
|---|---|
| `P-01` | La racine contient un fichier `metainfo.yaml`. |
| `P-02` | Le fichier `metainfo.yaml` de la racine contient uniquement `type: architecture.profiles`. |
| `P-03` | La racine ne contient que le fichier `metainfo.yaml` et des répertoires `<ProfileName>`. |

### 3.2. Profil

| Id | Règle |
|---|---|
| `P-10` | `<ProfileName>` est de la forme `<ProfileBaseName>.<ProfileVersion>`, où `<ProfileBaseName>` est une suite d'au moins deux segments non vides séparés par des `.`. |
| `P-11` | `<ProfileBaseName>` commence par un espace de noms déclaré dans les [conventions](../Conventions.md#espaces-de-noms) (`org.trainingorchestra.` ou `org.mathouic.modelia.`). |
| `P-12` | Un profil ne contient que le fichier `metainfo.yaml`, des fichiers de stéréotypes `<TypeName>.<TargetType>.yaml` et un répertoire `icons/` optionnel. |
| `P-13` | Un profil contient un fichier `metainfo.yaml`. |
| `P-14` | Le fichier `metainfo.yaml` d'un profil contient uniquement `type: architecture.profile`. |
| `P-15` | Le dernier segment de `<ProfileName>` est un `<ProfileVersion>` conforme au format `Vxx_yy_zzz` (cf. [conventions](../Conventions.md#variables)). |

### 3.3. Stéréotype

Ces règles valent pour chaque fichier de stéréotype d'un profil, c'est-à-dire chaque fichier autre que `metainfo.yaml`.

| Id | Règle |
|---|---|
| `P-20` | Le nom du fichier se découpe en exactement trois segments séparés par des `.` : `<TypeName>`, `<TargetType>`, `yaml`. |
| `P-21` | `<TypeName>` ne contient pas de `.`. Sinon la référence `<ProfileName>.<TypeName>` ne peut pas être découpée. |
| `P-22` | `<TargetType>` vaut `folder`, `component`, `dependency` ou `note`. Les valeurs `model` et `diagram` sont interdites. |
| `P-23` | `<TypeName>` est unique dans un profil, toutes cibles confondues. Exemple : `Version.folder.yaml` et `Version.component.yaml` ne peuvent pas coexister dans un même profil, car ils auraient la même référence `<ProfileName>.Version`. |
| `P-24` | Le fichier contient la clef `type` de valeur `architecture.stereotype`. |
| `P-25` | Le fichier ne contient pas d'autres clefs que `type` et `metainfo`. La clef `metainfo` est optionnelle : un stéréotype peut ne déclarer aucune méta-information. |
| `P-26` | Si elle est présente, `metainfo` est une map dont chaque clef est un `<MInfoName>` et chaque valeur un `<MInfoType>`. |
| `P-27` | Chaque `<MInfoType>` est une des valeurs listées dans les [conventions](../Conventions.md#valeurs-de-minfotype). |
| `P-28` | `<MInfoName>` est différent de `name`, réservé dans le modèle pour désigner le stéréotype appliqué. |
| `P-29` | `<MInfoName>` ne contient pas de `.`. Sinon le nom qualifié `<ProfileName>.<TypeName>.<MInfoName>` ne peut pas être découpé. |

### 3.4. Icône

| Id | Règle |
|---|---|
| `P-30` | Le répertoire `icons/` d'un profil ne contient que des fichiers `<TypeName>_<IconKey>.png`, où `<TypeName>` est un stéréotype du profil. |
| `P-31` | `<IconKey>` ne contient ni `.` ni `_`. Sinon le nom qualifié `<ProfileName>.<TypeName>_<IconKey>` ne peut pas être découpé. |
## 4. Modèle d'architecture

### 4.1. Racine

| Id | Règle |
|---|---|
| `M-01` | La racine contient un fichier `metainfo.yaml`. |
| `M-02` | Le fichier `metainfo.yaml` de la racine contient la clef `type`, de valeur `architecture.models`, et la clef optionnelle `profiles`, sans aucune autre clef. |
| `M-03` | Si elle est présente, `profiles` est une séquence de maps. |
| `M-04` | Chaque item de `profiles` contient exactement les clefs `profilePath` et `profileName`. |
| `M-05` | Deux items de `profiles` n'ont pas le même `<ProfileBaseName>` : un modèle n'utilise qu'une seule version d'un même profil. |

### 4.2. Folder

| Id | Règle |
|---|---|
| `M-10` | Le nom d'un folder ne contient pas de `.`, séparateur des segments de `<TreePath>`. |
| `M-11` | Un folder ne porte pas le même nom qu'un component du même folder. Sinon le répertoire serait interprété comme le répertoire des components internes de ce component (cf. [§ 5.1](ReferentialArchitectureModels.md#51-components-internes)). |
| `M-12` | Si le fichier `metainfo.yaml` d'un folder est présent, il contient la clef `type` de valeur `architecture.folder` et au moins une clef non vide parmi `stereotypes` et `dependencies`. |
| `M-13` | Le fichier `metainfo.yaml` d'un folder ne contient pas d'autres clefs que `type`, `stereotypes` et `dependencies`. |

### 4.3. Component

Ces règles valent pour chaque fichier `.yaml` d'un folder ou d'un répertoire de components internes, à l'exception de `metainfo.yaml`, des diagrammes `<DiagramName>.architecturediagram.yaml` et des notes `<NoteName>.note.yaml`.

| Id | Règle |
|---|---|
| `M-20` | `<ComponentName>` ne contient pas de `.`, séparateur des références `<TreePath>.<ComponentName>`. |
| `M-21` | Le fichier contient la clef `type` de valeur `architecture.component`. |
| `M-22` | Le fichier ne contient pas d'autres clefs que `type`, `stereotypes`, `dependencies` et `innerComponents`. En particulier, il n'a pas de clef `name` : le nom du component est celui de son fichier. |
| `M-23` | Chaque nom listé dans `innerComponents` correspond à un fichier `<ComponentName>/<Nom>.yaml`, placé à côté du fichier du component. |
| `M-24` | Réciproquement, chaque fichier `.yaml` du répertoire `<ComponentName>/` est listé dans `innerComponents`. |
| `M-25` | Le répertoire `<ComponentName>/` n'existe que si `innerComponents` est non vide, et il ne contient pas de fichier `metainfo.yaml`. |
| `M-26` | Un nom n'apparaît pas deux fois dans `innerComponents`. |

### 4.4. Dependency

Ces règles valent pour chaque item d'une clef `dependencies`, qu'elle soit portée par un component ou par le `metainfo.yaml` d'un folder.

| Id | Règle |
|---|---|
| `M-30` | Chaque item de `dependencies` a une clef `name` et une clef `target`. |
| `M-31` | Le `name` d'une dependency est unique parmi les dependencies de l'élément qui la porte. |
| `M-32` | `target` désigne un élément existant du modèle, du même genre que l'élément qui porte la dependency : un component pour une dependency de component, selon les règles de [référence à un component](ReferentialArchitectureModels.md#52-référence-à-un-component), un folder pour une dependency de folder, selon les règles de [référence à un folder](ReferentialArchitectureModels.md#41-référence-à-un-folder). |
| `M-33` | Un item de `dependencies` ne contient pas d'autres clefs que `name`, `target` et `stereotypes`. |
| `M-34` | `<DependencyName>` ne contient pas de `.`. Sinon le nom qualifié de la dependency ne peut pas être découpé. |
| `M-35` | Le `<DependencyName>` d'une dependency de component est différent du nom de chaque component interne de ce component. Sinon les deux auraient le même nom qualifié. |
| `M-36` | Le `<DependencyName>` d'une dependency de folder est différent du nom de chaque component, sous-folder, diagramme et note de ce folder. Sinon les deux auraient le même nom qualifié. |

### 4.5. Stéréotype appliqué

Ces règles valent pour chaque item d'une clef `stereotypes`, qu'elle soit portée par un folder, un component, une dependency ou une note.

| Id | Règle |
|---|---|
| `M-40` | L'item a une clef `name` de la forme `<ProfileName>.<TypeName>`. |
| `M-41` | Un même stéréotype n'est pas appliqué deux fois au même élément. |

### 4.6. Diagram

Cf. [ArchitectureDiagrams.md](ArchitectureDiagrams.md).

| Id | Règle |
|---|---|
| `M-60` | Un diagramme est un fichier `<DiagramName>.architecturediagram.yaml` placé dans un folder, ni à la racine du modèle, ni dans un répertoire de components internes. `<DiagramName>` ne contient pas de `.`. |
| `M-61` | `<DiagramName>` est différent du nom de chaque component et de chaque note du même folder. Sinon les deux auraient le même nom qualifié. |
| `M-62` | Le fichier contient exactement la clef `objects`. |
| `M-63` | `objects` est une séquence non vide. |
| `M-64` | Chaque item de `objects` contient exactement une clef de genre, `component`, `folder`, `dependency` ou `note`. Un item `component`, `folder` ou `note` peut aussi contenir `icon` et `visibleMetaInfos` ; un item `dependency` peut aussi contenir `visibleMetaInfos`. Aucune autre clef n'est autorisée. |
| `M-65` | La valeur de `component` est le nom qualifié d'un component existant du modèle. |
| `M-66` | La valeur de `dependency` est le nom qualifié d'une dependency existante du modèle, dependency de component ou de folder. |
| `M-67` | Un même objet n'apparaît pas deux fois dans `objects`. |
| `M-68` | Pour chaque item `dependency`, l'élément qui porte la dependency et son `target` figurent aussi dans `objects`. Sinon le dessin montrerait un objet absent de la liste. |
| `M-69` | La valeur de `folder` est le nom qualifié d'un folder existant du modèle. |

Les règles des items `note` d'un diagramme sont `M-75` et `M-76` (cf. [§ 4.7](#47-note)).

### 4.7. Note

Cf. [ReferentialArchitectureModels.md § 7](ReferentialArchitectureModels.md#7-note).

| Id | Règle |
|---|---|
| `M-70` | Une note est un fichier `<NoteName>.note.yaml` placé dans un folder, ni à la racine du modèle, ni dans un répertoire de components internes. `<NoteName>` ne contient pas de `.`. |
| `M-71` | Chaque fichier `<NoteName>.note.yaml` a un fichier `<NoteName>.note.md` dans le même folder, et réciproquement. |
| `M-72` | Le fichier `<NoteName>.note.yaml` contient la clef `type` de valeur `architecture.note` et la clef `target`, et pas d'autres clefs que `type`, `target` et `stereotypes`. |
| `M-73` | `target` est le nom qualifié d'un élément existant du modèle : un folder, un component (interne ou non), une dependency (de component ou de folder) ou un diagram. |
| `M-74` | `<NoteName>` est différent du nom de chaque component, sous-folder, diagramme et dependency de folder du même folder. Sinon les deux auraient le même nom qualifié. |
| `M-75` | Dans un diagramme, la valeur de `note` est le nom qualifié d'une note existante du modèle. |
| `M-76` | Dans un diagramme, pour chaque item `note`, l'élément désigné par le `target` de la note figure aussi dans `objects`. Sinon le dessin montrerait un objet absent de la liste. |

## 5. Modèle confronté au référentiel de profils

### 5.1. Profils déclarés

Ces règles valent pour chaque item de la clef `profiles` du `metainfo.yaml` racine du modèle.

| Id | Règle |
|---|---|
| `X-01` | `profilePath`, résolu depuis la racine du modèle s'il est relatif, désigne un répertoire existant qui respecte `P-01` et `P-02` : c'est la racine d'un référentiel de profils. |
| `X-02` | Ce référentiel contient un profil nommé `profileName`. |

### 5.2. Stéréotypes appliqués

Les profils utilisables par un modèle sont ceux qu'il déclare dans la clef `profiles` de son `metainfo.yaml` racine.

Pour résoudre une référence `<ProfileName>.<TypeName>`, on la découpe au dernier `.` : la partie gauche est `<ProfileName>`, la partie droite `<TypeName>` (ce découpage est rendu sûr par `P-21`).

Une méta-information déclarée par un stéréotype est optionnelle : l'élément stéréotypé n'est pas tenu de la renseigner.

| Id | Règle |
|---|---|
| `X-10` | `<ProfileName>` est le `profileName` d'un item de `profiles`. |
| `X-11` | Ce profil contient un fichier `<TypeName>.<TargetType>.yaml`. |
| `X-12` | Le `<TargetType>` du stéréotype correspond au méta-élément qui le porte : `folder` dans le `metainfo.yaml` d'un folder, `component` à la racine d'un fichier component, `dependency` dans un item de `dependencies`, que celui-ci soit dans un fichier component ou dans le `metainfo.yaml` d'un folder, `note` dans un fichier `<NoteName>.note.yaml`. |
| `X-13` | Chaque clef de l'item, hors `name`, est un `<MInfoName>` déclaré dans le `metainfo` du stéréotype. |
| `X-14` | Chaque `<MInfoValue>` est conforme à son `<MInfoType>` (cf. [§ 5.3](#53-conformité-dune-valeur)). |

### 5.3. Conformité d'une valeur

La conformité est vérifiée sur la forme textuelle de la valeur dans le fichier YAML, et non sur le type déduit par un parseur YAML (qui peut, par exemple, convertir `2026-10-15` en date ou `4.13` en nombre).

| `<MInfoType>` | Valeur conforme |
|---|---|
| `string` | Un scalaire YAML |
| `number` | Un nombre décimal, entier ou à virgule flottante, avec `.` comme séparateur décimal |
| `date` | Une date `yyyy-MM-dd` existante dans le calendrier |
| `time` | Un temps `hh:mm:ss:mmmm` |
| `<type>*` | Une séquence YAML, éventuellement vide, dont chaque élément est conforme à `<type>` |

### 5.4. Diagrammes

Ces règles valent pour chaque item de `objects` d'un diagramme.

Un nom qualifié d'icône `<ProfileName>.<TypeName>_<IconKey>` se découpe au dernier `.`, puis au dernier `_` (cf. `P-31`). Un nom qualifié de méta-information `<ProfileName>.<TypeName>.<MInfoName>` se découpe aux deux derniers `.` (cf. `P-21` et `P-29`).

| Id | Règle |
|---|---|
| `X-20` | Pour `icon`, `<ProfileName>` est le `profileName` d'un item de `profiles` et le profil contient le fichier `icons/<TypeName>_<IconKey>.png`. |
| `X-21` | Le stéréotype `<ProfileName>.<TypeName>` de l'icône est appliqué à l'objet (component, folder ou note). |
| `X-22` | Pour chaque item `<ProfileName>.<TypeName>.<MInfoName>` de `visibleMetaInfos`, le stéréotype `<ProfileName>.<TypeName>` existe et est appliqué à l'objet, et ce stéréotype déclare `<MInfoName>`. |
