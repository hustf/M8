# M8

M8 is a limited domain Julia package registry. Package registries are used by Julia's
package manager [Pkg.jl][pkg] after calling `registry add https://github.com/hustf/M8`. The registry 
includes information about packages such as versions, dependencies and compatibility constraints.

The M8 registry is open for everyone to use and provides access to a curated ecosystem
of packages specialized on physics, mechanical engineering and documentation.

You can register new packages by making a pull request here.

## Registering a package in General

It is ***highly recommended*** to use [TagBot][tagbot], which automatically tags a release in your
repository after the new release of your package is merged into the registry.

Registered packages in general have an [Open Source Initiative approved license](https://opensource.org/licenses), clearly marked via a `LICENSE.md`, `LICENSE`, `COPYING` or similarly named file in the package repository.

### Automatic merging of pull requests

The following criteria are applied for all pull requests
(regardless if it is a new package or just a new version):

 - Version number: Should be a standard increment and not skip versions, unless the package
   is a forked adaption of other packages. This means incrementing the patch/minor/major version with +1 compared to previous (if any) releases. If, for example, `1.0.0` and `1.1.0` are existing versions, valid new versions are `1.0.1`, `1.1.1`, `1.2.0` and `2.0.0`. Invalid new versions include
   `1.0.2` (skips `1.0.1`), `1.3.0` (skips `1.2.0`), `3.0.0` (skips `2.0.0`) etc.

 - Dependencies: All dependencies should have `[compat]` entries that are upper bounded and only include a finite number of breaking releases.
   For example, the following `[compat]` entries meet the version guideline:
   ```toml
   [compat]
   PackageA = "1"          # [1.0.0, 2.0.0), has upper bound (good)
   PackageB = "0.1, 0.2"   # [0.1.0, 0.3.0), has upper bound (good)
   ```
   The following `[compat]` entries do NOT meet the guideline, and are vulnerable to breaking changes in dependencies:
   ```toml
   [compat]
   PackageC = ">=3"        # [3.0.0, ∞), no upper bound (bad)
   PackageD = ">=0.4, <1"  # [-∞, ∞), no lower bound, no upper bound (very bad)
   ```
   Please note: each `[compat]` entry must include only a finite number of breaking releases. Therefore, the following `[compat]` entries do NOT meet the guideline:
   ```toml
   [compat]
   PackageE = "0"          # includes infinitely many breaking 0.x releases of PackageE (bad)
   PackageF = "0.2 - 0"    # includes infinitely many breaking 0.x releases of PackageF (bad)
   PackageG = "0.2 - 1"    # includes infinitely many breaking 0.x releases of PackageG (bad)
   ```
   See [Pkg's documentation][pkg-compat] for specification of `[compat]` entries in your
   `Project.toml` file.
   
   You may find [CompatHelper.jl](https://github.com/bcbi/CompatHelper.jl) helpful for maintaining up-to-date `[compat]` entries.

 - Package installation: The package should be installable (`Pkg.add("PackageName")`),
   and loadable (`import PackageName`).
   
 - License: The package in general should have an
   [OSI-approved software license](https://opensource.org/licenses/alphabetical) located in
   the top-level directory of the package code, e.g. in a file named `LICENSE` or `LICENSE.md`.

The following list is applied for new package registrations, in addition to the previous
list:

 - The package name should start with a capital letter, contain only ASCII
   alphanumeric characters, contain a lowercase letter, be at least 5
   characters long, and should not start with "Ju" or contain the string "julia".
 - To prevent confusion between similarly named packages, the names of new
   packages must also satisfy three checks:
      - the [Damerau–Levenshtein
        distance](https://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance)
        between the package name and the name of any existing package must be at
        least 3.
      - the Damerau–Levenshtein distance between the lowercased version of a
        package name and the lowercased version of the name of any existing
        package must be at least 2.
      - and a visual distance from
        [VisualStringDistances.jl](https://github.com/ericphanson/VisualStringDistances.jl)
        between the package name and any existing package must exceeds a certain
        a hand-chosen threshold (currently 2.5).
