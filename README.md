# RAEJUS Neo-Brutalist SvelteKit

## Run locally

```powershell
npm install
npx svelte-kit sync
npm run dev
```

The project uses Vite for the development/build commands because the SvelteKit CLI in this setup exposes `sync`, while Vite runs the SvelteKit application.

Open: http://localhost:5173/

## If npm reports the esbuild install-script warning

Run:

```powershell
npm install-scripts approve esbuild@0.28.2
npm rebuild esbuild
```

Then:

```powershell
npm run dev
```

## Production build

```powershell
npm run build
npm run preview
```

Place the original RAEJUS photos/videos under `static/media/existing-site/` without modifying them.
