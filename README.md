<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Turf Board</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Big+Shoulders+Display:wght@600;800&family=Work+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#F3EFE2; --surface:#FFFFFF; --ink:#20242B; --ink-soft:#5B5647;
  --border:#DCD3B8; --radius:12px;
  --shadow:0 1px 2px rgba(32,36,43,.08), 0 4px 14px rgba(32,36,43,.06);
  --font-display:'Big Shoulders Display','Arial Narrow',sans-serif;
  --font-body:'Work Sans',system-ui,-apple-system,sans-serif;
}
@media (prefers-color-scheme: dark){
  :root:not([data-theme="light"]){
    --bg:#15181B; --surface:#1D2124; --ink:#F1EEE3; --ink-soft:#B3AD9B; --border:#33383B;
    --shadow:0 1px 2px rgba(0,0,0,.4), 0 6px 20px rgba(0,0,0,.35);
  }
}
:root[data-theme="dark"]{
  --bg:#15181B; --surface:#1D2124; --ink:#F1EEE3; --ink-soft:#B3AD9B; --border:#33383B;
  --shadow:0 1px 2px rgba(0,0,0,.4), 0 6px 20px rgba(0,0,0,.35);
}
*{box-sizing:border-box;}
html,body{margin:0;padding:0;}
body{background:var(--bg);color:var(--ink);font-family:var(--font-body);line-height:1.5;-webkit-font-smoothing:antialiased;}

/* ---- scaling logo header ---- */
.site-header{
  text-align:center;
  padding:clamp(18px,5vw,36px) 16px clamp(10px,3vw,20px);
}
.logo-img{
  max-height:clamp(40px,10vw,72px);
  width:auto;
  display:inline-block;
}
.wordmark{
  font-family:var(--font-display);
  font-weight:800;
  font-size:clamp(26px,6.5vw,44px);
  text-transform:uppercase;
  letter-spacing:.01em;
  margin:0;
  line-height:1;
  text-wrap:balance;
}
.tagline{
  font-size:clamp(12px,2.4vw,15px);
  color:var(--ink-soft);
  margin:clamp(6px,1.5vw,10px) 0 0;
}

/* ---- scaling calendar ---- */
.cal-wrap{
  width:100%;
  max-width:900px;
  margin:0 auto;
  padding:0 16px clamp(24px,6vw,48px);
}
.cal-frame{
  border:0;
  display:block;
  width:100%;
  height:clamp(420px,75vh,750px);
  border-radius:var(--radius);
  box-shadow:var(--shadow);
  background:var(--surface);
}
.cal-note{
  text-align:center;
  font-size:12px;
  color:var(--ink-soft);
  margin-top:12px;
}
</style>
</head>
<body>
<div id="app">
  <header class="site-header" id="header"></header>
  <div class="cal-wrap">
    <iframe class="cal-frame" id="gcal" title="Volunteer shift calendar" scrolling="no"></iframe>
    <p class="cal-note">Tap a shift to see details and sign up on Mobilize.</p>
  </div>
</div>
<script>
/* ============================================================================
   EDIT THIS SECTION
   ----------------------------------------------------------------------------
   ORG_NAME / TAGLINE   plain text shown in the header.

   LOGO_IMAGE            leave as "" to show ORG_NAME as styled text (what
                         you see by default). To show an actual logo image
                         instead, set this to an image URL, e.g.
                         "https://yoursite.org/logo.png"

   GOOGLE_CALENDAR_ID    Google Calendar > Settings > your calendar (under
                         "Settings for my calendars") > Integrate calendar >
                         "Calendar ID". Usually looks like an email address.

   TIMEZONE              your calendar's timezone, e.g. "America/Chicago"

   DEFAULT_VIEW          "WEEK" or "MONTH" — which view loads first.
                         Volunteers can still switch views themselves using
                         the tabs Google shows inside the calendar.
   ============================================================================ */
const ORG_NAME = "Your Organization";
const TAGLINE = "Volunteer canvass calendar — pick a shift, sign up on Mobilize.";
const LOGO_IMAGE = "";
const GOOGLE_CALENDAR_ID = "your_calendar_id@group.calendar.google.com";
const TIMEZONE = "America/Chicago";
const DEFAULT_VIEW = "WEEK";

/* ============================================================================
   Nothing below this line needs editing.
   ============================================================================ */
function esc(s){
  return String(s==null?'':s).replace(/[&<>"']/g, function(c){
    return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c];
  });
}

function buildCalendarSrc(){
  var params = new URLSearchParams();
  params.set('src', GOOGLE_CALENDAR_ID);
  params.set('ctz', TIMEZONE);
  params.set('mode', DEFAULT_VIEW);
  params.set('showTitle', '0');
  params.set('showNav', '1');
  params.set('showDate', '1');
  params.set('showPrint', '0');
  params.set('showTabs', '1');
  params.set('showCalendars', '0');
  params.set('showTz', '0');
  return 'https://calendar.google.com/calendar/embed?' + params.toString();
}

function renderHeader(){
  var logo = LOGO_IMAGE
    ? '<img class="logo-img" src="'+esc(LOGO_IMAGE)+'" alt="'+esc(ORG_NAME)+'">'
    : '<h1 class="wordmark">'+esc(ORG_NAME)+'</h1>';
  document.getElementById('header').innerHTML = logo + '<p class="tagline">'+esc(TAGLINE)+'</p>';
}

renderHeader();
document.getElementById('gcal').src = buildCalendarSrc();
</script>
</body>
</html>
