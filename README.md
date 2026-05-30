import { useState } from "react";

const ACCENT = "#6C47FF";
const ACCENT2 = "#00D4AA";

const styles = `
  @import url('https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700;800&family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;1,9..40,300&display=swap');

  * { box-sizing: border-box; margin: 0; padding: 0; }
  body, #root { background: #0A0A12; min-height: 100vh; color: #E8E6F0; font-family: 'DM Sans', sans-serif; }
  .app { min-height: 100vh; background: #0A0A12; }

  /* NAV */
  .nav { display: flex; align-items: center; justify-content: space-between; padding: 1.25rem 2.5rem; border-bottom: 1px solid rgba(108,71,255,0.15); background: rgba(10,10,18,0.95); position: sticky; top: 0; z-index: 50; backdrop-filter: blur(12px); }
  .nav-logo { font-family: 'Syne', sans-serif; font-weight: 800; font-size: 1.35rem; letter-spacing: -0.02em; background: linear-gradient(135deg, #6C47FF, #00D4AA); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .nav-badge { font-size: 0.7rem; font-weight: 600; letter-spacing: 0.08em; text-transform: uppercase; color: #00D4AA; border: 1px solid rgba(0,212,170,0.3); padding: 0.25rem 0.6rem; border-radius: 20px; background: rgba(0,212,170,0.07); }
  .nav-right { display: flex; gap: 1rem; align-items: center; }
  .nav-link { font-size: 0.875rem; color: rgba(232,230,240,0.55); cursor: pointer; transition: color 0.2s; font-weight: 400; }
  .nav-link:hover { color: #E8E6F0; }
  .btn-nav { background: #6C47FF; color: #fff; border: none; padding: 0.5rem 1.2rem; border-radius: 8px; font-size: 0.875rem; font-weight: 500; cursor: pointer; font-family: 'DM Sans', sans-serif; transition: opacity 0.2s; }
  .btn-nav:hover { opacity: 0.88; }

  /* HERO */
  .hero { text-align: center; padding: 4rem 2rem 2.5rem; position: relative; overflow: hidden; }
  .hero-glow { position: absolute; top: -40px; left: 50%; transform: translateX(-50%); width: 600px; height: 260px; background: radial-gradient(ellipse, rgba(108,71,255,0.18) 0%, transparent 70%); pointer-events: none; }
  .hero-eyebrow { display: inline-flex; align-items: center; gap: 0.5rem; font-size: 0.78rem; font-weight: 600; letter-spacing: 0.1em; text-transform: uppercase; color: #00D4AA; background: rgba(0,212,170,0.08); border: 1px solid rgba(0,212,170,0.2); padding: 0.35rem 0.9rem; border-radius: 20px; margin-bottom: 1.5rem; }
  .hero-dot { width: 6px; height: 6px; border-radius: 50%; background: #00D4AA; animation: pulse 2s infinite; }
  @keyframes pulse { 0%,100%{opacity:1;transform:scale(1)} 50%{opacity:0.5;transform:scale(0.8)} }
  .hero h1 { font-family: 'Syne', sans-serif; font-size: clamp(2rem, 5vw, 3.4rem); font-weight: 800; line-height: 1.08; letter-spacing: -0.03em; color: #F0EEF8; margin-bottom: 1rem; }
  .hero h1 span { background: linear-gradient(135deg, #6C47FF 20%, #00D4AA 80%); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .hero p { font-size: 1rem; color: rgba(232,230,240,0.6); max-width: 520px; margin: 0 auto; line-height: 1.7; font-weight: 300; }

  /* TABS */
  .tabs-wrapper { max-width: 1100px; margin: 0 auto; padding: 2rem 1.5rem 0; }
  .tabs { display: flex; gap: 0; background: rgba(255,255,255,0.03); border: 1px solid rgba(255,255,255,0.08); border-radius: 14px; padding: 5px; width: fit-content; }
  .tab-btn { font-family: 'Syne', sans-serif; font-size: 0.88rem; font-weight: 600; padding: 0.6rem 1.4rem; border-radius: 10px; border: none; cursor: pointer; transition: all 0.22s; letter-spacing: 0.01em; display: flex; align-items: center; gap: 0.5rem; }
  .tab-btn.active { background: linear-gradient(135deg, #6C47FF, #8B6AFF); color: #fff; box-shadow: 0 4px 16px rgba(108,71,255,0.35); }
  .tab-btn.inactive { background: none; color: rgba(232,230,240,0.45); }
  .tab-btn.inactive:hover { color: rgba(232,230,240,0.8); background: rgba(255,255,255,0.04); }
  .tab-dot { width: 6px; height: 6px; border-radius: 50%; }
  .tab-dot-ads { background: #A78BFF; }
  .tab-dot-hl { background: #00D4AA; }
  .tab-dot-desc { background: #F77F67; }

  /* MAIN */
  .main { max-width: 1100px; margin: 0 auto; padding: 1.5rem 1.5rem 4rem; display: grid; grid-template-columns: 1fr 1fr; gap: 2rem; }
  .main-single { max-width: 1100px; margin: 0 auto; padding: 1.5rem 1.5rem 4rem; display: grid; grid-template-columns: 380px 1fr; gap: 2rem; }
  @media (max-width: 768px) { .main, .main-single { grid-template-columns: 1fr; } .nav { padding: 1rem; } .nav-right { display: none; } .tabs { width: 100%; } .tab-btn { flex: 1; justify-content: center; } }

  /* PANELS */
  .panel { background: rgba(255,255,255,0.03); border: 1px solid rgba(255,255,255,0.08); border-radius: 20px; padding: 2rem; }
  .panel-title { font-family: 'Syne', sans-serif; font-size: 1.1rem; font-weight: 700; color: #F0EEF8; margin-bottom: 0.35rem; }
  .panel-sub { font-size: 0.82rem; color: rgba(232,230,240,0.45); margin-bottom: 1.75rem; }
  .field { margin-bottom: 1.25rem; }
  .field label { display: block; font-size: 0.8rem; font-weight: 500; letter-spacing: 0.04em; text-transform: uppercase; color: rgba(232,230,240,0.5); margin-bottom: 0.5rem; }
  .field input, .field textarea, .field select { width: 100%; background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.1); border-radius: 10px; padding: 0.75rem 1rem; color: #E8E6F0; font-size: 0.9rem; font-family: 'DM Sans', sans-serif; outline: none; transition: border-color 0.2s, background 0.2s; resize: vertical; }
  .field select { appearance: none; cursor: pointer; }
  .field select option { background: #1A1A2E; color: #E8E6F0; }
  .field input:focus, .field textarea:focus, .field select:focus { border-color: rgba(108,71,255,0.6); background: rgba(108,71,255,0.05); }
  .field input::placeholder, .field textarea::placeholder { color: rgba(232,230,240,0.25); }
  .field textarea { min-height: 90px; }

  .btn-generate { width: 100%; padding: 0.95rem; background: linear-gradient(135deg, #6C47FF, #8B6AFF); color: #fff; border: none; border-radius: 12px; font-size: 1rem; font-weight: 600; font-family: 'Syne', sans-serif; cursor: pointer; transition: all 0.2s; letter-spacing: 0.01em; position: relative; overflow: hidden; margin-top: 0.5rem; }
  .btn-generate:hover:not(:disabled) { transform: translateY(-1px); box-shadow: 0 8px 30px rgba(108,71,255,0.4); }
  .btn-generate:active { transform: translateY(0); }
  .btn-generate:disabled { opacity: 0.6; cursor: not-allowed; }
  .btn-generate .btn-inner { display: flex; align-items: center; justify-content: center; gap: 0.6rem; }

  /* LOADING */
  .spinner { width: 18px; height: 18px; border: 2px solid rgba(255,255,255,0.3); border-top-color: #fff; border-radius: 50%; animation: spin 0.7s linear infinite; }
  @keyframes spin { to { transform: rotate(360deg); } }

  /* RESULTS */
  .results-panel { display: flex; flex-direction: column; gap: 1.25rem; }
  .result-card { background: rgba(255,255,255,0.03); border: 1px solid rgba(255,255,255,0.08); border-radius: 16px; padding: 1.4rem 1.6rem; animation: fadeUp 0.4s ease both; }
  @keyframes fadeUp { from { opacity:0; transform: translateY(12px); } to { opacity:1; transform: translateY(0); } }
  .result-card:nth-child(2) { animation-delay: 0.08s; }
  .result-card:nth-child(3) { animation-delay: 0.16s; }
  .result-card:nth-child(4) { animation-delay: 0.24s; }
  .card-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 1rem; }
  .card-tag { font-size: 0.7rem; font-weight: 700; letter-spacing: 0.1em; text-transform: uppercase; padding: 0.28rem 0.7rem; border-radius: 20px; }
  .tag-headlines { color: #A78BFF; background: rgba(167,139,255,0.12); border: 1px solid rgba(167,139,255,0.2); }
  .tag-descriptions { color: #00D4AA; background: rgba(0,212,170,0.1); border: 1px solid rgba(0,212,170,0.2); }
  .tag-keywords { color: #F9C74F; background: rgba(249,199,79,0.1); border: 1px solid rgba(249,199,79,0.2); }
  .tag-cta { color: #F77F67; background: rgba(247,127,103,0.1); border: 1px solid rgba(247,127,103,0.2); }
  .tag-hl50 { color: #00D4AA; background: rgba(0,212,170,0.1); border: 1px solid rgba(0,212,170,0.2); }
  .card-count { font-size: 0.75rem; color: rgba(232,230,240,0.35); }
  .copy-btn { background: none; border: 1px solid rgba(255,255,255,0.1); color: rgba(232,230,240,0.5); padding: 0.3rem 0.7rem; border-radius: 6px; font-size: 0.72rem; cursor: pointer; transition: all 0.2s; font-family: 'DM Sans', sans-serif; }
  .copy-btn:hover { border-color: rgba(108,71,255,0.5); color: #A78BFF; }

  /* HEADLINES GRID (Google Ads) */
  .headlines-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 0.5rem; }
  .headline-item { background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.07); border-radius: 8px; padding: 0.55rem 0.75rem; font-size: 0.82rem; color: #D4D0E8; display: flex; justify-content: space-between; align-items: center; gap: 0.5rem; transition: border-color 0.2s; cursor: pointer; }
  .headline-item:hover { border-color: rgba(108,71,255,0.35); }
  .char-count { font-size: 0.65rem; color: rgba(232,230,240,0.3); flex-shrink: 0; }
  .char-ok { color: #00D4AA; }
  .char-warn { color: #F9C74F; }

  /* HEADLINES 50 LIST */
  .hl50-list { display: flex; flex-direction: column; gap: 0; }
  .hl50-item { display: flex; align-items: center; gap: 0.75rem; padding: 0.65rem 0.85rem; border-radius: 9px; cursor: pointer; transition: background 0.15s; group: true; }
  .hl50-item:hover { background: rgba(255,255,255,0.04); }
  .hl50-num { font-size: 0.7rem; font-weight: 700; color: rgba(232,230,240,0.2); width: 20px; flex-shrink: 0; text-align: right; font-family: 'Syne', sans-serif; }
  .hl50-text { font-size: 0.88rem; color: #D4D0E8; line-height: 1.45; flex: 1; }
  .hl50-copy { opacity: 0; font-size: 0.68rem; color: rgba(108,71,255,0.7); background: rgba(108,71,255,0.1); border: 1px solid rgba(108,71,255,0.2); padding: 0.2rem 0.5rem; border-radius: 5px; flex-shrink: 0; transition: opacity 0.15s; }
  .hl50-item:hover .hl50-copy { opacity: 1; }
  .hl50-divider { height: 1px; background: rgba(255,255,255,0.04); margin: 0 0.85rem; }

  /* CATEGORY BADGE */
  .hl50-cat { font-size: 0.67rem; font-weight: 700; letter-spacing: 0.08em; text-transform: uppercase; padding: 0.18rem 0.55rem; border-radius: 20px; flex-shrink: 0; }
  .cat-urgencia { color: #F77F67; background: rgba(247,127,103,0.1); border: 1px solid rgba(247,127,103,0.15); }
  .cat-curiosidade { color: #F9C74F; background: rgba(249,199,79,0.08); border: 1px solid rgba(249,199,79,0.15); }
  .cat-beneficio { color: #00D4AA; background: rgba(0,212,170,0.08); border: 1px solid rgba(0,212,170,0.15); }
  .cat-pergunta { color: #A78BFF; background: rgba(167,139,255,0.1); border: 1px solid rgba(167,139,255,0.18); }
  .cat-social { color: #60C0FF; background: rgba(96,192,255,0.08); border: 1px solid rgba(96,192,255,0.15); }

  /* DESCRIPTIONS */
  .desc-list { display: flex; flex-direction: column; gap: 0.6rem; }
  .desc-item { background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.07); border-radius: 8px; padding: 0.7rem 0.9rem; font-size: 0.85rem; color: #D4D0E8; line-height: 1.5; cursor: pointer; transition: border-color 0.2s; }
  .desc-item:hover { border-color: rgba(0,212,170,0.3); }

  /* KEYWORDS */
  .keywords-wrap { display: flex; flex-wrap: wrap; gap: 0.5rem; }
  .kw-chip { background: rgba(249,199,79,0.08); border: 1px solid rgba(249,199,79,0.18); color: #F9C74F; padding: 0.38rem 0.85rem; border-radius: 20px; font-size: 0.8rem; cursor: pointer; transition: all 0.2s; }
  .kw-chip:hover { background: rgba(249,199,79,0.15); }

  /* CTA */
  .cta-wrap { display: flex; flex-wrap: wrap; gap: 0.5rem; }
  .cta-chip { background: rgba(247,127,103,0.08); border: 1px solid rgba(247,127,103,0.2); color: #F77F67; padding: 0.45rem 1.1rem; border-radius: 8px; font-size: 0.85rem; font-weight: 600; cursor: pointer; transition: all 0.2s; font-family: 'Syne', sans-serif; }
  .cta-chip:hover { background: rgba(247,127,103,0.15); transform: translateY(-1px); }

  /* EMPTY STATE */
  .empty-state { display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 420px; text-align: center; gap: 1rem; }
  .empty-icon { width: 64px; height: 64px; border-radius: 18px; background: rgba(108,71,255,0.1); border: 1px solid rgba(108,71,255,0.2); display: flex; align-items: center; justify-content: center; font-size: 1.8rem; }
  .empty-state h3 { font-family: 'Syne', sans-serif; font-size: 1.05rem; font-weight: 700; color: rgba(232,230,240,0.7); }
  .empty-state p { font-size: 0.83rem; color: rgba(232,230,240,0.35); max-width: 260px; line-height: 1.6; }

  /* TOAST */
  .toast { position: fixed; bottom: 2rem; left: 50%; transform: translateX(-50%); background: #1A1A2E; border: 1px solid rgba(108,71,255,0.3); color: #A78BFF; padding: 0.65rem 1.4rem; border-radius: 10px; font-size: 0.85rem; z-index: 999; animation: toastIn 0.3s ease; }
  @keyframes toastIn { from{opacity:0;transform:translateX(-50%) translateY(10px)} to{opacity:1;transform:translateX(-50%) translateY(0)} }

  /* STATS BAR */
  .stats-bar { display: flex; gap: 1.5rem; justify-content: center; padding: 0.75rem 2rem 2rem; }
  .stat { text-align: center; }
  .stat-num { font-family: 'Syne', sans-serif; font-size: 1.5rem; font-weight: 800; background: linear-gradient(135deg, #6C47FF, #00D4AA); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .stat-label { font-size: 0.72rem; color: rgba(232,230,240,0.4); text-transform: uppercase; letter-spacing: 0.06em; margin-top: 2px; }

  /* HL50 SEARCH */
  .hl50-search { width: 100%; background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.1); border-radius: 10px; padding: 0.6rem 1rem; color: #E8E6F0; font-size: 0.85rem; font-family: 'DM Sans', sans-serif; outline: none; transition: border-color 0.2s; margin-bottom: 1rem; }
  .hl50-search:focus { border-color: rgba(108,71,255,0.5); }
  .hl50-search::placeholder { color: rgba(232,230,240,0.25); }

  /* FILTER PILLS */
  .filter-pills { display: flex; flex-wrap: wrap; gap: 0.4rem; margin-bottom: 1rem; }
  .filter-pill { font-size: 0.72rem; font-weight: 600; letter-spacing: 0.06em; text-transform: uppercase; padding: 0.28rem 0.7rem; border-radius: 20px; cursor: pointer; transition: all 0.18s; border: 1px solid transparent; }
  .filter-pill.all { color: #A78BFF; background: rgba(167,139,255,0.12); border-color: rgba(167,139,255,0.2); }
  .filter-pill.all.off { color: rgba(232,230,240,0.3); background: rgba(255,255,255,0.03); border-color: rgba(255,255,255,0.07); }
  .filter-pill.urgencia { color: #F77F67; background: rgba(247,127,103,0.1); border-color: rgba(247,127,103,0.15); }
  .filter-pill.urgencia.off { color: rgba(232,230,240,0.3); background: rgba(255,255,255,0.03); border-color: rgba(255,255,255,0.07); }
  .filter-pill.curiosidade { color: #F9C74F; background: rgba(249,199,79,0.08); border-color: rgba(249,199,79,0.15); }
  .filter-pill.curiosidade.off { color: rgba(232,230,240,0.3); background: rgba(255,255,255,0.03); border-color: rgba(255,255,255,0.07); }
  .filter-pill.beneficio { color: #00D4AA; background: rgba(0,212,170,0.08); border-color: rgba(0,212,170,0.15); }
  .filter-pill.beneficio.off { color: rgba(232,230,240,0.3); background: rgba(255,255,255,0.03); border-color: rgba(255,255,255,0.07); }
  .filter-pill.pergunta { color: #A78BFF; background: rgba(167,139,255,0.1); border-color: rgba(167,139,255,0.18); }
  .filter-pill.pergunta.off { color: rgba(232,230,240,0.3); background: rgba(255,255,255,0.03); border-color: rgba(255,255,255,0.07); }
  .filter-pill.social { color: #60C0FF; background: rgba(96,192,255,0.08); border-color: rgba(96,192,255,0.15); }
  .filter-pill.social.off { color: rgba(232,230,240,0.3); background: rgba(255,255,255,0.03); border-color: rgba(255,255,255,0.07); }

  /* SCROLL AREA */
  .hl50-scroll { max-height: 560px; overflow-y: auto; scrollbar-width: thin; scrollbar-color: rgba(108,71,255,0.3) transparent; }
  .hl50-scroll::-webkit-scrollbar { width: 4px; }
  .hl50-scroll::-webkit-scrollbar-thumb { background: rgba(108,71,255,0.3); border-radius: 4px; }

  /* DESCRIPTIONS TAB */
  .tab-dot-desc { background: #F77F67; }

  .platform-toggle { display: flex; gap: 0; background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.08); border-radius: 10px; padding: 4px; margin-bottom: 1.25rem; }
  .platform-btn { flex: 1; padding: 0.5rem 0; border: none; border-radius: 7px; font-size: 0.82rem; font-weight: 600; font-family: 'Syne', sans-serif; cursor: pointer; transition: all 0.2s; display: flex; align-items: center; justify-content: center; gap: 0.45rem; letter-spacing: 0.01em; }
  .platform-btn.active-google { background: linear-gradient(135deg, #4285F4, #5A9BFF); color: #fff; box-shadow: 0 3px 12px rgba(66,133,244,0.35); }
  .platform-btn.active-facebook { background: linear-gradient(135deg, #1877F2, #42A5F5); color: #fff; box-shadow: 0 3px 12px rgba(24,119,242,0.35); }
  .platform-btn.inactive-plat { background: none; color: rgba(232,230,240,0.4); }
  .platform-btn.inactive-plat:hover { color: rgba(232,230,240,0.75); background: rgba(255,255,255,0.04); }

  .desc-platform-icon { width: 16px; height: 16px; border-radius: 3px; flex-shrink: 0; }
  .icon-google { background: linear-gradient(135deg, #EA4335 25%, #FBBC05 50%, #34A853 75%, #4285F4); border-radius: 50%; }
  .icon-facebook { background: #1877F2; border-radius: 3px; display: flex; align-items: center; justify-content: center; font-size: 0.65rem; font-weight: 800; color: #fff; }

  .tag-google { color: #5A9BFF; background: rgba(66,133,244,0.1); border: 1px solid rgba(66,133,244,0.2); }
  .tag-facebook { color: #60AAFF; background: rgba(24,119,242,0.1); border: 1px solid rgba(24,119,242,0.2); }

  .desc-gen-item { background: rgba(255,255,255,0.03); border: 1px solid rgba(255,255,255,0.07); border-radius: 10px; padding: 0; overflow: hidden; transition: border-color 0.18s; cursor: pointer; }
  .desc-gen-item:hover { border-color: rgba(108,71,255,0.3); }
  .desc-gen-item:hover .desc-gen-copy { opacity: 1; }
  .desc-gen-body { display: flex; align-items: flex-start; gap: 0.75rem; padding: 0.8rem 1rem; }
  .desc-gen-num { font-size: 0.68rem; font-weight: 700; color: rgba(232,230,240,0.2); min-width: 18px; text-align: right; padding-top: 2px; font-family: 'Syne', sans-serif; flex-shrink: 0; }
  .desc-gen-text { font-size: 0.875rem; color: #D4D0E8; line-height: 1.55; flex: 1; }
  .desc-gen-copy { opacity: 0; font-size: 0.68rem; color: rgba(108,71,255,0.8); background: rgba(108,71,255,0.1); border: 1px solid rgba(108,71,255,0.2); padding: 0.22rem 0.55rem; border-radius: 5px; flex-shrink: 0; transition: opacity 0.15s; align-self: center; white-space: nowrap; }
  .desc-gen-chars { font-size: 0.65rem; padding: 0 1rem 0.55rem 2.6rem; }

  .desc-scroll { max-height: 520px; overflow-y: auto; scrollbar-width: thin; scrollbar-color: rgba(108,71,255,0.25) transparent; display: flex; flex-direction: column; gap: 0.4rem; }
  .desc-scroll::-webkit-scrollbar { width: 4px; }
  .desc-scroll::-webkit-scrollbar-thumb { background: rgba(108,71,255,0.25); border-radius: 4px; }

  .copy-all-banner { display: flex; align-items: center; justify-content: space-between; background: rgba(108,71,255,0.07); border: 1px solid rgba(108,71,255,0.18); border-radius: 10px; padding: 0.75rem 1rem; margin-bottom: 1rem; }
  .copy-all-info { font-size: 0.82rem; color: rgba(232,230,240,0.55); }
  .copy-all-info strong { color: #A78BFF; font-weight: 600; }
  .btn-copy-all { background: rgba(108,71,255,0.15); border: 1px solid rgba(108,71,255,0.3); color: #A78BFF; padding: 0.4rem 1rem; border-radius: 7px; font-size: 0.78rem; font-weight: 600; cursor: pointer; font-family: 'Syne', sans-serif; transition: all 0.2s; white-space: nowrap; }
  .btn-copy-all:hover { background: rgba(108,71,255,0.25); }

  .desc-results-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 1.5rem; }
  @media (max-width: 900px) { .desc-results-grid { grid-template-columns: 1fr; } }

  .desc-platform-card { background: rgba(255,255,255,0.025); border: 1px solid rgba(255,255,255,0.07); border-radius: 16px; padding: 1.25rem 1.4rem; animation: fadeUp 0.4s ease both; }
  .desc-platform-card:nth-child(2) { animation-delay: 0.1s; }
  .platform-card-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 1rem; padding-bottom: 0.85rem; border-bottom: 1px solid rgba(255,255,255,0.06); }
  .platform-label { display: flex; align-items: center; gap: 0.6rem; }
  .platform-label-icon { width: 28px; height: 28px; border-radius: 8px; display: flex; align-items: center; justify-content: center; font-size: 0.7rem; font-weight: 900; flex-shrink: 0; }
  .plicon-google { background: rgba(66,133,244,0.15); border: 1px solid rgba(66,133,244,0.25); }
  .plicon-facebook { background: rgba(24,119,242,0.15); border: 1px solid rgba(24,119,242,0.25); }
  .platform-label-name { font-family: 'Syne', sans-serif; font-size: 0.9rem; font-weight: 700; color: #F0EEF8; }
  .platform-label-count { font-size: 0.72rem; color: rgba(232,230,240,0.35); }

  /* ANALYZER TAB */
  .tab-dot-analyzer { background: #F9C74F; }

  .metrics-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 0.85rem; margin-bottom: 0.25rem; }
  .metric-field { display: flex; flex-direction: column; gap: 0.4rem; }
  .metric-field label { font-size: 0.75rem; font-weight: 600; letter-spacing: 0.05em; text-transform: uppercase; color: rgba(232,230,240,0.45); }
  .metric-input-wrap { position: relative; display: flex; align-items: center; }
  .metric-input { width: 100%; background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.1); border-radius: 10px; padding: 0.65rem 0.85rem; color: #E8E6F0; font-size: 0.9rem; font-family: 'DM Sans', sans-serif; outline: none; transition: border-color 0.2s, background 0.2s; }
  .metric-input:focus { border-color: rgba(108,71,255,0.6); background: rgba(108,71,255,0.05); }
  .metric-input::placeholder { color: rgba(232,230,240,0.2); }
  .metric-suffix { position: absolute; right: 0.75rem; font-size: 0.75rem; color: rgba(232,230,240,0.35); font-weight: 500; pointer-events: none; }

  /* SCORE RING */
  .score-section { display: flex; flex-direction: column; align-items: center; padding: 2rem 1rem 1.5rem; }
  .score-ring-wrap { position: relative; width: 130px; height: 130px; margin-bottom: 1rem; }
  .score-ring-svg { width: 130px; height: 130px; transform: rotate(-90deg); }
  .score-ring-bg { fill: none; stroke: rgba(255,255,255,0.06); stroke-width: 8; }
  .score-ring-fill { fill: none; stroke-width: 8; stroke-linecap: round; transition: stroke-dashoffset 1.2s cubic-bezier(0.34,1.56,0.64,1); }
  .score-center { position: absolute; top: 50%; left: 50%; transform: translate(-50%,-50%); text-align: center; }
  .score-number { font-family: 'Syne', sans-serif; font-size: 2rem; font-weight: 800; line-height: 1; }
  .score-label { font-size: 0.65rem; font-weight: 600; letter-spacing: 0.08em; text-transform: uppercase; color: rgba(232,230,240,0.4); margin-top: 2px; }
  .score-verdict { font-family: 'Syne', sans-serif; font-size: 1rem; font-weight: 700; text-align: center; }
  .score-sub { font-size: 0.78rem; color: rgba(232,230,240,0.4); text-align: center; margin-top: 0.3rem; }

  /* DIAGNOSIS SECTIONS */
  .diag-section { margin-bottom: 1.1rem; }
  .diag-section-title { display: flex; align-items: center; gap: 0.55rem; font-family: 'Syne', sans-serif; font-size: 0.82rem; font-weight: 700; letter-spacing: 0.04em; text-transform: uppercase; margin-bottom: 0.7rem; padding-bottom: 0.55rem; border-bottom: 1px solid rgba(255,255,255,0.06); }
  .diag-icon { width: 22px; height: 22px; border-radius: 6px; display: flex; align-items: center; justify-content: center; font-size: 0.75rem; flex-shrink: 0; }
  .diag-icon-general { background: rgba(108,71,255,0.15); border: 1px solid rgba(108,71,255,0.25); }
  .diag-icon-strong { background: rgba(0,212,170,0.12); border: 1px solid rgba(0,212,170,0.22); }
  .diag-icon-problem { background: rgba(247,127,103,0.12); border: 1px solid rgba(247,127,103,0.22); }
  .diag-icon-suggest { background: rgba(249,199,79,0.1); border: 1px solid rgba(249,199,79,0.2); }
  .diag-title-general { color: #A78BFF; }
  .diag-title-strong { color: #00D4AA; }
  .diag-title-problem { color: #F77F67; }
  .diag-title-suggest { color: #F9C74F; }

  .diag-general-text { font-size: 0.88rem; color: rgba(232,230,240,0.75); line-height: 1.65; background: rgba(108,71,255,0.05); border: 1px solid rgba(108,71,255,0.12); border-radius: 10px; padding: 0.9rem 1.1rem; }

  .diag-list { display: flex; flex-direction: column; gap: 0.45rem; }
  .diag-list-item { display: flex; align-items: flex-start; gap: 0.65rem; font-size: 0.86rem; color: rgba(232,230,240,0.72); line-height: 1.55; padding: 0.6rem 0.85rem; border-radius: 8px; }
  .diag-list-item.strong { background: rgba(0,212,170,0.05); border: 1px solid rgba(0,212,170,0.1); }
  .diag-list-item.problem { background: rgba(247,127,103,0.05); border: 1px solid rgba(247,127,103,0.1); }
  .diag-list-item.suggest { background: rgba(249,199,79,0.04); border: 1px solid rgba(249,199,79,0.1); }
  .diag-bullet { width: 6px; height: 6px; border-radius: 50%; flex-shrink: 0; margin-top: 6px; }
  .bullet-strong { background: #00D4AA; }
  .bullet-problem { background: #F77F67; }
  .bullet-suggest { background: #F9C74F; }

  /* ANALYZER LAYOUT */
  .analyzer-results { display: flex; flex-direction: column; gap: 1.1rem; animation: fadeUp 0.4s ease both; }
  .analyzer-top { display: grid; grid-template-columns: 200px 1fr; gap: 1.1rem; }
  @media (max-width: 860px) { .analyzer-top { grid-template-columns: 1fr; } }
  .score-card { background: rgba(255,255,255,0.03); border: 1px solid rgba(255,255,255,0.08); border-radius: 16px; padding: 1.25rem 1.25rem 1rem; }
  .diag-card { background: rgba(255,255,255,0.03); border: 1px solid rgba(255,255,255,0.08); border-radius: 16px; padding: 1.4rem 1.6rem; }
  .metrics-summary { display: grid; grid-template-columns: repeat(3, 1fr); gap: 0.6rem; margin-bottom: 1rem; }
  .metric-pill { background: rgba(255,255,255,0.03); border: 1px solid rgba(255,255,255,0.07); border-radius: 10px; padding: 0.6rem 0.75rem; text-align: center; }
  .metric-pill-val { font-family: 'Syne', sans-serif; font-size: 1rem; font-weight: 700; color: #F0EEF8; }
  .metric-pill-lbl { font-size: 0.65rem; color: rgba(232,230,240,0.35); text-transform: uppercase; letter-spacing: 0.05em; margin-top: 2px; }

`;


