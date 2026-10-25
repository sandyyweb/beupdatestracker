# Boxedge Updates Tracker

Updated October 25, 2026. The full step-by-step guide is in
Boxedge_Updates_Tracker_Complete_Setup_Guide.pdf - follow that when setting up.

## Files
- index.html                 - the whole dashboard (single file, host anywhere static)
- be-logo.png, favicon-BE.webp (and be-favicon.png) - logo and browser icons, upload next to index.html
- database.sql               - main tables, admin/manager security, create/delete manager functions, starter accounts
- meta_ads.sql               - Meta Ads Tracker table (13 Kwality campaigns as starter rows)
- meta_ads_metrics.sql       - extra Meta Ads columns (Results, Cost/Result, Reach, Impressions, Amount Spent, CPM, CPC, Views, Clicks)
- google_ads.sql             - Google Ads Tracker table
- google_ads_metrices.sql    - extra Google Ads columns (Clicks, Impressions, Cost, CTR, Avg. CPC, Interaction Rate, Calls, Conversions)
- meta_insta.sql             - Meta and Instagram tables (accounts and posts, including Instagram stories)
- boost_remarks.sql          - per-month remarks for Meta Boosting
- manager_access.sql         - manager permissions upgrade
- change_history.sql         - Change History log and triggers (history kept for 7 days)
- Data to fill after project setup/  - optional old data scripts (.txt)
- Change History Delete SQL in Tracker Dashboard.pdf - queries for clearing Change History sooner

## Re-creating the project from scratch
1. Create a new Supabase project.
2. Authentication > Users > Add user: create the admin login
   (email + password, tick "Auto Confirm User").
3. Open database.sql and check section 3: the admin email (adminbe@gmail.com)
   must match the user from step 2; change the display name if you like.
4. Supabase > SQL Editor > New query. Paste and Run, in this order:
   database.sql, meta_ads.sql, meta_ads_metrics.sql, google_ads.sql,
   google_ads_metrices.sql, meta_insta.sql
   (If database.sql says "No admin created", fix the email in step 3 and run it again.)
5. Optional: run the old data scripts from "Data to fill after project setup", in month order.
6. Run boost_remarks.sql, then manager_access.sql, then change_history.sql (last,
   so the old-data scripts are not logged). Every SQL file is safe to run again.
7. Supabase > Project Settings > API: copy the Project URL and the anon public key
   into the two constants SUPABASE_URL and SUPABASE_KEY near the top of the
   main <script> in index.html (search for SUPABASE_URL). Use only the anon key.
8. Upload index.html, be-logo.png and favicon-BE.webp to your hosting and sign in as the admin.

## Roles
- Admin  : add / edit / undo / delete everything, add / disable / delete managers
           (sidebar > "Team & Managers"), view Change History.
- Manager: can add and edit posts, boosts, Meta and Instagram entries, boost remarks,
           and Meta Ads / Google Ads campaigns. Cannot delete campaigns, cannot add,
           edit or delete client accounts, and cannot see Team & Managers or Change History.
           (Needs manager_access.sql. Without it, managers are view-only.)
           Managers can always choose the month, filter, export CSV and Print / Save PDF.

## Change History
- Records what changed, when and who did it, on every tracker table (admin only).
- History is kept for 7 days, then deleted automatically. Posts, boosts and campaigns
  are never affected by this clean-up.
- To change the number of days, edit the '7 days' lines in change_history.sql and run
  it again in the SQL Editor (and the matching text on the Change History page).

## Notification bell (all pages, after login)
Settings are at the top of the notification block in index.html (search for EVENT_DAYS):
- POST_DAYS = 3   Reels & Posts and Meta and Instagram: no post for 3 or more days
- BOOST_DAYS = 3  Meta Boosting: not boosted for 3 or more days
- STORY_DAYS = 1  Meta and Instagram: no Instagram story logged today
- ADS_DAYS = 3    Meta Ads / Google Ads: campaign ends within 3 days
- EVENT_DAYS = 6  New manager added and dashboard changes (admin only): last 6 days
Keep EVENT_DAYS at or below the days Change History keeps (7).
"Mark as read" hides post, boost, story and campaign alerts for the rest of the day;
they return the next day until the task is done. Change alerts stay read once marked.
