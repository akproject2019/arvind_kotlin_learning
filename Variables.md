<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kotlin Variables — Complete Guide</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600&family=Google+Sans:wght@400;500;700&family=Google+Sans+Display:wght@700&display=swap" rel="stylesheet">
<link href="https://fonts.googleapis.com/icon?family=Material+Icons+Round" rel="stylesheet">
<style>
  :root {
    --md-primary: #7C4DFF;
    --md-primary-dark: #6200EA;
    --md-primary-light: #EDE7F6;
    --md-secondary: #03DAC6;
    --md-secondary-dark: #018786;
    --md-surface: #FFFFFF;
    --md-bg: #F3EFF8;
    --md-on-surface: #1C1B1F;
    --md-on-surface-variant: #49454F;
    --md-outline: #CAC4D0;
    --md-outline-variant: #E7E0EC;
    --md-error: #B3261E;
    --md-error-bg: #FDECEA;
    --md-success: #146C2E;
    --md-success-bg: #E6F4EA;
    --md-warning: #7C5700;
    --md-warning-bg: #FFF8E1;
    --md-info: #004E8C;
    --md-info-bg: #E3F2FD;
    --md-val-color: #6200EA;
    --md-val-bg: #EDE7F6;
    --md-var-color: #C62828;
    --md-var-bg: #FDECEA;
    --md-const-color: #1B5E20;
    --md-const-bg: #E8F5E9;
    --md-kotlin-purple: #7C4DFF;
    --shadow-sm: 0 1px 2px rgba(0,0,0,0.08), 0 1px 4px rgba(0,0,0,0.04);
    --shadow-md: 0 2px 8px rgba(0,0,0,0.10), 0 1px 3px rgba(0,0,0,0.06);
    --shadow-lg: 0 4px 20px rgba(0,0,0,0.12), 0 2px 8px rgba(0,0,0,0.08);
    --radius-sm: 8px;
    --radius-md: 12px;
    --radius-lg: 16px;
    --radius-xl: 28px;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'Google Sans', 'Segoe UI', sans-serif;
    background: var(--md-bg);
    color: var(--md-on-surface);
    line-height: 1.6;
    font-size: 15px;
  }

  /* ── HERO ── */
  .hero {
    background: linear-gradient(135deg, #4A148C 0%, #7C4DFF 50%, #651FFF 100%);
    padding: 64px 24px 80px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  .hero::before {
    content: '';
    position: absolute; inset: 0;
    background: radial-gradient(ellipse 80% 60% at 50% 110%, rgba(3,218,198,0.25) 0%, transparent 70%);
  }
  .hero-badge {
    display: inline-flex; align-items: center; gap: 6px;
    background: rgba(255,255,255,0.15);
    border: 1px solid rgba(255,255,255,0.3);
    border-radius: 100px;
    padding: 6px 16px;
    font-size: 13px;
    color: rgba(255,255,255,0.9);
    margin-bottom: 24px;
    backdrop-filter: blur(8px);
  }
  .hero h1 {
    font-family: 'Google Sans Display', 'Google Sans', sans-serif;
    font-size: clamp(32px, 5vw, 52px);
    font-weight: 700;
    color: #FFFFFF;
    margin-bottom: 16px;
    letter-spacing: -0.5px;
    position: relative;
  }
  .hero h1 span { color: #B39DDB; }
  .hero p {
    font-size: 17px;
    color: rgba(255,255,255,0.75);
    max-width: 540px;
    margin: 0 auto 32px;
    position: relative;
  }
  .hero-stats {
    display: flex; justify-content: center; gap: 32px;
    flex-wrap: wrap;
    position: relative;
  }
  .hero-stat {
    text-align: center;
  }
  .hero-stat .num {
    font-size: 28px; font-weight: 700; color: #FFFFFF;
    display: block;
  }
  .hero-stat .lbl {
    font-size: 12px; color: rgba(255,255,255,0.6);
    text-transform: uppercase; letter-spacing: 1px;
  }

  /* ── LAYOUT ── */
  .container {
    max-width: 960px;
    margin: 0 auto;
    padding: 0 20px;
  }

  /* ── NAVIGATION PILLS ── */
  .nav-rail {
    background: #FFFFFF;
    border-bottom: 1px solid var(--md-outline-variant);
    position: sticky;
    top: 0;
    z-index: 100;
    box-shadow: var(--shadow-sm);
  }
  .nav-scroll {
    display: flex; gap: 4px;
    overflow-x: auto;
    padding: 10px 20px;
    scrollbar-width: none;
    max-width: 960px;
    margin: 0 auto;
  }
  .nav-scroll::-webkit-scrollbar { display: none; }
  .nav-pill {
    display: inline-flex; align-items: center; gap: 4px;
    padding: 6px 14px;
    border-radius: 100px;
    font-size: 13px;
    font-weight: 500;
    white-space: nowrap;
    color: var(--md-on-surface-variant);
    cursor: pointer;
    transition: all 0.2s;
    text-decoration: none;
    border: 1px solid transparent;
  }
  .nav-pill:hover {
    background: var(--md-primary-light);
    color: var(--md-primary-dark);
  }
  .nav-pill.active {
    background: var(--md-primary-light);
    color: var(--md-primary-dark);
    border-color: var(--md-primary);
    font-weight: 600;
  }

  /* ── SECTIONS ── */
  .section {
    padding: 40px 0 16px;
  }
  .section-title {
    display: flex; align-items: center; gap: 12px;
    margin-bottom: 20px;
  }
  .section-icon {
    width: 40px; height: 40px;
    border-radius: var(--radius-sm);
    background: var(--md-primary-light);
    display: flex; align-items: center; justify-content: center;
    color: var(--md-primary-dark);
    font-size: 20px;
    flex-shrink: 0;
  }
  .section-title h2 {
    font-family: 'Google Sans Display', 'Google Sans', sans-serif;
    font-size: 22px;
    font-weight: 700;
    color: var(--md-on-surface);
  }

  /* ── CARDS ── */
  .card {
    background: #FFFFFF;
    border-radius: var(--radius-lg);
    box-shadow: var(--shadow-sm);
    border: 1px solid var(--md-outline-variant);
    overflow: hidden;
    margin-bottom: 16px;
  }
  .card-header {
    padding: 16px 20px 12px;
    border-bottom: 1px solid var(--md-outline-variant);
    display: flex; align-items: center; gap: 10px;
  }
  .card-body { padding: 20px; }

  /* ── DEFINITION CARD ── */
  .def-card {
    background: linear-gradient(135deg, #F3E5F5, #EDE7F6);
    border-radius: var(--radius-lg);
    padding: 28px;
    border: 1px solid #CE93D8;
    margin-bottom: 16px;
    position: relative;
    overflow: hidden;
  }
  .def-card::before {
    content: '{ }';
    position: absolute;
    right: 24px; top: 50%; transform: translateY(-50%);
    font-family: 'JetBrains Mono', monospace;
    font-size: 80px;
    color: rgba(124,77,255,0.08);
    font-weight: 700;
    pointer-events: none;
  }
  .def-card p {
    font-size: 16px;
    line-height: 1.7;
    color: #311B92;
    margin-bottom: 16px;
  }
  .def-props {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
    gap: 10px;
  }
  .def-prop {
    background: rgba(255,255,255,0.65);
    border-radius: var(--radius-sm);
    padding: 10px 14px;
    border: 1px solid rgba(124,77,255,0.2);
  }
  .def-prop .prop-key {
    font-size: 11px;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.8px;
    color: var(--md-primary-dark);
    margin-bottom: 2px;
  }
  .def-prop .prop-val {
    font-size: 13px;
    color: #4A148C;
    font-weight: 500;
  }

  /* ── VAL / VAR COMPARISON ── */
  .compare-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin-bottom: 16px;
  }
  @media(max-width: 600px) { .compare-grid { grid-template-columns: 1fr; } }

  .val-card {
    background: linear-gradient(135deg, #EDE7F6, #E8EAF6);
    border-radius: var(--radius-lg);
    padding: 24px;
    border: 2px solid #7C4DFF;
    position: relative;
  }
  .var-card {
    background: linear-gradient(135deg, #FDECEA, #FFF3E0);
    border-radius: var(--radius-lg);
    padding: 24px;
    border: 2px solid #EF5350;
    position: relative;
  }
  .type-badge {
    display: inline-flex; align-items: center;
    border-radius: 100px;
    padding: 4px 14px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 15px;
    font-weight: 600;
    margin-bottom: 12px;
  }
  .val-badge { background: #7C4DFF; color: white; }
  .var-badge { background: #EF5350; color: white; }
  .type-meaning {
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: 600;
    margin-bottom: 8px;
    opacity: 0.7;
  }
  .val-card .type-meaning { color: #4A148C; }
  .var-card .type-meaning { color: #7F0000; }
  .type-desc {
    font-size: 13px;
    line-height: 1.6;
  }
  .val-card .type-desc { color: #311B92; }
  .var-card .type-desc { color: #7F0000; }
  .type-rule {
    display: flex; align-items: center; gap: 8px;
    margin-top: 12px;
    font-size: 12px;
    padding: 8px 12px;
    border-radius: var(--radius-sm);
    font-weight: 500;
  }
  .val-card .type-rule { background: rgba(124,77,255,0.12); color: #4A148C; }
  .var-card .type-rule { background: rgba(239,83,80,0.12); color: #7F0000; }

  /* ── CODE BLOCK ── */
  .code-block {
    background: #1E1E2E;
    border-radius: var(--radius-md);
    overflow: hidden;
    margin-bottom: 16px;
    box-shadow: var(--shadow-md);
  }
  .code-header {
    background: #2A2A3E;
    padding: 10px 16px;
    display: flex; align-items: center; justify-content: space-between;
  }
  .code-lang {
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    color: #B39DDB;
    font-weight: 600;
    display: flex; align-items: center; gap: 6px;
  }
  .code-dots { display: flex; gap: 6px; }
  .dot { width: 10px; height: 10px; border-radius: 50%; }
  .dot-red { background: #FF5F57; }
  .dot-yellow { background: #FEBC2E; }
  .dot-green { background: #28C840; }
  pre {
    padding: 20px;
    overflow-x: auto;
    margin: 0;
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    line-height: 1.7;
  }
  .k { color: #BD93F9; font-weight: 600; } /* keyword val/var */
  .kw { color: #FF79C6; } /* keyword */
  .ty { color: #8BE9FD; } /* type */
  .st { color: #F1FA8C; } /* string */
  .nm { color: #FFB86C; } /* number */
  .co { color: #6272A4; font-style: italic; } /* comment */
  .fn { color: #50FA7B; } /* function */
  .bo { color: #FF79C6; } /* boolean */
  .va { color: #F8F8F2; } /* variable name */
  .pu { color: #FF79C6; } /* punctuation */

  /* ── DATA TYPES TABLE ── */
  .types-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 12px;
    margin-bottom: 16px;
  }
  .type-chip {
    background: #FFFFFF;
    border-radius: var(--radius-md);
    padding: 14px 16px;
    border: 1px solid var(--md-outline-variant);
    box-shadow: var(--shadow-sm);
    transition: transform 0.15s, box-shadow 0.15s;
  }
  .type-chip:hover { transform: translateY(-2px); box-shadow: var(--shadow-md); }
  .type-chip .t-name {
    font-family: 'JetBrains Mono', monospace;
    font-size: 15px;
    font-weight: 600;
    color: var(--md-primary-dark);
    margin-bottom: 4px;
  }
  .type-chip .t-size {
    font-size: 11px;
    color: var(--md-on-surface-variant);
    margin-bottom: 8px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }
  .type-chip .t-example {
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    background: #F3EFF8;
    padding: 4px 8px;
    border-radius: 4px;
    color: #4A148C;
    display: inline-block;
  }

  /* ── WHY TABLE ── */
  .why-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
    gap: 12px;
    margin-bottom: 16px;
  }
  .why-card {
    background: #FFFFFF;
    border-radius: var(--radius-md);
    padding: 16px;
    border: 1px solid var(--md-outline-variant);
    box-shadow: var(--shadow-sm);
    display: flex; gap: 12px;
    align-items: flex-start;
  }
  .why-icon {
    width: 36px; height: 36px;
    border-radius: var(--radius-sm);
    display: flex; align-items: center; justify-content: center;
    font-size: 18px;
    flex-shrink: 0;
  }
  .why-title { font-size: 14px; font-weight: 600; color: var(--md-on-surface); margin-bottom: 3px; }
  .why-desc { font-size: 12px; color: var(--md-on-surface-variant); line-height: 1.5; }

  /* ── SCOPE VISUAL ── */
  .scope-container {
    background: #1E1E2E;
    border-radius: var(--radius-lg);
    padding: 24px;
    margin-bottom: 16px;
  }
  .scope-level {
    border: 2px solid;
    border-radius: var(--radius-sm);
    padding: 16px 16px 12px;
    margin-bottom: 12px;
    position: relative;
  }
  .scope-level:last-child { margin-bottom: 0; }
  .scope-label {
    position: absolute;
    top: -11px; left: 12px;
    padding: 2px 10px;
    border-radius: 100px;
    font-size: 11px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.8px;
  }
  .scope-1 { border-color: #BD93F9; }
  .scope-1 .scope-label { background: #BD93F9; color: #1E1E2E; }
  .scope-2 { border-color: #8BE9FD; }
  .scope-2 .scope-label { background: #8BE9FD; color: #1E1E2E; }
  .scope-3 { border-color: #50FA7B; }
  .scope-3 .scope-label { background: #50FA7B; color: #1E1E2E; }
  .scope-4 { border-color: #FFB86C; }
  .scope-4 .scope-label { background: #FFB86C; color: #1E1E2E; }
  .scope-code {
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    color: #F8F8F2;
    margin-top: 4px;
  }
  .scope-desc {
    font-size: 11px;
    margin-top: 6px;
    opacity: 0.65;
    font-family: 'JetBrains Mono', monospace;
  }
  .scope-1 .scope-desc { color: #BD93F9; }
  .scope-2 .scope-desc { color: #8BE9FD; }
  .scope-3 .scope-desc { color: #50FA7B; }
  .scope-4 .scope-desc { color: #FFB86C; }

  /* ── EXAMPLES SECTION ── */
  .category-tabs {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-bottom: 20px;
  }
  .cat-tab {
    display: flex; align-items: center; gap: 6px;
    padding: 8px 14px;
    border-radius: 100px;
    border: 1.5px solid var(--md-outline);
    background: white;
    font-size: 13px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s;
    color: var(--md-on-surface-variant);
  }
  .cat-tab:hover, .cat-tab.active {
    background: var(--md-primary-light);
    border-color: var(--md-primary);
    color: var(--md-primary-dark);
  }
  .cat-tab .cat-emoji { font-size: 14px; }
  .cat-tab .cat-count {
    background: var(--md-primary-light);
    color: var(--md-primary-dark);
    border-radius: 100px;
    padding: 0 6px;
    font-size: 11px;
    font-weight: 700;
  }
  .cat-tab.active .cat-count {
    background: var(--md-primary);
    color: white;
  }

  .example-panel { display: none; }
  .example-panel.active { display: block; }

  /* ── SPECIAL INFO CARDS ── */
  .info-banner {
    border-radius: var(--radius-md);
    padding: 14px 18px;
    margin-bottom: 12px;
    display: flex; align-items: flex-start; gap: 12px;
    border-left: 4px solid;
  }
  .info-banner.tip { background: var(--md-info-bg); border-color: #1976D2; }
  .info-banner.warn { background: var(--md-warning-bg); border-color: #F57C00; }
  .info-banner.error { background: var(--md-error-bg); border-color: #D32F2F; }
  .info-banner.success { background: var(--md-success-bg); border-color: #2E7D32; }
  .info-banner .ib-icon { font-size: 20px; flex-shrink: 0; }
  .info-banner .ib-title { font-size: 13px; font-weight: 600; margin-bottom: 2px; }
  .info-banner.tip .ib-title { color: #0D47A1; }
  .info-banner.warn .ib-title { color: #E65100; }
  .info-banner.error .ib-title { color: #B71C1C; }
  .info-banner.success .ib-title { color: #1B5E20; }
  .info-banner .ib-body { font-size: 13px; color: var(--md-on-surface-variant); line-height: 1.5; }

  /* ── NULLABLE ── */
  .nullable-ops {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 12px;
    margin-bottom: 16px;
  }
  .op-card {
    background: #FFFFFF;
    border-radius: var(--radius-md);
    padding: 16px;
    border: 1px solid var(--md-outline-variant);
    box-shadow: var(--shadow-sm);
  }
  .op-symbol {
    font-family: 'JetBrains Mono', monospace;
    font-size: 20px;
    font-weight: 700;
    color: var(--md-primary-dark);
    margin-bottom: 6px;
  }
  .op-name { font-size: 12px; font-weight: 600; margin-bottom: 4px; color: var(--md-on-surface); }
  .op-desc { font-size: 12px; color: var(--md-on-surface-variant); line-height: 1.5; }

  /* ── BEST PRACTICES ── */
  .practices-list { list-style: none; margin-bottom: 16px; }
  .practices-list li {
    display: flex; align-items: flex-start; gap: 12px;
    padding: 12px 16px;
    border-radius: var(--radius-sm);
    margin-bottom: 8px;
    border: 1px solid var(--md-outline-variant);
    background: #FFFFFF;
  }
  .practices-list li .prac-icon {
    width: 28px; height: 28px;
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-size: 16px;
    flex-shrink: 0;
    background: var(--md-success-bg);
    color: var(--md-success);
  }
  .practices-list li .prac-title { font-size: 14px; font-weight: 600; color: var(--md-on-surface); }
  .practices-list li .prac-example {
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    color: var(--md-on-surface-variant);
    margin-top: 2px;
  }

  /* ── MISTAKES ── */
  .mistake-card {
    background: #FFFFFF;
    border-radius: var(--radius-md);
    border: 1px solid var(--md-outline-variant);
    overflow: hidden;
    margin-bottom: 12px;
    box-shadow: var(--shadow-sm);
  }
  .mistake-header {
    background: #FDECEA;
    padding: 10px 16px;
    display: flex; align-items: center; gap: 10px;
    border-bottom: 1px solid #F5C6C6;
  }
  .mistake-num {
    background: #D32F2F;
    color: white;
    border-radius: 50%;
    width: 22px; height: 22px;
    display: flex; align-items: center; justify-content: center;
    font-size: 11px;
    font-weight: 700;
    flex-shrink: 0;
  }
  .mistake-title { font-size: 13px; font-weight: 600; color: #7F0000; }
  .mistake-body { padding: 16px; }

  /* ── CHEAT SHEET ── */
  .cheat-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 12px;
    margin-bottom: 16px;
  }
  .cheat-group {
    background: #FFFFFF;
    border-radius: var(--radius-md);
    overflow: hidden;
    border: 1px solid var(--md-outline-variant);
    box-shadow: var(--shadow-sm);
  }
  .cheat-group-header {
    padding: 10px 16px;
    font-size: 12px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.8px;
  }
  .cheat-row {
    display: flex;
    padding: 8px 16px;
    border-top: 1px solid var(--md-outline-variant);
    gap: 12px;
    align-items: center;
  }
  .cheat-key {
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    font-weight: 600;
    min-width: 90px;
    flex-shrink: 0;
  }
  .cheat-val { font-size: 13px; color: var(--md-on-surface-variant); }

  /* ── FOOTER ── */
  .footer {
    background: #2A1F3D;
    color: rgba(255,255,255,0.75);
    text-align: center;
    padding: 40px 24px;
    margin-top: 48px;
  }
  .footer-kotlin {
    font-size: 32px; font-weight: 700;
    color: #B39DDB;
    font-family: 'JetBrains Mono', monospace;
    margin-bottom: 8px;
  }
  .footer p { font-size: 13px; opacity: 0.6; }
  .footer-tags {
    display: flex; flex-wrap: wrap; justify-content: center; gap: 8px;
    margin: 16px 0;
  }
  .footer-tag {
    background: rgba(255,255,255,0.1);
    border-radius: 100px;
    padding: 4px 12px;
    font-size: 12px;
    color: rgba(255,255,255,0.7);
    font-family: 'JetBrains Mono', monospace;
  }

  /* ── UTIL ── */
  .text-secondary { color: var(--md-on-surface-variant); font-size: 14px; line-height: 1.7; margin-bottom: 16px; }
  .divider { height: 1px; background: var(--md-outline-variant); margin: 8px 0 24px; }
  .inline-code {
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    background: #F3EFF8;
    color: #4A148C;
    padding: 2px 6px;
    border-radius: 4px;
    border: 1px solid #CE93D8;
  }
  .chip {
    display: inline-flex; align-items: center;
    border-radius: 100px;
    padding: 3px 10px;
    font-size: 12px;
    font-weight: 600;
  }
  .chip-val { background: var(--md-val-bg); color: var(--md-val-color); }
  .chip-var { background: var(--md-var-bg); color: var(--md-var-color); }
  .chip-const { background: var(--md-const-bg); color: var(--md-const-color); }

  @media(max-width: 600px) {
    .hero { padding: 48px 16px 60px; }
    .hero-stats { gap: 20px; }
    .section { padding: 28px 0 8px; }
  }
</style>
</head>
<body>

<!-- HERO -->
<div class="hero">
  <div class="hero-badge">
    <span style="font-size:14px">⬡</span> Kotlin Programming · Variables Guide
  </div>
  <h1>Kotlin <span>Variables</span><br>Complete Guide</h1>
  <p>Master <code style="background:rgba(255,255,255,0.15);padding:2px 8px;border-radius:6px;color:white">val</code>, <code style="background:rgba(255,255,255,0.15);padding:2px 8px;border-radius:6px;color:white">var</code>, <code style="background:rgba(255,255,255,0.15);padding:2px 8px;border-radius:6px;color:white">const</code> &amp; <code style="background:rgba(255,255,255,0.15);padding:2px 8px;border-radius:6px;color:white">lateinit</code> with 100 real-world examples across 12 domains</p>
  <div class="hero-stats">
    <div class="hero-stat"><span class="num">100</span><span class="lbl">Examples</span></div>
    <div class="hero-stat"><span class="num">12</span><span class="lbl">Categories</span></div>
    <div class="hero-stat"><span class="num">16</span><span class="lbl">Topics</span></div>
    <div class="hero-stat"><span class="num">4</span><span class="lbl">Var Types</span></div>
  </div>
</div>

<!-- NAV -->
<div class="nav-rail">
  <div class="nav-scroll">
    <a class="nav-pill active" href="#definition">Definition</a>
    <a class="nav-pill" href="#why">Why &amp; When</a>
    <a class="nav-pill" href="#types">val vs var</a>
    <a class="nav-pill" href="#syntax">Syntax</a>
    <a class="nav-pill" href="#datatypes">Data Types</a>
    <a class="nav-pill" href="#examples">100 Examples</a>
    <a class="nav-pill" href="#nullable">Nullable</a>
    <a class="nav-pill" href="#lateinit">lateinit</a>
    <a class="nav-pill" href="#const">const val</a>
    <a class="nav-pill" href="#scope">Scope</a>
    <a class="nav-pill" href="#practices">Best Practices</a>
    <a class="nav-pill" href="#mistakes">Mistakes</a>
    <a class="nav-pill" href="#cheatsheet">Cheat Sheet</a>
  </div>
</div>

<div class="container">

  <!-- DEFINITION -->
  <div class="section" id="definition">
    <div class="section-title">
      <div class="section-icon"><span class="material-icons-round" style="font-size:20px">auto_stories</span></div>
      <h2>What is a Variable?</h2>
    </div>
    <div class="def-card">
      <p>A <strong>variable</strong> is a named container in memory that stores a value which can be referenced and manipulated throughout a program. Think of it as a labelled box that holds your data.</p>
      <div class="def-props">
        <div class="def-prop"><div class="prop-key">Name</div><div class="prop-val">Identifier</div></div>
        <div class="def-prop"><div class="prop-key">Type</div><div class="prop-val">Data Type</div></div>
        <div class="def-prop"><div class="prop-key">Value</div><div class="prop-val">Stored Data</div></div>
        <div class="def-prop"><div class="prop-key">Scope</div><div class="prop-val">Accessibility</div></div>
      </div>
    </div>
    <div class="code-block">
      <div class="code-header">
        <div class="code-lang">⬡ Kotlin</div>
        <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
      </div>
      <pre><span class="co">// General syntax</span>
<span class="k">var</span><span class="pu">/</span><span class="k">val</span> <span class="va">variableName</span><span class="pu">:</span> <span class="ty">DataType</span> <span class="pu">=</span> value

<span class="co">// Real examples</span>
<span class="k">val</span> <span class="va">userName</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Ravi Patel"</span>
<span class="k">var</span> <span class="va">userAge</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">25</span>
<span class="k">val</span> <span class="va">isActive</span><span class="pu">:</span> <span class="ty">Boolean</span> <span class="pu">=</span> <span class="bo">true</span></pre>
    </div>
  </div>

  <div class="divider"></div>

  <!-- WHY & WHEN -->
  <div class="section" id="why">
    <div class="section-title">
      <div class="section-icon"><span class="material-icons-round" style="font-size:20px">lightbulb</span></div>
      <h2>Why &amp; When to Use Variables</h2>
    </div>
    <div class="why-grid">
      <div class="why-card">
        <div class="why-icon" style="background:#E8F5E9;color:#2E7D32">💾</div>
        <div><div class="why-title">Store Data</div><div class="why-desc">Hold information temporarily or permanently during program execution</div></div>
      </div>
      <div class="why-card">
        <div class="why-icon" style="background:#E3F2FD;color:#1565C0">♻️</div>
        <div><div class="why-title">Reusability</div><div class="why-desc">Reference the same value multiple times without repetition</div></div>
      </div>
      <div class="why-card">
        <div class="why-icon" style="background:#FFF3E0;color:#E65100">🔄</div>
        <div><div class="why-title">Flexibility</div><div class="why-desc">Change values dynamically during program execution when needed</div></div>
      </div>
      <div class="why-card">
        <div class="why-icon" style="background:#F3E5F5;color:#6A1B9A">📖</div>
        <div><div class="why-title">Readability</div><div class="why-desc">Meaningful names make code self-documenting and easier to understand</div></div>
      </div>
      <div class="why-card">
        <div class="why-icon" style="background:#FCE4EC;color:#880E4F">🔧</div>
        <div><div class="why-title">Maintainability</div><div class="why-desc">Change a value in one place; it updates everywhere it's referenced</div></div>
      </div>
      <div class="why-card">
        <div class="why-icon" style="background:#E8EAF6;color:#283593">🛡️</div>
        <div><div class="why-title">Type Safety</div><div class="why-desc">Kotlin's type system prevents incorrect data from being assigned</div></div>
      </div>
    </div>
    <div class="info-banner tip">
      <span class="ib-icon">💡</span>
      <div><div class="ib-title">When to use a variable</div><div class="ib-body">Use a variable whenever you store user input, hold calculation results, track state that changes over time, pass data between functions, or need to give a meaningful name to a magic number or string.</div></div>
    </div>
  </div>

  <div class="divider"></div>

  <!-- VAL VS VAR -->
  <div class="section" id="types">
    <div class="section-title">
      <div class="section-icon"><span class="material-icons-round" style="font-size:20px">compare_arrows</span></div>
      <h2>val vs var</h2>
    </div>
    <div class="compare-grid">
      <div class="val-card">
        <div><span class="type-badge val-badge">val</span></div>
        <div class="type-meaning">Immutable · Assign once</div>
        <div class="type-desc">Stands for <strong>value</strong>. Once assigned, cannot be reassigned. Similar to <code class="inline-code">final</code> in Java. Preferred by Kotlin developers.</div>
        <div class="type-rule">🔒 Cannot be reassigned after initialization</div>
      </div>
      <div class="var-card">
        <div><span class="type-badge var-badge">var</span></div>
        <div class="type-meaning">Mutable · Reassign freely</div>
        <div class="type-desc">Stands for <strong>variable</strong>. Can be reassigned any number of times. Use when the value needs to change during execution.</div>
        <div class="type-rule">✏️ Can be reassigned any number of times</div>
      </div>
    </div>
    <div class="code-block">
      <div class="code-header">
        <div class="code-lang">⬡ Kotlin — val vs var</div>
        <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
      </div>
      <pre><span class="co">// ✅ val — immutable</span>
<span class="k">val</span> <span class="va">pi</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">3.14159</span>
<span class="k">val</span> <span class="va">appName</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"MyApp"</span>
<span class="co">// pi = 3.14  ❌ ERROR: Val cannot be reassigned</span>

<span class="co">// ✅ var — mutable</span>
<span class="k">var</span> <span class="va">score</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">0</span>
<span class="va">score</span> <span class="pu">=</span> <span class="nm">10</span>      <span class="co">// ✅ Allowed</span>
<span class="va">score</span> <span class="pu">+=</span> <span class="nm">5</span>      <span class="co">// ✅ Allowed</span>

<span class="k">var</span> <span class="va">userName</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Ravi"</span>
<span class="va">userName</span> <span class="pu">=</span> <span class="st">"Priya"</span>   <span class="co">// ✅ Allowed</span></pre>
    </div>
  </div>

  <div class="divider"></div>

  <!-- SYNTAX PATTERNS -->
  <div class="section" id="syntax">
    <div class="section-title">
      <div class="section-icon"><span class="material-icons-round" style="font-size:20px">code</span></div>
      <h2>Declaration Syntax Patterns</h2>
    </div>
    <div class="code-block">
      <div class="code-header">
        <div class="code-lang">⬡ Kotlin — All Declaration Patterns</div>
        <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
      </div>
      <pre><span class="co">// Pattern 1 — Full declaration with explicit type and value</span>
<span class="k">val</span> <span class="va">name</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Ankit"</span>

<span class="co">// Pattern 2 — Type inference (Kotlin infers the type)</span>
<span class="k">val</span> <span class="va">name</span>       <span class="pu">=</span> <span class="st">"Ankit"</span>     <span class="co">// String inferred</span>
<span class="k">val</span> <span class="va">age</span>        <span class="pu">=</span> <span class="nm">25</span>          <span class="co">// Int inferred</span>
<span class="k">val</span> <span class="va">price</span>      <span class="pu">=</span> <span class="nm">99.99</span>       <span class="co">// Double inferred</span>
<span class="k">val</span> <span class="va">isActive</span>   <span class="pu">=</span> <span class="bo">true</span>        <span class="co">// Boolean inferred</span>

<span class="co">// Pattern 3 — Declare first, assign later (var only)</span>
<span class="k">var</span> <span class="va">score</span><span class="pu">:</span> <span class="ty">Int</span>
<span class="va">score</span> <span class="pu">=</span> <span class="nm">100</span>

<span class="co">// Pattern 4 — Nullable variable</span>
<span class="k">var</span> <span class="va">email</span><span class="pu">:</span> <span class="ty">String</span><span class="pu">?</span> <span class="pu">=</span> <span class="bo">null</span>

<span class="co">// Pattern 5 — String template</span>
<span class="k">val</span> <span class="va">city</span> <span class="pu">=</span> <span class="st">"Surat"</span>
<span class="k">val</span> <span class="va">greeting</span> <span class="pu">=</span> <span class="st">"Welcome to <span style="color:#FFB86C">$city</span>, Gujarat!"</span>
<span class="k">val</span> <span class="va">info</span> <span class="pu">=</span> <span class="st">"Length: <span style="color:#FFB86C">${city.length}</span> chars"</span></pre>
    </div>
  </div>

  <div class="divider"></div>

  <!-- DATA TYPES -->
  <div class="section" id="datatypes">
    <div class="section-title">
      <div class="section-icon"><span class="material-icons-round" style="font-size:20px">category</span></div>
      <h2>Kotlin Data Types</h2>
    </div>
    <div class="types-grid">
      <div class="type-chip"><div class="t-name">Int</div><div class="t-size">32-bit integer</div><div class="t-example">val age = 25</div></div>
      <div class="type-chip"><div class="t-name">Long</div><div class="t-size">64-bit integer</div><div class="t-example">val dist = 9999L</div></div>
      <div class="type-chip"><div class="t-name">Short</div><div class="t-size">16-bit integer</div><div class="t-example">val floors: Short = 20</div></div>
      <div class="type-chip"><div class="t-name">Byte</div><div class="t-size">8-bit integer</div><div class="t-example">val flag: Byte = 1</div></div>
      <div class="type-chip"><div class="t-name">Float</div><div class="t-size">32-bit decimal</div><div class="t-example">val weight = 72.5F</div></div>
      <div class="type-chip"><div class="t-name">Double</div><div class="t-size">64-bit decimal</div><div class="t-example">val pi = 3.14159</div></div>
      <div class="type-chip"><div class="t-name">Boolean</div><div class="t-size">true / false</div><div class="t-example">val active = true</div></div>
      <div class="type-chip"><div class="t-name">Char</div><div class="t-size">single character</div><div class="t-example">val grade = 'A'</div></div>
      <div class="type-chip"><div class="t-name">String</div><div class="t-size">text sequence</div><div class="t-example">val name = "Kotlin"</div></div>
    </div>
  </div>

  <div class="divider"></div>

  <!-- 100 EXAMPLES -->
  <div class="section" id="examples">
    <div class="section-title">
      <div class="section-icon"><span class="material-icons-round" style="font-size:20px">format_list_numbered</span></div>
      <h2>100 Real-Life Examples</h2>
    </div>
    <p class="text-secondary">Click a category to explore real-world Kotlin variable usage across 12 different domains.</p>

    <div class="category-tabs">
      <div class="cat-tab active" onclick="showCat('cat1',this)"><span class="cat-emoji">🧑‍💼</span> User Profile <span class="cat-count">10</span></div>
      <div class="cat-tab" onclick="showCat('cat2',this)"><span class="cat-emoji">🛒</span> E-Commerce <span class="cat-count">10</span></div>
      <div class="cat-tab" onclick="showCat('cat3',this)"><span class="cat-emoji">🏦</span> Banking <span class="cat-count">10</span></div>
      <div class="cat-tab" onclick="showCat('cat4',this)"><span class="cat-emoji">🏥</span> Healthcare <span class="cat-count">8</span></div>
      <div class="cat-tab" onclick="showCat('cat5',this)"><span class="cat-emoji">🎮</span> Gaming <span class="cat-count">8</span></div>
      <div class="cat-tab" onclick="showCat('cat6',this)"><span class="cat-emoji">📱</span> App Settings <span class="cat-count">8</span></div>
      <div class="cat-tab" onclick="showCat('cat7',this)"><span class="cat-emoji">🌦️</span> Weather <span class="cat-count">7</span></div>
      <div class="cat-tab" onclick="showCat('cat8',this)"><span class="cat-emoji">🚗</span> Navigation <span class="cat-count">7</span></div>
      <div class="cat-tab" onclick="showCat('cat9',this)"><span class="cat-emoji">📚</span> Education <span class="cat-count">8</span></div>
      <div class="cat-tab" onclick="showCat('cat10',this)"><span class="cat-emoji">🍕</span> Food Delivery <span class="cat-count">7</span></div>
      <div class="cat-tab" onclick="showCat('cat11',this)"><span class="cat-emoji">💬</span> Social <span class="cat-count">7</span></div>
      <div class="cat-tab" onclick="showCat('cat12',this)"><span class="cat-emoji">🏗️</span> System/Device <span class="cat-count">10</span></div>
    </div>

    <!-- Category 1 -->
    <div class="example-panel active" id="cat1">
      <div class="code-block">
        <div class="code-header">
          <div class="code-lang">🧑‍💼 User Profile &amp; Authentication — Examples 1–10</div>
          <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
        </div>
        <pre><span class="co">// 1. User's full name</span>
<span class="k">val</span> <span class="va">fullName</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Ravi Kumar Sharma"</span>

<span class="co">// 2. User's age</span>
<span class="k">var</span> <span class="va">age</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">28</span>

<span class="co">// 3. User's email address</span>
<span class="k">var</span> <span class="va">email</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"ravi.sharma@gmail.com"</span>

<span class="co">// 4. User's phone number</span>
<span class="k">val</span> <span class="va">phoneNumber</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"+91-9876543210"</span>

<span class="co">// 5. Is user logged in?</span>
<span class="k">var</span> <span class="va">isLoggedIn</span><span class="pu">:</span> <span class="ty">Boolean</span> <span class="pu">=</span> <span class="bo">false</span>

<span class="co">// 6. User's profile picture URL</span>
<span class="k">var</span> <span class="va">profilePicUrl</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"https://cdn.example.com/users/ravi.jpg"</span>

<span class="co">// 7. Account creation timestamp</span>
<span class="k">val</span> <span class="va">accountCreatedAt</span><span class="pu">:</span> <span class="ty">Long</span> <span class="pu">=</span> <span class="nm">1700000000000L</span>

<span class="co">// 8. Number of login attempts</span>
<span class="k">var</span> <span class="va">loginAttempts</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">0</span>

<span class="co">// 9. User's role</span>
<span class="k">val</span> <span class="va">userRole</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"ADMIN"</span>

<span class="co">// 10. Is account email verified?</span>
<span class="k">var</span> <span class="va">isEmailVerified</span><span class="pu">:</span> <span class="ty">Boolean</span> <span class="pu">=</span> <span class="bo">false</span></pre>
      </div>
    </div>

    <!-- Category 2 -->
    <div class="example-panel" id="cat2">
      <div class="code-block">
        <div class="code-header">
          <div class="code-lang">🛒 E-Commerce &amp; Shopping — Examples 11–20</div>
          <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
        </div>
        <pre><span class="co">// 11. Product name</span>
<span class="k">val</span> <span class="va">productName</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"OnePlus 12 Smartphone"</span>

<span class="co">// 12. Product price</span>
<span class="k">var</span> <span class="va">productPrice</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">64999.00</span>

<span class="co">// 13. Discounted price calculation</span>
<span class="k">val</span> <span class="va">discountPercent</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">15</span>
<span class="k">val</span> <span class="va">discountedPrice</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> productPrice <span class="pu">-</span> (productPrice <span class="pu">*</span> discountPercent <span class="pu">/</span> <span class="nm">100</span>)

<span class="co">// 14. Items in cart</span>
<span class="k">var</span> <span class="va">cartItemCount</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">3</span>

<span class="co">// 15. Total cart value</span>
<span class="k">var</span> <span class="va">cartTotal</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">12499.97</span>

<span class="co">// 16. Is product in stock?</span>
<span class="k">var</span> <span class="va">isInStock</span><span class="pu">:</span> <span class="ty">Boolean</span> <span class="pu">=</span> <span class="bo">true</span>

<span class="co">// 17. Product rating out of 5</span>
<span class="k">val</span> <span class="va">productRating</span><span class="pu">:</span> <span class="ty">Float</span> <span class="pu">=</span> <span class="nm">4.5F</span>

<span class="co">// 18. Number of customer reviews</span>
<span class="k">val</span> <span class="va">reviewCount</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">2847</span>

<span class="co">// 19. Estimated delivery in days</span>
<span class="k">val</span> <span class="va">estimatedDeliveryDays</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">3</span>

<span class="co">// 20. Applied coupon code</span>
<span class="k">var</span> <span class="va">appliedCouponCode</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"SAVE200"</span></pre>
      </div>
    </div>

    <!-- Category 3 -->
    <div class="example-panel" id="cat3">
      <div class="code-block">
        <div class="code-header">
          <div class="code-lang">🏦 Banking &amp; Finance — Examples 21–30</div>
          <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
        </div>
        <pre><span class="co">// 21. Bank account number</span>
<span class="k">val</span> <span class="va">accountNumber</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"9876543210123456"</span>

<span class="co">// 22. Account balance</span>
<span class="k">var</span> <span class="va">accountBalance</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">45230.75</span>

<span class="co">// 23. Minimum balance required</span>
<span class="k">val</span> <span class="va">minimumBalance</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">1000.00</span>

<span class="co">// 24. Transaction amount</span>
<span class="k">val</span> <span class="va">transactionAmount</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">5000.00</span>

<span class="co">// 25. Annual interest rate</span>
<span class="k">val</span> <span class="va">annualInterestRate</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">7.5</span>

<span class="co">// 26. Loan amount</span>
<span class="k">val</span> <span class="va">loanAmount</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">500000.00</span>

<span class="co">// 27. Monthly EMI amount</span>
<span class="k">val</span> <span class="va">emiAmount</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">9805.50</span>

<span class="co">// 28. Number of EMI months</span>
<span class="k">val</span> <span class="va">emiMonths</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">60</span>

<span class="co">// 29. IFSC code</span>
<span class="k">val</span> <span class="va">ifscCode</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"SBIN0001234"</span>

<span class="co">// 30. UPI ID</span>
<span class="k">val</span> <span class="va">upiId</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"ravi@upi"</span></pre>
      </div>
    </div>

    <!-- Category 4 -->
    <div class="example-panel" id="cat4">
      <div class="code-block">
        <div class="code-header">
          <div class="code-lang">🏥 Healthcare &amp; Medical — Examples 31–38</div>
          <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
        </div>
        <pre><span class="co">// 31. Patient name</span>
<span class="k">val</span> <span class="va">patientName</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Sunita Devi"</span>

<span class="co">// 32. Patient age</span>
<span class="k">val</span> <span class="va">patientAge</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">45</span>

<span class="co">// 33. Blood group</span>
<span class="k">val</span> <span class="va">bloodGroup</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"B+"</span>

<span class="co">// 34. Body temperature in Celsius</span>
<span class="k">var</span> <span class="va">bodyTemperature</span><span class="pu">:</span> <span class="ty">Float</span> <span class="pu">=</span> <span class="nm">37.0F</span>

<span class="co">// 35. Heart rate in BPM</span>
<span class="k">var</span> <span class="va">heartRate</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">72</span>

<span class="co">// 36. Systolic blood pressure</span>
<span class="k">var</span> <span class="va">systolicBP</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">120</span>

<span class="co">// 37. Body weight in kg</span>
<span class="k">var</span> <span class="va">bodyWeight</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">68.5</span>

<span class="co">// 38. Height in centimeters</span>
<span class="k">val</span> <span class="va">heightCm</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">165</span></pre>
      </div>
    </div>

    <!-- Category 5 -->
    <div class="example-panel" id="cat5">
      <div class="code-block">
        <div class="code-header">
          <div class="code-lang">🎮 Gaming — Examples 39–46</div>
          <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
        </div>
        <pre><span class="co">// 39. Player display name</span>
<span class="k">var</span> <span class="va">playerName</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"NinjaWarrior99"</span>

<span class="co">// 40. Player experience level</span>
<span class="k">var</span> <span class="va">playerLevel</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">42</span>

<span class="co">// 41. Current game score</span>
<span class="k">var</span> <span class="va">currentScore</span><span class="pu">:</span> <span class="ty">Long</span> <span class="pu">=</span> <span class="nm">985600L</span>

<span class="co">// 42. All-time high score</span>
<span class="k">var</span> <span class="va">highScore</span><span class="pu">:</span> <span class="ty">Long</span> <span class="pu">=</span> <span class="nm">1200000L</span>

<span class="co">// 43. Lives remaining</span>
<span class="k">var</span> <span class="va">livesRemaining</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">3</span>

<span class="co">// 44. In-game coin count</span>
<span class="k">var</span> <span class="va">coinCount</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">2500</span>

<span class="co">// 45. Is game paused?</span>
<span class="k">var</span> <span class="va">isGamePaused</span><span class="pu">:</span> <span class="ty">Boolean</span> <span class="pu">=</span> <span class="bo">false</span>

<span class="co">// 46. Current level name</span>
<span class="k">val</span> <span class="va">currentLevel</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Dark Forest - Stage 7"</span></pre>
      </div>
    </div>

    <!-- Category 6 -->
    <div class="example-panel" id="cat6">
      <div class="code-block">
        <div class="code-header">
          <div class="code-lang">📱 Mobile App Settings — Examples 47–54</div>
          <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
        </div>
        <pre><span class="co">// 47. App theme: dark or light</span>
<span class="k">var</span> <span class="va">appTheme</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"DARK"</span>

<span class="co">// 48. Push notifications enabled?</span>
<span class="k">var</span> <span class="va">notificationsEnabled</span><span class="pu">:</span> <span class="ty">Boolean</span> <span class="pu">=</span> <span class="bo">true</span>

<span class="co">// 49. App display language</span>
<span class="k">var</span> <span class="va">appLanguage</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"hi"</span> <span class="co">// Hindi</span>

<span class="co">// 50. Font size in sp</span>
<span class="k">var</span> <span class="va">fontSize</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">16</span>

<span class="co">// 51. Auto-save feature enabled?</span>
<span class="k">var</span> <span class="va">autoSaveEnabled</span><span class="pu">:</span> <span class="ty">Boolean</span> <span class="pu">=</span> <span class="bo">true</span>

<span class="co">// 52. App semantic version</span>
<span class="k">val</span> <span class="va">appVersion</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"3.2.1"</span>

<span class="co">// 53. Internal build number</span>
<span class="k">val</span> <span class="va">buildNumber</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">1045</span>

<span class="co">// 54. Screen brightness (0–100%)</span>
<span class="k">var</span> <span class="va">screenBrightness</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">70</span></pre>
      </div>
    </div>

    <!-- Category 7 -->
    <div class="example-panel" id="cat7">
      <div class="code-block">
        <div class="code-header">
          <div class="code-lang">🌦️ Weather App — Examples 55–61</div>
          <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
        </div>
        <pre><span class="co">// 55. Current city name</span>
<span class="k">val</span> <span class="va">cityName</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Surat"</span>

<span class="co">// 56. Current temperature in Celsius</span>
<span class="k">var</span> <span class="va">currentTemperature</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">34.5</span>

<span class="co">// 57. Relative humidity percentage</span>
<span class="k">var</span> <span class="va">humidityPercent</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">68</span>

<span class="co">// 58. Wind speed in km/h</span>
<span class="k">var</span> <span class="va">windSpeedKmh</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">18.3</span>

<span class="co">// 59. Weather condition label</span>
<span class="k">var</span> <span class="va">weatherCondition</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Partly Cloudy"</span>

<span class="co">// 60. UV index (0–11+)</span>
<span class="k">var</span> <span class="va">uvIndex</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">8</span>

<span class="co">// 61. Feels-like temperature</span>
<span class="k">var</span> <span class="va">feelsLikeTemperature</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">38.2</span></pre>
      </div>
    </div>

    <!-- Category 8 -->
    <div class="example-panel" id="cat8">
      <div class="code-block">
        <div class="code-header">
          <div class="code-lang">🚗 Transportation &amp; Navigation — Examples 62–68</div>
          <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
        </div>
        <pre><span class="co">// 62. Vehicle registration plate</span>
<span class="k">val</span> <span class="va">vehicleRegNumber</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"GJ05AB1234"</span>

<span class="co">// 63. Real-time speed (km/h)</span>
<span class="k">var</span> <span class="va">currentSpeedKmh</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">65</span>

<span class="co">// 64. Posted speed limit</span>
<span class="k">val</span> <span class="va">speedLimitKmh</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">80</span>

<span class="co">// 65. Fuel tank level (0–100%)</span>
<span class="k">var</span> <span class="va">fuelLevelPercent</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">55</span>

<span class="co">// 66. Remaining distance to destination</span>
<span class="k">var</span> <span class="va">distanceToDestinationKm</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">12.7</span>

<span class="co">// 67. GPS current latitude</span>
<span class="k">var</span> <span class="va">currentLatitude</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">21.1702</span>

<span class="co">// 68. GPS current longitude</span>
<span class="k">var</span> <span class="va">currentLongitude</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">72.8311</span></pre>
      </div>
    </div>

    <!-- Category 9 -->
    <div class="example-panel" id="cat9">
      <div class="code-block">
        <div class="code-header">
          <div class="code-lang">📚 Education &amp; Students — Examples 69–76</div>
          <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
        </div>
        <pre><span class="co">// 69. Student full name</span>
<span class="k">val</span> <span class="va">studentName</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Arjun Mehta"</span>

<span class="co">// 70. Enrollment roll number</span>
<span class="k">val</span> <span class="va">rollNumber</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"CS2021045"</span>

<span class="co">// 71. Current semester</span>
<span class="k">var</span> <span class="va">currentSemester</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">5</span>

<span class="co">// 72. Cumulative GPA</span>
<span class="k">var</span> <span class="va">cgpa</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">8.75</span>

<span class="co">// 73. Attendance percentage</span>
<span class="k">var</span> <span class="va">attendancePercent</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">87.5</span>

<span class="co">// 74. Subject name</span>
<span class="k">val</span> <span class="va">subjectName</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Data Structures &amp; Algorithms"</span>

<span class="co">// 75. Marks obtained in exam</span>
<span class="k">var</span> <span class="va">marksObtained</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">87</span>

<span class="co">// 76. Total marks available</span>
<span class="k">val</span> <span class="va">totalMarks</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">100</span></pre>
      </div>
    </div>

    <!-- Category 10 -->
    <div class="example-panel" id="cat10">
      <div class="code-block">
        <div class="code-header">
          <div class="code-lang">🍕 Food Delivery App — Examples 77–83</div>
          <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
        </div>
        <pre><span class="co">// 77. Restaurant name</span>
<span class="k">val</span> <span class="va">restaurantName</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Spice Garden"</span>

<span class="co">// 78. Ordered food item</span>
<span class="k">val</span> <span class="va">foodItemName</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Paneer Butter Masala"</span>

<span class="co">// 79. Item price in INR</span>
<span class="k">val</span> <span class="va">foodItemPrice</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">280.00</span>

<span class="co">// 80. Quantity ordered</span>
<span class="k">var</span> <span class="va">orderQuantity</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">2</span>

<span class="co">// 81. Delivery charge</span>
<span class="k">val</span> <span class="va">deliveryFee</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">30.00</span>

<span class="co">// 82. Estimated delivery in minutes</span>
<span class="k">var</span> <span class="va">estimatedDeliveryMinutes</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">35</span>

<span class="co">// 83. Current order status</span>
<span class="k">var</span> <span class="va">orderStatus</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Preparing"</span></pre>
      </div>
    </div>

    <!-- Category 11 -->
    <div class="example-panel" id="cat11">
      <div class="code-block">
        <div class="code-header">
          <div class="code-lang">💬 Messaging &amp; Social — Examples 84–90</div>
          <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
        </div>
        <pre><span class="co">// 84. Message text content</span>
<span class="k">val</span> <span class="va">messageContent</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Hey! Are you free this weekend?"</span>

<span class="co">// 85. Message sent timestamp</span>
<span class="k">val</span> <span class="va">messageTimestamp</span><span class="pu">:</span> <span class="ty">Long</span> <span class="pu">=</span> <span class="fn">System</span><span class="pu">.</span><span class="fn">currentTimeMillis</span><span class="pu">()</span>

<span class="co">// 86. Has the message been read?</span>
<span class="k">var</span> <span class="va">isMessageRead</span><span class="pu">:</span> <span class="ty">Boolean</span> <span class="pu">=</span> <span class="bo">false</span>

<span class="co">// 87. Number of unread messages</span>
<span class="k">var</span> <span class="va">unreadMessageCount</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">5</span>

<span class="co">// 88. Follower count</span>
<span class="k">var</span> <span class="va">followerCount</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">1234</span>

<span class="co">// 89. Following count</span>
<span class="k">var</span> <span class="va">followingCount</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">367</span>

<span class="co">// 90. Post total likes</span>
<span class="k">var</span> <span class="va">likeCount</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">892</span></pre>
      </div>
    </div>

    <!-- Category 12 -->
    <div class="example-panel" id="cat12">
      <div class="code-block">
        <div class="code-header">
          <div class="code-lang">🏗️ System &amp; Device Info — Examples 91–100</div>
          <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
        </div>
        <pre><span class="co">// 91. Device model name</span>
<span class="k">val</span> <span class="va">deviceModel</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Samsung Galaxy S24"</span>

<span class="co">// 92. Operating system version</span>
<span class="k">val</span> <span class="va">osVersion</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Android 14"</span>

<span class="co">// 93. Battery level (0–100%)</span>
<span class="k">var</span> <span class="va">batteryPercent</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">73</span>

<span class="co">// 94. Available internal storage in MB</span>
<span class="k">var</span> <span class="va">availableStorageMB</span><span class="pu">:</span> <span class="ty">Long</span> <span class="pu">=</span> <span class="nm">32768L</span>

<span class="co">// 95. Total device RAM in MB</span>
<span class="k">val</span> <span class="va">totalRamMB</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">8192</span>

<span class="co">// 96. Active network type</span>
<span class="k">var</span> <span class="va">networkType</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"5G"</span>

<span class="co">// 97. Signal strength in dBm</span>
<span class="k">var</span> <span class="va">signalStrengthDbm</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">-75</span>

<span class="co">// 98. Screen resolution — width</span>
<span class="k">val</span> <span class="va">screenWidthPx</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">1080</span>

<span class="co">// 99. Screen resolution — height</span>
<span class="k">val</span> <span class="va">screenHeightPx</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">2400</span>

<span class="co">// 100. Is device currently charging?</span>
<span class="k">var</span> <span class="va">isCharging</span><span class="pu">:</span> <span class="ty">Boolean</span> <span class="pu">=</span> <span class="bo">true</span></pre>
      </div>
    </div>
  </div>

  <div class="divider"></div>

  <!-- NULLABLE -->
  <div class="section" id="nullable">
    <div class="section-title">
      <div class="section-icon"><span class="material-icons-round" style="font-size:20px">help_outline</span></div>
      <h2>Nullable Variables</h2>
    </div>
    <p class="text-secondary">In Kotlin, variables <strong>cannot hold null by default</strong>. Append <span class="inline-code">?</span> to a type to make it nullable.</p>
    <div class="nullable-ops">
      <div class="op-card"><div class="op-symbol">?</div><div class="op-name">Nullable Type</div><div class="op-desc">Declare a variable that may hold null: <span class="inline-code">String?</span></div></div>
      <div class="op-card"><div class="op-symbol">?.</div><div class="op-name">Safe Call</div><div class="op-desc">Access property only if non-null, else returns null</div></div>
      <div class="op-card"><div class="op-symbol">?:</div><div class="op-name">Elvis Operator</div><div class="op-desc">Provide a default value when the expression is null</div></div>
      <div class="op-card"><div class="op-symbol">!!</div><div class="op-name">Non-null Assert</div><div class="op-desc">Force unwrap — throws NPE if value is actually null</div></div>
    </div>
    <div class="code-block">
      <div class="code-header">
        <div class="code-lang">⬡ Kotlin — Nullable Variables</div>
        <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
      </div>
      <pre><span class="co">// Non-nullable — cannot hold null</span>
<span class="k">var</span> <span class="va">userName</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Ravi"</span>
<span class="co">// userName = null  ❌ Compile error</span>

<span class="co">// Nullable — can hold null</span>
<span class="k">var</span> <span class="va">email</span><span class="pu">:</span> <span class="ty">String?</span> <span class="pu">=</span> <span class="bo">null</span>
<span class="va">email</span> <span class="pu">=</span> <span class="st">"ravi@gmail.com"</span>   <span class="co">// ✅ OK</span>
<span class="va">email</span> <span class="pu">=</span> <span class="bo">null</span>              <span class="co">// ✅ OK</span>

<span class="co">// Safe call ?.</span>
<span class="k">val</span> <span class="va">length</span><span class="pu">:</span> <span class="ty">Int?</span> <span class="pu">=</span> <span class="va">email</span><span class="pu">?.</span><span class="fn">length</span>

<span class="co">// Elvis operator ?:</span>
<span class="k">val</span> <span class="va">displayEmail</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="va">email</span> <span class="pu">?:</span> <span class="st">"No email provided"</span>

<span class="co">// Real-life nullable examples</span>
<span class="k">var</span> <span class="va">middleName</span><span class="pu">:</span> <span class="ty">String?</span> <span class="pu">=</span> <span class="bo">null</span>   <span class="co">// Not everyone has a middle name</span>
<span class="k">var</span> <span class="va">alternatePhone</span><span class="pu">:</span> <span class="ty">String?</span> <span class="pu">=</span> <span class="bo">null</span>
<span class="k">var</span> <span class="va">profileBio</span><span class="pu">:</span> <span class="ty">String?</span> <span class="pu">=</span> <span class="bo">null</span></pre>
    </div>
  </div>

  <div class="divider"></div>

  <!-- LATEINIT -->
  <div class="section" id="lateinit">
    <div class="section-title">
      <div class="section-icon"><span class="material-icons-round" style="font-size:20px">schedule</span></div>
      <h2>Late Initialization</h2>
    </div>
    <div class="info-banner warn">
      <span class="ib-icon">⚠️</span>
      <div><div class="ib-title">Use with caution</div><div class="ib-body"><span class="inline-code">lateinit</span> only works with <span class="inline-code">var</span>, non-nullable types. You must initialize it before use or it throws <span class="inline-code">UninitializedPropertyAccessException</span>.</div></div>
    </div>
    <div class="code-block">
      <div class="code-header">
        <div class="code-lang">⬡ Kotlin — lateinit var</div>
        <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
      </div>
      <pre><span class="kw">class</span> <span class="ty">UserProfile</span> <span class="pu">{</span>
    <span class="kw">lateinit</span> <span class="k">var</span> <span class="va">userName</span><span class="pu">:</span> <span class="ty">String</span>
    <span class="kw">lateinit</span> <span class="k">var</span> <span class="va">profilePicUrl</span><span class="pu">:</span> <span class="ty">String</span>

    <span class="kw">fun</span> <span class="fn">setup</span><span class="pu">(</span>name<span class="pu">:</span> <span class="ty">String</span>, picUrl<span class="pu">:</span> <span class="ty">String</span><span class="pu">) {</span>
        <span class="va">userName</span> <span class="pu">=</span> name
        <span class="va">profilePicUrl</span> <span class="pu">=</span> picUrl
    <span class="pu">}</span>

    <span class="kw">fun</span> <span class="fn">display</span><span class="pu">() {</span>
        <span class="co">// Check before accessing</span>
        <span class="kw">if</span> <span class="pu">(</span><span class="pu">::</span>userName<span class="pu">.</span><span class="fn">isInitialized</span><span class="pu">) {</span>
            <span class="fn">println</span><span class="pu">(</span><span class="st">"Hello, <span style="color:#FFB86C">$userName</span>"</span><span class="pu">)</span>
        <span class="pu">}</span>
    <span class="pu">}</span>
<span class="pu">}</span>

<span class="co">// Android Activity example</span>
<span class="kw">class</span> <span class="ty">MainActivity</span> <span class="pu">:</span> <span class="ty">AppCompatActivity</span><span class="pu">() {</span>
    <span class="kw">lateinit</span> <span class="k">var</span> <span class="va">submitButton</span><span class="pu">:</span> <span class="ty">Button</span>
    <span class="kw">lateinit</span> <span class="k">var</span> <span class="va">nameEditText</span><span class="pu">:</span> <span class="ty">EditText</span>

    <span class="kw">override fun</span> <span class="fn">onCreate</span><span class="pu">(...) {</span>
        <span class="va">submitButton</span> <span class="pu">=</span> <span class="fn">findViewById</span><span class="pu">(</span>R.id.submitBtn<span class="pu">)</span>
        <span class="va">nameEditText</span> <span class="pu">=</span> <span class="fn">findViewById</span><span class="pu">(</span>R.id.nameField<span class="pu">)</span>
    <span class="pu">}</span>
<span class="pu">}</span></pre>
    </div>
  </div>

  <div class="divider"></div>

  <!-- CONST VAL -->
  <div class="section" id="const">
    <div class="section-title">
      <div class="section-icon"><span class="material-icons-round" style="font-size:20px">lock</span></div>
      <h2>Constants — <span class="inline-code" style="font-size:18px">const val</span></h2>
    </div>
    <div class="info-banner success">
      <span class="ib-icon">✅</span>
      <div><div class="ib-title">Compile-time constants</div><div class="ib-body"><span class="inline-code">const val</span> values are resolved at compile time. They must be declared at the top level or inside a <span class="inline-code">companion object</span>, and can only hold primitive types or <span class="inline-code">String</span>.</div></div>
    </div>
    <div class="code-block">
      <div class="code-header">
        <div class="code-lang">⬡ Kotlin — const val</div>
        <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
      </div>
      <pre><span class="co">// Top-level constants</span>
<span class="kw">const</span> <span class="k">val</span> <span class="va">MAX_LOGIN_ATTEMPTS</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">5</span>
<span class="kw">const</span> <span class="k">val</span> <span class="va">APP_VERSION</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"1.0.0"</span>
<span class="kw">const</span> <span class="k">val</span> <span class="va">BASE_URL</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"https://api.example.com"</span>
<span class="kw">const</span> <span class="k">val</span> <span class="va">TAX_RATE</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">0.18</span>
<span class="kw">const</span> <span class="k">val</span> <span class="va">FREE_DELIVERY_THRESHOLD</span><span class="pu">:</span> <span class="ty">Double</span> <span class="pu">=</span> <span class="nm">499.00</span>
<span class="kw">const</span> <span class="k">val</span> <span class="va">OTP_EXPIRY_SECONDS</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">120</span>

<span class="co">// Inside companion object</span>
<span class="kw">class</span> <span class="ty">OrderManager</span> <span class="pu">{</span>
    <span class="kw">companion object</span> <span class="pu">{</span>
        <span class="kw">const</span> <span class="k">val</span> <span class="va">STATUS_PENDING</span>   <span class="pu">=</span> <span class="st">"PENDING"</span>
        <span class="kw">const</span> <span class="k">val</span> <span class="va">STATUS_CONFIRMED</span> <span class="pu">=</span> <span class="st">"CONFIRMED"</span>
        <span class="kw">const</span> <span class="k">val</span> <span class="va">STATUS_DELIVERED</span> <span class="pu">=</span> <span class="st">"DELIVERED"</span>
        <span class="kw">const</span> <span class="k">val</span> <span class="va">STATUS_CANCELLED</span> <span class="pu">=</span> <span class="st">"CANCELLED"</span>
    <span class="pu">}</span>
<span class="pu">}</span>

<span class="co">// Usage</span>
<span class="kw">if</span> <span class="pu">(</span>loginAttempts <span class="pu">&gt;=</span> MAX_LOGIN_ATTEMPTS<span class="pu">) {</span>
    <span class="fn">lockAccount</span><span class="pu">()</span>
<span class="pu">}</span></pre>
    </div>
  </div>

  <div class="divider"></div>

  <!-- SCOPE -->
  <div class="section" id="scope">
    <div class="section-title">
      <div class="section-icon"><span class="material-icons-round" style="font-size:20px">layers</span></div>
      <h2>Variable Scope</h2>
    </div>
    <p class="text-secondary">Variables in Kotlin have four levels of scope, from widest to narrowest:</p>
    <div class="scope-container">
      <div class="scope-level scope-1">
        <div class="scope-label">1 · Top-level (File Scope)</div>
        <div class="scope-code"><span class="k" style="color:#BD93F9">val</span> <span style="color:#F8F8F2">APP_NAME</span> <span style="color:#FF79C6">=</span> <span style="color:#F1FA8C">"SuperApp"</span></div>
        <div class="scope-desc">Accessible anywhere in the file</div>
        <div class="scope-level scope-2" style="margin-top:12px">
          <div class="scope-label">2 · Class Level (Properties)</div>
          <div class="scope-code"><span class="k" style="color:#BD93F9">var</span> <span style="color:#F8F8F2">instanceCount</span><span style="color:#FF79C6">:</span> <span style="color:#8BE9FD">Int</span> <span style="color:#FF79C6">=</span> <span style="color:#BD93F9">0</span></div>
          <div class="scope-desc">Accessible across all class methods</div>
          <div class="scope-level scope-3" style="margin-top:12px">
            <div class="scope-label">3 · Function Level (Local)</div>
            <div class="scope-code"><span class="k" style="color:#BD93F9">val</span> <span style="color:#F8F8F2">localResult</span> <span style="color:#FF79C6">=</span> <span style="color:#F8F8F2">instanceCount</span> <span style="color:#FF79C6">*</span> <span style="color:#BD93F9">2</span></div>
            <div class="scope-desc">Only accessible within this function</div>
            <div class="scope-level scope-4" style="margin-top:12px">
              <div class="scope-label">4 · Block Level (if / for / when)</div>
              <div class="scope-code"><span class="k" style="color:#BD93F9">val</span> <span style="color:#F8F8F2">blockVar</span> <span style="color:#FF79C6">=</span> <span style="color:#F1FA8C">"inside if"</span></div>
              <div class="scope-desc">Only accessible inside this block</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="divider"></div>

  <!-- BEST PRACTICES -->
  <div class="section" id="practices">
    <div class="section-title">
      <div class="section-icon"><span class="material-icons-round" style="font-size:20px">verified</span></div>
      <h2>Best Practices</h2>
    </div>
    <ul class="practices-list">
      <li>
        <div class="prac-icon">✓</div>
        <div><div class="prac-title">Prefer val over var</div><div class="prac-example">Use val unless you know the value must change</div></div>
      </li>
      <li>
        <div class="prac-icon">✓</div>
        <div><div class="prac-title">Use meaningful, descriptive names</div><div class="prac-example">userAge instead of a or x</div></div>
      </li>
      <li>
        <div class="prac-icon">✓</div>
        <div><div class="prac-title">Follow camelCase naming convention</div><div class="prac-example">productPrice, isUserLoggedIn, cartItemCount</div></div>
      </li>
      <li>
        <div class="prac-icon">✓</div>
        <div><div class="prac-title">Use const val for compile-time constants</div><div class="prac-example">const val MAX_SIZE = 100</div></div>
      </li>
      <li>
        <div class="prac-icon">✓</div>
        <div><div class="prac-title">Avoid nullable types when possible</div><div class="prac-example">Don't use String? if a value is always present</div></div>
      </li>
      <li>
        <div class="prac-icon">✓</div>
        <div><div class="prac-title">Leverage type inference for readability</div><div class="prac-example">val name = "Ravi" instead of val name: String = "Ravi"</div></div>
      </li>
      <li>
        <div class="prac-icon">✓</div>
        <div><div class="prac-title">Initialize at declaration whenever possible</div><div class="prac-example">Avoid uninitialized variables unless using lateinit</div></div>
      </li>
    </ul>
    <div class="code-block">
      <div class="code-header">
        <div class="code-lang">⬡ Kotlin — Good vs Bad Practice</div>
        <div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div>
      </div>
      <pre><span class="co">// ✅ Good Practice</span>
<span class="k">val</span> <span class="va">userName</span> <span class="pu">=</span> <span class="st">"Ravi Patel"</span>
<span class="k">val</span> <span class="va">isSubscribed</span> <span class="pu">=</span> <span class="bo">true</span>
<span class="k">var</span> <span class="va">cartItemCount</span> <span class="pu">=</span> <span class="nm">0</span>
<span class="kw">const</span> <span class="k">val</span> <span class="va">MAX_CART_ITEMS</span> <span class="pu">=</span> <span class="nm">20</span>

<span class="co">// ❌ Bad Practice</span>
<span class="k">var</span> <span class="va">a</span> <span class="pu">=</span> <span class="st">"Ravi Patel"</span>        <span class="co">// non-descriptive name</span>
<span class="k">val</span> <span class="va">x</span> <span class="pu">=</span> <span class="bo">true</span>                <span class="co">// vague name</span>
<span class="k">var</span> <span class="va">c</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">0</span>              <span class="co">// var when val is more appropriate</span></pre>
    </div>
  </div>

  <div class="divider"></div>

  <!-- COMMON MISTAKES -->
  <div class="section" id="mistakes">
    <div class="section-title">
      <div class="section-icon"><span class="material-icons-round" style="font-size:20px">warning</span></div>
      <h2>Common Mistakes to Avoid</h2>
    </div>
    <div class="mistake-card">
      <div class="mistake-header"><div class="mistake-num">1</div><div class="mistake-title">Reassigning a val</div></div>
      <div class="mistake-body">
        <div class="code-block" style="margin:0"><div class="code-header"><div class="code-lang">⬡ Kotlin</div><div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div></div><pre><span class="k">val</span> <span class="va">score</span> <span class="pu">=</span> <span class="nm">100</span>
<span class="co">// score = 200  ❌ ERROR: Val cannot be reassigned</span>
<span class="k">var</span> <span class="va">score</span> <span class="pu">=</span> <span class="nm">100</span>   <span class="co">// ✅ Use var if reassignment is needed</span></pre></div>
      </div>
    </div>
    <div class="mistake-card">
      <div class="mistake-header"><div class="mistake-num">2</div><div class="mistake-title">Using var when val is enough</div></div>
      <div class="mistake-body">
        <div class="code-block" style="margin:0"><div class="code-header"><div class="code-lang">⬡ Kotlin</div><div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div></div><pre><span class="k">var</span> <span class="va">taxRate</span> <span class="pu">=</span> <span class="nm">0.18</span>  <span class="co">// ❌ Bad — tax rate doesn't change</span>
<span class="k">val</span> <span class="va">taxRate</span> <span class="pu">=</span> <span class="nm">0.18</span>  <span class="co">// ✅ Good — immutable by nature</span></pre></div>
      </div>
    </div>
    <div class="mistake-card">
      <div class="mistake-header"><div class="mistake-num">3</div><div class="mistake-title">Wrong type inference assumption</div></div>
      <div class="mistake-body">
        <div class="code-block" style="margin:0"><div class="code-header"><div class="code-lang">⬡ Kotlin</div><div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div></div><pre><span class="k">val</span> <span class="va">price</span> <span class="pu">=</span> <span class="nm">100</span>     <span class="co">// ❌ Inferred as Int, not Double!</span>
<span class="k">val</span> <span class="va">price</span> <span class="pu">=</span> <span class="nm">100.0</span>   <span class="co">// ✅ Correctly inferred as Double</span></pre></div>
      </div>
    </div>
    <div class="mistake-card">
      <div class="mistake-header"><div class="mistake-num">4</div><div class="mistake-title">Using variable before initialization</div></div>
      <div class="mistake-body">
        <div class="code-block" style="margin:0"><div class="code-header"><div class="code-lang">⬡ Kotlin</div><div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div></div><pre><span class="k">var</span> <span class="va">count</span><span class="pu">:</span> <span class="ty">Int</span>
<span class="co">// println(count)  ❌ ERROR: Variable must be initialized
</span><span class="k">var</span> <span class="va">count</span><span class="pu">:</span> <span class="ty">Int</span> <span class="pu">=</span> <span class="nm">0</span>   <span class="co">// ✅ Initialize before use</span></pre></div>
      </div>
    </div>
    <div class="mistake-card">
      <div class="mistake-header"><div class="mistake-num">5</div><div class="mistake-title">Null assignment to non-nullable</div></div>
      <div class="mistake-body">
        <div class="code-block" style="margin:0"><div class="code-header"><div class="code-lang">⬡ Kotlin</div><div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div></div><pre><span class="k">var</span> <span class="va">name</span><span class="pu">:</span> <span class="ty">String</span> <span class="pu">=</span> <span class="st">"Ravi"</span>
<span class="co">// name = null  ❌ ERROR: Null can not be a value of non-null String</span>
<span class="k">var</span> <span class="va">name</span><span class="pu">:</span> <span class="ty">String?</span> <span class="pu">=</span> <span class="bo">null</span>  <span class="co">// ✅ Use String? for nullable</span></pre></div>
      </div>
    </div>
    <div class="mistake-card">
      <div class="mistake-header"><div class="mistake-num">6</div><div class="mistake-title">Variable shadowing in nested scope</div></div>
      <div class="mistake-body">
        <div class="code-block" style="margin:0"><div class="code-header"><div class="code-lang">⬡ Kotlin</div><div class="code-dots"><div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div></div></div><pre><span class="k">val</span> <span class="va">value</span> <span class="pu">=</span> <span class="nm">10</span>
<span class="kw">if</span> <span class="pu">(</span><span class="bo">true</span><span class="pu">) {</span>
    <span class="k">val</span> <span class="va">value</span> <span class="pu">=</span> <span class="nm">20</span>  <span class="co">// ❌ Shadows outer 'value' — confusing!</span>
<span class="pu">}</span>
<span class="co">// Use a different, descriptive name instead</span></pre></div>
      </div>
    </div>
  </div>

  <div class="divider"></div>

  <!-- CHEAT SHEET -->
  <div class="section" id="cheatsheet">
    <div class="section-title">
      <div class="section-icon"><span class="material-icons-round" style="font-size:20px">summarize</span></div>
      <h2>Cheat Sheet</h2>
    </div>
    <div class="cheat-grid">
      <div class="cheat-group">
        <div class="cheat-group-header" style="background:#EDE7F6;color:#4A148C">Variable Keywords</div>
        <div class="cheat-row"><span class="cheat-key" style="color:#7C4DFF">val</span><span class="cheat-val">Immutable — assign once</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#EF5350">var</span><span class="cheat-val">Mutable — reassign freely</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#2E7D32">const val</span><span class="cheat-val">Compile-time constant</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#E65100">lateinit var</span><span class="cheat-val">Late-initialized non-null</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#1565C0">Type?</span><span class="cheat-val">Nullable variable</span></div>
      </div>
      <div class="cheat-group">
        <div class="cheat-group-header" style="background:#E3F2FD;color:#0D47A1">Number Types</div>
        <div class="cheat-row"><span class="cheat-key" style="color:#1565C0">Int</span><span class="cheat-val">Whole numbers: 25, -10, 0</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#1565C0">Long</span><span class="cheat-val">Big whole numbers: 9999L</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#1565C0">Double</span><span class="cheat-val">Decimal: 3.14, 99.99</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#1565C0">Float</span><span class="cheat-val">Small decimal: 3.14F</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#1565C0">Short / Byte</span><span class="cheat-val">16-bit / 8-bit integers</span></div>
      </div>
      <div class="cheat-group">
        <div class="cheat-group-header" style="background:#E8F5E9;color:#1B5E20">Other Types</div>
        <div class="cheat-row"><span class="cheat-key" style="color:#2E7D32">Boolean</span><span class="cheat-val">true / false only</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#2E7D32">Char</span><span class="cheat-val">Single character: 'A'</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#2E7D32">String</span><span class="cheat-val">Text: "Hello Kotlin"</span></div>
      </div>
      <div class="cheat-group">
        <div class="cheat-group-header" style="background:#FFF3E0;color:#E65100">Nullable Operators</div>
        <div class="cheat-row"><span class="cheat-key" style="color:#E65100">?</span><span class="cheat-val">Makes a type nullable</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#E65100">?.</span><span class="cheat-val">Safe call operator</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#E65100">?:</span><span class="cheat-val">Elvis — provide default</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#E65100">!!</span><span class="cheat-val">Non-null assertion</span></div>
      </div>
      <div class="cheat-group">
        <div class="cheat-group-header" style="background:#FCE4EC;color:#880E4F">Scope Levels</div>
        <div class="cheat-row"><span class="cheat-key" style="color:#880E4F">Top-level</span><span class="cheat-val">Whole file scope</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#880E4F">Class</span><span class="cheat-val">All class methods</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#880E4F">Function</span><span class="cheat-val">Local to function</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#880E4F">Block</span><span class="cheat-val">if / for / when only</span></div>
      </div>
      <div class="cheat-group">
        <div class="cheat-group-header" style="background:#F3E5F5;color:#4A148C">Naming Rules</div>
        <div class="cheat-row"><span class="cheat-key" style="color:#6A1B9A">camelCase</span><span class="cheat-val">For variables: userAge</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#6A1B9A">UPPER_CASE</span><span class="cheat-val">For const val: MAX_SIZE</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#6A1B9A">PascalCase</span><span class="cheat-val">For classes: UserProfile</span></div>
        <div class="cheat-row"><span class="cheat-key" style="color:#6A1B9A">Descriptive</span><span class="cheat-val">userAge not a or ua</span></div>
      </div>
    </div>
  </div>

</div>

<!-- FOOTER -->
<div class="footer">
  <div class="footer-kotlin">⬡ Kotlin</div>
  <div class="footer-tags">
    <span class="footer-tag">val</span>
    <span class="footer-tag">var</span>
    <span class="footer-tag">const val</span>
    <span class="footer-tag">lateinit</span>
    <span class="footer-tag">nullable</span>
    <span class="footer-tag">type inference</span>
    <span class="footer-tag">scope</span>
    <span class="footer-tag">data types</span>
    <span class="footer-tag">best practices</span>
  </div>
  <p>Kotlin Variables — Complete Guide with 100 Real-Life Examples across 12 Domains</p>
</div>

<script>
  function showCat(id, tab) {
    document.querySelectorAll('.example-panel').forEach(p => p.classList.remove('active'));
    document.querySelectorAll('.cat-tab').forEach(t => t.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    tab.classList.add('active');
  }

  document.querySelectorAll('.nav-pill').forEach(pill => {
    pill.addEventListener('click', function(e) {
      document.querySelectorAll('.nav-pill').forEach(p => p.classList.remove('active'));
      this.classList.add('active');
    });
  });
</script>
</body>
</html>
