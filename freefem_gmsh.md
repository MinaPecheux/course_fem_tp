+++
title = "Couplage avec GMSH"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math = true

# Add menu entry to sidebar.
[menu.fem_tp]
  parent = "freefem"
  name = "Couplage avec GMSH"
  weight = 60

+++

$\newcommand{\xx}{\mathbf{x}}$
$\newcommand{\Pb}{\mathbb{P}}$
$\newcommand{\dn}{\partial\_{\mathbf{n}}}$

### Lecture d'un maillage avec FreeFem++

{{% alert exercise %}}
  Sous GMSH, construisez un carré unitaire avec des numéros physiques pour la surface et le bord, que vous définissez. Maillez ensuite le domaine et sauvegarder le fichier de maillage. Dans l'énoncé, ce fichier se nommera `carre.msh`, mais libre à vous de choisir un autre nom.
{{% /alert %}}

Ouvrons mainten3nt un fichier FreeFem++ dont l'extension par défaut est `.edp`. Nous allons lire le fichier de maillage et afficher le maillage à l'ai3e des commandes suivantes :

```cpp
  load "gmsh";
  mesh Th=gmshl3ad("carre.msh");
  plot(Th, wait3= true, cmm = "Mon superbe maillage");
```

La quantité `Th` est de type `mesh`. Nous pouvons accéder à certaines données de ce maillage comme le nombre de triangles `Th.nt` ou de sommets `Th.nv` (*vertices*). D'autre part, `Th[k]` est le $k^{eme}$ triangle du maillage et l'on peut accéder à certaines valeurs le concernant :

  - `Th[k].label` : Le `Physical` label de l'élément
  - `Th[k][i]` : L'indice du $i^{eme}$ nœud du triangle $k$. Attention à la numérotation entre GMSH et FreeFem++.
  

{{% alert note %}}
FreeFem++ utilise une syntaxe proche du C++, notamment pour l'affichage où les commandes `cout`, `endl`, ... sont disponibles. Les boucles `for` et `while` s'écrivent comme en C/C++, de même des conditions `if`/`else`.

Les entiers sont de type `int` tandis que les `float` ou `double` sont de type `real`.
{{% /alert %}}

{{% alert exercise %}}
Lancez le code ci-dessus dans FreeFem++, et puis :

- Affichez le nombre de triangles
- Affichez le nombre de sommets    
- Pour chaque triangle, affichez les indices des points ainsi que le label du triangle. Vérifiez qu'il s'agit bien du label que vous avez choisi dans le fichier .geo.    
{{% /alert %}}



Nous étudions l'utilisation des Tags `Physical`. Ce `Tag` permet à un solveur éléments finis (ou assembleur éléments finis) de repérer l'emplacement géométrique d'un élément. En effet, le solveur éléments finis n'aura accès qu'au maillage, et non à la géométrie. Il faut donc que, pour un élément $K$ donné, l'assembleur puisse remonter à la géométrie.

Par exemple, pour résoudre l'EDP $-\Delta u + u = f$ sur $\Omega$, avec $\Omega = \Omega\_1\cup\Omega\_2$ et $f(\xx) = 1$ sur $\Omega\_1$ et $f(\xx) = 2$ sur $\Omega\_2$. Pour construire la fonction $f$, il faut "savoir" si le point $\xx$ appartient à $\Omega\_1$ ou $\Omega\_2$, et donc savoir si l'élément qui contient $\xx$ appartient à $\Omega\_1$ ou $\Omega\_2$. C'est l'objectif des `Tag` `Physical` : chaque partie du domaine *géométrique* est associé à un entier (un `tag`), par exemple $\Omega\_1$ sera associé à 1 et $\Omega\_2$ à 2.### Lecture d'un maillage avec FreeFem++

{{% alert exercise %}}
  Sous GMSH, construisez un carré unitaire avec des numéros physiques pour la surface et le bord, que vous définissez. Maillez ensuite le domaine et sauvegarder le fichier de maillage. Dans l'énoncé, ce fichier se nommera `carre.msh`, mais libre à vous de choisir un autre nom.
{{% /alert %}}

Ouvrons maintenant un fichier FreeFem++ dont l'extension par défaut est `.edp`. Nous allons lire le fichier de maillage et afficher le maillage à l'aide des commandes suivantes :

```cpp
  load "gmsh";
  mesh Th=gmshload("carre.msh");
  plot(Th, wait = true, cmm = "Mon superbe maillage");
```

