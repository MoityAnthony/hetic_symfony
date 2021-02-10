# Installation

Pour utiliser Symfony il faut :

-  PHP 7.2.5 ou plus & les extensions suivantes :
 Ctype, iconv, JSON, PCRE, Session, SimpleXML, and Tokenizer.

 *Dans votre configuration MAMP ou WAMP, cela devrait être le cas.*

Ouvrez un terminal et tapez la ligne de commande suivante pour voir les extensions de votre PHP :

```bash
php -m
```

- De manière optionelle vous pouvez installer la CLI de Symfony :

*Remarques : une CLI ou Command Line Interface, est une suite de programmes exécutables en ligne de commandes, écrit en PHP et permettant de créer des controller, model, view. Elle permet également le testing et le déploiement de l'application.*


```bash
curl -sS https://get.symfony.com/cli/installer | bash
```

Une fois la CLI de Symfony installée vous pouvez y accéder de manière gloable :

```bash
mv /Users/votre_nom_de_machine/.symfony/bin/symfony /usr/local/bin/symfony
```

*Voir sur cette page les autres options selon votre OS.*

[Dowload Symfony](https://symfony.com/download)


- Il faut également le gestion de package PHP composer

Pour installer Symfony il faut composer d'installer sur votre machine et si c'est le cas il faut que celui-ci soit à jour.

Insallez le si ce n'est pas déjà fait ! Ou mettez le à jour si vous l'avez déjà sur votre machine :

```bash
# mise à jour de composer
composer self-update
```

- Installation du projet 

Vous pouvez soit installer Symfony soit en microservice, soit en full.

Microservice est très adapté par exemple pour la création d'API (Application programming Interface).

Full correspond à une installation pour les Web App "traditionnelle".

Pour découvrir Symfony nous allons faire les deux installations : microservice et full.

Vérifiez avant continuer que votre système est correctement configuré pour travailler avec SF :

```bash
symfony check:requirements
```

## Installation 

Nous allons découvrir deux types d'installation du Framework Symfony. 

Si vous avez la CLI Symfony tapez la ligne de commande suivante dans un dossier **Exercice** sur votre bureau. Pour l'instant vous n'avez pas besoin de Apache2 ou MySQL.

```bash
symfony new bar
```

Avec composer uniquement vous auriez pu taper la ligne de commande suivante :

```bash
composer create-project symfony/skeleton bar
```

Vérifiez la configuration, placez-vous dans le dossier **bar** et tapez la ligne de commande suivante :

```bash
php bin/console about
```

Nous allons également voir toutes les commandes de la console disponibles pour une application "microservice".

**Remarque** : une commande de la console permet par exemple de créer un "controller" automatiquement dans l'application (création de fichier). L'intérêt de créer un contrôleur à l'aide d'une commande de la console est que Symfony définit lui-même (import de classe du noyau) les dépendances de cette classe.

```bash
php bin/console
```

Nous reviendrons sur l'utilisation de ces commandes ci-dessous.

Nous allons lancer le serveur de test Symfony, dans le dossier de notre application, tapez la ligne de code suivante, vous pouvez avant de la lancer obtenir sa documentation, voyez la première ci-dessous.

```bash
symfony help server:start
```

Pour lancer le serveur lui-même :

```bash
symfony server:start
```

Ouvrez également un autre terminal à côté pour que l'on puisse exécuter des commmandes de la console Symfony.

## Application Framework HTTP

![HTTP workflow](images/http_application_flow.jpg)

1. Le framework utilise comme point d'entrée du traitement des requêtes un contrôleur (chef d'orchestre) particulier : FrontController.

2. Le Kernel en fonction de l'action du FrontController déclenchera une autre action spécifique, par exemple le router (gestion des urls). 

*Le router gère les actions/contrôleurs métiers.*

3. Une réponse est retournée au client HTTP. C'est Symfony qui définit cette réponse.


