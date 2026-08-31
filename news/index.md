# Changelog

## phylocomr 0.3.4

CRAN release: 2023-04-21

- Maintenance release to:
  - fix CRAN issues related to compiler warnings `[-Warray-parameter=]`,
    `[-Wstringop-overflow=]`, and `[-Wstringop-truncation]`

## phylocomr 0.3.3

CRAN release: 2022-12-05

- Maintenance release to:
  - fix CRAN issues related to compiler warnings
  - add new maintainer info
    ([\#33](https://github.com/ropensci/phylocomr/issues/33))

## phylocomr 0.3.2

CRAN release: 2019-12-20

#### MINOR IMPROVEMENTS

- move readme image into man/figures
  ([\#30](https://github.com/ropensci/phylocomr/issues/30))

#### BUG FIXES

- fix for gcc `-fno-common`
  ([\#29](https://github.com/ropensci/phylocomr/issues/29))
  ([\#31](https://github.com/ropensci/phylocomr/issues/31))

## phylocomr 0.3.0

CRAN release: 2019-11-25

#### NEW FEATURES

- via ([\#26](https://github.com/ropensci/phylocomr/issues/26)) (see
  below) - we now no longer use file paths passed in directly to
  functions, but instead write to temporary files to run with Phylocom
  so that we do not alter at all the users files. We note this in the
  README and package level manual file
  [`?phylocomr`](https://docs.ropensci.org/phylocomr/reference/phylocomr-package.md)
- package gains new manual file `?phylocomr-inputs` that details the
  four types of inputs to functions and what format they are expected
  in, including how they differ for passing in data.frame’s vs. file
  paths ([\#28](https://github.com/ropensci/phylocomr/issues/28))

#### BUG FIXES

- for all data.frame traits inputs to fxns, check that the first column
  is called `name` (Phylocom doesn’t accept anything else)
  ([\#27](https://github.com/ropensci/phylocomr/issues/27))
- fix was originally for
  [`ph_aot()`](https://docs.ropensci.org/phylocomr/reference/ph_aot.md),
  but realized this touches almost all functions: Phylocom is case
  sensitive. We were already making sure all taxon names in phylogenies
  (tips and nodes) were lowercased, and were lowercasing names in tables
  passed in, but were not fixing case in file paths passed in by the
  user. Now across all functions we make sure case is all lowercase for
  taxon names in any user inputs, so case problems should no longer be
  an issue. ([\#26](https://github.com/ropensci/phylocomr/issues/26))
  via [@Jez-R](https://github.com/Jez-R)

## phylocomr 0.2.0

CRAN release: 2019-11-13

#### BUG FIXES

- two fixes for
  [`ph_bladj()`](https://docs.ropensci.org/phylocomr/reference/ph_bladj.md): 1)
  now we lowercase the taxon name column in the ages data.frame before
  writing the data.frame to disk to avoid any mismatch due to case (we
  write the phylogeny to disk with lowercased names); 2) bladj expects
  the root node name from the phyologeny to be in the ages file; we now
  check that ([\#25](https://github.com/ropensci/phylocomr/issues/25))

## phylocomr 0.1.4

CRAN release: 2019-07-24

#### BUG FIXES

- fix examples ([\#22](https://github.com/ropensci/phylocomr/issues/22))
- improve class checks in internal code, swap `inherits` for `class`
  ([\#23](https://github.com/ropensci/phylocomr/issues/23))
- small fix to use of fread in C lib; check that fread worked, and if
  not if it was an EOF error or other error
  ([\#24](https://github.com/ropensci/phylocomr/issues/24))

## phylocomr 0.1.2

CRAN release: 2018-11-29

#### MINOR IMPROVEMENTS

- fixes for failed checks on Solaris

#### BUG FIXES

- fix to internals of all functions that handle a phylogeny.
  `ph_phylomatic` was working fine with very simple trees in all
  lowercase. we now internally lowercase all node and tip labels, on any
  phylogeny inputs (phylo object, newick string, file path (read, then
  re-write back to disk)). phylomatic wasn’t working with any uppercase
  labels.

## phylocomr 0.1.0

CRAN release: 2018-11-19

#### NEW FEATURES

- released to CRAN
