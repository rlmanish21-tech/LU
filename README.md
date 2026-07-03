# Mock Test App — Vercel deployment

## Structure
```
index.html      → the whole app (static, served as-is)
api/claude.js   → serverless function that proxies AI requests to Anthropic
package.json    → minimal config so Vercel detects api/ as Node functions
```

## Deploy steps

1. **Get an Anthropic API key** (if you don't have one): console.anthropic.com → API Keys.

2. **Push this folder to a GitHub repo**, or deploy directly with the Vercel CLI:
   ```
   npm install -g vercel
   cd mock-test-app
   vercel
   ```
   Follow the prompts (link/create a project, accept defaults — no build command needed).

   Or via the Vercel dashboard: "Add New Project" → import this folder/repo → deploy.

3. **Add your API key as an environment variable**:
   - Vercel dashboard → your project → Settings → Environment Variables
   - Add `ANTHROPIC_API_KEY` = `sk-ant-...` (your key)
   - Redeploy (Vercel dashboard → Deployments → ⋯ → Redeploy) so the function picks it up.

4. Open your `https://your-project.vercel.app` URL. PDF upload and the AI features
   ("Suggest missing answers" and "Get correct answer") will now work — no more
   `file://` worker issues, and your API key stays server-side.

## Notes
- The question bank and test history are stored via the app's own `Store` helper,
  which falls back to `localStorage` when `window.storage` isn't available — so
  each visitor's data lives in their own browser, not shared across users.
- If you ever see "Server is missing ANTHROPIC_API_KEY" in the app, it means the
  environment variable wasn't set (or you forgot to redeploy after adding it).
