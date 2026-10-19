# Conventions

Dans les documents d'architecture, une chaîne de caractères entre `<` et `>` est une variable.

## Variables

| Variable | Signification |
|---|---|
| `<ProfilePath>` | Chemin d'un référentiel de profils d'architecture, relatif à la racine du modèle ou absolu |
| `<ProfileName>` | Nom du profil, i.e. un répertoire nommé `<ProfileBaseName>.<ProfileVersion>` (ex. `org.trainingorchestra.architecture.profile.applicative.V01_00_000`) |
| `<ProfileBaseName>` | Nom du profil sans sa version, en notation inversée (ex. `org.trainingorchestra.architecture.profile.applicative`) |
| `<ProfileVersion>` | Version d'un profil au format `Vxx_yy_zzz`, où `xx`, `yy` et `zzz` sont des nombres de 2, 2 et 3 chiffres complétés par des zéros à gauche (ex. `V01_02_003`) |
| `<TypeName>` | Nom du stéréotype |
| `<TargetType>` | Target du stéréotype : `folder` \| `component` \| `dependency` \| `note` |
| `<MInfoType>` | Type de la méta-information (voir ci-dessous) |
| `<MInfoName>` | Nom de la méta-information |
| `<MInfoValue>` | Valeur de la méta-information, conforme à son `<MInfoType>` |
| `<ComponentName>` | Nom d'un component |
| `<DependencyName>` | Nom d'une dependency |
| `<DiagramName>` | Nom d'un diagramme |
| `<NoteName>` | Nom d'une note |
| `<QualifiedName>` | Nom qualifié d'un élément du modèle (folder, component, dependency, diagram ou note) |
| `<IconKey>` | Clef d'une icône d'un stéréotype, par exemple sa taille (ex. `24`, `48`) |
| `<TreePath>` | Chemin d'un folder du modèle depuis la racine du modèle, les noms de folders étant séparés par des `.` |

## Espaces de noms

| Espace de noms | Contenu |
|---|---|
| `org.trainingorchestra` | Les objets spécifiques à la société Training Orchestra (ex. le component Concerto) |
| `org.mathouic.modelia` | Les objets indépendants de Training Orchestra, destinés à être publiés en open source (ex. un navigateur) |

## Valeurs de `<MInfoType>`

| Type | Signification |
|---|---|
| `string` | Chaîne de caractères |
| `string*` | Liste de `string` |
| `number` | Nombre flottant |
| `number*` | Liste de `number` |
| `date` | Date au format `yyyy-MM-dd` |
| `date*` | Liste de `date` |
| `time` | Temps au format `hh:mm:ss:mmmm` |
| `time*` | Liste de `time` |
