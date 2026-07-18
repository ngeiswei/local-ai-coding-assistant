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

## Troubleshooting

### Model stopped because it reached the maximum output token limit

If you encounter the following error

```
Error: Model stopped because it reached the maximum output token limit. The response may be incomplete.
```

you are supposed to increase `num_predict`, for that you are supposed
to add `"num_predict": MAXTOKEN` next to `"id": "qwen3.6:latest"` in
file `models.json` under `HOME_FOLDER/.pi/agent` directory.  But it
does not seem to work...
