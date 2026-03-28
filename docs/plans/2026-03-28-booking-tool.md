# April 15 Booking Tool Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build `booking.html` — a mobile-first decision support tool Phil uses on his phone during the April 15 rec.gov lottery window to find the best available Desolation Camp date pair and book the full 7-night itinerary around it.

**Architecture:** Single self-contained HTML file. No build step, no dependencies beyond Google Fonts (same CDN as index.html). All date arithmetic computed in JavaScript at page load. Cards expand/collapse inline with a simple class toggle. Party size toggle persists across all cards.

**Tech Stack:** Vanilla HTML/CSS/JS · Google Fonts (Playfair Display, Source Sans 3, DM Mono) · Same CSS variables as index.html

---

### Task 1: Page scaffold — HTML skeleton, CSS, mobile viewport

**File:** Create `booking.html` in `/Users/wellesleychapman/Desolation-2026/`

**Step 1: Create the file with head, viewport, fonts, and CSS variables**

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>April 15 Booking Guide — Ross Lake</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,600;1,400&family=Source+Sans+3:wght@300;400;600&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --ink:          #1a1c18;
    --paper:        #f4f0e8;
    --muted:        #6b6b5e;
    --accent:       #2d5a3d;
    --accent-light: #4a8c60;
    --water:        #3b7cb8;
    --water-light:  #a8c8e8;
    --gold:         #c8902a;
    --rule:         #d4cfc0;
    --danger:       #8b3a2a;
    --card-bg:      #faf7f0;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: 'Source Sans 3', sans-serif;
    background: var(--paper);
    color: var(--ink);
    font-size: 16px;
    line-height: 1.5;
    max-width: 540px;
    margin: 0 auto;
  }
</style>
</head>
<body>
  <!-- content goes here -->
  <script>
    // JS goes here
  </script>
</body>
</html>
```

**Step 2: Open in browser, confirm warm paper background, no overflow**

---

### Task 2: Masthead + instruction strip

**Step 1: Add masthead and instruction strip HTML + CSS**

```html
<!-- MASTHEAD -->
<div class="masthead">
  <div class="masthead-inner">
    <div class="eyebrow">April 15 · 7am Lottery Window</div>
    <h1>Booking Guide</h1>
    <div class="sub">Ross Lake · Desolation Peak · 7 Nights</div>
  </div>
</div>

<!-- INSTRUCTION -->
<div class="instruction">
  Start at Priority 1. Check those Desolation dates on rec.gov first.
  If unavailable, move to Priority 2 and so on.
</div>
```

```css
.masthead {
  background: var(--ink);
  color: var(--paper);
  padding: 2rem 1.25rem 1.75rem;
  text-align: center;
}
.masthead-inner { position: relative; }
.eyebrow {
  font-family: 'DM Mono', monospace;
  font-size: 0.7rem;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--accent-light);
  margin-bottom: 0.5rem;
}
h1 {
  font-family: 'Playfair Display', serif;
  font-size: 2.25rem;
  font-weight: 400;
  line-height: 1.1;
  margin-bottom: 0.4rem;
}
.sub {
  font-size: 0.9rem;
  color: rgba(244,240,232,0.6);
  font-weight: 300;
}
.instruction {
  background: var(--accent);
  color: white;
  padding: 0.75rem 1.25rem;
  font-size: 0.85rem;
  line-height: 1.45;
  font-family: 'DM Mono', monospace;
}
```

**Step 2: Verify masthead and instruction render correctly on narrow viewport**

---

### Task 3: Party size toggle

**Step 1: Add toggle HTML below instruction strip**

```html
<!-- PARTY TOGGLE -->
<div class="party-bar">
  <span class="party-label">Party size:</span>
  <div class="toggle-group">
    <button class="toggle-btn" data-size="4" onclick="setParty(4)">4 people</button>
    <button class="toggle-btn active" data-size="2" onclick="setParty(2)">2 people</button>
  </div>
</div>
<div class="party-note" id="party-note">
  Try <strong>4-person sites</strong> first on rec.gov.
  If unavailable for any night, tap "2 people" and rebook.
