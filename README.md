# Pollify — Ask. Vote. Discover.

A production-quality frontend for a modern polling platform. Frontend only — built with mock
data behind a service layer so a real Node.js + Express + PostgreSQL backend can be dropped in
later without touching the UI.

## Stack

- React + Vite
- Tailwind CSS (custom design-token system, light/dark mode)
- React Router
- Framer Motion
- Recharts
- Lucide React icons

## Getting started

```bash
npm install
npm run dev
```

Then open the printed local URL (usually `http://localhost:5173`).

## Project structure

```
src/
├── components/
│   ├── ui/            Reusable primitives (Button, Card, Modal, Input, Switch, ...)
│   ├── charts/         Recharts wrappers (area, donut, bar timeline, results bar list)
│   ├── navigation/     Sidebar, Topbar, MobileNav, CommandPalette, notification/user menus
│   ├── polls/          Poll-specific components (PollCard, OptionRow, PollPreview)
│   └── dashboard/      Dashboard-specific components (StatCard)
├── pages/               One file per route (Landing, Dashboard, Polls, CreatePoll, ...)
├── services/api.js      Mock API layer — swap for real fetch() calls, UI won't need to change
├── data/mockData.js     Realistic mock data
├── hooks/                useTheme, useToast context hooks
├── utils/format.js      Formatting + poll-math helpers
├── layouts/              AppLayout (dashboard shell), AuthLayout (login/register)
├── App.jsx               Route definitions
└── main.jsx              Entry point
```

## Connecting a real backend

Every function in `src/services/api.js` currently resolves mock data after a short delay to
simulate network latency. Replace each function body with a `fetch` call to your API — the
function signatures and return shapes are already what the UI expects, e.g.:

```js
export async function getPolls(params) {
  const res = await fetch(`/api/polls?${new URLSearchParams(params)}`)
  if (!res.ok) throw new Error('Failed to load polls')
  return res.json()
}
```

No component imports mock data directly — everything goes through this service layer.

## Notable features

- Full light/dark theme system with token-based CSS variables, persisted to `localStorage`
- Command palette (⌘K / Ctrl+K) for jumping to polls and actions
- Poll builder with drag-to-reorder options and a live preview panel
- Public voting page with animated transition into results
- Deep analytics page with device/geography breakdowns and CSV/JSON export placeholders
- Responsive: sidebar becomes a bottom nav on mobile, tables become cards
- Toasts, skeleton loaders, empty states, and confirmation dialogs throughout
