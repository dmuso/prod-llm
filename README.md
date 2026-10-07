# Hosting an LLM in production

Slidev decks for Dan Harper's talk on self-hosting LLMs.

Node runs in Docker Compose. You do not need Node on the host.

## Decks

| Deck | File | Docker | Node |
| --- | --- | --- | --- |
| Meetup (default) | `decks/meetup/slides.md` | `docker compose up` | `npm run dev:meetup` |
| CTO cut | `decks/cto/slides.md` | `DECK=cto docker compose up` | `npm run dev:cto` |

Then open http://localhost:3030 (presenter view at http://localhost:3030/presenter).

Build / export: `npm run build:meetup`, `npm run build:cto` (output in `dist/<deck>`), `npm run export:meetup`, `npm run export:cto`.

Images live in the root `public/`. Each deck has a `public` symlink to it, because Slidev serves `public/` next to the entry file.
