# Mavericks Fit Call Practice

Deck Practice, Objection Handling Drill, and Mock Interview in one console. The deck, teleprompter, and the drill write, voice, and reveal all run with no key. Mav AI coaching, mock scoring, and drill scoring run through a serverless proxy so the API key never reaches the browser.

## What you need

1. A Vercel account.
2. An Anthropic API key.

## Deploy, the short path

From this folder:

```
npm install
npm install -g vercel    # only if you do not have the Vercel CLI
vercel                   # links the project and does a preview deploy
vercel --prod            # production deploy
```

Then set the key as an environment variable, once:

```
vercel env add ANTHROPIC_API_KEY
```

Paste your key when prompted, choose Production (and Preview if you want previews to work too), then redeploy:

```
vercel --prod
```

That is it. The app is live and the key stays on the server.

## Deploy, the GitHub path

1. Push this folder to a new GitHub repo.
2. In Vercel, New Project, import the repo. Framework preset detects Vite automatically.
3. In Project Settings, Environment Variables, add `ANTHROPIC_API_KEY` with your key.
4. Deploy.

## Run it locally first

```
npm install
echo "ANTHROPIC_API_KEY=sk-ant-your-key" > .env
vercel dev
```

`vercel dev` runs the Vite app and the `/api/mav` function together so the AI features work locally. Plain `npm run dev` runs the UI but the AI calls will fail because there is no function server.

## How the key is protected

The browser never sees the key. Every AI call (Mav coaching, mock scoring, the drill scorer inside its panel) posts to `/api/mav`, a serverless function in `api/mav.js`. That function reads `ANTHROPIC_API_KEY` from the server environment, calls Anthropic, and returns the result. If the key is missing or the call fails, the drill falls back to self rating and the coach and mock show a clear message, nothing crashes.

## Editing the Mav AI rules

The coaching rules are baked into `src/App.jsx` as `DEFAULT_RULES`. In-app edits behind the password are session only. To change the rules permanently, edit `DEFAULT_RULES` and redeploy.

## Notes

- No `vercel.json` is needed. Vercel detects Vite and serves `api/*.js` as functions automatically.
- The deck and the drill are embedded the same way, so this is one app, not three.
- Region is still set in two places for now, the deck screens and the drill panel. Unifying that into one control is the next planned change.