</div>
```

```css
.party-bar {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 1.25rem;
  border-bottom: 1px solid var(--rule);
  background: var(--card-bg);
}
.party-label {
  font-family: 'DM Mono', monospace;
  font-size: 0.75rem;
  color: var(--muted);
  white-space: nowrap;
}
.toggle-group { display: flex; gap: 0.4rem; }
.toggle-btn {
  font-family: 'DM Mono', monospace;
  font-size: 0.75rem;
  padding: 0.35rem 0.75rem;
  border: 1px solid var(--rule);
  border-radius: 3px;
  background: var(--paper);
  color: var(--muted);
  cursor: pointer;
  min-height: 36px;
}
.toggle-btn.active {
  background: var(--accent);
  color: white;
  border-color: var(--accent);
}
.party-note {
  padding: 0.6rem 1.25rem;
  font-size: 0.8rem;
  color: var(--muted);
  background: var(--card-bg);
  border-bottom: 1px solid var(--rule);
  display: block;
}
.party-note.hidden { display: none; }
```

**Step 2: Add JS toggle function**

```javascript
let partySize = 4;

function setParty(size) {
  partySize = size;
  document.querySelectorAll('.toggle-btn').forEach(btn => {
    btn.classList.toggle('active', parseInt(btn.dataset.size) === size);
  });
  const note = document.getElementById('party-note');
  note.classList.toggle('hidden', size === 2);
}
```

**Step 3: Default should be 4 people active. Verify toggle switches correctly.**

Note: The default in the HTML above has 2 as `.active` — fix to 4:
```html
<button class="toggle-btn active" data-size="4" onclick="setParty(4)">4 people</button>
<button class="toggle-btn" data-size="2" onclick="setParty(2)">2 people</button>
```

---

### Task 4: Date arithmetic engine

**Step 1: Add the camp data and date helper to the script block**

```javascript
const CAMPS = {
  desolation: { name: 'Desolation Camp', sites: '1 group', note: 'Separate NPS permit' },
  catIsland:  { name: 'Cat Island',      sites: '4 sites', note: 'Book immediately after Desolation' },
  tenMile:    { name: 'Ten Mile Island', sites: '3 sites', note: 'Alt if Cat Island unavailable' },
  bigBeaver:  { name: 'Big Beaver',      sites: '7 sites', note: '' },
  cougar:     { name: 'Cougar Island',   sites: '2 sites', note: 'Alt for Night 1' },
  rainbow:    { name: 'Rainbow Point',   sites: '3 sites', note: 'Alt for Night 6' },
};

// Format a Date as "Mon D" (e.g. "Jul 3") or "Mon D–D" range
function fmt(date) {
  return date.toLocaleDateString('en-US', { month: 'short', day: 'numeric' });
}

// Add N days to a Date, return new Date
function addDays(date, n) {
  const d = new Date(date);
  d.setDate(d.getDate() + n);
  return d;
}

// Build a full 7-night itinerary object given Desolation Night 1 date
function buildItinerary(desolationNight1) {
  const D = desolationNight1;
  return [
    { night: 1, date: addDays(D, -2), primary: CAMPS.bigBeaver,  alt: CAMPS.cougar   },
    { night: 2, date: addDays(D, -1), primary: CAMPS.catIsland,  alt: CAMPS.tenMile  },
    { night: 3, date: D,              primary: CAMPS.desolation, alt: null            },
    { night: 4, date: addDays(D,  1), primary: CAMPS.desolation, alt: null            },
    { night: 5, date: addDays(D,  2), primary: CAMPS.catIsland,  alt: CAMPS.tenMile  },
    { night: 6, date: addDays(D,  3), primary: CAMPS.bigBeaver,  alt: CAMPS.rainbow  },
    { night: 7, date: addDays(D,  4), primary: null,             alt: null, out: true },
  ];
}

