# Executables

Executables

## Usage

``` r
ecovolve(args = "--help", intern = FALSE)

phylocom(args = "help", intern = FALSE)

phylomatic(args = "--help", intern = FALSE)
```

## Arguments

- args:

  a character vector of arguments to command.

- intern:

  capture output as character vector. Default: `FALSE`

## Examples

``` r
ecovolve()
#> 
#>     ======================================================
#> 
#>                        E C O V O L V E
#>     PHYLOGENY AND TRAIT SIMULATOR WITH ECOLOGICAL PROCESSES
#>                 Cam Webb <campbell.webb@yale.edu>
#> 
#>     ======================================================
#> 
#> Usage: ecovolve [-s -e -t -m -c -p -l -d -h]
#> 
#> Switches:
#>   -s    prob of speciation per unit time[0.05]
#>   -e    prob of extinction per unit time[0.01]
#>   -t    time units to simulate over [100]
#>   -m    output mode (2 = LTT; 3 = newick) [3]
#>   -c    prob envelope for char change, string of 10 ints [3211000000]
#>   -l    stop simulation after this number of extant lineages [NO]
#>   -p    output phylogeny pruned only for extant taxa [NO]
#>   -d    taper character change by exp(-time / INT )
#>   -x    simulate competition, with trait proximity increasing extinction
#>   -h    help
#> 
#> View it!:
#>   $ ecovolve > ecovolve.phylo
#>   $ phylocom makenex -f ecovolve.phylo -t ecovolve.traits \
#>              -s ecovolve.sample > ecovolve.nex
#>   (open and trace character in Mesquite with BLs proportional)
#> 
phylocom()
#> 
#>   ==========================================================
#> 
#>                        P H Y L O C O M
#>         PHYLOGENETIC ANALYSIS OF ECOLOGICAL COMMUNITIES
#>           AND SPECIES TRAITS, WITH PHYLOGENETIC TOOLS
#>                 Cam Webb <cwebb@oeb.harvard.edu>,
#>                 David Ackerly <dackerly@berkeley.edu>,
#>                 Steve Kembel <skembel@uoregon.edu>
#> 
#>   ==========================================================
#> 
#>   Version 4.2.  Copyright (c) 2001-2009 Webb, Ackerly and Kembel
#>   This program comes with ABSOLUTELY NO WARRANTY.  This is free software,
#>   and you are welcome to redistribute it under certain conditions;
#>   type `phylocom license' for warranty, license and citation information.
#> 
#> Usage: phylocom method [ options ]
#> 
#> Phylogenetic community structure (intrasample):
#>   comstruct: Calculates mean phylogenetic distance and mean nearest
#>             taxon phylogenetic distance for each plot and all plots, and
#>             compares them to [999] random runs with matrices created by
#>             the null model specified by the -m switch.
#>             Alter # runs with a second argument.
#>   swap:     Swaps the input matrix 1000 times using the Independent Swap
#>             Algorithm (independent swap) and outputs swapped matrix console.
#>   pd:       Faith's Phylogenetic diversity; proportion of total branch length.
#>   ltt:      Prints the number of lineages at fixed divs of total time
#>             proportional to no. of tip taxa.  Phylo must be
#>             ultrametric. [lttr compares with random samples.]
#>   nodesig:  Tests each node for overabundance of terminal taxa.
#>   nodesigl: As nodesig, but output in table form.
#> 
#> Phylogenetic community structure (intersample):
#>   comdist:  Outputs the pairwise distance matrix between plots, based on mean
#>             phylogenetic distance of all possible pairs of taxa in one plot
#>             to the taxa in the other. [comdistnt uses nearest taxon method.]
#>   icomdist: Gives the mean distance from each taxon to taxa in other plots.
#>   rao     : Calculates Rao's entropy measure
#> 
#> Community trait structure:
#>   comtrait: Measures of trait value clustering and evenness within samples
#> 
#> Phylogenetic trait analysis:
#>   aot:      Trait analysis algorithms of David Ackerly
#>   aotn:     Trait analysis algorithms (Nexus output)
#>   aotf:     Trait analysis algorithms (full output)
#> 
#> Misc Phylogeny Tools:
#>   phydist:  Calculates the simple pairwise matrix of phylogenetic distances.
#>   phyvar:   Output phylogenetic variance-covariance matrix.
#>   agenode:  Output the ages of each node, calculated from BLs.
#>   ageterm:  Output the stem age of terminal taxon.
#>   makenex:  Convert all data files into a Mesquite-readable Nexus
#>   naf:      Convert all data files into a node-as-factor table
#>   new2nex:  Convert the phylo file into a Mesquite-readable Nexus
#>   new2fy:   Convert the phylo file into a tabular list of nodes
#>   bladj:    Branch length adjust alg.  Needs an 'ages' file.
#>   comnode:  Find common nodes between two trees of different size.
#>   sampleprune: Prunes the phylo for each list of taxa in sample.
#>   rndprune: Prunes the phylo [-r n] times, using [-p n] terminals.
#>   cleanphy: Prunes `empty' internal nodes (e.g., output from phylomatic)
#>   version:  Print software version.
#>   help:     This file
#> 
#> Options:
#>   -r INT    Number of randomizations to use [999].
#>   -m INT    Swap method to use with comstruct/swap: [2].
#>               0 = Shuffle phylogeny taxa labels.
#>               1 = Sample taxa become random draws from sample pool.
#>               2 = Sample taxa become random draws from phylogeny pool.
#>               3 = Shuffle sample using independent swap.
#>               4 = Shuffle sample using trial swap.
#>   -w INT    Number of swaps/trials per run (for independent/trial swap) [1000].
#>   -b INT    Number of burnin swaps/trials (for independent/trial swap) [0].
#>   -f filename   Use this file as the phylogeny file [phylo].
#>   -s filename   Use this file as the sample file [sample].
#>   -t filename   Use this file as the traits file [traits].
#>   -p INT    Number of taxa to include in pruned tree [5].
#>   -n        Add default internal node names (aot) or
#>             enable null model testing (comdist/comdistnt)
#>   -y        Output phylogeny in `fy` format (selected functions)
#>   -e        Suppress output of branch lengths.
#>   -a        Use the abundances in the sample file (otherwise just pres/abs).
#>   -v        Verbose output of raw MPD/MNTD values (comstruct).
#> 
phylomatic()
#> 
#>     ======================================================
#> 
#>                      P H Y L O M A T I C
#>                 Cam Webb <cwebb@oeb.harvard.edu>
#> 
#>     ======================================================
#> 
#> Usage: phylomatic [-t -f -n -h -l]
#> 
#> Switches:
#>   -t FILENAME use this as the taxa file
#>   -f FILENAME use this as the phylo file
#>   -n    label all nodes with default names
#>   -h    Help information
#>   -y    Output a tabular representation of phylogeny
#>   -l    Convert all chars in taxa file to lowercase
#> 
```
