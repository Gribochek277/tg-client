> Sanitized mirror of Forgejo `serhii/tg-client`. Source code is not published here.
>
> Commit texts: `commits/`. Need the code? Email: sergeyalpatov1@gmail.com
> Source: Forgejo `serhii/tg-client` | Synced: 2026-10-05T02:04:18Z

---

# tg-client

> Keyboard-first Telegram TUI client for the terminal — with native **Kitty
> graphics protocol** support for inline images.

```
┌─ Chat ─────────────┐┌─ Work: dev-team ───────────────────────┐
│ 📌 pinned channel  ││ #102 09:41 alice: shipped the fix ✅    │
│  3 Project Logs    ││ #103 09:42 bob:   let's demo tomorrow   │
│ @alice             ││ #104 09:43 you:   works on my machine   │
│ Team Alpha         ││                                        │
│ @bob               ││      [ 📷 photo — press 'o' to open ]   │
│ Project Logs       ││                                        │
│ muted group        ││ > type a message (enter to send)       │
└────────────────────┘└────────────────────────────────────────┘
```

Built in TypeScript on top of **teleproto** (maintained MTProto fork of
GramJS) with a custom ANSI renderer — so photos render as *real pixels*
in kitty / Ghostty / WezTerm, not ASCII art.

## Features

- 🔑 Full Telegram account (MTProto) support — chats, groups, channels, DMs
- 🖼 **Kitty graphics protocol**: photos & stickers render inline automatically
- ⌨️ Vim-style navigation, single-pane message view, tmux-like workflow
- 📤 Send / reply / edit / delete / forward / alias messages
- 💾 Session + layout + aliases persistence across restarts
- 🚀 Works over SSH; low memory footprint
- ✂️ Slash commands (`/help`, `/reply`, `/search`, `/media`, …)

## Install

```bash
# From source
git clone <repo-url> && cd tg_client
npm install
npm run build
npm link          # exposes `tg` on your PATH
```

Requires Node ≥ 20.

## First run

1. Get Telegram API credentials: https://my.telegram.org → *API development tools*
2. Run `tg --login` and follow the prompts (credentials are saved to
   `~/.config/tg-client/config.json`, session to `~/.local/share/tg-client/`)
3. Or set `TG_API_ID` / `TG_API_HASH` env vars instead

## Modes (Neovim-style)

The app mirrors Neovim's mode machine: **normal** (default), **insert**
(composer), **command-line** (`:`) and **search** (`/` / `?`).

### NORMAL (default)

| Keys | Action |
|---|---|
| `j`/`k` or `↑`/`↓` | navigate (sidebar: dialogs · pane: messages — view follows) |
| `h` / `l` | focus sidebar / focus message pane |
| `g` / `G` | jump to start / end of chat (or dialog list) |
| `Enter` | sidebar: open chat · pane: enter insert mode |
| `i` | enter insert mode (composer) |
| `:` | enter command-line mode |
| `/` or `?` | search forward / backward |
| `n` / `N` | repeat last search (same / opposite direction) |
| `f` | cycle chat folders (All chats / Chats / Bots …) |
| `o` | open large preview of the message under the cursor |
| `Ctrl+↑` / `Ctrl+↓` | scroll message view without moving the cursor |

### INSERT (composer)

| Keys | Action |
|---|---|
| type | compose a message |
| `Enter` | send |
| `Esc` | back to NORMAL (text is kept) |
| `Backspace` / `Ctrl+W` / `←` / `→` | edit |

### COMMAND-LINE (`:`)

| Keys | Action |
|---|---|
| type | type a `:command` |
| `Enter` | run the command |
| `Esc` | cancel (line is kept for recall) |
| `Tab` | complete the command name |

`:` recalls the last typed command between invocations (like Neovim).
`:q` quits; `Ctrl+Q` remains as an alternative.

### SEARCH (`/` / `?`)

| Keys | Action |
|---|---|
| type | type the query |
| `Enter` | jump to the next match (wrap-around) |
| `Esc` | cancel |
| `n` / `N` | repeat the last search forward / backward |

### Global

| Keys | Action |
|---|---|
| `Tab` / `Shift+Tab` | cycle focus |
| `Ctrl+Q` / `Ctrl+R` | quit / refresh dialogs |
| `Ctrl+←` / `Ctrl+→` | sidebar width |
| `Ctrl+T` / `Ctrl+G` / `Ctrl+M` | timestamps / line numbers / unread counts |

Photos and stickers render **inline** in the message view automatically
(Kitty graphics protocol). The newest five pictures are prefetched; older
ones download on demand as you scroll (the placeholder turns into the
image once it arrives). Inline previews use Telegram's **800px thumbnails**
for speed — press `o` (or `:media`) on a message to fetch and view the
full-resolution image. Downloads are cached on disk in
`~/.local/share/tg-client/media/` — restarts reuse the cache.

## Commands

```
:help  :reply <id> [text]  :search <q>  :media <id>  :edit <id> <text>
:delete <id>  :alias <name>  :forward <@user>  :new <@user>
:refresh  :quit
```

Commands run from the command-line mode (`:` key), like Neovim ex-commands.

## Development

```bash
npm run dev         # run with tsx (no build step)
npm run typecheck   # tsc --noEmit
npm run build       # emit to dist/
```

## Roadmap

See [docs/PRD.md](docs/PRD.md) — milestones for media sending, GIFs, live
updates, group management, notifications and themes.

## License

MIT
