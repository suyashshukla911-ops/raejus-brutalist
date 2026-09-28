# RAEJUS Brutalist - Runtime Fixes

The project targets Svelte 5 / SvelteKit 2.

## Fix included

- `IndexNav.svelte` uses `$app/state` correctly with `page.url.pathname` instead of `$page` auto-subscription.
- `+page.svelte` adds an ARIA role to the mouse-interactive home section.
- `package.json` uses `svelte-kit dev`, `svelte-kit build`, and `svelte-kit preview`.
- Navigation path comparison normalizes trailing slashes.

## Run

```powershell
npm install
npx svelte-kit sync
npm run dev
```

Then open `http://localhost:5173/`.