## Présentation de la structure d'une App Symfony

- config/
*Vous permet de configurer l'application ... Par exemples : définition des routes, services ou packages installés.*

- src/
*Cet ici que vous écrivez le code de l'application que vous développerez.*

- templates/
*Toutes les vues HTML/JS seront développées dans ce dossier, avec twig par exemple.*

- bin/
*Tous les exécutables PHP seront placés dans ce dossier. Vous trouverez nottament la fameuse console de Symfony.*
- var/
*Les fichiers qui se trouvent dans ce dossier sont créés automatiquement, vous trouverez par exemple les fichiers de cache de l'application ainsi que ses logs.*

- vendor/
Ce dossier contient toutes les dépendances de votre application comme Symfony par exemple.

- public/
*Dans ce dossier est placé tous les fichiers accessibles publiquement assets, js et le point d'entrée de votra application l'index.php*

## Projet 

### Installation

Nous allons créer une petite application "the bar". Utilisez l'application Symfony que nous avons créée précédement.

### Création du contrôleur BarController

Dans un Framework on utilise la ligne de commande pour créer les classes nécessaires. Voici les commandes utiles pour créer un contrôleur avec Symfony

Comme nous utilisons l'option **microservice** pour découvrir Symfony, vous n'avez pas de commande pour l'instant vous permettant de créer automatiquement un contrôleur.

- Installez dans la console la commande suivante 

```bash
symfony composer req maker --dev
```

Vous pouvez maintentant lister tous les make possibles pour votre application à l'aide de la commande suivante, vous y trouverez la commande make:controller que nous allons utiliser.

```bash
symfony console list make
```

Il nous faudra également installer le système d'annotations, elles permettent de définir au-dessus de chaque action les routes (urls) de notre application.

```bash
symfony composer req annotations
```

Ouvrez le dossier config puis le fichier annotations.yam, voyez maintenant la configuration suivante :

```yaml
controllers:
    resource: ../../src/Controller/        <-- Annotation pour les routes dans les contrôleurs
    type: annotation

kernel:
    resource: ../../src/Kernel.php
    type: annotation
```

### Installation de Twig moteur de template


Installez maintenant le moteur de template Twig, tapez la ligne de commande suivante :

```bash
composer require twig
```

Notez que vous pouvez également désinstaller un composant. Par exemple si vous souhaitez désinstaller Twig vous taperez :

```bash
composer remove twig
```

Un dernier point pour Twig, vous pouvez vérifier la configuration de Twig pour votre application à l'aide de la commande suivante :

```bash
 php bin/console debug:config twig
 ```

 Enfin installez le composant Symfony suivant pour la gestion des assets statiques (comme les images par exemple)

 ```bash
 composer require symfony/asset
 ```

 Dans vos page HTML vous utiliserez alors la syntaxe suivante pour liées vos images, JS ou CSS :

 ```html
 {# the image lives at "public/images/logo.png" #}
<img src="{{ asset('images/logo.png') }}" alt="Symfony!"/>

{# the CSS file lives at "public/css/blog.css" #}
<link href="{{ asset('css/blog.css') }}" rel="stylesheet"/>

{# the JS file lives at "public/bundles/acme/js/loader.js" #}
<script src="{{ asset('bundles/acme/js/loader.js') }}"></script>
```

### Création du controller BarController

```bash
symfony console make:controller BarController
# php bin/console make:controller BarController 
 ```

 Symfony crée un contrôleur en utilisant les routes de type annotation. Ouvrez le dossier **src/Controller**. Vous trouverez le contrôleur **BarController**. 

```php
<?php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class BarController extends AbstractController
{
    #[Route('/bar', name: 'bar')]
    public function index(): Response
    {
        return $this->render('bar/index.html.twig', [
            'title' => 'The Bar',
        ]);
    }
}
```

Si votre serveur de test est lancé, testez la route suivante, vous devriez voir la page Twig que nous venons de créer.