const CATEGORIES = ["Urgência", "Curiosidade", "Benefício", "Pergunta", "Prova Social"];
const CAT_MAP = {
  "Urgência": "urgencia",
  "Curiosidade": "curiosidade",
  "Benefício": "beneficio",
  "Pergunta": "pergunta",
  "Prova Social": "social",
};

function copyToClipboard(text, showToast) {
  navigator.clipboard.writeText(text).then(() => showToast("Copiado!"));
}

function CharBadge({ text, max }) {
  const len = text.length;
  const cls = len <= max ? "char-ok" : "char-warn";
  return <span className={`char-count ${cls}`}>{len}/{max}</span>;
}

// ─── ABA 1: Google Ads ───────────────────────────────────────────────────────
function GoogleAdsTab({ showToast }) {
  const [form, setForm] = useState({ product: "", audience: "", benefits: "", url: "" });
  const [loading, setLoading] = useState(false);
  const [result, setResult] = useState(null);

  const handleChange = (e) => setForm(f => ({ ...f, [e.target.name]: e.target.value }));
  const copyAll = (arr) => copyToClipboard(arr.join("\n"), showToast);

  const handleGenerate = async () => {
    if (!form.product.trim()) { showToast("Informe o nome do produto!"); return; }
    setLoading(true); setResult(null);
    const prompt = `Você é um especialista em Google Ads. Crie anúncios de alta conversão para o seguinte produto:

Produto: ${form.product}
Público-alvo: ${form.audience || "não especificado"}
Benefícios: ${form.benefits || "não especificado"}
URL de destino: ${form.url || "não informada"}

Retorne APENAS um JSON válido, sem markdown, sem texto extra, exatamente neste formato:
{"headlines":["h1","h2","h3","h4","h5","h6","h7","h8","h9","h10","h11","h12","h13","h14","h15"],"descriptions":["d1","d2","d3","d4"],"keywords":["k1","k2","k3","k4","k5","k6","k7","k8","k9","k10"],"ctas":["c1","c2","c3","c4","c5"]}

REGRAS:
- Headlines: exatamente 15 itens, ATÉ 30 caracteres cada, em português brasileiro
- Descriptions: exatamente 4 itens, ATÉ 90 caracteres cada, com CTA
- Keywords: exatamente 10 itens, palavras-chave para Google Ads
- CTAs: exatamente 5 itens, 2-5 palavras, em português
Responda APENAS com o JSON.`;
    try {
      const res = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST", headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ model: "claude-sonnet-4-20250514", max_tokens: 1000, messages: [{ role: "user", content: prompt }] })
      });
      const data = await res.json();
      const text = data.content?.map(b => b.text || "").join("") || "";
      setResult(JSON.parse(text.replace(/```json|```/g, "").trim()));
    } catch { showToast("Erro ao gerar. Tente novamente."); }
    finally { setLoading(false); }
  };

  return (
    <div className="main">
      <div className="panel">
        <div className="panel-title">Dados do Anúncio</div>
        <div className="panel-sub">Preencha as informações para gerar seus anúncios Google Ads</div>
        <div className="field"><label>Nome do Produto *</label><input name="product" value={form.product} onChange={handleChange} placeholder="Ex: Curso de Marketing Digital" /></div>
        <div className="field"><label>Público-alvo</label><input name="audience" value={form.audience} onChange={handleChange} placeholder="Ex: Empreendedores de 25-45 anos" /></div>
        <div className="field"><label>Principais Benefícios</label><textarea name="benefits" value={form.benefits} onChange={handleChange} placeholder="Ex: Aprenda em casa, certificado reconhecido, suporte 24h" /></div>
        <div className="field"><label>URL de Destino</label><input name="url" value={form.url} onChange={handleChange} placeholder="https://seusite.com.br/produto" /></div>
        <button className="btn-generate" onClick={handleGenerate} disabled={loading}>
          <div className="btn-inner">{loading ? <><div className="spinner" /> Gerando anúncios...</> : <>✦ Gerar Anúncios com IA</>}</div>
        </button>
      </div>

      <div className="results-panel">
        {!result && !loading && (
          <div className="panel"><div className="empty-state">
            <div className="empty-icon">✦</div>
            <h3>Seus anúncios aparecerão aqui</h3>
            <p>Preencha o formulário ao lado e clique em gerar para criar seus anúncios com IA.</p>
          </div></div>
        )}
        {loading && (
          <div className="panel"><div className="empty-state">
            <div className="spinner" style={{ width: 36, height: 36, borderWidth: 3 }} />
            <p style={{ color: "rgba(232,230,240,0.5)", fontSize: "0.9rem" }}>Criando anúncios otimizados…</p>
          </div></div>
        )}
        {result && <>
          <div className="result-card">
            <div className="card-header">
              <div style={{ display: "flex", alignItems: "center", gap: "0.6rem" }}>
                <span className="card-tag tag-headlines">Headlines</span>
                <span className="card-count">{result.headlines?.length} geradas · máx 30 chars</span>
              </div>
              <button className="copy-btn" onClick={() => copyAll(result.headlines)}>Copiar todas</button>
            </div>
            <div className="headlines-grid">
              {result.headlines?.map((h, i) => (
                <div className="headline-item" key={i} onClick={() => copyToClipboard(h, showToast)} title="Clique para copiar">
                  <span>{h}</span><CharBadge text={h} max={30} />
                </div>
              ))}
            </div>
          </div>

          <div className="result-card">
            <div className="card-header">
              <div style={{ display: "flex", alignItems: "center", gap: "0.6rem" }}>
                <span className="card-tag tag-descriptions">Descrições</span>
                <span className="card-count">{result.descriptions?.length} geradas · máx 90 chars</span>
              </div>
              <button className="copy-btn" onClick={() => copyAll(result.descriptions)}>Copiar todas</button>
            </div>
            <div className="desc-list">
              {result.descriptions?.map((d, i) => (
                <div className="desc-item" key={i} onClick={() => copyToClipboard(d, showToast)} title="Clique para copiar">
                  <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", gap: "0.75rem" }}>
                    <span>{d}</span><CharBadge text={d} max={90} />
                  </div>
                </div>
              ))}
            </div>
          </div>

          <div className="result-card">
            <div className="card-header">
              <div style={{ display: "flex", alignItems: "center", gap: "0.6rem" }}>
                <span className="card-tag tag-keywords">Palavras-chave</span>
                <span className="card-count">{result.keywords?.length} sugeridas</span>
              </div>
              <button className="copy-btn" onClick={() => copyAll(result.keywords)}>Copiar todas</button>
            </div>
            <div className="keywords-wrap">
              {result.keywords?.map((k, i) => <span className="kw-chip" key={i} onClick={() => copyToClipboard(k, showToast)}>{k}</span>)}
            </div>
          </div>

          <div className="result-card">
            <div className="card-header">
              <div style={{ display: "flex", alignItems: "center", gap: "0.6rem" }}>
                <span className="card-tag tag-cta">Call to Action</span>
                <span className="card-count">{result.ctas?.length} opções</span>
              </div>
              <button className="copy-btn" onClick={() => copyAll(result.ctas)}>Copiar todos</button>
            </div>
            <div className="cta-wrap">
              {result.ctas?.map((c, i) => <span className="cta-chip" key={i} onClick={() => copyToClipboard(c, showToast)}>{c}</span>)}
            </div>
          </div>
        </>}
      </div>
    </div>
  );
}

