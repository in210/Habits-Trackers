# Habits-Trackers<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Notion-Style Habit Tracker</title>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg-main: #F7F9F6;
      --card-bg: #FFFFFF;
      --accent: #84A98C;
      --accent-soft: #E8F0EC;
      --text-main: #2F3E46;
      --text-muted: #6B7A82;
      --border: #E0E7E1;
    }

    [data-theme="sage"] {
      --bg-main: #F4F7F4;
      --card-bg: #FFFFFF;
      --accent: #769FCD;
      --accent-soft: #EAF2F8;
      --text-main: #2D3748;
      --text-muted: #718096;
      --border: #E2E8F0;
    }

    [data-theme="pink"] {
      --bg-main: #FAF4F5;
      --card-bg: #FFFFFF;
      --accent: #E5989B;
      --accent-soft: #F9ECEC;
      --text-main: #4A3E3D;
      --text-muted: #8C7A79;
      --border: #F0E1E1;
    }

    [data-theme="lavender"] {
      --bg-main: #F8F6FA;
      --card-bg: #FFFFFF;
      --accent: #B5A0D0;
      --accent-soft: #F0ECF6;
      --text-main: #3C354A;
      --text-muted: #7A728A;
      --border: #E6E0F0;
    }

    [data-theme="beige"] {
      --bg-main: #FBF8F3;
      --card-bg: #FFFFFF;
      --accent: #D4A373;
      --accent-soft: #F7EFE5;
      --text-main: #4A3F35;
      --text-muted: #8C7C6D;
      --border: #ECE3D5;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Plus Jakarta Sans', sans-serif;
    }

    body {
      background-color: var(--bg-main);
      color: var(--text-main);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      padding: 20px;
      transition: all 0.3s ease;
    }

    .container {
      width: 100%;
      max-width: 900px;
    }

    /* TAMPILAN PERTAMA: PEMILIHAN TEMPLATE */
    #page-templates {
      text-align: center;
      padding: 40px 20px;
    }

    .template-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      gap: 20px;
      margin-top: 30px;
    }

    .template-card {
      background: #ffffff;
      border: 2px solid transparent;
      border-radius: 16px;
      padding: 24px;
      cursor: pointer;
      box-shadow: 0 4px 15px rgba(0,0,0,0.03);
      transition: all 0.2s ease;
    }

    .template-card:hover {
      transform: translateY(-4px);
      box-shadow: 0 8px 25px rgba(0,0,0,0.06);
    }

    .color-preview {
      height: 60px;
      border-radius: 10px;
      margin-bottom: 12px;
    }

    /* TAMPILAN KEDUA: DASHBOARD UTAMA */
    #page-dashboard {
      display: none;
    }

    .header-nav {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 24px;
      flex-wrap: wrap;
      gap: 12px;
    }

    .view-tabs {
      display: flex;
      background: var(--card-bg);
      padding: 4px;
      border-radius: 12px;
      border: 1px solid var(--border);
    }

    .tab-btn {
      border: none;
      background: transparent;
      padding: 8px 16px;
      border-radius: 8px;
      font-weight: 600;
      color: var(--text-muted);
      cursor: pointer;
      font-size: 0.9rem;
    }

    .tab-btn.active {
      background: var(--accent-soft);
      color: var(--accent);
    }

    .card {
      background: var(--card-bg);
      border-radius: 16px;
      padding: 20px;
      border: 1px solid var(--border);
      margin-bottom: 20px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.02);
    }

    .card-title {
      font-size: 1.1rem;
      font-weight: 700;
      margin-bottom: 16px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    /* NOTIFIKASI PENGINGAT */
    .notification-banner {
      background: var(--accent-soft);
      border-left: 4px solid var(--accent);
      padding: 12px 16px;
      border-radius: 8px;
      margin-bottom: 20px;
      font-size: 0.9rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    /* HABIT CHECKLIST */
    .habit-item {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 10px 0;
      border-bottom: 1px solid var(--border);
    }

    .habit-item:last-child { border-bottom: none; }

    .habit-left {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .habit-checkbox {
      width: 20px;
      height: 20px;
      accent-color: var(--accent);
      cursor: pointer;
    }

    .completed {
      text-decoration: line-through;
      color: var(--text-muted);
    }

    /* DIAGRAM PROGRESS BAR */
    .progress-outer {
      background: var(--accent-soft);
      height: 12px;
      border-radius: 6px;
      overflow: hidden;
      margin-top: 8px;
    }

    .progress-inner {
      height: 100%;
      background: var(--accent);
      width: 0%;
      transition: width 0.4s ease;
    }

    /* GOALS MALAM & REFLEKSI PEMBELAJARAN */
    textarea {
      width: 100%;
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 12px;
      font-size: 0.9rem;
      resize: vertical;
      min-height: 80px;
      outline: none;
      background: var(--bg-main);
      color: var(--text-main);
    }

    textarea:focus {
      border-color: var(--accent);
    }

    /* PENCATATAN KEUANGAN */
    .finance-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }

    .finance-input {
      width: 100%;
      padding: 10px;
      border: 1px solid var(--border);
      border-radius: 8px;
      background: var(--bg-main);
      outline: none;
      color: var(--text-main);
    }

    .btn-change-theme {
      background: transparent;
      border: 1px solid var(--border);
      padding: 6px 12px;
      border-radius: 8px;
      cursor: pointer;
      font-size: 0.85rem;
      color: var(--text-muted);
    }
  </style>
</head>
<body>

<div class="container">

  <!-- TAMPILAN PERTAMA: PEMILIHAN TEMPLATE WARNA -->
  <div id="page-templates">
    <h1 style="font-size: 2rem; margin-bottom: 8px;">Pilih Aesthetic Template</h1>
    <p style="color: var(--text-muted);">Pilih nuansa warna soft yang paling cocok dengan mood kamu.</p>
    
    <div class="template-grid">
      <div class="template-card" onclick="selectTemplate('sage')">
        <div class="color-preview" style="background: #84A98C;"></div>
        <h3>Soft Sage</h3>
        <p style="font-size: 0.8rem; color: #888; margin-top: 4px;">Ketenangan & Fokus</p>
      </div>

      <div class="template-card" onclick="selectTemplate('pink')">
        <div class="color-preview" style="background: #E5989B;"></div>
        <h3>Dusty Pink</h3>
        <p style="font-size: 0.8rem; color: #888; margin-top: 4px;">Kehangatan & Kreativitas</p>
      </div>

      <div class="template-card" onclick="selectTemplate('lavender')">
        <div class="color-preview" style="background: #B5A0D0;"></div>
        <h3>Soft Lavender</h3>
        <p style="font-size: 0.8rem; color: #888; margin-top: 4px;">Inspirasi & Ide</p>
      </div>

      <div class="template-card" onclick="selectTemplate('beige')">
        <div class="color-preview" style="background: #D4A373;"></div>
        <h3>Warm Beige</h3>
        <p style="font-size: 0.8rem; color: #888; margin-top: 4px;">Minimalis & Nyaman</p>
      </div>
    </div>
  </div>

  <!-- TAMPILAN KEDUA: DASHBOARD UTAMA -->
  <div id="page-dashboard">
    <div class="header-nav">
      <div>
        <h2 id="view-title">Schedule Harian</h2>
        <span style="font-size: 0.85rem; color: var(--text-muted);" id="current-date"></span>
      </div>
      <div style="display: flex; gap: 10px; align-items: center;">
        <div class="view-tabs">
          <button class="tab-btn active" onclick="switchView('Harian')">Harian</button>
          <button class="tab-btn" onclick="switchView('Mingguan')">Mingguan</button>
          <button class="tab-btn" onclick="switchView('Bulanan')">Bulanan</button>
          <button class="tab-btn" onclick="switchView('Tahunan')">Tahunan</button>
        </div>
        <button class="btn-change-theme" onclick="resetTemplate()">Tema</button>
      </div>
    </div>

    <!-- NOTIFIKASI PENGINGAT -->
    <div class="notification-banner">
      <span>🔔 <b>Pengingat:</b> <span id="next-task-text">Jadwal berikutnya: Belajar Kode & Review Catatan jam 19:00</span></span>
    </div>

    <!-- DIAGRAM PROGRESS TARGET -->
    <div class="card">
      <div class="card-title">
        <span>📊 Progress Target</span>
        <span id="progress-percent" style="margin-left: auto; color: var(--accent);">0%</span>
      </div>
      <div class="progress-outer">
        <div class="progress-inner" id="progress-bar"></div>
      </div>
    </div>

    <!-- CHECKLIST HABIT -->
    <div class="card">
      <div class="card-title">✨ Habit Checklist</div>
      <div id="habit-list">
        <div class="habit-item">
          <div class="habit-left">
            <input type="checkbox" class="habit-checkbox" onchange="toggleHabit(this)">
            <span>Bangun jam 05:00 & Minum Air Putih</span>
          </div>
        </div>
        <div class="habit-item">
          <div class="habit-left">
            <input type="checkbox" class="habit-checkbox" onchange="toggleHabit(this)">
            <span>Membaca Buku / Artikel (15 Menit)</span>
          </div>
        </div>
        <div class="habit-item">
          <div class="habit-left">
            <input type="checkbox" class="habit-checkbox" onchange="toggleHabit(this)">
            <span>Olahraga Ringan / Stretches</span>
          </div>
        </div>
      </div>
    </div>

    <!-- KUESIONER GOALS MALAM & KOTAK REFLEKSI PEMBELAJARAN -->
    <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 20px;">
      <div class="card">
        <div class="card-title">🌙 Goals Terlaksana (Malam)</div>
        <textarea placeholder="Tuliskan target-target yang berhasil kamu selesaikan hari ini..."></textarea>
      </div>

      <div class="card">
        <div class="card-title">💡 Pembelajaran Hari Ini</div>
        <textarea placeholder="Satu pelajaran atau wawasan baru yang kamu dapatkan hari ini..."></textarea>
      </div>
    </div>

    <!-- PENCATATAN KEUANGAN -->
    <div class="card">
      <div class="card-title">💰 Keuangan Sederhana</div>
      <div class="finance-grid">
        <div>
          <label style="font-size: 0.8rem; color: var(--text-muted);">Pendapatan (Rp)</label>
          <input type="number" class="finance-input" id="income" placeholder="0" oninput="calculateFinance()">
        </div>
        <div>
          <label style="font-size: 0.8rem; color: var(--text-muted);">Pengeluaran (Rp)</label>
          <input type="number" class="finance-input" id="expense" placeholder="0" oninput="calculateFinance()">
        </div>
      </div>
      <div style="margin-top: 12px; font-weight: 600; font-size: 0.9rem;">
        Sisa Saldo: <span id="balance" style="color: var(--accent);">Rp 0</span>
      </div>
    </div>

  </div>

</div>

<script>
  // Date Setup
  const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
  document.getElementById('current-date').innerText = new Date().toLocaleDateString('id-ID', options);

  // Switch Template
  function selectTemplate(theme) {
    document.body.setAttribute('data-theme', theme);
    document.getElementById('page-templates').style.display = 'none';
    document.getElementById('page-dashboard').style.display = 'block';
  }

  function resetTemplate() {
    document.getElementById('page-templates').style.display = 'block';
    document.getElementById('page-dashboard').style.display = 'none';
  }

  // Switch View Tabs
  function switchView(viewName) {
    document.getElementById('view-title').innerText = `Schedule ${viewName}`;
    const buttons = document.querySelectorAll('.tab-btn');
    buttons.forEach(btn => btn.classList.remove('active'));
    event.target.classList.add('active');
  }

  // Calculate Progress
  function toggleHabit(checkbox) {
    const textSpan = checkbox.nextElementSibling;
    if (checkbox.checked) {
      textSpan.classList.add('completed');
    } else {
      textSpan.classList.remove('completed');
    }

    const total = document.querySelectorAll('.habit-checkbox').length;
    const checked = document.querySelectorAll('.habit-checkbox:checked').length;
    const percentage = Math.round((checked / total) * 100);

    document.getElementById('progress-bar').style.width = percentage + '%';
    document.getElementById('progress-percent').innerText = percentage + '%';
  }

  // Calculate Finance
  function calculateFinance() {
    const income = parseFloat(document.getElementById('income').value) || 0;
    const expense = parseFloat(document.getElementById('expense').value) || 0;
    const balance = income - expense;

    document.getElementById('balance').innerText = `Rp ${balance.toLocaleString('id-ID')}`;
  }
</script>

</body>
</html>
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Aesthetic Digital Planner & Habit Tracker</title>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&family=Caveat:wght@600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg-main: #F7F9F6;
      --card-bg: #FFFFFF;
      --accent: #84A98C;
      --accent-soft: #E8F0EC;
      --text-main: #2F3E46;
      --text-muted: #6B7A82;
      --border: #E0E7E1;
    }

    [data-theme="sage"] {
      --bg-main: #F4F7F4;
      --card-bg: #FFFFFF;
      --accent: #769FCD;
      --accent-soft: #EAF2F8;
      --text-main: #2D3748;
      --text-muted: #718096;
      --border: #E2E8F0;
    }

    [data-theme="pink"] {
      --bg-main: #FAF4F5;
      --card-bg: #FFFFFF;
      --accent: #E5989B;
      --accent-soft: #F9ECEC;
      --text-main: #4A3E3D;
      --text-muted: #8C7A79;
      --border: #F0E1E1;
    }

    [data-theme="lavender"] {
      --bg-main: #F8F6FA;
      --card-bg: #FFFFFF;
      --accent: #B5A0D0;
      --accent-soft: #F0ECF6;
      --text-main: #3C354A;
      --text-muted: #7A728A;
      --border: #E6E0F0;
    }

    [data-theme="beige"] {
      --bg-main: #FBF8F3;
      --card-bg: #FFFFFF;
      --accent: #D4A373;
      --accent-soft: #F7EFE5;
      --text-main: #4A3F35;
      --text-muted: #8C7C6D;
      --border: #ECE3D5;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Plus Jakarta Sans', sans-serif;
    }

    body {
      background-color: var(--bg-main);
      color: var(--text-main);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      padding: 20px 12px;
      transition: all 0.3s ease;
    }

    .container {
      width: 100%;
      max-width: 900px;
    }

    /* STIKER AESTHETIC PINTEST STYLE */
    .sticker {
      display: inline-block;
      font-size: 1.4rem;
      animation: floatSticker 3s ease-in-out infinite alternate;
      user-select: none;
    }
    
    .sticker-tape {
      background: rgba(255, 255, 255, 0.6);
      border: 1px dashed var(--accent);
      padding: 2px 10px;
      border-radius: 4px;
      font-family: 'Caveat', cursive;
      font-size: 1.1rem;
      color: var(--text-main);
      box-shadow: 0 2px 5px rgba(0,0,0,0.03);
    }

    @keyframes floatSticker {
      0% { transform: translateY(0px) rotate(-2deg); }
      100% { transform: translateY(-6px) rotate(3deg); }
    }

    /* SLIDE 1: PEMILIHAN TEMPLATE */
    #page-templates {
      text-align: center;
      padding: 30px 10px;
      position: relative;
    }

    .hero-title {
      font-family: 'Caveat', cursive;
      font-size: 3rem;
      color: var(--accent);
      margin-top: 10px;
    }

    .template-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      gap: 20px;
      margin-top: 30px;
    }

    .template-card {
      background: #ffffff;
      border: 2px solid transparent;
      border-radius: 20px;
      padding: 24px;
      cursor: pointer;
      box-shadow: 0 6px 20px rgba(0,0,0,0.04);
      transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
      position: relative;
    }

    .template-card:hover {
      transform: translateY(-8px) scale(1.02);
      box-shadow: 0 12px 30px rgba(0,0,0,0.08);
      border-color: var(--accent);
    }

    .color-preview {
      height: 70px;
      border-radius: 14px;
      margin-bottom: 14px;
    }

    /* SLIDE 2: DASHBOARD UTAMA */
    #page-dashboard {
      display: none;
    }

    .header-nav {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
      flex-wrap: wrap;
      gap: 12px;
    }

    .view-tabs {
      display: flex;
      background: var(--card-bg);
      padding: 4px;
      border-radius: 12px;
      border: 1px solid var(--border);
      overflow-x: auto;
    }

    .tab-btn {
      border: none;
      background: transparent;
      padding: 8px 16px;
      border-radius: 8px;
      font-weight: 600;
      color: var(--text-muted);
      cursor: pointer;
      font-size: 0.85rem;
      white-space: nowrap;
    }

    .tab-btn.active {
      background: var(--accent-soft);
      color: var(--accent);
    }

    .card {
      background: var(--card-bg);
      border-radius: 18px;
      padding: 20px;
      border: 1px solid var(--border);
      margin-bottom: 20px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.02);
      position: relative;
    }

    .card-title {
      font-size: 1.05rem;
      font-weight: 700;
      margin-bottom: 16px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    /* TAB SCHEDULE CONTENTS */
    .tab-content { display: none; }
    .tab-content.active { display: block; }

    /* KALENDER SIMPEL */
    .calendar-box {
      display: grid;
      grid-template-columns: repeat(7, 1fr);
      gap: 6px;
      text-align: center;
      margin-top: 10px;
    }

    .cal-day-head {
      font-size: 0.75rem;
      font-weight: 700;
      color: var(--text-muted);
      padding-bottom: 4px;
    }

    .cal-date {
      padding: 10px 4px;
      border-radius: 10px;
      background: var(--bg-main);
      font-size: 0.85rem;
      cursor: pointer;
      border: 1px solid transparent;
    }

    .cal-date.active {
      background: var(--accent);
      color: white;
      font-weight: 700;
    }

    /* CUSTOM HABIT SPREADSHEET LIST */
    .add-input-group {
      display: flex;
      gap: 8px;
      margin-bottom: 16px;
    }

    .text-input {
      width: 100%;
      padding: 10px 12px;
      border: 1px solid var(--border);
      border-radius: 10px;
      background: var(--bg-main);
      outline: none;
      font-size: 0.85rem;
      color: var(--text-main);
    }

    .btn-add {
      background: var(--accent);
      color: white;
      border: none;
      padding: 0 16px;
      border-radius: 10px;
      cursor: pointer;
      font-weight: 600;
      font-size: 1.1rem;
    }

    .habit-item {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 10px;
      border-bottom: 1px solid var(--border);
      gap: 8px;
    }

    .habit-left {
      display: flex;
      align-items: center;
      gap: 10px;
      flex: 1;
    }

    .habit-date-tag {
      font-size: 0.75rem;
      background: var(--accent-soft);
      color: var(--accent);
      padding: 2px 8px;
      border-radius: 6px;
      border: none;
    }

    .habit-checkbox {
      width: 18px;
      height: 18px;
      accent-color: var(--accent);
      cursor: pointer;
    }

    .completed {
      text-decoration: line-through;
      color: var(--text-muted);
    }

    .btn-delete {
      background: transparent;
      border: none;
      color: #E5989B;
      cursor: pointer;
      font-size: 1rem;
    }

    /* DIAGRAM TEMPLATES */
    .progress-outer {
      background: var(--accent-soft);
      height: 14px;
      border-radius: 8px;
      overflow: hidden;
      margin-top: 8px;
    }

    .progress-inner {
      height: 100%;
      background: var(--accent);
      width: 0%;
      transition: width 0.4s ease;
    }

    .style-segmented {
      display: flex;
      gap: 4px;
      background: transparent;
      height: 14px;
    }

    .segment-block {
      flex: 1;
      background: var(--accent-soft);
      border-radius: 3px;
    }

    .segment-block.active {
      background: var(--accent);
    }

    /* SLEEP & WAKE TRACKER */
    .time-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }

    /* SPREADSHEET GRID FOR WEEKLY/MONTHLY/YEARLY */
    .sheet-grid {
      width: 100%;
      border-collapse: collapse;
      margin-top: 10px;
      font-size: 0.85rem;
    }

    .sheet-grid th, .sheet-grid td {
      border: 1px solid var(--border);
      padding: 8px;
      text-align: left;
    }

    .sheet-grid th {
      background: var(--accent-soft);
      color: var(--accent);
    }

    textarea {
      width: 100%;
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 10px;
      font-size: 0.85rem;
      resize: vertical;
      min-height: 70px;
      outline: none;
      background: var(--bg-main);
      color: var(--text-main);
    }

    .btn-theme-reset {
      background: transparent;
      border: 1px solid var(--border);
      padding: 6px 12px;
      border-radius: 8px;
      cursor: pointer;
      font-size: 0.8rem;
      color: var(--text-muted);
    }
  </style>