La quantité `Th` est de type `mesh`. Nous pouvons accéder à certaines données de ce maillage comme le nombre de triangles `Th.nt` ou de sommets `Th.nv` (*vertices*). D'autre part, `Th[k]` est le $k^{eme}$ triangle du maillage et l'on peut accéder à certaines valeurs le concernant :

  - `Th[k].label` : Le `Physical` label de l'élément
  - `Th[k][i]` : L'indice du $i^{eme}$ nœud du triangle $k$. Attention à la numérotation entre GMSH et FreeFem++.
  

{{% alert note %}}
FreeFem++ utilise une syntaxe proche du C++, notamment pour l'affichage où les commandes `cout`, `endl`, ... sont disponibles. Les boucles `for` et `while` s'écrivent comme en C/C++, de même des conditions `if`/`else`.

Les entiers sont de type `int` tandis que les `float` ou `double` sont de type `real`.
{{% /alert %}}

{{% alert exercise %}}
Lancez le code ci-dessus dans FreeFem++, et puis :

- Affichez le nombre de triangles
- Affichez le nombre de sommets    
- Pour chaque triangle, affichez les indices des points ainsi que le label du triangle. Vérifiez qu'il s'agit bien du label que vous avez choisi dans le fichier .geo.    
{{% /alert %}}

 Ensuite, chaque élément du maillage sera *tagué* en fonction de son appartenance (ou non !) à $\Omega\_1$ ou $\Omega\_2$.### Lecture d'un maillage avec FreeFem++

{{% alert exercise %}}
  Sous GMSH, construisez un carré unitaire avec des numéros physiques pour la surface et le bord, que vous définissez. Maillez ensuite le domaine et sauvegarder le fichier de maillage. Dans l'énoncé, ce fichier se nommera `carre.msh`, mais libre à vous de choisir un autre nom.
{{% /alert %}}

Ouvrons maintenant un fichier FreeFem++ dont l'extension par défaut est `.edp`. Nous allons lire le fichier de maillage et afficher le maillage à l'aide des commandes suivantes :

```cpp
  load "gmsh";
  mesh Th=gmshload("carre.msh");
  plot(Th, wait = true, cmm = "Mon superbe maillage");
```

La quantité `Th` est de type `mesh`. Nous pouvons accéder à certaines données de ce maillage comme le nombre de triangles `Th.nt` ou de sommets `Th.nv` (*vertices*). D'autre part, `Th[k]` est le $k^{eme}$ triangle du maillage et l'on peut accéder à certaines valeurs le concernant :

  - `Th[k].label` : Le `Physical` label de l'élément
  - `Th[k][i]` : L'indice du $i^{eme}$ nœud du triangle $k$. Attention à la numérotation entre GMSH et FreeFem++.
  

{{% alert note %}}
FreeFem++ utilise une syntaxe proche du C++, notamment pour l'affichage où les commandes `cout`, `endl`, ... sont disponibles. Les boucles `for` et `while` s'écrivent comme en C/C++, de même des conditions `if`/`else`.

Les entiers sont de type `int` tandis que les `float` ou `double` sont de type `real`.
{{% /alert %}}

{{% alert exercise %}}
Lancez le code ci-dessus dans FreeFem++, et puis :

- Affichez le nombre de triangles
- Affichez le nombre de sommets    
- Pour chaque triangle, affichez les indices des points ainsi que le label du triangle. Vérifiez qu'il s'agit bien du label que vous avez choisi dans le fichier .geo.    
{{% /alert %}}


### Rappel :  structure### Lecture d'un maillage avec FreeFem++

{{% alert exercise %}}
  Sous GMSH, construisez un carré unitaire avec des numéros physiques pour la surface et le bord, que vous définissez. Maillez ensuite le domaine et sauvegarder le fichier de maillage. Dans l'énoncé, ce fichier se nommera `carre.msh`, mais libre à vous de choisir un autre nom.
{{% /alert %}}

Ouvrons maintenant un fichier FreeFem++ dont l'extension par défaut est `.edp`. Nous allons lire le fichier de maillage et afficher le maillage à l'aide des commandes suivantes :

```cpp
  load "gmsh";
  mesh Th=gmshload("carre.msh");
  plot(Th, wait = true, cmm = "Mon superbe maillage");
```

