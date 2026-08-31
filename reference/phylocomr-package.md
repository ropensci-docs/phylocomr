# Phylocom interface

`phylocomr` gives you access to Phylocom, specifically the Phylocom C
library (https://github.com/phylocom/phylocom/), licensed under BSD
2-clause (http://www.opensource.org/licenses/bsd-license.php)

## Details

This package isn't doing system calls to a separately installed Phylocom
instance - but actually includes Phylocom itself in the package.

Phylocom is usually used either on the command line or through the R
package picante, which has duplicated some of the Phylocom
functionality.

In terms of performance, some functionality will be faster here than in
`picante`, but the maintainers of `picante` have re-written some
Phylocom functionality in C/C++, so performance should be similar in
those cases.

## A note about files

As a convenience you can pass ages, sample and trait data.frame's, and
phylogenies as strings, to `phylocomr` functions. However, `phylocomr`
has to write these data.frame's/strings to disk (your computer's file
system) to be able to run the Phylocom code on them. Internally,
`phylocomr` is writing to a temporary file to run Phylocom code, and
then the file is removed.

In addition, you can pass in files instead of data.frame's/strings.
These are not themselves used. Instead, we read and write those files to
temporary files. We do this for two reasons. First, Phylocom expects the
files its using to be in the same directory, so if we control the file
paths that becomes easier. Second, Phylocom is case sensitive, so we
simply standardize all taxon names by lower casing all of them. We do
this case manipulation on the temporary files so that your original data
files are not modified.

## Package API

- [`ecovolve()`](https://docs.ropensci.org/phylocomr/reference/executables.md)/[`ph_ecovolve()`](https://docs.ropensci.org/phylocomr/reference/ph_ecovolve.md) -
  interface to `ecovolve` executable, and a higher level interface

- [`phylomatic()`](https://docs.ropensci.org/phylocomr/reference/executables.md)/[`ph_phylomatic()`](https://docs.ropensci.org/phylocomr/reference/ph_phylomatic.md) -
  interface to `phylomatic` executable, and a higher level interface

- [`phylocom()`](https://docs.ropensci.org/phylocomr/reference/executables.md) -
  interface to `phylocom` executable

- [`ph_aot()`](https://docs.ropensci.org/phylocomr/reference/ph_aot.md) -
  higher level interface to `aot`

- [`ph_bladj()`](https://docs.ropensci.org/phylocomr/reference/ph_bladj.md) -
  higher level interface to `bladj`

- [`ph_comdist()`](https://docs.ropensci.org/phylocomr/reference/ph_comdist.md)/[`ph_comdistnt()`](https://docs.ropensci.org/phylocomr/reference/ph_comdist.md) -
  higher level interface to comdist

- [`ph_comstruct()`](https://docs.ropensci.org/phylocomr/reference/ph_comstruct.md) -
  higher level interface to comstruct

- [`ph_comtrait()`](https://docs.ropensci.org/phylocomr/reference/ph_comtrait.md) -
  higher level interface to comtrait

- [`ph_pd()`](https://docs.ropensci.org/phylocomr/reference/ph_pd.md) -
  higher level interface to Faith's phylogenetic diversity

## Author

Scott Chamberlain

Jeroen Ooms