</head>
<body>

<div class="container">

  <!-- SLIDE 1: PEMILIHAN TEMPLATE WARNA SOFT (PINTEREST STYLE) -->
  <div id="page-templates">
    <div style="display: flex; justify-content: center; gap: 15px; align-items: center; margin-bottom: 10px;">
      <span class="sticker">🎀</span>
      <span class="sticker-tape">📌 Digital Planner 2026</span>
      <span class="sticker">✨</span>
    </div>
    
    <h1 class="hero-title">Pilih Aesthetic Mood</h1>
    <p style="color: var(--text-muted); font-size: 0.9rem; margin-top: 5px;">Sesuaikan nuansa warna planner kesukaanmu!</p>
    
    <div class="template-grid">
      <div class="template-card" onclick="selectTemplate('sage')">
        <span class="sticker" style="position: absolute; top: 10px; right: 12px;">🌿</span>
        <div class="color-preview" style="background: #769FCD;"></div>
        <h3>Soft Sage</h3>
        <p style="font-size: 0.75rem; color: #888; margin-top: 4px;">Fokus & Calm</p>
      </div>

      <div class="template-card" onclick="selectTemplate('pink')">
        <span class="sticker" style="position: absolute; top: 10px; right: 12px;">🌷</span>
        <div class="color-preview" style="background: #E5989B;"></div>
        <h3>Dusty Pink</h3>
        <p style="font-size: 0.75rem; color: #888; margin-top: 4px;">Kreatif & Warm</p>
      </div>

      <div class="template-card" onclick="selectTemplate('lavender')">
        <span class="sticker" style="position: absolute; top: 10px; right: 12px;">🍇</span>
        <div class="color-preview" style="background: #B5A0D0;"></div>
        <h3>Soft Lavender</h3>
        <p style="font-size: 0.75rem; color: #888; margin-top: 4px;">Inspirasi & Ide</p>
      </div>

      <div class="template-card" onclick="selectTemplate('beige')">
        <span class="sticker" style="position: absolute; top: 10px; right: 12px;">☕</span>
        <div class="color-preview" style="background: #D4A373;"></div>
        <h3>Warm Beige</h3>
        <p style="font-size: 0.75rem; color: #888; margin-top: 4px;">Cozy & Minimalis</p>
      </div>
    </div>
  </div>

  <!-- SLIDE 2: DASHBOARD UTAMA -->
  <div id="page-dashboard">
    
    <!-- HEADER NAVIGASI SLIDE/TAB -->
    <div class="header-nav">
      <div>
        <span class="sticker-tape" id="current-date-tape">Agustus 2026</span>
        <h2 id="view-title" style="margin-top: 6px; font-size: 1.3rem;">Schedule Harian</h2>
      </div>
      
      <div style="display: flex; gap: 8px; align-items: center;">
        <div class="view-tabs">
          <button class="tab-btn active" onclick="switchView('Harian')">Harian</button>
          <button class="tab-btn" onclick="switchView('Mingguan')">Mingguan</button>
          <button class="tab-btn" onclick="switchView('Bulanan')">Bulanan</button>
          <button class="tab-btn" onclick="switchView('Tahunan')">Tahunan</button>
        </div>
        <button class="btn-theme-reset" onclick="resetTemplate()">🎨</button>
      </div>
    </div>

    <!-- TAB CONTENT 1: HARIAN -->
    <div id="tab-Harian" class="tab-content active">
      
      <!-- KALENDER KECIL & EVENT EVENT -->
      <div class="card">
        <div class="card-title">
          <span>📅 Kalender Event Harian</span>
          <span class="sticker">☁️</span>
        </div>
        <div class="calendar-box">
          <div class="cal-day-head">M</div><div class="cal-day-head">S</div><div class="cal-day-head">S</div><div class="cal-day-head">R</div><div class="cal-day-head">K</div><div class="cal-day-head">J</div><div class="cal-day-head">S</div>
          <div class="cal-date">1</div><div class="cal-date">2</div><div class="cal-date">3</div><div class="cal-date">4</div><div class="cal-date">5</div><div class="cal-date">6</div><div class="cal-date">7</div>
          <div class="cal-date active">8</div><div class="cal-date">9</div><div class="cal-date">10</div><div class="cal-date">11</div><div class="cal-date">12</div><div class="cal-date">13</div><div class="cal-date">14</div>
        </div>
        <input type="text" id="event-note" class="text-input" style="margin-top: 12px;" placeholder="Catatan event hari ini..." oninput="saveData()">
      </div>

      <!-- DIAGRAM PROGRESS TARGET (MULTI TEMPLATE) -->
      <div class="card">
        <div class="card-title">
          <span>📊 Progress Habits</span>
          <select id="diagram-style" class="habit-date-tag" onchange="changeDiagramStyle()">
            <option value="smooth">Gaya Smooth</option>
            <option value="segmented">Gaya Segmen Block</option>
          </select>
        </div>
        
        <!-- Style 1: Smooth -->
        <div id="diagram-smooth" class="progress-outer">
          <div class="progress-inner" id="progress-bar"></div>
        </div>

        <!-- Style 2: Segmented -->
        <div id="diagram-segmented" class="style-segmented" style="display:none; margin-top: 10px;">
          <div class="segment-block"></div><div class="segment-block"></div><div class="segment-block"></div><div class="segment-block"></div><div class="segment-block"></div>
        </div>
        <div style="text-align: right; font-size: 0.8rem; margin-top: 6px; font-weight: 600;" id="progress-percent">0%</div>
      </div>

      <!-- HABIT CHECKLIST CUSTOM & SPREADSHEET INPUT -->
      <div class="card">
        <div class="card-title">
          <span>✨ Custom Habit Checklist</span>
          <span class="sticker">📝</span>
        </div>
        
        <div class="add-input-group">
          <input type="text" id="new-habit-text" class="text-input" placeholder="Tambah kebiasaan baru...">
          <input type="date" id="new-habit-date" class="text-input" style="width: 130px;">
          <button class="btn-add" onclick="addHabit()">+</button>
        </div>

        <div id="habit-list"></div>
      </div>

      <!-- TRACKER BANGUN & TIDUR -->
      <div class="card">
        <div class="card-title">
          <span>🌙 Tracker Waktu Tidur</span>
          <span id="sleep-total" style="font-size: 0.85rem; color: var(--accent);">0 Jam</span>
        </div>
        <div class="time-grid">
          <div>
            <label style="font-size: 0.75rem; color: var(--text-muted);">Jam Tidur Malam</label>
            <input type="time" id="sleep-time" class="text-input" onchange="calculateSleep()">
          </div>
          <div>
            <label style="font-size: 0.75rem; color: var(--text-muted);">Jam Bangun Pagi</label>
            <input type="time" id="wake-time" class="text-input" onchange="calculateSleep()">
          </div>
        </div>
      </div>

      <!-- REFLEKSI & KEUANGAN -->
      <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); gap: 16px;">
        <div class="card">
          <div class="card-title">💡 Catatan Pembelajaran</div>
          <textarea id="reflection-text" placeholder="Tuliskan pelajaran berharga hari ini..." oninput="saveData()"></textarea>
        </div>

        <div class="card">
          <div class="card-title">💰 Keuangan Sederhana</div>
          <div class="time-grid">
            <input type="number" id="income" class="text-input" placeholder="Masuk (Rp)" oninput="calculateFinance()">
            <input type="number" id="expense" class="text-input" placeholder="Keluar (Rp)" oninput="calculateFinance()">
          </div>
          <div style="margin-top: 10px; font-weight: 600; font-size: 0.85rem;">
            Sisa Saldo: <span id="balance" style="color: var(--accent);">Rp 0</span>
          </div>
        </div>
      </div>

    </div>

    <!-- TAB CONTENT 2: MINGGUAN -->
    <div id="tab-Mingguan" class="tab-content">
      <div class="card">
        <div class="card-title">🗓️ Schedule & Target Mingguan</div>
        <table class="sheet-grid">
          <tr><th>Hari</th><th>Target / Agenda Utama</th></tr>
          <tr><td>Senin</td><td><input type="text" class="text-input" id="w-mon" oninput="saveData()"></td></tr>
          <tr><td>Selasa</td><td><input type="text" class="text-input" id="w-tue" oninput="saveData()"></td></tr>
          <tr><td>Rabu</td><td><input type="text" class="text-input" id="w-wed" oninput="saveData()"></td></tr>
          <tr><td>Kamis</td><td><input type="text" class="text-input" id="w-thu" oninput="saveData()"></td></tr>
          <tr><td>Jumat</td><td><input type="text" class="text-input" id="w-fri" oninput="saveData()"></td></tr>
          <tr><td>Sabtu</td><td><input type="text" class="text-input" id="w-sat" oninput="saveData()"></td></tr>
          <tr><td>Minggu</td><td><input type="text" class="text-input" id="w-sun" oninput="saveData()"></td></tr>
        </table>
      </div>
    </div>

    <!-- TAB CONTENT 3: BULANAN -->
    <div id="tab-Bulanan" class="tab-content">
      <div class="card">
        <div class="card-title">📌 Monthly Tracker Spreadsheet</div>
        <textarea id="m-goals" style="min-height: 120px;" placeholder="Tuliskan semua daftar project dan fokus utama bulan ini..." oninput="saveData()"></textarea>
      </div>
    </div>

    <!-- TAB CONTENT 4: TAHUNAN -->
    <div id="tab-Tahunan" class="tab-content">
      <div class="card">
        <div class="card-title">🎯 Big Goals 2026</div>
        <textarea id="y-goals" style="min-height: 150px;" placeholder="Tuliskan mimpi besar, rencana tabungan, atau target tahun ini..." oninput="saveData()"></textarea>
      </div>
    </div>

  </div>

