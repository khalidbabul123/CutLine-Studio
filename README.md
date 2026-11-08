# Cutline Studio

A Vercel-ready video editing workspace for Instagram Reels, TikTok, YouTube Shorts, and long-form YouTube. Add clips, describe the edit you want, review an AI-generated first cut, adjust trims and captions, and export in your browser.

## What works in this MVP

- Upload local video clips, preview them, reorder or remove them, and adjust in/out points.
- Choose a platform format and generate a clip sequence and caption suggestions from a creative brief.
- Review the applied first cut, adjust its captions, then export a combined video with original clip audio.
- Browser rendering keeps the video files on the user's device. The AI provider receives only the creative brief and clip metadata (names, duration, and dimensions), not video or audio.

Export uses the format supported by the browser (typically WebM in Chrome/Edge; MP4 where the browser supports it). Rendering uses the user's device and is intended for short-form and modest-length edits; it is not a cloud render farm.

## Run locally

```bash
npm install
Copy-Item .env.example .env.local
npm run dev
```

Set the AI environment variables in `.env.local` to enable edit-plan generation:

```env
AI_BASE_URL=https://your-provider.example/v1
AI_API_KEY=your-private-api-key
AI_MODEL=your-chat-model
```

The provider endpoint must support the OpenAI Chat Completions request/response format. The app appends `/chat/completions` to `AI_BASE_URL` unless the URL already ends with that path. Keep the key server-side; never add it to a `NEXT_PUBLIC_` variable or commit it.

## Deploy to Vercel

Import this project into Vercel and add `AI_BASE_URL`, `AI_API_KEY`, and `AI_MODEL` under **Project Settings → Environment Variables**. Redeploy after setting them. Without these values, clip editing and browser export still work; AI plan generation returns a setup message.

## Browser requirements and notes

- Use a current desktop Chrome or Edge for the most reliable browser-based video export.
- Keep the editor tab open while exporting; the browser renders the timeline in real time.
- Project name, timeline, and edit settings are held in the current tab only; refresh clears the session. Download the export you want to keep.
- Uploaded footage is not sent to the app server. Only the text brief and clip metadata are sent to the configured AI provider when you request a plan.
- The AI can plan clip order, trims, and on-screen captions from metadata and your brief; it does not analyze visual or audio content in the source files.