// Build the booking queue for a given D
function buildQueue(D) {
  return [
    { camp: CAMPS.desolation, date: `${fmt(D)} + ${fmt(addDays(D,1))}`, note: 'Book first — anchor the trip' },
    { camp: CAMPS.catIsland,  date: fmt(addDays(D,-1)), note: 'Night 2 staging — 4 sites, fills fast' },
    { camp: CAMPS.catIsland,  date: fmt(addDays(D, 2)), note: 'Night 5 recovery' },
    { camp: CAMPS.bigBeaver,  date: fmt(addDays(D,-2)), note: 'Night 1 arrival — 7 sites' },
    { camp: CAMPS.bigBeaver,  date: fmt(addDays(D, 3)), note: 'Night 6 west shore' },
  ];
}
```

**Step 2: Test in browser console**

```javascript
// In browser console:
const d = new Date(2026, 6, 3); // July 3
buildItinerary(d).forEach(n => console.log(n.night, fmt(n.date), n.primary?.name));
// Expected: 1 Jul 1 Big Beaver, 2 Jul 2 Cat Island, 3 Jul 3 Desolation Camp ...
```

---

### Task 5: July priority card list — render and expand/collapse

**Step 1: Add priority data array**

```javascript
// July priority pairs — Desolation Night 1 dates in priority order
const JULY_PRIORITIES = [
  { d1: new Date(2026, 6,  3), label: 'First Choice'  },
  { d1: new Date(2026, 6,  2), label: 'Second Choice' },
  { d1: new Date(2026, 6,  4), label: null },
  { d1: new Date(2026, 6,  1), label: null },
  { d1: new Date(2026, 6,  5), label: null },
  { d1: new Date(2026, 5, 30), label: null },
  { d1: new Date(2026, 6,  6), label: null },
  { d1: new Date(2026, 5, 29), label: null },
  { d1: new Date(2026, 6,  7), label: null },
  { d1: new Date(2026, 6,  8), label: null },
  { d1: new Date(2026, 6,  9), label: null },
  { d1: new Date(2026, 6, 10), label: null },
  { d1: new Date(2026, 6, 11), label: null },
  { d1: new Date(2026, 6, 12), label: null },
];

