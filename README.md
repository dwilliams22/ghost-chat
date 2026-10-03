# Ghost Chat

A private, self-destructing chat room for two people.

## Overview

Ghost Chat is a real-time, anonymous messaging app built for quick conversations that leave no trace. You don't sign up or create an account. Click a button to make a secure room, share the link with one other person, and chat. Every room deletes itself after 10 minutes, and either person can wipe it right away with a single click.

## Key Features

- **Anonymous identities:** each visitor gets a randomly generated handle (e.g. `anonymous-otter-x7k2p`) stored only in their browser.
- **One-click room creation:** rooms get unique, hard-to-guess IDs and a shareable invite link with copy-to-clipboard.
- **Two-person limit:** access is checked at the edge. The first two visitors each get a secure, HTTP-only auth cookie, and anyone after that is turned away with a "Room Full" notice.
- **Real-time messaging:** messages reach both people instantly through a publish/subscribe channel, with no page refresh needed.
- **Self-destruct timer:** a live countdown shows how long the room has left and turns red in the final minute. When it hits zero, the room and all of its messages are permanently deleted.
- **"Destroy Now" button:** either person can end the room early. This sends a real-time event that removes both people from the room and deletes all data.
- **Server-side expiry:** room data and message history are stored with matching expiry times (TTLs) in Redis, so nothing outlives the room.
- **Clear status messages** for expired, missing, full, or destroyed rooms.
- **Terminal-style dark UI** with a minimal, hacker-inspired look.

## Tech Stack

- **Frontend:** Next.js 16 (App Router), React 19 with React Compiler, TypeScript, Tailwind CSS v4
- **Data fetching:** TanStack React Query
- **API:** Elysia running inside a Next.js catch-all route handler, with Eden for end-to-end type-safe API calls from client and server
- **Database:** Upstash Redis (serverless) for room metadata, message history, and expiry times
- **Real-time:** Upstash Realtime for typed pub/sub events (new messages, room destroyed)
- **Validation:** Zod schemas for request bodies, queries, and real-time events
- **Auth/access control:** Next.js proxy (middleware) with token-based room membership
- **Tooling:** Bun, ESLint

## Technical Highlights

- Designing short-lived data with Redis TTLs so cleanup happens automatically and the app stays private by design.
- Building type-safe APIs end to end by pairing Elysia, Eden, and Zod, so frontend and backend share the same types.
- Controlling access at the edge without user accounts by using unguessable room IDs, HTTP-only cookies, and a per-room member list.
- Keeping clients in sync with real-time pub/sub events, including a shared "destroy" signal that ends the session for both people at once.

## Getting Started

This is a [Next.js](https://nextjs.org) project bootstrapped with [`create-next-app`](https://nextjs.org/docs/app/api-reference/cli/create-next-app).

### Environment variables

The app connects to Upstash Redis using `Redis.fromEnv()`. Create a `.env.local` file in the project root with your Upstash credentials:

```bash
UPSTASH_REDIS_REST_URL=your-upstash-redis-url
UPSTASH_REDIS_REST_TOKEN=your-upstash-redis-token
```

### Run the development server

Install dependencies, then start the dev server:

```bash
bun install
bun dev
# or
npm run dev
# or
yarn dev
# or
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `src/app/page.tsx`. The page auto-updates as you edit the file.

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js. Remember to add the Upstash environment variables to your Vercel project settings.

Check out the [Next.js deployment documentation](https://nextjs.org/docs/app/building-your-application/deploying) for more details.
