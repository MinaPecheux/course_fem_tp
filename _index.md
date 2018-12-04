+++
title = "MEF : TPs"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math = true

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_fem_tp"
  issue = "https://github.com/Bertbk/course_fem_tp/issues"
  prose = "https://prose.io/#Bertbk/course_fem_tp/edit/master/"


# Add menu entry to sidebar.
[menu.fem_tp]
  name = "MEF : TPs"
  weight = 1

+++

Les TP associés au cours Maillage et Éléments Finis sont divisés en deux parties. Une première partie consiste à utiliser des logiciels open-source afin de résoudre un problème complet à l'aide de la méthode des éléments finis : de la création de la géométrie à la visualisation de la solution. La deuxième partie vous propose d'implémenter vous même un (petit) assembleur éléments finis en Python.

## Partie 1 : vive l'open-source

Pour cette partie nous utilisons deux logiciels :

1. [GMSH](https://gmsh.info), dont un [tutoriel est disponible sur le site]({{<ref "/course/gmsh/_index.md">}})
2. [FreeFem++](https://freefem.org) : [Accès au TPs]({{< relref "menu_freefem.md">}})


## Partie 2 : l'assemblage en Python

En cours de construction...

## Sur les machines de Polytech...

### GMSH
Installé et exécutable depuis le terminal
```
gmsh
```

### FreeFem++

Pour l'exécuter :
```
/opt/freefem/bin/FreeFem++
```
Vous pouvez ajouter un alias par exemple
```
alias ff=/opt/freefem/bin/FreeFem++
```

Pour lire un fichier GMSH dans FreeFem++, nous utilisons le module `load("gmsh)`. Pour cela, vous devez ajouter un lien vers les modules de FreeFem++
```
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/freefem/lib/ff++/3.61/lib
```
ou copiez le fichier suivant dans votre dossier courant :
```
/opt/freefem/lib/ff++/3.61/lib/gmsh.so
```

Vous pouvez bien entendu automatiser tout cela dans votre `.bash_profile` à la racine de votre Home :
```
#.bash_profile
alias ff=/opt/freefem/bin/FreeFem++
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/freefem/lib/ff++/3.61/lib
```