// September priority pairs
const SEPT_PRIORITIES = Array.from({ length: 13 }, (_, i) => ({
  d1: new Date(2026, 8, 4 + i),
  label: i === 0 ? 'Earliest' : null,
}));
```

**Step 2: Add card rendering function**

```javascript
function renderCard(priority, idx, isOpen) {
  const D = priority.d1;
  const itin = buildItinerary(D);
  const queue = buildQueue(D);
  const tripStart = fmt(addDays(D, -2));
  const tripEnd   = fmt(addDays(D,  4));
  const isTop = idx < 2;

  const labelHtml = priority.label
    ? `<span class="priority-badge ${idx === 0 ? 'gold' : 'green'}">${priority.label}</span>`
    : '';

  const itinRows = itin.map(n => {
    if (n.out) return `
      <div class="itin-row out">
        <span class="itin-night">Day 7</span>
        <span class="itin-date">${fmt(n.date)}</span>
        <span class="itin-camp">→ Paddle out to resort</span>
      </div>`;
    const isDesolation = n.primary === CAMPS.desolation;
    const altHtml = n.alt ? `<span class="itin-alt"> · Alt: ${n.alt.name}</span>` : '';
    return `
      <div class="itin-row ${isDesolation ? 'summit' : ''}">
        <span class="itin-night">Night ${n.night}</span>
        <span class="itin-date">${fmt(n.date)}</span>
        <span class="itin-camp">${isDesolation ? '⛺ ' : ''}${n.primary.name}${altHtml}</span>
      </div>`;
  }).join('');

  const queueRows = queue.map((q, qi) => `
    <div class="queue-row">
      <span class="queue-num">${qi + 1}</span>
      <span class="queue-body">
        <strong>${q.camp.name}</strong> — ${q.date}
        <span class="queue-note">${q.note}</span>
      </span>
    </div>`).join('') + `
    <div class="queue-fallback">
      ↳ Fallback: Ten Mile Island for Night 2 or Night 5 if Cat Island unavailable
    </div>`;

  return `
    <div class="card ${isTop ? 'card-top' : ''} ${isOpen ? 'open' : ''}" id="card-${idx}" onclick="toggleCard(${idx})">
      <div class="card-header">
        <div class="card-priority">#${idx + 1}</div>
        <div class="card-info">
          ${labelHtml}
          <div class="card-dates">Desolation: <strong>${fmt(D)} &amp; ${fmt(addDays(D,1))}</strong></div>
          <div class="card-trip">Trip: ${tripStart} – ${tripEnd}</div>
        </div>
        <div class="card-chevron">▾</div>
      </div>
      <div class="card-body">
        <div class="itin-block">
          <div class="block-label">7-Night Itinerary</div>
          ${itinRows}
        </div>
        <div class="queue-block">
          <div class="block-label">Book in this order on rec.gov</div>
          ${queueRows}
        </div>
      </div>
    </div>`;
}
```

**Step 3: Add CSS for cards**

```css
.section-header {
  padding: 1rem 1.25rem 0.5rem;
  font-family: 'DM Mono', monospace;
  font-size: 0.7rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--muted);
  border-top: 1px solid var(--rule);
}
.card {
  border-bottom: 1px solid var(--rule);
  background: var(--paper);
  cursor: pointer;
  user-select: none;
}
.card-top { border-left: 3px solid var(--gold); }
.card-header {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 1rem 1rem 1rem 1.25rem;
  min-height: 56px;
}
.card-priority {
  font-family: 'DM Mono', monospace;
  font-size: 1.1rem;
  font-weight: 500;
  color: var(--muted);
  min-width: 2rem;
}
.card-info { flex: 1; }
.priority-badge {
  display: inline-block;
  font-family: 'DM Mono', monospace;
  font-size: 0.65rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  padding: 0.15rem 0.5rem;
  border-radius: 2px;
  margin-bottom: 0.25rem;
}
.priority-badge.gold { background: var(--gold); color: white; }
.priority-badge.green { background: var(--accent); color: white; }
.card-dates {
  font-size: 0.95rem;
  line-height: 1.3;
}
.card-trip {
  font-family: 'DM Mono', monospace;
  font-size: 0.7rem;
  color: var(--muted);
  margin-top: 0.1rem;
}
.card-chevron {
  font-size: 1rem;
  color: var(--muted);
  transition: transform 0.2s;
  padding-left: 0.5rem;
}
.card.open .card-chevron { transform: rotate(180deg); }
.card-body { display: none; padding: 0 1.25rem 1.25rem; }
.card.open .card-body { display: block; }

/* Itinerary block */
.block-label {
  font-family: 'DM Mono', monospace;
  font-size: 0.65rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--muted);
  margin: 0.75rem 0 0.4rem;
}
.itin-row {
  display: grid;
  grid-template-columns: 3.5rem 3.5rem 1fr;
  gap: 0.25rem 0.5rem;
  padding: 0.3rem 0;
  font-size: 0.85rem;
  border-bottom: 1px solid var(--rule);
}
.itin-row:last-child { border-bottom: none; }
.itin-row.summit { background: rgba(45,90,61,0.06); margin: 0 -1.25rem; padding: 0.3rem 1.25rem; }
.itin-row.out { color: var(--muted); }
.itin-night { font-family: 'DM Mono', monospace; font-size: 0.7rem; color: var(--muted); padding-top: 0.1rem; }
.itin-date  { font-family: 'DM Mono', monospace; font-size: 0.7rem; padding-top: 0.1rem; }
.itin-alt   { color: var(--muted); font-size: 0.8rem; }

/* Queue block */
.queue-row {
  display: flex;
  gap: 0.75rem;
  padding: 0.4rem 0;
  border-bottom: 1px solid var(--rule);
  font-size: 0.85rem;
}
.queue-row:last-of-type { border-bottom: none; }
.queue-num {
  font-family: 'DM Mono', monospace;
  font-size: 0.75rem;
  color: white;
  background: var(--accent);
  width: 1.4rem;
  height: 1.4rem;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  margin-top: 0.1rem;
}
.queue-body { flex: 1; line-height: 1.35; }
.queue-note { display: block; font-size: 0.75rem; color: var(--muted); }
.queue-fallback {
  font-size: 0.78rem;
  color: var(--muted);
  padding: 0.5rem 0 0 2.15rem;
  font-style: italic;
}
```

**Step 4: Add render + toggle functions and call on page load**

```javascript
let openCard = null;

