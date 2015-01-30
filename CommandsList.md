# Chocolatey List (choco list / clist) / Search (choco search)

Chocolatey will perform a search for a package local or remote. Some folks will shorten with `clist` as in `clist filter`

## Usage



    choco search filter [options/switches]
    choco list filter [options/switches]

## Examples

    choco list --local-only
    choco list -lp
    choco list -lap
    choco search git
    choco search git -s "https://somewhere/out/there"

## Options and Switches

Includes [[default options/switches|CommandsReference#default-options-and-switches]]

```
-s, --source=VALUE
  Source - Source location for install. Can include special 'webpi'.
  Defaults to sources.

-l, --lo, --localonly, --local-only
  LocalOnly - Only search against local machine items.

-p, --includeprograms, --include-programs
  IncludePrograms - Used in conjuction with LocalOnly, filters out apps
  chocolatey has listed as packages and includes those in the list.
  Defaults to false.

-a, --all, --allversions, --all-versions
  AllVersions - include results from all versions.
```

## Alternative Sources
**NOTE**: THIS IS NOT YET REIMPLEMENTED.

### WebPI
This specifies the source is Web PI (Web Platform Installer) and that we
are searching for a WebPI product, such as IISExpress. If you do not
have the Web PI command line installed, it will install that first and
then perform the search requested.
e.g. `choco list --source webpi`

### Windows Features
This specifies that the source is a Windows Feature and we should
install via the Deployment Image Servicing and Management tool (DISM) on
the local machine.
e.g. `choco list --source windowsfeatures`
