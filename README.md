<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SAB Value Calculator | Steal a Brainrot</title>
<link href="https://fonts.googleapis.com/css2?family=Fredoka+One&family=Nunito:wght@400;600;700;800;900&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0d0d1a; --card: #151528; --card2: #1a1a35; --border: #2a2a55;
    --accent: #ff3cac; --accent2: #7b2fff; --accent3: #00f5ff; --gold: #ffd700;
    --text: #e8e8ff; --muted: #8888bb;
    --common: #9ca3af; --rare: #3b82f6; --epic: #a855f7;
    --legendary: #f59e0b; --mythic: #ef4444; --god: #ff3cac;
    --secret: #00f5ff; --og: #ffd700;
    --win: #22c55e; --lose: #ef4444; --fair: #f59e0b;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: 'Nunito', sans-serif; background: var(--bg); color: var(--text); min-height: 100vh; overflow-x: hidden; }
  body::before {
    content: ''; position: fixed; inset: 0; pointer-events: none; z-index: 0;
    background: radial-gradient(ellipse at 20% 20%, rgba(123,47,255,0.15) 0%, transparent 50%),
                radial-gradient(ellipse at 80% 80%, rgba(255,60,172,0.12) 0%, transparent 50%);
  }
  header {
    position: relative; z-index: 10; padding: 16px 24px;
    display: flex; align-items: center; justify-content: space-between;
    border-bottom: 1px solid var(--border); background: rgba(13,13,26,0.92); backdrop-filter: blur(12px);
  }
  .logo { display: flex; align-items: center; gap: 10px; text-decoration: none; }
  .logo-icon { width: 38px; height: 38px; background: linear-gradient(135deg, var(--accent2), var(--accent)); border-radius: 10px; display: flex; align-items: center; justify-content: center; font-size: 20px; }
  .logo-text { font-family: 'Fredoka One', cursive; font-size: 22px; background: linear-gradient(90deg, var(--accent3), var(--accent)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  nav { display: flex; gap: 8px; }
  nav a { color: var(--muted); text-decoration: none; font-weight: 700; font-size: 13px; padding: 6px 12px; border-radius: 8px; transition: all 0.2s; }
  nav a:hover, nav a.active { color: var(--accent3); background: var(--border); }
  .hero { position: relative; z-index: 1; text-align: center; padding: 40px 24px 24px; }
  .hero h1 { font-family: 'Fredoka One', cursive; font-size: clamp(26px, 5vw, 50px); background: linear-gradient(135deg, var(--accent3), var(--accent2) 40%, var(--accent)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; line-height: 1.1; margin-bottom: 8px; }
  .hero p { color: var(--muted); font-size: 15px; font-weight: 600; }
  .main { position: relative; z-index: 1; max-width: 1200px; margin: 0 auto; padding: 24px 16px 48px; }
  .calc-wrapper { display: grid; grid-template-columns: 1fr auto 1fr; gap: 16px; align-items: start; margin-bottom: 24px; }
  .trade-side { background: var(--card); border: 1px solid var(--border); border-radius: 20px; padding: 20px; position: relative; overflow: hidden; }
  .trade-side::before { content: ''; position: absolute; top: 0; left: 0; right: 0; height: 3px; }
  .trade-side.yours::before { background: linear-gradient(90deg, var(--accent2), var(--accent)); }
  .trade-side.theirs::before { background: linear-gradient(90deg, var(--accent3), var(--accent2)); }
  .side-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 16px; }
  .side-title { font-family: 'Fredoka One', cursive; font-size: 18px; }
  .clear-btn { font-size: 11px; font-weight: 700; color: var(--muted); background: none; border: 1px solid var(--border); padding: 4px 10px; border-radius: 6px; cursor: pointer; transition: all 0.2s; font-family: 'Nunito', sans-serif; }
  .clear-btn:hover { color: var(--accent); border-color: var(--accent); }
  .value-display { background: var(--card2); border: 1px solid var(--border); border-radius: 12px; padding: 14px 16px; margin-bottom: 14px; display: flex; justify-content: space-between; align-items: center; }
  .value-label { font-size: 11px; font-weight: 800; color: var(--muted); text-transform: uppercase; letter-spacing: 1px; }
  .value-number { font-family: 'Fredoka One', cursive; font-size: 24px; }
  .items-list { min-height: 80px; margin-bottom: 14px; }
  .trade-item { display: flex; align-items: center; gap: 10px; background: var(--card2); border: 1px solid var(--border); border-radius: 10px; padding: 8px 12px; margin-bottom: 8px; animation: slideIn 0.2s ease; }
  @keyframes slideIn { from { opacity: 0; transform: translateY(-8px); } to { opacity: 1; transform: translateY(0); } }
  .item-emoji { font-size: 20px; flex-shrink: 0; }
  .item-info { flex: 1; min-width: 0; }
  .item-name { font-weight: 800; font-size: 12px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .item-value-tag { font-family: 'Fredoka One', cursive; font-size: 13px; color: var(--gold); flex-shrink: 0; }
  .remove-item { background: none; border: none; color: var(--muted); cursor: pointer; font-size: 14px; padding: 2px 6px; border-radius: 6px; transition: all 0.2s; font-weight: 900; }
  .remove-item:hover { color: var(--accent); background: rgba(255,60,172,0.1); }
  .add-item-btn { width: 100%; padding: 12px; border: 2px dashed var(--border); border-radius: 12px; background: none; color: var(--muted); font-family: 'Nunito', sans-serif; font-weight: 800; font-size: 14px; cursor: pointer; transition: all 0.2s; display: flex; align-items: center; justify-content: center; gap: 8px; }
  .add-item-btn:hover { border-color: var(--accent2); color: var(--accent3); background: rgba(123,47,255,0.06); }
  .calc-center { display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 12px; padding-top: 80px; }
  .vs-badge { font-family: 'Fredoka One', cursive; font-size: 28px; background: linear-gradient(135deg, var(--accent2), var(--accent)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .result-badge { font-family: 'Fredoka One', cursive; font-size: 16px; padding: 10px 18px; border-radius: 12px; text-align: center; min-width: 90px; transition: all 0.3s; }
  .result-badge.win { background: rgba(34,197,94,0.15); border: 2px solid var(--win); color: var(--win); }
  .result-badge.lose { background: rgba(239,68,68,0.15); border: 2px solid var(--lose); color: var(--lose); }
  .result-badge.fair { background: rgba(245,158,11,0.15); border: 2px solid var(--fair); color: var(--fair); }
  .result-badge.neutral { background: rgba(136,136,187,0.1); border: 2px solid var(--border); color: var(--muted); }
  .diff-label { font-size: 11px; font-weight: 700; color: var(--muted); text-align: center; }
  .modal-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.78); z-index: 100; backdrop-filter: blur(4px); align-items: center; justify-content: center; padding: 16px; }
  .modal-overlay.open { display: flex; }
  .modal { background: var(--card); border: 1px solid var(--border); border-radius: 24px; width: 100%; max-width: 660px; max-height: 82vh; display: flex; flex-direction: column; overflow: hidden; animation: popIn 0.25s cubic-bezier(0.34,1.56,0.64,1); }
  @keyframes popIn { from { transform: scale(0.9); opacity: 0; } to { transform: scale(1); opacity: 1; } }
  .modal-header { padding: 20px 20px 12px; border-bottom: 1px solid var(--border); position: relative; }
  .modal-title { font-family: 'Fredoka One', cursive; font-size: 20px; margin-bottom: 12px; background: linear-gradient(90deg, var(--accent3), var(--accent2)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .search-input { width: 100%; padding: 10px 14px; background: var(--card2); border: 1px solid var(--border); border-radius: 10px; color: var(--text); font-family: 'Nunito', sans-serif; font-weight: 700; font-size: 14px; outline: none; }
  .search-input:focus { border-color: var(--accent2); }
  .search-input::placeholder { color: var(--muted); }
  .rarity-filters { display: flex; gap: 5px; flex-wrap: wrap; margin-top: 10px; }
  .rarity-filter { font-size: 10px; font-weight: 800; padding: 3px 9px; border-radius: 99px; cursor: pointer; border: 1.5px solid var(--border); font-family: 'Nunito', sans-serif; background: var(--card2); color: var(--muted); transition: all 0.15s; }
  .rarity-filter[data-rarity="all"].active    { border-color: var(--accent3); color: var(--accent3); }
  .rarity-filter[data-rarity="Common"].active { border-color: var(--common); color: var(--common); }
  .rarity-filter[data-rarity="Rare"].active   { border-color: var(--rare); color: var(--rare); }
  .rarity-filter[data-rarity="Epic"].active   { border-color: var(--epic); color: var(--epic); }
  .rarity-filter[data-rarity="Legendary"].active { border-color: var(--legendary); color: var(--legendary); }
  .rarity-filter[data-rarity="Mythic"].active { border-color: var(--mythic); color: var(--mythic); }
  .rarity-filter[data-rarity="Brainrot God"].active { border-color: var(--god); color: var(--god); }
  .rarity-filter[data-rarity="Secret"].active { border-color: var(--secret); color: var(--secret); }
  .rarity-filter[data-rarity="OG"].active     { border-color: var(--og); color: var(--og); }
  .items-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(128px, 1fr)); gap: 8px; padding: 14px 16px; overflow-y: auto; flex: 1; }
  .item-card { background: var(--card2); border: 1px solid var(--border); border-radius: 12px; padding: 10px 8px; text-align: center; cursor: pointer; transition: all 0.2s; }
  .item-card:hover { border-color: var(--accent2); transform: translateY(-2px); box-shadow: 0 8px 20px rgba(123,47,255,0.2); }
  .ic-emoji { font-size: 28px; display: block; margin-bottom: 5px; }
  .ic-name { font-weight: 800; font-size: 10px; line-height: 1.3; margin-bottom: 4px; color: var(--text); min-height: 26px; display: flex; align-items: center; justify-content: center; }
  .ic-rarity { font-size: 9px; font-weight: 800; padding: 2px 6px; border-radius: 99px; display: inline-block; text-transform: uppercase; }
  .ic-stats { font-size: 10px; color: var(--muted); margin-top: 3px; font-weight: 700; }
  .modal-close { position: absolute; top: 16px; right: 16px; background: var(--card2); border: 1px solid var(--border); color: var(--muted); font-size: 16px; width: 30px; height: 30px; border-radius: 8px; cursor: pointer; display: flex; align-items: center; justify-content: center; font-weight: 900; transition: all 0.2s; }
  .modal-close:hover { color: var(--accent); border-color: var(--accent); }
  .value-table-section { background: var(--card); border: 1px solid var(--border); border-radius: 20px; padding: 24px; margin-top: 24px; }
  .section-title { font-family: 'Fredoka One', cursive; font-size: 22px; margin-bottom: 16px; display: flex; align-items: center; gap: 10px; }
  .section-title::after { content: ''; flex: 1; height: 1px; background: var(--border); }
  .vt-filters { display: flex; gap: 7px; margin-bottom: 14px; flex-wrap: wrap; }
  .vt-filter { font-size: 11px; font-weight: 800; padding: 5px 12px; border-radius: 10px; cursor: pointer; border: 1.5px solid var(--border); background: var(--card2); color: var(--muted); transition: all 0.15s; font-family: 'Nunito', sans-serif; }
  .vt-filter:hover { border-color: var(--accent2); color: var(--text); }
  .vt-filter.active { border-color: var(--accent3); color: var(--accent3); background: rgba(0,245,255,0.05); }
  .vt-search { width: 100%; padding: 10px 14px; background: var(--card2); border: 1px solid var(--border); border-radius: 10px; color: var(--text); font-family: 'Nunito', sans-serif; font-weight: 700; font-size: 14px; outline: none; margin-bottom: 14px; }
  .vt-search:focus { border-color: var(--accent2); }
  .vt-search::placeholder { color: var(--muted); }
  table { width: 100%; border-collapse: collapse; }
  th { font-size: 11px; font-weight: 800; text-transform: uppercase; letter-spacing: 1px; color: var(--muted); padding: 0 12px 10px; text-align: left; border-bottom: 1px solid var(--border); }
  td { padding: 9px 12px; border-bottom: 1px solid rgba(42,42,85,0.4); vertical-align: middle; }
  tr:last-child td { border-bottom: none; }
  tr:hover td { background: rgba(42,42,85,0.25); }
  .td-name { display: flex; align-items: center; gap: 9px; }
  .td-emoji { font-size: 20px; flex-shrink: 0; }
  .name-text { font-weight: 800; font-size: 12px; }
  .td-val { font-family: 'Fredoka One', cursive; font-size: 14px; color: var(--gold); }
  .td-income { font-weight: 700; font-size: 12px; color: var(--accent3); }
  .td-price { font-weight: 700; font-size: 12px; color: var(--muted); }
  .rarity-pill { font-size: 9px; font-weight: 800; padding: 2px 7px; border-radius: 99px; display: inline-block; text-transform: uppercase; letter-spacing: 0.5px; }
  .r-Common { background: rgba(156,163,175,0.15); color: var(--common); }
  .r-Rare    { background: rgba(59,130,246,0.15); color: var(--rare); }
  .r-Epic    { background: rgba(168,85,247,0.15); color: var(--epic); }
  .r-Legendary { background: rgba(245,158,11,0.15); color: var(--legendary); }
  .r-Mythic  { background: rgba(239,68,68,0.15); color: var(--mythic); }
  .r-God     { background: rgba(255,60,172,0.15); color: var(--god); }
  .r-Secret  { background: rgba(0,245,255,0.15); color: var(--secret); }
  .r-OG      { background: rgba(255,215,0,0.15); color: var(--og); }
  .empty-items { text-align: center; padding: 24px; color: var(--muted); font-size: 13px; font-weight: 700; }
  .empty-icon { font-size: 30px; display: block; margin-bottom: 8px; }
  .no-results { text-align: center; padding: 32px; color: var(--muted); font-weight: 700; }
  footer { position: relative; z-index: 1; text-align: center; padding: 24px; color: var(--muted); font-size: 12px; border-top: 1px solid var(--border); font-weight: 700; }
  @media (max-width: 700px) {
    .calc-wrapper { grid-template-columns: 1fr; }
    .calc-center { flex-direction: row; padding: 0; margin: -8px 0; }
    .vs-badge { font-size: 20px; }
    .items-grid { grid-template-columns: repeat(auto-fill, minmax(100px, 1fr)); }
    th:nth-child(4), td:nth-child(4) { display: none; }
  }
</style>
</head>
<body>

<div style="background:linear-gradient(90deg,#5865F2,#7b2fff);text-align:center;padding:8px 16px;font-size:13px;font-weight:800;color:#fff;display:flex;align-items:center;justify-content:center;gap:10px;position:relative;z-index:11;">
  <span>⚡ Made by <strong>MSY</strong> — DC Server Owner</span>
  <a href="https://discord.gg/Auue8hpf" target="_blank" style="background:rgba(255,255,255,0.2);color:#fff;text-decoration:none;padding:4px 12px;border-radius:99px;font-size:12px;display:inline-flex;align-items:center;gap:5px;" onmouseover="this.style.background='rgba(255,255,255,0.35)'" onmouseout="this.style.background='rgba(255,255,255,0.2)'">
    <svg width="14" height="14" viewBox="0 0 24 24" fill="white"><path d="M20.317 4.37a19.791 19.791 0 0 0-4.885-1.515.074.074 0 0 0-.079.037c-.21.375-.444.864-.608 1.25a18.27 18.27 0 0 0-5.487 0 12.64 12.64 0 0 0-.617-1.25.077.077 0 0 0-.079-.037A19.736 19.736 0 0 0 3.677 4.37a.07.07 0 0 0-.032.027C.533 9.046-.32 13.58.099 18.057a.082.082 0 0 0 .031.057 19.9 19.9 0 0 0 5.993 3.03.078.078 0 0 0 .084-.028 14.09 14.09 0 0 0 1.226-1.994.076.076 0 0 0-.041-.106 13.107 13.107 0 0 1-1.872-.892.077.077 0 0 1-.008-.128 10.2 10.2 0 0 0 .372-.292.074.074 0 0 1 .077-.01c3.928 1.793 8.18 1.793 12.062 0a.074.074 0 0 1 .078.01c.12.098.246.198.373.292a.077.077 0 0 1-.006.127 12.299 12.299 0 0 1-1.873.892.077.077 0 0 0-.041.107c.36.698.772 1.362 1.225 1.993a.076.076 0 0 0 .084.028 19.839 19.839 0 0 0 6.002-3.03.077.077 0 0 0 .032-.054c.5-5.177-.838-9.674-3.549-13.66a.061.061 0 0 0-.031-.03z"/></svg>
    Join Discord
  </a>
</div>
<header>
  <a class="logo" href="#"><div class="logo-icon">🧠</div><span class="logo-text">SAB Values</span></a>
  <nav><a href="#" class="active">Calculator</a><a href="#value-list">Value List</a></nav>
</header>

<div class="hero">
  <h1>Steal a Brainrot<br>Trade Calculator</h1>
  <p>Check values, compare trades, and never get scammed 🔥</p>
</div>

<div class="main">
  <div class="calc-wrapper">
    <div class="trade-side yours">
      <div class="side-header">
        <span class="side-title">Your Offer</span>
        <button class="clear-btn" onclick="clearSide('yours')">Clear</button>
      </div>
      <div class="value-display">
        <span class="value-label">💰 Buy Price Total</span>
        <span class="value-number" id="yours-value">$0</span>
      </div>
      <div class="items-list" id="yours-items">
        <div class="empty-items"><span class="empty-icon">🫙</span>Add brainrots to your offer</div>
      </div>
      <button class="add-item-btn" onclick="openModal('yours')">＋ Add Brainrot</button>
    </div>

    <div class="calc-center">
      <div class="vs-badge">VS</div>
      <div class="result-badge neutral" id="result-badge">—</div>
      <div class="diff-label" id="diff-label">Add items</div>
    </div>

    <div class="trade-side theirs">
      <div class="side-header">
        <span class="side-title">Their Offer</span>
        <button class="clear-btn" onclick="clearSide('theirs')">Clear</button>
      </div>
      <div class="value-display">
        <span class="value-label">💰 Buy Price Total</span>
        <span class="value-number" id="theirs-value">$0</span>
      </div>
      <div class="items-list" id="theirs-items">
        <div class="empty-items"><span class="empty-icon">🫙</span>Add brainrots to their offer</div>
      </div>
      <button class="add-item-btn" onclick="openModal('theirs')">＋ Add Brainrot</button>
    </div>
  </div>

  <div class="value-table-section" id="value-list">
    <div class="section-title">📋 Full Value List</div>
    <div class="vt-filters" id="vt-filters">
      <button class="vt-filter active" data-rarity="all">All</button>
      <button class="vt-filter" data-rarity="Common">Common</button>
      <button class="vt-filter" data-rarity="Rare">Rare</button>
      <button class="vt-filter" data-rarity="Epic">Epic</button>
      <button class="vt-filter" data-rarity="Legendary">Legendary</button>
      <button class="vt-filter" data-rarity="Mythic">Mythic</button>
      <button class="vt-filter" data-rarity="Brainrot God">Brainrot God</button>
      <button class="vt-filter" data-rarity="Secret">Secret</button>
      <button class="vt-filter" data-rarity="OG">OG</button>
    </div>
    <input class="vt-search" type="text" placeholder="🔍 Search brainrots..." id="vt-search">
    <div style="overflow-x:auto;">
      <table>
        <thead><tr><th>Brainrot</th><th>Rarity</th><th>Buy Price</th><th>Income/s</th></tr></thead>
        <tbody id="value-table-body"></tbody>
      </table>
    </div>
    <div id="vt-no-results" class="no-results" style="display:none;">No brainrots found 🔍</div>
  </div>
</div>

<div class="modal-overlay" id="modal" onclick="closeModalOutside(event)">
  <div class="modal">
    <div class="modal-header">
      <div class="modal-title">Select a Brainrot</div>
      <button class="modal-close" onclick="closeModal()">✕</button>
      <input class="search-input" type="text" placeholder="🔍 Search..." id="modal-search" oninput="renderModalItems()">
      <div class="rarity-filters">
        <button class="rarity-filter active" data-rarity="all" onclick="setModalRarity('all')">All</button>
        <button class="rarity-filter" data-rarity="Common" onclick="setModalRarity('Common')">Common</button>
        <button class="rarity-filter" data-rarity="Rare" onclick="setModalRarity('Rare')">Rare</button>
        <button class="rarity-filter" data-rarity="Epic" onclick="setModalRarity('Epic')">Epic</button>
        <button class="rarity-filter" data-rarity="Legendary" onclick="setModalRarity('Legendary')">Legendary</button>
        <button class="rarity-filter" data-rarity="Mythic" onclick="setModalRarity('Mythic')">Mythic</button>
        <button class="rarity-filter" data-rarity="Brainrot God" onclick="setModalRarity('Brainrot God')">God</button>
        <button class="rarity-filter" data-rarity="Secret" onclick="setModalRarity('Secret')">Secret</button>
        <button class="rarity-filter" data-rarity="OG" onclick="setModalRarity('OG')">OG</button>
      </div>
    </div>
    <div class="items-grid" id="items-grid"></div>
  </div>
</div>

<footer>
  <p style="margin-bottom:8px;">SAB Value Calculator — Community resource. Not affiliated with Roblox or Steal a Brainrot™.</p>
  <p style="font-size:13px;">
    Made with ❤️ by <strong style="color:var(--accent3);">MSY</strong> — DC Server Owner &nbsp;·&nbsp;
    <a href="https://discord.gg/Auue8hpf" target="_blank" style="color:#5865F2;font-weight:800;text-decoration:none;display:inline-flex;align-items:center;gap:5px;">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="#5865F2" style="vertical-align:middle;"><path d="M20.317 4.37a19.791 19.791 0 0 0-4.885-1.515.074.074 0 0 0-.079.037c-.21.375-.444.864-.608 1.25a18.27 18.27 0 0 0-5.487 0 12.64 12.64 0 0 0-.617-1.25.077.077 0 0 0-.079-.037A19.736 19.736 0 0 0 3.677 4.37a.07.07 0 0 0-.032.027C.533 9.046-.32 13.58.099 18.057a.082.082 0 0 0 .031.057 19.9 19.9 0 0 0 5.993 3.03.078.078 0 0 0 .084-.028 14.09 14.09 0 0 0 1.226-1.994.076.076 0 0 0-.041-.106 13.107 13.107 0 0 1-1.872-.892.077.077 0 0 1-.008-.128 10.2 10.2 0 0 0 .372-.292.074.074 0 0 1 .077-.01c3.928 1.793 8.18 1.793 12.062 0a.074.074 0 0 1 .078.01c.12.098.246.198.373.292a.077.077 0 0 1-.006.127 12.299 12.299 0 0 1-1.873.892.077.077 0 0 0-.041.107c.36.698.772 1.362 1.225 1.993a.076.076 0 0 0 .084.028 19.839 19.839 0 0 0 6.002-3.03.077.077 0 0 0 .032-.054c.5-5.177-.838-9.674-3.549-13.66a.061.061 0 0 0-.031-.03z"/></svg>
      Join Discord
    </a>
  </p>
</footer>

<script>
const brainrots = [
  // COMMON
  { name:"Noobini Pizzanini",         emoji:"🍕", rarity:"Common",       income:1,        price:25 },
  { name:"Lirili Larila",             emoji:"🌺", rarity:"Common",       income:3,        price:250 },
  { name:"Tim Cheese",                emoji:"🧀", rarity:"Common",       income:5,        price:500 },
  { name:"FluriFlura",                emoji:"🌸", rarity:"Common",       income:7,        price:750 },
  { name:"Talpa Di Fero",             emoji:"🦔", rarity:"Common",       income:9,        price:1000 },
  { name:"Noobini Santanini",         emoji:"🎅", rarity:"Common",       income:11,       price:1100 },
  { name:"Svinina Bombardino",        emoji:"🐷", rarity:"Common",       income:10,       price:1200 },
  { name:"Racooni Jandelini",         emoji:"🦝", rarity:"Common",       income:12,       price:1300 },
  { name:"Pipi Kiwi",                 emoji:"🥝", rarity:"Common",       income:13,       price:1500 },
  { name:"Tartaragno",                emoji:"🐢", rarity:"Common",       income:13,       price:1600 },
  { name:"Pipi Corni",                emoji:"🌽", rarity:"Common",       income:14,       price:1700 },
  // RARE
  { name:"Trippi Troppi",             emoji:"🦩", rarity:"Rare",         income:15,       price:2000 },
  { name:"Tung Tung Tung Sahur",      emoji:"🥁", rarity:"Rare",         income:25,       price:3000 },
  { name:"Gangster Footera",          emoji:"👟", rarity:"Rare",         income:30,       price:4000 },
  { name:"Bandito Bobritto",          emoji:"🦫", rarity:"Rare",         income:35,       price:4500 },
  { name:"Boneca Ambalabu",           emoji:"🎎", rarity:"Rare",         income:40,       price:5000 },
  { name:"Cacto Hipopotamo",          emoji:"🦛", rarity:"Rare",         income:50,       price:6500 },
  { name:"Ta Ta Ta Ta Sahur",         emoji:"🌙", rarity:"Rare",         income:55,       price:7500 },
  { name:"Tric Trac Baraboom",        emoji:"💥", rarity:"Rare",         income:65,       price:9000 },
  { name:"Frogo Elfo",                emoji:"🐸", rarity:"Rare",         income:67,       price:9200 },
  { name:"Pipi Avocado",              emoji:"🥑", rarity:"Rare",         income:70,       price:9500 },
  { name:"Pinealotto Fruttarino",     emoji:"🍍", rarity:"Rare",         income:75,       price:9700 },
  // EPIC
  { name:"Cappuccino Assassino",      emoji:"☕", rarity:"Epic",         income:75,       price:10000 },
  { name:"Brr Brr Patapim",           emoji:"🐻", rarity:"Epic",         income:100,      price:15000 },
  { name:"Trulimero Trulicina",       emoji:"🎭", rarity:"Epic",         income:125,      price:20000 },
  { name:"Bambini Crostini",          emoji:"🍞", rarity:"Epic",         income:130,      price:22500 },
  { name:"Bananita Dolphinita",       emoji:"🐬", rarity:"Epic",         income:150,      price:25000 },
  { name:"Perochello Lemonchello",    emoji:"🍋", rarity:"Epic",         income:160,      price:27500 },
  { name:"Brri Brri Bicus Dicus Bombicus",emoji:"🪲",rarity:"Epic",     income:175,      price:30000 },
  { name:"Avocadini Guffo",           emoji:"🦉", rarity:"Epic",         income:225,      price:35000 },
  { name:"Ti Ti Ti Sahur",            emoji:"🌙", rarity:"Epic",         income:225,      price:37500 },
  { name:"Salamino Penguino",         emoji:"🐧", rarity:"Epic",         income:250,      price:40000 },
  { name:"Penguin Tree",              emoji:"🌲", rarity:"Epic",         income:270,      price:42000 },
  { name:"Penguino Cocosino",         emoji:"🥥", rarity:"Epic",         income:300,      price:45000 },
  { name:"Mummio Rappitto",           emoji:"🧟", rarity:"Epic",         income:310,      price:47000 },
  // LEGENDARY
  { name:"Burbaloni Loliloli",        emoji:"🫧", rarity:"Legendary",    income:300,      price:50000 },
  { name:"Chimpanzini Bananini",      emoji:"🐒", rarity:"Legendary",    income:350,      price:60000 },
  { name:"Kirilikalika",              emoji:"🦜", rarity:"Legendary",    income:400,      price:70000 },
  { name:"Kirilikalato",              emoji:"🦚", rarity:"Legendary",    income:430,      price:75000 },
  { name:"Ballerina Cappuccina",      emoji:"💃", rarity:"Legendary",    income:500,      price:90000 },
  { name:"Chef Crabracadaba",         emoji:"🦀", rarity:"Legendary",    income:550,      price:100000 },
  { name:"Lionel Cactuseli",          emoji:"🌵", rarity:"Legendary",    income:600,      price:110000 },
  { name:"Glorbo Fruttodillo",        emoji:"🍓", rarity:"Legendary",    income:650,      price:120000 },
  { name:"Quivioli Ameleonni",        emoji:"🦎", rarity:"Legendary",    income:720,      price:140000 },
  { name:"Blueberrenni",              emoji:"🫐", rarity:"Legendary",    income:750,      price:150000 },
  { name:"Octopusini Clickerino",     emoji:"🐙", rarity:"Legendary",    income:800,      price:160000 },
  { name:"Clabo Caramello",           emoji:"🍮", rarity:"Legendary",    income:850,      price:175000 },
  { name:"Pipi Potato",               emoji:"🥔", rarity:"Legendary",    income:900,      price:185000 },
  { name:"Strawberrlli",              emoji:"🍓", rarity:"Legendary",    income:950,      price:200000 },
  { name:"Flamingelli",               emoji:"🦩", rarity:"Legendary",    income:1000,     price:210000 },
  { name:"Cocosino Mamá",             emoji:"🥥", rarity:"Legendary",    income:1050,     price:220000 },
  { name:"Pandaccini Bananini",       emoji:"🐼", rarity:"Legendary",    income:1100,     price:240000 },
  { name:"Quackula",                  emoji:"🦆", rarity:"Legendary",    income:1150,     price:255000 },
  { name:"Pipi Watermelon",           emoji:"🍉", rarity:"Legendary",    income:1200,     price:270000 },
  { name:"Signore Carapece",          emoji:"🐊", rarity:"Legendary",    income:1250,     price:290000 },
  { name:"Sigma Boy",                 emoji:"😤", rarity:"Legendary",    income:1300,     price:310000 },
  { name:"Chocco Bunny",              emoji:"🐰", rarity:"Legendary",    income:1350,     price:320000 },
  { name:"Puffaball",                 emoji:"🐡", rarity:"Legendary",    income:1400,     price:330000 },
  { name:"Sigma Girl",                emoji:"💪", rarity:"Legendary",    income:1500,     price:340000 },
  { name:"Sealo Regalo",              emoji:"🦭", rarity:"Legendary",    income:1800,     price:345000 },
  { name:"Buho De Fuego",             emoji:"🦅", rarity:"Legendary",    income:1800,     price:345000 },
  // MYTHIC
  { name:"Frigo Camelo",              emoji:"🐫", rarity:"Mythic",       income:1400,     price:300000 },
  { name:"Orangutini Ananassini",     emoji:"🦧", rarity:"Mythic",       income:1700,     price:400000 },
  { name:"Rhino Toasterino",          emoji:"🦏", rarity:"Mythic",       income:2100,     price:450000 },
  { name:"Bombardiro Crocodilo",      emoji:"🐊", rarity:"Mythic",       income:2500,     price:500000 },
  { name:"Spioniro Golubiro",         emoji:"🕊️", rarity:"Mythic",       income:3500,     price:750000 },
  { name:"Bombombini Gusini",         emoji:"💣", rarity:"Mythic",       income:5000,     price:1000000 },
  { name:"Zibra Zubra Zibralini",     emoji:"🦓", rarity:"Mythic",       income:6000,     price:1000000 },
  { name:"Tigrilini Watermelini",     emoji:"🐯", rarity:"Mythic",       income:6500,     price:1000000 },
  { name:"Avocadorilla",              emoji:"🥑", rarity:"Mythic",       income:7500,     price:2000000 },
  { name:"Cavallo Virtuso",           emoji:"🐴", rarity:"Mythic",       income:7500,     price:2500000 },
  { name:"Te Te Te Sahur",            emoji:"🌙", rarity:"Mythic",       income:9500,     price:2500000 },
  { name:"Gorillo Watermelondrillo",  emoji:"🦍", rarity:"Mythic",       income:8000,     price:3000000 },
  { name:"Tracoducotulu Delapeladustuz",emoji:"🐛",rarity:"Mythic",      income:12000,    price:3000000 },
  { name:"Lerulerulerule",            emoji:"🌀", rarity:"Mythic",       income:8700,     price:3500000 },
  { name:"Tob Tobi Tobi",             emoji:"🐸", rarity:"Mythic",       income:8500,     price:3500000 },
  { name:"Ganganzelli Trulala",       emoji:"🎶", rarity:"Mythic",       income:9000,     price:4000000 },
  { name:"Carloo",                    emoji:"🚗", rarity:"Mythic",       income:13500,    price:4500000 },
  { name:"Tree Tree Tree Sahur",      emoji:"🌲", rarity:"Mythic",       income:17000,    price:4900000 },
  // BRAINROT GOD
  { name:"Cocofanto Elefanto",        emoji:"🐘", rarity:"Brainrot God", income:17500,    price:5000000 },
  { name:"Coco Elefanto",             emoji:"🐘", rarity:"Brainrot God", income:10000,    price:5000000 },
  { name:"Los Crocodillitos",         emoji:"🐊", rarity:"Brainrot God", income:55000,    price:12500000 },
  { name:"Trigoligre Frutonni",       emoji:"🐅", rarity:"Brainrot God", income:60000,    price:15000000 },
  { name:"Orcalero Orcala",           emoji:"🐋", rarity:"Brainrot God", income:100000,   price:15000000 },
  { name:"Odin Din Din Dun",          emoji:"⚡", rarity:"Brainrot God", income:75000,    price:15000000 },
  { name:"Girafa Celestre",           emoji:"🦒", rarity:"Brainrot God", income:20000,    price:7500000 },
  { name:"Gattatino Nyanino",         emoji:"🐱", rarity:"Brainrot God", income:35000,    price:7500000 },
  { name:"Matteo",                    emoji:"👦", rarity:"Brainrot God", income:50000,    price:10000000 },
  { name:"Tralalero Tralala",         emoji:"🦈", rarity:"Brainrot God", income:50000,    price:10000000 },
  { name:"Alessio",                   emoji:"🧒", rarity:"Brainrot God", income:85000,    price:17500000 },
  { name:"Tipi Topi Taco",            emoji:"🌮", rarity:"Brainrot God", income:75000,    price:20000000 },
  { name:"Tralalita Tralala",         emoji:"🦈", rarity:"Brainrot God", income:100000,   price:20000000 },
  { name:"Statutino Libertino",       emoji:"🗽", rarity:"Brainrot God", income:75000,    price:20000000 },
  { name:"Tukanno Bananno",           emoji:"🦜", rarity:"Brainrot God", income:100000,   price:22500000 },
  { name:"Trenostruzzo Turbo 3000",   emoji:"🚂", rarity:"Brainrot God", income:150000,   price:25000000 },
  { name:"Espresso Signora",          emoji:"☕", rarity:"Brainrot God", income:70000,    price:25000000 },
  { name:"Urubini Flamenguini",       emoji:"🦅", rarity:"Brainrot God", income:150000,   price:30000000 },
  { name:"Trippi Troppi Troppa Trippa",emoji:"🦩",rarity:"Brainrot God", income:175000,   price:30000000 },
  { name:"Ballerino Lololo",          emoji:"🕺", rarity:"Brainrot God", income:200000,   price:35000000 },
  { name:"Pakrahmatmamat",            emoji:"🕌", rarity:"Brainrot God", income:215000,   price:37500000 },
  { name:"Los Tungtungtungcitos",     emoji:"🥁", rarity:"Brainrot God", income:210000,   price:37500000 },
  { name:"Piccione Macchina",         emoji:"🐦", rarity:"Brainrot God", income:225000,   price:40000000 },
  { name:"Los Orcalitos",             emoji:"🐋", rarity:"Brainrot God", income:235000,   price:45000000 },
  { name:"Bombardini Tortinii",       emoji:"💣", rarity:"Brainrot God", income:225000,   price:50000000 },
  { name:"Pop Pop Sahur",             emoji:"🌙", rarity:"Brainrot God", income:295000,   price:60000000 },
  // SECRET
  { name:"La Vacca Saturno Saturnita",emoji:"🪐", rarity:"Secret",       income:250000,   price:50000000 },
  { name:"Blackhole Goat",            emoji:"🐐", rarity:"Secret",       income:400000,   price:75000000 },
  { name:"Agarrini la Palini",        emoji:"🌴", rarity:"Secret",       income:425000,   price:80000000 },
  { name:"Los Matteos",               emoji:"👦", rarity:"Secret",       income:300000,   price:100000000 },
  { name:"Sammyini Spyderini",        emoji:"🕷️", rarity:"Secret",       income:325000,   price:100000000 },
  { name:"Chimpanzini Spiderini",     emoji:"🦍", rarity:"Secret",       income:325000,   price:100000000 },
  { name:"Karkerkar Kurkur",          emoji:"🦀", rarity:"Secret",       income:2400000,  price:100000000 },
  { name:"Dul Dul Dul",               emoji:"🌙", rarity:"Secret",       income:375000,   price:150000000 },
  { name:"Los Tralaleritos",          emoji:"🦈", rarity:"Secret",       income:500000,   price:150000000 },
  { name:"Las Tralaleritas",          emoji:"🦈", rarity:"Secret",       income:650000,   price:150000000 },
  { name:"Las Vaquitas Saturnitas",   emoji:"🪐", rarity:"Secret",       income:750000,   price:160000000 },
  { name:"Graipuss Medussi",          emoji:"🪼", rarity:"Secret",       income:1000000,  price:250000000 },
  { name:"Los Hotspotsitos",          emoji:"📡", rarity:"Secret",       income:20000000, price:300000000 },
  { name:"Tortuginni Dragonfruitini", emoji:"🐢", rarity:"Secret",       income:350000,   price:500000000 },
  { name:"Nooo My Hotspot",           emoji:"📡", rarity:"Secret",       income:1500000,  price:500000000 },
  { name:"Pot Hotspot",               emoji:"🫖", rarity:"Secret",       income:2500000,  price:500000000 },
  { name:"Chicleteira Bicicleteira",  emoji:"🚲", rarity:"Secret",       income:3500000,  price:750000000 },
  { name:"Esok Sekolah",              emoji:"🎒", rarity:"Secret",       income:30000000, price:750000000 },
  { name:"La Grande Combinasion",     emoji:"✨", rarity:"Secret",       income:10000000, price:1000000000 },
  { name:"Los Combinasionas",         emoji:"✨", rarity:"Secret",       income:63700000, price:2000000000 },
  { name:"Nuclearo Dinossauro",       emoji:"☢️", rarity:"Secret",       income:15000000, price:2500000000 },
  { name:"La Supreme Combinasion",    emoji:"💫", rarity:"Secret",       income:40000000, price:7000000000 },
  { name:"Ketupat Kepat",             emoji:"🫙", rarity:"Secret",       income:35000000, price:5000000000 },
  { name:"Garama and Madundung",      emoji:"🌊", rarity:"Secret",       income:50000000, price:10000000000 },
  { name:"Dragon Cannelloni",         emoji:"🐉", rarity:"Secret",       income:100000000,price:100000000000 },
  { name:"Dragon Gingerini",          emoji:"🐲", rarity:"Secret",       income:150000000,price:150000000000 },
  // OG
  { name:"Skibidi Toilet",            emoji:"🚽", rarity:"OG",           income:450000000,price:450000000000 },
  { name:"Meowl",                     emoji:"🦉", rarity:"OG",           income:550000000,price:550000000000 },
  { name:"Strawberry Elephant",       emoji:"🐘", rarity:"OG",           income:750000000,price:750000000000 },
];

let modalTarget='yours', modalRarity='all';
const tradeItems={yours:[],theirs:[]};

function fmtV(n){
  if(n>=1e12)return(n/1e12).toFixed(2)+'T';
  if(n>=1e9)return(n/1e9).toFixed(2)+'B';
  if(n>=1e6)return(n/1e6).toFixed(2)+'M';
  if(n>=1e3)return(n/1e3).toFixed(1)+'K';
  return n.toFixed(0);
}
function rc(r){return r==='Brainrot God'?'r-God':'r-'+r;}
function rl(r){return r==='Brainrot God'?'God':r;}

function openModal(side){
  modalTarget=side; modalRarity='all';
  document.querySelectorAll('.rarity-filter').forEach(b=>b.classList.toggle('active',b.dataset.rarity==='all'));
  document.getElementById('modal-search').value='';
  document.getElementById('modal').classList.add('open');
  renderModalItems();
  document.getElementById('modal-search').focus();
}
function closeModal(){document.getElementById('modal').classList.remove('open');}
function closeModalOutside(e){if(e.target===document.getElementById('modal'))closeModal();}
function setModalRarity(r){
  modalRarity=r;
  document.querySelectorAll('.rarity-filter').forEach(b=>b.classList.toggle('active',b.dataset.rarity===r));
  renderModalItems();
}
function renderModalItems(){
  const q=document.getElementById('modal-search').value.toLowerCase();
  const grid=document.getElementById('items-grid');
  const filtered=brainrots.filter(b=>(modalRarity==='all'||b.rarity===modalRarity)&&b.name.toLowerCase().includes(q));
  if(!filtered.length){grid.innerHTML='<div class="no-results">No brainrots found 🔍</div>';return;}
  grid.innerHTML=filtered.map(b=>`
    <div class="item-card" onclick="addItem(${JSON.stringify(b.name)})">
      <span class="ic-emoji">${b.emoji}</span>
      <div class="ic-name">${b.name}</div>
      <span class="ic-rarity rarity-pill ${rc(b.rarity)}">${rl(b.rarity)}</span>
      <div class="ic-stats">💰 $${fmtV(b.price)}</div>
      <div class="ic-stats" style="color:var(--accent3)">📈 $${fmtV(b.income)}/s</div>
    </div>`).join('');
}

function addItem(name){
  const b=brainrots.find(x=>x.name===name);
  if(!b)return;
  tradeItems[modalTarget].push(b);
  renderSide(modalTarget); updateResult(); closeModal();
}
function removeItem(side,idx){tradeItems[side].splice(idx,1);renderSide(side);updateResult();}
function clearSide(side){tradeItems[side]=[];renderSide(side);updateResult();}

function renderSide(side){
  const total=tradeItems[side].reduce((s,b)=>s+b.price,0);
  document.getElementById(side+'-value').textContent='$'+fmtV(total);
  const c=document.getElementById(side+'-items');
  if(!tradeItems[side].length){
    c.innerHTML=`<div class="empty-items"><span class="empty-icon">🫙</span>Add brainrots to ${side==='yours'?'your':'their'} offer</div>`;
    return;
  }
  c.innerHTML=tradeItems[side].map((b,i)=>`
    <div class="trade-item">
      <span class="item-emoji">${b.emoji}</span>
      <div class="item-info">
        <div class="item-name">${b.name}</div>
        <span class="rarity-pill ${rc(b.rarity)}" style="font-size:9px;padding:1px 6px;">${rl(b.rarity)}</span>
      </div>
      <span class="item-value-tag">$${fmtV(b.price)}</span>
      <button class="remove-item" onclick="removeItem('${side}',${i})">✕</button>
    </div>`).join('');
}

function updateResult(){
  const yv=tradeItems.yours.reduce((s,b)=>s+b.price,0);
  const tv=tradeItems.theirs.reduce((s,b)=>s+b.price,0);
  const badge=document.getElementById('result-badge');
  const label=document.getElementById('diff-label');
  if(!tradeItems.yours.length&&!tradeItems.theirs.length){badge.className='result-badge neutral';badge.textContent='—';label.textContent='Add items';return;}
  if(yv===tv){badge.className='result-badge fair';badge.textContent='FAIR';label.textContent='Equal trade';return;}
  const ratio=tv===0?99:yv/tv;
  if(ratio>1.05){badge.className='result-badge win';badge.textContent='WIN';label.textContent=`+$${fmtV(yv-tv)} your favor`;}
  else if(ratio<0.95){badge.className='result-badge lose';badge.textContent='LOSE';label.textContent=`−$${fmtV(tv-yv)} against you`;}
  else{badge.className='result-badge fair';badge.textContent='FAIR';label.textContent=`≈$${fmtV(Math.abs(yv-tv))} diff`;}
}

let vtRarity='all';
document.querySelectorAll('.vt-filter').forEach(btn=>{
  btn.addEventListener('click',()=>{
    vtRarity=btn.dataset.rarity;
    document.querySelectorAll('.vt-filter').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active'); renderTable();
  });
});
document.getElementById('vt-search').addEventListener('input',renderTable);

function renderTable(){
  const q=document.getElementById('vt-search').value.toLowerCase();
  const filtered=brainrots
    .filter(b=>(vtRarity==='all'||b.rarity===vtRarity)&&b.name.toLowerCase().includes(q))
    .sort((a,b)=>b.price-a.price);
  const tbody=document.getElementById('value-table-body');
  if(!filtered.length){tbody.innerHTML='';document.getElementById('vt-no-results').style.display='block';return;}
  document.getElementById('vt-no-results').style.display='none';
  tbody.innerHTML=filtered.map(b=>`
    <tr>
      <td><div class="td-name"><span class="td-emoji">${b.emoji}</span><div class="name-text">${b.name}</div></div></td>
      <td><span class="rarity-pill ${rc(b.rarity)}">${rl(b.rarity)}</span></td>
      <td class="td-val">$${fmtV(b.price)}</td>
      <td class="td-income">$${fmtV(b.income)}/s</td>
    </tr>`).join('');
}

renderTable();
renderSide('yours');
renderSide('theirs');
</script>
</body>
</html>
