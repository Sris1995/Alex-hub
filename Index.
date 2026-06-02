<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<meta name="theme-color" content="#3b6bcc">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<meta name="apple-mobile-web-app-title" content="Alex Hub">
<title>Alex Hub</title>
<script src="https://accounts.google.com/gsi/client" async defer></script>
<style>
  /* ── Reset & Base ─────────────────────────────────── */
  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
  :root {
    --blue: #3b6bcc;
    --blue-light: #eef2fb;
    --bg: #f0f4f8;
    --white: #ffffff;
    --text: #1a1a2e;
    --muted: #6b7280;
    --border: #e5e7eb;
    --red: #ef4444;
    --orange: #f97316;
    --green: #22c55e;
    --nav-h: 68px;
    --safe-bottom: env(safe-area-inset-bottom, 0px);
  }
  html, body { height: 100%; background: var(--bg); font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; color: var(--text); overscroll-behavior: none; }

  /* ── Layout ───────────────────────────────────────── */
  #app { display: flex; flex-direction: column; height: 100%; max-width: 480px; margin: 0 auto; position: relative; }
  #screen { flex: 1; overflow-y: auto; padding-bottom: calc(var(--nav-h) + var(--safe-bottom) + 16px); }
  .page { display: none; padding: 0 16px; }
  .page.active { display: block; }

  /* ── Header ───────────────────────────────────────── */
  .page-header { padding: 52px 16px 16px; background: var(--bg); position: sticky; top: 0; z-index: 10; }
  .page-header .eyebrow { font-size: 11px; font-weight: 700; letter-spacing: .08em; text-transform: uppercase; color: var(--muted); margin-bottom: 2px; }
  .page-header h1 { font-size: 26px; font-weight: 800; color: var(--text); }

  /* ── Cards ────────────────────────────────────────── */
  .card { background: var(--white); border-radius: 16px; padding: 16px; margin-bottom: 12px; box-shadow: 0 1px 3px rgba(0,0,0,.07); }
  .card-title { font-size: 11px; font-weight: 700; letter-spacing: .07em; text-transform: uppercase; color: var(--muted); margin-bottom: 12px; }

  /* ── Tasks ────────────────────────────────────────── */
  .task-item { display: flex; align-items: flex-start; gap: 10px; padding: 8px 0; border-bottom: 1px solid var(--border); }
  .task-item:last-child { border-bottom: none; }
  .task-cb { width: 20px; height: 20px; border-radius: 50%; border: 2px solid var(--border); flex-shrink: 0; margin-top: 1px; cursor: pointer; display: flex; align-items: center; justify-content: center; transition: all .15s; }
  .task-cb.done { background: var(--green); border-color: var(--green); }
  .task-cb.done::after { content: '✓'; color: white; font-size: 11px; font-weight: 700; }
  .task-body { flex: 1; min-width: 0; }
  .task-name { font-size: 14px; font-weight: 500; line-height: 1.3; }
  .task-name.done { text-decoration: line-through; color: var(--muted); }
  .task-meta { display: flex; gap: 6px; margin-top: 4px; flex-wrap: wrap; }
  .badge { font-size: 10px; font-weight: 600; padding: 2px 7px; border-radius: 20px; }
  .badge.urgent { background: #fef2f2; color: var(--red); }
  .badge.soon { background: #fff7ed; color: var(--orange); }
  .badge.ok { background: #f0fdf4; color: var(--green); }
  .badge.cat { background: var(--blue-light); color: var(--blue); }
  .badge.pts { background: #f3f4f6; color: var(--muted); }

  /* ── Calendar ─────────────────────────────────────── */
  .cal-event { display: flex; gap: 12px; padding: 8px 0; border-bottom: 1px solid var(--border); align-items: flex-start; }
  .cal-event:last-child { border-bottom: none; }
  .cal-time { font-size: 11px; color: var(--muted); min-width: 52px; font-weight: 500; padding-top: 2px; }
  .cal-dot { width: 8px; height: 8px; border-radius: 50%; background: var(--blue); flex-shrink: 0; margin-top: 5px; }
  .cal-name { font-size: 14px; font-weight: 500; line-height: 1.3; }
  .cal-desc { font-size: 12px; color: var(--muted); margin-top: 2px; }

  /* ── Email ────────────────────────────────────────── */
  .email-item { padding: 10px 0; border-bottom: 1px solid var(--border); cursor: pointer; }
  .email-item:last-child { border-bottom: none; }
  .email-from { font-size: 13px; font-weight: 600; margin-bottom: 2px; }
  .email-subject { font-size: 13px; color: var(--text); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .email-preview { font-size: 12px; color: var(--muted); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; margin-top: 1px; }
  .email-unread .email-from { color: var(--blue); }

  /* ── Quick Ref ────────────────────────────────────── */
  .ref-item { display: flex; gap: 8px; padding: 8px 0; border-bottom: 1px solid var(--border); font-size: 13px; }
  .ref-item:last-child { border-bottom: none; }
  .ref-key { font-weight: 700; min-width: 90px; color: var(--text); }
  .ref-val { color: var(--muted); }

  /* ── Bottom Nav ───────────────────────────────────── */
  #nav { position: fixed; bottom: 0; left: 50%; transform: translateX(-50%); width: 100%; max-width: 480px; height: calc(var(--nav-h) + var(--safe-bottom)); background: var(--white); border-top: 1px solid var(--border); display: flex; align-items: flex-start; padding-top: 8px; z-index: 100; box-shadow: 0 -2px 12px rgba(0,0,0,.06); }
  .nav-btn { flex: 1; display: flex; flex-direction: column; align-items: center; gap: 3px; cursor: pointer; padding: 4px 0; border: none; background: transparent; color: var(--muted); font-size: 10px; font-weight: 600; letter-spacing: .02em; transition: color .15s; }
  .nav-btn svg { width: 22px; height: 22px; stroke-width: 1.8; }
  .nav-btn.active { color: var(--blue); }

  /* ── FAB ──────────────────────────────────────────── */
  #fab { position: fixed; bottom: calc(var(--nav-h) + var(--safe-bottom) + 16px); right: 20px; width: 52px; height: 52px; border-radius: 50%; background: var(--blue); color: white; border: none; font-size: 24px; cursor: pointer; box-shadow: 0 4px 16px rgba(59,107,204,.4); display: flex; align-items: center; justify-content: center; z-index: 99; transition: transform .15s; }
  #fab:active { transform: scale(.93); }

  /* ── Modal ────────────────────────────────────────── */
  .modal-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,.4); z-index: 200; align-items: flex-end; }
  .modal-overlay.open { display: flex; }
  .modal { background: var(--white); border-radius: 24px 24px 0 0; padding: 24px 20px calc(24px + var(--safe-bottom)); width: 100%; max-width: 480px; margin: 0 auto; }
  .modal h2 { font-size: 18px; font-weight: 700; margin-bottom: 16px; }
  .modal input, .modal select, .modal textarea {
    width: 100%; padding: 12px 14px; border: 1.5px solid var(--border); border-radius: 12px;
    font-size: 15px; margin-bottom: 10px; outline: none; font-family: inherit; background: var(--bg);
    color: var(--text);
  }
  .modal input:focus, .modal select:focus, .modal textarea:focus { border-color: var(--blue); }
  .modal-row { display: flex; gap: 10px; }
  .modal-row input, .modal-row select { flex: 1; }
  .btn-primary { width: 100%; padding: 14px; background: var(--blue); color: white; border: none; border-radius: 14px; font-size: 16px; font-weight: 700; cursor: pointer; margin-top: 4px; }
  .btn-cancel { width: 100%; padding: 12px; background: transparent; color: var(--muted); border: none; font-size: 15px; cursor: pointer; margin-top: 4px; }

  /* ── Sign-in screen ───────────────────────────────── */
  #signin-screen { display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 20px; padding: 40px 24px; text-align: center; }
  #signin-screen .logo { width: 72px; height: 72px; background: var(--blue); border-radius: 22px; display: flex; align-items: center; justify-content: center; font-size: 36px; box-shadow: 0 4px 20px rgba(59,107,204,.35); margin-bottom: 8px; }
  #signin-screen h1 { font-size: 28px; font-weight: 800; }
  #signin-screen p { color: var(--muted); font-size: 15px; max-width: 280px; line-height: 1.5; }

  /* ── Loading ──────────────────────────────────────── */
  .spinner { width: 20px; height: 20px; border: 2.5px solid var(--border); border-top-color: var(--blue); border-radius: 50%; animation: spin .7s linear infinite; margin: 20px auto; }
  @keyframes spin { to { transform: rotate(360deg); } }
  .empty { text-align: center; color: var(--muted); font-size: 14px; padding: 24px 0; }

  /* ── Schedule block ───────────────────────────────── */
  .sched-block { display: flex; gap: 12px; align-items: center; padding: 10px 12px; background: var(--blue-light); border-radius: 12px; margin-bottom: 8px; }
  .sched-block:last-child { margin-bottom: 0; }
  .sched-time { font-size: 12px; font-weight: 700; color: var(--blue); min-width: 70px; }
  .sched-name { font-size: 13px; font-weight: 600; color: var(--text); }
  .sched-tag { font-size: 10px; padding: 2px 7px; border-radius: 20px; font-weight: 600; margin-left: auto; }
  .sched-tag.low { background: #dcfce7; color: #15803d; }
  .sched-tag.med { background: #ffedd5; color: #c2410c; }
</style>
</head>
<body>

<!-- ── SIGN-IN SCREEN ─────────────────────────────── -->
<div id="signin-screen">
  <div class="logo">📋</div>
  <h1>Alex Hub</h1>
  <p>Your personal dashboard for teaching, dissertation, and daily tasks.</p>
  <div id="g_id_onload"
    data-client_id="404296154246-2e0rgnfmmtfrgfdt0egvblccj7854p6p.apps.googleusercontent.com"
    data-callback="onGoogleSignIn"
    data-auto_select="true">
  </div>
  <div class="g_id_signin"
    data-type="standard"
    data-shape="pill"
    data-theme="outline"
    data-text="sign_in_with"
    data-size="large"
    data-logo_alignment="left">
  </div>
  <p style="font-size:12px;color:#aaa;">Sign in to load your calendar and email</p>
</div>

<!-- ── MAIN APP ────────────────────────────────────── -->
<div id="app" style="display:none;">

  <div id="screen">

    <!-- TODAY PAGE -->
    <div class="page active" id="page-today">
      <div class="page-header">
        <div class="eyebrow" id="today-date"></div>
        <h1>Today</h1>
      </div>

      <!-- EP750 Deadlines -->
      <div class="card">
        <div class="card-title">📚 EP750 Deadlines</div>
        <div id="ep750-tasks">
          <div class="task-item">
            <div class="task-cb" onclick="toggleTask(this)"></div>
            <div class="task-body">
              <div class="task-name">Unit 9: Chapter 3, Part 4</div>
              <div class="task-meta"><span class="badge urgent">Due Jun 4</span><span class="badge pts">50 pts</span></div>
            </div>
          </div>
          <div class="task-item">
            <div class="task-cb" onclick="toggleTask(this)"></div>
            <div class="task-body">
              <div class="task-name">Unit 10: Capstone Reader Selection</div>
              <div class="task-meta"><span class="badge urgent">Due Jun 7</span><span class="badge pts">50 pts</span></div>
            </div>
          </div>
          <div class="task-item">
            <div class="task-cb" onclick="toggleTask(this)"></div>
            <div class="task-body">
              <div class="task-name">Unit 11: Chapter 1, Part 1</div>
              <div class="task-meta"><span class="badge soon">Due Jun 11</span><span class="badge pts">50 pts</span></div>
            </div>
          </div>
          <div class="task-item">
            <div class="task-cb" onclick="toggleTask(this)"></div>
            <div class="task-body">
              <div class="task-name">Unit 12: Chapter 1, Part 2</div>
              <div class="task-meta"><span class="badge soon">Due Jun 14</span><span class="badge pts">50 pts</span></div>
            </div>
          </div>
          <div class="task-item">
            <div class="task-cb" onclick="toggleTask(this)"></div>
            <div class="task-body">
              <div class="task-name">Unit 13 Signature: Completed Capstone Proposal (Ch 1–3)</div>
              <div class="task-meta"><span class="badge ok">Due Jun 17</span><span class="badge pts">100 pts</span></div>
            </div>
          </div>
        </div>
      </div>

      <!-- Today's Schedule -->
      <div class="card">
        <div class="card-title">📅 Today's Schedule</div>
        <div class="sched-block">
          <span class="sched-time">2:45 PM</span>
          <span class="sched-name">After Work Wrap-Up</span>
          <span class="sched-tag low">Low</span>
        </div>
        <div class="sched-block">
          <span class="sched-time">7:00 PM</span>
          <span class="sched-name">Dissertation Work Block</span>
          <span class="sched-tag med">Medium</span>
        </div>
      </div>

      <!-- Custom Tasks -->
      <div class="card">
        <div class="card-title">✅ My Tasks</div>
        <div id="custom-tasks-list">
          <div class="empty">No tasks yet — tap + to add one</div>
        </div>
      </div>

      <!-- Calendar Events Today -->
      <div class="card">
        <div class="card-title">🗓 Calendar Events</div>
        <div id="today-events"><div class="spinner"></div></div>
      </div>

      <!-- Recent Emails -->
      <div class="card">
        <div class="card-title">✉️ Recent Emails</div>
        <div id="today-emails"><div class="spinner"></div></div>
      </div>
    </div>

    <!-- CALENDAR PAGE -->
    <div class="page" id="page-calendar">
      <div class="page-header">
        <div class="eyebrow" id="cal-month"></div>
        <h1>Calendar</h1>
      </div>
      <div class="card">
        <div class="card-title">This Week</div>
        <div id="week-events"><div class="spinner"></div></div>
      </div>
      <div class="card">
        <div class="card-title">Upcoming EP750</div>
        <div id="ep750-upcoming">
          <div class="cal-event">
            <div class="cal-time">Jun 4</div>
            <div><div class="cal-dot" style="background:#ef4444;margin-top:5px;float:left;margin-right:8px;"></div><div class="cal-name">Chapter 3, Part 4 due</div><div class="cal-desc">50 pts</div></div>
          </div>
          <div class="cal-event">
            <div class="cal-time">Jun 7</div>
            <div><div class="cal-name">Capstone Reader Selection due</div><div class="cal-desc">50 pts</div></div>
          </div>
          <div class="cal-event">
            <div class="cal-time">Jun 11</div>
            <div><div class="cal-name">Chapter 1, Part 1 due</div><div class="cal-desc">50 pts</div></div>
          </div>
          <div class="cal-event">
            <div class="cal-time">Jun 14</div>
            <div><div class="cal-name">Chapter 1, Part 2 due</div><div class="cal-desc">50 pts</div></div>
          </div>
          <div class="cal-event" style="background:#f0fdf4;border-radius:10px;padding:8px 10px;">
            <div class="cal-time" style="color:#15803d;font-weight:700;">Jun 17</div>
            <div><div class="cal-name" style="font-weight:700;">🎓 Capstone Proposal due</div><div class="cal-desc">Signature Assignment · 100 pts</div></div>
          </div>
        </div>
      </div>
    </div>

    <!-- EMAIL PAGE -->
    <div class="page" id="page-email">
      <div class="page-header">
        <div class="eyebrow">Inbox</div>
        <h1>Email</h1>
      </div>
      <div class="card">
        <div class="card-title">Recent</div>
        <div id="email-list"><div class="spinner"></div></div>
      </div>
    </div>

    <!-- REFERENCE PAGE -->
    <div class="page" id="page-ref">
      <div class="page-header">
        <div class="eyebrow">Quick Lookup</div>
        <h1>Reference</h1>
      </div>
      <div class="card">
        <div class="card-title">📖 Key Terms</div>
        <div class="ref-item"><span class="ref-key">EP750</span><span class="ref-val">Doctoral capstone course</span></div>
        <div class="ref-item"><span class="ref-key">Chair</span><span class="ref-val">Dr. Statti</span></div>
        <div class="ref-item"><span class="ref-key">Capstone</span><span class="ref-val">Dissertation Ch 1–3</span></div>
        <div class="ref-item"><span class="ref-key">ESOL</span><span class="ref-val">English language learner support</span></div>
        <div class="ref-item"><span class="ref-key">ESE</span><span class="ref-val">Special education support</span></div>
        <div class="ref-item"><span class="ref-key">Bell work</span><span class="ref-val">Warm-up at class start</span></div>
        <div class="ref-item"><span class="ref-key">Exit ticket</span><span class="ref-val">End-of-class check</span></div>
        <div class="ref-item"><span class="ref-key">APA</span><span class="ref-val">Citation format for dissertation</span></div>
      </div>
      <div class="card">
        <div class="card-title">📋 After Work Checklist</div>
        <div class="ref-item"><span class="ref-key">1</span><span class="ref-val">Review missing assignments, blank submissions, grades</span></div>
        <div class="ref-item"><span class="ref-key">2</span><span class="ref-val">Draft parent/student messages (missing work, behavior, retakes)</span></div>
        <div class="ref-item"><span class="ref-key">3</span><span class="ref-val">Prep: bell work, exit ticket, Google Classroom post, ESOL/ESE supports</span></div>
      </div>
      <div class="card">
        <div class="card-title">🎓 Dissertation Checklist</div>
        <div class="ref-item"><span class="ref-key">Ch 2</span><span class="ref-val">Lit review, source summaries, conceptual framework, methodology</span></div>
        <div class="ref-item"><span class="ref-key">Format</span><span class="ref-val">APA throughout, chair questions addressed</span></div>
        <div class="ref-item"><span class="ref-key">Weekly</span><span class="ref-val">Check EP750 assignment deadlines</span></div>
      </div>
    </div>

  </div><!-- /screen -->

  <!-- BOTTOM NAV -->
  <nav id="nav">
    <button class="nav-btn active" onclick="showPage('today', this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="M3 9l9-7 9 7v11a2 2 0 01-2 2H5a2 2 0 01-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg>
      Today
    </button>
    <button class="nav-btn" onclick="showPage('calendar', this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><rect x="3" y="4" width="18" height="18" rx="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/></svg>
      Calendar
    </button>
    <button class="nav-btn" onclick="showPage('email', this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>
      Email
    </button>
    <button class="nav-btn" onclick="showPage('ref', this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/></svg>
      Reference
    </button>
  </nav>

  <!-- FAB -->
  <button id="fab" onclick="openModal()">+</button>

</div><!-- /app -->

<!-- ── ADD TASK MODAL ─────────────────────────────── -->
<div class="modal-overlay" id="modal">
  <div class="modal">
    <h2>Add Task</h2>
    <input type="text" id="task-name-input" placeholder="Task name" />
    <div class="modal-row">
      <input type="date" id="task-date-input" />
      <select id="task-cat-input">
        <option value="General">General</option>
        <option value="University">University</option>
        <option value="Teaching">Teaching</option>
        <option value="Personal">Personal</option>
      </select>
    </div>
    <select id="task-priority-input">
      <option value="Medium">Medium priority</option>
      <option value="High">High priority</option>
      <option value="Low">Low priority</option>
    </select>
    <button class="btn-primary" onclick="addTask()">Add Task</button>
    <button class="btn-cancel" onclick="closeModal()">Cancel</button>
  </div>
</div>

<script>
// ── CONFIG ────────────────────────────────────────────
// Replace with your Google OAuth Client ID
const CLIENT_ID = '404296154246-2e0rgnfmmtfrgfdt0egvblccj7854p6p.apps.googleusercontent.com';
const SCOPES = 'https://www.googleapis.com/auth/calendar.readonly https://www.googleapis.com/auth/gmail.readonly';

// ── STATE ─────────────────────────────────────────────
let accessToken = null;
let customTasks = JSON.parse(localStorage.getItem('customTasks') || '[]');
let taskStates = JSON.parse(localStorage.getItem('taskStates') || '{}');

// ── GOOGLE SIGN-IN ────────────────────────────────────
function onGoogleSignIn(response) {
  // Get access token via OAuth implicit flow
  const client = google.accounts.oauth2.initTokenClient({
    client_id: CLIENT_ID,
    scope: SCOPES,
    callback: (tokenResponse) => {
      accessToken = tokenResponse.access_token;
      showApp();
      loadAllData();
    },
  });
  client.requestAccessToken();
}

function showApp() {
  document.getElementById('signin-screen').style.display = 'none';
  document.getElementById('app').style.display = 'flex';
}

// ── DATE SETUP ────────────────────────────────────────
function setupDates() {
  const now = new Date();
  const days = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'];
  const months = ['January','February','March','April','May','June','July','August','September','October','November','December'];
  document.getElementById('today-date').textContent =
    `${days[now.getDay()]}, ${months[now.getMonth()]} ${now.getDate()}`;
  document.getElementById('cal-month').textContent =
    `${months[now.getMonth()]} ${now.getFullYear()}`;
}

// ── NAVIGATION ────────────────────────────────────────
function showPage(name, btn) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
  document.getElementById('page-' + name).classList.add('active');
  btn.classList.add('active');
  document.getElementById('screen').scrollTop = 0;
}

// ── LOAD ALL DATA ─────────────────────────────────────
function loadAllData() {
  setupDates();
  loadTodayCalendar();
  loadWeekCalendar();
  loadEmails();
  renderCustomTasks();
  restoreTaskStates();
}

// ── CALENDAR ──────────────────────────────────────────
async function fetchCalendarEvents(timeMin, timeMax) {
  const params = new URLSearchParams({
    timeMin: timeMin.toISOString(),
    timeMax: timeMax.toISOString(),
    singleEvents: true,
    orderBy: 'startTime',
    maxResults: 20,
  });
  const res = await fetch(
    `https://www.googleapis.com/calendar/v3/calendars/primary/events?${params}`,
    { headers: { Authorization: `Bearer ${accessToken}` } }
  );
  const data = await res.json();
  return data.items || [];
}

async function loadTodayCalendar() {
  if (!accessToken) return;
  const now = new Date();
  const start = new Date(now); start.setHours(0,0,0,0);
  const end = new Date(now); end.setHours(23,59,59,999);
  try {
    const events = await fetchCalendarEvents(start, end);
    renderTodayEvents(events);
  } catch(e) { document.getElementById('today-events').innerHTML = '<div class="empty">Could not load events</div>'; }
}

async function loadWeekCalendar() {
  if (!accessToken) return;
  const now = new Date();
  const end = new Date(now); end.setDate(now.getDate() + 7);
  try {
    const events = await fetchCalendarEvents(now, end);
    renderWeekEvents(events);
  } catch(e) { document.getElementById('week-events').innerHTML = '<div class="empty">Could not load events</div>'; }
}

function formatTime(dateStr) {
  if (!dateStr) return 'All day';
  const d = new Date(dateStr);
  return d.toLocaleTimeString([], { hour: 'numeric', minute: '2-digit' });
}

function formatDate(dateStr) {
  const d = new Date(dateStr);
  return d.toLocaleDateString([], { weekday: 'short', month: 'short', day: 'numeric' });
}

function renderTodayEvents(events) {
  const el = document.getElementById('today-events');
  if (!events.length) { el.innerHTML = '<div class="empty">No events today 🎉</div>'; return; }
  el.innerHTML = events.map(e => `
    <div class="cal-event">
      <div class="cal-time">${e.start?.dateTime ? formatTime(e.start.dateTime) : 'All day'}</div>
      <div>
        <div class="cal-name">${e.summary || 'Untitled'}</div>
        ${e.location ? `<div class="cal-desc">${e.location}</div>` : ''}
      </div>
    </div>`).join('');
}

function renderWeekEvents(events) {
  const el = document.getElementById('week-events');
  if (!events.length) { el.innerHTML = '<div class="empty">Nothing this week</div>'; return; }
  el.innerHTML = events.map(e => `
    <div class="cal-event">
      <div class="cal-time">${e.start?.dateTime ? formatDate(e.start.dateTime) : formatDate(e.start?.date)}</div>
      <div>
        <div class="cal-name">${e.summary || 'Untitled'}</div>
        ${e.start?.dateTime ? `<div class="cal-desc">${formatTime(e.start.dateTime)}</div>` : ''}
      </div>
    </div>`).join('');
}

// ── EMAIL ─────────────────────────────────────────────
async function loadEmails() {
  if (!accessToken) return;
  try {
    const listRes = await fetch(
      'https://gmail.googleapis.com/gmail/v1/users/me/messages?maxResults=8&labelIds=INBOX',
      { headers: { Authorization: `Bearer ${accessToken}` } }
    );
    const listData = await listRes.json();
    const messages = listData.messages || [];
    const details = await Promise.all(
      messages.slice(0, 6).map(m =>
        fetch(`https://gmail.googleapis.com/gmail/v1/users/me/messages/${m.id}?format=metadata&metadataHeaders=From&metadataHeaders=Subject`,
          { headers: { Authorization: `Bearer ${accessToken}` } }
        ).then(r => r.json())
      )
    );
    renderEmails(details);
  } catch(e) {
    const el = '<div class="empty">Could not load emails</div>';
    document.getElementById('today-emails').innerHTML = el;
    document.getElementById('email-list').innerHTML = el;
  }
}

function getHeader(msg, name) {
  return msg.payload?.headers?.find(h => h.name === name)?.value || '';
}

function renderEmails(messages) {
  const html = messages.map(msg => {
    const from = getHeader(msg, 'From').replace(/<.*>/, '').trim();
    const subject = getHeader(msg, 'Subject') || '(no subject)';
    const unread = msg.labelIds?.includes('UNREAD');
    return `<div class="email-item ${unread ? 'email-unread' : ''}">
      <div class="email-from">${from || 'Unknown'}</div>
      <div class="email-subject">${subject}</div>
    </div>`;
  }).join('');
  const fallback = '<div class="empty">No emails</div>';
  document.getElementById('today-emails').innerHTML = html || fallback;
  document.getElementById('email-list').innerHTML = html || fallback;
}

// ── CUSTOM TASKS ──────────────────────────────────────
function renderCustomTasks() {
  const el = document.getElementById('custom-tasks-list');
  if (!customTasks.length) {
    el.innerHTML = '<div class="empty">No tasks yet — tap + to add one</div>';
    return;
  }
  el.innerHTML = customTasks.map((t, i) => {
    const done = taskStates['custom-' + i];
    const dueClass = getDueClass(t.date);
    return `<div class="task-item">
      <div class="task-cb ${done ? 'done' : ''}" onclick="toggleCustomTask(${i}, this)"></div>
      <div class="task-body">
        <div class="task-name ${done ? 'done' : ''}">${t.name}</div>
        <div class="task-meta">
          ${t.date ? `<span class="badge ${dueClass}">Due ${formatShortDate(t.date)}</span>` : ''}
          <span class="badge cat">${t.category}</span>
          ${t.priority !== 'Medium' ? `<span class="badge ${t.priority === 'High' ? 'urgent' : 'ok'}">${t.priority}</span>` : ''}
        </div>
      </div>
    </div>`;
  }).join('');
}

function getDueClass(dateStr) {
  if (!dateStr) return 'ok';
  const due = new Date(dateStr);
  const today = new Date();
  const diff = (due - today) / (1000*60*60*24);
  if (diff < 3) return 'urgent';
  if (diff < 7) return 'soon';
  return 'ok';
}

function formatShortDate(dateStr) {
  const d = new Date(dateStr + 'T00:00:00');
  return d.toLocaleDateString([], { month: 'short', day: 'numeric' });
}

function toggleCustomTask(i, el) {
  const key = 'custom-' + i;
  taskStates[key] = !taskStates[key];
  localStorage.setItem('taskStates', JSON.stringify(taskStates));
  renderCustomTasks();
}

function toggleTask(el) {
  const idx = Array.from(document.querySelectorAll('#ep750-tasks .task-cb')).indexOf(el);
  const key = 'ep750-' + idx;
  const isDone = !el.classList.contains('done');
  el.classList.toggle('done', isDone);
  el.nextElementSibling?.querySelector('.task-name')?.classList.toggle('done', isDone);
  taskStates[key] = isDone;
  localStorage.setItem('taskStates', JSON.stringify(taskStates));
}

function restoreTaskStates() {
  document.querySelectorAll('#ep750-tasks .task-cb').forEach((el, i) => {
    if (taskStates['ep750-' + i]) {
      el.classList.add('done');
      el.nextElementSibling?.querySelector('.task-name')?.classList.add('done');
    }
  });
}

// ── MODAL ─────────────────────────────────────────────
function openModal() {
  document.getElementById('modal').classList.add('open');
  document.getElementById('task-name-input').focus();
}
function closeModal() { document.getElementById('modal').classList.remove('open'); }
function addTask() {
  const name = document.getElementById('task-name-input').value.trim();
  if (!name) return;
  customTasks.push({
    name,
    date: document.getElementById('task-date-input').value,
    category: document.getElementById('task-cat-input').value,
    priority: document.getElementById('task-priority-input').value,
  });
  localStorage.setItem('customTasks', JSON.stringify(customTasks));
  document.getElementById('task-name-input').value = '';
  document.getElementById('task-date-input').value = '';
  closeModal();
  renderCustomTasks();
}
document.getElementById('modal').addEventListener('click', function(e) {
  if (e.target === this) closeModal();
});

// ── INIT ──────────────────────────────────────────────
setupDates();
restoreTaskStates();
renderCustomTasks();

// Auto sign-in if token cached
if (window.localStorage.getItem('gAccessToken')) {
  accessToken = localStorage.getItem('gAccessToken');
  showApp();
  loadAllData();
}
</script>
</body>
</html>
