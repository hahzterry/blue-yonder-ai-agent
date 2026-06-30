# Yard Planner AI — WMS Conversational Agent Demo

**▶ Live demo: https://danielli5.github.io/blue-yonder-ai-agent/**

A clickable prototype that recreates a **Blue Yonder WMS "Door Activity"** screen and embeds a
**conversational AI agent** that re-plans the dock/yard schedule from plain-English "what-if" requests.

Built to show a SaaS customer what an AI feature *feels like* inside a tool they already use —
the gap between "we could add AI" and "here's exactly what your dock supervisor would type."

## Run it

Just open `index.html` in any browser — it's a single self-contained file (no install, no server, no API key).
To host it: drag the file onto Netlify Drop or push to GitHub Pages.

## What's real vs. mocked

- **Mocked (static):** the entire Blue Yonder chrome — top nav, tabs, toolbar, and the door/timeline
  gantt visuals. None of those links or buttons do anything (by design).
- **Real (functional):** the **Yard Planner AI** panel on the right. It parses your request, edits the
  live schedule, auto-detects and resolves double-booked docks, and updates the metrics — all in-browser.

## Demo script (≈90 seconds)

Tap the suggestion chips in order:

1. **"Move S12 to door 10 at 2pm"** — targeted change; the green bar jumps doors and times.
2. **"S8 is 2 hours late — re-plan"** — model a delayed truck; the agent cascades the fix.
3. **"Optimize the whole schedule"** — board starts with 4 red conflicts; watch them drop to 0.
4. **"Which doors are free between 2pm and 4pm?"** — a read-only query, no changes.
5. **"Reset the plan"** — restore the original board to run it again.

You can also free-type, e.g. *"what if S22 moves to door 9 at 9am"*.

Doors are named **Door 1–14** (1–7 inbound/receiving, 8–14 outbound/shipping) and shipments are
**S1–S34** — both short enough to type quickly during a live demo.

## How the agent works (talking points)

The "AI" here is **deterministic local logic**, chosen on purpose for a live demo: it never needs a key,
never times out, and never hallucinates on stage. It does three things a real LLM-backed agent would do:

1. **Intent + entity extraction** — pulls the shipment (`S12`), target door (`door 10`), time
   (`2pm`, `from 6pm to 8pm`), and relative shifts (`2 hours late`) out of free text.
2. **Constraint solving** — applies the change, detects overlapping dock assignments, and resolves them
   by relocating to a compatible free door or pushing the dwell window.
3. **Grounded response** — reports what it did with live metrics (dock utilization, late count, conflicts).

For a production build this same UX would swap the local parser for a Claude tool-use loop, where the
model calls `move_shipment`, `find_free_door`, and `optimize_docks` functions against the real WMS API.
The interaction design — and the customer's "now I get it" moment — stays identical.
