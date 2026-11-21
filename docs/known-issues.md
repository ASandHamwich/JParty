# Known issues and quirks

Found by reading the source while writing these docs. None were reproduced by running the app, so treat each as "the code says this", not as a confirmed bug report. Nothing here has been fixed.

## Likely bugs

| Where | Issue |
| --- | --- |
| [game.py:304](../jparty/game.py#L304) | A player who joins during the final round after everyone else has wagered does not block the game. If the host continues and that player never wagers, judging them fails because `player.wager` is `None`. |
| [game.py:95](../jparty/game.py#L95) | `KeystrokeManager._activate` calls `e.hint_setter(True)` before checking that it is not `None`, and then calls it a second time. Harmless today because every event has a hint setter. |
| [main_display.py:236](../jparty/main_display.py#L236) | `HostDisplayWindow.hide_welcome_widgets` calls a `super()` method that does not exist. Never called. |
| [controller.py:182](../jparty/controller.py#L182) | The `else` branch of `BuzzerController.buzz` uses `self.game` after finding it falsy, and refers to a `buzz_hint_trigger` signal that does not exist. Unreachable in practice. |
| [controller.py:51](../jparty/controller.py#L51) | `POST /play` renders `play.html`, which is not in the repo. |
| [borders.py:57](../jparty/borders.py#L57) | `HostBorders.buzz_hint` references a `__buzz_hint` method that is not defined. Never called. |
| [scoreboard.py:149](../jparty/scoreboard.py#L149) | `if self.game.modify_players:` tests a bound method, which is always true, so clicking a podium always opens player settings. |
| [logger.py:49](../jparty/logger.py#L49) | The crash reporter calls `os.uname()`, which does not exist on Windows, so sending a report there would itself fail. |
| [game.py:358](../jparty/game.py#L358) | Only the most recent wrong answerer is locked out. With three players, A can buzz again after B has also answered wrong. |
| [game.py:413](../jparty/game.py#L413), [game.py:440](../jparty/game.py#L440) | `prev_round` / `next_round` log an error at the first/last round but carry on. The settings dialog disables the buttons, which hides this. |

## Quirks

- **Must run from `jparty/`.** Asset, template and static paths are relative to the working directory in development (see [architecture.md](architecture.md)).
- **`latest.log` location.** Written to the working directory in development, and into PyInstaller's temporary `_MEIPASS` folder in a built app, where it is hard to find and is removed on exit.
- **Hard requirements at startup.** Two screens and a reachable `j-archive.com`, even when playing a Google Sheets game.
- **Player count.** The README says 3–8 players; the code allows starting with 1.
- **Two different template links.** The README points at one Google Sheets template; the in-app link and the comment in [retrieve.py](../jparty/retrieve.py#L13) point at another.
- **Game loads on every keystroke.** Each edit of the Game ID box starts a download thread; stale results are discarded by comparing against the current text.
- **GUI updates from worker threads.** Podium lights, border flashes and key hints call widget methods from plain Python threads rather than through Qt signals.
- **No cookie secret, no authentication.** Anyone on the LAN can join. Player tokens are random 15-byte values.
- **CI artifact typo.** The x86 macOS artifact is named `JParty_macox_x86` in [main.yml](../.github/workflows/main.yml#L51).
- **Dead code.** `get_game_sum`, `Welcome.show_settings`, `BuzzerSocketHandler.cache`, `BoardWidget.responses_open`.
- **No tests.**
