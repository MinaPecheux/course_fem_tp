+++
title = "Annexe : Commandes Utiles"

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
  name = "Annexe : Commandes Utiles"
  weight = 100

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

### Annexe : quelques commandes FreeFem++


#### Boucle for
Dans FreeFem++, les boucles `for` se construisent comme en C++. Tout simplement.

#### I/O

Pour écrire dans un fichier, on utilise `ofstream`, comme en C++ (sans les `std::`). Par exemple :

```cpp
  real pipi = pi^2;
  ofstream sortie("MonFichier.txt");
  sortie << "pi*pi=" << pipi << endl;
```

Attention, il ne faut surtout pas cloturer les `ofstream` car FreeFem++ le fait pour nous en sortie !

#### Portée d'une variable (=durée de vie)

Comme en C/C++, la portée d'une variable (= sa durée de vie) est déterminée par la prochaine accolade fermante. Cela vaut pour les `fespace` et autres variables propres à FreeFem++ ! :
```cpp
  {
    int a = 10;
    mesh Th = square(a,a);
    cout << "a=" << a << endl; // OK
    plot(Th);                      // OK
  }
  cout << "a=" << a << endl; // Pas OK : a est détruit
  plot(Th);                      // Pas OK : le maillage est détruit
```
Cette remarque est pratique pour la définition de vos boucles `for`.

#### Tableaux

Nous pouvons définir des tableaux dynamiques dans FreeFem++, un peu comme en C. De plus, dans FreeFem++, un tableau possède plusieurs paramètres et fonctions accessibles facilement :

  - `MonTableau.max` : Renvoie la valeur max du tableau 
  - `MonTableau.min` : Renvoie la valeur m du tableau
  - `MonTableau.l1`  : Renvoie la norme $L^1$ du tableau
  - `MonTableau.l2`  : Renvoie la norme $L^2$ du tableau
  - `MonTableau.linfinity`  : Renvoie la norme $L^{\infty}$ du tableau
  - `MonTableau'`    : Transposé du vecteur
  - `a'*b`    : Produit scalaire entre `a` et `b`.
  

  Ce qui peut donner, par exemple :

```cpp
  int n = 10;
  real[int] t(n);
  for (int i =0; i < n ; i++)
    t[i] = cos(pi/n*i);
  cout << t;            // Affichage de tout le tableau
  cout << t[0] << endl; // Affiche de la première valeur
  cout << "t_max=" << t.max<<endl;
  cout << "t_min=" << t.min<<endl;
```

La notation "à la Matlab" est valable aussi :
```cpp
  t(3:4) = 4; // t[3] = t[4] = 4;
```

Les tableaux peuvents être ajoutés, soustraits, divisés/multipliés point à point :
```cpp
  real[int] a(10), b(10), c(10);
  c = a + b;
  c = a - b;
  c = a.*b;
  c = a./b;
  c = 2*a;
```

### GnuPlot

Si vous voulez tester GnuPlot pour afficher vos courbes, voici quelques notes pour afficher des courbes à partir de fichier de données. Nous supposons que le fichier se nomme `data.txt`, contienne 100 valeurs et soit de la forme suivante
```
x1    y1
x2    y2
x3    y3
x4    y4
...  
x100 y100
```

Une possibilité\footnote{Voir la documentation \url{http://www.gnuplot.info/docs_5.0/gnuplot.pdf}, et aussi des exemples :\\ \url{http://gnuplot.sourceforge.net/demo/lines_arrows.html}\\\url{http://www.gnuplotting.org/plotting-data/}\\\url{http://www.gnuplotting.org/plotting-functions/}} est alors d'écrire un fichier gnuplot. Ouvrez un fichier, par exemple `affiche.gnu`, et placez ceci dans son en-tête :
```gnuplot
  #En tete de votre script pour pouvoir le lancer depuis un terminal
set terminal wxt size 700,524 enhanced font 'Verdana,10' persist
```
Ensuite, pour afficher la courbe :
```gnuplot
plot 'data.txt'
```
Lancez le code depuis le terminal avec la commande suivante :
\begin{lstlisting}[language=bertbash]
gnuplot affiche.gnu
```
Vous devriez voir apparaître une courbe, ou plutôt, uniquement les points. Pour tracer la courbe (droite) entre les points, nous ajoutons des options :
```gnuplot
plot 'data.txt' with linespoints
```
Pour obtenir une courbe en pointillées :
```gnuplot
plot 'data.txt' dashtype 2 with linespoints
```
Vous pouvez modifier le 2 pour d'autres valeurs (3, 4,...) et observez les différentes possibilités. La valeur 1 est une ligne solide. Nous pouvons aussi directement, comme dans Matlab, proposer un `dashtype` :
```gnuplot
plot 'data.txt' dashtype '-.' with linespoints
```

#### Modifier/Supprimer la légende d'une courbe

Pas de légende :
```gnuplot
plot 'data.txt' notitle
```
Avec un joli titre 
```gnuplot
plot 'data.txt' title 'Ma courbe'
```

#### Plusieurs courbes sur une figure

Pour afficher une deuxième courbe sur la même figure, utiliser `replot` :
```gnuplot
  plot 'data1.txt' [OPTIONS]
  replot 'data2.txt' [OPTIONS]
```
ou mettez les fichiers les uns à la suite des autres (avec une virgule entre chaque et un backslash) :
```gnuplot
plot 'data.txt' dashtype 4 with linespoints, \
     'data2.txt' dashtype 3 with linespoints
```
Par exemple :
```gnuplot
  plot 'data1.txt' with linespoints \
  'data2.txt' with linespoints
```
'
#### Afficher des fonctions

Pour afficher $y=x$ :
```gnuplot
y(x) = x
plot y(x) title 'y=x'
```
