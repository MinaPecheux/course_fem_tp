+++
title = "Premiers pas"

date = 2018-09-09T00:00:00
# lastmod = 2018-11-21T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math = true

# Add menu entry to sidebar.
[menu.fem_tp]
  parent = "freefem"
  name = "Premiers pas"
  weight = 20

+++

$\newcommand{\diff}{\mathrm{d}}$
$\newcommand{\xx}{\mathbf{x}}$
$\newcommand{\vec}[1]{\mathbf{#1}}$
$\newcommand{\conj}[1]{\overline{#1}}$
$\newcommand{\Pb}{\mathbb{P}}$
$\newcommand{\dn}{\partial\_{\mathbf{n}}}$
$\newcommand{\Lo}{L^2(\Omega)}$
$\newcommand{\Ho}{H^1(\Omega)}$
$\newcommand{\dsp}{\displaystyle}$
$\newcommand{\uh}{u\_h}$
$\newcommand{\eh}{e\_h}$
$\newcommand{\norm}[1]{\left\\|#1\right\\|}$
$\newcommand{\normL}[1]{\norm{#1}\_{\Lo}}$
$\newcommand{\normH}[1]{\norm{#1}\_{\Ho}}$

FreeFem++ fournit une infrastructure permettant de manipuler relativement facilement les concepts liés à la méthode des éléments finis : espaces fonctionnels, formulations variationnelles, résolution,... Le but de cette section est de comprendre comment se servir des briques de base de FreeFem++ pour résoudre un problème simple.

## Problème modèle

Soit $\Omega$ le carré unité et le problème suivant
$$
\left\\{
  \begin{array}{r c l l}
    -\Delta u + u & = & f &(\Omega)\\\\\\
    \dn u & = & 0 & (\Gamma := \partial\Omega).
  \end{array}
\right.
$$
La formulation faible associée est
\begin{equation}
\label{eq:pb}
\left\\{
  \begin{array}{l}
    \text{Trouver }u\in\Ho \text{ tel que},\\
      \forall v\in\Ho, \quad a(u,v) = \ell(v),
  \end{array}
\right.
\end{equation}

avec
$$
\begin{array}{r c l }
a(u,v) &=& \dsp \int\_{\Omega} \nabla u(\xx)\cdot \conj{\nabla v(\xx)}\diff \xx + \int\_{\Omega} u(\xx)\cdot \conj{v(\xx)}\diff \xx,
\\\\\\
\ell(v) &=& \dsp \int\_{\Omega} f(\xx)\conj{v(\xx)}\diff s(\xx).
\end{array}
$$
Nous avons vu en cours que ce problème admet une unique solution.

{{% alert exercise %}}
Prenons $f(x,y) = (2\pi^2+1)\sin(\pi x)\sin(\pi y)$. Montrez que la solution au problème \eqref{eq:pb} est $u(x,y) = \sin(\pi x)\sin(\pi y)$.
{{% /alert %}}

## Maillage : `mesh`

FreeFem++ permet de mailler des domaines simples, comme des rectangles. La [documentation vous donnera plus de détails](http://www3.freefem.org/ff++/ftp/freefem++doc.pdf#chapter.5) (Chapter 5 - Mesh Generation). 

Par exemple, la commande suivante :
```cpp
mesh Th = square (4,5);
```
génère un carré unitaire de 4x5 points. Chaque frontière possède son propre *label* (équivalent de `Physical Line` dans GMSH), permettant de le différencier.
Un autre exemple pour une grille de `nxm` points et du rectangle `[x0, x1]x[y0,y1]` : 
```cpp
real x0=1.2, x1=1.8;
real y0=0, y1=1;
int n=5,m=20;
mesh Th = square(n,m,[x0+(x1-x0)*x,y0+(y1-y0)*y]);
```

Pour afficher le maillage, utilisez simplement
```cpp
plot(Th);
```

{{< figure library="1" src="/course/fem_tp/square.png" title="Carré unitaire avec 10 points de grille dans chaque direction (repris de la documentation officielle)" numbered="true" >}}

{{% alert note %}}
Le langage propre à FreeFem++ est un idiome du C++ : sa syntaxe ressemble fortement à celle du C++. Par exemple, `mesh` est un type (comme une classe). Les nombres ont deux types : `real` ou `int`. Certaines fonctions et fonctionnalités standard sont accessibles dans FreeFem++ comme : 

- Affichage : `cout` (sans le `std::`)
- Conditionnel : `for`, `if`
- Tableaux : `a = int[2]` et l'accès `a[0]`

De nombreuses autres sont présentées [dans ce tutoriel]({{< relref "freefem_annexe.md">}}).
{{% /alert %}}

## Fonctions analytiques : `func`

Pour définir une fonction analytique dans FreeFem++, nous utilisons le type `func`, et nous nous aidons des mots syntaxiques `x`, `y` et `z`, qui représentent respectivement la coordonnée x, y et z. Par exemple la commande suivante définit la fonction $g(x,y) = x^2 + y^2$ :
```cpp
func g = x*x + y*y; // Définition de g
```
Les fonctions mathématiques usuelles comme le sinus, cosinus, exponentielle,... sont disponibles sous FreeFem++.


## Espace fonctionnel : `fespace`

### Typage et utilisation

Les espaces fonctionnels ont leur propre type, `fespace` (*Finite Element Space*) et sont basés sur un `mesh`. Un espace fonctionnel `Vh`, de type $\Pb\_1$-Lagrange sur un maillage `Th` est défini ainsi :
```cpp
fespace Vh(Th, P1);
```
Le terme `P1` fait parti de la syntaxe propre à FreeFem++ spécifiant que nous utilisons des éléments finis $\Pb\_1$-Lagrange (il est possible de définir un espace $\Pb\_0, \Pb\_2,...$ de la même manière). Autrement dit, l'espace est généré par les fonctions linéaires par morceaux et égales à 1 sur un unique sommet, et 0 sur tous les autres. Cet espace nous permet de calculer les solutions mais aussi d'interpoler toute fonction analytique (*i.e.* dont on connait l'expression).

Une fois un espace fonctionnel défini, son *"nom devient un type"*. Autrement dit, nous pouvons définir une quantité `uh` sur l'espace `Vh` par :
```cpp
Vh uh;
```

### Interpolation de fonction

Une fonction (`func`) peut être interpolée dans un espace éléments finis (`fespace`). Si on reprend l'exemple de la fonction $g(x,y)$ précédente :
```cpp
func g = x*x + y*y; // Définition de g
Vh gh = g;          // Interpolation de g sur l'espace Vh
plot(gh, fill=true, dim=3, wait=true); // Affichage
```

{{% alert exercise %}}
Recopiez le code ci-dessus (ainsi que ce qui concerne l'espace fonctionnel) pour afficher la fonction $g$, puis :

1. Modifiez l'espace fonctionnel pour prendre des éléments finis $\Pb\_0$ (constant par morceau), observez le résultat sur l'interpolée de $g$.
2. De la même manière, affichez l'interpolation de la fonction $h(x,y) = \sin(\pi x).\sin(\pi y)$, en $\Pb\_1$ et $\Pb\_2$.
{{% /alert %}}

### Structure d'une fonction éléments finis

Une fonction éléments finis, comme par exemple l'interpolée d'une fonction analytique, peut aussi être vue comme un tableau de valeurs (ses coefficients). Ce tableau s'accède via l'opérateur `[]` et l'on peut modifier les valeurs de ce dernier :

```cpp
gh[] = 1.0; // met tous les coefficients à 1.0
plot(gh, wait=true, fill=true);
```

De plus, le nombre de degrés de liberté (*ddl* en français, *dof* en anglais) d'une fonction éléments finis (respectivement d'un `fespace`) s'obtient par le paramètre `n` (respectivement `ndof`) :
```cpp
cout << "gh[].n = " << gh[].n << ", Vh.ndof = " << Vh.ndof << endl;
```

La valeur d'une fonction éléments finis en un *dof* particulier peut être modifiée directement dans le tableau de valeurs :
```cpp
gh[][10] = 1.0;  // Mise à 1.0 du dof numéro 10
```

{{% alert exercise %}}
Amusons nous un peu :

1. Comparez le nombre de degrés de liberté selon l'ordre des éléments finis, c'est-à-dire entre $\Pb\_0, \Pb\_1$ et $\Pb\_2$. Vérifiez aussi que pour les éléments finis $\Pb\_0$ nous avons bien autant de *dof* (degrees of freedom) que de triangles et pour $\Pb\_1$ autant que de sommets.
2. Affichez une fonction de forme $\Pb\_1$ associée à un nœud quelconque (que vous choisissez).
3. Faites de même pour $\Pb\_0$.
{{% /alert %}}


### Formulations variationnelles

Dans FreeFem++, une formulation variationnelle (*variational formulation*) est de type `varf`. Les opérateurs différentiels $\partial\_{x/y/z}$ s'écrivent `dx`, `dy` et `dz`. Ainsi, la formulation variationnelle précédente s'écrit dans FreeFem++ :
```cpp
Vh uh,vh; // vh sera la fonction test
varf MonProbleme(uh,vh) = int2d(Th)(dx(uh)*dx(vh)) + int2d(Th)(dy(uh)*dy(vh)) + int2d(Th)(uh*vh) - int2d(Th)(f*vh);
```
Dans la quantité `varf(uh,vh)`, le **premier** argument est **l'inconnue** tandis que le **second** est la **fonction test**. Notez que l'équation est supposée égale à 0 ("tout est passé à gauche").

### Résolution en une ligne

Il est possible de résoudre directement une formulation variationnelle sans passer par la matrice en utilisant `solve` à la place de `varf` :
```cpp
Vh uh,vh; // vh sera la fonction test
solve MonProbleme(uh,vh) = int2d(Th)(dx(uh)*dx(vh)) + int2d(Th)(dy(uh)*dy(vh)) + int2d(Th)(uh*vh) - int2d(Th)(f*vh);
```
Cette commande est (très) pratique, cependant, nous perdons la main sur certains éléments.

### Matrice, membre de droite et résolution

Nous pouvons construire la matrice du système linéaire ainsi que le membre de droit (*ou "rhs" pour Right Hand Side*): 
```cpp
matrix A = MonProbleme(Vh, Vh);
Vh b, solution;
b[] = MonProbleme(0, Vh);
solution[] = A^−1 * b[];
plot(solution, wait = true, cmm = "Solution", value = true, fill = true, dim = 3);
```

Quand `MonProbleme` est instancié avec deux arguments, la matrice est calculée (format creux), tandis que si le premier argument est nul, alors seul le membre de droite l'est (vecteur $b$).

{{% alert note %}}
Il est possible de sortir la matrice dans un fichier texte :
```cpp
{ 
  ofstream fout("mat_FF.txt") ;
  fout << matA << endl ;
}  
```
On peut la lire ensuite sous [MATLAB par exemple](http://www.um.es/freefem/ff++/uploads/Main/FFmatrix_fread.m) ou en Python (évidemment).
{{% /alert %}}


{{% alert exercise %}}
Résolvez le problème avec FreeFem++ avec la commande `solve` et en extrayant la matrice et le membre de droite (*i.e.*: recopiez les codes ci-dessus).
{{% /alert %}}

### Conditions aux limites

Pour calculer une intégrale sur un bord 1D (ligne) de label `L`, nous utiliserons la commande
```cpp
int1d(Th, L)(...)
```
Pour imposer à l'inconnue `uh` de valoir `g` sur un bord portant le label `L`, nous utiliserons la commande
```cpp
+ on(L, uh=g)
```
{{% alert exercise %}}
Intéressons nous au problème de Poisson suivant dans $\Omega$ le carré [0,1]x[0,1], avec condition de Dirichlet et $f(x,y) = 2\pi^2\sin(\pi x)\sin(\pi y)$ : 
$$
\left\\{
  \begin{array}{r c l l}
    -\Delta u & = & f & (\Omega)\\\\\\
    u & = & 0 & (\partial\Omega)
  \end{array}
\right.
$$
La formulation faible sera donnée par (*sans être totalement mathématiquement correcte*) (nous n'avons pas encore les outils mathématiques) :
$$
\left\\{
  \begin{array}{l}
    \dsp \int\_{\Omega} \nabla u(\xx)\cdot\nabla v(\xx)\diff\xx - \int\_{\Omega} f(\xx) v(\xx)\diff\xx = 0 \\\\\\
    \text{ et } \dsp u = 0 (\partial\Omega)
  \end{array}
\right.
$$

1. En admettant que ce problème admette une unique solution, montrez que la solution est $u(x,y) = \sin(\pi x)\sin(\pi y)$.
2. Résolvez ce problème avec FreeFem++.

{{% /alert %}}

## Exercices supplémentaires

{{% alert exercise %}}
Décomposer la frontière $\partial\Omega$ en 4 bords, $\Gamma\_1,\Gamma\_2,\Gamma\_3$ et $\Gamma\_4$, puis résoudre le problème de Poisson suivant $f(x,y) = 2\pi^2\sin(\pi x)\sin(\pi y)$  : 
$$
\left\\{
  \begin{array}{r c l l}
    -\Delta u & = & f & (\Omega)\\\\\\
    \dn u - 10u & = & \cos(y) & (\Gamma\_1)\\\\\\
      u & = & 0 & (\Gamma\_2\cup\Gamma\_3\cup\Gamma\_4)
  \end{array}
\right.
$$
{{% /alert %}}

{{% alert exercise %}}
Résoudre l'équation d'advection diffusion avec $f(x,y) = 1+x+y$ et $\vec{w} = (0,1)^T$ :
$$
\left\\{
  \begin{array}{r c l l}
    -\Delta u + \nabla\cdot(u \vec{w}) & = & f & (\Omega)\\\\\\
      u & = & 0 & (\Gamma\_1)
  \end{array}
\right.
$$
{{% /alert %}}
