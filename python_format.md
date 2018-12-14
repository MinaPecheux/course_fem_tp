+++
title = "(Aide) Formats de fichiers"

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
  identifier = "python_format"
  name = "Formats de fichiers"
  weight = 20

+++

## Format GMSH

Voir [le format de fichier v2]({{<ref "/course/gmsh/basics_meshformatv2.md" >}}) et surtout le warning ci-dessous :

{{% alert warning %}}
Ajoutez ceci en haut de vos fichiers `.geo` :
```
Mesh.MshFileVersion = 2.2;
```
{{% /alert %}}


## Format Parview : `.vtu`

Voici un exemple très simple d'un fichier de données complexes au format Paraview basé sur un maillage non structuré (*UnstructuredGrid*), mais vous pouvez agir différemment (via des fichiers `.csv` par exemple) :

```
<VTKFile type="UnstructuredGrid" version="1.0" byte_order="LittleEndian" header_type="UInt64">
<UnstructuredGrid>
<Piece NumberOfPoints="5" NumberOfCells="4">
<Points>
<DataArray NumberOfComponents="3" type="Float64">
0.0 0.0 0 
1.0 0.0 0 
1.0 1.0 0 
0.0 1.0 0 
0.5 0.5 0 
</DataArray>
</Points>
<Cells>
<DataArray type="Int32" Name="connectivity">
0 1 4
1 2 4
2 3 4
3 0 4
</DataArray>
<DataArray type="Int32" Name="offsets">
3
6
9
12
</DataArray>
<DataArray type="UInt8" Name="types">
5 
5 
5 
5 
</DataArray>
</Cells>
<PointData Scalars="solution">
<DataArray type="Float64" Name="Real part" format="ascii">
1.0
0.9999949269133752
0.9999949269133752
1.0
-0.9999987317275395
</DataArray>
<DataArray type="Float64" Name="Imag part" format="ascii">
0.0
-0.0031853017931379904
-0.0031853017931379904
0.0
0.0015926529164868282
</DataArray>
</PointData>
</Piece>
</UnstructuredGrid>
</VTKFile>
```

## C'est parti !

<div style="width:100%;height:0;padding-bottom:76%;position:relative;"><iframe src="https://giphy.com/embed/1APcn7WntDBd0ZcZEm" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div>