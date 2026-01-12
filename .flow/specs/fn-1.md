# fn-1 Add job statistics dashboard

## Overview
Create a simple HTML dashboard that displays statistics from the job applications tracker.

## Scope
- Read `applications.json`
- Generate static `dashboard.html` with stats

## Approach
1. Parse applications.json
2. Calculate statistics (totals, by type, by status)
3. Generate HTML with inline CSS
4. No external dependencies

## Quick commands
- `python3 -m http.server 8000` to preview dashboard
- `open dashboard.html` to view

## Acceptance
- [ ] Dashboard displays total application count
- [ ] Shows breakdown by type (easy_apply vs external)
- [ ] Shows breakdown by status
- [ ] Valid HTML5 file

## References
- applications.json in repo root
