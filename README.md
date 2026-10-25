# Fitness Tracker — first deployable milestone

Two private accounts, 48 original workout sheets organized into 12 weeks, weigh-ins, personal goals, workout progress, and set/reps/weight logging.

## Deployment — Cloudflare D1 already initialized
The `fitness-tracker` D1 database has been created in your Cloudflare account and its six tables and two indexes were installed on October 24, 2026. Its binding is already included in `wrangler.toml`. **Do not create another database.**

### Deploy from a local computer
1. Ensure Node.js is installed, then run `npm install` and `npx wrangler login`.
2. Enter `npx wrangler secret put SETUP_KEY` after your first `npm run deploy` (or configure the secret through Cloudflare dashboard after the Worker exists). Choose a long, random secret and keep it private.
3. Run `npm run deploy`, then open the resulting Worker URL.
4. Select **Create your invited account** and enter the setup key to register each of the two intended users. Once two users exist, account creation is blocked.

### Git-connected deployment
Cloudflare dashboard → Workers & Pages → Create application → Connect GitHub → select `MNTR-Lab/Fitness-Tracker`, then configure the Worker build/deploy integration according to Cloudflare's current interface. The project uses `wrangler.toml` and needs the `SETUP_KEY` Worker secret prior to registration. Avoid publishing credentials.

### Important privacy note
The linked GitHub repository currently appears to be **public**. Its `public/workouts` content is also meant to be served as public static assets, even though user accounts protect workout logs. To keep workout scans restricted, move the repository to private and later migrate protected images to authenticated storage; changing repository visibility alone does not secure files already exposed as public website assets. Ensure you have rights to redistribute the workout scans.

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

## Milestone 3: Registration troubleshooting
- Reduced PBKDF2 iterations from 210,000 to 100,000 to reduce Worker CPU demand during registration and sign-in. Existing accounts created with the prior setting would need migration; verify account count before relying on this change.
- Matched Worker name to the Git-connected Cloudflare project `fitness-tracker`.
- This is a targeted mitigation based on the registration symptom, not a confirmed root cause. If the issue persists, enable Worker observability and inspect runtime errors.
- Rotate the SETUP_KEY after both accounts are created because the original value was shared in chat.


## Milestone 4: Registration diagnostics
- The registration endpoint now returns a safe `diagnosticCode` on internal failures: `SETUP_DB_COUNT`, `SETUP_PASSWORD_HASH`, `SETUP_USER_INSERT`, or `SETUP_SESSION_CREATE`. No passwords, setup keys, or SQL error details are sent to the browser.
- If `SETUP_SESSION_CREATE` is returned, the user record may have been created successfully. **Try signing in** rather than creating a duplicate account.
- No database schema changes. No changes to password hashing or existing credentials.
- To see the code, use the browser's Network panel, select the `/api/setup` request, and inspect its Response JSON. The UI may only show the generic error.


## Coded workouts (in progress)
- `public/workouts-coded.json` contains draft exercise data for Week 1 Day 1 only; original wording requires final review.
- Remaining workouts intentionally display “conversion pending” instead of fabricated prescriptions.
- Exercises show expandable original instructions and set-by-set weight/reps fields; saved via existing D1 `exercise_logs`.
- Preserve `public/workouts/*.jpg` as reference during transcription.
- This is a functional staged milestone, **not** completion of all 48 conversions.

## Milestone 6 — program naming and colorful theme
- The existing 12-week plan is labeled **12 Weeks of Pain and Suffering** in the dashboard and workout library.
- Bright purple/pink/teal styling replaces the dark interface.
- No database migration; workout progress and logins remain unchanged.
- Future additional programs require separate program IDs and program-specific progress before they are added.
