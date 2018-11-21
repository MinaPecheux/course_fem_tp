+++
title = "Getting Started"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math = true

# Add menu entry to sidebar.
[menu.fem_tp]
  parent = "freefem"
  name = "Getting Started"
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


[FreeFem++](https://freefem.org) est développé (majoritairement) au Laboratoire Jacques-Louis Lions à Sorbonne Université. Logiciel open-source, il permet de résoudre des problèmes simples ou compliqués à l'aide de la méthode des éléments finis. Il est utilisé à la fois dans le monde académique pour la recherche et l'enseignement, mais aussi dans le monde industriel. Une de ses forces réside dans son langage facile à prendre en main.

Tout au long de ces TPs, nous présentons l'utilisation de FreeFem++ seul puis couplé avec GMSH. Notre but étant de résoudre, dans un premier temps, un problème simple afin d'évaluer les performances de la méthode des éléments finis. Nous utiliserons ensuite ces logiciels afin de résoudre un problème plus compliqué et concret.

## Installation

Les instructions sont fournies sur le [dépôt Github du logiciel](https://github.com/FreeFem/FreeFem-sources/releases). En particulier, des binaires sont fournies pour Mac Os, Windows et Ubuntu. Pour Linux, en général, il faut (malheureusement) passer par la [case compilation](https://github.com/FreeFem/FreeFem-sources) (voir sur [l'ancienne version du site](http://www3.freefem.org/ff++/linux.php)).

## Utilisation

FreeFem++ ne dispose pas d'interface graphique, il s'utilise uniquement via le terminal. Ses fichiers ont en général pour extension `.edp` (pour Equations aux Dérivées Partielles). La commande pour lancer FreeFem++ sur un fichier `Coucou.edp` est donc 
```bash
FreeFem++ coucou.edp
```
Si vraiment vous êtes allergique au terminal, il existe [FreeFem++-cs](https://www.ljll.math.upmc.fr/lehyaric/ffcs/index.htm).

## Documentation

Ce tutoriel est bien loin d'être exhaustif, il vous faudra vous référer à [la documentation officielle](https://doc.freefem.org/) de temps en temps (le site a l'air de parfois planter, dans ce cas référez vous à [la documentation pdf](http://www3.freefem.org/ff++/ftp/freefem++doc.pdf)).

En fin de section, vous trouverez aussi un récapitulatif de [commandes et fonctions pratiques dans FreeFem++]({{< relref "freefem_annexe.md">}})