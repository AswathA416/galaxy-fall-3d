/* Layout: full-bleed WebGL scene, glass HUD at the edges, a galaxy-bordered launch box in the lobby. No external files. */
  @property --ang { syntax: '<angle>'; initial-value: 0deg; inherits: false; }
  :root {
    --bg: #04030d; --fg: #ece9ff; --muted: #a8a4d8; --accent: #8ff0ff; --danger: #ff4f8b; --ok: #5cffb0; --shield: #5cb8ff; --gold: #ffd36b;
    --line: #3a3688;
    --font: ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", sans-serif;
    color-scheme: dark;
  }
  [hidden] { display: none !important; }
  html, body { height: 100%; }
  body {
    margin: 0; background: var(--bg); color: var(--fg); font-family: var(--font); overflow: hidden;
    padding-left: max(16px, env(safe-area-inset-left, 0px)); padding-right: max(16px, env(safe-area-inset-right, 0px));
    box-sizing: border-box; touch-action: none; user-select: none; -webkit-user-select: none;
  }
  canvas#c { position: fixed; inset: 0; width: 100%; height: 100%; display: block; }
  #nogl { position: fixed; inset: 0; display: flex; align-items: center; justify-content: center; padding: 24px; text-align: center; line-height: 1.5; }

  /* ---- Lobby: the galaxy box ---- */
  #ui {
    position: fixed; inset: 0; display: flex; align-items: center; justify-content: center; overflow-y: auto; touch-action: pan-y;
    padding: env(safe-area-inset-top, 0px) max(16px, env(safe-area-inset-right, 0px)) env(safe-area-inset-bottom, 0px) max(16px, env(safe-area-inset-left, 0px));
  }
  #ui.off { display: none; }
  #menu {
    position: relative; width: min(600px, 100%); box-sizing: border-box; margin: 16px 0; padding: 28px 24px;
    display: flex; flex-direction: column; gap: 16px; text-align: center; border-radius: 20px; border: 2px solid transparent;
    background:
      radial-gradient(1.3px 1.3px at 12% 22%, #fff, transparent 60%), radial-gradient(1px 1px at 38% 70%, #cfe9ff, transparent 60%),
      radial-gradient(1.4px 1.4px at 66% 18%, #ffe9cf, transparent 60%), radial-gradient(1px 1px at 84% 62%, #fff, transparent 60%),
      radial-gradient(1.2px 1.2px at 52% 44%, #e6d4ff, transparent 60%),
      radial-gradient(1px 1px at 8% 82%, #fff, transparent 60%), radial-gradient(1.2px 1.2px at 92% 90%, #cfe9ff, transparent 60%),
      radial-gradient(ellipse at 15% 0%, rgba(120, 60, 220, 0.55), transparent 55%),
      radial-gradient(ellipse at 90% 100%, rgba(30, 140, 230, 0.45), transparent 55%),
      radial-gradient(ellipse at 50% 60%, rgba(220, 60, 160, 0.14), transparent 60%),
      linear-gradient(rgba(9, 6, 30, 0.9), rgba(5, 4, 20, 0.93)),
      conic-gradient(from var(--ang), #7cf3ff, #a56bff, #ff4fb0, #ffd36b, #5cffb0, #7cf3ff);
    background-origin: padding-box; background-clip: padding-box, padding-box, padding-box, padding-box, padding-box, padding-box, padding-box, padding-box, padding-box, padding-box, padding-box, border-box;
    background-size: 220px 220px, 160px 160px, 260px 260px, 190px 190px, 300px 300px, 240px 240px, 180px 180px, auto, auto, auto, auto, auto;
    background-repeat: repeat, repeat, repeat, repeat, repeat, repeat, repeat, no-repeat, no-repeat, no-repeat, no-repeat, no-repeat;
    box-shadow: 0 0 50px rgba(130, 90, 255, 0.4), 0 0 140px rgba(60, 190, 255, 0.18), inset 0 0 40px rgba(120, 90, 255, 0.12);
    animation: spin 9s linear infinite, drift 40s linear infinite;
  }
  @keyframes spin { to { --ang: 360deg; } }
  @keyframes drift {
    from { background-position: 0 0, 0 0, 0 0, 0 0, 0 0, 0 0, 0 0, 0 0, 0 0, 0 0, 0 0, 0 0; }
    to { background-position: -220px 220px, -160px 320px, 260px 260px, -190px 380px, 300px -300px, 240px 480px, -180px 360px, 0 0, 0 0, 0 0, 0 0, 0 0; }
  }
  h1 {
    margin: 0; font-size: clamp(30px, 9vw, 54px); letter-spacing: 0.2em; font-weight: 800; color: var(--accent); text-wrap: balance;
    text-shadow: 0 0 22px rgba(120, 220, 255, 0.6), 0 0 64px rgba(170, 90, 255, 0.55);
  }
  .tag { color: var(--muted); font-size: 12px; letter-spacing: 0.22em; text-transform: uppercase; margin-top: -8px; }
  .sub { color: var(--muted); font-size: 13px; line-height: 1.6; text-align: left; }
  .sub b { color: var(--fg); font-weight: 700; }
  .over { font-weight: 800; font-size: 24px; letter-spacing: 0.08em; }
  .over.win { color: var(--gold); text-shadow: 0 0 24px rgba(255, 200, 80, 0.6); }
  .over.lose { color: var(--danger); }
  .stats { font-variant-numeric: tabular-nums; font-size: 15px; line-height: 1.6; }
  .skins { display: flex; flex-wrap: wrap; gap: 8px; justify-content: center; }
  button {
    font: inherit; cursor: pointer; color: var(--fg); background: rgba(14, 14, 50, 0.85); border: 1px solid var(--line);
    border-radius: 9px; padding: 8px 14px; min-width: 88px;
  }
  button:focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; }
  button small { display: block; color: var(--muted); font-size: 12px; }
  button.skin { color: var(--sk); }
  button.skin[aria-pressed="true"] { border-color: var(--sk); box-shadow: 0 0 14px var(--sk); }
  button.skin:disabled { opacity: 0.42; cursor: not-allowed; }
  #go { font-size: 20px; font-weight: 800; letter-spacing: 0.1em; padding: 13px 40px; background: linear-gradient(90deg, #7cf3ff, #b58cff); color: #05121c; border: 0; box-shadow: 0 0 28px rgba(140, 150, 255, 0.5); }

  /* ---- In-game HUD ---- */
  .gx { background: rgba(8, 6, 28, 0.62); border: 1px solid rgba(120, 110, 255, 0.45); border-radius: 10px; backdrop-filter: blur(3px); box-shadow: 0 0 18px rgba(110, 80, 255, 0.2); }
  #hud { position: fixed; inset: 0; pointer-events: none; font-weight: 700; font-variant-numeric: tabular-nums; text-shadow: 0 0 6px #000; }
  #topc { position: absolute; top: calc(env(safe-area-inset-top, 0px) + 10px); left: 50%; transform: translateX(-50%); padding: 6px 16px; font-size: 14px; white-space: nowrap; text-align: center; }
  #stormmsg { display: block; font-size: 12px; color: #d9a6ff; margin-top: 2px; letter-spacing: 0.04em; }
  #feed { position: absolute; top: calc(env(safe-area-inset-top, 0px) + 10px); left: max(12px, env(safe-area-inset-left, 0px)); font-size: 13px; display: flex; flex-direction: column; gap: 4px; max-width: 42vw; }
  #feed div { padding: 3px 9px; }
  #mapbox { position: absolute; top: calc(env(safe-area-inset-top, 0px) + 10px); right: max(12px, env(safe-area-inset-right, 0px)); width: 150px; height: 150px; border-radius: 50%; overflow: hidden; }
  #map { width: 150px; height: 150px; display: block; }
  #bars { position: absolute; left: max(14px, env(safe-area-inset-left, 0px)); bottom: calc(env(safe-area-inset-bottom, 0px) + 14px); width: min(260px, 44vw); padding: 8px 10px; }
  .bar { height: 14px; border-radius: 7px; background: rgba(20, 22, 60, 0.85); overflow: hidden; position: relative; margin: 3px 0; }
  .bar i { display: block; height: 100%; width: 100%; }
  .bar span { position: absolute; right: 8px; top: -1px; font-size: 12px; }
  #hpf { background: linear-gradient(90deg, #2fd68a, #5cffb0); }
  #shf { background: linear-gradient(90deg, #2f8fe0, #5cb8ff); }
  #slots { position: absolute; right: max(14px, env(safe-area-inset-right, 0px)); bottom: calc(env(safe-area-inset-bottom, 0px) + 14px); display: flex; flex-direction: column; align-items: flex-end; gap: 6px; }
  #slotrow { display: flex; gap: 6px; }
  .slot { min-width: 62px; padding: 6px 8px; text-align: center; font-size: 11px; border-width: 2px; opacity: 0.8; }
  .slot b { display: block; font-size: 10px; color: var(--muted); }
  .slot.sel { opacity: 1; box-shadow: 0 0 16px currentColor; transform: translateY(-4px); }
  #res { padding: 5px 12px; font-size: 13px; }
  #cross { position: absolute; left: 50%; top: 50%; width: 22px; height: 22px; margin: -11px 0 0 -11px; }
  #cross::before, #cross::after { content: ""; position: absolute; background: #fff; box-shadow: 0 0 4px #000; }
  #cross::before { left: 10px; top: 0; width: 2px; height: 22px; -webkit-mask: linear-gradient(#000 0 30%, transparent 30% 70%, #000 70%); mask: linear-gradient(#000 0 30%, transparent 30% 70%, #000 70%); }
  #cross::after { top: 10px; left: 0; height: 2px; width: 22px; -webkit-mask: linear-gradient(90deg, #000 0 30%, transparent 30% 70%, #000 70%); mask: linear-gradient(90deg, #000 0 30%, transparent 30% 70%, #000 70%); }
  #cross.hit::before, #cross.hit::after { background: var(--danger); }
  #cross.build { display: none; }
  #toast { position: absolute; left: 0; right: 0; top: 24%; text-align: center; font-size: 22px; letter-spacing: 0.06em; opacity: 0; transition: opacity 0.25s; color: var(--accent); }
  #toast.show { opacity: 1; }
  #buildhint { position: absolute; left: 50%; transform: translateX(-50%); bottom: calc(env(safe-area-inset-bottom, 0px) + 86px); padding: 6px 14px; font-size: 13px; color: var(--accent); white-space: nowrap; }
  #hurt { position: absolute; inset: 0; background: radial-gradient(ellipse at center, transparent 45%, rgba(255, 30, 90, 0.55)); opacity: 0; transition: opacity 0.2s; }
  #touch { position: fixed; inset: 0; pointer-events: none; }
  #touch button { pointer-events: auto; position: absolute; min-width: 0; width: 64px; height: 64px; border-radius: 50%; padding: 0; font-size: 12px; font-weight: 800; opacity: 0.85; }
  #tFire { right: max(18px, env(safe-area-inset-right, 0px)); bottom: calc(env(safe-area-inset-bottom, 0px) + 120px); width: 78px !important; height: 78px !important; background: rgba(255, 79, 139, 0.6); }
  #tJump { right: max(104px, env(safe-area-inset-right, 0px)); bottom: calc(env(safe-area-inset-bottom, 0px) + 90px); }
  #tBuild { right: max(18px, env(safe-area-inset-right, 0px)); bottom: calc(env(safe-area-inset-bottom, 0px) + 210px); }
  #tWeap { right: max(104px, env(safe-area-inset-right, 0px)); bottom: calc(env(safe-area-inset-bottom, 0px) + 170px); }
  #mute { position: fixed; left: max(10px, env(safe-area-inset-left, 0px)); top: calc(env(safe-area-inset-top, 0px) + 10px); min-width: 0; padding: 5px 9px; font-size: 11px; opacity: 0.7; z-index: 6; }
  #ui:not(.off) ~ #mute { top: auto; bottom: calc(env(safe-area-inset-bottom, 0px) + 10px); }
  /* ---- loader ---- */
  #loader { position: fixed; inset: 0; z-index: 50; display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 18px; background: radial-gradient(ellipse at 50% 40%, #1a0f40, #04030d 70%); transition: opacity 0.6s; }
  #loader.gone { opacity: 0; pointer-events: none; }
  #loader .lg { font-size: clamp(32px, 9vw, 64px); font-weight: 800; letter-spacing: 0.22em; color: var(--accent); text-shadow: 0 0 30px rgba(120, 220, 255, 0.7); }
  #loader .ld { color: var(--muted); letter-spacing: 0.12em; font-size: 14px; text-transform: uppercase; }
  #loader .ld b { color: var(--fg); }
  .lbar { width: min(320px, 70vw); height: 8px; border-radius: 4px; background: rgba(40, 40, 90, 0.7); overflow: hidden; }
  #lbf { display: block; height: 100%; width: 0; background: linear-gradient(90deg, #7cf3ff, #b58cff); transition: width 0.2s; }
  /* ---- tabs and panes ---- */
  .tabs { display: flex; gap: 6px; justify-content: center; flex-wrap: wrap; }
  .tab { min-width: 0; padding: 7px 13px; font-size: 13px; letter-spacing: 0.06em; }
  .tab[aria-pressed="true"] { background: var(--accent); color: #05121c; border-color: var(--accent); }
  .pane { display: flex; flex-direction: column; gap: 14px; align-items: center; }
  .pane[hidden] { display: none !important; }
  .row { width: 100%; display: flex; gap: 10px; align-items: center; justify-content: space-between; padding: 9px 12px; border-radius: 10px; background: rgba(12, 10, 40, 0.7); border: 1px solid var(--line); box-sizing: border-box; text-align: left; }
  .row small { color: var(--muted); display: block; font-size: 12px; }
  .grid { width: 100%; display: grid; grid-template-columns: repeat(auto-fill, minmax(140px, 1fr)); gap: 10px; }
  .card { text-align: left; padding: 10px; border-radius: 12px; background: rgba(12, 10, 40, 0.75); border: 1px solid var(--line); display: flex; flex-direction: column; gap: 6px; }
  .card .sw { height: 46px; border-radius: 8px; }
  .card.own { border-color: #5cffb0; }
  .card button { min-width: 0; }
  .pill { display: inline-block; padding: 2px 8px; border-radius: 9px; font-size: 11px; letter-spacing: 0.06em; }
  .field { display: flex; flex-direction: column; gap: 4px; width: 100%; text-align: left; font-size: 13px; }
  .field input[type="range"] { width: 100%; }
  .field input[type="text"], .field select { background: #0b0a24; color: var(--fg); border: 1px solid var(--line); border-radius: 7px; padding: 6px 8px; font: inherit; }
  .sw2 { display: flex; gap: 8px; align-items: center; }
  @media (prefers-reduced-motion: reduce) { #menu { animation: none; } #toast, #hurt, #loader { transition: none; } }
