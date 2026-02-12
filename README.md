# Module 4 - Création d'applications contenant plusieurs activités

Ce travail pratique a pour objectif de comprendre et maîtriser la
gestion de plusieurs Activity dans une application Android, ainsi que
l'utilisation des Intent explicites pour naviguer et transmettre des
données entre écrans.

# Objectifs pédagogiques

À la fin de ce TP, vous serez capable de :

-   Créer une nouvelle Activity dans Android Studio
-   Comprendre la pile d'activités (Back Stack)
-   Démarrer une Activity avec un Intent explicite
-   Transmettre des données entre activités via des extras
-   Renvoyer des données vers l'Activity appelante
-   Utiliser l'API ActivityResultLauncher
-   Mettre en place une relation parent/enfant dans le
    AndroidManifest.xml

# Application à réaliser : TwoActivities

L'application contient :

-   MainActivity
-   SecondActivity

## Fonctionnement attendu

1.  L'utilisateur saisit un message dans MainActivity
2.  Il clique sur Send
3.  SecondActivity s'ouvre et affiche le message reçu
4.  L'utilisateur saisit une réponse
5.  Il clique sur Reply
6.  La réponse est renvoyée vers MainActivity
7.  MainActivity affiche la réponse reçue

# Concepts clés abordés

## Activity

Une Activity représente un écran unique dans Android.

Chaque Activity : - possède son propre fichier XML - est déclarée dans
le AndroidManifest.xml - est indépendante des autres

Android utilise une pile (Back Stack) pour gérer la navigation.

## Intent explicite

Un Intent explicite cible une Activity précise :

``` java
Intent intent = new Intent(this, SecondActivity.class);
startActivity(intent);
```

Il permet : - de naviguer entre écrans - de transmettre des données

## Transmission de données (Extras)

Les données sont envoyées sous forme de paires clé/valeur :

``` java
intent.putExtra(EXTRA_MESSAGE, message);
```

Dans l'Activity cible :

``` java
Intent intent = getIntent();
String message = intent.getStringExtra(MainActivity.EXTRA_MESSAGE);
```

## Renvoyer des données

Initialisation :

``` java
launcher = registerForActivityResult(
    new ActivityResultContracts.StartActivityForResult(),
    result -> {
        if (result.getResultCode() == RESULT_OK) {
            Intent data = result.getData();
            String reply = data.getStringExtra(SecondActivity.EXTRA_REPLY);
        }
    }
);
```

Lancement :

``` java
launcher.launch(intent);
```

Dans SecondActivity :

``` java
Intent replyIntent = new Intent();
replyIntent.putExtra(EXTRA_REPLY, reply);
setResult(RESULT_OK, replyIntent);
finish();
```


# Structure attendue du projet
```
TwoActivities/
│
├── MainActivity.java
├── SecondActivity.java
├── res/
│   ├── layout/
│   │   ├── activity_main.xml
│   │   └── activity_second.xml
│   ├── values/
│   │   └── strings.xml
│
└── AndroidManifest.xml
```

# Compétences validées

-   Navigation entre écrans
-   Gestion du cycle de vie d'une Activity
-   Utilisation des Intent
-   Gestion des extras
-   Manipulation de View (visibility)
-   Utilisation d'ActivityResultLauncher
-   Compréhension de la pile Android

# Tests à effectuer

-   Cliquer sur Send sans message
-   Tester le bouton Retour
-   Vérifier la flèche de navigation (parentActivityName)
-   Tester plusieurs échanges successifs
-   Observer le comportement en rotation

# Points d'attention

-   Les clés d'Intent doivent être constantes
-   Toujours vérifier que data != null
-   Ne pas oublier setResult() avant finish()
-   Bien importer les classes AndroidX nécessaires

# Travail demandé

-   Implémentation complète
-   Code propre et commenté
-   Respect des bonnes pratiques Android
-   Test fonctionnel complet

## Licence

Ce projet est distribué sous la licence **Academic Free License v3.0 ([AFL-3.0](https://opensource.org/licenses/AFL-3.0))**.
