# Shared-host footprint

Everything this project installs outside its own repo, declared per realisateur
build discipline. Filed with senechal 2026-07-29 (`notify-senechal`, senechal
commit `829be49`). **front-door owns these; senechal owns knowing they exist.**

Moved here from `.scheduler/FOCUS.md` on 2026-08-15 when that file was retired
to a pointer stub (`hf7y/scheduler#66`, `hf7y/realisateur#230`). It is a live
declaration, not backlog, so it stays beside the mechanism rather than becoming
an issue. Add and retire entries here as things are installed and removed.

## On `mandark`

| What | Where | State |
|---|---|---|
| `front-door-watch.service` | `~/.config/systemd/user/` + `default.target.wants` symlink | enabled, active |

Source of truth is [`etc/front-door-watch.service`](etc/front-door-watch.service)
in this repo; the installed copy is a deployment of it. Runs
`bin/watch --notify -i 120`, `Restart=always`. Polls `hf7y/front-door` and
relays knocks, amendment PRs, failed CI, and milestone deadlines to WhatsApp via
`bin/ping`.

It exists because **GitHub Actions cannot reach the WhatsApp bridge** — the
bridge binds `127.0.0.1` only — so mandark has to be the bridge.

Teardown:

```
systemctl --user disable --now front-door-watch.service
rm ~/.config/systemd/user/front-door-watch.service
```

*Verified 2026-07-29 via `systemctl --user is-active/is-enabled`, `ss -ltn`
(bridge on 127.0.0.1:3000), and an end-to-end relay test where a real GitHub PR
event produced a WhatsApp message.*

**Open:** whether to keep or retire this under the mandark→monkey teardown is
[#8](https://github.com/hf7y/front-door/issues/8).

## Off-machine

The cloud doorkeeper routine (`trig_01Jqm3wbMwfw2maKKo93U688`) is the only thing
that can answer a knock unattended, and the only part of the footprint that does
not die with mandark. Its full record — schedule, env, how to disable, and the
fact that its Art. 9 authority fence is prose rather than mechanism — is
[#10](https://github.com/hf7y/front-door/issues/10).

## Not front-door's domain — flagged for hermes' owner

Changes made to **hermes** to get the transport working, listed for honesty and
not claimed as owned. Tracked at
[#9](https://github.com/hf7y/front-door/issues/9).

- Started + enabled the pre-existing-but-stopped `hermes-gateway.service`, then
  restarted it.
- **Edited `~/.hermes/.env`** — `WHATSAPP_HOME_CHANNEL` held the display name
  `Hermes Baudin` where a JID belongs, so the Baileys bridge threw
  `Cannot destructure property 'user' of 'jidDecode(...)'` on every send. Set to
  the account's own JID. Backup: `~/.hermes/.env.bak.frontdoor`.
- `npm install` (145 packages) into
  `~/.hermes/hermes-agent/scripts/whatsapp-bridge/node_modules` — the bundled
  bridge shipped without them and pairing could not work until it ran.

The `.env` edit is the item most in need of a second opinion: a config change in
another project's domain, which senechal's own rules would have handled as a
remedy script rather than a live edit.

## Deliberately not touched

No crontab entries. Nothing in `~/.local/bin`. No `~/.claude` hooks. No
system-wide (non-`--user`) units.

## Standing limitation

Every local transport dies with the machine. WhatsApp, KDE Connect, and desktop
notification all need mandark awake. Only GitHub Actions survives a sleeping
laptop, and it can only comment on the repo.