</div>

<script>
  // STATE MANAGEMENT WITH LOCALSTORAGE (SIMPAN OTOMATIS)
  let habits = JSON.parse(localStorage.getItem('my_habits')) || [];

  window.onload = function() {
    loadData();
    renderHabits();
    // Default Tanggal Hari Ini
    const now = new Date();
    document.getElementById('new-habit-date').valueToDate = now;
  };

  function selectTemplate(theme) {
    document.body.setAttribute('data-theme', theme);
    document.getElementById('page-templates').style.display = 'none';
    document.getElementById('page-dashboard').style.display = 'block';
    localStorage.setItem('saved_theme', theme);
  }

  function resetTemplate() {
    document.getElementById('page-templates').style.display = 'block';
    document.getElementById('page-dashboard').style.display = 'none';
  }

  function switchView(viewName) {
    document.getElementById('view-title').innerText = `Schedule ${viewName}`;
    
    // Switch Active Buttons
    document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
    event.target.classList.add('active');

    // Switch Active Tab Content
    document.querySelectorAll('.tab-content').forEach(tab => tab.classList.remove('active'));
    document.getElementById(`tab-${viewName}`).classList.add('active');
  }

  /* HABIT FUNCTIONS */
  function addHabit() {
    const textInput = document.getElementById('new-habit-text');
    const dateInput = document.getElementById('new-habit-date');

    if (!textInput.value.trim()) return;

    const newHabit = {
      id: Date.now(),
      text: textInput.value,
      date: dateInput.value || 'Hari ini',
      completed: false
    };

    habits.push(newHabit);
    textInput.value = '';
    renderHabits();
    saveHabits();
  }

  function deleteHabit(id) {
    habits = habits.filter(h => h.id !== id);
    renderHabits();
    saveHabits();
  }

  function toggleHabit(id) {
    const habit = habits.find(h => h.id
