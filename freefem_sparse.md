+++
title = "Matrice creuse ?"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math = true

# Add menu entry to sidebar.
[menu.fem_tp]
  parent = "freefem"
  name = "Matrice creuse ?"
  weight = 40

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
$\newcommand{\norm}[1]{\left\\|#1\right\\|}$
$\newcommand{\normL}[1]{\norm{#1}\_{\Lo}}$
$\newcommand{\normH}[1]{\norm{#1}\_{\Ho}}$


Analysons maintenant la matrice du système linéaire et notamment son caractère creux : l'est-il vraiment ? Nous en avons parlé en cours, mais nous avons ici une très bonne occasion de l'observer par nous même (*aka* l'enseignant ne raconte pas (que) de la daube).

## Nombre de coefficiens Non-Zéro (nnz)

Dans FreeFem++, une `matrix A` possède différents paramètres :

- `A.n` : taille de la matrice (dans une direction). En $\Pb\_1$ cette valeur est égale au nombre de sommets du maillage. La matrice possède donc $n^2$ coefficients.
- `A.nnz` : Nombre de coefficients non nulles de la matrice.


## C'est parti

Nous réutilisons les briques précédemment introduites. 

{{% alert exercise %}}
Ouvrez un nouveau fichier `sparse.edp` dans lequel :
  Modifiez votre code pour ajouter une boucle portant sur la finesse de maillage : partant d'un maillage très grossier à un très fin ($nx= 100$ par exemple). Pour chaque maillage, stockez dans un fichier le ratio $100.nnz/n^2$, puis, à la fin, affichez la courbe de ce ratio en fonction de $h$.

  Modifiez votre code pour ajouter une boucle portant sur la finesse de maillage : partant d'un maillage très grossier à un très fin ($nx= 100$ par exemple). Pour chaque maillage, stockez dans un fichier le ratio $100.nnz/n^2$, puis, à la fin, affichez la courbe de ce ratio en fonction de $h$.
ge du carré unité
  Modifiez votre code pour ajouter une boucle portant sur la finesse de maillage : partant d'un maillage très grossier à un très fin ($nx= 100$ par exemple). Pour chaque maillage, stockez dans un fichier le ratio $100.nnz/n^2$, puis, à la fin, affichez la courbe de ce ratio en fonction de $h$.
 construisez un espace éléments finis `P1` ainsi que la formulation faible du problème de Dirichlet. Au lieu de   Modifiez votre code pour ajouter une boucle portant sur la finesse de maillage : partant d'un maillage très grossier à un très fin ($nx= 100$ par exemple). Pour chaque maillage, stockez dans un fichier le ratio $100.nnz/n^2$, puis, à la fin, affichez la courbe de ce ratio en fonction de $h$.
ns ici une `varf` :
  Modifiez votre code pour ajouter une boucle portant sur la finesse de maillage : partant d'un maillage très grossier à un très fin ($nx= 100$ par exemple). Pour chaque maillage, stockez dans un fichier le ratio $100.nnz/n^2$, puis, à la fin, affichez la courbe de ce ratio en fonction de $h$.

  Modifiez votre code pour ajouter une boucle portant sur la finesse de maillage : partant d'un maillage très grossier à un très fin ($nx= 100$ par exemple). Pour chaque maillage, stockez dans un fichier le ratio $100.nnz/n^2$, puis, à la fin, affichez la courbe de ce ratio en fonction de $h$.
.)...;
  Modifiez votre code pour ajouter une boucle portant sur la finesse de maillage : partant d'un maillage très grossier à un très fin ($nx= 100$ par exemple). Pour chaque maillage, stockez dans un fichier le ratio $100.nnz/n^2$, puis, à la fin, affichez la courbe de ce ratio en fonction de $h$.

- Construisez la `matrix` de cette `varf` (voir TP1). Nous appellerons cette matrice `A`.
- Calculez le rapport $nnz/n^2$ entre le nombre de coefficients non nul de la matrice et son nombre total de coefficients.
    
{{% /alert %}}
  Modifiez votre code pour ajouter une boucle portant sur la finesse de maillage : partant d'un maillage très grossier à un très fin ($nx= 100$ par exemple). Pour chaque maillage, stockez dans un fichier le ratio $100.nnz/n^2$, puis, à la fin, affichez la courbe de ce ratio en fonction de $h$.

  Modifiez votre code pour ajouter une boucle portant sur la finesse de maillage : partant d'un maillage très grossier à un très fin ($nx= 100$ par exemple). Pour chaque maillage, stockez dans un fichier le ratio $100.nnz/n^2$, puis, à la fin, affichez la courbe de ce ratio en fonction de $h$.

  Modifiez votre code pour ajouter une boucle portant sur la finesse de maillage : partant d'un maillage très grossier à un très fin ($nx= 100$ par exemple). Pour chaque maillage, stockez dans un fichier le ratio $100.nnz/n^2$, puis, à la fin, affichez la courbe de ce ratio en fonction de $h$.
 boucle portant sur la finesse de maillage : partant d'un maillage très grossier à un très fin   Modifiez votre code pour ajouter une boucle portant sur la finesse de maillage : partant d'un maillage très grossier à un très fin ($nx= 100$ par exemple). Pour chaque maillage, stockez dans un fichier le ratio $100.nnz/n^2$, puis, à la fin, affichez la courbe de ce ratio en fonction de $h$.
 maillage, stockez dans un fichier le ratio $100.nnz/n^2$, puis, à la fin, affichez la courbe de   Modifiez votre code pour ajouter une boucle portant sur la finesse de maillage : partant d'un maillage très grossier à un très fin ($nx= 100$ par exemple). Pour chaque maillage, stockez dans un fichier le ratio $100.nnz/n^2$, puis, à la fin, affichez la courbe de ce ratio en fonction de $h$.

  Modifiez votre code pour ajouter une boucle portant sur la finesse de maillage : partant d'un maillage très grossier à un très fin ($nx= 100$ par exemple). Pour chaque maillage, stockez dans un fichier le ratio $100.nnz/n^2$, puis, à la fin, affichez la courbe de ce ratio en fonction de $h$.

  Modifiez votre code pour ajouter une boucle portant sur la finesse de maillage : partant d'un maillage très grossier à un très fin ($nx= 100$ par exemple). Pour chaque maillage, stockez dans un fichier le ratio $100.nnz/n^2$, puis, à la fin, affichez la courbe de ce ratio en fonction de $h$.
