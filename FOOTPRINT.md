# Shared-host footprint

Everything this project installs outside its own repo, declared per realisateur
build discipline. Filed with senechal 2026-07-29 (`notify-senechal`, senechal
commit `829be49`). **front-door owns these; senechal owns knowing they exist.**

Moved here from `.scheduler/FOCUS.md` on 2026-08-15 when that file was retired
to a pointer stub (`hf7y/scheduler#66`, `hf7y/realisateur#230`). It is a live
declaration, not backlog, so it stays beside the mechanism rather than becoming
an issue. Add and retire entries here as things are installed and removed.

## On `mandark`

Nothing installed, currently. `front-door-watch.service` is **retired**: removed
2026-09-18 by Zach's direct ruling ("front-door service needs to be ripped
out"), running the exact teardown below. Verified after:
`systemctl --user list-unit-files | grep -c front-door` = 0, and the unit file
gone. Filed to senechal as a `footprint-correction` moving
`front-door-watch-unit` from `retiring` to `retired` (hf7y/senechal#937,
closed/absorbed). Full history in
[#8](https://github.com/hf7y/front-door/issues/8).

<details>
<summary>Retired: <code>front-door-watch.service</code> (2026-07-29 → 2026-09-18)</summary>

| What | Where | State |
|---|---|---|
| `front-door-watch.service` | `~/.config/systemd/user/` + `default.target.wants` symlink | removed |

Source of truth was [`etc/front-door-watch.service`](etc/front-door-watch.service)
in this repo; the installed copy was a deployment of it. Ran
`bin/watch --notify -i 120`, `Restart=always`. Polled `hf7y/front-door` and
relayed knocks, amendment PRs, failed CI, and milestone deadlines to WhatsApp
via `bin/ping`.

It existed because **GitHub Actions cannot reach the WhatsApp bridge** — the
bridge binds `127.0.0.1` only — so mandark had to be the bridge.

Teardown, run verbatim on 2026-09-18:

```
systemctl --user disable --now front-door-watch.service
rm ~/.config/systemd/user/front-door-watch.service
systemctl --user daemon-reload
```

*Installed 2026-07-29, verified via `systemctl --user is-active/is-enabled`,
`ss -ltn` (bridge on 127.0.0.1:3000), and an end-to-end relay test where a real
GitHub PR event produced a WhatsApp message. It had gone inert well before
removal — `is-active: inactive`, no timer, and `WorkingDirectory` pointing at a
checkout that no longer existed — so nothing was degrading while the coupled
teardown below waited.*

</details>

**Still open:** the coupled `~/.hermes/.env` revert named in #8's title was
**not** done as part of this teardown — removing the unit and reverting the
`.env` were coupled in the title, not in the mechanism — and #8 stays open for
it.

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
