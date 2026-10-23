# TripCraft

AI-assisted group trip planning: the AI researches a destination, the group votes on what to do, and the AI assembles the votes into a day-by-day itinerary.

> [!NOTE]
> Public overview of a private project. The source code is not included.

## The problem

One person in a friend group ends up researching the destination across blogs, maps and social media, then collecting preferences over chat threads. TripCraft moves the research to a search-backed LLM pipeline and the decision to a shared vote.

## How it works

1. The creator signs in with Google and enters destination, dates, budget and activity preferences.
2. An edge function searches the web (Tavily) and has Claude Haiku 4.5 turn the results into structured activity suggestions.
3. The creator shares a link. Participants join with a display name only, no account, via a UUID token in the URL.
4. Participants vote include / maybe / skip. Votes sync live over Supabase Realtime.
5. A second edge function assembles the top-voted activities into a day-by-day itinerary, shown on a map.
6. The full itinerary is unlocked with a one-time Stripe payment.

## Design decisions

- **Guests without accounts.** Voting is the part that needs the whole group, so it has no sign-up step. Identity is a token in the link, with a recovery function for lost devices.
- **Server-side AI pipeline.** Suggestion generation, itinerary assembly and all vote writes are Supabase Edge Functions (Deno), keeping API keys off the clients. Row-level security covers direct table access.
- **Live search instead of model memory.** Suggestions come from current web results, not only training data.

## Stack

| Layer | Technology |
|---|---|
| Web | Next.js, React, Tailwind CSS, shadcn/ui |
| Mobile | Expo (React Native), NativeWind |
| State | Zustand, TanStack Query |
| Backend | Supabase: PostgreSQL with RLS, Realtime, Edge Functions |
| AI | Tavily (search), Claude Haiku 4.5 (generation) |
| Payments | Stripe |
| Maps | Leaflet, OpenStreetMap |
| Tooling | pnpm workspaces, Turborepo, Vitest, Playwright |

Solo project by [Jannis Menzler](https://github.com/jmenzler).
