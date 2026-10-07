# Hosting an LLM in production

Slidev deck for a meetup talk on 2 Sept 2026. Speaker: Dan Harper.

There is also a shorter CTO cut in `slides-cto.md`.

Node runs in Docker Compose. You do not need Node on the host.

## Run

Meetup deck (default):

```bash
docker compose up
```

CTO deck:

```bash
docker compose run --service-ports slidev sh -c "npm install && npx slidev slides-cto.md --remote"
```

Or with Node locally:

```bash
npm run dev        # meetup deck
npm run dev:cto    # CTO deck
```

Then open http://localhost:3030

Presenter view is at http://localhost:3030/presenter

## Notes

Meetup deck: `slides.md`. CTO cut: `slides-cto.md`.
