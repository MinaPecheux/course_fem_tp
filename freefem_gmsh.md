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
Physical Surface(10) = {1}; // Surface du carré
Physical Line(1) = {1}; // Bord 1 ("bas")
Physical Line(2) = {2}; // Bord 2 ("droite")
Physical Line(3) = {3}; // Bord 3 ("haut")
Physical Line(4) = {4}; // Bord 4 ("gauche")
```
{{% /alert %}}

Ouvrons maintenant un fichier FreeFem++ dont l'extension par défaut est `.edp`. Nous allons lire le fichier de maillage et afficher le maillage à l'ai3e des commandes suivantes :

```cpp
  load "gmsh";
  mesh Th=gmshload("carre.msh");
  plot(Th, wait= true, cmm = "Mon superbe maillage");
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

## Exercices

### Diffraction accoustique

#### Disque

En dimension 2, dessiner un cercle de rayon 1, noté $\Gamma$, entouré d'un cercle de même centre et de rayon 3, noté $\Gamma^{\infty}$. Le domaine de calcul, $\Omega$ est le domaine contenu entre $\Gamma$ et $\Gamma^{\infty}$. Nous cherchons à calculer l'onde diffractée $u$ (complexe !) par une onde incidente $u^{inc}$ sur $\Gamma$, solution de l'équation de Helmholtz :
$$
\left\\{
  \begin{array}{r c l l}
    \Delta u + k^2 u & =& 0 & (\Omega)\\\\\\
    u & = & - u^{inc} & (\Gamma)\\\\\\
    \dn u + iku & =& 0 & (\Gamma^{\infty})
  \end{array}
\right.
$$
Le nombre d'onde $k$ sera fixé à $2\pi$ dans un premier temps. Le pas de maillage $h$ dépend du nombre d'onde et nous choisirons $h = \frac{2\pi}{kn\_{\lambda}}$ avec $n\_{\lambda} = 15$. L'onde incidente sera plane et de direction $\alpha \in [0,2\pi]$ : $u^{inc}(x,y) =e^{ik(x\cos(\alpha) + y\sin(\alpha))}$. La quantité à afficher est le champ total physique $u\_t = u + u^{inc}$. Pour gagner un peu de mémoire, **ne maillez pas le disque intérieur** ! 

{{< figure src="../disk.svg" title="Sous-marin 2D" numbered="true" width="500px">}}


{{% alert note %}}
Pour avoir des quantités complexes dans FreeFem++, il suffit de "templater" les espaces fonctionnels (comme en C++) et le $i$ complexe est obtenu par `1i` :
```
[...]
//1i représente le i complexe :
func uinc = exp(1i*k*(x*cos(alpha) + y*sin(alpha)));
[...]
Vh<complex> uh,vh;
[...]
// Pour obtenir les parties réelles, valeur absolue :
Vh<complex> uabs = abs(uh + uinc);
Vh<complex> ure = real(uh);
```
Vous pouvez exporter le résultat au format GMSH (mais Paraview fait un meilleur boulot !). Pour cela, télécharger [le fichier gmshExport.idp](https://github.com/Bertbk/ff_gmsh/blob/master/gmshExport.idp) 
```
//Exporter en GMSH (avec gmshExport.idp)
include "gmshExport.idp";
gmshExport(Th, uh[], "uh.pos");
gmshExport(Th, ure[], "ure.pos");
gmshExport(Th, uabs[], "uabs.pos");
```
{{% /alert %}}

#### Plusieurs disques

Faites de même avec plusieurs disques, ou alors un objet de forme différente. Le bord $\Gamma$ sera alors l'union des bords de chaque obstacle.

#### Sous-marin

Toujours en dimension 2, dessiner un (joli) sous-marin entouré d'une ellipse. Faites en sorte que la longueur du sous-marin soit égale à 1. Le domaine de calcul, $\Omega$ est situé entre le sous-marin et l'ellipse. Le bord du sous-marin est noté $\Gamma$ et l'ellipse $\Gamma^{\infty}$. Prenez $R1$ et $R2$ suffisamment grand (mais pas trop !). Les équations sont les même que précédemment et comme pour les disques, évitez de mailler le sous-marin !

{{< figure src="../submarine.svg" title="Sous-marin 2D" numbered="true" >}}


### Mécanique des fluides

TODO: