# CLUTCH ZONE — Premium Match Schedule Update

## What's changed
- Match Schedule now lists **all admin-created, not-yet-completed tournaments**, not just one (even if room access is not released yet).
- Schedule excludes tournaments marked completed.
- Admin → Room Access lists each tournament with an **APPROVE DONE** action.
- Confirming completion marks the tournament completed, hides its room release, and removes it from the public Match Schedule.
- Admin Update type selector (INFO, SUCCESS, TOURNAMENT, ROOM, IMPORTANT) has premium dark/gold styling.
- Match cards use a gold/yellow background with dark readable text.
- Tournament times display in **12-hour AM/PM format**.
- Every scheduled tournament has a live days/hours/minutes/seconds countdown.
- Each card has a **VIEW ID / PASSWORD** shortcut that opens the Room ID Password page, where the player enters their tracking code.

## Required Supabase step
Before uploading this website build, open Supabase → SQL Editor and run `CLUTCH_ZONE_TOURNAMENT_COMPLETION.sql` once. It adds the `is_completed` flag used to keep completed tournaments off the public schedule. No existing tournament records are deleted; existing tournaments default to not completed.

## Deployment
Upload the contents of this ZIP to the same hosting location as the current site, preserving `config.js`, `app.js`, `style.css`, `index.html`, and the logo/video assets. Do not expose the Supabase service-role key in `config.js`; use only the project's publishable/anon key and enforce admin permissions in database functions/policies.

## v7 additions
- Room ID / Password cards use a larger, schedule-like responsive layout. Individual COPY ID / COPY PASSWORD and COPY ALL DETAILS actions are available on released rooms.
- Player navigation now says NOTE PAD/ CALCULATOR. After unlocking with a valid tracking code, players can calculate arithmetic expressions and save the expression/result into their private notes using the existing Supabase note system.
- In the authenticated admin panel, click the website logo 10 times quickly to reveal the Admin Calculator Dashboard. It shows tournament record count, income totals from saved calculator entries, and saved calculations. Admin calculator entries are stored in this browser's local storage, so they do not sync to another device/browser. Match count reflects tournament records currently loaded from Supabase.
- This is a front-end update. Live Supabase permissions and deployment behavior still need to be checked on the deployed website.


## v8 calculator fixes
- Player calculator keypad is read-only and uses button taps only; tapping 0–9/operators will not open the phone keyboard. The note editor remains a normal text area and can open the keyboard.
- In the logged-in Control Center/Admin Panel, click/tap the website logo 5 times quickly (within about 2.2 seconds) to reveal the private Admin Calculator Dashboard.
- Admin calculator saves remain in this browser/device local storage; no additional SQL table is required for this local-only version.

## Persistent Admin Calculator Dashboard (v9)
- Run `CLUTCH_ZONE_ADMIN_CALCULATOR.sql` once in Supabase Dashboard → SQL Editor before uploading this version.
- The admin calculator saves each title, expression, result, income amount, and timestamp in `public.admin_calculator_entries`.
- RLS restricts reading/inserting/deleting entries to the authenticated admin who owns the row, verified through `public.admin_roles` (`role='admin'`). No service-role key is used in the website.
- After the migration, log in to Control Center, tap/click the website logo five times quickly, then calculate and save. Saved entries load from Supabase across devices for the same admin account.
- If the SQL migration has not been run, the dashboard will show a database error and entries will not be saved; do not assume a save succeeded.


## v10 update — live calculator + admin logo shortcut
- User and admin calculator result previews update as the expression changes; use Save to persist the valid result.
- Admin dashboard shortcut listens on the full CONTROL CENTER / ADMIN PANEL brand header; tap/click five times within five seconds while signed in as an authorized admin.
- User calculator expression field remains read-only with `inputmode=none` so tapping calculator buttons does not summon the phone keyboard. The Note Pad textarea still opens the keyboard normally.
- Admin calculator records require `CLUTCH_ZONE_ADMIN_CALCULATOR.sql` to be run in Supabase SQL Editor.


## v10 usability updates
- Player calculator result updates live as keypad buttons are tapped; calculator field is read-only so the mobile keyboard does not open. Use **SAVE TO NOTE PAD** to save the expression/result as a note. Authorized admins can view each player's saved notes in the admin **NOTE PAD / CALCULATOR** section.
- Admin calculator result updates live while entering an expression.
- Admin website logo tap handling now listens directly on the logo; five taps within 2.2 seconds while the authenticated Control Center is open reveal and scroll to the Supabase-backed Admin Calculator Dashboard.
- Admin section tabs have distinct icons, including 🏆 WINNER 🏆, 👤 PLAYER DATA, 🔐 ROOM ID / PASSWORD, 🎬 TUTORIAL, 🔔 UPDATES, 💬 WORK CHAT, 📝 NOTE PAD / CALCULATOR, and 👥 GROUP CHAT.
- Run `CLUTCH_ZONE_ADMIN_CALCULATOR.sql` in Supabase SQL Editor once if it has not already been applied.

## v11 Calculator and Control Center updates
- User calculator shows a live result above the keypad, supports the `=` key, and uses a single `SAVE TO NOTE PAD` action. Calculator buttons do not open the phone keyboard; the regular note editor remains typeable.
- Calculator operators display as ×, ÷ and − while calculations normalize those symbols correctly.
- Admin calculator now has its own on-screen keypad and live result above it.
- Tap/click the full CONTROL CENTER / ADMIN PANEL logo block five times quickly while logged in to reveal the admin calculator dashboard.
- Admin calculator entries continue to save to Supabase `admin_calculator_entries`; run `CLUTCH_ZONE_ADMIN_CALCULATOR.sql` in Supabase SQL Editor if not already applied.

## v12 fixes
- Fixed the calculator script scope issue that could trigger `ReferenceError: $ is not defined` by keeping calculator handlers inside the main app scope.
- User calculator save now calls the existing `nalox_notes_save` RPC and shows the site's styled modal on success or error.
- The private admin calculator dashboard now opens after five rapid clicks/taps specifically on the `ADMIN PANEL` heading (after an authorized admin login), rather than on the logo block.
- Re-run `CLUTCH_ZONE_ADMIN_CALCULATOR.sql` only if its table/policies have not yet been created; no duplicate migration is needed if it already ran successfully.
