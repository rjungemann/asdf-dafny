# asdf-dafny

[asdf](https://asdf-vm.com) / [mise](https://mise.jdx.dev) plugin for
[dafny](https://github.com/dafny-lang/dafny), the verification-aware
programming language and compiler maintained by dafny-lang.

Installs the official prebuilt release archives published on the
[dafny releases page](https://github.com/dafny-lang/dafny/releases).
Prebuilt binaries are only published for:

- macOS x64 and arm64
- Linux x64 (no Linux arm64 builds are published upstream)

## Install

### asdf

```shell
asdf plugin add dafny https://github.com/rjungemann/asdf-dafny.git
asdf install dafny latest
asdf global dafny latest
```

### mise

mise can use asdf plugins directly:

```shell
mise plugin install dafny https://github.com/rjungemann/asdf-dafny.git
mise use --global dafny@latest
```

Or in a project `.mise.toml` / `.tool-versions`:

```toml
[tools]
dafny = "latest"
```

## Usage

```shell
dafny --version
dafny verify MyProgram.dfy
```

## Testing locally

With asdf:

```shell
asdf plugin test dafny "$(pwd)" "dafny --version" --asdf-tool-version latest
```

With mise (note the `file://` prefix — a bare path is not accepted):

```shell
mise plugins install dafny "file://$(pwd)"
mise install dafny@4.11.0
mise x dafny@4.11.0 -- dafny --version
```

## How it works

- `bin/list-all` lists released versions from the GitHub Releases API
  (filtered to plain `X.Y.Z` tags with published binaries).
- `bin/download` picks the release asset matching the current OS/arch and
  extracts it.
- `bin/install` copies the extracted `dafny/` directory into
  `<install-path>/bin`, preserving the layout dafny expects (it locates its
  bundled `z3` solver via a path relative to the `dafny` executable).

Set `GITHUB_API_TOKEN` to avoid GitHub API rate limits.

## License

MIT, see [LICENSE](LICENSE).
