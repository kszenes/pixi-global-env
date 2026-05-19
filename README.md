# pixi-env

`pixi-env` adds global conda-style environments to `pixi` via a `pixi env ...` shell wrapper.

All environments are stored under one directory:

- default: `~/.pixi-envs`
- override: set `PIXI_ENV_HOME=/some/directory`

## Install

Install with:

```sh
curl -LsSf https://raw.githubusercontent.com/kszenes/pixi-env/refs/heads/master/install.sh | sh
```

Then enable activation/deactivation and the `pixi env ...` wrapper in your shell (`bash` or `zsh`):

```sh
eval "$(pixi-env shell-init)"
```

Add that line to `~/.bashrc` or `~/.zshrc` to make it permanent.

## Usage

```sh
~ $ pixi-env --help
pixi-env - pixi global environments

Usage:
  pixi-env create -n <name> [package ...]  Create a global pixi environment
  pixi-env add [-n <name>] <package ...>   Add packages to an environment (or active env)
  pixi-env remove -n <name>                Delete an environment
  pixi-env list                            List environments
  pixi-env path [-n <name>]                Print global env root or environment path
  pixi-env activate <name>                 Activate an environment (after shell init)
  pixi-env deactivate                      Deactivate the active environment
  pixi-env run -n <name> -- <command>      Run a command in an environment
  pixi-env shell-init                      Print shell integration for bash/zsh
  pixi-env help                            Show this help

Pixi wrapper after shell-init:
  pixi env create -n <name> [pkg ...]
  pixi env list
  pixi env activate <name>
  pixi env deactivate
  pixi env add [-n <name>] <pkg ...>
  pixi env remove -n <name>
  pixi env path [-n <name>]
  pixi env run -n <name> -- <cmd>


Environment variables:
  PIXI_ENV_HOME                            Directory where environments are stored (default: ~/.pixi-envs)

Install shell integration by adding this to ~/.bashrc or ~/.zshrc:
  eval "$(pixi-env shell-init)"
```

Example use cases:

```sh
pixi env create -n py311 python==3.11
pixi env activate py311
pixi add pandas matplotlib              # adds to active pixi-env env
pixi env add -n py311 pandas matplotlib # explicit env
pixi env run -n py311 -- python -c 'import numpy; print(numpy.__version__)'
pixi env path                           # prints the env root directory
pixi env path -n py311                  # prints one env directory
pixi env remove -n py311
```

`pixi-env` remains available as the backend command if you prefer it:

```sh
pixi-env create -n py311 python=3.11
pixi-env activate py311
```

## Notes

Each global environment is a normal pixi project stored at `$PIXI_ENV_HOME/<name>`. Its default environment prefix is `$PIXI_ENV_HOME/<name>/.pixi/envs/default`.

`pixi env activate` / `pixi-env activate` and `pixi env deactivate` / `pixi-env deactivate` require the shell integration because a subprocess cannot modify the parent shell environment directly.

On activation, `pixi-env` also sets conda-compatible environment variables (`CONDA_PREFIX`, `CONDA_DEFAULT_ENV`, `CONDA_PROMPT_MODIFIER`, `CONDA_SHLVL`) so prompt tools such as Starship can display the active environment.
