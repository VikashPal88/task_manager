# DailyTM

## Environment variables

Copy [`.env.example`](./.env.example) to `.env.local` for local development. Never
commit `.env.local` or put secret values in client-side (`NEXT_PUBLIC_`) variables.

### Required

| Variable | Purpose | Example |
| --- | --- | --- |
| `DATABASE_URL` | PostgreSQL connection used by Prisma and the app | `postgresql://user:password@host:5432/db?sslmode=require` |
| `BETTER_AUTH_URL` | Canonical public URL used by Better Auth | `https://tasks.example.com` |
| `BETTER_AUTH_SECRET` | Secret used to sign Better Auth sessions | A unique random value |

`DATABASE_URL` must point to PostgreSQL. Create the database schema before starting
the production server:

```bash
pnpm install --frozen-lockfile
pnpm exec prisma migrate deploy
pnpm run build
pnpm start
```

For a new local database, use `pnpm run db:push` instead of
`pnpm exec prisma migrate deploy` if you do not need migration history.

### Optional OAuth

The app configures GitHub and Google OAuth. Set both variables for a provider
before enabling that provider in the UI:

- `GITHUB_CLIENT_ID`
- `GITHUB_CLIENT_SECRET`
- `GOOGLE_CLIENT_ID`
- `GOOGLE_CLIENT_SECRET`

Register these callback URLs with the provider:

```text
https://YOUR_DOMAIN/api/auth/callback/github
https://YOUR_DOMAIN/api/auth/callback/google
```

For local development, replace `https://YOUR_DOMAIN` with
`http://localhost:3000`.

### Optional billing

`POLARIS_API_KEY` and `POLARIS_API_SECRET` are only checked to select the
default entitlement plan. If both are absent, new organizations use the
unlimited `SELF_HOSTED` plan. The repository does not currently call a billing
API.

### Email verification and password reset

Email verification is required for sign-up, but the current implementation logs
verification and password-reset URLs on the server instead of sending email.
Configure an email provider and replace the TODO handlers in
[`src/lib/auth.ts`](./src/lib/auth.ts) before relying on those flows in
production.

This is a [Next.js](https://nextjs.org) project bootstrapped with [`create-next-app`](https://nextjs.org/docs/app/api-reference/cli/create-next-app).

## Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.

This project uses [`next/font`](https://nextjs.org/docs/app/building-your-application/optimizing/fonts) to automatically optimize and load [Geist](https://vercel.com/font), a new font family for Vercel.

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js) - your feedback and contributions are welcome!

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/app/building-your-application/deploying) for more details.
