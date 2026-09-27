# VoiceGuard — GitHub project

This repository includes the web frontend, Vercel API proxy, and Python detector backend source.

## Push this project to GitHub

1. Create a new repository on GitHub. Choose **Private** if you do not want the source code to be public.
2. Upload the contents of this project folder to the repository root. `index.html`, `api/`, `vercel.json`, and `detector-backend/` should appear at the top level as shown here.
3. Import the repository into Vercel and deploy with the default settings.

## Start voice detection

The detector backend is included in `detector-backend/`, but GitHub and Vercel do not run that Docker folder as a persistent model server. Deploy that folder to a compatible Docker host with enough memory for PyTorch and the model (about 1.2 GB of model weights). The backend downloads its model on first startup.

Once the backend is deployed and `/api/health` reports `"modelLoaded": true`, copy its base URL. In Vercel, open **Project Settings → Environment Variables**, add `VOICEGUARD_API_URL` with that base URL, and redeploy.

For an example backend URL: `https://your-service.example.com` (no trailing slash).

## What this project does

- The live page samples audio from the browser's selected microphone and sends short windows to the detector.
- Uploaded audio is sent to the same detector backend.
- The classifier returns an estimate of human vs. synthetic voice. It can be wrong; it is not proof of authenticity.
- A browser cannot directly capture a separate telephone call unless the call audio is routed into the selected microphone/input.

The demo login and history are stored in the browser only. They are not secure accounts or a shared database. Microphone permission requires HTTPS or localhost. Audio is sent to the backend URL configured in Vercel; disclose this to users and obtain consent before analyzing call audio.

## Free hosting note

GitHub stores the code; GitHub Pages is static hosting and cannot run this Python/PyTorch backend. Vercel can host the frontend and proxy, but the detector needs a separate running backend. Availability and cost depend on the backend host and its current plan.