// ─── ABA 2: Gerador de Headlines ─────────────────────────────────────────────
const OBJETIVOS = [
  "Aumentar vendas",
  "Gerar leads",
  "Aumentar engajamento",
  "Promover evento",
  "Lançar produto",
  "Fidelizar clientes",
  "Aumentar tráfego",
  "Divulgar promoção",
];

function HeadlinesTab({ showToast }) {
  const [form, setForm] = useState({ product: "", audience: "", objective: "" });
  const [loading, setLoading] = useState(false);
  const [headlines, setHeadlines] = useState(null);
  const [search, setSearch] = useState("");
  const [activeFilter, setActiveFilter] = useState("all");

  const handleChange = (e) => setForm(f => ({ ...f, [e.target.name]: e.target.value }));

  const handleGenerate = async () => {
    if (!form.product.trim()) { showToast("Informe o nome do produto!"); return; }
    setLoading(true); setHeadlines(null);
    const prompt = `Você é um especialista em copywriting persuasivo. Crie 50 headlines poderosas para o seguinte contexto:

Produto/Serviço: ${form.product}
Público-alvo: ${form.audience || "geral"}
Objetivo: ${form.objective || "aumentar vendas"}

Retorne APENAS um JSON válido, sem markdown, exatamente neste formato:
{"headlines":[{"text":"texto da headline","category":"Urgência"},{"text":"outra headline","category":"Benefício"}]}

REGRAS OBRIGATÓRIAS:
- Exatamente 50 headlines no array
- Cada headline deve ter "text" e "category"
- Categories PERMITIDAS (use exatamente estas): "Urgência", "Curiosidade", "Benefício", "Pergunta", "Prova Social"
- Distribua aproximadamente 10 de cada categoria
- Headlines variadas, criativas e persuasivas em português brasileiro
- Diferentes comprimentos: algumas curtas (3-5 palavras), outras médias, algumas longas
- Use gatilhos mentais, números, poder de urgência, benefícios claros
Responda APENAS com o JSON.`;
    try {
      const res = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST", headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ model: "claude-sonnet-4-20250514", max_tokens: 2000, messages: [{ role: "user", content: prompt }] })
      });
      const data = await res.json();
      const text = data.content?.map(b => b.text || "").join("") || "";
      const parsed = JSON.parse(text.replace(/```json|```/g, "").trim());
      setHeadlines(parsed.headlines);
    } catch { showToast("Erro ao gerar. Tente novamente."); }
    finally { setLoading(false); }
  };

  const filtered = headlines ? headlines.filter(h => {
    const matchSearch = h.text.toLowerCase().includes(search.toLowerCase());
    const matchFilter = activeFilter === "all" || CAT_MAP[h.category] === activeFilter;
    return matchSearch && matchFilter;
  }) : [];

  const copyAll = () => copyToClipboard(filtered.map(h => h.text).join("\n"), showToast);

  return (
    <div className="main-single">
      {/* FORM */}
      <div className="panel" style={{ alignSelf: "start" }}>
        <div className="panel-title">Gerador de Headlines</div>
        <div className="panel-sub">50 headlines persuasivas para qualquer objetivo</div>
        <div className="field"><label>Produto / Serviço *</label><input name="product" value={form.product} onChange={handleChange} placeholder="Ex: Plataforma de cursos online" /></div>
        <div className="field"><label>Público-alvo</label><input name="audience" value={form.audience} onChange={handleChange} placeholder="Ex: Mães empreendedoras" /></div>
        <div className="field">
          <label>Objetivo</label>
          <select name="objective" value={form.objective} onChange={handleChange}>
            <option value="">Selecione um objetivo…</option>
            {OBJETIVOS.map(o => <option key={o} value={o}>{o}</option>)}
          </select>
        </div>
        <button className="btn-generate" onClick={handleGenerate} disabled={loading}>
          <div className="btn-inner">{loading ? <><div className="spinner" /> Gerando headlines...</> : <>✦ Gerar 50 Headlines</>}</div>
        </button>

        {headlines && (
          <div style={{ marginTop: "1.5rem", padding: "1rem", background: "rgba(0,212,170,0.06)", border: "1px solid rgba(0,212,170,0.15)", borderRadius: "12px" }}>
            <div style={{ fontSize: "0.75rem", color: "rgba(232,230,240,0.45)", textTransform: "uppercase", letterSpacing: "0.06em", marginBottom: "0.75rem", fontWeight: 600 }}>Por categoria</div>
            {CATEGORIES.map(cat => {
              const count = headlines.filter(h => h.category === cat).length;
              return (
                <div key={cat} style={{ display: "flex", justifyContent: "space-between", alignItems: "center", padding: "0.3rem 0" }}>
                  <span className={`hl50-cat cat-${CAT_MAP[cat]}`}>{cat}</span>
                  <span style={{ fontSize: "0.8rem", color: "rgba(232,230,240,0.5)", fontWeight: 600 }}>{count}</span>
                </div>
              );
            })}
            <div style={{ borderTop: "1px solid rgba(255,255,255,0.06)", marginTop: "0.75rem", paddingTop: "0.75rem", display: "flex", justifyContent: "space-between", alignItems: "center" }}>
              <span style={{ fontSize: "0.8rem", color: "rgba(232,230,240,0.4)" }}>Total</span>
              <span style={{ fontSize: "0.9rem", fontWeight: 700, color: "#00D4AA", fontFamily: "'Syne', sans-serif" }}>{headlines.length}</span>
            </div>
          </div>
        )}
      </div>

      {/* RESULTS */}
      <div>
        {!headlines && !loading && (
          <div className="panel"><div className="empty-state">
            <div className="empty-icon">✍️</div>
            <h3>50 headlines vão aparecer aqui</h3>
            <p>Preencha os dados ao lado e clique em gerar para criar suas headlines persuasivas.</p>
          </div></div>
        )}
        {loading && (
          <div className="panel"><div className="empty-state">
            <div className="spinner" style={{ width: 36, height: 36, borderWidth: 3 }} />
            <p style={{ color: "rgba(232,230,240,0.5)", fontSize: "0.9rem" }}>Criando 50 headlines persuasivas…</p>
          </div></div>
        )}
        {headlines && (
          <div className="result-card" style={{ animation: "fadeUp 0.4s ease both" }}>
            <div className="card-header">
              <div style={{ display: "flex", alignItems: "center", gap: "0.6rem" }}>
                <span className="card-tag tag-hl50">Headlines</span>
                <span className="card-count">{filtered.length} de {headlines.length}</span>
              </div>
              <button className="copy-btn" onClick={copyAll}>Copiar visíveis</button>
            </div>

            <input className="hl50-search" placeholder="Buscar headline..." value={search} onChange={e => setSearch(e.target.value)} />

            <div className="filter-pills">
              {[["all", "Todas"], ["urgencia", "Urgência"], ["curiosidade", "Curiosidade"], ["beneficio", "Benefício"], ["pergunta", "Pergunta"], ["social", "Prova Social"]].map(([val, label]) => (
                <span key={val} className={`filter-pill ${val} ${activeFilter !== val ? "off" : ""}`} onClick={() => setActiveFilter(val)}>{label}</span>
              ))}
            </div>

            <div className="hl50-scroll">
              <div className="hl50-list">
                {filtered.map((h, i) => (
                  <div key={i}>
                    <div className="hl50-item" onClick={() => copyToClipboard(h.text, showToast)} title="Clique para copiar">
                      <span className="hl50-num">{i + 1}</span>
                      <span className="hl50-text">{h.text}</span>
                      <span className={`hl50-cat cat-${CAT_MAP[h.category] || "urgencia"}`}>{h.category}</span>
                      <span className="hl50-copy">copiar</span>
                    </div>
                    {i < filtered.length - 1 && <div className="hl50-divider" />}
                  </div>
                ))}
                {filtered.length === 0 && (
                  <div style={{ textAlign: "center", padding: "2rem", color: "rgba(232,230,240,0.3)", fontSize: "0.85rem" }}>Nenhuma headline encontrada.</div>
                )}
              </div>
            </div>
          </div>
        )}
      </div>
    </div>
  );
}


