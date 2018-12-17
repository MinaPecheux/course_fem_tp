+++
title = "Projet"

date = 2018-09-09T00:00:00
# lastmod = 2018-11-21T:00:00

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
  parent = "python"
  identifier = "python_overview"
  name = "Projet"
  weight = 10

+++

$\newcommand{\diff}{\mathrm{d}}$
$\newcommand{\xx}{\mathbf{x}}$
$\newcommand{\vec}[1]{\mathbf{#1}}$
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

## Objectifs

Le but de ce projet est de résoudre le problème de diffraction d'une onde acoustique par un sous-marin par la méthode des éléments finis $\Pb\_1$-Lagrange, dont [voici les détails mathématiques]({{<relref "freefem_gmsh.md#sous-marin">}}). En bref, vous devez :

1. Générer le **maillage sur GMSH**
1. Implémenter un assembleur éléments finis $\Pb\_1$ en 2D et **en Python**, *from scratch*
2. Visualiser la solution obtenue par votre code **sur ParaView**

## Maillage 

Il sera généré par GMSH. Vous devez donc implémenter une fonction de lecture de [fichier `.msh`]({{<ref "/course/gmsh/basics_meshformatv2.md">}}). N'oubliez pas que le label `Physical` sera utile et doit donc être stocké dans votre code Python. La numérotation des points et des éléments est à votre discrétion (non nécessairement identique à celle de GMSH).

## Bibliothèques

Voici une liste de bibliothèques Python que vous pouvez utiliser. Libre à vous d'en utiliser d'autres si vous le souhaitez :

1. Algèbre linéaire : [Numpy](https://www.numpy.org/)
2. Affichage (rappel : affichage final sur Paraview) : [Matplotlib](https://matplotlib.org)

## Contraintes

### Pratique

1. Deadline : Vous avez jusqu'au **dimanche 27 janvier inclus** pour m'envoyer votre code, soit par email soit par lien vers dépôt git (préférable).
2. Vous pouvez réaliser ce projet à 2, mais chaque participant(e) **doit être actif/active** et **connaître le code**.
3. Format : plusieurs solutions :
  - Vos **fichiers et script Python** `*.py` le tout complété par un **fichier README** court et efficace expliquant comment lancer le (ou les) programme(s) ainsi que la structure globale de votre code. La personne qui réceptionne votre code (*i.e.* moi) ne doit passer plus de 2 minutes pour comprendre comment lancer le programme, sinon il (rappel : moi) arrête et passe au suivant. 
  - **Notebook Jupyter** (ou autre) mais alors complété par des explications (sinon à quoi bon un Notebook ?).

### De programmation

1. Votre code doit être **propre**, c'est à dire :
  1. **Commenté**
  2. **Structuré** : pas de mono-fichier de 1000 lignes
  3. **Lisible** : les variables et les fonctions ont un nom *compréhensible* par un humain tel que moi
  4. Compartimenté : utilisez **des classes** et des **méthodes**
2. En plus de la matrice de la formulation faible lié au problème considéré, votre programme doit pouvoir calculer **séparément** (*i.e* posséder des fonctions à part) :
  1. La matrice de masse
  2. La matrice de rigidité
  3. Le membre de droite
3. (Relativement) **Performant** : la matrice du système étant creuse, elle sera stockée **sous format creux**, COO ou CSR à votre aide. Vous devez en revanche pouvoir m'expliquer votre choix.
4. Je dois pouvoir fournir **mon propre maillage** et faire fonctionner votre code dessus.

{{% alert note %}}
Pour pouvoir utiliser Scipy et ses [**matrices creuses**](https://docs.scipy.org/doc/scipy/reference/sparse.html) (*sparse matrices* en anglais), nous devons utiliser **Python2** (et non Python3). Le plus pratique pour construire la matrice du système au format CSR est certainement de créer une matrice [au format COO](https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.coo_matrix.html#scipy.sparse.coo_matrix) en ajoutant chaque contribution élémentaire à la suite (sans les sommer) puis de [convertir la matrice au format CSR](https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.coo_matrix.tocsr.html#scipy.sparse.coo_matrix.tocsr) à l'aide de `tocsr`. La sommation sera automatiquement effectuée par Scipy.
{{% /alert %}}

## Astuces

1. Avant de faire le sous-marin, testez votre code sur des *petits cas*, par exemple [l'équation de Laplace]({{<relref "freefem_first_step.md#problème-modèle">}}) puis la [diffraction par un disque]({{<relref "freefem_gmsh.md#disque">}})
2. Comparez vos résultats avec ceux obtenus avec FreeFem
3. Vérifiez que vos matrices de masse $M$ et de rigidité $D$ sont valides par les formules suivantes, où $U=[1,\ldots,1]^T$ :
$$
\left\\{
  \begin{array}{r c l}
    U^T M U &=& \left|\Omega\right| \\\\\\
    D U &=& 0
  \end{array}
\right.
$$

4. Soyez rigoureux et avancez à petits pas : à chaque nouvelle fonction, vous devez **valider le code**. Si le code **plante**, vous **corrigez**. Si le code tourne mais le **résultat est faux**, vous **corrigez**.
5. Le programme ne donne pas le bon résultat ou plante mais vous êtes sûrs et certain(e)s que votre code est juste et sans erreur ? C'est sûr, le problème vient ~~de l'ordinateur~~ de votre code, navré pour vous.
6. Utilisez [Git](https://git-scm.com) pour gérez vos sources et un dépôt en ligne est souhaitable ([github](https://github.com), gitlab ([gitlab.com](https://gitlab.com) ou d'autres instances comme [framagit](https://framagit.org)), [bitbucket](https://bitbucket.org), ...). Vous ne savez pas utiliser Git ? **Alors apprenez**, c'est l'occasion !
6. Les formats de fichiers (I/O) pour [GMSH et Paraview sont disponibles ici]({{<relref "python_format.md">}})

## C'est parti !

<div style="width:100%;height:0;padding-bottom:76%;position:relative;"><iframe src="https://giphy.com/embed/1APcn7WntDBd0ZcZEm" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div>