Les templates se trouvent dans les dossiers suivants :

```txt
templates/
    bar/
        index.html.twig
    base.html.twig
```

Le fichier **base.html.twig** est un fichier que les vues composites comme **index.html.twig** hériteront. Cela permet de factoriser du code qui se répète dans toutes les pages.

**Fichier home.html.twig nous injectons la data title qui vient de l'action index de notre contrôleur :**

```html

{% extends('base.html.twig') %}

{% block body %}
<h1>{{ title }}</h1>
{% endblock %}

```

### Configuration de Webpack

Installez la dépendance suivante :

```bash
composer require symfony/webpack-encore-bundle
```

Puis avec npm installez les dépendances JS de notre projet

```bash
npm i
```

Allez dans le fichier base.html.twig et décommentez les parties de code suivantes :

```html
{% block stylesheets %}
    {{ encore_entry_link_tags('app') }}
{% endblock %}

{% block javascripts %}
    {{ encore_entry_script_tags('app') }}
{% endblock %}
```

Vous devriez avoir un dossier assets à la racine du projet. C'est dans ce dossier que sont placés les fichiers JS, SASS par exemple.

- Quelques commandes utiles

```bash
#compile les assets une fois
 npm run dev
#re-compile les assets automatiquement
 npm run watch
 # Créez le dossier build
 npm run build
 ```

 Installez le Bootstrap Twitter avec SASS 

 ```bash
npm install sass-loader@^10.0.0 sass --save-dev
 ```

 Dans le fichier webpack.config.js à la racine de votre projet modifiez les lignes suivantes :

 ```js
 // ...
Encore
  
    .addEntry('app', './assets/app.js')
    .addStyleEntry('styles', './assets/styles/app.scss') // Ajoutez cette ligne

    // ...

    // enables Sass/SCSS support
    .enableSassLoader()  // décommentez celle-ci

;

module.exports = Encore.getWebpackConfig();


 ```

### Exercice mise en page

Créez une structure sur deux colonnes à l'aide de la grille Bootstrap. Nous aimerions avoir la structure suivante, pour l'instant les blocks de la colonne de droite sont "statiques".

![bar 001](images/bar_01.jpg)

 ### Créez une nouvelle page "Mentions légales"

Dans le même contrôleur BarController créez une action (méthode) mentions() permetttant d'afficher une page **mentions.html.twig.**

L'action doit-être correctement décorée pour définir la route vers cette page, voyez le Wireframe ci-après :

```js
/**
 * @Route("/mention", name="mention")
 */
public function mention(){
    // ...
}
```

Wireframe

![bar 001](images/bar_02_mentions.png)

## Exercice Afficher la liste des bières

Installez la dépendance Symfony suivante pour effectuer des requêtes HTTP.

```text
composer require symfony/http-client
```

Soit la liste de bières suivantes de la côtes-d'Armor, nous allons l'afficher dans une nouvelle page **beers** que vous allez crée.

```php
{
    "beers" : [ 
        {"name" : "Rousse" , "degree" : 5.6},
        {"name" : "Stout" , "degree" : 6},
        {"name" : "Triple 'Spoum" , "degree" : 7},
        {"name" : "Malt fumé à la Tourbée" , "degree" : 8},
        {"name" : "Brune 'Spoum des Talus', bière millésimée à la mûre sauvage" , "degree" : 5},
        {"name" : "Philomenn blonde houblonnée à cru" , "degree" : 5.6},
        {"name" : "bière blonde houblonnée à cru" , "degree" : 7},
        {"name" : "HAC, bière blonde houblonnée à cru" , "degree" : 56.5}
    ]
}
```

Vous utiliserez l'adresse suivante pour récupérer ces données :

``text
https://raw.githubusercontent.com/Antoine07/hetic_symfony/main/Introduction/Data/beers.json
``


Mise en place


