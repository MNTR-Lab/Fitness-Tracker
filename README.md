# Fitness Tracker — first deployable milestone

Two private accounts, 48 original workout sheets organized into 12 weeks, weigh-ins, personal goals, workout progress, and set/reps/weight logging.

## Setup (Cloudflare Workers + D1)
1. Create a **new** GitHub repository and upload **all contents of this ZIP** (including `public/workouts`).
2. On a computer with Node.js installed, run `npm install`.
3. Log into Cloudflare with `npx wrangler login`.
4. Create a database: `npx wrangler d1 create fitness-tracker-db`.
5. Copy the database ID returned by Cloudflare into `wrangler.toml`, replacing `REPLACE_WITH_YOUR_D1_DATABASE_ID`.
6. Initialize the database: `npm run db:remote`.
7. Create a private setup key: `npx wrangler secret put SETUP_KEY` and enter a long, unique random phrase. **Never commit this key to GitHub.**
8. Deploy: `npm run deploy`.
9. Open the Worker URL. Select **Create your invited account**. Use the setup key to create each of the two accounts. After two users are created, additional accounts are blocked by the server.
10. Each person logs in using their own email/password. Never share passwords.

For local testing, run `npm run db:local`, then create a `.dev.vars` file with `SETUP_KEY=your-local-test-key`, and run `npm run dev`. Do not commit `.dev.vars`.

## What works now
- Two separate password-protected user accounts; session expiry after 30 days; user-scoped data access.
- 48 original workout sheets displayed by week/day, with independent completion for each user.
- Optional pre-workout weigh-in; manual weight entry; progress chart; goal weight.
- Resume an unfinished workout; log exercise names, sets, reps, and lifted weights; finish a workout.
- History and dashboard; responsive layout for mobile and desktop.

## Next milestone / limitations
- Original sheets are displayed as images; exercise text is **not yet automatically transcribed** into individual rows. Exercise names are currently entered manually during training.
- Workout schedules are selected manually, not tied to fixed weekdays.
- Account creation is limited to two users; password reset is not yet implemented.
- The workout images are reproduced from the user-supplied archive; ensure you have the necessary rights to host them, especially if you later make the app public.
- The ZIP is deployment-ready source code, but deployment and live testing have **not** been performed in your Cloudflare account.
