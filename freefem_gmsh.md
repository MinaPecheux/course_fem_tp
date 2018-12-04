+++
title = "Couplage avec GMSH"

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
  parent = "freefem"
  name = "Couplage avec GMSH"
  weight = 60

+++

$\newcommand{\xx}{\mathbf{x}}$
$\newcommand{\Pb}{\mathbb{P}}$
$\newcommand{\dn}{\partial\_{\mathbf{n}}}$

{{% alert warning %}}
Si vous utilisez GMSH en version 4 ou supérieur, vous devez ajouter ceci dans vos fichiers `.geo` :
```
Mesh.MshFileVersion = 2.2;
```
{{% /alert %}}

{{% alert warning %}}
FreeFem++ doit disposer du module GMSH ([voir page accueil]({{<relref "_index.md#freefem">}}))
{{% /alert %}}
## Lecture d'un maillage GMSH avec FreeFem++

{{% alert exercise %}}
Sous GMSH, construisez un carré unitaire avec des numéros physiques pour la surface et les 4 bords. Maillez ensuite le domaine et sauvegarder le fichier de maillage. Dans l'énoncé, ce fichier se nommera `carre.msh`, mais libre à vous de choisir un autre nom. Pour gagner du temps, voici un exemple de fichier
```
// carre.geo
Mesh.MshFileVersion = 2.2;
h = 0.1;
Point(1) = {0,0,0,h};
Point(2) = {1,0,0,h};
Point(3) = {1,1,0,h};
Point(4) = {0,1,0,h};
Line(1) = {1,2};
Line(2) = {2,3};
Line(3) = {3,4};
Line(4) = {4,1};
Line Loop(1) = {1,2,3,4};
Surface(1) = {1};
Physical Surface(10) = {1} // Surface du carré
Physical Line(1) = {1}; // Bord 1 ("bas")
Physical Line(2) = {2}; // Bord 2 ("droite")
Physical Line(3) = {3}; // Bord 3 ("haut")
Physical Line(4) = {4}; // Bord 4 ("gauche")
```
{{% /alert %}}

Ouvrons maintenant un fichier FreeFem++ dont l'extension par défaut est `.edp`. Nous allons lire le fichier de maillage et afficher le maillage à l'ai3e des commandes suivantes :

```cpp
  load "gmsh";
  mesh Th=gmshread("carre.msh");
  plot(Th, wait3= true, cmm = "Mon superbe maillage");
```

La quantité `Th` est de type `mesh`. Nous pouvons accéder à certaines données de ce maillage comme le nombre de triangles `Th.nt` ou de sommets `Th.nv` (*vertices*). D'autre part, `Th[k]` est le $k^{eme}$ triangle du maillage et l'on peut accéder à certaines valeurs le concernant :

  - `Th[k].label` : `Physical` label de l'élément
  - `Th[k][i]` : indice du $i^{eme}$ nœud du triangle $k$. Attention à la numérotation entre GMSH et FreeFem++ qui peut différer.
  

{{% alert exercise %}}
Lancez le code ci-dessus dans FreeFem++, et puis :

- Affichez le nombre de triangles
- Affichez le nombre de sommets    
- Pour chaque triangle, affichez les indices des points ainsi que le label du triangle. Vérifiez qu'il s'agit bien du label que vous avez choisi dans le fichier .geo.    
{{% /alert %}}

{{% alert tips %}}
Nous rappelons qu'[une page du tutoriel GMSH]({{<ref "/course/gmsh/basics_meshformatv2.md">}}) est dédiée aux labels `Physical`.
{{% /alert %}}

## Exercice

<!-- FreeFem++ fluide-->
