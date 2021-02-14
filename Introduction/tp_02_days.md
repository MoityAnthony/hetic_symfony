
# TP

Lisez attentivement ce TP tout est détaillé pour réaliser le projet The Simple Bar.

La partie 1 & 2 est obligatoire. La partie 3 est facultative.

## Partie 1

Vous avez au moins 20 bières dans la table beer. Vous pouvez utiliser le dépôt suivant pour vous aidez :

[Soucre](https://github.com/Antoine07/bar)

### Création des countries AppFixtures

Dans AppFixtures créez les pays suivants, voir la liste ci-dessous, puis en utilisant setCountry de Beer associez un pays à une bière :

```php
$countries = ['belgium', 'french', 'English', 'germany'];
```

### Catégories et bières AppFixtures

Attention, si ce n'est pas déjà fait vous devez modifier la structure de la table category avant d'ajouter les catégories aux bières.

- Modifiez l'entité Category et ajoutez un champ "term", par défaut ce champ est "normal" ajoutez dans l'entité Category le code suivant au-dessus du champ term :

```php
/**
* @ORM\Column(type="string", length=100, options={"default": "normal"})
*/
private $term ;
```

Ainsi que dans le constructeur de l'entité, ainsi à chaque fois que vous créez une catégorie par défaut la valeur "normal" sera attribuée au champ term :

```php
 public function __construct()
    {
        $this->beers = new ArrayCollection();
        $this->setTerm('normal'); // valeur par défaut 
    }
```

Lorsque vous modifiez une entité pensez à vérifier sa cohérence en base de données pour créez une nouvelle migration qui modifiera la structure de la table :

1. Création de la migration et vérification de la cohérence de la base de données 

```bash
php bin/console doctrine:migrations:diff
```

2. Créez les modifications en base de données

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

Créez une méthode (repository) qui affiche pour l'instant les 3 dernières bières de votre table beer. Vous afficherez son nom et sa description. Chaque nom de bière sera cliquable. Le lien conduira à la page de présentation de la bière. Cette page affichera le nom de la bière, sa description, son prix, son degré, son pays et ses deux catégories normale (Blonde, Brune, ...) et spéciale (Houblon, bio, ...).

Voyez les Wireframes de cette section ci-après pour vous aidez à réaliser ces pages.

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

La méthode du repository sera à utiliser comme suit dans vos contrôleurs ou Fixtures (données d'exemple)

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

Gestion des routes dans les pages Twig avec le helper path

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

Vous devez également créer une méthode findByTerm pour récupérer la catégorie spéciale dans la page d'une bière. Créez cette méthode dans le repository BeerRepository

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

Nous allons utilisez le helper **render** de Twig pour afficher notre menu principal. Il remplacera le code que nous avons écrit dans le fichier main_menu.html.twig. Notez la présence du paramètre d'URL **routeName** que nous avons défini dans le code ci-après. Il permet de récupérer le nom de la route pour définir la classe active du lien actif.

Chacune des catégories affichera ses propres bières, même disposition que la page d'accueil.
Voyez les Wireframes ci-après, retirez le lien beers si ce dernier existe dans le menu précédent.

1. Créez une méthode mainMenu dans BarController comme suit, et récupérez les noms des catégories normales pour les afficher dans le menu principal.

```php
/**
* @Route("/menu", name="menu")
*/
public function mainMenu(Request $request): Response{
    dump($request->get('routeName'));

    return new Response('TODO MENU');
}

```

2. Appelez cette méthode dans le template Twig  main_menu.html.twig à la place du précédent menu.

```php
{{render(controller('App\\Controller\\BarController:mainMenu',
  {
    'routeName' : app.request.attributes.get('_route')
}))}}

```

- Menu principale avec les catégories normales cliquables

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

Ajoutez maintenant le lien Statistic dans le menu principal de l'application et affichez dans la colonne de droite le nombre de client et dans la colonne de gauche le nom des clients et le nombre de bière acheté :

```text
------------------------------------------
The simple Bar/Beer Home blonde brune blanche Mentions [Statistic]
------------------------------------------
Nombre de clients : 10 |   Tony age 56 nombre de bière acheté : 17
                       |   Alice age 45 nombre de bière acheté : 11
```