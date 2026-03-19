# NuPhoenix Scheduler — Claude Code Context

## Project Overview
Single-file HTML/JS/CSS application: `scheduler.html` (~8000+ lines)
No build step, no external dependencies (except Anthropic API for AI assistant feature).
Runs from `file://` — no server needed.

## Architecture
- **All-in-one**: HTML, CSS, and JS in a single file
- **Data source**: `M1_REAL_JOBS` array (hardcoded job data from Nupress M1 ERP)
- **Rendering**: Vanilla JS DOM manipulation, no framework
- **State**: localStorage for persistence between sessions (keyed `nupress_*`)

## Key Functions (in order of execution)
1. `genJobs()` — Builds `jobs[]` array from `M1_REAL_JOBS`, schedules on machines, adds simulation layer
2. `recalcCascade()` — Recalculates job timing cascades per machine
3. `renderSchedule()` → `renderStats()` + `renderGantt()` + `renderLatePanel()`
4. Login flow: `attemptLogin()` → `hideLoginOverlay()` → `renderSchedule()`

## File Structure
- Lines 1-800: HTML/CSS
- Lines 800-1505: More HTML (login overlay, modals, panels)
- Lines 1505-1510: Core JS constants (TODAY, HOUR_PX, scale)
- Lines 1510-1730: Machine/employee/shift config, M1_REAL_JOBS data
- Lines 1730-1950: genJobs(), simulation layer, recalcCascade()
- Lines 1950-2400: Rendering (Gantt, stats, late panel, job details)
- Lines 2400-4000: Advanced features (allocation, scrap, corrections)
- Lines 4000-5200: AI assistant, reports
- Lines 5200-6800: Setup, user management, kiosk/Pi mode
- Lines 6800-7000: Init block (genJobs call, session check)
- Lines 7000-8100: Login, auth, utilities

## Login (Demo Mode)
- Username: `test` / Password: `test` → Full admin access
- Any other credentials rejected

## Common Issues
- localStorage from previous sessions can cause stale state
- Init block clears `nupress_*` localStorage on each load (except current_user)
- Simulation layer uses Math.random() — results vary per page load
- Jobs scheduled forward from TODAY; late jobs need end > due (not just delayed)

## When Editing
- Make surgical edits — do NOT rewrite large sections
- Test changes by checking JS syntax: extract the `<script>` block and run `new Function(code)`
- The file is too large to read in full — use line ranges and search
