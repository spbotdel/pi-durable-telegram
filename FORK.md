# BanBao fork

This is a modified fork of [pi-durable-telegram](https://github.com/smixs/pi-durable-telegram),
MIT License, Copyright (c) 2026 smixs. The original `LICENSE` is kept unchanged.

- Upstream: https://github.com/smixs/pi-durable-telegram
- Fork: https://github.com/spbotdel/pi-durable-telegram
- Base: upstream `main` at 0.1.8 (`06362c9`)

## Changes on top of upstream

1. **Custom providers from `models.json`** — cherry-picked from upstream PR #5
   ([smixs/pi-durable-telegram#5](https://github.com/smixs/pi-durable-telegram/pull/5),
   author Jemy Zhang, MIT). Lets `PI_PROVIDER` name a provider declared in
   `~/.pi/agent/models.json` (or `$PI_CODING_AGENT_DIR/models.json`) with
   `baseUrl`, `api: "openai-completions"`, `apiKey` (`${ENV_VAR}` or literal)
   and a list of models.
2. **Thinking levels fixed** — the PR defaulted `off` to the literal string
   `"off"`, which OpenAI-compatible endpoints reject with
   `level "off" not supported, valid levels: low, medium, high`.
   The default map now omits the level for `off`, and a model may define its own
   `thinkingLevelMap` in `models.json`.
3. **Cross-platform build** — `npm run build` removes `dist` with Node instead
   of `rm -rf`, so it also works on Windows.

## Install (Linux server)

```sh
npm install -g <path-to-pi-durable-telegram-0.1.8.tgz>
```

Then create `~/.config/pi-durable-telegram/env` and
`~/.pi/agent/models.json`, install the systemd user unit from
`deploy/pi-durable-telegram.service`, and enable lingering:

```sh
systemctl --user daemon-reload
systemctl --user enable --now pi-durable-telegram
loginctl enable-linger "$USER"
```

## Keeping up with upstream

- `main` tracks `upstream/main`.
- `banbao` is the branch we deploy.
- When upstream merges PR #5, drop the cherry-picked commit and rebase `banbao`
  on the new upstream release; keep the thinking-level fix only if upstream
  still sends `off`.
