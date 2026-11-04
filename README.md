# semver

Versions for [Moth](https://github.com/samyyy2423/moth), as [Semantic Versioning 2.0.0](https://semver.org/)
writes them (`1.4.0`, `2.0.0-beta.2`, `1.0.0+build.7`): read, compared, sorted and moved on, and ranges
such as `^1.2.0`, `~1.4`, `>=1.0.0 <2.0.0` and `1.x` checked the way npm checks them. In Moth alone, so it
works everywhere Moth does.

```text
moth add semver
```

```
use the package "semver" as semver

set v to semver's parse("1.4.0-beta.2+build.7")
say v " " v's major " " v's minor " " v's patch " " v's pre     # 1.4.0-beta.2+build.7 1 4 0 ["beta", "2"]

say semver's compare("1.10.0", "1.9.3")                         # 1: the first is newer (-1 older, 0 the same)
say semver's sorted(list of "1.10.0", "1.2.0", "1.9.3")          # ["1.2.0", "1.9.3", "1.10.0"]
say semver's bump("1.4.2", "minor")                             # 1.5.0
say semver's satisfies("1.8.0", "^1.2.0")                       # yes
say semver's newest_satisfying((list of "1.2.0", "1.2.7", "1.9.0", "2.1.0"), "~1.2")   # 1.2.7
```

## What it has

| | |
|---|---|
| `parse(words)` | a version: a record of the kind `"version"`, with its `major`, `minor` and `patch` numbers, `pre` (the pre-release's parts, as text) and `build`, shown as it's written. A `v` or `=` in front, as git tags have, is fine; anything else that isn't a version fails, saying why |
| `is_valid(words)` | whether `parse` can read it |
| `written(v)` | the version as text |
| `compare(a, b)` | -1, 0 or 1, as the standard orders versions: `1.0.0-alpha` < `1.0.0-alpha.1` < `1.0.0-beta` < `1.0.0-beta.11` < `1.0.0-rc.1` < `1.0.0`, and a build is left out |
| `is_newer(a, b)`, `is_older(a, b)`, `is_same(a, b)` | the same, as yes or no |
| `sorted(versions)`, `newest(versions)` | oldest first; the newest, or `nothing` |
| `bump(v, part)` | the next version: `"major"`, `"minor"`, `"patch"` or `"pre"` (`1.3.0-beta.1` to `1.3.0-beta.2`) |
| `satisfies(v, range)` | whether the version is in the range |
| `newest_satisfying(versions, range)`, `oldest_satisfying(versions, range)` | the newest or oldest in the range, or `nothing` |
| `is_valid_range(words)` | whether `satisfies` can read it |

Anything that takes a version takes the record or its text.

## Ranges

| | |
|---|---|
| `1.2.3`, `=1.2.3` | that version |
| `>1.2.3`, `>=1.2.3`, `<2.0.0`, `<=2.0.0` | compared |
| `^1.2.3` | up to the next major: `>=1.2.3 <2.0.0` (`^0.2.3` is up to `0.3.0`, `^0.0.3` only `0.0.3`) |
| `~1.2.3` | up to the next minor: `>=1.2.3 <1.3.0` |
| `1.2.x`, `1.2`, `1.x`, `1`, `*`, "" | what's missing can be anything |
| `1.2.3 - 2.0.0` | everything between, both ends in |
| `>=1.0.0 <2.0.0` | comparisons with spaces between must all hold |
| `^1.0.0 \|\| ^2.0.0` | either side |

A pre-release such as `1.3.0-beta` only satisfies a range that names a pre-release of the same major, minor
and patch (`>=1.3.0-alpha`), as npm has it, so `^1.2.0` doesn't pick up a beta by mistake.

## Tests

```text
moth test semver.test.moth
```

## License

Apache License 2.0, as Moth is: see [LICENSE](LICENSE).
