# chatgpt-cli

[![test](https://github.com/rcmdnk/chatgpt-cli/actions/workflows/test.yml/badge.svg)](https://github.com/rcmdnk/chatgpt-cli/actions/workflows/test.yml)
[![test coverage](https://img.shields.io/badge/coverage-check%20here-blue.svg)](https://github.com/rcmdnk/chatgpt-cli/tree/coverage)

Python CLI implementation for [ChatGPT](https://openai.com/blog/chatgpt).

## Requirement

- Python 3.9, 3.10, 3.11
- Poetry (For development)

## Installation

```
$ pip install chatgpt-cli
```

## Usage

### Command line interface

```
usage: cg [-h] [-k KEY] [-c CONF] [-m MODEL] [-t TOKENS] [-r RETURN_TOKENS] subcommand [message ...]

positional arguments:
  subcommand            Subcommand to run.
  message               Message to send to ChatGPT

options:
  -h, --help            show this help message and exit
  -k KEY, --key KEY     OpenAI API key.
  -c CONF, --conf CONF  Path to the configuration toml file.
  -m MODEL, --model MODEL
                        ChatGPT Model to use.
  -t TOKENS, --tokens TOKENS
                        The maximum number of tokens to generate in the chat completion. Set 0 to use the
                        max values for the model.
  -r RETURN_TOKENS, --return_tokens RETURN_TOKENS
                        The reserved number of tokens for the return.
```

### Configuration file

#### File path

The default path to the configuration file is **$XDG_CONFIG_HOME/cg/config.toml**.

If **$XDG_CONFIG_HOME** is not defined, use **~/.config/cg/config.toml**.

If it does not exist and **~/.cg/config.toml** or **~/.cg.toml** exists,
the existing file is used.

You can change the path by `-c <file>` (`--conf <file>`) option.

#### How to write the configuration file

The configuration file is written in the TOML format.

Subcommand is defined as the top table name.

The options for each table can be:

- `description`: Description of the command.
- `model`: The model to use. (default: "gpt-3.5-turbo")
- `max_tokens`: The maximum number of tokens to generate in the chat completion. Set 0 to use the max values for the model. (default: 0)
- `temperature`: Sampling temperature (0 ~ 2). (default: 1)
- `top_p`: Probability (0 ~ 1) that the model will consider the top_p tokens. Do not set both temperature and top_p in the same time. (default: 1)
- `presence_penalty`: The penalty for the model to return the same token (-2 ~ 2). (default: 0)
- `frequency_penalty`: The penalty for the model to return the same token multiple times (-2 ~ 2). (default: 0)
- List of `messages`: Dictionary of message, which must have `role` ('system', 'user' or 'assistant') and `content`. You can optionally give `name`.

```
[test_cmd]
description = "Example command to test the OpenAI API."

[[test_cmd.messages]]
role = "system"
content = "You are a helpful assistant."
[[test_cmd.messages]]
role = "user"
content = "Who won the world series in 2020?"
[[test_cmd.messages]]
role = "assistant"
"content" = "The Los Angeles Dodgers won the World Series in 2020."
[[test_cmd.messages]]
role = "user"
content = "Where was it played?"

[shell]
description = "Ask a shell scripting question."
[[shell.messages]]
role = "user"
content = "You are an expert of the shell scripting. Answer the following questions."

[py]
description = "Ask a python programming question."
[[py.messages]]
role = "user"
content = "You are an expert python programmer. Answer the following questions."
```

These messages will be sent as an prompt before your input message.

You can give full questions and use `cg` w/o input messages like a first example `test_cmd`.

## Development

### Poetry

Use [Poetry](https://python-poetry.org/) to setup environment.

To install poetry, run:

```
$ pip install poetry
```

or use `pipx` (`x` is `3` or anything of your python version).

Setup poetry environment:

```
$ poetry install
```

Then enter the environment:

```
$ poetry shell
```

## pre-commit

To check codes at the commit, use [pre-commit](https://pre-commit.com/).

`pre-commit` command will be installed in the poetry environment.

First, run:

```
$ pre-commit install
```

Then `pre-commit` will be run at the commit.

Sometimes, you may want to skip the check. In that case, run:

```
$ git commit --no-verify
```

You can run `pre-commit` on entire repository manually:

```
$ pre-commit run -a
```

### pytest

Tests are written with [pytest](https://docs.pytest.org/).

Write tests in **/tests** directory.

To run tests, run:

```
$ pytest
```

The default setting runs tests in parallel with `-n auto`.
If you run tests in serial, run:

```
$ pytest -n 0
```

## GitHub Actions

If you push a repository to GitHub, GitHub Actions will run a test job
by [GitHub Actions](https://github.co.jp/features/actions).

The job runs at the Pull Request, too.

It checks codes with `pre-commit` and runs tests with `pytest`.
It also makes a test coverage report and uploads it to [the coverage branch](https://github.com/rcmdnk/chatgpt-cli/tree/coverage).

You can see the test status as a badge in the README.

### Renovate

If you want to update dependencies automatically, [install Renovate into your repository](https://docs.renovatebot.com/getting-started/installing-onboarding/).