// ─── ABA 3: Gerador de Descrições ────────────────────────────────────────────
function DescricoesTab({ showToast }) {
  const [form, setForm] = useState({ product: "", benefits: "" });
  const [loading, setLoading] = useState(false);
  const [result, setResult] = useState(null);
  const [platform, setPlatform] = useState("google");

  const handleChange = (e) => setForm(f => ({ ...f, [e.target.name]: e.target.value }));

  const handleGenerate = async () => {
    if (!form.product.trim()) { showToast("Informe o nome do produto!"); return; }
    setLoading(true); setResult(null);

    const prompt = `Você é um especialista em copywriting para anúncios pagos. Crie descrições persuasivas para:\n\nProduto: ${form.product}\nBenefícios: ${form.benefits || "não especificado"}\n\nRetorne APENAS um JSON válido, sem markdown:\n{"google":["d1","d2","d3","d4","d5","d6","d7","d8","d9","d10","d11","d12","d13","d14","d15","d16","d17","d18","d19","d20"],"facebook":["d1","d2","d3","d4","d5","d6","d7","d8","d9","d10","d11","d12","d13","d14","d15","d16","d17","d18","d19","d20"]}\n\nREGRAS:\n- Google: 20 descrições, ATÉ 90 chars cada, diretas, com CTA\n- Facebook: 20 descrições, ATÉ 125 chars cada, mais emotivas e conversacionais\n- Todas em português brasileiro, variadas, sem repetição de estrutura\nResponda APENAS com o JSON.`;

    try {
      const res = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST", headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ model: "claude-sonnet-4-20250514", max_tokens: 2500, messages: [{ role: "user", content: prompt }] })
      });
      const data = await res.json();
      const text = data.content?.map(b => b.text || "").join("") || "";
      const parsed = JSON.parse(text.replace(/```json|```/g, "").trim());
      setResult(parsed);
    } catch { showToast("Erro ao gerar. Tente novamente."); }
    finally { setLoading(false); }
  };

  const activeList = result ? (platform === "google" ? result.google : result.facebook) : [];
  const maxChars = platform === "google" ? 90 : 125;

  const copyPlatform = () => copyToClipboard(activeList.join("\n"), showToast);
  const copyAll = () => {
    if (!result) return;
    const all = ["=== GOOGLE ADS ===", ...result.google, "", "=== FACEBOOK ADS ===", ...result.facebook].join("\n");
    copyToClipboard(all, showToast);
  };

  return (
    <div className="main-single">
      <div className="panel" style={{ alignSelf: "start" }}>
        <div className="panel-title">Gerador de Descrições</div>
        <div className="panel-sub">20 descrições para Google Ads + 20 para Facebook Ads</div>
        <div className="field">
          <label>Nome do Produto *</label>
          <input name="product" value={form.product} onChange={handleChange} placeholder="Ex: Software de gestão financeira" />
        </div>
        <div className="field">
          <label>Benefícios do Produto</label>
          <textarea name="benefits" value={form.benefits} onChange={handleChange} placeholder="Ex: Economize 3h por dia, relatórios automáticos, integração com bancos..." />
        </div>
        <button className="btn-generate" onClick={handleGenerate} disabled={loading}>
          <div className="btn-inner">{loading ? <><div className="spinner" /> Gerando descrições...</> : <>✦ Gerar 40 Descrições</>}</div>
        </button>

        {result && (
          <div style={{ marginTop: "1.5rem", padding: "1rem", background: "rgba(247,127,103,0.06)", border: "1px solid rgba(247,127,103,0.15)", borderRadius: "12px" }}>
            <div style={{ fontSize: "0.75rem", color: "rgba(232,230,240,0.45)", textTransform: "uppercase", letterSpacing: "0.06em", marginBottom: "0.85rem", fontWeight: 600 }}>Resumo gerado</div>
            {[
              { label: "Google Ads", count: result.google?.length, limit: "90 chars", color: "#5A9BFF", bg: "rgba(66,133,244,0.1)", icon: "G" },
              { label: "Facebook Ads", count: result.facebook?.length, limit: "125 chars", color: "#60AAFF", bg: "rgba(24,119,242,0.1)", icon: "f" },
            ].map(({ label, count, limit, color, bg, icon }) => (
              <div key={label} style={{ display: "flex", alignItems: "center", justifyContent: "space-between", padding: "0.45rem 0", borderBottom: "1px solid rgba(255,255,255,0.05)" }}>
                <div style={{ display: "flex", alignItems: "center", gap: "0.55rem" }}>
                  <div style={{ width: 22, height: 22, borderRadius: 5, background: bg, display: "flex", alignItems: "center", justifyContent: "center", fontSize: "0.65rem", fontWeight: 900, color }}>{icon}</div>
                  <span style={{ fontSize: "0.82rem", color: "rgba(232,230,240,0.65)" }}>{label}</span>
                </div>
                <div style={{ display: "flex", alignItems: "center", gap: "0.5rem" }}>
                  <span style={{ fontSize: "0.7rem", color: "rgba(232,230,240,0.3)" }}>máx {limit}</span>
                  <span style={{ fontSize: "0.88rem", fontWeight: 700, color, fontFamily: "'Syne', sans-serif" }}>{count}</span>
                </div>
              </div>
            ))}
            <div style={{ marginTop: "0.85rem" }}>
              <button className="btn-copy-all" style={{ width: "100%" }} onClick={copyAll}>↓ Copiar todas as 40 descrições</button>
            </div>
          </div>
        )}
      </div>

      <div>
        {!result && !loading && (
          <div className="panel"><div className="empty-state">
            <div className="empty-icon">📝</div>
            <h3>Descrições vão aparecer aqui</h3>
            <p>Preencha o formulário ao lado para gerar 20 descrições para Google e 20 para Facebook Ads.</p>
          </div></div>
        )}
        {loading && (
          <div className="panel"><div className="empty-state">
            <div className="spinner" style={{ width: 36, height: 36, borderWidth: 3 }} />
            <p style={{ color: "rgba(232,230,240,0.5)", fontSize: "0.9rem" }}>Criando 40 descrições persuasivas…</p>
          </div></div>
        )}
        {result && (
          <div style={{ display: "flex", flexDirection: "column", gap: "1.25rem", animation: "fadeUp 0.4s ease both" }}>
            <div className="result-card" style={{ padding: "1.25rem 1.5rem" }}>
              <div className="copy-all-banner">
                <span className="copy-all-info">Total: <strong>40 descrições</strong> prontas para usar</span>
                <button className="btn-copy-all" onClick={copyAll}>Copiar todas ↓</button>
              </div>
              <div className="platform-toggle">
                <button className={`platform-btn ${platform === "google" ? "active-google" : "inactive-plat"}`} onClick={() => setPlatform("google")}>
                  <div style={{ width: 14, height: 14, borderRadius: "50%", background: "linear-gradient(135deg,#EA4335 0%,#FBBC05 33%,#34A853 66%,#4285F4 100%)", flexShrink: 0 }} />
                  Google Ads
                  <span style={{ fontSize: "0.7rem", opacity: 0.75, fontWeight: 400 }}>20</span>
                </button>
                <button className={`platform-btn ${platform === "facebook" ? "active-facebook" : "inactive-plat"}`} onClick={() => setPlatform("facebook")}>
                  <div style={{ width: 14, height: 14, borderRadius: 3, background: "#1877F2", display: "flex", alignItems: "center", justifyContent: "center", fontSize: "0.6rem", fontWeight: 900, color: "#fff", flexShrink: 0 }}>f</div>
                  Facebook Ads
                  <span style={{ fontSize: "0.7rem", opacity: 0.75, fontWeight: 400 }}>20</span>
                </button>
              </div>
              <div style={{ display: "flex", alignItems: "center", justifyContent: "space-between", marginBottom: "0.85rem" }}>
                <div style={{ display: "flex", alignItems: "center", gap: "0.6rem" }}>
                  <span className={`card-tag ${platform === "google" ? "tag-google" : "tag-facebook"}`}>
                    {platform === "google" ? "Google Ads" : "Facebook Ads"}
                  </span>
                  <span className="card-count">{activeList.length} descrições · máx {maxChars} chars</span>
                </div>
                <button className="copy-btn" onClick={copyPlatform}>Copiar estas {activeList.length}</button>
              </div>
              <div className="desc-scroll">
                {activeList.map((d, i) => (
                  <div key={`${platform}-${i}`} className="desc-gen-item" onClick={() => copyToClipboard(d, showToast)} title="Clique para copiar">
                    <div className="desc-gen-body">
                      <span className="desc-gen-num">{i + 1}</span>
                      <span className="desc-gen-text">{d}</span>
                      <span className="desc-gen-copy">copiar</span>
                    </div>
                    <div className="desc-gen-chars"><CharBadge text={d} max={maxChars} /></div>
                  </div>
                ))}
              </div>
            </div>
          </div>
        )}
      </div>
    </div>
  );
}