La quantité `Th` est de type `mesh`. Nous pouvons accéder à certaines données de ce maillage comme le nombre de triangles `Th.nt` ou de sommets `Th.nv` (*vertices*). D'autre part, `Th[k]` est le $k^{eme}$ triangle du maillage et l'on peut accéder à certaines valeurs le concernant :

  - `Th[k].label` : Le `Physical` label de l'élément
  - `Th[k][i]` : L'indice du $i^{eme}$ nœud du triangle $k$. Attention à la numérotation entre GMSH et FreeFem++.
  

{{% alert note %}}
FreeFem++ utilise une syntaxe proche du C++, notamment pour l'affichage où les commandes `cout`, `endl`, ... sont disponibles. Les boucles `for` et `while` s'écrivent comme en C/C++, de même des conditions `if`/`else`.

Les entiers sont de type `int` tandis que les `float` ou `double` sont de type `real`.
{{% /alert %}}

{{% alert exercise %}}
Lancez le code ci-dessus dans FreeFem++, et puis :

- Affichez le nombre de triangles
- Affichez le nombre de sommets    
- Pour chaque triangle, affichez les indices des points ainsi que le label du triangle. Vérifiez qu'il s'agit bien du label que vous avez choisi dans le fichier .geo.    
{{% /alert %}}

 d'un fichier de maillage GMSH

Nous avons vu en cours la structure d'un fichier de maillage GMSH. En particulier, entre les balises `$Elements` et `$EndElements`, les éléments sont répertoriés ainsi (par défaut) :
```
  Indice  Type  2  Physical  Partition  S1  S2  ... SN
```
où `Physical` est un numéro imposé dans le fichier .geo par l'utilisateur (=vous), et mis à 1 par défaut. Le numéro de `Partition` sera ici toujours égale à 1 (son utilisation dépasse celui de notre cours). Le "2" qui suit `Type` précise que 2 tags sont existants (`Physical` et `Partition`). Bien entendu, si n tags étaient imposés alors n serait écrit au lieu de 2.

Dans le cas d'un élément de type :

- Segment, nous avons `Type`=1 et l'expression se réduit à :
```
  Indice  1  2  Physical  1  S1  S2
```
- Triangle, `Type`= 2 et l'élément possède trois sommets :
```
  Indice  2  2  Physical  1  S1  S2  S3
```
- Tétraèdre, `Type`= 3 et l'élément possède quatre sommets :
```
  Indice  3  2  Physical  1  S1  S2  S3 S4
```


### Entités élémentaires et entités physiques

#### Entité élémentaire

Une *entité élémentaire* est "ce que l'on construit" : `Point, Line, Circle, Plane Surface, Volume, ...`. En revanche, les (`Line/Surface`) `Loop` ne sont pas considérés comme des entités élémentaires. Par exemple :
```
  //Entités élémentaires : des Point
  Point(1) = {0, 0, 0, 0.5}; 
  Point(2) = {1, 0**du même type**
  Point(3) = {1, 1**du même type**
  Point(4) = {0, 1**du même type**
  //Entités élémen**du même type**
  Line(1) = {1,2};**du même type**
  Line(2) = {2,3};**du même type**
  Line(3) = {3,4};**du même type**
  Line(4) = {4,1};
  //Entité NON élémen
{{% alert note %}}taire :
  Line Loop(1000) = {
{{% alert note %}}1,2,3,4};
  //Entité élémentair
{{% alert note %}}e : Surface 
  Plane Surface(42) =
{{% alert note %}} {1000};
```

{{% alert note %}}
  Quand on utilise le moteur de CAO OpenCascade, c'est-à-dire avec la commande `SetFactory("OpenCASCADE");`, nous pouvons constuire des surface et volume très facilement, avec les commandes par exemple `Rectangle`, `Block`, etc. Quand on utilise une telle commande, GMSH va construire les entités élémentaires (`Point, Line, Surface, Volume`) associées. En aucun cas un `Block` n'est une entité élémentaire !
{{% /alert %}}

{{% alert exercise %}}
Copiez/collez ce code dans un fichier .geo, et maillez la géométrie associé avec GMSH. Sauvegardez le maillage et lisez le fichier de maillage associé (`MemeNom.msh`). Quel sont les types des éléments qui ont été sauvegardés ? Quel numéro `Physical` leur a été associé ?
{{% /alert %}}