- Pour la vue Twig créez un dossier beers et une vue composite index.html.twig


- Importez l'interface suivante :

```php
use Symfony\Contracts\HttpClient\HttpClientInterface;
```

- Injection de la dépendance

```php
    public function __construct(HttpClientInterface $client)
    {
        $this->client = $client;
    }

```

- Implémentez la méthode privée suivante 

```php
<?php

private function beers_api(): Array
{
    $response = $this->client->request(
        'GET',
        'https://raw.githubusercontent.com/Antoine07/hetic_symfony/main/Introduction/Data/beers.json'
    );

    $statusCode = $response->getStatusCode();
    // $statusCode = 200
    $contentType = $response->getHeaders()['content-type'][0];
    // $contentType = 'application/json'
    $content = $response->getContent();
    // $content = '{"id":521583, "name":"symfony-docs", ...}'
    $content = $response->toArray();
    // $content = ['id' => 521583, 'name' => 'symfony-docs', ...]

    return $content ;
}
```

Vous utiliserez un for in Twig pour affichez. Pensez à utiliser la fonction Symfony dump pour voir le contenu de votre requête.

```php
dump($this->beers_api());
````

- Exemple de boucle Twig 

```html

    {% for beer in beers %}
        {{ user.name }}
    {% endfor %}

```

Wireframe 

![beers](images/bar_03_beers.jpg)


## Menu principal

Créez le menu nous l'importerons dans le fichier **base.html.twig**, faites un fichier menu.html.twig que nous placerons dans un dossier partials. Voici la syntaxe pour importer le menu dans base.html.twig et le code HTML :

- Import du menu

```html
{% include 'partials/menu.html.twig' %}
```

- Le menu HTML

```html
<nav class="navbar navbar-expand-lg navbar-dark bg-primary">
    <a class="navbar-brand" href="#">The Simple Bar/beers</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
            <li class="nav-item">
                <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="#">Features</a>
            </li>
        </ul>
    </div>
</nav>
```

## Exercice route et href dans le menu

Puis utilisez dans le template le helper (fonction Symfony) suivant : path, créez le lien vers la page home, puis le lien vers la page show_beer pour afficher une page spécifique (bières belges par exemple) :

Utilisez le helper path, créez un lien vers une page d'accueil, un lien vers la page affichant les bières et enfin un lien vers les mentions légales :

```html
<!-- dans le menu -->
<a href="{{ path('home') }}">Home</a>

{# show_beer nom de la route #}
<a href="{{ path('beers') }}">Beer Belgium</a>

```

## Exercice classe active sur un lien cliqué

Définissez une classe active sur les liens actifs. Utilisez la syntaxe suivante dans Twig pour récupérer la valeur de la route du lien suivi :

```html
{% set route_name = app.request.attributes.get('_route') %}
```

## Exercice images statiques

Trouvez des images de bières et placez ces images pour accompagner nos bières sur la page beers de l'application. Voyez la syntaxe suivante :

```html
<img src="{{ asset('assets/images/mon_image.png') }}" />
```

## Exercices database

Dépendances pour utiliser Doctrine

```bash
composer require symfony/orm-pack
composer require --dev symfony/maker-bundle
```

Une fois ces lignes exécutées vous devriez remarquer que dans votre fichier .env vous avez maintenant la possiblité de définir les paramètres vous permettant de vous connecter à votre base de données depuis l'application :

```text
# DATABASE_URL="mysql://db_user:db_password@127.0.0.1:3306/db_name?serverVersion=5.7"
```

Décommentez cette ligne pour créer la base de données db_bar dans MySQL. Faites attention au port de votre serveur de base de données.

```text
DATABASE_URL="mysql://root:root@127.0.0.1:8889/db_bafr?serverVersion=5.7"
```

Tapez maintenant la ligne de commande suivante, elle devrait créer la base de données dans MySQL automatiquement.

```bash
php bin/console doctrine:database:create
```