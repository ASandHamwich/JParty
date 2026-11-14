# JParty documentation

JParty is a Jeopardy!-style party game: a PyQt6 desktop app that shows a game board on a TV, gives the host a private view with the answers, and lets players buzz in from their phones over the local network.

| Document | For | Contents |
| --- | --- | --- |
| [how-to.md](how-to.md) | Anyone | Install, set up and host a game, with troubleshooting |
| [architecture.md](architecture.md) | Developers | Components, startup, module map, data model, game sources, build |
| [game-flow.md](game-flow.md) | Developers | The game state machine and key bindings |
| [buzzer-protocol.md](buzzer-protocol.md) | Developers | Phone ↔ server WebSocket messages |
| [knowledge-graph.md](knowledge-graph.md) | Developers | Diagrams of objects, classes and concepts |
| [knowledge-graph.json](knowledge-graph.json) | Tools | The same graph as nodes and edges |
| [known-issues.md](known-issues.md) | Developers | Bugs and quirks noticed in the code |

These documents describe version 2.1.1 and were written from reading the source code; the app was not run to verify them.