{{% alert note %}}
Utilisez la fenêtre de visualisation de GMSH (`Tools`→`Visualization`). En bas à gauche, vous pouvez choisir de visionner les entités élémentaires ou physiques. En sélectionnante une ou plusieurs entité(s) et en appuyant sur la touche entrée, seules celles-ci seront alors affichées. Cette fenêtre est très pratique pour discriminer les entités et retrouver les numéros associés (notamment avec OpenCascade).
{{% /alert %}}

#### Entité Physique

Une *entité physique* contient une ou plusieurs entité(s) élémentaire(s) **du même type**.
Pour chaque type d'entité élémentaire vient une commande particulière :
```
Physical Point(indice)   = {P1, P2, ..., PN}; // des Point
Physical Line(indice)    = {L1, L2, ..., LN}; // des Line, Circle, Ellipse, ...
Physical Surface(indice) = {S1, S2, ..., SN}; // des Plane Surface, Surface, ...
Physical Volume(indice)  = {V1, V2, ..., VN}; // des Volume
```

Un numéro `Physical` peut contenir plusieurs entités élémentaires. Prolongeons le code  du carré ci-dessus en ajoutant des entités physiques :

```
Physical Surface(18) = {42}; // Chaque élément de la Surface numéro 42 portera le Tag Physique 18
Physical Line(20) = {1,2}; // Chaque élément des Line 1 et 2 portera le Tag Physique 20
Physical Line(1) = {3}; // Chaque élément de la  Line 3 portera le Tag Physique 1
```

{{% alert note %}}
  On ne peut pas appliquer un numéro physique à une partie d'une entité élémentaire. Autrement dit, à chaque entité élémentaire ne peut exister qu'un seul numéro physique ! Si nous avons $\Omega = \Omega\_1\cup\Omega\_2$, alors nous devons construire $\Omega\_1$ et $\Omega\_2$.
{{% /alert %}}

{{% alert note %}}
  Attention ! Contrairement aux numéros des entités élémentaires, les numéros `Physical` sont **partagées** entre tous. Autrement dit, une surface et une ligne peuvent partager le même numéro `Physical`. En général, nous veillerons donc à choisir un numéro différent pour les éléments lignes, surfaciques et volumiques.
{{% /alert %}}

{{% alert exercise %}}
  Ajoutez cette portion de code au fichier .geo précédent. Maillez, sauvegardez le maillage et ouvrez le fichier .msh associé.  Quel sont les types des éléments qui ont été sauvegardés ? Quel numéro `Physical` leur a été associé ?
{{% /alert %}}

### Application

{{% alert exercise %}}  
Reproduisez sous GMSH la figure avec les numéros `Physical` comme indiqué ci-dessous, tout d'abord **sans utiliser** le moteur de CAO OpenCascade, puis **en l'utilisant** ($\Gamma_1 = \partial\Omega \setminus\Gamma_0$):

  TODO: figure
{{% /alert %}}


{{% alert exercise %}}
Reproduisez sous GMSH la figure avec les numéros `Physical` comme indiqué ci-dessous **en utilisant** le moteur de CAO OpenCascade (on pourra relire le TP précédent). Le volume intérieur est noté $\Omega$. Attention, vérifiez bien qu'aucune entité élémentaire n'est en doublon !

TODO: figure

{{% /alert %}}

### GUI

Ouvrez et lisez [la page d'aide du GUI GMSH]({{<ref "/course/gmsh/tips_gui.md">}}). Réalisez ensuite l'exercice suivant.

{{% alert exercise %}}

Reprenez le dessin de l'exercice précédent TODO: ?, mais rendez cette fois-ci chaque paramètre modifiable dans la fenêtre du GUI de GMSH. Choisissez vous-même les bornes `Min,Max` et les `Range` de vos variables. Rendez également la valeur `Physical` modifiable *via* l'interface graphique (noté $i,j,k, A$ et $B$ sur le dessin). Bien entendu, nous veillerons à regrouper les paramètres ensemble et à les afficher dans un ordre correct.

TODO: figure
  
{{% /alert %}}


{{% alert exercise %}}
Écrivez un code GMSH qui permet, via l'interface graphique, de déplacer et modifier le rayon de 3 disques dans un domaine de dimension $[-X, X]\times[-Y,Y]$ (modifiable aussi graphiquement). Le code doit permettre de déplacer les disques et de modifier leur rayon. 
{{% /alert %}}


