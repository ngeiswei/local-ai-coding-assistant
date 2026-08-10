# Information Gathered to Run Local AI Coding Assistant

## Overview

The current plan is to combine [Ollama](https://ollama.com) with
[Pi](https://pi.dev/).

## Warnings

It is recommended to run this in a docker container because Pi have
access to write actions.

## Prerequsites

Actually install commands are given, but you should check if they are
up to date before running them.

1. Install [Node.js](https://nodejs.org).  It is highly recommended to
   use [NVM](https://www.nvmnode.com/).  The command should be
   ```bash
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.6/install.sh | bash
   nvm install 24
   ```
2. Install [Ollama](https://ollama.com/). The command should be
   ```bash
   curl -fsSL https://ollama.com/install.sh | sh
   ```

## Usage

Simply launch Pi via Ollama from your project folder (the program you
wish to be assisted for).

```
ollama launch pi
```

This will install Pi the first time.  Then will ask you to select a
model (`qwen3.6` seems to be the popular choice) and download it.

After that you can also launch Pi by typing

```
pi
```

under your project folder.

## Configure Pi

### Connect to other models

Pi allows you to use any model (local or remote) supporting the OpenAI
API, not just Ollama.  To do that add the following

```json
    "PROVIDER_NAME": {
      "api": "openai-completions",
      "apiKey": "API_KEY",
      "baseUrl": "PROVIDER_URL/v1",
      "models": [
        {
          "_launch": true,
          "contextWindow": CONTEXT_WINDOW_SIZE,
          "id": "MODEL",
          "input": [
            "text"
          ],
          "reasoning": true
        }
      ]
    },
```

under the `providers` field of `.pi/agent/models.json`.

Once under Pi, you should be able to select such model(s) with the
`/model` command.

### Extensions

Pi offers vast collection of [extensions](https://pi.dev/packages).

#### List installed extensions

To list extensions installed on your system, type

```bash
pi list
```

You should have at least one, web search, installed by Ollama.

#### Install extension

To install an extension, type

```bash
pi install EXTENSION
```

#### Install permission system

To keep Pi from modifying or writing files, or running software
without restraint on your system, you may want to install the
[Permission System](https://pi.dev/packages/pi-permission-system)
extension.

For that, run the following command

```bash
pi install npm:pi-permission-system
```

Then you may copy/paste the following

```jsonc
{
  "defaultPolicy": {
    "tools": "ask",
    "bash": "ask",
    "mcp": "ask",
    "skills": "ask",
    "special": "ask"
  },
  "tools": {
    "read": "allow",
    "grep": "allow",
    "find": "allow",
    "ls": "allow",
    "write": "ask",
    "edit": "ask"
  },
  "bash": {
    "*": "ask",
    "ls": "allow",
    "ls *": "allow",
    "wc": "allow",
    "wc *": "allow",
    "head": "allow",
    "head *": "allow",
    "tail": "allow",
    "tail *": "allow",
    "sort": "allow",
    "sort *": "allow",
    "pwd": "allow",
    "git status": "allow",
    "git diff": "allow"
  }
}
```

in `.pi/agent/pi-permissions.jsonc`.

Feel free to modify the permissions as you see fit.

## Troubleshooting

### Pi keep asking permissions on things that are allowed.

TODO

### Model stopped because it reached the maximum output token limit

If you encounter the following error

```
Error: Model stopped because it reached the maximum output token limit. The response may be incomplete.
```

you are supposed to increase `num_predict`, for that you are supposed
to add `"num_predict": MAXTOKEN` next to `"id": "qwen3.6:latest"` in
file `models.json` under `HOME_FOLDER/.pi/agent` directory.  But it
does not seem to work...
