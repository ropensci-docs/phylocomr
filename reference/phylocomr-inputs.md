# Expected inputs

Expected inputs

## Ages data

A file or data.frame, with 2 columns:

- (character) node taxonomic name

- (numeric) age estimate for the node

If a file path is used, the table must not have headers

Applies to:

- [`ph_bladj()`](https://docs.ropensci.org/phylocomr/reference/ph_bladj.md)

## Sample data

A file or data.frame, with 3 columns, sorted by column 1, one row per
taxon:

- (character) sample plot/quadrat/trap/etc. name (no spaces, must begin
  with a letter, not a number or symbol)

- (integer) abundance (leave as 1 for presence/absence data)

- (character) species code (same as in the phylogeny, must begin with a
  letter, not a number or symbol)

If a file path is used, the table must not have headers, and must be
tab-delimited

Applies to:

- [`ph_comtrait()`](https://docs.ropensci.org/phylocomr/reference/ph_comtrait.md)

- [`ph_comstruct()`](https://docs.ropensci.org/phylocomr/reference/ph_comstruct.md)

- [`ph_comdist()`](https://docs.ropensci.org/phylocomr/reference/ph_comdist.md)

- [`ph_pd()`](https://docs.ropensci.org/phylocomr/reference/ph_pd.md)

- [`ph_rao()`](https://docs.ropensci.org/phylocomr/reference/ph_rao.md)

## Traits data

A tab-delimited file with the first line as
`type<TAB>n<TAB>n<TAB>... [up to the number of traits]`, for example:
`type<TAB>3<TAB>3<TAB>3<TAB>0`

where n indicates the type of trait in each of the four columns. Types:

- `0`: binary (only one binary trait may be included, and it must be in
  the first column) 1 for unordered multistate (no algorithms currently
  implemented)

- `2`: ordered multistate (currently treated as continuous)

- `3`: continuous

Optional: The second line can start with the word name (lower case only)
and then list the names of the traits in order. These will appear in the
full output file

Subsequent lines should have the taxon name, which must be identical to
its appearance in phylo, and the data columns separated by tabs. For
example: `sp1<TAB>1<TAB>1<TAB>1<TAB>0`

- OR -

A data.frame, where the first column called `name` has each taxon,
followed by any number of columns with traits. The first column name
must be `name`, and the following columns should be named using the name
of the trait.

Applies to:

- [`ph_comtrait()`](https://docs.ropensci.org/phylocomr/reference/ph_comtrait.md)

- [`ph_aot()`](https://docs.ropensci.org/phylocomr/reference/ph_aot.md)

## Phylogenies

Phylocom expects trees in Newick format. The basic Newick format used by
Phylocom is: `((A,B),C);`. See the Phylocom manual
(https://phylodiversity.net/phylocom/) for more details on what they
expect.

Applies to: all functions except
[`ph_phylomatic()`](https://docs.ropensci.org/phylocomr/reference/ph_phylomatic.md)
and
[`ph_ecovolve()`](https://docs.ropensci.org/phylocomr/reference/ph_ecovolve.md)