// ─── ABA 4: Analisador de Campanhas ──────────────────────────────────────────
function scoreColor(n) {
  if (n >= 8) return "#00D4AA";
  if (n >= 6) return "#F9C74F";
  if (n >= 4) return "#F77F67";
  return "#E85C5C";
}
function scoreVerdict(n) {
  if (n >= 9) return "Campanha Excelente";
  if (n >= 8) return "Muito Boa";
  if (n >= 7) return "Boa Performance";
  if (n >= 6) return "Desempenho Médio";
  if (n >= 5) return "Abaixo do Esperado";
  if (n >= 3) return "Campanha Fraca";
  return "Crítica — Revise Urgente";
}

function ScoreRing({ score }) {
  const r = 55;
  const circ = 2 * Math.PI * r;
  const pct = Math.max(0, Math.min(10, score)) / 10;
  const offset = circ * (1 - pct);
  const color = scoreColor(score);
  return (
    <div className="score-section">
      <div className="score-ring-wrap">
        <svg className="score-ring-svg" viewBox="0 0 130 130">
          <circle className="score-ring-bg" cx="65" cy="65" r={r} />
          <circle className="score-ring-fill" cx="65" cy="65" r={r}
            stroke={color} strokeDasharray={circ} strokeDashoffset={offset} />
        </svg>
        <div className="score-center">
          <div className="score-number" style={{ color }}>{score}</div>
          <div className="score-label">/ 10</div>
        </div>
      </div>
      <div className="score-verdict" style={{ color }}>{scoreVerdict(score)}</div>
      <div className="score-sub">Nota geral da campanha</div>
    </div>
  );
}

