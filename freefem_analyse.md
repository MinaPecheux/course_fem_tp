+++
title = "Analyse de l'Erreur"

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
  name = "Analyse de l'Erreur"
  weight = 30

+++

$\newcommand{\diff}{\mathrm{d}}$
$\newcommand{\xx}{\mathbf{x}}$
$\newcommand{\vec}[1]{\mathbf{#1}}$
$\newcommand{\conj}[1]{\overline{#1}}$
$\newcommand{\Pb}{\mathbb{P}}$
$\newcommand{\dn}{\partial\_{\mathbf{n}}}$
$\newcommand{\Lo}{L^2(\Omega)}$
$\newcommand{\Ho}{H^1(\Omega)}$
$\newcommand{\Hoz}{H^1\_0(\Omega)}$
$\newcommand{\dsp}{\displaystyle}$
$\newcommand{\uh}{u\_h}$
$\newcommand{\eh}{e\_h}$
$\newcommand{\Pih}{\Pi\_h}$
$\newcommand{\Vh}{V\_h}$
$\newcommand{\Vhref}{V\_h^{\text{ref}}}$
$\newcommand{\Nh}{N\_h}$
$\newcommand{\Pihref}{\Pi\_h^{\text{ref}}}$
$\newcommand{\norm}[1]{\left\\|#1\right\\|}$
$\newcommand{\normL}[1]{\norm{#1}\_{\Lo}}$
$\newcommand{\normH}[1]{\norm{#1}\_{\Ho}}$
$\newcommand{\Thref}{T\_{h}^{\text{ref}}}$
$\newcommand{\varphiref}{\varphi^{\text{ref}}}$
$\newcommand{\Nhref}{N\_{h}^{\text{ref}}}$



## Problème modèle

Nous considérons le carré unité $\Omega = [0,1]\times[0,1]$, la fonction $f(x,y) = (2\pi^2 + 1)\sin(\pi x)\sin(\pi y)$ et le problème suivant :
$$
\left\\{
  \begin{array}{r c l l }
    -\Delta u + u & = & f & (\Omega),\\\\\\
    u & = & 0 & (\partial\Omega).
  \end{array}
\right.
$$

La solution exacte de ce problème est $u(x,y) = \sin(\pi x)\sin(\pi y)$.

## Rappel

Notons $h$ le diamètre du plus grand triangle du maillage. Si la solution exacte $u$ est dans $H^{k+1}(\Omega)$, nous avons vu (ou allons voir) que la convergence de la méthode des éléments finis $\Pb\_k$ était la suivante :
$$
\normH{u-\uh}  = \normH{\eh} \leq C h^k \norm{u}\_{H^{k+1}(\Omega)},
$$
où $\uh$ est la solution exacte du problème approché et $\eh = u-\uh$ est l'erreur commise. Autrement dit, en $\Pb\_1$ (*i.e.* $k=1$), à un maillage donné, si nous souhaitons obtenir une précision 10 fois supérieure, nous devons raffiner le maillage 10 plus (*i.e.* diviser $h$ par 10). En $\Pb\_2$, il nous suffirait de diviser $h$ par $\sqrt{10} \sim 3.16$).

En pratique, cette estimation est plutôt difficile à utiliser (du fait des normes). Nous utiliserons plutôt cette estimation, qui en découle (la constante $C$ est différente naturellement) :
\begin{equation}
\label{eq:erreur}
\frac{\normL{\eh}}{h} \leq C h^{k},
\end{equation}
et $k$ reste le degré des polynômes.


Notons aussi que si $u$ est linéaire (ou affine) par morceau, alors des éléments finis $\Pb\_1$ suffiraient pour obtenir la solution exacte (puisque $u$ appartiendrait en fait à l'espace éléments finis).

Le but de cette partie du TP est de vérifier l'estimation de l'erreur \eqref{eq:erreur}.  Pour éviter tout problème avec GMSH, nous utiliserons le mailleur intégré de FreeFem++ et partirons de ce code:

```cpp
  real h = 0.1     ;   // Diamètre des triangles (A modifier !)
  int nx = floor(1/h); // Nombre de points en x
  int ny = nx;         // Nombre de points en y (= Nbre de points en x)
  mesh Th = square(nx, ny);
  // plot(Th, wait=1);    // Pour visualiser le maillage (facultatif)
```
## Fonctions : $f$ et la solution de référence (ou solution exacte)

Nous connaissons la solution exacte de notre problème, nous l'implémentons donc dans notre code.

{{% alert exercise %}}
Dans votre fichier `analyse.edp`, construisez la solution exacte 
```cpp
func f   = ... // A remplir par vos soins (fonction f)
func uex = ... // A remplir par vos soins (solution exacte)
```
{{% / alert %}}

## Formulation variationnelle : calcul de la solution approchée

La formulation faible de notre problème s'écrit
$$
  \left\\{
    \begin{array}{l}
      \text{Trouver }u\in\Hoz\text{ tel que },\\\\\\
      \forall v\in\Hoz, a(u,v) - \ell(v) = 0,
    \end{array}
  \right.
$$
avec
$$
\begin{array}{rcl}
a(u,v) &=& \dsp\int\_{\Omega}\nabla u(\xx)\cdot\nabla v(\xx)\diff \xx +\int\_{\Omega}u(\xx)v(\xx)\diff\xx\\\\\\
\ell(v) &=& \dsp\int\_{\Omega}f(\xx)v(\xx)\diff\xx
\end{array}
$$
Nous avons vu (ou admettons) que cette formulation variationnelle (qui n'en est pas vraiment une) admet une unique solution. Nous rappelons que $\Hoz$ est l'espace des fonctions $v\in\Ho$ dont la trace $v|\_{\partial\Omega}$ sur $\partial\Omega$ est nulle. Dans FreeFem, cette condition est rajoutée dans la formulation faible par `+on(L1,L2,L3,..., uh=0.)` où `L1,L2,L3,...` sont les numéros des bords sur lesquels s'applique la condition de Dirichlet.

{{% alert exercise %}}
  À l'aide du TP précédent, résolvez ce problème dans FreeFem++. Plus précisément, n'oubliez pas de :

  - Définir un espace fonctionnel `fespace` dans FreeFem++ (`P1` pour l'instant)
  - Introduire deux variables `uh` et `vh` définies sur cet espace fonctionnel
  - Définir et résoudre la formulation variationnelle (cf ci-dessous).

Pour résoudre la formulation variationnelle, nous utiliserons directement `solve` (si `uh` et `vh` sont les variables définies sur votre espace fonctionnel) plutôt que de passer par `varf` et les `matrix` :
```cpp
  solve NomDuProbleme(uh, vh, solver=LU) = int2D(...) +...;
```
Avec cette commande, la solution du problème est directement stockée dans `uh`, et il n'est plus besoin de construire la matrice et le membre de droite (ni la `varf`). Le terme `solver=LU` est ici utilisé pour forcer la résolution avec un solveur direct plutôt qu'itératif.

Pour les conditions aux limites : les Labels des 4 frontières sont : 1, 2, 3 et 4.  Une fois ceci fait, comparez visuellement la solution exacte avec la solution obtenue pour être sûr que c'est (à priori) tout bon. 
{{% /alert %}}


## Calcul de l'erreur


Nous arrivons maintenant au cœur de notre problème : le calcul de l'erreur entre la solution approchée et la solution exacte. La quantité qui nous intéresse est la norme $L^2(\Omega)$ de $\eh = \uh -u$ divisé par $h$ (l'erreur relative):
$$
\frac{1}{h}\normL{\eh}^2 =\frac{1}{h}\normL{\uh-u}^2= \frac{1}{h}\int\_{\Omega}(\uh(\xx) -u(\xx))^2\diff \xx.
$$
Cette intégrale est calculée numériquement à l'aide d'une triangulation de $\Omega$. Nous pouvons utiliser la même triangulation que celle de $\Vh$, auquel cas le code FreeFem s'écrit très simplement :
```
real errL2 = sqrt(int2d(Th)((uh - uexh)^2))/h;
```
Cependant, cela revient, en quelque sorte, à interpoler $u$ sur l'espace $\Vh$ ($u$ est échantillonné sur les points d'intégration associé au maillage de $\Vh$). Cela peut polluer le calcul de l'erreur (en "bien" ou en "mal"!). Pour remédier à cela, l'idée est de calculer l'erreur sur un maillage très fin, $\Thref$, appelé *maillage de référence*. Nous construisons un autre espace éléments finis $\Pb\_1$ basé sur ce maillage, que l'on nommera $\Vhref$. La quantité $\uh$ doit être interpollée sur ce maillage :
```
Thref = ...
fespace Vhref(Thref, P1);
Vhref uhref = uh; //interpolation
real errL2 = sqrt(int2d(Thref)((uhref - uexh)^2))/h;
```

{{% alert note %}}
Sur le maillage de référence $\Thref$ nous n'effectuons qu'un produit matrice-vecteur qui est bien moins coûteux qu'une résolution de système linéaire. C'est pourquoi nous pouvons nous permettre de prendre un maillage très fin.
{{% /alert %}}

{{% alert exercise %}}
C'est parti :

1. À l'aide du code ci-dessus, construisez un maillage structuré `Thref` du carré unité avec 200 points dans chaque direction
2. Calculez l'erreur en norme $\Lo$, le tout divisé par $h$ (comme dans \eqref{eq:erreur}). 
3. Affichez ensuite cette quantité à l'écran avec la commande `cout` (comme en C++). 

Dans la suite, quand nous parlerons de "l'erreur relative $L^2$" nous entendrons cette quantité, c'est à dire l'erreur divisée par $h$.
{{% /alert %}}


## Courbe de convergence

Maintenant que nous avons calculé l'erreur pour un raffinement de maillage particulier, nous pouvons le faire sur d'autres maillages ! En d'autres termes, nous construisons une boucle pour différentes valeurs de h et stockons l'erreur relative à chaque itération. En reportant les données dans un fichier, nous serons à même d'afficher la courbe de l'erreur relative en fonction de h. 

{{% alert tips %}}
On trouvera dans la section dédiée [des commandes et informations utiles]({{< relref "freefem_annexe.md">}}) sur FreeFem++.
{{% /alert %}}

{{% alert exercise %}}
  Modifiez votre code en ajoutant une boucle pour que, à chaque itération, on ait :

- la résolution du problème approché pour un maillage avec `nx = ny = 5, 10, 15, ..., 100`
- le calcul de l'erreur
- le stockage de `h` et de l'erreur dans un fichier
  
{{% /alert %}}


{{% alert exercise %}}
À l'aide du logiciel que vous préférez (Python (matplotlib), excel, gnuplot, MATLAB, ...), afficher la courbe $\normL{\eh}/h$ en fonction de $h$. Attention, pour obtenir de jolies courbes :
  
- Affichez la en échelle $\log-\log$
- Normalisez la : divisez chaque valeur de $\eh$ par $\max{\eh}$, de sorte que la plus haute valeur soit 1, de même pour les valeurs $h$ par rapport à leur max.
- Affichez sur la même courbe (en pointillés par exemple), la courbe $y=x$ qui correspond à $\eh=h$. 

Les deux premiers points peuvent être effectués dans FreeFem++ de sorte que les données du fichier de sortie soient déjà normalisés et en échelle $\log-\log$.
{{% /alert %}} 
  

{{% alert exercise %}}
Faites de même mais avec des éléments finis `P2` (à la fois pour l'espace de référence et les solutions approchées !). Plutôt que de modifier le code, vous pouvez rajouter la portion de code permettant d'obtenir l'erreur en $\Pb\_2$.
{{% /alert %}}

{{% alert exercise %}}
Sur une même figure, affichez les courbes d'erreur `P1` et `P2`, et les droites de prédictions ($\eh = h$ et $\eh=2h$).
{{% /alert %}}

## What about a circle?

Nous allons regarder ce qu'il se passe si $\Omega$ est le disque unité. Pour mailler un cercle unité dans FreeFem++, voici comment faire :

```cpp
border C(t=0,2*pi){x=cos(t); y = sin(t);} // définition d'un bord (ici : cercle unité)
mesh Thref = buildmesh(C(n)); // où n est le nombre de points discrétisant le cercle
```
Ensuite, dans la formulation faible, le `label` correspondant au bord sera (ici) `C`.

Nous prendrons ici comme paramètres $f(x,y) = -4 + x^2 + y^2$, $g(x,y) = x^2 + y^2$ et nous résolvons le problème suivant : 
$$
\left\\{
  \begin{array}{r c l l }
    -\Delta u + u & = & f & (\Omega),\\\\\\
    u & = & g & (\partial\Omega).
  \end{array}
\right.
$$

La solution exacte est $u(x,y) = x^2 + y^2$.

{{% alert exercise %}}
Copiez votre fichier `analyse.edp` dans `analyse_disk.edp` et adaptez le au cas du cercle. Nous supposerons que la valeur de $h$ est toujours obtenue par $\frac{\sqrt{2}}{n}$, même si cela est sans doute légèrement faux. Obtenez les courbes de convergence pour $\Pb\_1$ et $\Pb\_2$. Commentez.
{{% /alert %}}


## Et pour une condition de Neumann ?

Considérons le problème de Neumann homogène suivant  où $f(x,y) = (2\pi^2+1)\cos(\pi x)\cos(\pi y)$ dans le carré unité :
$$
\left\\{
  \begin{array}{r c l l }
    -\Delta u + u & = & f & (\Omega),\\\\\\
    \dn u & = & 0 & (\partial\Omega).
  \end{array}
\right.
$$
La solution exacte est $u(x,y) = \cos(\pi x)\cos(\pi y)$.

{{% alert exercise %}}
Copiez votre fichier `analyse.edp` dans `neumann.edp` et adaptez le pour résoudre le problème ci-dessus. Comme précédemment : nous voulons voir les courbes de convergence ! 
{{% /alert %}}



