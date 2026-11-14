# JParty

Jeopardy!-style party game: a PyQt6 desktop app with two full-screen windows (host view, player board) and a Tornado WebSocket server that phones use as buzzers. About 3,900 lines of Python in `jparty/`, plus a one-page phone client in `jparty/buzzer/`.

## Read the docs before the code

`docs/` maps the whole codebase. Read the relevant file before exploring the source; it is usually enough to go straight to the right function.

| Task touches | Read first |
| --- | --- |
| Anything, when unsure where it lives | `docs/architecture.md` (module map, design patterns), then the "Concepts and where they live" table in `docs/knowledge-graph.md` |
| Game rules, rounds, scoring, key bindings | `docs/game-flow.md` |
| Phone client, WebSocket messages, joining/reconnecting | `docs/buzzer-protocol.md` |
| A bug report | `docs/known-issues.md` (it may already be listed, with a file and line) |
| User-facing behaviour or setup instructions | `docs/how-to.md` |

`docs/knowledge-graph.json` holds modules, classes, functions and their relations as nodes and edges. Query it with `jq` instead of grepping when the question is structural:

```
# what does a module import / who imports it
jq '.edges[] | select(.relation=="imports" and .target=="module:jparty.game")' docs/knowledge-graph.json
# subclasses of a class
jq -r '.edges[] | select(.relation=="inherits" and .target=="class:QuestionWidget") | .source' docs/knowledge-graph.json
# runtime relationships of a class (hand-written, with notes)
jq '.edges[] | select(.curated and (.source=="class:Game" or .target=="class:Game"))' docs/knowledge-graph.json
```

The docs and graph describe v2.1.1 and were written from reading the source, not from running the app. If the code disagrees with them, trust the code and fix the doc.

## Keep the docs current

When a change alters behaviour that a doc describes, update that doc in the same change:

- New, renamed or removed module, class or module-level function: update the module map in `docs/architecture.md`, and the matching nodes and edges in `docs/knowledge-graph.json` (and the diagrams in `docs/knowledge-graph.md` if a class hierarchy or ownership changed).
- New or changed key event or `Game` state transition: `docs/game-flow.md`.
- New or changed WebSocket message or route: `docs/buzzer-protocol.md`.
- A bug from `docs/known-issues.md` is fixed: remove its row.
- Anything a host or player would notice: `docs/how-to.md`. Keep it free of jargon; its reader is not technical.

## Running

```
pip install -r requirements.txt   # Python 3.11, as in CI
cd jparty
python ../run.py
```

- It must be launched from inside `jparty/`. Assets, templates and static files resolve relative to the working directory.
- It exits at startup unless two displays are attached and `j-archive.com` is reachable, so it usually cannot be launched from an agent session. Say so instead of claiming a change was verified by running it.
- There are no tests. Check changes with `python -m py_compile jparty/*.py` and by reasoning through the flow in `docs/game-flow.md`.
- Runtime log: `jparty/latest.log` (overwritten each run).
- Build: `pyinstaller -y JParty.spec`.

## Conventions that are easy to break

- **Host/player class pairs.** Each screen element has a base class for the player screen and a `Host…` subclass for the host screen, created through factory methods on `DisplayWindow` that `HostDisplayWindow` overrides. Never put the answer or host controls in a base class: it would show on the TV.
- **`Game.dc` fans out to both windows.** `self.dc.x()` calls `x()` on the host and the player display. Use `self.host_display` or `self.main_display` when only one should change. A method called through `dc` must exist on both.
- **State lives in `KeystrokeManager`.** There is no state enum. Each `Game` method activates the key events that are valid next. A new step in the flow needs an `addEvent` in `Game.__init__` and an `activate` at the right point.
- **Tornado runs in another thread.** WebSocket handlers must reach the GUI through Qt signals on `Game` (`buzz_trigger`, `wager_trigger`, `new_player_trigger`), not by calling widget methods.
- **`BUZZ` is matched as a raw substring** before JSON parsing in `BuzzerSocketHandler.on_message`, for latency. Do not add a message type containing that string.
- **Protocol changes are two-sided.** Message names must match in `jparty/controller.py` and `jparty/buzzer/static/buzzer.js`.
- **New assets** go in `jparty/data/` and are loaded with `resource_path("name")`; that folder is what `JParty.spec` bundles.
- **Version** is in `jparty/version.py` only.
