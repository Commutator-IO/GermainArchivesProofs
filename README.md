# Lean proofs of Sophie Germain's archives

Lean 4 proofs, built on [Mathlib](https://github.com/leanprover-community/mathlib4), of statements
read in Sophie Germain's manuscripts at the Bibliothèque nationale de France, digitised in
Gallica. The statements are the modernised readings of the edition
[germain-project](https://github.com/Commutator-IO/germain-project)
([germain.commutator.io](https://germain.commutator.io)), where each file is linked to its volume,
its views and its findings.

The files are in topic directories, as in Mathlib, and keep the shelfmark (cote) of their
volume in their name:

- `Germain/NumberTheory/Naf4073Note.lean`: NAF 4073, f. 32v (Gallica view 42), the note
  « le triangulaire d'un quarré est la somme des quarrés des deux Triangulaires consécutifs »,
  that is `T_{n−1}² + T_n² = T_{n²}`; Germain's difference `T_n² − T_{n−1}² = n³` and
  Nicomachus' identity; the line `1 + 64 … 189` under the formula.
- `Germain/NumberTheory/Naf4073Triangular.lean`: `T_{mn} = T_m T_n + T_{m−1} T_{n−1}`, the
  triangular number of a power of `n`, the steps `T_{b²} − T_{a²}`, and the differences and sums
  `T_{n+1}^k ∓ T_n^k`.
- `Germain/Algebra/BigOperators/Naf4073PowerSums.lean`: the triangular system
  `2^(k−1) T_n^k = ∑ C(k, m) S_{k+m}`, which gives the sums of odd powers as polynomials in `T_n`.
- `Germain/NumberTheory/Naf4073Gersonides.lean`: the uniqueness of Lagrange's formula
  (`S_p = T_n^q` for all `n` only for `(1, 1)` and `(3, 2)`), through Levi ben Gershon's
  `2^p + 1 = 3^q`.

The last three prove the general statements of the second part of the short article on the note
(`docs/naf-4073/article.tex` in germain-project). All declarations are in the namespace
`Germain.Naf4073`.

`import GermainArchivesProofs` brings in all four. Lean's kernel checks every proof; the build treats
warnings as errors, so a `sorry` fails it.

## Versions

- `lean-toolchain`: Lean `v4.35.0-rc4` (elan installs it on the first `lake` command);
- `lake-manifest.json`: the exact commits of every dependency, Mathlib `715a03a` and its own
  dependencies; `lakefile.toml` names Mathlib.

These are the pins of germain-project's `lean/` directory, where the same proofs are kept.

## Building

From a clean clone, with [elan](https://github.com/leanprover/elan) installed:

```
lake exe cache get      # Mathlib's compiled files, from its cache
lake build
lake env lean --run scripts/CheckAxioms.lean GermainArchivesProofs GermainArchivesProofs Germain
```

The last command walks the dependencies of every constant of this repository, through Mathlib,
and fails on any axiom other than `propext`, `Classical.choice` and `Quot.sound`. The continuous
integration (`.github/workflows/build.yml`) runs the three commands on every push to `main` and
on pull requests.

## Conventions

As in [GrothendieckArchivesProofs](https://github.com/Commutator-IO/GrothendieckArchivesProofs):
one file per shelfmark (cote) of the Gallica manuscripts, named after it; module docstrings give
the source (volume, folio, Gallica view) and list the main results; a statement that the
manuscript does not contain says so.

## Licence

[Apache 2.0](LICENSE). The same proofs are under CC0 in germain-project. Short quotations of the
manuscripts in comments, in « », are from documents in the public domain; the digitisations are
the Bibliothèque nationale de France's (source gallica.bnf.fr / BnF).