function DiagList({ items, type }) {
  const bulletClass = { strong: "bullet-strong", problem: "bullet-problem", suggest: "bullet-suggest" }[type];
  return (
    <div className="diag-list">
      {items.map((item, i) => (
        <div key={i} className={`diag-list-item ${type}`}>
          <span className={`diag-bullet ${bulletClass}`} />
          <span>{item}</span>
        </div>
      ))}
    </div>
  );
}

function AnalysisTab({ showToast }) {
  const [form, setForm] = useState({
    impressions: "", clicks: "", ctr: "", cpc: "", conversions: "", cpa: "", investment: ""
  });
  const [loading, setLoading] = useState(false);
  const [result, setResult] = useState(null);

  const handleChange = (e) => setForm(f => ({ ...f, [e.target.name]: e.target.value }));

  const handleAnalyze = async () => {
    const filled = Object.values(form).filter(v => v.trim() !== "");
    if (filled.length < 3) { showToast("Preencha ao menos 3 métricas!"); return; }
    setLoading(true); setResult(null);

    const prompt = `Você é um especialista em marketing digital e análise de campanhas Google Ads / Facebook Ads. Analise os dados abaixo e retorne um diagnóstico completo.

DADOS DA CAMPANHA:
- Impressões: ${form.impressions || "não informado"}
- Cliques: ${form.clicks || "não informado"}
- CTR: ${form.ctr || "não informado"}%
- CPC médio: R$ ${form.cpc || "não informado"}
- Conversões: ${form.conversions || "não informado"}
- CPA (custo por aquisição): R$ ${form.cpa || "não informado"}
- Investimento total: R$ ${form.investment || "não informado"}

Retorne APENAS um JSON válido, sem markdown:
{
  "score": 7,
  "general": "Parágrafo de diagnóstico geral em 2-3 frases, comentando a saúde geral da campanha e os principais achados.",
  "strengths": ["ponto forte 1","ponto forte 2","ponto forte 3"],
  "problems": ["problema 1","problema 2","problema 3"],
  "suggestions": ["sugestão 1","sugestão 2","sugestão 3","sugestão 4"]
}

REGRAS:
- score: número inteiro de 0 a 10 baseado nos dados fornecidos
- general: 2-3 frases de diagnóstico objetivo em português
- strengths: array com 2-4 pontos fortes específicos (baseados nos dados)
- problems: array com 2-4 problemas identificados (baseados nos dados)
- suggestions: array com 3-5 sugestões de melhoria práticas e acionáveis
- Se alguma métrica estiver faltando, considere isso como ponto de atenção
- Seja específico e técnico, mencione os valores fornecidos nas análises
Responda APENAS com o JSON.`;

    try {
      const res = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST", headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ model: "claude-sonnet-4-20250514", max_tokens: 1200, messages: [{ role: "user", content: prompt }] })
      });
      const data = await res.json();
      const text = data.content?.map(b => b.text || "").join("") || "";
      const parsed = JSON.parse(text.replace(/```json|```/g, "").trim());
      setResult(parsed);
    } catch { showToast("Erro ao analisar. Tente novamente."); }
    finally { setLoading(false); }
  };

  const metricFields = [
    { name: "impressions", label: "Impressões", placeholder: "Ex: 45000", suffix: null },
    { name: "clicks", label: "Cliques", placeholder: "Ex: 1350", suffix: null },
    { name: "ctr", label: "CTR", placeholder: "Ex: 3.0", suffix: "%" },
    { name: "cpc", label: "CPC Médio", placeholder: "Ex: 2.50", suffix: "R$" },
    { name: "conversions", label: "Conversões", placeholder: "Ex: 27", suffix: null },
    { name: "cpa", label: "CPA", placeholder: "Ex: 125.00", suffix: "R$" },
    { name: "investment", label: "Investimento", placeholder: "Ex: 3375.00", suffix: "R$" },
  ];

  const filledMetrics = [
    form.impressions && { label: "Impressões", val: Number(form.impressions).toLocaleString("pt-BR") },
    form.clicks && { label: "Cliques", val: Number(form.clicks).toLocaleString("pt-BR") },
    form.ctr && { label: "CTR", val: form.ctr + "%" },
    form.cpc && { label: "CPC", val: "R$ " + form.cpc },
    form.conversions && { label: "Conversões", val: form.conversions },
    form.cpa && { label: "CPA", val: "R$ " + form.cpa },
    form.investment && { label: "Investimento", val: "R$ " + form.investment },
  ].filter(Boolean).slice(0, 6);

  return (
    <div className="main-single">
      {/* FORM */}
      <div className="panel" style={{ alignSelf: "start" }}>
        <div className="panel-title">Analisador de Campanhas</div>
        <div className="panel-sub">Insira as métricas da sua campanha para receber um diagnóstico completo com IA</div>

        <div className="metrics-grid">
          {metricFields.map(({ name, label, placeholder, suffix }) => (
            <div className="metric-field" key={name} style={name === "investment" ? { gridColumn: "1 / -1" } : {}}>
              <label>{label}</label>
              <div className="metric-input-wrap">
                <input
                  className="metric-input"
                  name={name}
                  value={form[name]}
                  onChange={handleChange}
                  placeholder={placeholder}
                  type="number"
                  min="0"
                  style={suffix ? { paddingRight: suffix === "R$" ? "2.5rem" : "2rem" } : {}}
                />
                {suffix && <span className="metric-suffix">{suffix}</span>}
              </div>
            </div>
          ))}
        </div>

        <button className="btn-generate" style={{ marginTop: "1rem" }} onClick={handleAnalyze} disabled={loading}>
          <div className="btn-inner">{loading ? <><div className="spinner" /> Analisando campanha...</> : <>⚡ Analisar Campanha com IA</>}</div>
        </button>

        {filledMetrics.length > 0 && !result && (
          <div style={{ marginTop: "1.25rem", background: "rgba(249,199,79,0.05)", border: "1px solid rgba(249,199,79,0.12)", borderRadius: "12px", padding: "0.9rem 1rem" }}>
            <div style={{ fontSize: "0.7rem", fontWeight: 700, letterSpacing: "0.07em", textTransform: "uppercase", color: "rgba(249,199,79,0.6)", marginBottom: "0.65rem" }}>Métricas inseridas</div>
            <div style={{ display: "flex", flexWrap: "wrap", gap: "0.4rem" }}>
              {filledMetrics.map(({ label, val }) => (
                <div key={label} style={{ background: "rgba(249,199,79,0.07)", border: "1px solid rgba(249,199,79,0.15)", borderRadius: "6px", padding: "0.28rem 0.65rem", fontSize: "0.75rem", color: "#F9C74F" }}>
                  <span style={{ color: "rgba(232,230,240,0.4)", marginRight: "0.3rem" }}>{label}:</span>{val}
                </div>
              ))}
            </div>
          </div>
        )}
      </div>

      {/* RESULTS */}
      <div>
        {!result && !loading && (
          <div className="panel"><div className="empty-state">
            <div className="empty-icon">⚡</div>
            <h3>Diagnóstico aparecerá aqui</h3>
            <p>Insira as métricas da sua campanha ao lado para receber uma análise completa com IA.</p>
          </div></div>
        )}
        {loading && (
          <div className="panel"><div className="empty-state">
            <div className="spinner" style={{ width: 36, height: 36, borderWidth: 3 }} />
            <p style={{ color: "rgba(232,230,240,0.5)", fontSize: "0.9rem" }}>Analisando sua campanha…</p>
          </div></div>
        )}
        {result && (
          <div className="analyzer-results">
            {/* Top row: score + general */}
            <div className="analyzer-top">
              <div className="score-card">
                <ScoreRing score={result.score} />
              </div>
              <div className="diag-card">
                {filledMetrics.length > 0 && (
                  <div className="metrics-summary">
                    {filledMetrics.slice(0, 6).map(({ label, val }) => (
                      <div className="metric-pill" key={label}>
                        <div className="metric-pill-val">{val}</div>
                        <div className="metric-pill-lbl">{label}</div>
                      </div>
                    ))}
                  </div>
                )}
                <div className="diag-section">
                  <div className="diag-section-title diag-title-general">
                    <span className="diag-icon diag-icon-general">🔍</span>
                    Diagnóstico Geral
                  </div>
                  <div className="diag-general-text">{result.general}</div>
                </div>
              </div>
            </div>

            {/* Strengths + Problems side by side */}
            <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: "1.1rem" }}>
              <div className="diag-card">
                <div className="diag-section">
                  <div className="diag-section-title diag-title-strong">
                    <span className="diag-icon diag-icon-strong">✓</span>
                    Pontos Fortes
                  </div>
                  <DiagList items={result.strengths || []} type="strong" />
                </div>
              </div>
              <div className="diag-card">
                <div className="diag-section">
                  <div className="diag-section-title diag-title-problem">
                    <span className="diag-icon diag-icon-problem">!</span>
                    Problemas Encontrados
                  </div>
                  <DiagList items={result.problems || []} type="problem" />
                </div>
              </div>
            </div>

            {/* Suggestions full width */}
            <div className="diag-card">
              <div className="diag-section" style={{ marginBottom: 0 }}>
                <div className="diag-section-title diag-title-suggest">
                  <span className="diag-icon diag-icon-suggest">★</span>
                  Sugestões de Melhoria
                </div>
                <DiagList items={result.suggestions || []} type="suggest" />
              </div>
            </div>
          </div>
        )}
      </div>
    </div>
  );
}

