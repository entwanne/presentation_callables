# Devenir incollable sur les callables !

Conférence présentée à la PyConFr 2023.

> Il n'y a pas que les fonctions qui sont appelables en Python : on peut aussi appeler des types (`int()`), des méthodes (`foo.bar()`) et plus généralement toutes sortes d'objets bien particuliers.  
> Quels sont-ils ? Comment fonctionnent-ils ? Et plus généralement que se passe-t-il lors d'un appel ?  
> Ce sont à ces questions qu'entend répondre la présentation.  
> Seront aussi abordées les notions d'arguments et de paramètres, les différents types d'arguments (positionnels, nommés), les signatures de fonctions et les décorateurs.

L'exposé est basé en partie sur les cotenus suivants :
* https://zestedesavoir.com/tutoriels/2514/un-zeste-de-python/7-perfectionnement/4-fonctions/
* https://zestedesavoir.com/tutoriels/954/notions-de-python-avancees/2-functions/1-callables/
* https://zestedesavoir.com/tutoriels/954/notions-de-python-avancees/2-functions/2-annotations-signatures/
* https://zestedesavoir.com/tutoriels/954/notions-de-python-avancees/2-functions/3-decorators/

Le support est disponible à l'adresse <https://entwanne.github.io/presentation_callables/>.

## Support de présentation

Le support de présentation utilise [`lucina`](https://pypi.org/project/lucina/), un outil permettant de générer un notebook Jupyter à partir de fichiers Markdown.
C'est ensuite le plugin rise qui est utilisé pour rendre une présentation Reveal.js à partir de ce notebook, afin d'avoir une présentation interactive.

L'environnement de travail peut alors être installé à l'aide de la commande shell `pip install -r requirements`.

`make run` permet ensuite de générer et démarrer la présentation.

Différents exports (notebook, reveal, PDF) sont présentés sur la branche [exports](https://github.com/entwanne/presentation_callables/tree/exports).
