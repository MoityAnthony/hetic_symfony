
# TP

Lisez attentivement ce TP tout est détaillé pour réaliser le projet The Simple Bar. Vous avez deux jours pour le réaliser : lundi & mercredi.

Les parties 1 & 2 sont obligatoires. La partie 3 est facultative mais, si vous avez terminé avant essayer de la faire.

## Partie 1

Rappel : vous avez au moins 20 bières dans la table beer. Vous pouvez utiliser le dépôt suivant pour vous aidez (corrections des exercices précédents) :

[Corrections](https://github.com/Antoine07/bar)

### Création des countries, AppFixtures

Dans AppFixtures créez les pays suivants, voir la liste ci-dessous, puis en utilisant setCountry de Beer associez un pays au moins à une bière :

```php
$countries = ['belgium', 'french', 'English', 'germany'];
```

### Catégories et bières AppFixtures

Avant d'ajouter les catégories vous devez modifier sa structure.

- Modifiez l'entité Category et ajoutez un champ "term" de type string 100 caractères non nul, par défaut ce champ est "normal".

Ajoutez dans l'entité Category le code suivant au-dessus du champ **term** :

```php
/**
* @ORM\Column(type="string", length=100, options={"default": "normal"})
*/
private $term ;
```

Ainsi que dans le constructeur de l'entité ajoutez le setter suivant. A chaque fois que vous créez une nouvelle catégorie la valeur "normal" sera alors attribuée par défaut au champ term :

```php
 public function __construct()
    {
        $this->beers = new ArrayCollection();
        $this->setTerm('normal'); // valeur par défaut 
    }
```

Migration et modification de la structure de la table.

1. Création de la migration en fonction de la cohérence des donnes dans la/les table(s) 

```bash
php bin/console doctrine:migrations:diff
```

2. Créez effectivement les modifications en base de données

```bash
php bin/console doctrine:migrations:migrate
```

- Dans AppFixture explicitez les noms des catégories

Vous allez maintenant créez les catégories suivantes, chaque bière aura une catégorie "normal" et au moins une catégorie spéciale, renseignez en fonction du nom de la catégorie le champ term dans la table category :

```php
// catégories normals
$categoriesNormals = ['blonde', 'brune', 'blanche'];

// catégories specials
$categoriesSpecials = ['houblon', 'rose', 'menthe', 'grenadine', 'réglisse', 'marron', 'whisky', 'bio'] ;

```

### Page d'accueil & page d'une bière

Créez une méthode dans le repository **BeerRepository** qui vous permettra d'afficher les 3 dernières bières de votre table beer sur la page d'accueil, vous afficherez son nom et sa description. Chaque nom de bière sera cliquable. Le lien conduira à la page de présentation de la bière. Elle affichera le nom de la bière, sa description, son prix, son degré, son pays et ses deux catégories normale (Blonde, Brune, ...) et spéciale(s) (Houblon, bio, ...).

Voyez les Wireframes ci-après pour vous aidez à réaliser ces deux pages.

- Exemple de la création d'une méthode dans le repository :

```php

public function findByExampleField($value)
{
    return $this->createQueryBuilder('b')
        ->andWhere('b.exampleField = :val')
        ->setParameter('val', $value)
        ->orderBy('b.id', 'ASC')
        ->setMaxResults(10)
        ->getQuery()
        ->getResult()
    ;
}
```

La méthode du repository sera à utiliser comme suit dans vos contrôleurs ou Fixtures :

- Appelle de la méthode dans un Controller

```php
$repoBeer = $this->getDoctrine()->getRepository(Beer::class);
$repoBeer->findByExampleField(1);
```

- Appelle de la méthode dans l'AppFixture

```php
$repoBeer = $manager->getRepository(Beer::class);
$repoBeer->findByExampleField(1);
```

Gestion des routes dans les pages Twig avec le helper **path**

```php
{#
Twig
Votre url aura la forme suivante : http://localhost:8000/beer/1
#}
<a href="{{ path('beer', { 'id' : beer.id }) }}">
    {{ beer.name }}
</a>
```

Dans le contrôleur pour récupérer l'identifiant dans le path vous écrirez :

```php
/**
* @Route("/beer/{id}", name="beer")
*/
public function show(int $id): Response
{
    // TODO
}
```

Utilisez le repository suivant défini par défaut dans Doctrine pour récupérer une bière en fonction de son identifiant dans votre contrôleur BarController :

```php
$repoBeer = $this->getDoctrine()->getRepository(Beer::class);
$repoBeer->find(1);
```

Vous devez également créer une méthode **findByTerm** pour récupérer la catégorie spéciale dans la page d'une bière. Créez cette méthode dans le repository BeerRepository

```php
$repoBeer = $this->getDoctrine()->getRepository(Beer::class);
$repoBeer ->findByTerm('special');
```

#### Wireframes

Wireframe page d'accueil :

```text
------------------------------------------
The simple Bar/Beer [Home] Beers Mentions
------------------------------------------
Sidebar |  
        |   Beer : [beer super]    <-- cliquable
        |   description :lorem ...
        |
```

Wireframe page d'une bière :

```text
------------------------------------------
The simple Bar/Beer Home Beers Mentions
------------------------------------------
Sidebar |  
        |   Beer : [beer super]    <-- cliquable
        |   description :lorem ...
        |   price : 6.5
        |   degree : 5
        |   Country : [Belgium]  <-- cliquable
        |   Category : 
                - Blonde
                - Houblon
```

## Partie 2

### Main menu

Nous allons utilisez le helper **render** de Twig pour afficher notre menu principal. Il remplacera le code que nous avons écrit dans le fichier main_menu.html.twig. Notez la présence du paramètre d'URL **routeName** que nous avons défini dans le code ci-après, il permet de récupérer le nom de la route pour définir la classe active du lien actif.

Chacune des catégories affichera ses propres bières, même disposition que la page d'accueil.

Voyez les Wireframes ci-après, pensez à retirer le lien beers si ce dernier existe encore dans le menu.

1. Créez une méthode mainMenu dans BarController comme suit, et récupérez les noms des catégories normales pour les afficher dans le menu principal.

```php
/**
* @Route("/menu", name="menu")
*/
public function mainMenu(string $category_id, string $routeName): Response{
    

    return new Response('TODO MENU TWIG');
}

```

2. Appelez cette méthode dans le template Twig main_menu.html.twig.

```php
{{render(controller('App\\Controller\\BarController:mainMenu',
  {
    'routeName' : app.request.attributes.get('_route'),
    'category_id' : app.request.attributes.get('id') ?? ''
}))}}

```

- Menu principale avec les catégories blonde, brune, et blanche normales cliquables

```text
------------------------------------------
The simple Bar/Beer [Home] blonde brune blanche Mentions
------------------------------------------
Sidebar |  
        |   Beer : [beer super]    <-- cliquable
        |   description :lorem ...
        |
```

- Page d'une catégorie

```text
------------------------------------------
The simple Bar/Beer Home [blonde] brune blanche Mentions
------------------------------------------
Sidebar |   Bières blodes
        |   Beer : [beer super]    <-- cliquable
        |   description :lorem ...
        |
```

## Partie 3

### Nouvelles entités

Créez les deux nouvelles entités suivantes : Statistic & Client.

- Entité Client 

Des clients achètent des bières et une bière peut être achetée par plusieurs clients. Complétez le schéma sur papier que vous avez déjà commencer en cours pour explicitez les relations.

- Entité Statistic 

Chaque client attribuera un score aux bières qu'ils ont acheté, cette information sera reportée dans la table statistic.

Créez des Fixtures dans AppFixture pour ces deux entités. Vous pouvez créer une deuxième classe de Fixture comme suit. Créez au moins 10 clients.

1. Création de la classe StatisticFixtures

```bash
php bin/console make:fixtures StatisticFixtures
```

2. Dans la classe StatisticFixtures vous définirez l'ordre dans lequel seront exécuté les Fixtures, ici AppFixtures puis StatisticFixtures. La méthode getDependencies définie AppFixtures comme dépendance.

```php
namespace App\DataFixtures;

use Doctrine\Bundle\FixturesBundle\Fixture;
use Doctrine\Persistence\ObjectManager;
use Doctrine\Common\DataFixtures\DependentFixtureInterface;

// Vos entités
use App\Entity\Client;
use App\Entity\Statistic;

class Statistic extends Fixture implements DependentFixtureInterface
{ 
    public function load(ObjectManager $manager)
    {
       // TODO FIXTURES
    }

    public function getDependencies()
    {
        return array(
            AppFixtures::class,
        );
    }
}
```

Pour vous aidez dans la réalisation de ces nouvelles fonctionnalités dans l'application nous vous proposons le schéma UML partiel suivant :

![database schema](images/simplebar_03.png)

### Intérêt et âge

Ajoutez un champ age à l'entité Client. Mettez à jour la base de données. Et ajoutez des donnes d'exemple dans StatisticFixtures.

### Menu principal

Ajoutez maintenant le lien Statistic dans le menu principal de l'application et affichez dans la colonne de droite le nombre de client et dans la colonne de gauche le nom des clients et le nombre de bière(s) achetée(s) :

```text
------------------------------------------
The simple Bar/Beer Home blonde brune blanche Mentions [Statistic]
------------------------------------------
Nombre de clients : 10 |   Tony, age 56 ans, nombre de bière acheté : 17
                       |   Alice, age 45 ans, nombre de bière acheté : 11
```

Ajoutez d'autre statistiques de votre choix si vous avez terminé.


## Partie 4

Voici un code Doctrine à écrire dans le Repository CategoryRepository, qu'en pensez vous ? Décrivez son utilité dans l'application si on devait le mettre en place.

```php
public function findCatSpecial(int $id)
    {
        return $this->createQueryBuilder('c')
            ->join('c.beers', 'b') // raisonner en terme de relation
            ->where('b.id = :id')
            ->setParameter('id', $id)
            ->andWhere('c.term = :term')
            ->setParameter('term', 'special')
            ->getQuery()
            ->getResult();
    }
```