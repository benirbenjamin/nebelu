# NebeluRw Service Portal

React + Vite frontend and Express API deployed together on **Vercel**, with Prisma/PostgreSQL and **Resend** for OTP and notification emails.

## Deploy to Vercel
1. Push this repository to GitHub and import `benirbenjamin/nebelu` into Vercel.
2. Set the project root to the repository root. Framework: Vite. Build command: `npm run build`; output: `dist`.
3. Add environment variables in Vercel: `DATABASE_URL` (managed PostgreSQL connection string, e.g. Neon or Supabase), `JWT_SECRET` (long random secret), `ADMIN_EMAIL=benirabok@gmail.com`, `RESEND_API_KEY`, `RESEND_FROM` (a sender on a domain verified in Resend), and `OTP_TTL_MINUTES=10`.
4. Run `npx prisma db push --schema server/prisma/schema.prisma` once against the production database. You can run it locally with production `DATABASE_URL`, or use a deployment job.
5. Deploy. Frontend calls `/api` on the same Vercel domain. No Render service or SMTP configuration is used.

## Local development
- Install dependencies: `npm install`
- Copy `server/.env.example` to `.env` in the project root and set credentials.
- Run `npm run db:push` then `npm run dev`. Vite runs at `http://localhost:5173`; for local API development run `npm start` in another terminal on port 4000 and set `VITE_API_URL=http://localhost:4000` in a local `.env` file.

## Features
- Final service catalogue prices and step-by-step booking wizard.
- Email OTP verification before request submission and dashboard access.
- Client dashboard with request history, progress timeline, price proposals, decisions and report links.
- Admin dashboard determined server-side by `ADMIN_EMAIL`; update service status, agreed amount, notes and report/image URLs.
- Resend sends verification codes and request/price/status notifications.

## Security and launch notes
Never commit secrets. Configure a verified Resend sender domain and production PostgreSQL with backups. Use HTTPS and publish a privacy policy. The admin email is checked on the server and is not exposed as a client-side secret. This is a deployable starter project; production deployment still requires your Vercel and database setup.
