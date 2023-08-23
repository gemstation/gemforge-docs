---
icon: terminal
order: -3
---

All Gemforge commands are executed by running `gemforge <command name>` in a terminal.

## Help

To see list of commands use the `--help` option:

```shell
> gemforge --help
Usage: gemforge [options] [command]

Options:
  -V, --version               output the version number
  -h, --help                  display help for command

Commands:
  init [options]              Initialize a gemforge config file for an existing project.
  scaffold [options]          Generate diamond smart contract project scaffolding.
  build [options]             Build a project.
  deploy [options] [network]  Deploy the diamond to a network.
  help [command]              display help for command
```

To see usage help for a particular command type `gemforge <command name> --help`. For example:

```shell
> gemforge init --help
Usage: gemforge init [options]

Initialize a gemforge config file for an existing project.

Options:
  -v, --verbose          verbose logging output
  -q, --quiet            disable logging output
  -f, --folder <folder>  folder to run gemforge in (default: ".")
  -n, --name <name>      name to use for the config file (default: "gemforge.config.cjs")
  -o, --overwrite        overwrite config file if it already exists
  --hardhat              generate config for a Hardhat project
  -h, --help             display help for command
```

## Logging

To get verbose logging output, append `-v` or `--verbose` to any command, e.g:

```shell
gemforge build -v
```

To turn off logging completely, append `-q` or `--quiet`:

```shell
gemforge build -q
```

!!!
Note: Fatal errors are always logged, even if normal logging is turned off.
!!!