// ─── ROOT ─────────────────────────────────────────────────────────────────────
export default function AdMakerAI() {
  const [activeTab, setActiveTab] = useState("ads");
  const [toast, setToast] = useState("");

  const showToast = (msg) => { setToast(msg); setTimeout(() => setToast(""), 2200); };

  return (
    <>
      <style>{styles}</style>
      <div className="app">
        <nav className="nav">
          <div style={{ display: "flex", alignItems: "center", gap: "0.75rem" }}>
            <span className="nav-logo">AdMaker AI</span>
            <span className="nav-badge">Beta</span>
          </div>
          <div className="nav-right">
            <span className="nav-link">Recursos</span>
            <span className="nav-link">Preços</span>
            <button className="btn-nav">Começar grátis</button>
          </div>
        </nav>

        <div className="hero">
          <div className="hero-glow" />
          <div className="hero-eyebrow"><span className="hero-dot" /> Powered by Claude AI</div>
          <h1>Crie anúncios e headlines<br /><span>que convertem de verdade</span></h1>
          <p>Gere headlines, descrições, palavras-chave e CTAs otimizados com inteligência artificial em segundos.</p>
        </div>

        <div className="stats-bar">
          {[["15", "Headlines Ads"], ["50", "Headlines"], ["40", "Descrições"], ["10", "Diagnóstico"]].map(([n, l]) => (
            <div className="stat" key={l}><div className="stat-num">{n}</div><div className="stat-label">{l}</div></div>
          ))}
        </div>

        <div className="tabs-wrapper">
          <div className="tabs">
            <button className={`tab-btn ${activeTab === "ads" ? "active" : "inactive"}`} onClick={() => setActiveTab("ads")}>
              <span className="tab-dot tab-dot-ads" />
              Google Ads
            </button>
            <button className={`tab-btn ${activeTab === "headlines" ? "active" : "inactive"}`} onClick={() => setActiveTab("headlines")}>
              <span className="tab-dot tab-dot-hl" />
              Gerador de Headlines
            </button>
            <button className={`tab-btn ${activeTab === "descricoes" ? "active" : "inactive"}`} onClick={() => setActiveTab("descricoes")}>
              <span className="tab-dot tab-dot-desc" />
              Gerador de Descrições
            </button>
            <button className={`tab-btn ${activeTab === "analyzer" ? "active" : "inactive"}`} onClick={() => setActiveTab("analyzer")}>
              <span className="tab-dot tab-dot-analyzer" />
              Analisador de Campanhas
            </button>
          </div>
        </div>

        {activeTab === "ads" && <GoogleAdsTab showToast={showToast} />}
        {activeTab === "headlines" && <HeadlinesTab showToast={showToast} />}
        {activeTab === "descricoes" && <DescricoesTab showToast={showToast} />}
        {activeTab === "analyzer" && <AnalysisTab showToast={showToast} />}

        {toast && <div className="toast">{toast}</div>}
      </div>
    </>
  );
}
