[![Discourse topics](https://img.shields.io/discourse/topics?color=limegreen&server=https%3A%2F%2Figraph.discourse.group)](https://igraph.discourse.group)
[![GitHub (pre-)release](https://img.shields.io/github/release/szhorvat/IGraphM/all.svg)](https://github.com/szhorvat/IGraphM/releases)
[![GitHub All Releases](https://img.shields.io/github/downloads/szhorvat/IGraphM/total.svg)](https://github.com/szhorvat/IGraphM/releases)
[![Contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)](https://github.com/szhorvat/IGraphM#contributions)
[![DOI](https://zenodo.org/badge/41793262.svg)](https://zenodo.org/badge/latestdoi/41793262)

# [IGraph/M – igraph for Mathematica][main]

The IGraph/M package provides a [_Mathematica_](http://www.wolfram.com/mathematica/) interface to the popular [igraph](http://igraph.org/) network analysis and graph theory package, as well as many other functions for working with graphs in _Mathematica_.  Check out the [blog post][main] for an overview.


## Installation

First, note that the system requirements are _Mathematica_ 10.0.2 or later, 64-bit Windows/macOS/Linux, or Raspberry Pi.

The simplest way to install automatically from the internet is to evaluate the following line in Mathematica:

```mathematica
Get["https://raw.githubusercontent.com/szhorvat/IGraphM/master/IGInstaller.m"]
```

IGraph/M can also be installed manually in the same way as any _Mathematica_ application distributed as a paclet.

Download the `.paclet` file from [the GitHub releases page](https://github.com/szhorvat/IGraphM/releases), and [install it using the `PacletInstall` function in Mathematica](http://mathematica.stackexchange.com/q/141887/12).  For example, assuming that the file `IGraphM-0.3.114.paclet` was downloaded into the directory `~/Downloads`, evaluate

```mathematica
Needs["PacletManager`"]
PacletInstall["~/Downloads/IGraphM-0.3.114.paclet"]
```

IGraph/M requires Mathematica 10.0.2 or later.  Binaries are included for Windows 64-bit, OS X 10.9 or later, Linux x86_64 and Raspbian (Linux ARM on Raspberry Pi).  For other operating systems the package must be compiled from source (see [Development.md](Development.md) for guidance).

After installation, the package can now be loaded with

    << IGraphM`

Check that it works by evaluating `IGVersion[]`, then continue to the documentation with `IGDocumentation[]`.

To uninstall all currently installed versions of IGraph/M, evaluate `PacletUninstall["IGraphM]`. This will remove all traces of IGraph/M from your system.


## Documentation

To open the documentation notebook, evaluate

    Needs["IGraphM`"]
    IGDocumentation[]

or search for "igraphm" in Mathematica's Documentation Center.

The documentation is not yet complete and contributions are very welcome.  If you would like to help out with expanding the documentation, send me an email.

For additional details about functions, or for paper references for the methods used, also check [the igraph documentation pages](http://igraph.org/c/doc/).


## Contributions

**Update: Help wanted with editing documentation and writing unit tests! Only basic Mathematica knowledge is required for this.**

Contributions to IGraph/M are very welcome!  Mathematica programmers of all levels of expertise can contribute.

In order of increasing difficulty, help is needed with the following tasks:

 - Just test the package and try to find problems.
 - Create examples for the documentation or edit the documentation.
 - Write formal unit tests.
 - Implement new functions in pure Wolfram Language code.
 - Expose more igraph library functions to IGraph/M (C++ knowledge required)
 - Implement entirely new functions in C++.

If you are interested in contributing, send me an email for guidance. Evaluate the following in Mathematica to get my email address:

    Uncompress["1:eJxTTMoPChZiYGAorsrILypLLHFIz03MzNFLzs8FAG/xCOI="]

C++ programmers should look at [Development.md](Development.md) for additional information.


## Bugs and troubleshooting

IGraph/M is currently under development, and a few bugs are to be expected.  However, I try not to release a new version until most problems I know of are fixed.  If you do find a problem, please [open an issue on GitHub](https://github.com/szhorvat/IGraphM/issues) or write [in the chatroom](https://gitter.im/IGraphM/Lobby). **In the bug report include the output of ``IGraphM`Developer`GetInfo[]``.**

### Troubleshooting

  * **"Cannot open ``IGraphM` ``"**

    This message will be shown in the following situations:

    - IGraph/M is not installed. Please follow the installation instructions above carefully.

    - IGraph/M is not compatible with your system. Please review the requirements in the Installation section above.  Additional symptoms will be that `PacletFind["IGraphM"]` returns `{}` but `PacletFind["IGraphM", "MathematicaVersion" -> All, "SystemID" -> All]` returns a non-empty list.

  * **"Cannot open ``LTemplate`LTemplatePrivate` ``"**

    Loading fails with this error if one tries to use the `master` branch (development branch) without the necessary dependencies.

    Please download IGraph/M from the [GitHub releases page instead](https://github.com/szhorvat/IGraphM/releases).  It includes everything needed to run the package.

    Do not clone the git repository and do not use the `master` branch unless you want to develop IGraph/M.

  * **"No build settings found. Please check `BuildSettings.m`"**

    This error may be shown when trying to load IGraph/M on an incompatible platform.  Currently, the following platforms are supported: Windows 64-bit, OS X 10.9 or later, Linux x86_64 with glibc 2.14 or later, Raspberry Pi with Raspbian Stretch.

    It may be possible to run IGraph/M on other platforms, however, it will be necessary to compile it from source.

    If you see this error on a supported platform, please file a bug report and include the output of ``IGraphM`Developer`GetInfo[]``.

  * **`The function IGlobal_get_collection was not loaded from the file ...` or similar error appears on Linux**

    Check that you are using a Linux distribution with glibc 2.14 or later. This is the minimum requirement. If your Linux distribution does have the required glibc version, then please open a new issue.

### Known issues and workarounds

   * `Graph[Graph[...], ...]` is returned

     Sometimes layout functions may return an expression which looks like

         Graph[ Graph[...], VertexCoordinates -> {...} ]

     or similar. A property does not get correctly applied to the graph.  This is due to a bug in Mathematica. I believe I have worked around most of these issues, but if you encounter them, one possible workaround is to cycle the graph `g` through some other representation, e.g. `g = Uncompress@Compress[g]`.

   * The graphs returned by `IGBipartiteGameGNM` and `IGBipartiteGameGNP` may not render when using the `DirectedEdges -> True` and `"Bidirectional" -> True` options.  This is due to a bug in Mathematica's `"BipartiteEmbedding"` graph layout and can be corrected by passing `GraphLayout -> Automatic` to these functions. Alternatively, they may be rendered using `IGLayoutBipartite`.

   * LAD functions may crash with certain inputs.  [This is a bug in the igraph C core.](https://github.com/szhorvat/IGraphM/issues/1)

   * `IGDocumentation[]` may not work with Mathematica 11.1 on Linux.  Please enter `IGraphM/IGDocumentation` in the Documentation Center address bar to access it (or simply search for `igraph` in the Documentation Center).

   * `IGZeroDiagonal` will crash with non-square sparse matrices in Mathematica 11.1 and earlier. This is a bug in those versions of Mathematica.

   * When loading IGraph/M in Mathematica 10.0 on recent versions of macOS (e.g. Mojave), the front end might crash. This is a problem specific to Mathematica 10.0, and, as far as I am aware, does not occur with newer versions. If this problem affects you, load IGraph/M as ``Block[{Print}, Needs["IGraphM`"]]``.

   * See also https://github.com/szhorvat/IGraphM/issues


## Revision history

##### v0.4.0dev (work in progress)

New functions in this release:

 - Deterministic graph generators: `IGKautzGraph`, `IGCompleteGraph`, `IGCompleteAcyclicGraph`, `IGDeBruijnGraph`, `IGChordalRing`, `IGEmptyGraph`, `IGRealizeDegreeSequence`, `IGFromPrufer`, `IGToPrufer`, `IGKaryTree`, `IGSymmetricTree`, `IGBetheLattice`, `IGTriangularLattice`, `IGMycielskian`, `IGExpressionTree`, `IGShorthand`, `IGFromNauty`.
 - Random graph generators: `IGWattsStrogatzGame`, `IGCallawayTraitsGame`, `IGEstablishmentGame`, `IGTreeGame`, `IGErdosRenyiGameGNM`, `IGErdosRenyiGameGNP`.
 - Weighted graph functions: `IGWeightedSimpleGraph`, `IGWeightedUndirectedGraph`, `IGWeightedVertexDelete`, `IGWeightedSubgraph`, `IGUnweighted`, `IGDistanceWeighted`, `IGWeightedAdjacencyGraph`, `IGVertexWeightedQ`, `IGEdgeWeightedQ`, `IGVertexStrength`, `IGVertexInStrength`, `IGVertexOutStrength`.
 - Graph colouring functions: `IGVertexColoring`, `IGEdgeColoring`, `IGKVertexColoring`, `IGKEdgeColoring`, `IGMinimumVertexColoring`, `IGMinimumEdgeColoring`, `IGChromaticNumber`, `IGChromaticIndex`, `IGVertexColoringQ`.
 - Clique cover: `IGCliqueCover`, `IGCliqueCoverNumber`.
 - Mesh/graph conversion: `IGMeshGraph`, `IGMeshCellAdjacencyMatrix`, `IGMeshCellAdjacencyGraph`.
 - Lattice generation: `IGLatticeMesh`, `IGTriangularLattice`.
 - Proximity graphs: `IGDelaunayGraph`, `IGGabrielGraph`, `IGRelativeNeighborhoodGraph`, `IGLuneBetaSkeleton`, `IGCircleBetaSkeleton`.
 - Centralization: `IGDegreeCentralization`, `IGBetweennessCentralization`, `IGClosenessCentralization`, `IGEigenvectorCentralization`.
 - Planar graphs: `IGPlanarQ`, `IGMaximalPlanarQ`, `IGOuterplanarQ`, `IGKuratowskiEdges`, `IGFaces`, `IGDualGraph`, `IGEmbeddingQ`, `IGPlanarEmbedding`, `IGOuterplanarEmbedding`, `IGCoordinatesToEmbedding`, `IGEmbeddingToCoordinates`, `IGLayoutPlanar`, `IGLayoutTutte`.
 - Spanning trees and other tree-related functionality: `IGSpanningTree`, `IGRandomSpanningTree`, `IGSpanningTreeCount`, `IGUnfoldTree`, `IGTreeQ`, `IGForestQ`, `IGTreelikeComponents`, `IGTreeGame`, `IGStrahlerNumber`, `IGOrientTree`.
 - Matching functions: `IGMaximumMatching`, `IGMatchingNumber`.
 - Dominance: `IGDominatorTree`, `IGImmediateDominators`
 - Maximum flow: `IGMaximumFlowValue`, `IGMaximumFlowMatrix`.
 - Isomorphism: `IGGetIsomorphism` and `IGGetSubisomorphism` (they work with multigraphs), `IGColoredSimpleGraph` (for transforming multigraph isomorphism to coloured graph isomorphism).
 - Transitivity: `IGVertexTransitiveQ`, `IGEdgeTransitiveQ`, `IGSymmetricQ`, `IGDistanceTransitiveQ`.
 - Regular graphs: `IGRegularQ`, `IGStronglyRegularQ`, `IGStronglyRegularParameters`, `IGDistanceRegularQ`, `IGIntersectonArray`.
 - A framework for easy property transformations and graph styling: `IGVertexProp`, `IGEdgeProp`, `IGEdgeVertexProp`, `IGVertexMap`, `IGEdgeMap`, `IGVertexPropertyList`, `IGEdgePropertyList`.
 - Import function: `IGImport`, `IGImportString`, `$IGImportFormats`; support for importing Graph6, Digraph6 and Sparse6.
 - Export functions: `IGExport`, `IGExportString`, `$IGExportFormats`; support for exporting standards-compliant GraphML that can be read by other igraph interfaces (R, Python).
 - Matrix functions: `IGZeroDiagonal`, `IGTakeUpper`, `IGTakeLower`, `IGAdjacencyMatrixPlot`, `IGKirchhoffMatrix`, `IGJointDegreeMatrix`
 - Added `IGIndexEdgeList` for retrieving the edge list of a graph in terms of vertex indices. This function is very fast and returns a packed array. It facilitates the efficient implementation of graph processing functions in pure Mathematica code, or interfacing with C libraries.
 - Bipartite graphs: `IGBipartiteIncidenceMatrix`, `IGBipartiteIncidenceGraph`, `IGBipartiteProjections`
 - Random walks:  `IGRandomEdgeWalk`, `IGRandomEdgeIndexWalk`
 - Connectivity: `IGGiantComponent`, `IGMinimalSeparators`, `IGMinimumEdgeCuts`, `IGMinimalEdgeCuts`, `IGBridges`, `IGConnectedComponentSizes`, `IGWeaklyConnectedComponentSizes`, `IGGomoryHuTree`
 - Other testing functions: `IGTriangleFreeQ`, `IGSelfComplementaryQ`,  `IGCactusQ`,`IGNullGraphQ`, `IGHomeomorphicQ`.
 - Other new functions: `IGNeighborhoodSize`, `IGCoreness`, `IGVoronoiCells`, `IGSmoothen`, `IGSimpleGraph`, `IGPartitionsToMembership`, `IGMembershipToPartitions`, `IGSinkVertexList`, `IGSourceVertexList`, `IGIsolatedVertexList`, `IGReorderVertices`, `IGTakeSubgraph`, `IGDisjointUnion`, `IGAdjacencyList`, `IGAdjacencyGraph`, `IGTryUntil`

Updates to existing functions:

 - Community detection: Several functions support the `"ClusterCount"` option now; added `IGCommunitiesFluid`.
 - `IGRewireEdges` now supports rewiring only the start or endpoint of directed edges (instead of both).
 - `IGBipartiteQ` now supports checking that a given partitioning is valid for a bipartite graph.
 - `IGBipartitePartitions` now provides control over the ordering of partitions using its second argument.
 - Isomorphism functions now ignore the directedness of empty graphs.
 - Isomorphism functions can now take vertex or edge colours from graph attributes.
 - `IGBlissCanonicalGraph` will now include include vertex colours into its output when appropriate, encoded as the `"Color"` vertex property.
 - `IGDistanceCounts` now optionally takes a list of starting vertices.
 - `IGBetweenness(Estimate)` and `IGCloseness(Estimate)` now optionally take a set of vertices to do the calculation on.
 - `IGBetweenness(Estimate)` and `IGEdgeBetweenness(Estimate)` now have the `Normalized` option.
 - `IGRandomWalk` now supports edge weights and the `EdgeWeights` option. Use `EdgeWeights -> None` to ignore weights, and thus restore the previous behaviour.
 - Clustering coefficient functions now support the `"ExcludeIsolates"` option.

Incompatible changes from IGraph/M 0.3:

 - A flat namespace structure is used. Functions from ``IGraphM`Utilities` `` have been moved to ``IGraphM` ``.
 - Renamed `"MultipleEdges"` option to `MultiEdges` for convenient typing and auto-completion.
 - Renamed `IGMinSeparators` to `IGMinimumSeparators`.
 - Renamed `IGMakeLattice` to `IGSquareLattice`. The name `IGMakeLattice` works, but it is deprecated.

Other changes:

 - Improved compatibility with Mathematica 11.2, 11.3 and 12.0; handling of `TwoWayRule` as an edge specification.
 - Bug fixes, performance improvements, documentation updates, and general polish.

Notes:

 - IGraph/M 0.4 will be the last version of IGraph/M to support Mathematica 10.0. Starting with IGraph/M 0.5, it will only work [with versions of Mathematica that are still supported by Wolfram Research](http://support.wolfram.com/kb/22107).

##### v0.3.0

 - Compatibility with *Mathematica* 11.1.
 - Additional random graph generators (`IGBarabasiAlbertGame`, `IGStaticFitnessGame`, `IGStaticPowerLawGame`, `IGGeometricGame`, `IGGrowingGame`, `IGForestFireGame`).
 - Bipartite graph layout, `IGLayoutBipartite`.
 - Performance improvements in `IGMaximalCliqueSizeCounts`.
 - `IGEccentricity` and `IGRadius`.
 - `IGVertexContract`, to contract multiple vertex sets simultaneously.
 - `IGRandomWalk`, to take a random walk on a graph.
 - ``IGraphM`Utilities` `` package.
 - Significantly improved package loading performance.
 - Bug fixes and polish.

##### v0.2.2

This is a bugfix release with several minor fixes.  Important changes to be aware of:

 - VF2 isomorphism functions now work when *both* vertex and edge colours are specified at the same time.
 - `IGBlissCanonicalGraph` is now suitable for filtering isomorphic duplicates. Its output may be different from previous releases.

##### v0.2.1

This is a bugfix release.  The following changes require special mention:

 - `IGFeedbackArcSet` could return wrong results for some graphs. This is now fixed.
 - The `"RemovedEdges"` property returned by `IGCommunitiesEdgeBetweenness` could be incorrect for some graphs.  This is now fixed.  Other properties were not affected.
 - The Hierarchical Clustering package is no longer auto-loaded by IGraph/M to avoid shadowing `DistanceMatrix`, a new built-in function added in Mathematica 10.3.  Load this package manually (``<<HierarchicalClustering` ``) to work with the `"HierarchicalClusters"` property of `IGClusterData` objects.

A number of other small bugs were also fixed.

##### v0.2.0

- Significant performance improvements for many functions
- New and extended functions for shortest path calculations (extended `IGDistanceMatrix`, `IGDistanceCounts`, `IGDistanceHistogram`, `IGDiameter`, `IGFindDiameter`)
- Support for weighted clique calculations (`IGWeightedCliques`, `IGMaximalWeightedCliques`, `IGLargestWeightedCliques`, `IGWeightedCliqueNumber`)
- Additional new functions
- Syntax highlighting for functions
- Raspberry Pi support
- Compatibility with Mathematica 10.4 on OS X.
- Bug fixes

## License

IGraph/M is licensed under the [GNU GPLv2](http://opensource.org/licenses/GPL-2.0). See [LICENSE.txt](LICENSE.txt) for details.

 [ltemplate]: https://github.com/szhorvat/LTemplate/
 [main]: http://szhorvat.net/mathematica/IGraphM
 [chat]: https://gitter.im/IGraphM/Lobby
