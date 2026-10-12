# Running OpenBot on your laptop

Steps for a local run from this clone. Source: the README and `.env.example`.

## You need

- [Docker](https://www.docker.com/) with Docker Compose, running. It starts PostgreSQL and the sample Bots.
- [Bun](https://bun.sh) (the repo pins 1.3.14) and Node/npx.
- A free CopilotKit account.
- An OpenAI API key.

## Steps

1. Install dependencies and create your `.env`:

   ```sh
   cp .env.example .env
   bun install
   ```

2. Log in to CopilotKit and connect a project. The first command opens a browser:

   ```sh
   npx --yes copilotkit@latest login
   npx --yes copilotkit@latest project select
   bun scripts/setup-learning.ts
   ```

   This writes `INTELLIGENCE_API_KEY` (and a Learning container) into `.env`.

3. Open `.env` and set your OpenAI key:

   ```
   OPENAI_API_KEY=sk-...
   ```

   Leave the other values as they are. `OPENBOT_SINGLE_USER=true` is already set, so
   no sign-in is needed locally. `scripts/start.sh` generates the remaining local secrets.

4. Start everything. Docker must be running:

   ```sh
   bash scripts/start.sh
   ```

   It starts PostgreSQL, applies migrations, then starts the API (port 3001), the
   routine worker and the app, and checks that each one answers.

5. Open <http://localhost:3010>.

To stop: `bash scripts/stop.sh` (add `--keep-computers` to leave the Bots' browsers running).
Nothing is deleted; your data is kept in Docker volumes.

## Try it

- Open `/bot` and ask: `Open news.ycombinator.com and tell me the top story.`
- Look at `/admin/audit` to see what the Bot did.

## Before anyone else uses it

`OPENBOT_SINGLE_USER=true` makes every visitor an administrator. Remove it and
configure Google, Microsoft or Okta sign-in (see `.env.example`) before exposing
this beyond your own machine.

## What OpenBot does

OpenBot is a company-owned alternative to ChatGPT or Claude that you run yourself.
You chat with AI "coworkers" (Bots). Each Bot gets its own computer: a real web
browser with its own logins, files and a list of tools you allow. When a Bot clicks,
types, opens a file or calls a tool, a central gateway first checks your rules,
then logs the action so you can review it later. You can watch a Bot work on its
screen, take over when it reaches something it shouldn't do alone, and hand it back.
Bots can also run on a schedule, and you can reach them from Slack, Teams or text.
It is a template to clone and customize, not a hosted product, and it is alpha software.
