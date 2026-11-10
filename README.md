# AI Code Reviewer & Explainer

A specification for a web app that reviews a code snippet, explains a function, and drafts unit tests. The model runs on the server. The browser talks only to this application's API.

This repository does not contain application code yet. Nothing here can be installed or run.

## Status

**Now:** this README, `LICENSE`, and `.gitignore`.

**First milestone:** an Express API that accepts a code snippet and returns one of the three results below, using the Gemini API. The React client comes after that API works.

## What the app will do

A signed-in user pastes a snippet and chooses one action:

| Action | Input | Result |
| --- | --- | --- |
| Review | A code snippet | Findings, each with a severity and a suggested change |
| Explain | A function | What the function does, its inputs, and its return value, in plain language |
| Generate tests | A function | Unit tests for the stated behavior of that function |

Language support for the first milestone is the language of the snippet the user pastes. The API does not clone repositories or review pull requests.

Accounts use JWT. Reviews are stored in MongoDB so a user can return to them.

## Tech stack

- Frontend: React with Vite (`client/`)
- Backend: Node.js and Express (`server/`)
- Database: MongoDB with Mongoose
- AI: Google Gemini API, called only from `server/`
- Authentication: JWT

Planned layout:

```text
client/     React + Vite UI
server/     Express API, Gemini calls, and MongoDB models
```

## Prerequisites

These are required once `client/` and `server/` exist:

- Node.js (LTS)
- A running MongoDB instance
- A Google Gemini API key

## Configuration

Create `server/.env` from `server/.env.example` when that example file is added. Expected names:

| Variable | Purpose |
| --- | --- |
| `GEMINI_API_KEY` | Server-side calls to Gemini |
| `MONGODB_URI` | MongoDB connection string |
| `JWT_SECRET` | Signing key for authentication tokens |
| `PORT` | Port the API listens on |

Do not commit `.env` files. Do not put `GEMINI_API_KEY` or `JWT_SECRET` in frontend code or in this README.

## Running the app

Install and start commands will be documented here when `client/` and `server/` exist. There is no `package.json` in this repository yet.

The intended flow is: start MongoDB, start the API from `server/`, start the Vite app from `client/`, then submit a snippet from the UI.

## Example

A user pastes this function and chooses **Explain**:

```js
function clamp(value, min, max) {
  return Math.min(Math.max(value, min), max);
}
```

The API is expected to return a short explanation: `clamp` keeps `value` inside the inclusive range from `min` to `max`, and it returns that bounded number.

The same snippet sent as **Generate tests** should come back as tests for values below `min`, inside the range, and above `max`. A **Review** of the same snippet should note that the function does not check that `min` is less than or equal to `max`.

## License

[MIT](LICENSE)
