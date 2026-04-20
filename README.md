<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Networking Lab 3 — Enterprise Networks & Internet</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=DM+Sans:wght@400;500;600&family=Syne:wght@700;800&display=swap" rel="stylesheet">
<style>
  :root {
    /* Light Theme - Main colors */
    --bg: #f8fafc;
    --surface: #ffffff;
    --surface2: #f1f5f9;
    --border: #e2e8f0;
    
    /* Text */
    --text: #0f172a;
    --text-muted: #64748b;
    --text-dim: #475569;
    
    /* Section Colors */
    --vlan-color: #0284c7;    /* Sky Blue */
    --router-color: #6d28d9;  /* Purple */
    --internet-color: #059669;/* Emerald */
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--bg);
    color: var(--text);
    line-height: 1.7;
    overflow-x: hidden;
  }

  /* ── HERO ── */
  .hero {
    position: relative;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    padding: 4rem 2rem;
    overflow: hidden;
  }

  .hero-grid {
    position: absolute; inset: 0;
    background-image:
      linear-gradient(rgba(15,23,42,.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(15,23,42,.03) 1px, transparent 1px);
    background-size: 60px 60px;
    animation: gridPan 30s linear infinite;
  }

  @keyframes gridPan { to { transform: translate(60px, 60px); } }

  .hero-glow {
    position: absolute;
    width: 600px; height: 600px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(5,150,105,.08), transparent 70%);
    top: 50%; left: 50%;
    transform: translate(-50%, -50%);
    animation: pulse 4s ease-in-out infinite;
  }

  @keyframes pulse {
    0%, 100% { transform: translate(-50%,-50%) scale(1); opacity: .7; }
    50% { transform: translate(-50%,-50%) scale(1.15); opacity: 1; }
  }

  .hero-badge {
    background: rgba(5,150,105,.1);
    border: 1px solid rgba(5,150,105,.2);
    color: var(--internet-color);
    font-family: 'Space Mono', monospace;
    font-size: .75rem;
    padding: .4rem 1rem;
    border-radius: 999px;
    letter-spacing: .1em;
    margin-bottom: 1.5rem;
    position: relative;
    font-weight: 700;
  }

  .hero h1 {
    font-family: 'Syne', sans-serif;
    font-size: clamp(2.8rem, 7vw, 5.5rem);
    font-weight: 800;
    line-height: 1.05;
    letter-spacing: -.02em;
    position: relative;
    margin-bottom: 1rem;
  }

  .hero h1 .line1 { display: block; color: var(--text); }
  .hero h1 .line2 { display: block; color: var(--internet-color); }

  .hero-sub {
    font-size: 1.15rem;
    color: var(--text-dim);
    max-width: 600px;
    position: relative;
    margin-bottom: 3rem;
  }

  .hero-pills {
    display: flex; gap: 1rem; flex-wrap: wrap; justify-content: center;
    position: relative;
  }

  .pill {
    display: flex; align-items: center; gap: .5rem;
    padding: .6rem 1.2rem;
    border-radius: 999px;
    font-size: .85rem;
    font-weight: 600;
    border: 1px solid;
    background: var(--surface);
  }

  .pill.vlan { border-color: rgba(2,132,199,.3); color: var(--vlan-color); }
  .pill.rt   { border-color: rgba(109,40,217,.3); color: var(--router-color); }
  .pill.inet { border-color: rgba(5,150,105,.3); color: var(--internet-color); }

  .scroll-hint {
    position: absolute; bottom: 2.5rem; left: 50%; transform: translateX(-50%);
    color: var(--text-muted); font-family: 'Space Mono', monospace; font-size: .7rem;
    letter-spacing: .12em; display: flex; flex-direction: column; align-items: center; gap: .5rem;
    animation: fadeInUp 1s 1s both;
    font-weight: 700;
  }

  .scroll-hint .arrow { width: 2px; height: 40px; background: linear-gradient(var(--internet-color), transparent); }

  @keyframes fadeInUp {
    from { opacity: 0; transform: translateX(-50%) translateY(10px); }
    to   { opacity: 1; transform: translateX(-50%) translateY(0); }
  }

  /* ── MAIN LAYOUT ── */
  main { max-width: 1100px; margin: 0 auto; padding: 0 2rem 6rem; }

  /* ── SECTION HEADERS ── */
  .section-header {
    display: flex; align-items: flex-start; gap: 1.5rem;
    margin: 5rem 0 2.5rem;
    padding-bottom: 1.5rem;
    border-bottom: 1px solid var(--border);
  }

  .section-number {
    font-family: 'Space Mono', monospace;
    font-size: 3.5rem;
    font-weight: 700;
    line-height: 1;
    color: transparent;
    -webkit-text-stroke: 2px #cbd5e1;
    min-width: 80px;
  }

  .section-info { flex: 1; }
  .section-label {
    font-family: 'Space Mono', monospace;
    font-size: .7rem;
    letter-spacing: .15em;
    color: var(--text-muted);
    text-transform: uppercase;
    margin-bottom: .4rem;
    font-weight: 700;
  }

  .section-title {
    font-family: 'Syne', sans-serif;
    font-size: 2rem;
    font-weight: 700;
    line-height: 1.1;
  }

  .section-title .accent-vlan { color: var(--vlan-color); }
  .section-title .accent-router { color: var(--router-color); }
  .section-title .accent-inet { color: var(--internet-color); }

  .section-desc {
    color: var(--text-dim);
    margin-top: .5rem;
    font-size: .95rem;
  }

  /* ── OBJECTIVE BOX ── */
  .objective {
    background: var(--surface2);
    border-radius: 0 8px 8px 0;
    padding: 1rem 1.4rem;
    margin-bottom: 2rem;
    font-size: .95rem;
    color: var(--text-dim);
  }

  .obj-vlan { border-left: 4px solid var(--vlan-color); }
  .obj-vlan strong { color: var(--vlan-color); font-family: 'Space Mono', monospace; font-size: .85rem; letter-spacing: .1em; }
  
  .obj-rt { border-left: 4px solid var(--router-color); }
  .obj-rt strong { color: var(--router-color); font-family: 'Space Mono', monospace; font-size: .85rem; letter-spacing: .1em; }
  
  .obj-inet { border-left: 4px solid var(--internet-color); }
  .obj-inet strong { color: var(--internet-color); font-family: 'Space Mono', monospace; font-size: .85rem; letter-spacing: .1em; }

  /* ── STEPS ── */
  .steps { display: flex; flex-direction: column; gap: 1.2rem; margin-bottom: 2rem; }

  .step {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    overflow: hidden;
    transition: border-color .2s, box-shadow .2s;
    box-shadow: 0 2px 4px rgba(15,23,42,0.02);
  }

  .step-vlan:hover { border-color: var(--vlan-color); box-shadow: 0 4px 12px rgba(2,132,199,0.08); }
  .step-rt:hover   { border-color: var(--router-color); box-shadow: 0 4px 12px rgba(109,40,217,0.08); }
  .step-inet:hover { border-color: var(--internet-color); box-shadow: 0 4px 12px rgba(5,150,105,0.08); }

  .step-header {
    display: flex; align-items: center; gap: 1rem;
    padding: 1rem 1.4rem;
    cursor: pointer;
    user-select: none;
  }

  .step-num {
    width: 32px; height: 32px; border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-family: 'Space Mono', monospace;
    font-size: .85rem; font-weight: 700;
    flex-shrink: 0;
  }
  
  .step-vlan .step-num { background: rgba(2,132,199,.1); color: var(--vlan-color); }
  .step-rt .step-num   { background: rgba(109,40,217,.1); color: var(--router-color); }
  .step-inet .step-num { background: rgba(5,150,105,.1); color: var(--internet-color); }

  .step-title { font-weight: 600; flex: 1; color: var(--text); }
  .step-chevron { color: var(--text-muted); transition: transform .25s; font-size: 1.1rem; }
  .step.open .step-chevron { transform: rotate(90deg); }

  .step-body {
    max-height: 0; overflow: hidden;
    transition: max-height .4s ease, padding .3s;
    padding: 0 1.4rem;
  }

  .step.open .step-body {
    max-height: 1500px;
    padding: 0 1.4rem 1.2rem;
  }

  .step-body ul { padding-left: 1.4rem; color: var(--text-dim); }
  .step-body li { margin-bottom: .4rem; }
  .step-body code {
    font-family: 'Space Mono', monospace;
    font-size: .85rem;
    background: var(--surface2);
    color: var(--text);
    border: 1px solid var(--border);
    padding: .1rem .4rem;
    border-radius: 4px;
    font-weight: 600;
  }

  /* Terminal Style */
  .step-body .cmd-block {
    background: #0f172a;
    border-radius: 8px;
    padding: 1rem 1.2rem;
    font-family: 'Space Mono', monospace;
    font-size: .85rem;
    color: #38bdf8;
    margin: .8rem 0;
    position: relative;
    box-shadow: inset 0 2px 4px rgba(0,0,0,0.2);
    white-space: pre;
    overflow-x: auto;
  }

  /* ── DIAGRAMS ── */
  .diagram-wrap {
    margin: 2rem 0;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    overflow: hidden;
    box-shadow: 0 4px 20px rgba(15,23,42,0.03);
  }

  .diagram-label {
    font-family: 'Space Mono', monospace;
    font-size: .75rem;
    letter-spacing: .1em;
    color: var(--text-muted);
    text-transform: uppercase;
    padding: .8rem 1.2rem;
    border-bottom: 1px solid var(--border);
    display: flex; align-items: center; gap: .5rem;
    background: #f8fafc;
    font-weight: 700;
  }

  .dl-vlan::before { content: '●'; color: var(--vlan-color); }
  .dl-rt::before   { content: '●'; color: var(--router-color); }
  .dl-inet::before { content: '●'; color: var(--internet-color); }

  .diagram-wrap svg { display: block; width: 100%; max-width: 100%; background: transparent; }

  /* ── IP TABLE ── */
  .ip-table-wrap {
    overflow-x: auto;
    margin: 1.5rem 0;
    border-radius: 10px;
    border: 1px solid var(--border);
    background: var(--surface);
  }

  table { width: 100%; border-collapse: collapse; font-size: .9rem; }
  thead tr { background: var(--surface2); }
  th {
    padding: .85rem 1.1rem;
    text-align: left;
    font-family: 'Space Mono', monospace;
    font-size: .75rem;
    letter-spacing: .05em;
    color: var(--text-muted);
    text-transform: uppercase;
    border-bottom: 1px solid var(--border);
  }
  td {
    padding: .75rem 1.1rem;
    border-bottom: 1px solid var(--border);
    color: var(--text-dim);
  }
  tr:last-child td { border-bottom: none; }

  /* ── CALLOUTS ── */
  .callout {
    border-radius: 10px;
    padding: 1rem 1.2rem;
    margin: 1.5rem 0;
    display: flex; gap: 1rem; align-items: flex-start;
    font-size: .95rem;
  }

  .callout-icon { font-size: 1.3rem; flex-shrink: 0; margin-top: .1rem; }
  .callout.tip  { background: rgba(5,150,105,.08); border: 1px solid rgba(5,150,105,.2); color: #064e3b; }
  .callout.warn { background: rgba(217,119,6,.08); border: 1px solid rgba(217,119,6,.2); color: #78350f; }
  .callout.info { background: rgba(2,132,199,.08);  border: 1px solid rgba(2,132,199,.2); color: #0c4a6e; }

  /* ── SUMMARY ── */
  .summary-section { margin-top: 5rem; }
  .summary-section h2 { font-family: 'Syne', sans-serif; font-size: 1.8rem; margin-bottom: 1.5rem; color: var(--text); }
  
  .summary-grid {
    display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 1.5rem;
  }

  .summary-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 1.4rem;
    transition: transform .2s, box-shadow .2s;
    box-shadow: 0 4px 6px rgba(15,23,42,0.02);
  }

  .summary-card:hover { transform: translateY(-5px); box-shadow: 0 10px 20px rgba(15,23,42,0.05); }
  .summary-card.vlan { border-top: 4px solid var(--vlan-color); }
  .summary-card.rt   { border-top: 4px solid var(--router-color); }
  .summary-card.inet { border-top: 4px solid var(--internet-color); }

  .summary-card .sc-num { font-family: 'Space Mono', monospace; font-size: .75rem; letter-spacing: .12em; color: var(--text-muted); margin-bottom: .5rem; font-weight: 700;}
  .summary-card .sc-title { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 1.1rem; margin-bottom: .6rem; }
  .summary-card .sc-key { font-size: .9rem; color: var(--text-dim); line-height: 1.5; }

  /* ── SECTION WRAPPERS ── */
  .section-vlan { background: #f5fcff; }
  .section-rt   { background: #fdfafb; }
  .section-inet { background: #f5fff9; }

  .section-wrap {
    border-radius: 20px;
    padding: 0 2rem 2rem;
    margin-bottom: 3rem;
    border: 1px solid var(--border);
    box-shadow: 0 10px 30px rgba(15,23,42,0.02);
  }

  /* ── FOOTER ── */
  footer {
    text-align: center; padding: 3rem 2rem; border-top: 1px solid var(--border);
    color: var(--text-muted); font-family: 'Space Mono', monospace; font-size: .75rem;
    letter-spacing: .08em; font-weight: 700;
  }

  @media (max-width: 600px) {
    .section-number { font-size: 2.5rem; min-width: 55px; }
    .section-title { font-size: 1.5rem; }
    .section-wrap { padding: 0 1rem 1rem; }
  }
</style>
</head>
<body>

<!-- ═══ HERO ═══ -->
<section class="hero">
  <div class="hero-grid"></div>
  <div class="hero-glow"></div>
  <div class="hero-badge">CISCO PACKET TRACER — LAB 3</div>
  <h1>
    <span class="line1">Enterprise</span>
    <span class="line2">Networking</span>
  </h1>
  <h2>Dr. Mohamed Abdou SOUIDI</h2>
  <p class="hero-sub">From LAN to WAN. Create isolated VLANs with multiple PCs, enable routing between them, and securely connect the entire company to the Internet.</p>
  <div class="hero-pills">
    <span class="pill vlan">3.1 Create 3 VLANs</span>
    <span class="pill rt">3.2 Inter-VLAN Routing</span>
    <span class="pill inet">3.3 NAT & Internet</span>
  </div>
  <div class="scroll-hint">
    <span>BEGIN LAB 3</span>
    <div class="arrow"></div>
  </div>
</section>

<main>

<!-- ═══════════════════════════════════════════
     3.1  THREE VLANs
════════════════════════════════════════════════ -->
<div class="section-wrap section-vlan">
  <div class="section-header">
    <div class="section-number">3.1</div>
    <div class="section-info">
      <div class="section-label">Step 1</div>
      <div class="section-title">Create 3 <span class="accent-vlan">Virtual LANs</span></div>
      <div class="section-desc">Logically divide a single physical switch into three completely isolated networks, each containing 3 PCs.</div>
    </div>
  </div>

  <div class="objective obj-vlan"><strong>OBJECTIVE —</strong> Configure VLAN 10 (HR), VLAN 20 (IT), and VLAN 30 (Sales) on the same switch with 3 PCs each. Verify that PCs can ping within their own VLAN but cannot cross into others.</div>

  <div class="diagram-wrap">
    <div class="diagram-label dl-vlan">Topology — 3 Isolated VLANs (9 PCs Total)</div>
    <svg viewBox="0 0 780 240" xmlns="http://www.w3.org/2000/svg">
      
      <!-- Switch -->
      <rect x="340" y="160" width="100" height="60" rx="8" fill="#ffffff" stroke="#0f172a" stroke-width="2"/>
      <text x="390" y="185" text-anchor="middle" fill="#0f172a" font-family="Space Mono,monospace" font-size="12" font-weight="700">SWITCH</text>
      <text x="390" y="200" text-anchor="middle" fill="#64748b" font-family="Space Mono,monospace" font-size="9">2960</text>

      <!-- VLAN 10 (Blue) -->
      <rect x="15" y="30" width="230" height="80" rx="8" fill="rgba(2,132,199,.05)" stroke="#0284c7" stroke-width="1.5" stroke-dasharray="4,4"/>
      <text x="130" y="45" text-anchor="middle" fill="#0284c7" font-family="Space Mono,monospace" font-size="10" font-weight="700">VLAN 10 (HR)</text>
      
      <rect x="25" y="55" width="60" height="40" rx="4" fill="#ffffff" stroke="#0284c7" stroke-width="1.5"/>
      <text x="55" y="73" text-anchor="middle" fill="#0284c7" font-weight="700" font-size="10">PC0</text>
      <text x="55" y="85" text-anchor="middle" fill="#64748b" font-size="8" font-family="Space Mono,monospace">.10.10</text>
      
      <rect x="100" y="55" width="60" height="40" rx="4" fill="#ffffff" stroke="#0284c7" stroke-width="1.5"/>
      <text x="130" y="73" text-anchor="middle" fill="#0284c7" font-weight="700" font-size="10">PC1</text>
      <text x="130" y="85" text-anchor="middle" fill="#64748b" font-size="8" font-family="Space Mono,monospace">.10.11</text>

      <rect x="175" y="55" width="60" height="40" rx="4" fill="#ffffff" stroke="#0284c7" stroke-width="1.5"/>
      <text x="205" y="73" text-anchor="middle" fill="#0284c7" font-weight="700" font-size="10">PC2</text>
      <text x="205" y="85" text-anchor="middle" fill="#64748b" font-size="8" font-family="Space Mono,monospace">.10.12</text>

      <line x1="130" y1="110" x2="340" y2="180" stroke="#0284c7" stroke-width="2"/>
      <text x="210" y="155" fill="#0284c7" font-family="Space Mono,monospace" font-size="8">Fa0/1-3</text>

      <!-- VLAN 20 (Orange) -->
      <rect x="275" y="30" width="230" height="80" rx="8" fill="rgba(217,119,6,.05)" stroke="#d97706" stroke-width="1.5" stroke-dasharray="4,4"/>
      <text x="390" y="45" text-anchor="middle" fill="#d97706" font-family="Space Mono,monospace" font-size="10" font-weight="700">VLAN 20 (IT)</text>
      
      <rect x="285" y="55" width="60" height="40" rx="4" fill="#ffffff" stroke="#d97706" stroke-width="1.5"/>
      <text x="315" y="73" text-anchor="middle" fill="#d97706" font-weight="700" font-size="10">PC3</text>
      <text x="315" y="85" text-anchor="middle" fill="#64748b" font-size="8" font-family="Space Mono,monospace">.20.10</text>

      <rect x="360" y="55" width="60" height="40" rx="4" fill="#ffffff" stroke="#d97706" stroke-width="1.5"/>
      <text x="390" y="73" text-anchor="middle" fill="#d97706" font-weight="700" font-size="10">PC4</text>
      <text x="390" y="85" text-anchor="middle" fill="#64748b" font-size="8" font-family="Space Mono,monospace">.20.11</text>

      <rect x="435" y="55" width="60" height="40" rx="4" fill="#ffffff" stroke="#d97706" stroke-width="1.5"/>
      <text x="465" y="73" text-anchor="middle" fill="#d97706" font-weight="700" font-size="10">PC5</text>
      <text x="465" y="85" text-anchor="middle" fill="#64748b" font-size="8" font-family="Space Mono,monospace">.20.12</text>

      <line x1="390" y1="110" x2="390" y2="160" stroke="#d97706" stroke-width="2"/>
      <text x="400" y="145" fill="#d97706" font-family="Space Mono,monospace" font-size="8">Fa0/11-13</text>

      <!-- VLAN 30 (Pink) -->
      <rect x="535" y="30" width="230" height="80" rx="8" fill="rgba(219,39,119,.05)" stroke="#db2777" stroke-width="1.5" stroke-dasharray="4,4"/>
      <text x="650" y="45" text-anchor="middle" fill="#db2777" font-family="Space Mono,monospace" font-size="10" font-weight="700">VLAN 30 (SALES)</text>
      
      <rect x="545" y="55" width="60" height="40" rx="4" fill="#ffffff" stroke="#db2777" stroke-width="1.5"/>
      <text x="575" y="73" text-anchor="middle" fill="#db2777" font-weight="700" font-size="10">PC6</text>
      <text x="575" y="85" text-anchor="middle" fill="#64748b" font-size="8" font-family="Space Mono,monospace">.30.10</text>

      <rect x="620" y="55" width="60" height="40" rx="4" fill="#ffffff" stroke="#db2777" stroke-width="1.5"/>
      <text x="650" y="73" text-anchor="middle" fill="#db2777" font-weight="700" font-size="10">PC7</text>
      <text x="650" y="85" text-anchor="middle" fill="#64748b" font-size="8" font-family="Space Mono,monospace">.30.11</text>

      <rect x="695" y="55" width="60" height="40" rx="4" fill="#ffffff" stroke="#db2777" stroke-width="1.5"/>
      <text x="725" y="73" text-anchor="middle" fill="#db2777" font-weight="700" font-size="10">PC8</text>
      <text x="725" y="85" text-anchor="middle" fill="#64748b" font-size="8" font-family="Space Mono,monospace">.30.12</text>

      <line x1="650" y1="110" x2="440" y2="180" stroke="#db2777" stroke-width="2"/>
      <text x="540" y="155" fill="#db2777" font-family="Space Mono,monospace" font-size="8">Fa0/21-23</text>

    </svg>
  </div>

  <div class="steps">
    <div class="step step-vlan open" onclick="toggle(this)">
      <div class="step-header">
        <div class="step-num">1</div>
        <div class="step-title">Build the Topology</div>
        <div class="step-chevron">▶</div>
      </div>
      <div class="step-body">
        <ul>
          <li>Open Cisco Packet Tracer and add <strong>1 Switch (2960)</strong> and <strong>9 PCs</strong>.</li>
          <li>Connect <strong>PC0, PC1, PC2</strong> to switch ports <code>Fa0/1</code>, <code>Fa0/2</code>, and <code>Fa0/3</code> (VLAN 10).</li>
          <li>Connect <strong>PC3, PC4, PC5</strong> to switch ports <code>Fa0/11</code>, <code>Fa0/12</code>, and <code>Fa0/13</code> (VLAN 20).</li>
          <li>Connect <strong>PC6, PC7, PC8</strong> to switch ports <code>Fa0/21</code>, <code>Fa0/22</code>, and <code>Fa0/23</code> (VLAN 30).</li>
        </ul>
      </div>
    </div>

    <div class="step step-vlan open" onclick="toggle(this)">
      <div class="step-header">
        <div class="step-num">2</div>
        <div class="step-title">Assign Static IP Addresses</div>
        <div class="step-chevron">▶</div>
      </div>
      <div class="step-body">
        <p style="color:var(--text-dim);">Go to each PC's <code>Desktop → IP Configuration</code> and assign the following:</p>
        <div class="ip-table-wrap">
          <table>
            <thead><tr><th>Devices</th><th>VLAN Name</th><th>IP Range</th><th>Subnet Mask</th></tr></thead>
            <tbody>
              <tr><td>PC0 - PC2</td><td>VLAN 10 (HR)</td><td><code>192.168.10.10</code> to <code>.12</code></td><td><code>255.255.255.0</code></td></tr>
              <tr><td>PC3 - PC5</td><td>VLAN 20 (IT)</td><td><code>192.168.20.10</code> to <code>.12</code></td><td><code>255.255.255.0</code></td></tr>
              <tr><td>PC6 - PC8</td><td>VLAN 30 (Sales)</td><td><code>192.168.30.10</code> to <code>.12</code></td><td><code>255.255.255.0</code></td></tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <div class="step step-vlan open" onclick="toggle(this)">
      <div class="step-header">
        <div class="step-num">3</div>
        <div class="step-title">Create and Assign VLANs using the Range Command</div>
        <div class="step-chevron">▶</div>
      </div>
      <div class="step-body">
        <p style="color:var(--text-dim);">Click the <strong>Switch</strong> → <code>CLI</code> tab. Use the <code>interface range</code> command to configure multiple ports at once:</p>
        <div class="cmd-block">Switch> enable
Switch# configure terminal

! Create the 3 VLANs
Switch(config)# vlan 10
Switch(config-vlan)# name HR
Switch(config-vlan)# vlan 20
Switch(config-vlan)# name IT
Switch(config-vlan)# vlan 30
Switch(config-vlan)# name SALES
Switch(config-vlan)# exit

! Assign Ports Fa0/1 to Fa0/3 to VLAN 10
Switch(config)# interface range fa0/1-3
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10

! Assign Ports Fa0/11 to Fa0/13 to VLAN 20
Switch(config)# interface range fa0/11-13
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20

! Assign Ports Fa0/21 to Fa0/23 to VLAN 30
Switch(config)# interface range fa0/21-23
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 30
Switch(config-if-range)# end</div>
        <div class="callout warn">
          <span class="callout-icon">⚠️</span>
          <span><strong>Test Isolation:</strong> PC0 can successfully ping PC1 (192.168.10.11), but if you try to ping PC3 (192.168.20.10) or PC6 (192.168.30.10), it will fail. Without a router, VLANs cannot talk to each other.</span>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════════════
     3.2  INTER-VLAN ROUTING
════════════════════════════════════════════════ -->
<div class="section-wrap section-rt">
  <div class="section-header">
    <div class="section-number">3.2</div>
    <div class="section-info">
      <div class="section-label">Step 2</div>
      <div class="section-title">Connection between them <span class="accent-router">(Inter-VLAN)</span></div>
      <div class="section-desc">Use a Router to allow traffic to flow between the isolated VLANs (Router-on-a-Stick).</div>
    </div>
  </div>

  <div class="objective obj-rt"><strong>OBJECTIVE —</strong> Connect a Router using a "Trunk" link. Create virtual sub-interfaces to act as the Default Gateway for each of the 3 VLANs, enabling communication.</div>

  <div class="diagram-wrap">
    <div class="diagram-label dl-rt">Topology — Router on a Stick</div>
    <svg viewBox="0 0 780 200" xmlns="http://www.w3.org/2000/svg">
      
      <!-- Router -->
      <ellipse cx="390" cy="40" rx="50" ry="30" fill="#ffffff" stroke="#6d28d9" stroke-width="2"/>
      <text x="390" y="35" text-anchor="middle" fill="#6d28d9" font-family="Space Mono,monospace" font-size="11" font-weight="700">ROUTER</text>
      <text x="390" y="50" text-anchor="middle" fill="#64748b" font-family="Space Mono,monospace" font-size="8">G0/0</text>

      <!-- Sub interfaces details -->
      <rect x="180" y="80" width="90" height="35" rx="4" fill="rgba(2,132,199,.1)" stroke="#0284c7" stroke-width="1"/>
      <text x="225" y="93" text-anchor="middle" fill="#0284c7" font-family="Space Mono,monospace" font-size="8" font-weight="700">Sub: G0/0.10</text>
      <text x="225" y="105" text-anchor="middle" fill="#64748b" font-family="Space Mono,monospace" font-size="7">192.168.10.1</text>
      <line x1="390" y1="90" x2="270" y2="90" stroke="#0284c7" stroke-width="1" stroke-dasharray="2,2"/>

      <rect x="290" y="80" width="90" height="35" rx="4" fill="rgba(217,119,6,.1)" stroke="#d97706" stroke-width="1"/>
      <text x="335" y="93" text-anchor="middle" fill="#d97706" font-family="Space Mono,monospace" font-size="8" font-weight="700">Sub: G0/0.20</text>
      <text x="335" y="105" text-anchor="middle" fill="#64748b" font-family="Space Mono,monospace" font-size="7">192.168.20.1</text>
      <line x1="390" y1="90" x2="380" y2="90" stroke="#d97706" stroke-width="1" stroke-dasharray="2,2"/>

      <rect x="500" y="80" width="90" height="35" rx="4" fill="rgba(219,39,119,.1)" stroke="#db2777" stroke-width="1"/>
      <text x="545" y="93" text-anchor="middle" fill="#db2777" font-family="Space Mono,monospace" font-size="8" font-weight="700">Sub: G0/0.30</text>
      <text x="545" y="105" text-anchor="middle" fill="#64748b" font-family="Space Mono,monospace" font-size="7">192.168.30.1</text>
      <line x1="390" y1="90" x2="500" y2="90" stroke="#db2777" stroke-width="1" stroke-dasharray="2,2"/>

      <text x="405" y="115" fill="#6d28d9" font-family="Space Mono,monospace" font-size="9" font-weight="700">TRUNK (802.1Q)</text>
      <path d="M 390 70 L 390 140" stroke="#6d28d9" stroke-width="5"/>

      <!-- Switch -->
      <rect x="340" y="140" width="100" height="40" rx="6" fill="#f1f5f9" stroke="#0f172a" stroke-width="2"/>
      <text x="390" y="165" text-anchor="middle" fill="#0f172a" font-family="Space Mono,monospace" font-size="11" font-weight="700">SWITCH</text>

    </svg>
  </div>

  <div class="steps">
    <div class="step step-rt open" onclick="toggle(this)">
      <div class="step-header"><div class="step-num">1</div><div class="step-title">Connect the Router and Configure Trunking</div><div class="step-chevron">▶</div></div>
      <div class="step-body">
        <ul>
          <li>Add a <strong>Router (e.g., 2911)</strong> to your workspace.</li>
          <li>Connect Router <code>G0/0</code> to Switch <code>G0/4</code> (or any open port).</li>
          <li>On the Switch, configure <code>G0/4</code> as a Trunk port so it can carry all 3 VLANs:</li>
        </ul>
        <div class="cmd-block">Switch> enable
Switch# configure terminal
Switch(config)# interface g0/4
Switch(config-if)# switchport mode trunk
Switch(config-if)# end</div>
      </div>
    </div>

    <div class="step step-rt open" onclick="toggle(this)">
      <div class="step-header"><div class="step-num">2</div><div class="step-title">Configure Router Sub-Interfaces</div><div class="step-chevron">▶</div></div>
      <div class="step-body">
        <p style="color:var(--text-dim);">Go to the Router's CLI. Turn on the physical interface, then create the 3 virtual sub-interfaces.</p>
        <div class="cmd-block">Router> enable
Router# configure terminal
Router(config)# interface g0/0
Router(config-if)# no shutdown

! Gateway for VLAN 10
Router(config-if)# interface g0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0

! Gateway for VLAN 20
Router(config-subif)# interface g0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0

! Gateway for VLAN 30
Router(config-subif)# interface g0/0.30
Router(config-subif)# encapsulation dot1Q 30
Router(config-subif)# ip address 192.168.30.1 255.255.255.0
Router(config-subif)# end</div>
      </div>
    </div>

    <div class="step step-rt open" onclick="toggle(this)">
      <div class="step-header"><div class="step-num">3</div><div class="step-title">Update PCs and Test Routing</div><div class="step-chevron">▶</div></div>
      <div class="step-body">
        <ul>
          <li>Go back to your 9 PCs and update their <strong>Default Gateway</strong> in the IP Configuration:
            <ul>
              <li>PC0 to PC2 (VLAN 10) Gateway: <code>192.168.10.1</code></li>
              <li>PC3 to PC5 (VLAN 20) Gateway: <code>192.168.20.1</code></li>
              <li>PC6 to PC8 (VLAN 30) Gateway: <code>192.168.30.1</code></li>
            </ul>
          </li>
          <li>Open PC0's Command Prompt and ping a PC in the Sales department (PC6): <code>ping 192.168.30.10</code>.</li>
        </ul>
        <div class="callout tip">
          <span class="callout-icon">🎯</span>
          <span><strong>Success!</strong> The ping might fail the first time (due to ARP), but will then succeed. The traffic goes from PC0 -> Switch -> Router -> Switch -> PC6.</span>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════════════
     3.3  INTERNET & NAT
════════════════════════════════════════════════ -->
<div class="section-wrap section-inet">
  <div class="section-header">
    <div class="section-number">3.3</div>
    <div class="section-info">
      <div class="section-label">Step 3</div>
      <div class="section-title">Connect everything to the <span class="accent-inet">Internet</span></div>
      <div class="section-desc">Connect the internal network to the outside world using Network Address Translation and a Default Route.</div>
    </div>
  </div>

  <div class="objective obj-inet"><strong>OBJECTIVE —</strong> Add a server to simulate the Internet. Configure NAT Overload (PAT) on the router so all 9 PCs across the 3 private VLANs can share a single public IP to browse the web.</div>

  <div class="diagram-wrap">
    <div class="diagram-label dl-inet">Topology — NAT & Default Routing</div>
    <svg viewBox="0 0 780 200" xmlns="http://www.w3.org/2000/svg">
      
      <!-- Internal Network representation -->
      <rect x="20" y="80" width="120" height="60" rx="8" fill="rgba(15,23,42,.03)" stroke="#64748b" stroke-width="1.5" stroke-dasharray="4,4"/>
      <text x="80" y="105" text-anchor="middle" fill="#475569" font-family="Space Mono,monospace" font-size="10" font-weight="700">Internal LANs</text>
      <text x="80" y="120" text-anchor="middle" fill="#64748b" font-family="Space Mono,monospace" font-size="8">192.168.x.x</text>

      <line x1="140" y1="110" x2="220" y2="110" stroke="#cbd5e1" stroke-width="2"/>

      <!-- Router -->
      <ellipse cx="270" cy="110" rx="50" ry="35" fill="#ffffff" stroke="#6d28d9" stroke-width="2"/>
      <text x="270" y="105" text-anchor="middle" fill="#6d28d9" font-family="Space Mono,monospace" font-size="11" font-weight="700">ROUTER</text>
      
      <text x="220" y="95" text-anchor="middle" fill="#059669" font-family="Space Mono,monospace" font-size="8" font-weight="700">NAT INSIDE</text>
      <text x="325" y="95" text-anchor="middle" fill="#059669" font-family="Space Mono,monospace" font-size="8" font-weight="700">NAT OUTSIDE</text>

      <line x1="320" y1="110" x2="480" y2="110" stroke="#059669" stroke-width="3"/>
      <text x="400" y="100" text-anchor="middle" fill="#059669" font-family="Space Mono,monospace" font-size="9" font-weight="700">WAN LINK (Public IP)</text>
      <text x="400" y="125" text-anchor="middle" fill="#475569" font-family="Space Mono,monospace" font-size="8">203.0.113.1</text>

      <!-- Internet Cloud/Server -->
      <path d="M 500 110 C 500 70, 550 60, 580 80 C 620 60, 680 80, 660 120 C 690 140, 640 170, 590 150 C 530 170, 500 150, 500 110" fill="rgba(5,150,105,.05)" stroke="#059669" stroke-width="1.5" stroke-dasharray="3,3"/>
      <text x="590" y="95" text-anchor="middle" fill="#059669" font-family="Space Mono,monospace" font-size="12" font-weight="700">INTERNET</text>

      <rect x="650" y="100" width="80" height="40" rx="4" fill="#ffffff" stroke="#059669" stroke-width="2"/>
      <text x="690" y="118" text-anchor="middle" fill="#059669" font-family="Space Mono,monospace" font-size="10" font-weight="700">8.8.8.8</text>
      <text x="690" y="130" text-anchor="middle" fill="#64748b" font-family="Space Mono,monospace" font-size="7">Web Server</text>

      <line x1="620" y1="120" x2="650" y2="120" stroke="#059669" stroke-width="2"/>

      <!-- NAT Table illustration -->
      <rect x="200" y="15" width="230" height="50" rx="4" fill="#ffffff" stroke="#cbd5e1" stroke-width="1"/>
      <text x="315" y="30" text-anchor="middle" fill="#475569" font-family="Space Mono,monospace" font-size="9" font-weight="700">NAT TRANSLATION</text>
      <text x="315" y="45" text-anchor="middle" fill="#64748b" font-family="Space Mono,monospace" font-size="8">192.168.10.10:4321  ==>  203.0.113.1:4321</text>
      <text x="315" y="55" text-anchor="middle" fill="#64748b" font-family="Space Mono,monospace" font-size="8">192.168.30.12:1234  ==>  203.0.113.1:1234</text>
    </svg>
  </div>

  <div class="steps">
    <div class="step step-inet open" onclick="toggle(this)">
      <div class="step-header"><div class="step-num">1</div><div class="step-title">Simulate the Internet Connection</div><div class="step-chevron">▶</div></div>
      <div class="step-body">
        <ul>
          <li>Add a <strong>Server</strong> to the workspace. Rename it "Internet Server".</li>
          <li>Connect the Server's FastEthernet port to your Router's <code>G0/1</code> port using a <strong>Crossover</strong> cable.</li>
          <li>Configure the Server's IP address (Desktop -> IP Configuration):
            <ul>
              <li><strong>IP:</strong> <code>8.8.8.8</code></li>
              <li><strong>Mask:</strong> <code>255.0.0.0</code></li>
              <li><strong>Gateway:</strong> <code>8.8.8.1</code> (The Router's future public IP).</li>
            </ul>
          </li>
        </ul>
      </div>
    </div>

    <div class="step step-inet open" onclick="toggle(this)">
      <div class="step-header"><div class="step-num">2</div><div class="step-title">Configure the WAN Interface & Default Route</div><div class="step-chevron">▶</div></div>
      <div class="step-body">
        <p style="color:var(--text-dim);">On the Router, assign the "Public" IP and tell the router to send all unknown traffic to the Internet (Default Route).</p>
        <div class="cmd-block">Router> enable
Router# configure terminal
Router(config)# interface g0/1
Router(config-if)# ip address 8.8.8.1 255.0.0.0
Router(config-if)# no shutdown
Router(config-if)# exit

! Add a Default Static Route (Send everything out G0/1)
Router(config)# ip route 0.0.0.0 0.0.0.0 g0/1</div>
      </div>
    </div>

    <div class="step step-inet open" onclick="toggle(this)">
      <div class="step-header"><div class="step-num">3</div><div class="step-title">Configure NAT Overload (PAT)</div><div class="step-chevron">▶</div></div>
      <div class="step-body">
        <p style="color:var(--text-dim);">Private IPs (192.168.x.x) are not allowed on the Internet. We use NAT to translate them into the Router's single public IP.</p>
        <div class="cmd-block">! 1. Define who is allowed to be translated (Access-List)
Router(config)# access-list 1 permit 192.168.0.0 0.0.255.255

! 2. Link the Access-List to the WAN interface with OVERLOAD
Router(config)# ip nat inside source list 1 interface g0/1 overload

! 3. Define the OUTSIDE interface
Router(config)# interface g0/1
Router(config-if)# ip nat outside
Router(config-if)# exit

! 4. Define the INSIDE interfaces (You must do this for all 3 sub-interfaces!)
Router(config)# interface g0/0.10
Router(config-subif)# ip nat inside
Router(config-subif)# interface g0/0.20
Router(config-subif)# ip nat inside
Router(config-subif)# interface g0/0.30
Router(config-subif)# ip nat inside
Router(config-subif)# end</div>
      </div>
    </div>

    <div class="step step-inet open" onclick="toggle(this)">
      <div class="step-header"><div class="step-num">4</div><div class="step-title">Test the Internet Connection</div><div class="step-chevron">▶</div></div>
      <div class="step-body">
        <ul>
          <li>Open <strong>PC4</strong> (VLAN 20) Command Prompt.</li>
          <li>Type: <code>ping 8.8.8.8</code>.</li>
          <li>Repeat from <strong>PC7</strong> (VLAN 30).</li>
        </ul>
        <div class="callout tip">
          <span class="callout-icon">🌐</span>
          <span><strong>Success!</strong> All 9 PCs across the entire company can now reach the simulated Internet. In Packet Tracer Simulation Mode, you can click on the packet at the Router and look at the "Inbound" vs "Outbound" details to see the NAT translation modifying the IP address!</span>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════════════
     SUMMARY
════════════════════════════════════════════════ -->
<div class="summary-section">
  <h2>📋 Lab 3 Summary Table</h2>
  <div class="summary-grid">
    <div class="summary-card vlan">
      <div class="sc-num">LAB 3.1</div>
      <div class="sc-title">VLAN Segmentation</div>
      <div class="sc-key">VLANs divide a switch into isolated broadcast domains. Without routing, PCs in VLAN 10 <strong>cannot</strong> reach PCs in VLAN 20 or 30.</div>
    </div>
    <div class="summary-card rt">
      <div class="sc-num">LAB 3.2</div>
      <div class="sc-title">Router-on-a-Stick</div>
      <div class="sc-key">Uses an <strong>802.1Q Trunk</strong>. The router uses virtual sub-interfaces (G0/0.10, .20, .30) as default gateways to route traffic between the VLANs.</div>
    </div>
    <div class="summary-card inet">
      <div class="sc-num">LAB 3.3</div>
      <div class="sc-title">NAT & Default Route</div>
      <div class="sc-key"><strong>NAT Overload (PAT)</strong> translates internal private IPs to a single public IP. The <strong>Default Route</strong> tells the router where to send Internet traffic.</div>
    </div>
  </div>
</div>

</main>

<footer>
  Networking Lab 3 — Cisco Packet Tracer &nbsp;·&nbsp; Enterprise Architecture &nbsp;·&nbsp; Great job!
</footer>

<script>
  function toggle(el) {
    el.classList.toggle('open');
  }
</script>
</body>
</html>
