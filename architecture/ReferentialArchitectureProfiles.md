# Implémentation d'un référentiel de profils d'architecture

## 0. Conventions

Les conventions (variables `<...>`, types de méta-informations) sont décrites dans [Conventions.md](../Conventions.md).

## 1. Objet

L'objet de ce document est l'implémentation d'un référentiel de profils d'architecture.

## 2. Méta-éléments

Cf. les méta-éléments d'un modèle d'architecture dans les [définitions](../Definitions.md#modèle-darchitecture).

Chaque méta-élément, à l'exception de `model` et `diagram`, peut être étendu par un stéréotype défini dans un profil.

## 3. Racine du référentiel

Le fichier `metainfo.yaml` à la racine d'un référentiel de profils d'architecture contient uniquement :

```yaml
type: architecture.profiles
```

## 4. Profil

Un profil est un répertoire `<ProfileName>` placé à la racine d'un référentiel de profils d'architecture.

Le nom d'un profil porte sa version : `<ProfileName>` est construit comme `<ProfileBaseName>.<ProfileVersion>`. Chaque version d'un profil est donc un répertoire distinct, et plusieurs versions d'un même profil peuvent coexister dans un référentiel.

Exemple : `architectureProfile/org.mathouic.modelia.architecture.profile.applicative.V01_00_000/`.

Un profil contient :

- un fichier `metainfo.yaml` obligatoire ;
- ses stéréotypes (cf. [§ 5](#5-stéréotype)) ;
- un répertoire `icons/` optionnel, qui contient les icônes des stéréotypes.

Une icône est un fichier `icons/<TypeName>_<IconKey>.png`, où `<TypeName>` est un stéréotype du profil. Un stéréotype peut avoir plusieurs icônes, par exemple une par taille : `User_24.png`, `User_48.png`.

Le fichier `metainfo.yaml` d'un profil contient uniquement :

```yaml
type: architecture.profile
```

## 5. Stéréotype

Un stéréotype se matérialise par un fichier du profil nommé `<TypeName>.<TargetType>.yaml`.

Template du fichier `<TypeName>.<TargetType>.yaml` :

```yaml
type: architecture.stereotype
metainfo:
  <MInfoName>: <MInfoType>
  <MInfoName>: <MInfoType>
  <MInfoName>: <MInfoType>
```

- `metainfo` déclare les méta-informations du stéréotype, chacune par son nom et son type. Un nom de méta-information est unique dans un stéréotype.

Exemple : le fichier `Version.folder.yaml` du profil `org.mathouic.modelia.architecture.profile.applicative.V01_00_000` :

```yaml
type: architecture.stereotype
metainfo:
  version: string
```

## 6. Nom qualifié

Cf. la notion de nom qualifié dans les [définitions](../Definitions.md#nom-qualifié). Pour faire référence à un élément d'un profil dans un fichier yaml, on utilise son nom qualifié.

| Élément | Nom qualifié | Exemple |
|---|---|---|
| profil | `<ProfileName>` | `org.mathouic.modelia.architecture.profile.applicative.V01_00_000` |
| stéréotype | `<ProfileName>.<TypeName>` | `org.mathouic.modelia.architecture.profile.applicative.V01_00_000.Version` |
| méta-information | `<ProfileName>.<TypeName>.<MInfoName>` | `org.mathouic.modelia.architecture.profile.applicative.V01_00_000.Version.version` |
| icône | `<ProfileName>.<TypeName>_<IconKey>` | `org.mathouic.modelia.architecture.profile.applicative.V01_00_000.User_24` |