function toggleCard(idx) {
  const card = document.getElementById(`card-${idx}`);
  if (openCard !== null && openCard !== idx) {
    const prev = document.getElementById(`card-${openCard}`);
    if (prev) prev.classList.remove('open');
  }
  card.classList.toggle('open');
  openCard = card.classList.contains('open') ? idx : null;
}

function renderWindow(priorities, containerId, offset) {
  const container = document.getElementById(containerId);
  container.innerHTML = priorities.map((p, i) => renderCard(p, i + offset, false)).join('');
}

window.addEventListener('DOMContentLoaded', () => {
  renderWindow(JULY_PRIORITIES, 'july-cards', 0);
  renderWindow(SEPT_PRIORITIES, 'sept-cards', 100); // offset 100 to avoid ID collisions
});
```

**Step 5: Add container divs to HTML body**

```html
<div class="section-header">Primary Window — July 2026</div>
<div id="july-cards"></div>

<div class="sept-section">
  <button class="sept-toggle" onclick="toggleSept()">
    <span>September Fallback — Sept 4–20, 2026</span>
    <span id="sept-chevron">▾</span>
  </button>
  <div id="sept-body" class="sept-body">
    <div id="sept-cards"></div>
  </div>
</div>
```

**Step 6: Add September section CSS**

```css
.sept-section {
  border-top: 2px solid var(--rule);
  margin-top: 0.5rem;
}
.sept-toggle {
  width: 100%;
  background: var(--card-bg);
  border: none;
  padding: 1rem 1.25rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-family: 'DM Mono', monospace;
  font-size: 0.75rem;
  color: var(--muted);
  cursor: pointer;
  text-align: left;
  letter-spacing: 0.05em;
}
.sept-body { display: none; }
.sept-body.open { display: block; }
```

**Step 7: Add September toggle function**

```javascript
function toggleSept() {
  const body = document.getElementById('sept-body');
  const chevron = document.getElementById('sept-chevron');
  body.classList.toggle('open');
  chevron.textContent = body.classList.contains('open') ? '▴' : '▾';
}
```

**Step 8: Verify in browser**
- Tap Priority 1 card → itinerary shows July 1–7, queue shows Desolation Jul 3+4 first
- Tap Priority 2 card → Priority 1 collapses, itinerary shows June 30–July 6
- Tap September toggle → September cards appear
- Narrow the browser to 375px width (iPhone SE) — verify nothing overflows

---

### Task 6: Footer + link back to main page

**Step 1: Add footer HTML**

```html
<div class="footer">
  <a href="index.html">← Full trip planning doc</a>
</div>
```

```css
.footer {
  padding: 1.5rem 1.25rem 3rem;
  text-align: center;
  font-family: 'DM Mono', monospace;
  font-size: 0.75rem;
}
.footer a { color: var(--accent); text-decoration: none; }
```

---

### Task 7: Final verification + git commit

**Step 1: Open booking.html in browser at ~375px width**

Verify all checklist items:
- [ ] Priority 1 card shows Desolation: July 3 & 4, Trip: July 1–7
- [ ] Priority 2 card shows Desolation: July 2 & 3, Trip: June 30–July 6
- [ ] Tapping Priority 1 then Priority 2 closes Priority 1
- [ ] Booking queue order: Desolation → Cat Island ×2 → Big Beaver ×2
- [ ] Party toggle switches between 4 and 2, note disappears on 2
- [ ] September section collapsed by default, expands on tap
- [ ] September Priority 1 shows Sept 4 & 5
- [ ] No horizontal overflow at 375px

**Step 2: Commit**

```bash
git -C /Users/wellesleychapman/Desolation-2026 add booking.html docs/
git commit -m "Add April 15 booking decision tool (booking.html)"
```

**Step 3: Push**

```bash
git -C /Users/wellesleychapman/Desolation-2026 push origin main
```
