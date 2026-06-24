<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Resume Builder</title>
<style>
  :root {
    --primary: #8b5cf6;
    --primary-dark: #6d28d9;
    --secondary: #22d3ee;
    --accent: #ec4899;
    --success: #34d399;
    --danger: #fb7185;
    --bg: #090b16;
    --bg-alt: #111827;
    --card: rgba(15, 23, 42, 0.94);
    --card-soft: rgba(30, 41, 59, 0.95);
    --border: rgba(148, 163, 184, 0.28);
    --text: #f8fafc;
    --text-light: #94a3b8;
    --radius: 18px;
    --shadow: 0 18px 60px rgba(15, 23, 42, 0.45);
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }
  html, body { min-height: 100%; }
  body {
    font-family: 'Segoe UI', Roboto, Arial, sans-serif;
    background: radial-gradient(circle at top left, rgba(139,92,246,0.18), transparent 25%),
                radial-gradient(circle at right center, rgba(2,132,199,0.16), transparent 18%),
                linear-gradient(180deg, #090b16 0%, #111827 100%);
    color: var(--text);
    font-size: 16px;
    line-height: 1.5;
    overflow-x: hidden;
  }
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(135deg, rgba(255,255,255,0.02) 0 1px, transparent 1px 20px);
    pointer-events: none;
    opacity: 0.18;
  }

  #app { display: flex; flex-direction: column; min-height: 100vh; position: relative; }

  header {
    background: linear-gradient(135deg, rgba(139,92,246,0.96), rgba(37,99,235,0.92));
    color: #fff;
    padding: 18px 24px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    box-shadow: 0 20px 40px rgba(15, 23, 42, 0.35);
    position: sticky;
    top: 0;
    z-index: 110;
    border-bottom: 1px solid rgba(255,255,255,0.08);
  }
  header::after {
    content: '';
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    height: 4px;
    background: linear-gradient(90deg, rgba(236,72,153,0.9), rgba(37,99,235,0.9), rgba(56,189,248,0.9));
  }
  header h1 {
    font-size: 1.45rem;
    font-weight: 800;
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }
  header h1 span {
    display: inline-block;
    margin-left: 6px;
    color: rgba(255,255,255,0.8);
    font-weight: 500;
    letter-spacing: 0.08em;
  }
  .header-actions { display: flex; gap: 10px; flex-wrap: wrap; align-items: center; }

  .btn {
    padding: 9px 18px;
    border: none;
    border-radius: 999px;
    cursor: pointer;
    font-size: 0.88rem;
    font-weight: 700;
    transition: transform 0.18s ease, box-shadow 0.18s ease, opacity 0.18s ease;
    display: inline-flex;
    align-items: center;
    gap: 8px;
    box-shadow: 0 12px 24px rgba(15, 23, 42, 0.18);
  }
  .btn:hover { transform: translateY(-1px); }
  .btn:active { transform: translateY(0); }
  .btn-white { background: rgba(255,255,255,0.95); color: #3b82f6; }
  .btn-white:hover { opacity: 0.95; }
  .btn-outline { background: transparent; color: #fff; border: 2px solid rgba(255,255,255,0.35); }
  .btn-outline:hover { background: rgba(255,255,255,0.08); }
  .btn-primary { background: linear-gradient(135deg, #8b5cf6, #2563eb); color: #fff; }
  .btn-success { background: linear-gradient(135deg, #22c55e, #10b981); color: #fff; }
  .btn-danger { background: transparent; color: #fb7185; border: 1.5px solid rgba(251,113,133,0.95); }
  .btn-danger:hover { background: rgba(251,113,133,0.1); }
  .btn-sm { padding: 7px 12px; font-size: 0.82rem; }
  .btn-add { background: rgba(99,102,241,0.12); color: #e0e7ff; border: 1.5px dashed rgba(139,92,246,0.75); width: 100%; margin-top: 10px; justify-content: center; padding: 11px; }
  .btn-add:hover { background: rgba(139,92,246,0.18); }

  main { display: flex; flex: 1; overflow: hidden; gap: 0; }

  #left-panel {
    width: 420px;
    min-width: 320px;
    background: radial-gradient(circle at top left, rgba(59,130,246,0.16), transparent 30%),
                linear-gradient(180deg, rgba(15,23,42,0.98), rgba(30,41,59,0.98));
    display: flex;
    flex-direction: column;
    border-right: 1px solid rgba(148,163,184,0.22);
    overflow-y: auto;
    height: calc(100vh - 70px);
    box-shadow: inset 0 0 0 1px rgba(255,255,255,0.02);
    position: relative;
  }
  #left-panel::before {
    content: '';
    position: absolute;
    top: 32px;
    right: -38px;
    width: 120px;
    height: 120px;
    background: rgba(99,102,241,0.12);
    border-radius: 50%;
    z-index: 0;
  }

  #progress-bar {
    background: transparent;
    border-bottom: 1px solid rgba(255,255,255,0.08);
    padding: 18px 20px 14px;
    position: relative;
    z-index: 1;
  }
  .progress-steps { display: flex; gap: 8px; flex-wrap: wrap; }
  .step-dot {
    flex: 1;
    min-width: 32px;
    height: 8px;
    border-radius: 999px;
    background: rgba(148,163,184,0.18);
    cursor: pointer;
    transition: background 0.2s ease;
  }
  .step-dot.active { background: linear-gradient(90deg, #8b5cf6, #22d3ee); }
  .step-dot.done { background: linear-gradient(90deg, #34d399, #22d3ee); }
  .progress-label { font-size: 0.78rem; color: rgba(248,250,252,0.76); margin-top: 10px; }

  #section-nav {
    display: flex;
    gap: 8px;
    padding: 14px 16px;
    flex-wrap: wrap;
    border-bottom: 1px solid rgba(255,255,255,0.08);
    background: rgba(255,255,255,0.04);
    backdrop-filter: blur(10px);
  }
  .nav-tab {
    padding: 8px 14px;
    border-radius: 999px;
    font-size: 0.78rem;
    font-weight: 700;
    cursor: pointer;
    border: 1.5px solid transparent;
    color: rgba(248,250,252,0.78);
    transition: all 0.18s ease;
    background: rgba(255,255,255,0.04);
  }
  .nav-tab:hover { background: rgba(255,255,255,0.1); color: #fff; }
  .nav-tab.active {
    background: linear-gradient(135deg, rgba(139,92,246,0.96), rgba(59,130,246,0.9));
    color: #fff;
    border-color: rgba(255,255,255,0.15);
  }

  .form-section {
    padding: 24px;
    display: none;
    position: relative;
    z-index: 1;
  }
  .form-section.active { display: block; }
  .section-title {
    font-size: 1rem;
    font-weight: 800;
    color: #fff;
    margin-bottom: 18px;
    display: flex;
    align-items: center;
    gap: 10px;
    text-transform: uppercase;
    letter-spacing: 0.06em;
  }
  .section-title .icon { font-size: 1.25rem; }
  .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
  .form-group { margin-bottom: 14px; }
  .form-group label {
    display: block;
    font-size: 0.75rem;
    font-weight: 700;
    color: rgba(248,250,252,0.64);
    margin-bottom: 6px;
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }
  .form-group input,
  .form-group textarea,
  .form-group select {
    width: 100%;
    padding: 12px 14px;
    border: 1px solid rgba(148,163,184,0.18);
    border-radius: 14px;
    font-size: 0.95rem;
    color: #0f172a;
    background: rgba(255,255,255,0.93);
    transition: border-color 0.18s ease, transform 0.18s ease;
    font-family: inherit;
  }
  .form-group input:focus,
  .form-group textarea:focus,
  .form-group select:focus {
    outline: none;
    border-color: rgba(99,102,241,0.9);
    transform: translateY(-1px);
    box-shadow: 0 10px 24px rgba(15,23,42,0.12);
  }
  .form-group textarea { resize: vertical; min-height: 90px; }
  .form-group input.error { border-color: rgba(251,113,133,0.9); }
  .error-msg { font-size: 0.78rem; color: var(--danger); margin-top: 5px; }

  .entry-card {
    background: rgba(15, 23, 42, 0.96);
    border: 1px solid rgba(99,102,241,0.24);
    border-radius: 20px;
    padding: 18px;
    margin-bottom: 14px;
    position: relative;
    box-shadow: 0 24px 48px rgba(15,23,42,0.18);
  }
  .entry-card::before {
    content: '';
    position: absolute;
    inset: 0;
    border-radius: inherit;
    background: linear-gradient(135deg, rgba(99,102,241,0.06), transparent 50%);
    pointer-events: none;
  }
  .entry-card-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 14px; gap: 12px; }
  .entry-card-title { font-size: 0.92rem; font-weight: 800; color: #fff; }
  .bullets-label { font-size: 0.78rem; font-weight: 700; color: rgba(248,250,252,0.68); margin-bottom: 8px; text-transform: uppercase; letter-spacing: 0.08em; }
  .bullet-row { display: flex; gap: 10px; margin-bottom: 8px; align-items: flex-start; }
  .bullet-row input { flex: 1; }
  .bullet-num { font-size: 0.78rem; color: rgba(248,250,252,0.66); padding-top: 10px; min-width: 18px; font-weight: 700; }

  .skill-tags { display: flex; flex-wrap: wrap; gap: 9px; margin-bottom: 14px; }
  .skill-tag {
    background: rgba(99,102,241,0.18);
    color: #e0e7ff;
    border: 1px solid rgba(99,102,241,0.4);
    padding: 6px 12px;
    border-radius: 999px;
    font-size: 0.82rem;
    font-weight: 700;
    display: inline-flex;
    align-items: center;
    gap: 8px;
  }
  .skill-tag .rm { cursor: pointer; font-size: 1rem; line-height: 1; color: rgba(248,250,252,0.72); }
  .skill-tag .rm:hover { color: #fb7185; }
  .skill-input-row { display: flex; gap: 10px; }
  .skill-input-row input { flex: 1; }
  .skill-input-row select { width: 135px; }

  .ai-btn { box-shadow: 0 16px 32px rgba(59,130,246,0.22); }
  .ai-btn:hover { opacity: 0.96; }

  .template-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; }
  .template-card {
    border: 2px solid rgba(148,163,184,0.16);
    border-radius: 20px;
    padding: 14px;
    cursor: pointer;
    transition: transform 0.2s ease, border-color 0.2s ease, box-shadow 0.2s ease, background 0.2s ease;
    text-align: center;
    background: rgba(255,255,255,0.05);
    box-shadow: inset 0 0 0 1px rgba(255,255,255,0.02);
  }
  .template-card:hover { transform: translateY(-2px); border-color: rgba(139,92,246,0.9); background: rgba(99,102,241,0.08); }
  .template-card.selected {
    border-color: #8b5cf6;
    background: rgba(139,92,246,0.18);
    box-shadow: 0 18px 36px rgba(139,92,246,0.14);
  }
  .template-thumb {
    height: 82px;
    border-radius: 16px;
    margin-bottom: 10px;
    display: grid;
    gap: 6px;
    padding: 10px;
    overflow: hidden;
    background: rgba(15,23,42,0.9);
  }
  .t-line {
    height: 5px;
    border-radius: 999px;
    background: #334155;
  }
  .t-line.thick { height: 9px; }
  .template-card .t-name { font-size: 0.82rem; font-weight: 800; color: #f8fafc; }

  #right-panel {
    flex: 1;
    background: linear-gradient(180deg, rgba(15,23,42,0.96), rgba(17,24,39,0.96));
    display: flex;
    flex-direction: column;
    align-items: center;
    overflow-y: auto;
    padding: 28px 18px;
    height: calc(100vh - 70px);
  }
  #preview-wrapper {
    width: 100%;
    max-width: 820px;
    position: relative;
  }
  #preview-toolbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 16px;
    flex-wrap: wrap;
    gap: 12px;
    padding: 14px 18px;
    border-radius: 18px;
    background: rgba(15,23,42,0.78);
    border: 1px solid rgba(255,255,255,0.06);
    box-shadow: 0 22px 44px rgba(15,23,42,0.18);
  }
  #preview-toolbar span { color: #fff; font-size: 0.9rem; font-weight: 700; letter-spacing: 0.04em; }

  #resume {
    background: #0f172a;
    border-radius: 30px;
    min-height: 900px;
    overflow: hidden;
    box-shadow: 0 28px 80px rgba(15,23,42,0.4);
    border: 1px solid rgba(255,255,255,0.08);
  }
  #resume::before {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(circle at top right, rgba(59,130,246,0.14), transparent 18%),
                radial-gradient(circle at bottom left, rgba(236,72,153,0.10), transparent 15%);
    pointer-events: none;
  }

  .tpl-modern #resume-header,
  .tpl-classic #resume-header,
  .tpl-creative #resume-header {
    position: relative;
    overflow: hidden;
  }

  .tpl-modern #resume-header {
    background: linear-gradient(135deg, rgba(30,41,59,0.98), rgba(15,23,42,0.98));
    color: #fff;
    padding: 40px 44px 36px;
  }
  .tpl-modern #resume-header::after {
    content: '';
    position: absolute;
    right: -60px;
    bottom: -40px;
    width: 180px;
    height: 180px;
    background: rgba(99,102,241,0.12);
    border-radius: 50%;
  }
  .tpl-modern #resume-name { font-size: 2.45rem; font-weight: 900; letter-spacing: 0.07em; line-height: 1.05; }
  .tpl-modern #resume-title { font-size: 1.05rem; color: rgba(148,163,184,0.9); margin-top: 8px; }
  .tpl-modern #resume-contacts { display: flex; flex-wrap: wrap; gap: 14px; margin-top: 18px; }
  .tpl-modern .contact-item { font-size: 0.82rem; color: rgba(203,213,225,0.88); display: flex; align-items: center; gap: 8px; }
  .tpl-modern #resume-body { display: grid; grid-template-columns: 1fr 270px; gap: 0; }
  .tpl-modern #resume-main { padding: 32px 34px 36px; border-right: 1px solid rgba(226,232,240,0.18); background: rgba(255,255,255,0.02); }
  .tpl-modern #resume-sidebar { padding: 32px 28px; background: rgba(255,255,255,0.04); }
  .tpl-modern .res-section { margin-bottom: 28px; }
  .tpl-modern .res-sec-title { font-size: 0.72rem; font-weight: 900; text-transform: uppercase; letter-spacing: 0.24em; color: #8b5cf6; border-bottom: 2px solid rgba(139,92,246,0.25); padding-bottom: 6px; margin-bottom: 16px; }
  .tpl-modern .res-entry { margin-bottom: 18px; }
  .tpl-modern .res-entry-header { display: flex; justify-content: space-between; flex-wrap: wrap; gap: 8px; }
  .tpl-modern .res-entry-title { font-weight: 900; font-size: 1.12rem; color: #111827; }
  .tpl-modern .res-entry-sub { font-size: 0.96rem; color: #334155; margin-top: 4px; font-weight: 700; }
  .tpl-modern .res-entry-date { font-size: 0.82rem; color: #475569; white-space: nowrap; }
  .tpl-modern .res-bullets { margin-top: 10px; padding-left: 18px; }
  .tpl-modern .res-bullets li { font-size: 0.96rem; color: #243145; line-height: 1.8; margin-bottom: 6px; }
  .tpl-modern .summary-text { font-size: 1rem; color: #243145; font-weight: 600; line-height: 1.8; }
  .tpl-modern .skill-list { display: flex; flex-wrap: wrap; gap: 8px; }
  .tpl-modern .skill-pill { background: rgba(99,102,241,0.14); color: #c7d2fe; font-size: 0.78rem; font-weight: 700; padding: 4px 12px; border-radius: 999px; }
  .tpl-modern .sidebar-sec-title { font-size: 0.7rem; font-weight: 800; text-transform: uppercase; letter-spacing: 0.18em; color: #94a3b8; margin-bottom: 14px; border-bottom: 1.5px solid rgba(226,232,240,0.18); padding-bottom: 6px; }
  .tpl-modern .sidebar-item { font-size: 0.82rem; color: #cbd5e1; margin-bottom: 10px; line-height: 1.6; }
  .tpl-modern .sidebar-item strong { display: block; color: #f8fafc; font-weight: 700; }
  .tpl-modern .lang-item, .tpl-modern .cert-item { font-size: 0.82rem; color: #cbd5e1; margin-bottom: 8px; }

  .tpl-classic #resume-header {
    text-align: center;
    padding: 36px 40px 24px;
    border-bottom: 3px double rgba(248,250,252,0.12);
    background: rgba(248,250,252,0.03);
  }
  .tpl-classic #resume-name { font-size: 2.3rem; font-weight: 800; color: #f8fafc; text-transform: uppercase; letter-spacing: 0.14em; font-family: Georgia, serif; }
  .tpl-classic #resume-title { font-size: 1rem; color: #94a3b8; font-style: italic; margin-top: 6px; }
  .tpl-classic #resume-contacts { display: flex; justify-content: center; flex-wrap: wrap; gap: 14px; margin-top: 14px; }
  .tpl-classic .contact-item { font-size: 0.82rem; color: #cbd5e1; }
  .tpl-classic #resume-body { padding: 0 40px 34px; }
  .tpl-classic #resume-main { width: 100%; }
  .tpl-classic .res-section { margin-bottom: 24px; }
  .tpl-classic .res-sec-title { font-size: 0.86rem; font-weight: 800; text-transform: uppercase; letter-spacing: 0.2em; color: #f8fafc; border-bottom: 1.5px solid rgba(248,250,252,0.15); padding-bottom: 5px; margin-bottom: 14px; font-family: Georgia, serif; }
  .tpl-classic .res-entry { margin-bottom: 16px; display: flex; flex-wrap: wrap; gap: 18px; }
  .tpl-classic .res-entry-left { min-width: 120px; text-align: right; }
  .tpl-classic .res-entry-date { font-size: 0.78rem; color: #94a3b8; font-style: italic; }
  .tpl-classic .res-entry-right { flex: 1; border-left: 2px solid rgba(226,232,240,0.18); padding-left: 16px; }
  .tpl-classic .res-entry-title { font-weight: 900; font-size: 1.08rem; color: #111827; }
  .tpl-classic .res-entry-sub { font-size: 0.96rem; color: #334155; font-weight: 700; }
  .tpl-classic .res-bullets { margin-top: 8px; padding-left: 18px; }
  .tpl-classic .res-bullets li { font-size: 0.96rem; color: #243145; line-height: 1.8; margin-bottom: 4px; }
  .tpl-classic .summary-text { font-size: 1rem; color: #243145; font-weight: 600; line-height: 1.75; }
  .tpl-classic .skill-list { display: flex; flex-wrap: wrap; gap: 8px; }
  .tpl-classic .skill-pill { font-size: 0.82rem; color: #cbd5e1; }
  .tpl-classic .skill-pill::before { content: '▪ '; }
  .tpl-classic .classic-inline { display: inline; }
  .tpl-classic .sidebar-sections { width: 100%; }
  .tpl-classic .sidebar-sec-title { font-size: 0.85rem; font-weight: 800; text-transform: uppercase; letter-spacing: 0.2em; color: #f8fafc; border-bottom: 1.5px solid rgba(248,250,252,0.15); padding-bottom: 5px; margin: 20px 0 12px; font-family: Georgia, serif; }
  .tpl-classic .sidebar-item, .tpl-classic .lang-item, .tpl-classic .cert-item { font-size: 0.84rem; color: #cbd5e1; margin-bottom: 6px; display: inline-block; margin-right: 16px; }
  .tpl-classic .sidebar-item strong { font-weight: 700; }

  .tpl-creative #resume-header {
    background: linear-gradient(135deg, rgba(139,92,246,0.98), rgba(37,99,235,0.96));
    color: #fff;
    padding: 34px 38px 38px;
    position: relative;
    overflow: hidden;
  }
  .tpl-creative #resume-header::before {
    content: '';
    position: absolute;
    left: -40px;
    top: -40px;
    width: 160px;
    height: 160px;
    background: rgba(255,255,255,0.1);
    border-radius: 50%;
  }
  .tpl-creative #resume-header::after {
    content: '';
    position: absolute;
    right: 28px;
    bottom: -36px;
    width: 140px;
    height: 140px;
    background: rgba(255,255,255,0.08);
    border-radius: 50%;
  }
  .tpl-creative #resume-name { font-size: 2.5rem; font-weight: 900; letter-spacing: 0.08em; }
  .tpl-creative #resume-title { font-size: 1.05rem; color: rgba(255,255,255,0.86); margin-top: 6px; font-weight: 600; }
  .tpl-creative #resume-contacts { display: flex; flex-wrap: wrap; gap: 14px; margin-top: 16px; }
  .tpl-creative .contact-item { font-size: 0.82rem; color: rgba(255,255,255,0.9); background: rgba(255,255,255,0.12); padding: 6px 12px; border-radius: 999px; display: flex; align-items: center; gap: 8px; }
  .tpl-creative #resume-body { display: grid; grid-template-columns: 1fr 250px; gap: 0; }
  .tpl-creative #resume-main { padding: 32px 36px 34px; }
  .tpl-creative #resume-sidebar { padding: 32px 26px; background: rgba(248,250,252,0.05); border-left: 3px solid rgba(139,92,246,0.9); }
  .tpl-creative .res-section { margin-bottom: 26px; }
  .tpl-creative .res-sec-title {
    font-size: 0.72rem;
    font-weight: 900;
    text-transform: uppercase;
    letter-spacing: 0.2em;
    color: #d8b4fe;
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 16px;
  }
  .tpl-creative .res-sec-title::after { content: ''; flex: 1; height: 2px; background: linear-gradient(90deg, rgba(237, 73, 153, 0.8), transparent); }
  .tpl-creative .res-entry { margin-bottom: 18px; }
  .tpl-creative .res-entry-header { display: flex; justify-content: space-between; flex-wrap: wrap; gap: 8px; }
  .tpl-creative .res-entry-title { font-weight: 900; font-size: 1.12rem; color: #111827; }
  .tpl-creative .res-entry-sub { font-size: 0.96rem; color: #334155; font-weight: 700; }
  .tpl-creative .res-entry-date { font-size: 0.82rem; background: rgba(236,72,153,0.15); color: #be185d; padding: 4px 10px; border-radius: 999px; white-space: nowrap; }
  .tpl-creative .res-bullets { margin-top: 10px; padding-left: 18px; }
  .tpl-creative .res-bullets li { font-size: 0.96rem; color: #243145; line-height: 1.8; margin-bottom: 5px; }
  .tpl-creative .summary-text { font-size: 1rem; color: #243145; font-weight: 600; line-height: 1.8; }
  .tpl-creative .skill-list { display: flex; flex-wrap: wrap; gap: 10px; }
  .tpl-creative .skill-pill { background: linear-gradient(135deg, rgba(139,92,246,0.95), rgba(37,99,235,0.9)); color: #fff; font-size: 0.78rem; font-weight: 700; padding: 5px 12px; border-radius: 999px; }
  .tpl-creative .sidebar-sec-title { font-size: 0.72rem; font-weight: 900; text-transform: uppercase; letter-spacing: 0.16em; color: #a78bfa; margin-bottom: 14px; border-bottom: 1.5px solid rgba(248,250,252,0.12); padding-bottom: 6px; }
  .tpl-creative .sidebar-item { font-size: 0.82rem; color: #dbeafe; margin-bottom: 8px; line-height: 1.6; }
  .tpl-creative .sidebar-item strong { display: block; color: #f8fafc; font-weight: 700; }
  .tpl-creative .lang-item, .tpl-creative .cert-item { font-size: 0.82rem; color: #dbeafe; margin-bottom: 6px; }

  .classic-sidebar-sections { margin-top: 0; }

  @media print {
    body * { visibility: hidden; }
    #resume, #resume * { visibility: visible; }
    #resume { position: fixed; left: 0; top: 0; width: 100%; box-shadow: none; }
    #app header, #left-panel, #preview-toolbar, #right-panel > *:not(#preview-wrapper) { display: none !important; }
    #right-panel { background: white; padding: 0; height: auto; overflow: visible; }
    #preview-wrapper { max-width: 100%; }
  }

  @media (max-width: 820px) {
    main { flex-direction: column; overflow: visible; }
    #left-panel { width: 100%; min-width: unset; height: auto; border-right: none; border-bottom: 1px solid rgba(255,255,255,0.08); }
    #right-panel { height: auto; padding: 18px 10px; }
    .tpl-modern #resume-body,
    .tpl-creative #resume-body { grid-template-columns: 1fr; }
    .tpl-modern #resume-sidebar,
    .tpl-creative #resume-sidebar { border: none; border-top: 1px solid rgba(255,255,255,0.08); }
    .form-row { grid-template-columns: 1fr; }
  }

  #toast {
    position: fixed;
    bottom: 24px;
    right: 24px;
    background: rgba(15,23,42,0.96);
    color: #fff;
    padding: 12px 20px;
    border-radius: 16px;
    font-size: 0.88rem;
    font-weight: 700;
    opacity: 0;
    transform: translateY(14px);
    transition: all 0.25s ease;
    pointer-events: none;
    z-index: 999;
    border: 1px solid rgba(255,255,255,0.08);
  }
  #toast.show { opacity: 1; transform: translateY(0); }

  .empty-hint { color: rgba(203,213,225,0.72); font-size: 0.84rem; font-style: italic; text-align: center; padding: 12px 0; }
</style>
</head>
<body>
<div id="app">
  <header>
    <h1>📄 Resume<span>Builder</span></h1>
    <div class="header-actions">
      <button class="btn btn-outline btn-sm" onclick="clearAll()">🗑 Clear</button>
      <button class="btn btn-white btn-sm" onclick="downloadText()">📋 Text</button>
      <button class="btn btn-success btn-sm" onclick="downloadPDF()">⬇ PDF</button>
    </div>
  </header>
  <main>
    <!-- LEFT PANEL -->
    <div id="left-panel">
      <div id="progress-bar">
        <div class="progress-steps" id="progress-steps"></div>
        <div class="progress-label" id="progress-label">Step 1 of 7</div>
      </div>
      <div id="section-nav"></div>

      <!-- PERSONAL INFO -->
      <div class="form-section active" data-section="0">
        <div class="section-title"><span class="icon">👤</span> Personal Info</div>
        <div class="form-row">
          <div class="form-group"><label>Full Name *</label><input id="f-name" placeholder="John Doe" oninput="updatePreview()"></div>
          <div class="form-group"><label>Job Title</label><input id="f-title" placeholder="Software Engineer" oninput="updatePreview()"></div>
        </div>
        <div class="form-row">
          <div class="form-group"><label>Email *</label><input id="f-email" type="email" placeholder="john@email.com" oninput="updatePreview()"></div>
          <div class="form-group"><label>Phone</label><input id="f-phone" placeholder="+1 234 567 8900" oninput="updatePreview()"></div>
        </div>
        <div class="form-row">
          <div class="form-group"><label>Location</label><input id="f-location" placeholder="New York, NY" oninput="updatePreview()"></div>
          <div class="form-group"><label>LinkedIn</label><input id="f-linkedin" placeholder="linkedin.com/in/johndoe" oninput="updatePreview()"></div>
        </div>
        <div class="form-group"><label>Portfolio / Website</label><input id="f-portfolio" placeholder="github.com/johndoe" oninput="updatePreview()"></div>
      </div>

      <!-- SUMMARY -->
      <div class="form-section" data-section="1">
        <div class="section-title"><span class="icon">📝</span> Professional Summary</div>
        <button class="ai-btn" id="ai-summary-btn" onclick="aiGenerateSummary()">✨ AI Write Summary</button>
        <p style="margin:10px 0 16px;font-size:.82rem;color:var(--text-light);">Use AI to generate a strong, ATS-friendly summary from your profile and skills.</p>
        <div class="form-group">
          <label>Summary (2–4 sentences)</label>
          <textarea id="f-summary" rows="4" placeholder="Results-driven software engineer with 3+ years of experience..." oninput="updatePreview()"></textarea>
        </div>
      </div>

      <!-- EXPERIENCE -->
      <div class="form-section" data-section="2">
        <div class="section-title"><span class="icon">💼</span> Work Experience</div>
        <div id="exp-container"></div>
        <button class="btn btn-add" onclick="addEntry('exp')">＋ Add Experience</button>
      </div>

      <!-- EDUCATION -->
      <div class="form-section" data-section="3">
        <div class="section-title"><span class="icon">🎓</span> Education</div>
        <div id="edu-container"></div>
        <button class="btn btn-add" onclick="addEntry('edu')">＋ Add Education</button>
      </div>

      <!-- SKILLS -->
      <div class="form-section" data-section="4">
        <div class="section-title"><span class="icon">⚡</span> Skills</div>
        <div class="skill-tags" id="skill-tags-container"></div>
        <div class="skill-input-row">
          <input id="skill-input" placeholder="e.g. JavaScript" onkeydown="if(event.key==='Enter'){addSkill();}">
          <select id="skill-level">
            <option value="Expert">Expert</option>
            <option value="Advanced">Advanced</option>
            <option value="Intermediate" selected>Intermediate</option>
            <option value="Beginner">Beginner</option>
          </select>
          <button class="btn btn-primary btn-sm" onclick="addSkill()">Add</button>
        </div>
        <p style="font-size:.75rem;color:var(--text-light);margin-top:8px;">Press Enter or click Add to insert a skill.</p>
      </div>

      <!-- PROJECTS -->
      <div class="form-section" data-section="5">
        <div class="section-title"><span class="icon">🚀</span> Projects</div>
        <div id="proj-container"></div>
        <button class="btn btn-add" onclick="addEntry('proj')">＋ Add Project</button>
      </div>

      <!-- EXTRAS -->
      <div class="form-section" data-section="6">
        <div class="section-title"><span class="icon">🏆</span> Certifications, Languages & Extras</div>
        <div id="cert-container"></div>
        <button class="btn btn-add" onclick="addEntry('cert')">＋ Add Certification</button>
        <div style="height:16px;"></div>
        <div id="lang-container"></div>
        <button class="btn btn-add" onclick="addEntry('lang')">＋ Add Language</button>
      </div>

      <!-- TEMPLATE -->
      <div class="form-section" data-section="7">
        <div class="section-title"><span class="icon">🎨</span> Choose Template</div>
        <div class="template-grid">
          <div class="template-card selected" data-tpl="modern" onclick="setTemplate('modern')">
            <div class="template-thumb" style="background:#1e293b;border-radius:5px;padding:8px;">
              <div class="t-line thick" style="background:#fff;width:60%;"></div>
              <div class="t-line" style="background:rgba(255,255,255,.4);width:80%;"></div>
              <div class="t-line" style="background:#3b82f6;width:40%;"></div>
              <div class="t-line" style="width:90%;"></div>
              <div class="t-line" style="width:75%;"></div>
            </div>
            <div class="t-name">Modern</div>
          </div>
          <div class="template-card" data-tpl="classic" onclick="setTemplate('classic')">
            <div class="template-thumb" style="background:#fff;border:1px solid #e2e8f0;border-radius:5px;padding:8px;">
              <div class="t-line thick" style="background:#1e293b;width:70%;margin:0 auto;"></div>
              <div class="t-line" style="background:#64748b;width:50%;margin:0 auto;"></div>
              <div class="t-line" style="background:#1e293b;width:80%;margin-top:4px;"></div>
              <div class="t-line" style="width:90%;"></div>
              <div class="t-line" style="width:75%;"></div>
            </div>
            <div class="t-name">Classic</div>
          </div>
          <div class="template-card" data-tpl="creative" onclick="setTemplate('creative')">
            <div class="template-thumb" style="background:linear-gradient(135deg,#7c3aed,#2563eb);border-radius:5px;padding:8px;">
              <div class="t-line thick" style="background:#fff;width:65%;"></div>
              <div class="t-line" style="background:rgba(255,255,255,.5);width:80%;"></div>
              <div class="t-line" style="background:rgba(255,255,255,.3);width:90%;margin-top:4px;"></div>
              <div class="t-line" style="background:rgba(255,255,255,.5);width:70%;"></div>
            </div>
            <div class="t-name">Creative</div>
          </div>
        </div>
      </div>
    </div>

    <!-- RIGHT PANEL -->
    <div id="right-panel">
      <div id="preview-wrapper">
        <div id="preview-toolbar">
          <span>📄 Live Preview</span>
          <div style="display:flex;gap:8px;">
            <button class="btn btn-white btn-sm" onclick="downloadText()">📋 Text</button>
            <button class="btn btn-success btn-sm" onclick="downloadPDF()">⬇ Download PDF</button>
          </div>
        </div>
        <div id="resume" class="tpl-modern">
          <div id="resume-header">
            <div id="resume-name">Your Name</div>
            <div id="resume-title-display"></div>
            <div id="resume-contacts"></div>
          </div>
          <div id="resume-body">
            <div id="resume-main"></div>
            <div id="resume-sidebar"></div>
          </div>
        </div>
      </div>
    </div>
  </main>
</div>
<div id="toast"></div>

<script>
// ── STATE ──
let state = {
  template: 'modern',
  personal: {},
  summary: '',
  experience: [],
  education: [],
  skills: [],
  projects: [],
  certs: [],
  langs: []
};

const SECTIONS = ['Personal','Summary','Experience','Education','Skills','Projects','Extras','Template'];
let currentSection = 0;

// ── INIT ──
function init() {
  loadState();
  buildNav();
  buildProgress();
  renderForms();
  updatePreview();
}

function buildNav() {
  const nav = document.getElementById('section-nav');
  nav.innerHTML = SECTIONS.map((s,i)=>`<div class="nav-tab${i===currentSection?' active':''}" onclick="goSection(${i})">${s}</div>`).join('');
}

function buildProgress() {
  const c = document.getElementById('progress-steps');
  c.innerHTML = SECTIONS.map((_,i)=>`<div class="step-dot${i===currentSection?' active':i<currentSection?' done':''}" onclick="goSection(${i})"></div>`).join('');
  document.getElementById('progress-label').textContent = `Step ${currentSection+1} of ${SECTIONS.length}`;
}

function goSection(i) {
  currentSection = i;
  document.querySelectorAll('.form-section').forEach((s,idx)=>s.classList.toggle('active', idx===i));
  buildNav();
  buildProgress();
}

// ── ENTRY MANAGEMENT ──
function addEntry(type) {
  const id = Date.now();
  if(type==='exp') state.experience.push({id,title:'',company:'',start:'',end:'',location:'',bullets:['','','']});
  if(type==='edu') state.education.push({id,degree:'',institution:'',start:'',end:'',gpa:'',honors:''});
  if(type==='proj') state.projects.push({id,name:'',tech:'',desc:'',link:''});
  if(type==='cert') state.certs.push({id,name:'',issuer:'',year:''});
  if(type==='lang') state.langs.push({id,lang:'',level:'Fluent'});
  renderForms();
  updatePreview();
  saveState();
}

function removeEntry(type, id) {
  if(type==='exp') state.experience = state.experience.filter(e=>e.id!==id);
  if(type==='edu') state.education = state.education.filter(e=>e.id!==id);
  if(type==='proj') state.projects = state.projects.filter(e=>e.id!==id);
  if(type==='cert') state.certs = state.certs.filter(e=>e.id!==id);
  if(type==='lang') state.langs = state.langs.filter(e=>e.id!==id);
  renderForms();
  updatePreview();
  saveState();
}

function renderForms() {
  renderExp(); renderEdu(); renderProj(); renderCerts(); renderLangs(); renderSkillTags();
}

function renderExp() {
  const c = document.getElementById('exp-container');
  if(!state.experience.length){c.innerHTML='<p class="empty-hint">No experience added yet.</p>';return;}
  c.innerHTML = state.experience.map((e,i)=>`
    <div class="entry-card">
      <div class="entry-card-header">
        <span class="entry-card-title">Experience ${i+1}</span>
        <button class="btn btn-danger" onclick="removeEntry('exp',${e.id})">✕ Remove</button>
      </div>
      <div class="form-row">
        <div class="form-group"><label>Job Title</label><input value="${esc(e.title)}" oninput="state.experience[${i}].title=this.value;updatePreview();saveState();" placeholder="Software Engineer"></div>
        <div class="form-group"><label>Company</label><input value="${esc(e.company)}" oninput="state.experience[${i}].company=this.value;updatePreview();saveState();" placeholder="Google"></div>
      </div>
      <div class="form-row">
        <div class="form-group"><label>Start</label><input value="${esc(e.start)}" oninput="state.experience[${i}].start=this.value;updatePreview();saveState();" placeholder="Jan 2022"></div>
        <div class="form-group"><label>End</label><input value="${esc(e.end)}" oninput="state.experience[${i}].end=this.value;updatePreview();saveState();" placeholder="Present"></div>
      </div>
      <div class="form-group"><label>Location</label><input value="${esc(e.location)}" oninput="state.experience[${i}].location=this.value;updatePreview();saveState();" placeholder="New York, NY"></div>
      <div class="bullets-label">Key Achievements (use action verbs + numbers)</div>
      ${e.bullets.map((b,bi)=>`
        <div class="bullet-row">
          <span class="bullet-num">•${bi+1}</span>
          <input value="${esc(b)}" oninput="state.experience[${i}].bullets[${bi}]=this.value;updatePreview();saveState();" placeholder="Increased revenue by 30% through...">
        </div>`).join('')}
      <button class="btn btn-add" style="margin-top:6px;padding:6px;" onclick="state.experience[${i}].bullets.push('');renderForms();updatePreview();">＋ Add Bullet</button>
    </div>`).join('');
}

function renderEdu() {
  const c = document.getElementById('edu-container');
  if(!state.education.length){c.innerHTML='<p class="empty-hint">No education added yet.</p>';return;}
  c.innerHTML = state.education.map((e,i)=>`
    <div class="entry-card">
      <div class="entry-card-header">
        <span class="entry-card-title">Education ${i+1}</span>
        <button class="btn btn-danger" onclick="removeEntry('edu',${e.id})">✕ Remove</button>
      </div>
      <div class="form-group"><label>Degree / Certification</label><input value="${esc(e.degree)}" oninput="state.education[${i}].degree=this.value;updatePreview();saveState();" placeholder="B.S. Computer Science"></div>
      <div class="form-group"><label>Institution</label><input value="${esc(e.institution)}" oninput="state.education[${i}].institution=this.value;updatePreview();saveState();" placeholder="MIT"></div>
      <div class="form-row">
        <div class="form-group"><label>Start</label><input value="${esc(e.start)}" oninput="state.education[${i}].start=this.value;updatePreview();saveState();" placeholder="2018"></div>
        <div class="form-group"><label>End</label><input value="${esc(e.end)}" oninput="state.education[${i}].end=this.value;updatePreview();saveState();" placeholder="2022"></div>
      </div>
      <div class="form-row">
        <div class="form-group"><label>GPA</label><input value="${esc(e.gpa)}" oninput="state.education[${i}].gpa=this.value;updatePreview();saveState();" placeholder="3.8/4.0"></div>
        <div class="form-group"><label>Honors</label><input value="${esc(e.honors)}" oninput="state.education[${i}].honors=this.value;updatePreview();saveState();" placeholder="Cum Laude"></div>
      </div>
    </div>`).join('');
}

function renderProj() {
  const c = document.getElementById('proj-container');
  if(!state.projects.length){c.innerHTML='<p class="empty-hint">No projects added yet.</p>';return;}
  c.innerHTML = state.projects.map((p,i)=>`
    <div class="entry-card">
      <div class="entry-card-header">
        <span class="entry-card-title">Project ${i+1}</span>
        <button class="btn btn-danger" onclick="removeEntry('proj',${p.id})">✕ Remove</button>
      </div>
      <div class="form-row">
        <div class="form-group"><label>Project Name</label><input value="${esc(p.name)}" oninput="state.projects[${i}].name=this.value;updatePreview();saveState();" placeholder="Portfolio Website"></div>
        <div class="form-group"><label>Tech Stack</label><input value="${esc(p.tech)}" oninput="state.projects[${i}].tech=this.value;updatePreview();saveState();" placeholder="React, Node.js"></div>
      </div>
      <div class="form-group"><label>Description</label><textarea oninput="state.projects[${i}].desc=this.value;updatePreview();saveState();" placeholder="Built a full-stack application that...">${esc(p.desc)}</textarea></div>
      <div class="form-group"><label>Link</label><input value="${esc(p.link)}" oninput="state.projects[${i}].link=this.value;updatePreview();saveState();" placeholder="github.com/..."></div>
    </div>`).join('');
}

function renderCerts() {
  const c = document.getElementById('cert-container');
  c.innerHTML = (state.certs.length ? '' : '<p class="empty-hint" style="margin-bottom:8px;">No certifications added.</p>') +
    state.certs.map((e,i)=>`
      <div class="entry-card">
        <div class="entry-card-header"><span class="entry-card-title">Cert ${i+1}</span><button class="btn btn-danger" onclick="removeEntry('cert',${e.id})">✕</button></div>
        <div class="form-row">
          <div class="form-group"><label>Name</label><input value="${esc(e.name)}" oninput="state.certs[${i}].name=this.value;updatePreview();saveState();" placeholder="AWS Certified..."></div>
          <div class="form-group"><label>Issuer</label><input value="${esc(e.issuer)}" oninput="state.certs[${i}].issuer=this.value;updatePreview();saveState();" placeholder="Amazon"></div>
        </div>
        <div class="form-group"><label>Year</label><input value="${esc(e.year)}" oninput="state.certs[${i}].year=this.value;updatePreview();saveState();" placeholder="2023"></div>
      </div>`).join('');
}

function renderLangs() {
  const c = document.getElementById('lang-container');
  c.innerHTML = `<div class="section-title" style="font-size:.85rem;margin-bottom:10px;"><span class="icon">🌐</span> Languages</div>` +
    (state.langs.length ? '' : '<p class="empty-hint" style="margin-bottom:8px;">No languages added.</p>') +
    state.langs.map((e,i)=>`
      <div class="entry-card">
        <div class="entry-card-header"><span class="entry-card-title">Language ${i+1}</span><button class="btn btn-danger" onclick="removeEntry('lang',${e.id})">✕</button></div>
        <div class="form-row">
          <div class="form-group"><label>Language</label><input value="${esc(e.lang)}" oninput="state.langs[${i}].lang=this.value;updatePreview();saveState();" placeholder="Spanish"></div>
          <div class="form-group"><label>Level</label>
            <select onchange="state.langs[${i}].level=this.value;updatePreview();saveState();">
              ${['Native','Fluent','Advanced','Intermediate','Basic'].map(l=>`<option${l===e.level?' selected':''}>${l}</option>`).join('')}
            </select>
          </div>
        </div>
      </div>`).join('');
}

function renderSkillTags() {
  const c = document.getElementById('skill-tags-container');
  c.innerHTML = state.skills.map((s,i)=>`
    <span class="skill-tag">${esc(s.name)} <small style="opacity:.7">${s.level}</small><span class="rm" onclick="removeSkill(${i})">×</span></span>`).join('');
}

function addSkill() {
  const inp = document.getElementById('skill-input');
  const lvl = document.getElementById('skill-level').value;
  const name = inp.value.trim();
  if(!name) return;
  if(state.skills.find(s=>s.name.toLowerCase()===name.toLowerCase())){showToast('Skill already added!');return;}
  state.skills.push({name,level:lvl});
  inp.value='';
  renderSkillTags();
  updatePreview();
  saveState();
}

function removeSkill(i) {
  state.skills.splice(i,1);
  renderSkillTags();
  updatePreview();
  saveState();
}

// ── TEMPLATE ──
function setTemplate(tpl) {
  state.template = tpl;
  document.getElementById('resume').className = 'tpl-'+tpl;
  document.querySelectorAll('.template-card').forEach(c=>c.classList.toggle('selected',c.dataset.tpl===tpl));
  updatePreview();
  saveState();
}

// ── PREVIEW ──
function gv(id){ const el=document.getElementById(id); return el?el.value.trim():''; }

function updatePreview() {
  // Sync personal from inputs
  state.personal = {
    name: gv('f-name'), title: gv('f-title'), email: gv('f-email'),
    phone: gv('f-phone'), location: gv('f-location'), linkedin: gv('f-linkedin'), portfolio: gv('f-portfolio')
  };
  state.summary = gv('f-summary');

  const p = state.personal;
  document.getElementById('resume-name').textContent = p.name || 'Your Name';
  document.getElementById('resume-title-display').textContent = p.title || '';

  // Contacts
  const contacts = [
    p.email && `<span class="contact-item">✉ ${p.email}</span>`,
    p.phone && `<span class="contact-item">📞 ${p.phone}</span>`,
    p.location && `<span class="contact-item">📍 ${p.location}</span>`,
    p.linkedin && `<span class="contact-item">🔗 ${p.linkedin}</span>`,
    p.portfolio && `<span class="contact-item">🌐 ${p.portfolio}</span>`
  ].filter(Boolean).join('');
  document.getElementById('resume-contacts').innerHTML = contacts;

  // Build main + sidebar based on template
  const isSidebar = state.template !== 'classic';
  let main = '', sidebar = '';

  // Summary
  if(state.summary) main += `<div class="res-section"><div class="res-sec-title">Summary</div><p class="summary-text">${esc(state.summary)}</p></div>`;

  // Experience
  if(state.experience.length) {
    main += `<div class="res-section"><div class="res-sec-title">Experience</div>` +
      state.experience.map(e=>{
        const bullets = e.bullets.filter(b=>b.trim());
        if(state.template==='classic') return `
          <div class="res-entry">
            <div class="res-entry-left"><div class="res-entry-date">${esc(e.start)}${e.end?'–'+esc(e.end):''}</div>${e.location?`<div style="font-size:.75rem;color:#94a3b8;">${esc(e.location)}</div>`:''}</div>
            <div class="res-entry-right">
              <div class="res-entry-title">${esc(e.title)||'Job Title'}</div>
              <div class="res-entry-sub">${esc(e.company)||'Company'}</div>
              ${bullets.length?`<ul class="res-bullets">${bullets.map(b=>`<li>${esc(b)}</li>`).join('')}</ul>`:''}
            </div>
          </div>`;
        return `<div class="res-entry">
          <div class="res-entry-header">
            <div><div class="res-entry-title">${esc(e.title)||'Job Title'}</div><div class="res-entry-sub">${esc(e.company)||'Company'}${e.location?' · '+esc(e.location):''}</div></div>
            <div class="res-entry-date">${esc(e.start)}${e.end?' – '+esc(e.end):''}</div>
          </div>
          ${bullets.length?`<ul class="res-bullets">${bullets.map(b=>`<li>${esc(b)}</li>`).join('')}</ul>`:''}
        </div>`;
      }).join('') + '</div>';
  }

  // Projects
  if(state.projects.length) {
    main += `<div class="res-section"><div class="res-sec-title">Projects</div>` +
      state.projects.map(p=>`
        <div class="res-entry">
          <div class="res-entry-header">
            <div><div class="res-entry-title">${esc(p.name)||'Project'}</div>${p.tech?`<div class="res-entry-sub">${esc(p.tech)}</div>`:''}</div>
            ${p.link?`<div class="res-entry-date">${esc(p.link)}</div>`:''}
          </div>
          ${p.desc?`<p style="font-size:.82rem;color:#475569;margin-top:5px;line-height:1.5;">${esc(p.desc)}</p>`:''}
        </div>`).join('') + '</div>';
  }

  // Education (main for classic, sidebar for others)
  const eduHTML = state.education.length ? `
    <div class="res-section">
      <div class="${isSidebar?'sidebar-sec-title':'res-sec-title'}">Education</div>
      ${state.education.map(e=>{
        if(state.template==='classic') return `
          <div class="res-entry">
            <div class="res-entry-left"><div class="res-entry-date">${esc(e.start)}${e.end?'–'+esc(e.end):''}</div></div>
            <div class="res-entry-right">
              <div class="res-entry-title">${esc(e.degree)||'Degree'}</div>
              <div class="res-entry-sub">${esc(e.institution)||'Institution'}</div>
              ${e.gpa||e.honors?`<div style="font-size:.78rem;color:#94a3b8;">${[e.gpa&&'GPA: '+esc(e.gpa), e.honors&&esc(e.honors)].filter(Boolean).join(' · ')}</div>`:''}
            </div>
          </div>`;
        return `<div class="sidebar-item"><strong>${esc(e.degree)||'Degree'}</strong>${esc(e.institution)||'Institution'}${e.start||e.end?`<br><span style="font-size:.75rem;color:#94a3b8;">${esc(e.start)}${e.end?' – '+esc(e.end):''}</span>`:''}${e.gpa?`<br><span style="font-size:.75rem;">GPA: ${esc(e.gpa)}</span>`:''}</div>`;
      }).join('')}
    </div>` : '';

  // Skills
  const skillHTML = state.skills.length ? `
    <div class="res-section">
      <div class="${isSidebar?'sidebar-sec-title':'res-sec-title'}">Skills</div>
      <div class="skill-list">${state.skills.map(s=>`<span class="skill-pill">${esc(s.name)}</span>`).join('')}</div>
    </div>` : '';

  // Certs
  const certHTML = state.certs.length ? `
    <div class="res-section">
      <div class="${isSidebar?'sidebar-sec-title':'res-sec-title'}">Certifications</div>
      ${state.certs.map(c=>`<div class="${isSidebar?'cert-item':'res-entry-sub'}" style="margin-bottom:5px;font-size:.82rem;color:#475569;">🏅 ${esc(c.name)}${c.issuer?' · '+esc(c.issuer):''}${c.year?' ('+esc(c.year)+')':''}</div>`).join('')}
    </div>` : '';

  // Langs
  const langHTML = state.langs.length ? `
    <div class="res-section">
      <div class="${isSidebar?'sidebar-sec-title':'res-sec-title'}">Languages</div>
      ${state.langs.map(l=>`<div class="${isSidebar?'lang-item':'res-entry-sub'}" style="margin-bottom:5px;font-size:.82rem;color:#475569;">🌐 ${esc(l.lang)}${l.level?' · '+l.level:''}</div>`).join('')}
    </div>` : '';

  if(isSidebar) {
    sidebar = eduHTML + skillHTML + certHTML + langHTML;
  } else {
    main += eduHTML + `<div class="classic-sidebar-sections">${skillHTML}${certHTML}${langHTML}</div>`;
  }

  document.getElementById('resume-main').innerHTML = main || '<p style="color:#94a3b8;font-size:.85rem;padding:20px;">Start filling in the form to see your resume here.</p>';
  document.getElementById('resume-sidebar').innerHTML = isSidebar ? sidebar : '';
}

// ── AI GENERATE ──
async function aiGenerateSummary() {
  const name = gv('f-name');
  const title = gv('f-title');
  const skills = state.skills.map(s=>s.name).join(', ');
  const exp = state.experience.map(e=>`${e.title} at ${e.company}`).join('; ');
  if(!name && !title && !skills){showToast('Fill in your name, title, or skills first!'); return;}
  const btn = document.getElementById('ai-summary-btn');
  btn.disabled = true;
  btn.innerHTML = '<span class="ai-loading"></span> Generating...';

  const localSummary = () => {
    const items = [];
    if (title) items.push(`${title}`);
    if (skills) items.push(`expertise in ${skills}`);
    if (exp) items.push(`proven success in ${state.experience.length} role${state.experience.length===1?'':'s'}`);
    const opener = title ? `${title}` : 'A motivated professional';
    const body = items.length ? `${opener} with ${items.join(', ')}.` : `${opener} with a strong foundation in career development.`;
    return `${name ? name + ' is ' : ''}${body} Highly collaborative and results-driven, ready to deliver value with a polished and strategic approach.`;
  };

  try {
    const prompt = `Write a professional resume summary (2-3 sentences, ATS-friendly, strong action verbs) for:\nName: ${name||'a professional'}\nTitle: ${title||'Job Seeker'}\nSkills: ${skills||'various skills'}\nExperience: ${exp||'various roles'}\nOutput ONLY the summary text, no labels or quotes.`;

    const res = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: {"Content-Type":"application/json"},
      body: JSON.stringify({
        model: "claude-sonnet-4-6",
        max_tokens: 200,
        messages: [{role:"user", content: prompt}]
      })
    });

    const data = await res.json();
    const text = data.completion || data.content?.find(c=>c.type==='text')?.text || data.output?.[0]?.content?.[0]?.text || '';
    if(text.trim()) {
      document.getElementById('f-summary').value = text.trim();
      state.summary = text.trim();
      updatePreview();
      saveState();
      showToast('✨ Summary generated!');
    } else {
      throw new Error('No summary returned');
    }
  } catch (e) {
    const fallback = localSummary();
    document.getElementById('f-summary').value = fallback;
    state.summary = fallback;
    updatePreview();
    saveState();
    showToast('✨ AI fallback summary generated.');
  }

  btn.disabled = false;
  btn.innerHTML = '✨ AI Write Summary';
}

// ── EXPORT ──
async function downloadPDF() {
  const btns = [...document.querySelectorAll('button[onclick="downloadPDF()"]')];
  btns.forEach(b => { b.disabled = true; b.dataset.orig = b.innerHTML; b.innerHTML = '📦 Generating...'; });

  try {
    const jsPDFlib = window.jspdf || window.jspPDF || window.jsPDF || null;
    const jsPDF = jsPDFlib?.jsPDF || jsPDFlib;
    if (!jsPDF) {
      console.warn('PDF export failed because jsPDF was not loaded.', { jspdf: window.jspdf, jspPDF: window.jspPDF, jsPDF: window.jsPDF });
      showToast('PDF library missing. Falling back to print dialog.');
      setTimeout(() => window.print(), 250);
      return;
    }

    const resume = document.getElementById('resume');
    const clone = resume.cloneNode(true);
    clone.style.transform = 'scale(1)';
    clone.style.width = `${resume.offsetWidth}px`;
    clone.style.padding = '0';
    clone.style.margin = '0';
    clone.style.boxShadow = 'none';
    clone.style.background = '#fff';

    const wrapper = document.createElement('div');
    wrapper.style.position = 'fixed';
    wrapper.style.top = '-9999px';
    wrapper.style.left = '-9999px';
    wrapper.style.width = `${resume.offsetWidth}px`;
    wrapper.style.pointerEvents = 'none';
    wrapper.appendChild(clone);
    document.body.appendChild(wrapper);

    const canvas = await html2canvas(clone, {
      scale: 2,
      useCORS: true,
      backgroundColor: '#ffffff'
    });

    document.body.removeChild(wrapper);

    const imgData = canvas.toDataURL('image/png');
    const pdf = new jsPDF({ orientation: 'portrait', unit: 'px', format: [canvas.width, canvas.height] });
    pdf.addImage(imgData, 'PNG', 0, 0, canvas.width, canvas.height);
    const filename = `${(state.personal.name || 'resume').replace(/\s+/g, '_')}_resume.pdf`;
    pdf.save(filename);
    showToast('✅ PDF downloaded!');
  } catch (error) {
    console.error(error);
    showToast('PDF export failed. Try again.');
  } finally {
    btns.forEach(b => { b.disabled = false; b.innerHTML = b.dataset.orig || '⬇ Download PDF'; });
  }
}

function downloadText() {
  const p = state.personal;
  let txt = `${p.name||'Name'}\n${p.title||''}\n`;
  if(p.email) txt+=`Email: ${p.email}\n`;
  if(p.phone) txt+=`Phone: ${p.phone}\n`;
  if(p.location) txt+=`Location: ${p.location}\n`;
  if(p.linkedin) txt+=`LinkedIn: ${p.linkedin}\n`;
  if(p.portfolio) txt+=`Portfolio: ${p.portfolio}\n`;
  if(state.summary) txt+=`\nSUMMARY\n${state.summary}\n`;
  if(state.experience.length){
    txt+=`\nEXPERIENCE\n`;
    state.experience.forEach(e=>{
      txt+=`${e.title} at ${e.company} (${e.start}–${e.end||'Present'})\n`;
      e.bullets.filter(b=>b).forEach(b=>txt+=`  • ${b}\n`);
    });
  }
  if(state.education.length){
    txt+=`\nEDUCATION\n`;
    state.education.forEach(e=>txt+=`${e.degree} – ${e.institution} (${e.start}–${e.end||''})\n`);
  }
  if(state.skills.length) txt+=`\nSKILLS\n${state.skills.map(s=>s.name).join(', ')}\n`;
  if(state.projects.length){
    txt+=`\nPROJECTS\n`;
    state.projects.forEach(p=>txt+=`${p.name}: ${p.desc}\n`);
  }
  const blob = new Blob([txt],{type:'text/plain'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = `${(p.name||'resume').replace(/\s+/g,'_')}_resume.txt`;
  a.click();
  showToast('📋 Text file downloaded!');
}

// ── SAVE / LOAD ──
function saveState() {
  try { localStorage.setItem('resumeBuilderState', JSON.stringify(state)); } catch(e){}
}

function loadState() {
  try {
    const saved = localStorage.getItem('resumeBuilderState');
    if(saved) {
      const s = JSON.parse(saved);
      Object.assign(state, s);
      // Restore inputs
      const p = state.personal||{};
      const fields = ['name','title','email','phone','location','linkedin','portfolio'];
      fields.forEach(f=>{ const el=document.getElementById('f-'+f); if(el&&p[f]) el.value=p[f]; });
      const sum = document.getElementById('f-summary');
      if(sum&&state.summary) sum.value = state.summary;
      // Restore template
      document.getElementById('resume').className = 'tpl-'+(state.template||'modern');
      document.querySelectorAll('.template-card').forEach(c=>c.classList.toggle('selected',c.dataset.tpl===state.template));
      showToast('💾 Progress restored!');
    }
  } catch(e){}
}

function clearAll() {
  if(!confirm('Clear all resume data? This cannot be undone.')) return;
  localStorage.removeItem('resumeBuilderState');
  location.reload();
}

// ── TOAST ──
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'), 2500);
}

// ── UTILS ──
function esc(s) {
  if(!s) return '';
  return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}

// ── START ──
init();
</script>
<script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/jspdf@2.5.1/dist/jspdf.umd.min.js"></script>
</body>
</html>
