# Campus Library

Small web catalogue of the campus library: search the books by title or
author and see which ones are available or on loan.

> **Example repository for a thesis proof of concept.** This project is one
> of the repositories that the AI agents of
> [*A Novel Framework for Delegated Authorization and AI Agent Interoperability in Cloud Software Engineering Workflows*](https://github.com/DarkGh0st03/delegation) work on. It is a real, runnable application, but it
> exists to show the delegation mechanism of that proof of concept at work.
> See [Role in the proof of concept](#role-in-the-proof-of-concept).

## Role in the proof of concept

In the proof of concept, a Software Engineer assigns a requirement in natural
language and a team of AI agents implements it on a repository. Every
operation of the agents is authorized by a chain of Delegation Credentials and
checked by a Gateway (Rust verification and OPA policy); the result is a Draft
Pull Request that a human reviews and merges.

Campus Library exists to show **a requirement that needs the whole team**.
Its files map one-to-one onto the specialist agents:

| Files | Agent |
| --- | --- |
| `server.js`, `src/catalog.js` | Backend |
| `public/` (page, script, styles) | Frontend |
| `data/books.json` | Data |
| `README.md` | Documentation |
| `Dockerfile` | DevOps |
| `test/` | Test |

A single requirement can therefore involve six agents. The Orchestrator gives
each one a Delegation Credential limited to its own files, so the example
shows least-privilege delegation across many roles at once.

It is also deliberately simple: plain Node.js with no dependencies, the
default test profile (`npm ci`, `npm test`) and a server that also serves the
pages, which the proof of concept detects as a web interface to deploy and
preview.

Requirements used in the demonstration:

- *"Every book must have a genre: add it to the data, filter the books by
  genre in the API (`genre` parameter), add a drop-down menu to the page and
  document the filter in the README"* (Data, Backend, Frontend,
  Documentation, Test).
- *"Make the maximum number of API results configurable with the
  `MAX_RESULTS` variable (default 50), set it in the Dockerfile and document
  it in the README"* (Backend, DevOps, Documentation, Test).

## Run

Requires Node.js 22 (no external dependencies).

```bash
npm start
```

Then open http://127.0.0.1:3000. The server reads `PORT` (default `3000`)
and `HOST` (default `127.0.0.1`).

## API

| Method | Path | Description |
| --- | --- | --- |
| GET | `/api/books?q=<text>` | Books whose title or author contains `q`, sorted by title, with a summary (`total`, `available`, `onLoan`). |

## Data

The catalogue is in `data/books.json`: one object per book with `id`,
`title`, `author`, `year` and `available`.

## Tests

```bash
npm test
```

## Docker

```bash
docker build -t campus-library .
docker run -p 3000:3000 campus-library
```
