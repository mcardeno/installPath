# Install Path — brief (Rev 0.2, 2026-11-25)

History: Rev 0.0 first published to GitHub Pages (public repo installPath) 2026-11-25. Rev 0.1 adds the Microsoft Project Export, full-scope S-curves, one project-long time axis and shorter S-curves. Rev 0.2 adds the "Show original plan for installed work" toggle.

## What it is
A what-if board for the lab installation schedule, shown on a big screen in meetings. Move work around, change hours, slip deliveries or change the technician count, and see the finish date, weekly loading and S-curves update straight away against the Baseline. Separate app from Schedule Path.

The Baseline is the schedule as first issued to the team. (Internal note: it was built from working plan "2E"; the app and its outputs never show that name, by decision on 2026-11-25, because this is everyone's first view of the schedule.)

## How it runs
- One file: `index.html`. No outside libraries (Excel and PDF are read and written by the page itself). Fonts from Google Fonts (falls back to system fonts offline).
- Published with no project data. The user opens the data file (.xlsx) in the browser; nothing is uploaded. The last session is kept in this browser (localStorage) so it can be continued.
- `?today=YYYY-MM-DD` in the address overrides "today" (testing or rehearsing a future meeting).
- Opening "Install Path - Data - 2026-11-25.xlsx" and downloading the Schedule Summary with no changes reproduces the reference report "Install Path - Schedule Summary - 2026-11-25 0756.pdf" (only the generated time and the Today line differ).

## Two copies, one source
- `index.html` (GitHub Pages) is the source. The phone/claude.ai artifact (https://claude.ai/artifact/8hJ9fx1ipe9ZHvRtxxmkPK, private) is generated from it by stripping the html/head/body wrapper; the code is identical. Every update: change `index.html`, regenerate and republish the artifact, upload `index.html` to GitHub.
- Both show the same revision label in the header ("Rev 0.2"). The label changes only when an update is published to both copies, never for work in progress; matching labels = in sync. Revision numbers change only at milestones.
- Data is not synced between devices (decided 2026-11-25): each device opens the data file, and the Save as file carries a meeting's changes from one device to another.
- In the artifact, saving goes through the claude.ai save prompt (the `downloads` capability); on GitHub Pages files download or use the browser's save dialog.

## Data file (tabs)
Settings · Milestones (NAV = navigation consoles 2027-04-15, FDCR 2027-05-19) · Rows (54: 27 system windows + 27 consoles/racks) · Tags (249, with part number, Row ID, baseline status, status, actuals, forecasts, original planned window for tags already installed, comment, Task ID) · Parts (158 part numbers, baseline hours and hours) · Calendar (BC statutory + company holidays and shutdowns) · Changes.
"Baseline" columns hold the Baseline and are never changed by the app. The Save as file writes the same tabs back (plus output tabs), so it reopens and carries on with the same Baseline.

## Screen and files
- Schedule tab: full-width timeline (one bar per row, faint Baseline bar under each) with the weekly loading chart directly underneath on exactly the same dates; S-curves below; the rows table (dates, hours, tags) below that. Tapping a row name in the timeline jumps to that row in the table with its tags open. On a phone the timeline and loading scroll sideways together.
- Milestones (navigation consoles and FDCR deliveries) are always marked as milestones: ◆ diamond, orange, labelled "Milestone: …", on screen, on the timeline and charts, and in a "Milestones" group on the Schedule Summary.
- Open… opens a data file or a saved working file. Save as… writes the working file (.xlsx that reopens; Chrome/Edge show a save dialog, other browsers download it; in the artifact the claude.ai save prompt). Ctrl/Cmd+S also saves.
- Download… is for reports: Schedule Summary (one landscape PDF page: key figures, milestones, timeline, loading on the same dates) and/or Full report (key figures + loading, S-curves, what changed).
- All charts and the timeline share one time axis covering the whole project: from the first install in autumn 2026 to the last in September 2027. The timeline's first row, "Installed to date", marks each install date of finished tags. The loading chart shows remaining work from the first week that has any.
- S-curves cover all 249 tags. Baseline line = finished tags on their original planned windows + remaining work on the Baseline windows, so planned vs actual is visible for the work already done (at Nov 25, 2026: about 845 h planned vs 891 h installed). Actual = finished tags at their installed dates. Forecast = actual + remaining work as currently planned. S-curves are short and side by side on wide screens.
- "Show original plan for installed work" (checkbox above the timeline, off by default, remembered per browser): adds one row per SWBS with finished tags under "Installed to date", with a dashed bar for the original planned window and green marks at the actual install dates, labelled early/late. Screen only; reports stay clean.
- Original planned dates for installed tags (resolved 2026-11-25): the tag sheet had reset them to Nov 27+; the team supplied the original planned request/receipt/install dates (each planned install is 7 days after its actual install; Mark confirmed these are the real dates). The data file uses planned receipt -> planned install as each finished tag's Baseline window. At Nov 25, 2026 the Baseline shows about 845 h planned vs 891 h installed.
- Microsoft Project Export (header button): an .xlsx for Project's import wizard. Read Me (import steps and assumptions), Tasks (Outline Level 1 SWBS + a Milestones group, 2 tag, 3 steps Request/Receive/Install/Engineer Reviewed; Duration, Predecessors with calendar-day lags, Constraint Type/Date, Actual Start/Finish, % Complete, Milestone, Text1 fixed task ID, Start1/Finish1 = Install Path dates, Baseline Start/Finish, Number1 = install hours, Notes), Calendar (non-working dates to enter in Project). Assumptions to confirm with the team: desktop Project 2016+ / Project for Microsoft 365; calendar entered by hand. No resources yet (step 2), no cable milestone yet.
- Unsaved changes: shown in the header; closing the tab or opening another file with unsaved changes asks first. The browser also keeps the last session, but the saved file is the real backup (incognito keeps nothing).

## Decisions (from the planning conversation)
- Rows: one row per system install window, with consoles and racks on their own rows.
- Dates: edit start (duration kept, finish moves), duration (finish moves) or finish (duration follows). Durations are working days. Drag a bar to move it, drag its right end to stretch it.
- Requests and receipts follow the install: receipt 7 days before the install start, request 14 days before receipt, never before the 2027-01-11 restart. Done steps keep their actual dates.
- Factory consoles have no request step; receipt = delivery milestone. A row can't start before its delivery or before any forecast/late receipt ("held by …"). Slipping a delivery pushes the linked console installs.
- Hours are per part number (same part = same hours). Hours and durations are independent: hours change the loading, not the dates.
- Loading = install hours spread evenly over each row's working days, plus 0.25 h paperwork per remaining request/receipt step on that step's date. Capacity = technicians × hours/day × working days. Overtime = hours above capacity, summed by week. Baseline: peak 232 h, overtime 729 h, last install Sep 17, 2027 (all but FDCR Aug 31).
- Engineer Reviewed = 7 days after install finish (next working day), never before 2027-05-03.
- Look-ahead counts from today, 2 weeks by default or to a chosen date. Late items = planned date passed, not done. Each line: mark done (with date), reforecast (moves the schedule), or comment. Install lines are per row; request/receipt per tag; reviews per row (and per SWBS for already-installed tags).
- Undo for every step; Reset = plan back to the Baseline (status updates kept) or back to the file as opened.
- Save as file = reopenable tabs + Summary, Tag Dates (every planned/actual date with Task ID and basis, for the MS Project import), Weekly Loading, S-curve data, Look-ahead, Late Items.
- Look: Penpath style — flat background, outline-only, blue outlines for controls only, Lato with a serif wordmark; day and night modes.
- Console cables are not a milestone in this app (status still being discussed with the team).

## Not yet / ideas
- "Set current as new baseline" option.
- Partial progress (%) on long installs.
