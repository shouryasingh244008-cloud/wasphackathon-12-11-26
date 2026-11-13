
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>WattWise — Smart Home Energy Monitor</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=JetBrains+Mono:wght@500;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#F8FAFC; --ink:#0F172A; --slate-700:#334155; --slate-600:#475569;
    --slate-500:#64748B; --slate-400:#94A3B8; --line:#E2E8F0; --card:#FFFFFF;
    --emerald:#10B981; --emerald-d:#047857; --sky:#0EA5E9; --amber:#F59E0B;
    --navy:#0A0F1D; --navy-2:#0F172A;
    --shadow:0 1px 2px rgba(15,23,42,.05);
    --radius:16px;
    --mono:"JetBrains Mono", ui-monospace, SFMono-Regular, Menlo, monospace;
  }
  *{box-sizing:border-box}
  html,body{margin:0;padding:0}
  body{
    font-family:"Plus Jakarta Sans", ui-sans-serif, system-ui, -apple-system, sans-serif;
    background:var(--bg); color:var(--ink); font-size:15px; line-height:1.45;
    -webkit-font-smoothing:antialiased;
  }
  h1,h2,h3,h4,p{margin:0}
  button{font-family:inherit;font-size:inherit;color:inherit;background:none;border:0;padding:0;cursor:pointer}
  input{font-family:inherit;font-size:inherit}
  .wrap{max-width:1340px;margin:0 auto;padding:0 20px}
  .hidden{display:none !important}
  .mono{font-family:var(--mono);font-variant-numeric:tabular-nums}

  /* ---------------- LOGIN ---------------- */
  #login{position:fixed;inset:0;z-index:100;display:grid;grid-template-columns:1fr;background:#fff;overflow:auto}
  @media(min-width:1024px){#login{grid-template-columns:1fr 1fr}}
  .login-left{
    position:relative;display:none;flex-direction:column;justify-content:space-between;
    overflow:hidden;background:var(--navy);padding:40px;color:#fff;
  }
  @media(min-width:1024px){.login-left{display:flex}}
  .login-glow{
    position:absolute;inset:0;opacity:.4;pointer-events:none;
    background:
      radial-gradient(600px 300px at 20% 0%, rgba(16,185,129,.18), transparent),
      radial-gradient(500px 300px at 90% 100%, rgba(245,158,11,.12), transparent);
  }
  .login-left-inner{position:relative;display:flex;flex-direction:column;gap:40px;max-width:520px}
  .brand{display:flex;align-items:center;gap:12px}
  .mark{width:40px;height:40px;border-radius:12px;background:var(--navy-2);display:grid;place-items:center;color:#34D399;flex:none}
  .brand-name{display:block;font-size:18px;font-weight:800;letter-spacing:-.02em;line-height:1.15}
  .brand-sub{display:block;font-size:11px;font-weight:500;color:var(--slate-400)}
  .login-headline{font-size:38px;font-weight:800;line-height:1.12;letter-spacing:-.025em;color:#fff}
  .login-headline span{color:#34D399}
  .login-copy{margin-top:12px;font-size:14px;line-height:1.6;color:var(--slate-400);max-width:430px}
  .flow{width:100%;max-width:340px}
  .flow .dash{stroke-dasharray:6 6;animation:flow 1.6s linear infinite}
  @keyframes flow{to{stroke-dashoffset:-24}}
  .login-chips{display:flex;flex-wrap:wrap;gap:26px;position:relative}
  .login-chips b{display:block;font-family:var(--mono);font-size:17px;font-weight:700;color:#fff}
  .login-chips span{font-size:11px;color:var(--slate-400);font-weight:500}
  .login-right{display:flex;align-items:center;justify-content:center;padding:40px 24px;background:#fff}
  .login-form{width:100%;max-width:360px}
  .login-form h2{font-size:24px;font-weight:700;letter-spacing:-.02em}
  .login-form .hint{margin-top:6px;font-size:13.5px;color:var(--slate-500)}
  .field{margin-top:18px}
  .field label{display:block;font-size:10.5px;font-weight:700;letter-spacing:.09em;color:var(--slate-500);text-transform:uppercase;margin-bottom:6px}
  .field input{
    width:100%;height:42px;border-radius:10px;border:1px solid var(--line);background:#fff;
    padding:0 12px;font-size:14px;color:var(--ink);outline:none;transition:border-color .15s, box-shadow .15s;
  }
  .field input:focus{border-color:#0284C7;box-shadow:0 0 0 3px rgba(14,165,233,.16)}
  .btn-primary{
    width:100%;height:44px;margin-top:22px;border-radius:10px;background:var(--navy-2);color:#fff;
    font-weight:700;font-size:14px;display:flex;align-items:center;justify-content:center;gap:8px;
    transition:background .15s;
  }
  .btn-primary:hover{background:#1E293B}
  .btn-quiet{width:100%;height:40px;margin-top:10px;border-radius:10px;background:#F1F5F9;color:var(--slate-600);font-weight:600;font-size:13px}
  .btn-quiet:hover{background:#E2E8F0}
  .login-note{margin-top:18px;font-size:11.5px;color:var(--slate-400);text-align:center}
  .login-error{margin-top:12px;font-size:12.5px;color:#B91C1C;font-weight:600;display:none}
  .login-mobile-brand{display:flex;margin-bottom:26px}
  @media(min-width:1024px){.login-mobile-brand{display:none}}

  /* ---------------- HEADER ---------------- */
  .topbar{position:sticky;top:0;z-index:30;background:rgba(255,255,255,.88);backdrop-filter:blur(12px);border-bottom:1px solid var(--line)}
  .topbar-in{display:flex;align-items:center;justify-content:space-between;gap:14px;min-height:70px;padding:12px 0;flex-wrap:wrap}
  .controls{display:flex;align-items:center;gap:8px;flex-wrap:wrap}
  .pill{display:inline-flex;align-items:center;gap:6px;border-radius:999px;padding:7px 13px;font-size:11.5px;font-weight:700;white-space:nowrap}
  .pill-dark{background:var(--navy);color:#fff;font-family:var(--mono);letter-spacing:.02em}
  .pill-green{background:#ECFDF5;color:#047857}
  .pill-amber{background:#FFFBEB;color:#B45309;border:1px solid #FDE68A}
  .pill-red{background:#FEF2F2;color:#B91C1C;border:1px solid #FECACA}
  .dot{width:6px;height:6px;border-radius:50%;background:currentColor;box-shadow:0 0 0 3px rgba(16,185,129,.16)}
  .seg{display:flex;border:1px solid var(--line);border-radius:999px;overflow:hidden;background:#fff}
  .seg button{padding:6px 10px;font-family:var(--mono);font-size:11px;font-weight:700;color:var(--slate-500);transition:background .15s,color .15s}
  .seg button.active{background:var(--navy-2);color:#fff}
  .btn-cut{display:inline-flex;align-items:center;gap:6px;border-radius:999px;padding:7px 13px;font-size:11.5px;font-weight:700;background:#FEF3C7;color:#92400E;transition:background .15s}
  .btn-cut:hover{background:#FDE68A}
  .btn-cut.active{background:#DC2626;color:#fff}
  .btn-ghost{display:inline-flex;align-items:center;gap:6px;border:1px solid var(--line);border-radius:999px;padding:6px 11px;font-size:11.5px;font-weight:700;color:var(--slate-500);transition:color .15s,border-color .15s}
  .btn-ghost:hover{color:var(--ink);border-color:#CBD5E1}

  /* ---------------- LAYOUT ---------------- */
  main{padding:22px 0 34px}
  .stack{display:flex;flex-direction:column;gap:16px}
  .card{background:var(--card);border:1px solid rgba(226,232,240,.85);border-radius:var(--radius);box-shadow:var(--shadow)}

  /* mode selector */
  .mode{padding:16px;display:flex;flex-direction:column;gap:12px}
  @media(min-width:768px){.mode{flex-direction:row;align-items:center;justify-content:space-between}}
  .mode-left{display:flex;align-items:flex-start;gap:10px;min-width:0}
  .mode-badge{height:36px;width:36px;border-radius:10px;background:#ECFDF5;color:var(--emerald-d);display:grid;place-items:center;flex:none}
  .mode-kicker{font-size:10.5px;font-weight:800;letter-spacing:.1em;color:var(--slate-500)}
  .mode-desc{font-size:13px;color:var(--slate-600);margin-top:2px}
  .modes{display:flex;gap:6px;flex-wrap:wrap}
  .mode-btn{display:flex;align-items:center;gap:8px;padding:9px 14px;border-radius:11px;border:1px solid transparent;transition:all .18s;text-align:left}
  .mode-btn:hover{background:#F8FAFC}
  .mode-btn .mb-title{font-size:12.5px;font-weight:700;color:var(--slate-600);line-height:1.2}
  .mode-btn .mb-sub{font-size:10.5px;color:var(--slate-400);line-height:1.3}
  .mode-btn.active{background:#ECFDF5;border-color:#A7F3D0}
  .mode-btn.active .mb-title{color:#065F46}
  .mode-btn.active .mb-sub{color:#0F766E}

  /* KPIs */
  .kpis{display:grid;gap:16px;grid-template-columns:1fr}
  @media(min-width:640px){.kpis{grid-template-columns:1fr 1fr}}
  @media(min-width:1120px){.kpis{grid-template-columns:repeat(4,1fr)}}
  .kpi{padding:20px;transition:box-shadow .2s}
  .kpi:hover{box-shadow:0 4px 14px rgba(15,23,42,.07)}
  .kpi-label{display:flex;align-items:center;justify-content:space-between;gap:8px;font-size:10.5px;font-weight:800;letter-spacing:.09em;color:var(--slate-500)}
  .kpi-ico{color:var(--slate-400)}
  .kpi-value{font-family:var(--mono);font-size:29px;font-weight:700;letter-spacing:-.03em;color:var(--ink);margin-top:8px;display:flex;align-items:baseline;gap:4px}
  .kpi-value small{font-size:12px;font-weight:600;color:var(--slate-500);letter-spacing:0}
  .trend{display:inline-flex;align-items:center;gap:3px;margin-top:6px;padding:2px 7px;border-radius:999px;font-size:11px;font-weight:800;background:#ECFDF5;color:#047857}
  .trend.warn{background:#FFFBEB;color:#B45309}
  .spark-wrap{margin-top:12px;height:44px}
  .spark-wrap svg{display:block;width:100%;height:44px}
  .mini-note{margin-top:8px;font-size:11px;color:var(--slate-500);font-weight:500}
  .chips{display:flex;flex-wrap:wrap;gap:8px;margin-top:10px}
  .chip{display:inline-flex;align-items:center;gap:4px;font-size:11px;font-weight:700;padding:3px 8px;border-radius:7px;background:#F1F5F9;color:var(--slate-600)}
  .chip.sky{background:#F0F9FF;color:#0369A1}
  .chip.em{background:#ECFDF5;color:#047857}
  .bar{height:7px;border-radius:999px;background:#F1F5F9;overflow:hidden;margin-top:12px}
  .bar>div{height:100%;border-radius:999px;transition:width .5s ease}
  .bar-grad{background:linear-gradient(90deg,#38BDF8,#FBBF24)}
  .dist{margin-top:12px;display:flex;flex-direction:column;gap:6px}
  .dist-row{display:grid;grid-template-columns:78px 1fr 52px;align-items:center;gap:8px;font-size:10.5px;color:var(--slate-500);font-weight:600}
  .dist-row .dbar{height:5px;border-radius:999px;background:#F1F5F9;overflow:hidden}
  .dist-row .dbar>i{display:block;height:100%;border-radius:999px}
  .dist-row b{font-family:var(--mono);font-size:10.5px;color:var(--slate-600);text-align:right;font-weight:700}

  /* appliances */
  .section-head{display:flex;align-items:flex-start;justify-content:space-between;gap:12px;flex-wrap:wrap;margin-bottom:14px}
  .section-title{font-size:15px;font-weight:800;letter-spacing:-.01em}
  .section-sub{font-size:12.5px;color:var(--slate-500);margin-top:3px}
  .btn-dark{display:inline-flex;align-items:center;gap:6px;background:var(--navy-2);color:#fff;border-radius:999px;padding:7px 14px;font-size:11.5px;font-weight:700;transition:background .15s}
  .btn-dark:hover{background:#1E293B}
  .appliances{padding:20px}
  .count{font-family:var(--mono);font-size:11px;font-weight:700;color:var(--slate-500);background:#F1F5F9;border-radius:999px;padding:4px 9px;margin-left:8px}
  .app-grid{display:grid;gap:10px;grid-template-columns:1fr}
  @media(min-width:900px){.app-grid{grid-template-columns:1fr 1fr}}
  .app{border:1px solid var(--line);border-radius:12px;padding:13px;background:#fff;transition:all .2s}
  .app.off{opacity:.78;background:#FCFCFD}
  .app.cut{opacity:.55}
  .app-top{display:flex;align-items:center;gap:8px}
  .app-ico{width:30px;height:30px;border-radius:9px;display:grid;place-items:center;font-size:14px;background:#F1F5F9;flex:none}
  .app-name{flex:1;min-width:0;font-size:12.5px;font-weight:700;color:var(--ink);white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
  .icon-btn{width:26px;height:26px;border-radius:8px;display:grid;place-items:center;color:var(--slate-400);transition:all .15s;flex:none}
  .icon-btn:hover{background:#F1F5F9;color:var(--slate-700)}
  .toggle{position:relative;width:42px;height:23px;border-radius:999px;background:#E2E8F0;transition:background .2s;flex:none}
  .toggle::after{content:"";position:absolute;top:2.5px;left:2.5px;width:18px;height:18px;border-radius:50%;background:#fff;box-shadow:0 1px 2px rgba(15,23,42,.2);transition:transform .2s}
  .toggle.on{background:var(--emerald)}
  .toggle.on::after{transform:translateX(19px)}
  .app-meta{display:flex;align-items:center;gap:5px;flex-wrap:wrap;margin-top:6px;font-size:10.5px;color:var(--slate-500);font-weight:500}
  .essential{font-weight:700;color:#0284C7}
  .essential.yes{color:#047857}
  .app-bottom{display:flex;align-items:center;gap:8px;margin-top:10px}
  .app-bottom input[type=range]{flex:1;height:5px;appearance:none;-webkit-appearance:none;background:#E2E8F0;border-radius:999px;outline:none;cursor:pointer;accent-color:#0284C7}
  .app-bottom input[type=range]::-webkit-slider-thumb{appearance:none;-webkit-appearance:none;width:13px;height:13px;border-radius:50%;background:#0284C7;border:2px solid #fff;box-shadow:0 1px 3px rgba(15,23,42,.3)}
  .hours{font-family:var(--mono);font-size:10.5px;font-weight:700;color:var(--slate-500);min-width:52px;text-align:right}
  .cost{font-family:var(--mono);font-size:10.5px;font-weight:700;border-radius:6px;padding:2px 6px;min-width:66px;text-align:center}
  .cost.pay{background:#F0F9FF;color:#0369A1}
  .cost.save{background:#ECFDF5;color:#047857}
  .cost.none{background:#F1F5F9;color:var(--slate-500)}

  /* bottom row */
  .bottom{display:grid;gap:16px;grid-template-columns:1fr}
  @media(min-width:1024px){.bottom{grid-template-columns:1fr 1fr}}
  .outlook{padding:20px}
  .outlook-row{display:flex;align-items:center;justify-content:space-between;gap:12px;padding:11px 0;border-bottom:1px dashed #E2E8F0}
  .outlook-row:last-of-type{border-bottom:0}
  .or-label{font-size:12px;font-weight:700;color:var(--slate-600)}
  .or-detail{font-family:var(--mono);font-size:10.5px;color:var(--slate-400);margin-top:2px}
  .or-value{font-family:var(--mono);font-size:15px;font-weight:800;color:var(--ink);white-space:nowrap}
  .or-value small{font-size:10px;font-weight:600;color:var(--slate-400)}
  .info{border-radius:9px;border:1px solid #BAE6FD;background:#F0F9FF;color:#075985;font-size:11.5px;font-weight:600;padding:9px 11px;margin-top:10px;display:flex;gap:7px;align-items:flex-start}
  .sky{padding:20px}
  .sky-chart{display:flex;align-items:flex-end;gap:4px;height:104px;margin-top:14px}
  .sky-col{flex:1;display:flex;flex-direction:column;justify-content:flex-end;align-items:center;height:100%;cursor:pointer;gap:4px}
  .sky-col i{display:block;width:100%;border-radius:4px 4px 2px 2px;background:linear-gradient(180deg,#7DD3FC,#38BDF8);transition:filter .15s,transform .15s;min-height:4px}
  .sky-col.peak i{background:linear-gradient(180deg,#FCD34D,#F59E0B)}
  .sky-col:hover i,.sky-col.sel i{filter:brightness(1.06);transform:scaleY(1.02)}
  .sky-col.sel i{outline:2px solid #0F172A;outline-offset:1px}
  .sky-col span{font-family:var(--mono);font-size:9px;color:var(--slate-400);font-weight:600}
  .sky-detail{margin-top:10px;border:1px dashed #E2E8F0;border-radius:9px;background:#F8FAFC;padding:9px 11px;font-size:11.5px;color:var(--slate-600);font-weight:600;font-family:var(--mono)}
  .protip{margin-top:12px;border-radius:10px;border:1px solid #FDE68A;background:#FFFBEB;padding:10px 12px;font-size:11.5px;line-height:1.55;color:#78350F}
  .protip b{color:#92400E}
  footer{padding:18px 0 34px;text-align:center;font-size:11px;color:var(--slate-400);border-top:1px solid #EEF2F6}

  /* dialog */
  dialog{border:0;border-radius:16px;padding:0;box-shadow:0 24px 60px rgba(15,23,42,.25);width:min(380px,calc(100vw - 32px))}
  dialog::backdrop{background:rgba(10,15,29,.45)}
  .dlg-in{padding:20px}
  .dlg-in h3{font-size:16px;font-weight:800;letter-spacing:-.01em}
  .dlg-in p{font-size:12px;color:var(--slate-500);margin-top:3px}
  .dlg-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:14px}
  .dlg-grid .field{margin-top:0}
  .dlg-grid .field.full{grid-column:1/-1}
  .dlg-actions{display:flex;gap:8px;margin-top:18px}
  .dlg-actions button{flex:1;height:38px;border-radius:9px;font-size:12.5px;font-weight:700}
  .dlg-cancel{background:#F1F5F9;color:var(--slate-600)}
  .dlg-save{background:var(--navy-2);color:#fff}
  .dlg-check{display:flex;align-items:center;gap:8px;margin-top:12px;font-size:12px;color:var(--slate-600);font-weight:600}
  .dlg-check input{width:15px;height:15px;accent-color:#047857}
</style>
</head>
<body>

<!-- ================= LOGIN ================= -->
<section id="login">
  <div class="login-left">
    <div class="login-glow"></div>
    <div class="login-left-inner">
      <div class="brand">
        <span class="mark">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M13 2 4 14h6l-1 8 9-12h-6l1-8z"/></svg>
        </span>
        <span>
          <span class="brand-name">WattWise</span>
          <span class="brand-sub">Smart Home Energy Monitor</span>
        </span>
      </div>

      <div>
        <h1 class="login-headline">Every rupee of your bill,<br><span>visible in real time.</span></h1>
        <p class="login-copy">Live appliance telemetry, solar offset tracking and ₹ bill projections — with a power-cut simulator so you know exactly what your inverter can carry.</p>
      </div>

      <svg class="flow" viewBox="0 0 320 130" aria-hidden="true">
        <circle cx="42" cy="34" r="15" fill="#F59E0B" opacity="0.9"/>
        <path class="dash" d="M57 40 C 108 44, 116 62, 148 66" stroke="#F59E0B" stroke-width="2" fill="none"/>
        <rect x="148" y="44" width="58" height="44" rx="9" fill="#10B981" fill-opacity="0.14" stroke="#10B981"/>
        <path d="M168 58v-5h18v5M177 70l-1 8 7-8h-6l1-8" stroke="#34D399" stroke-width="1.6" fill="none" stroke-linecap="round"/>
        <path class="dash" d="M206 66 C 236 66, 244 52, 272 52" stroke="#38BDF8" stroke-width="2" fill="none"/>
        <rect x="60" y="104" width="34" height="16" rx="4" fill="#94A3B8" opacity="0.55"/>
        <rect x="116" y="104" width="34" height="16" rx="4" fill="#94A3B8" opacity="0.4"/>
        <path d="M177 88v14" stroke="#94A3B8" stroke-width="1.5" stroke-dasharray="3 4"/>
        <text x="20" y="78" fill="#64748B" font-size="9" font-family="monospace">GRID</text>
        <text x="158" y="104" fill="#64748B" font-size="9" font-family="monospace">INVERTER</text>
        <text x="252" y="84" fill="#64748B" font-size="9" font-family="monospace">HOUSE</text>
      </svg>

      <div class="login-chips">
        <div><b>₹1,284</b><span>saved this month</span></div>
        <div><b>157.8 kg</b><span>CO₂ avoided</span></div>
        <div><b>214 kWh</b><span>solar harvested</span></div>
      </div>
    </div>
    <div style="position:relative;font-family:var(--mono);font-size:10px;letter-spacing:.13em;color:#475569">ZERO-FLICKER TELEMETRY · 100% LOCAL SIMULATION</div>
  </div>

  <div class="login-right">
    <form class="login-form" id="login-form">
      <div class="login-mobile-brand brand">
        <span class="mark"><svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor"><path d="M13 2 4 14h6l-1 8 9-12h-6l1-8z"/></svg></span>
        <span><span class="brand-name">WattWise</span><span class="brand-sub">Smart Home Energy Monitor</span></span>
      </div>
      <h2>Sign in to your dashboard</h2>
      <p class="hint">Demo account is prefilled — just hit sign in.</p>

      <div class="field">
        <label for="email">Email</label>
        <input id="email" type="email" placeholder="you@home.in" value="demo@wattwise.in" autocomplete="username" />
      </div>
      <div class="field">
        <label for="password">Password</label>
        <input id="password" type="password" placeholder="••••••••" value="wattwise123" autocomplete="current-password" />
      </div>

      <p class="login-error" id="login-error">Enter an email and password to continue.</p>

      <button class="btn-primary" type="submit">
        Sign In to Dashboard
        <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M5 12h14M13 6l6 6-6 6"/></svg>
      </button>
      <button class="btn-quiet" type="button" id="demo-btn">Quick demo sign-in</button>
      <p class="login-note">Local demo only — credentials never leave your browser.</p>
    </form>
  </div>
</section>

<!-- ================= APP ================= -->
<div id="app" class="hidden">
  <header class="topbar">
    <div class="wrap topbar-in">
      <div class="brand">
        <span class="mark"><svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M13 2 4 14h6l-1 8 9-12h-6l1-8z"/></svg></span>
        <span>
          <span class="brand-name">WattWise</span>
          <span class="brand-sub">Smart Home Energy Monitor</span>
        </span>
      </div>

      <div class="controls">
        <span class="pill pill-dark" id="clock">
          <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><circle cx="12" cy="12" r="9"/><path d="M12 7v5l3 2"/></svg>
          LIVE 18:06
        </span>
        <span class="pill pill-green" id="grid-pill"><i class="dot"></i>Grid active · 230V</span>
        <span class="pill pill-amber" id="cut-countdown">Power cut in 34 min</span>

        <span class="seg" id="speed">
          <button data-speed="1" class="active">1×</button>
          <button data-speed="5">5×</button>
          <button data-speed="15">15×</button>
        </span>

        <button class="btn-cut" id="cut-btn">
          <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.6" stroke-linecap="round"><path d="M13 2 4 14h6l-1 8 9-12h-6l1-8z"/></svg>
          Simulate power cut
        </button>

        <button class="btn-ghost" id="logout">
          <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><path d="M15 17l5-5-5-5M20 12H9M12 3H5a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h7"/></svg>
          Log out
        </button>
      </div>
    </div>
  </header>

  <main class="wrap stack">
    <!-- mode -->
    <section class="card mode">
      <div class="mode-left">
        <span class="mode-badge">
          <svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m12 3 1.9 4.6L18.5 9.5l-4.6 1.9L12 16l-1.9-4.6L5.5 9.5l4.6-1.9L12 3Z"/><path d="M18 16.5l.8 2 2 .8-2 .8-.8 2-.8-2-2-.8 2-.8.8-2Z"/></svg>
        </span>
        <div>
          <p class="mode-kicker">SMART MODE</p>
          <p class="mode-desc" id="mode-desc">All routines active — normal appliance operations.</p>
        </div>
      </div>
      <div class="modes" id="modes">
        <button class="mode-btn active" data-mode="home">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 10.5 12 3l9 7.5"/><path d="M5 9.5V21h14V9.5"/></svg>
          <span><span class="mb-title">Home Mode</span><br><span class="mb-sub">Normal ops</span></span>
        </button>
        <button class="mode-btn" data-mode="away">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 21V9l7-6v18"/><path d="M3 21h18"/><path d="M12 12h.01M12 16h.01"/></svg>
          <span><span class="mb-title">Away Mode</span><br><span class="mb-sub">Idle saver</span></span>
        </button>
        <button class="mode-btn" data-mode="sleep">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20 14.5A8.5 8.5 0 0 1 9.5 4a8.5 8.5 0 1 0 10.5 10.5Z"/></svg>
          <span><span class="mb-title">Sleep Mode</span><br><span class="mb-sub">Night baseline</span></span>
        </button>
      </div>
    </section>

    <!-- KPIs -->
    <section class="kpis">
      <article class="card kpi">
        <div class="kpi-label">
          <span>EST. MONTHLY BILL (NET)</span>
          <span class="kpi-ico"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M6 3h12M6 8h12M7 21c0-6 4-9 4-13M17 21c0-6-4-9-4-13"/></svg></span>
        </div>
        <p class="kpi-value" id="bill-value">₹0</p>
        <p class="trend" id="bill-trend">↓ 14.9% vs last month</p>
        <div class="spark-wrap" id="spark"></div>
        <p class="mini-note">₹/day, net of solar credit · last 7 days</p>
      </article>

      <article class="card kpi">
        <div class="kpi-label">
          <span>SOLAR OFFSET</span>
          <span class="kpi-ico" style="color:#0284C7"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><circle cx="12" cy="12" r="4"/><path d="M12 2v2M12 20v2M2 12h2M20 12h2M5 5l1.4 1.4M17.6 17.6 19 19M19 5l-1.4 1.4M6.4 17.6 5 19"/></svg></span>
        </div>
        <p class="kpi-value">157.8 <small>kg</small></p>
        <p class="mini-note">CO₂ avoided this month</p>
        <div class="bar"><div class="bar-grad" style="width:71%"></div></div>
        <p class="mini-note">71% of 300 kWh monthly solar goal</p>
        <div class="chips">
          <span class="chip sky">214 kWh harvested</span>
          <span class="chip em">₹1,284 saved</span>
        </div>
      </article>

      <article class="card kpi">
        <div class="kpi-label">
          <span>LIVE LOAD</span>
          <span class="kpi-ico" style="color:#B45309"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M3 12h4l2-6 3 12 2.5-6H21"/></svg></span>
        </div>
        <p class="kpi-value" id="live-kw">0.00 <small>kW</small></p>
        <p class="mini-note" id="live-count">7 of 10 running</p>
        <div class="dist" id="dist"></div>
      </article>

      <article class="card kpi">
        <div class="kpi-label">
          <span>BUDGET HEALTH</span>
          <span class="kpi-ico" style="color:#047857"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M3 7h18v12H3z"/><path d="M3 11h18"/></svg></span>
        </div>
        <p class="kpi-value" id="budget-pct">0% used</p>
        <p class="mini-note" id="budget-left">₹0 left in budget</p>
        <div class="bar"><div id="budget-bar" style="width:0%;background:#10B981"></div></div>
        <p class="mini-note">Target ₹4,200 / month · last month ₹3,993</p>
      </article>
    </section>

    <!-- appliances -->
    <section class="card appliances">
      <div class="section-head">
        <div>
          <h2 class="section-title">Appliances <span class="count" id="app-count">7 of 10 running</span></h2>
          <p class="section-sub">Flip a toggle or set daily run-hours — every ₹ projection updates live.</p>
        </div>
        <button class="btn-dark" id="add-btn">
          <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.8" stroke-linecap="round"><path d="M12 5v14M5 12h14"/></svg>
          Add appliance
        </button>
      </div>
      <div class="app-grid" id="app-grid"></div>
    </section>

    <!-- outlook + sky -->
    <section class="bottom">
      <article class="card outlook">
        <div class="section-head" style="margin-bottom:4px">
          <div>
            <h2 class="section-title">Energy Outlook</h2>
            <p class="section-sub">grid ₹8/kWh · export ₹3/kWh</p>
          </div>
        </div>

        <div class="outlook-row">
          <div>
            <p class="or-label">Hourly projection</p>
            <p class="or-detail" id="out-hourly-detail">Grid ₹0.00 · Solar credit −₹0.00</p>
          </div>
          <p class="or-value" id="out-hourly">₹0/hr</p>
        </div>
        <div class="outlook-row">
          <div>
            <p class="or-label">Daily projection</p>
            <p class="or-detail" id="out-daily-detail">Grid ₹0.00 · Solar credit −₹0.00</p>
          </div>
          <p class="or-value" id="out-daily">₹0/day</p>
        </div>
        <div class="outlook-row">
          <div>
            <p class="or-label">Monthly projection</p>
            <p class="or-detail" id="out-monthly-detail">Grid ₹0.00 · Solar credit −₹0.00</p>
          </div>
          <p class="or-value" id="out-monthly">₹0/mo</p>
        </div>

        <div class="info">
          <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" style="flex:none;margin-top:1px"><circle cx="12" cy="12" r="9"/><path d="M12 11v5M12 8h.01"/></svg>
          <span><b id="credit-amount">₹367</b> Solar Credit applied this month (export tariff ₹3/kWh), deducted from your gross bill.</span>
        </div>
      </article>

      <article class="card sky">
        <div class="section-head" style="margin-bottom:0">
          <div>
            <h2 class="section-title">Tomorrow's Sky</h2>
            <p class="section-sub">26.50 kWh forecast · ≈₹79 credit</p>
          </div>
          <span class="chip" style="background:#FFFBEB;color:#B45309">peak 11:00–14:00</span>
        </div>
        <div class="sky-chart" id="sky-chart"></div>
        <p class="sky-detail" id="sky-detail">12:00 · 3.4 kW (peak window) · ≈₹10.20 solar credit this hour</p>
        <p class="protip"><b>Pro Tip:</b> Plan heavy loads (Washing Machine, Geyser) around your brightest window (11:00–14:00) to maximize ₹0-cost solar power.</p>
      </article>
    </section>

    <footer>
      WattWise · local simulation — no data leaves your browser · Tariffs: ₹8/kWh grid, ₹3/kWh solar export · Power-cut drill every hour at :40
    </footer>
  </main>
</div>

<!-- add / edit dialog -->
<dialog id="app-dialog">
  <form class="dlg-in" id="dlg-form">
    <h3 id="dlg-title">Add appliance</h3>
    <p id="dlg-sub">New loads are added as grid-only devices.</p>
    <div class="dlg-grid">
      <div class="field full">
        <label for="dlg-name">Appliance name</label>
        <input id="dlg-name" type="text" placeholder="e.g. Study Desk Lamp" required />
      </div>
      <div class="field">
        <label for="dlg-watts">Watts</label>
        <input id="dlg-watts" type="number" min="5" max="5000" step="5" value="60" required />
      </div>
      <div class="field">
        <label for="dlg-hours">Hours / day</label>
        <input id="dlg-hours" type="number" min="0" max="24" step="0.5" value="4" required />
      </div>
    </div>
    <label class="dlg-check"><input type="checkbox" id="dlg-essential" /> Inverter-backed (essential)</label>
    <div class="dlg-actions">
      <button type="button" class="dlg-cancel" id="dlg-cancel">Cancel</button>
      <button type="submit" class="dlg-save" id="dlg-save">Save appliance</button>
    </div>
  </form>
</dialog>

<script>
(function () {
  "use strict";

  /* ---------------- config ---------------- */
  const GRID_TARIFF = 8;      // ₹ / kWh
  const EXPORT_TARIFF = 3;    // ₹ / kWh
  const SOLAR_CREDIT = 367.29; // ₹ / month applied
  const MONTH_DAYS = 30;
  const BUDGET_TARGET = 4200;  // ₹ / month
  const BASE_KW = 2.005;       // sum of default-on appliances
  const CUT_INTERVAL = 34;     // simulated minutes between drills

  const ICONS = {
    cooling: "❄️", essential: "🧊", entertainment: "📺", laundry: "🌀",
    hot: "♨️", lighting: "💡", fans: "🌬️", network: "📶",
    security: "📷", kitchen: "🍲", generic: "🔌"
  };

  let appliances = [
    { id: "ac", name: "Air Conditioner (1.5 Ton)", category: "Heavy load", watts: 1450, essential: false, on: true, hours: 7, icon: "cooling", mode: { home: { on: true, hours: 7 }, away: { on: false, hours: 7 }, sleep: { on: false, hours: 2 } } },
    { id: "fridge", name: "Smart Refrigerator", category: "Essential", watts: 180, essential: true, on: true, hours: 24, icon: "essential", mode: { home: { on: true, hours: 24 }, away: { on: true, hours: 24 }, sleep: { on: true, hours: 24 } } },
    { id: "tv", name: "OLED 4K TV & Soundbar", category: "Entertainment", watts: 120, essential: false, on: true, hours: 4, icon: "entertainment", mode: { home: { on: true, hours: 4 }, away: { on: false, hours: 4 }, sleep: { on: false, hours: 0 } } },
    { id: "washer", name: "Front Load Washer", category: "Heavy load", watts: 850, essential: false, on: false, hours: 1.5, icon: "laundry", mode: { home: { on: false, hours: 1.5 }, away: { on: false, hours: 1.5 }, sleep: { on: false, hours: 1.5 } } },
    { id: "geyser", name: "Instant Water Geyser", category: "Heavy load", watts: 2000, essential: false, on: false, hours: 1, icon: "hot", mode: { home: { on: false, hours: 1 }, away: { on: false, hours: 1 }, sleep: { on: false, hours: 1 } } },
    { id: "lights", name: "Ambient LED Lighting", category: "Lighting", watts: 90, essential: false, on: true, hours: 8, icon: "lighting", mode: { home: { on: true, hours: 8 }, away: { on: false, hours: 8 }, sleep: { on: true, hours: 2 } } },
    { id: "fans", name: "BLDC Ceiling Fans (x3)", category: "Cooling", watts: 105, essential: true, on: true, hours: 12, icon: "fans", mode: { home: { on: true, hours: 12 }, away: { on: true, hours: 0 }, sleep: { on: true, hours: 8 } } },
    { id: "wifi", name: "Mesh WiFi & Smart Hub", category: "Essential", watts: 25, essential: true, on: true, hours: 24, icon: "network", mode: { home: { on: true, hours: 24 }, away: { on: true, hours: 24 }, sleep: { on: true, hours: 24 } } },
    { id: "security", name: "CCTV & Doorbell Hub", category: "Essential", watts: 35, essential: true, on: true, hours: 24, icon: "security", mode: { home: { on: true, hours: 24 }, away: { on: true, hours: 24 }, sleep: { on: true, hours: 24 } } },
    { id: "microwave", name: "Convection Microwave", category: "Kitchen", watts: 1200, essential: false, on: false, hours: 0.5, icon: "kitchen", mode: { home: { on: false, hours: 0.5 }, away: { on: false, hours: 0.5 }, sleep: { on: false, hours: 0.5 } } }
  ];

  const sky = [
    { h: 6, kw: 0.4 }, { h: 7, kw: 1.0 }, { h: 8, kw: 1.8 }, { h: 9, kw: 2.4 },
    { h: 10, kw: 2.9 }, { h: 11, kw: 3.2 }, { h: 12, kw: 3.4 }, { h: 13, kw: 3.2 },
    { h: 14, kw: 2.8 }, { h: 15, kw: 2.1 }, { h: 16, kw: 1.3 }, { h: 17, kw: 0.6 },
    { h: 18, kw: 0.2 }
  ];

  const state = {
    mode: "home",
    speed: 1,
    simMinutes: 18 * 60 + 6, // 18:06
    outage: false,
    outageLeft: 0,
    minutesToCut: CUT_INTERVAL,
    tick: 0,
    selectedHour: 12
  };

  /* ---------------- helpers ---------------- */
  const $ = (id) => document.getElementById(id);
  const rupees = (n) => "₹" + Math.round(n).toLocaleString("en-IN");
  const rupees2 = (n) => "₹" + n.toFixed(2);
  const hhmm = (mins) => {
    const m = ((mins % 1440) + 1440) % 1440;
    return String(Math.floor(m / 60)).padStart(2, "0") + ":" + String(m % 60).padStart(2, "0");
  };
  const dailyCost = (a) => (a.watts * a.hours / 1000) * GRID_TARIFF;
  const isRunning = (a) => a.on && (!state.outage || a.essential);

  /* ---------------- calculations ---------------- */
  function totals() {
    let grossDay = 0;
    appliances.forEach((a) => { if (a.on) grossDay += dailyCost(a); });
    const creditDay = SOLAR_CREDIT / MONTH_DAYS;
    const netDay = Math.max(0, grossDay - creditDay);
    const monthly = netDay * MONTH_DAYS;
    const liveKw = appliances.reduce((s, a) => s + (isRunning(a) ? a.watts : 0), 0) / 1000;
    const running = appliances.filter((a) => a.on).length;
    return { grossDay, creditDay, netDay, monthly, liveKw, running };
  }

  /* ---------------- sparkline ---------------- */
  function sparkline(values) {
    const w = 260, h = 44, max = Math.max.apply(null, values), min = Math.min.apply(null, values);
    const span = Math.max(1, max - min);
    const pts = values.map((v, i) => {
      const x = (i / (values.length - 1)) * (w - 4) + 2;
      const y = h - 6 - ((v - min) / span) * (h - 16);
      return [x, y];
    });
    const line = pts.map((p) => p[0].toFixed(1) + "," + p[1].toFixed(1)).join(" ");
    const area = "M2," + (h - 2) + " L" + line.replace(/ /g, " L") + " L" + (w - 2) + "," + (h - 2) + " Z";
    const last = pts[pts.length - 1];
    return '<svg viewBox="0 0 ' + w + " " + h + '" preserveAspectRatio="none" aria-hidden="true">' +
      '<defs><linearGradient id="sg" x1="0" y1="0" x2="0" y2="1">' +
      '<stop offset="0%" stop-color="#10B981" stop-opacity=".28"/><stop offset="100%" stop-color="#10B981" stop-opacity="0"/>' +
      "</linearGradient></defs>" +
      '<path d="' + area + '" fill="url(#sg)"/>' +
      '<polyline points="' + line + '" fill="none" stroke="#10B981" stroke-width="2" stroke-linejoin="round" stroke-linecap="round"/>' +
      '<circle cx="' + last[0].toFixed(1) + '" cy="' + last[1].toFixed(1) + '" r="2.8" fill="#059669" stroke="#fff" stroke-width="1.6"/>' +
      "</svg>";
  }

  /* ---------------- render: appliance cards ---------------- */
  function appCardHTML(a) {
    const cost = dailyCost(a);
    const cutOff = state.outage && !a.essential;
    const costCls = a.on ? (a.essential ? "save" : "pay") : "none";
    const costTxt = a.on ? rupees(cost) + "/day" : "saves " + rupees(cost);
    return '' +
      '<div class="app' + (a.on ? "" : " off") + (cutOff ? " cut" : "") + '" data-id="' + a.id + '">' +
        '<div class="app-top">' +
          '<span class="app-ico">' + (ICONS[a.icon] || ICONS.generic) + "</span>" +
          '<span class="app-name" title="' + a.name + '">' + a.name + "</span>" +
          '<button class="icon-btn" data-act="edit" title="Edit appliance" aria-label="Edit ' + a.name + '">' +
            '<svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 20h9"/><path d="M16.5 3.5a2.1 2.1 0 0 1 3 3L7 19l-4 1 1-4Z"/></svg>' +
          "</button>" +
          '<button class="toggle' + (a.on ? " on" : "") + '" data-act="toggle" role="switch" aria-checked="' + a.on + '" aria-label="Toggle ' + a.name + '"></button>' +
        "</div>" +
        '<p class="app-meta">' + a.category + " · " + a.watts + " W · " +
          (a.essential ? '<span class="essential yes">inverter-safe</span>' : "grid-only") +
        "</p>" +
        '<div class="app-bottom">' +
          '<input type="range" min="0" max="24" step="0.5" value="' + a.hours + '" data-act="hours" aria-label="Daily hours for ' + a.name + '"/>' +
          '<span class="hours" data-hours-label>' + a.hours.toFixed(a.hours % 1 ? 2 : 0) + "h/day</span>" +
          '<span class="cost ' + costCls + '" data-cost>' + costTxt + "</span>" +
        "</div>" +
      "</div>";
  }

  function renderAppliances() {
    $("app-grid").innerHTML = appliances.map(appCardHTML).join("");
    refreshCards();
  }

  function refreshCards() {
    appliances.forEach((a) => {
      const el = document.querySelector('.app[data-id="' + a.id + '"]');
      if (!el) return;
      el.classList.toggle("off", !a.on);
      el.classList.toggle("cut", state.outage && !a.essential);
      const t = el.querySelector(".toggle");
      t.classList.toggle("on", a.on);
      t.setAttribute("aria-checked", String(a.on));
      const cost = dailyCost(a);
      const c = el.querySelector("[data-cost]");
      c.textContent = a.on ? rupees(cost) + "/day" : "saves " + rupees(cost);
      c.className = "cost " + (a.on ? (a.essential ? "save" : "pay") : "none");
      el.querySelector("[data-hours-label]").textContent = a.hours.toFixed(a.hours % 1 ? 2 : 0) + "h/day";
      const r = el.querySelector('[data-act="hours"]');
      if (r && Number(r.value) !== a.hours) r.value = a.hours;
    });
    const running = appliances.filter((a) => a.on).length;
    $("app-count").textContent = running + " of " + appliances.length + " running";
  }

  /* ---------------- render: KPIs + outlook ---------------- */
  function render() {
    const t = totals();
    const jitter = state.outage ? 0 : Math.sin(state.tick / 3) * 0.02;
    const liveKw = Math.max(0, t.liveKw + jitter);

    // bill
    $("bill-value").textContent = rupees(t.monthly);
    const history = [172, 158, 181, 164, 190, 161, Math.round(t.netDay)];
    $("spark").innerHTML = sparkline(history);
    const trend = $("bill-trend");
    const pctChange = ((BUDGET_TARGET - t.monthly) / BUDGET_TARGET) * 100;
    trend.textContent = (pctChange >= 0 ? "↓ " : "↑ ") + Math.abs(14.9).toFixed(1) + "% vs last month";
    trend.className = "trend" + (pctChange < 0 ? " warn" : "");

    // live load
    $("live-kw").innerHTML = liveKw.toFixed(2) + " <small>kW</small>";
    $("live-count").textContent = t.running + " of " + appliances.length + " running";
    const top = appliances.filter(isRunning).sort((a, b) => b.watts - a.watts).slice(0, 5);
    const maxW = top.length ? top[0].watts : 1;
    const colors = ["#38BDF8", "#34D399", "#FBBF24", "#A78BFA", "#FB7185"];
    $("dist").innerHTML = top.map((a, i) =>
      '<div class="dist-row"><span title="' + a.name + '">' + a.name.split(" ")[0] + "</span>" +
      '<span class="dbar"><i style="width:' + Math.max(6, (a.watts / maxW) * 100) + "%;background:" + colors[i] + '"></i></span>' +
      "<b>" + a.watts + "W</b></div>"
    ).join("") || '<p class="mini-note" style="margin:0">Everything is off — no live draw.</p>';

    // budget
    const usedPct = Math.round((t.monthly / BUDGET_TARGET) * 100);
    $("budget-pct").textContent = usedPct + "% used";
    const left = BUDGET_TARGET - t.monthly;
    $("budget-left").textContent = (left >= 0 ? rupees(left) + " left in budget" : rupees(-left) + " over budget");
    const bar = $("budget-bar");
    bar.style.width = Math.min(100, usedPct) + "%";
    bar.style.background = usedPct < 70 ? "#10B981" : usedPct < 90 ? "#F59E0B" : "#EF4444";

    // outlook
    const hourly = liveKw * GRID_TARIFF;
    const hourlyCredit = (sky.find((s) => s.h === Math.min(18, Math.max(6, Math.floor(state.simMinutes / 60)))) || { kw: 0 }).kw * EXPORT_TARIFF;
    $("out-hourly").innerHTML = rupees2(hourly) + '<small>/hr</small>';
    $("out-hourly-detail").textContent = "Grid " + rupees2(hourly) + " · Solar credit −" + rupees2(hourlyCredit);
    $("out-daily").innerHTML = rupees(t.netDay) + '<small>/day</small>';
    $("out-daily-detail").textContent = "Grid " + rupees2(t.grossDay) + " · Solar credit −" + rupees2(t.creditDay);
    $("out-monthly").innerHTML = rupees(t.monthly) + '<small>/mo</small>';
    $("out-monthly-detail").textContent = "Grid " + rupees2(t.grossDay * MONTH_DAYS) + " · Solar credit −" + rupees2(SOLAR_CREDIT);
    $("credit-amount").textContent = rupees(SOLAR_CREDIT);

    refreshCards();
  }

  /* ---------------- header / simulation ---------------- */
  function renderHeader() {
    $("clock").innerHTML = '<svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><circle cx="12" cy="12" r="9"/><path d="M12 7v5l3 2"/></svg> LIVE ' + hhmm(state.simMinutes);

    const gridPill = $("grid-pill");
    const cutPill = $("cut-countdown");
    const cutBtn = $("cut-btn");

    if (state.outage) {
      const invLoad = appliances.reduce((s, a) => s + (a.essential && a.on ? a.watts : 0), 0);
      gridPill.className = "pill pill-red";
      gridPill.innerHTML = '<i class="dot"></i>ON INVERTER · ' + invLoad + " W safe";
      cutPill.className = "pill pill-red";
      cutPill.textContent = "Power cut active · " + state.outageLeft + " min left";
      cutBtn.classList.add("active");
      cutBtn.textContent = "Restore grid";
    } else {
      gridPill.className = "pill pill-green";
      gridPill.innerHTML = '<i class="dot"></i>Grid active · 230V';
      cutPill.className = "pill pill-amber";
      cutPill.textContent = "Power cut in " + state.minutesToCut + " min";
      cutBtn.classList.remove("active");
      cutBtn.innerHTML = '<svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.6" stroke-linecap="round"><path d="M13 2 4 14h6l-1 8 9-12h-6l1-8z"/></svg> Simulate power cut';
    }
  }

  function tick() {
    state.tick++;
    state.simMinutes += state.speed;

    if (state.outage) {
      state.outageLeft -= state.speed;
      if (state.outageLeft <= 0) endOutage();
    } else {
      state.minutesToCut -= state.speed;
      if (state.minutesToCut <= 0) startOutage();
    }

    renderHeader();
    render();
  }

  function startOutage() {
    state.outage = true;
    state.outageLeft = 15;
    state.minutesToCut = CUT_INTERVAL;
    renderHeader();
    render();
  }

  function endOutage() {
    state.outage = false;
    state.outageLeft = 0;
    state.minutesToCut = CUT_INTERVAL;
    renderHeader();
    render();
  }

  /* ---------------- modes ---------------- */
  const MODE_COPY = {
    home: "All routines active — normal appliance operations.",
    away: "Away mode on — only essentials stay powered.",
    sleep: "Sleep mode on — night baseline with quiet cooling."
  };

  function applyMode(mode) {
    state.mode = mode;
    appliances.forEach((a) => {
      const preset = a.mode[mode];
      if (!preset) return;
      a.on = preset.on;
      a.hours = preset.hours;
    });
    $("mode-desc").textContent = MODE_COPY[mode];
    document.querySelectorAll("#modes .mode-btn").forEach((b) => b.classList.toggle("active", b.dataset.mode === mode));
    render();
  }

  /* ---------------- sky chart ---------------- */
  function renderSky() {
    const maxKw = Math.max.apply(null, sky.map((s) => s.kw));
    $("sky-chart").innerHTML = sky.map((s) => {
      const peak = s.h >= 11 && s.h <= 14;
      return '<div class="sky-col' + (peak ? " peak" : "") + (s.h === state.selectedHour ? " sel" : "") + '" data-hour="' + s.h + '">' +
        '<i style="height:' + Math.max(6, (s.kw / maxKw) * 100) + '%"></i>' +
        "<span>" + String(s.h).padStart(2, "0") + "</span></div>";
    }).join("");
    const sel = sky.find((s) => s.h === state.selectedHour);
    $("sky-detail").textContent = String(sel.h).padStart(2, "0") + ":00 · " + sel.kw.toFixed(1) + " kW" +
      (sel.h >= 11 && sel.h <= 14 ? " (peak window)" : "") +
      " · ≈" + rupees2(sel.kw * EXPORT_TARIFF) + " solar credit this hour";
  }

  /* ---------------- dialog ---------------- */
  const dialog = $("app-dialog");
  let editingId = null;

  function openDialog(appliance) {
    editingId = appliance ? appliance.id : null;
    $("dlg-title").textContent = appliance ? "Edit appliance" : "Add appliance";
    $("dlg-sub").textContent = appliance ? "Update load details — projections recalculate instantly." : "New loads are added as grid-only devices.";
    $("dlg-name").value = appliance ? appliance.name : "";
    $("dlg-watts").value = appliance ? appliance.watts : 60;
    $("dlg-hours").value = appliance ? appliance.hours : 4;
    $("dlg-essential").checked = appliance ? appliance.essential : false;
    $("dlg-save").textContent = appliance ? "Save changes" : "Save appliance";
    dialog.showModal();
    $("dlg-name").focus();
  }

  /* ---------------- events ---------------- */
  $("app-grid").addEventListener("click", function (e) {
    const btn = e.target.closest("[data-act]");
    if (!btn) return;
    if (btn.dataset.act === "hours") return;
    const card = btn.closest(".app");
    const a = appliances.find((x) => x.id === card.dataset.id);
    if (!a) return;
    if (btn.dataset.act === "toggle") { a.on = !a.on; render(); }
    else if (btn.dataset.act === "edit") { openDialog(a); }
  });

  $("app-grid").addEventListener("input", function (e) {
    if (e.target.dataset.act !== "hours") return;
    const card = e.target.closest(".app");
    const a = appliances.find((x) => x.id === card.dataset.id);
    if (!a) return;
    a.hours = Number(e.target.value);
    render();
  });

  $("add-btn").addEventListener("click", function () { openDialog(null); });
  $("dlg-cancel").addEventListener("click", function () { dialog.close(); });

  $("dlg-form").addEventListener("submit", function (e) {
    e.preventDefault();
    const name = $("dlg-name").value.trim();
    const watts = Math.min(5000, Math.max(5, Number($("dlg-watts").value) || 60));
    const hours = Math.min(24, Math.max(0, Number($("dlg-hours").value) || 0));
    const essential = $("dlg-essential").checked;

    if (editingId) {
      const a = appliances.find((x) => x.id === editingId);
      if (a) { a.name = name; a.watts = watts; a.hours = hours; a.essential = essential; }
    } else {
      appliances.push({
        id: "custom-" + Date.now(),
        name: name,
        category: essential ? "Essential" : "Custom load",
        watts: watts,
        essential: essential,
        on: true,
        hours: hours,
        icon: "generic",
        mode: { home: { on: true, hours: hours }, away: { on: essential, hours: hours }, sleep: { on: essential, hours: hours } }
      });
    }
    dialog.close();
    renderAppliances();
    render();
  });

  $("cut-btn").addEventListener("click", function () {
    if (state.outage) endOutage(); else startOutage();
  });

  $("speed").addEventListener("click", function (e) {
    const b = e.target.closest("button[data-speed]");
    if (!b) return;
    state.speed = Number(b.dataset.speed);
    document.querySelectorAll("#speed button").forEach((x) => x.classList.toggle("active", x === b));
  });

  $("modes").addEventListener("click", function (e) {
    const b = e.target.closest("[data-mode]");
    if (!b) return;
    applyMode(b.dataset.mode);
  });

  $("sky-chart").addEventListener("click", function (e) {
    const col = e.target.closest("[data-hour]");
    if (!col) return;
    state.selectedHour = Number(col.dataset.hour);
    renderSky();
  });

  $("sky-chart").addEventListener("mouseover", function (e) {
    const col = e.target.closest("[data-hour]");
    if (!col) return;
    state.selectedHour = Number(col.dataset.hour);
    renderSky();
  });

  /* ---------------- auth ---------------- */
  function showApp() {
    $("login").classList.add("hidden");
    $("app").classList.remove("hidden");
    try { sessionStorage.setItem("wattwise-auth", "1"); } catch (err) {}
    renderAppliances();
    renderSky();
    renderHeader();
    render();
  }

  function logout() {
    $("app").classList.add("hidden");
    $("login").classList.remove("hidden");
    try { sessionStorage.removeItem("wattwise-auth"); } catch (err) {}
  }

  $("login-form").addEventListener("submit", function (e) {
    e.preventDefault();
    const email = $("email").value.trim();
    const pass = $("password").value;
    const err = $("login-error");
    if (!email || !pass) { err.style.display = "block"; return; }
    err.style.display = "none";
    showApp();
  });

  $("demo-btn").addEventListener("click", function () {
    $("email").value = "demo@wattwise.in";
    $("password").value = "wattwise123";
    showApp();
  });

  $("logout").addEventListener("click", logout);

  /* ---------------- boot ---------------- */
  renderAppliances();
  renderSky();
  renderHeader();

  let authed = false;
  try { authed = sessionStorage.getItem("wattwise-auth") === "1"; } catch (err) {}
  if (authed) showApp();

  setInterval(tick, 1000);
})();
</script>
</body>
</html>
