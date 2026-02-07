# Scratchd

`Scratchd` - CLI app to manipulate temporary files and workspaces under user homedir

## Core concept

`scratchd` allow to create temporary workspace. This will be folder at
`$TMP_RESERVED_LOCATION/scratchd/$WS_NAME`.
User can quick - create, rm, list those workspaces.

In workspace folder - no dev files by `scratchd`, everything there created
and managed by user.

`scratchd` also provides shortcuts for jumping into workspace or manage it somehow.
More details in [Scenarios](#scenarios)

## Scenarios

In case of CLI - each scenario is CLI command or subcommands

### Create

Creating workspace

```shell
sd c <NAME> # Alias
sd create <NAME> # Full version
```

Here important things happens:

* `scratchd` creates new workspace under `$TMP_RESERVED_LOCATION/scratchd`
named `<NAME>`
* Via shell integration (like `worktrunk` have - [doc](https://worktrunk.dev/config/#shell-integration))
`scratchd` jumps in shell to workspace location automatically
* Prints to user workspace name and location

Expected output and behaviour:

```shell
sd c demo

sd: Creating workspace demo
sd: Workspace created
sd: Switching cwd
**sd jumps to workspace location**
```

Flags to command(form of full version and shortcut):

* `no-jump,nj` -  do not switch cwd
* `copy,c` - copies paths into workspace.
`c=.` - copies current folder; `c=README.md` copies only `README.md` file

### Select

Table list prompt of workspaces, where user can choose workspaces

```shell
sd s # Alias
sd switch # Full version
```

Expected menu:

```shell
Select workspace to jump with <Enter>:
  NAME   PATH                                   EMPTY
  ws-1   $TMP_RESERVED_LOCATION/scratchd/ws-1   Yes
* ws-2   $TMP_RESERVED_LOCATION/scratchd/ws-2   No
  ws-3   $TMP_RESERVED_LOCATION/scratchd/ws-3   Yes
```

* `*` - means selected workspace
* User hits Enter and jumps to workspace (`scratchd` handles this seamlessly
because of shell-integration)

### Remove

Removes workspace. Opens same prompt as [Select](#select)

```shell
Remove workspace with <Enter>; Trash with <Space>
  NAME   PATH                                   EMPTY
  ws-1   $TMP_RESERVED_LOCATION/scratchd/ws-1   Yes
* ws-2   $TMP_RESERVED_LOCATION/scratchd/ws-2   No
  ws-3   $TMP_RESERVED_LOCATION/scratchd/ws-3   Yes
```

Notes:

* `<Enter>` removes workspace permanently
* `<Space>` moves it to trash

### List

Listing created workspaces

```shell
sd ls # Alias
sd list # Full version
```

Prints list of workspaces as table (here i specify as yaml for simplicitly)

```yaml
- name: ws-1
  path: $TMP_RESERVED_LOCATION/scratchd/ws-1
- name: ws-2
  path: $TMP_RESERVED_LOCATION/scratchd/ws-2
```

## Functional requirements

* `scratchd` installed as shortcut `sd`. So in CLI it will be used as `sd`

## Non-functional requirements

### `$TMP_RESERVED_LOCATION`

On linux/macos it should be `$HOME/.cache`. So full path to workspaces will be `$HOME/.cache/scratchd`

### Tech stack

* Golang - primary language. Latest version at project initialization
* golangci-lint - golang linter. Latest version at project initialization
* mise - toolchain setup and local automation
* goreleaser - release to github and homebrew tap. Latest version at project initialization

### Workspaces location structure

```
$TMP_RESERVED_LOCATION/scratchd/
  ws-1/<USER FILES AND FOLDER>
  ws-2/<USER FILES AND FOLDER>
```
