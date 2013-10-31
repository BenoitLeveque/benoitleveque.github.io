---
layout: post
title:  "Tri de données avec sylius/resource-bundle"
date:   2013-10-30 16:02:21
---

Dans cet article je vais vous montrer comment trier un tableau en utilisant simplement la configuration du resource-bundle de Sylius et un helper twig.

Pré-requis: pour ajouter un tri sur un tableau avec Sylius, vous aurez besoin d'une ``resource`` configuré correctement en suivant la [documentation du ``resource`` bundle][sylius_resource_docs].

Pour les personnes qui n'ont pas envie d'aller voir la documentation, j'ai repris dans le gist ci-dessous la configuration de ma ``resource``. On définit simplement que l'on a une ``resource app.book`` qui correspond à mon entité doctrine ``Book`` et que les différent template pourront être retrouvé dans ``AppBookBundle:Book``. Vous pouvez retrouver plus d'informations sur la configuration d'une ``resource``avec Sylius dans [documentation][sylius_resource_docs].

<script src="https://gist.github.com/BenoitLeveque/1319918d5a5be8d4249a.js"></script>

Dans le second gist (le fichier de routing), on retrouve une route classique, la partie qui nous intéresse ici se trouve en dessous du ``_sylius``. On décrit deux possibilitées de tri indépendante l'une de l'autre, la première en utilisant ``sortable`` permet d'indiquer a sylius que la liste de livres peut être triée par l'utilisateur en utilisant comme paramètre d'url ``sorting[column]=asc``. la seconde en utilisant ``sorting`` ici on peut indiquer une liste de colonnes de tri qui sera utilisé pour le tri par défaut.

L'utilisation de cette seconde possibilité est plutôt bien documentée dans la page [Sorting collection or paginator][sylius_sorting]. Cette option transmet simplement un tableau de couple colonne-ordre aux méthodes ``findBy`` et ``createPaginator`` que l'on peut retrouver dans la méthode  [indexAction du fichier ResourceController.php][resource_controller].

<script src="https://gist.github.com/BenoitLeveque/9a248b96a530531ff36f.js"></script>

Dans le dernier fichier, i.e. le fichier de template twig on utilise l'helper ``sylius_resource_sort`` qui ajoute simplement un lien dans l'en-tête du tableau, le code de cet helper peut être trouvé dans la class [SyliusResourceExtension.php][sylius_resource_extension].

<script src="https://gist.github.com/BenoitLeveque/e728455eb5884696e4ea.js"></script>

[sylius_resource_docs]: http://sylius.readthedocs.org/en/latest/bundles/SyliusResourceBundle/installation.html#registering-model-as-resource
[sylius_sorting]: http://sylius.readthedocs.org/en/latest/bundles/SyliusResourceBundle/index_resources.html#sorting-collection-or-paginator
[resource_controller]: https://github.com/Sylius/Sylius/blob/master/src/Sylius/Bundle/ResourceBundle/Controller/ResourceController.php#L68-L100
[sylius_resource_extension]: https://github.com/Sylius/Sylius/blob/master/src/Sylius/Bundle/ResourceBundle/Twig/SyliusResourceExtension.php