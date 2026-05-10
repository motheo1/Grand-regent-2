# Grand-regent-2
The 8 soldiers and a leader
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="theme-color" content="#080808" />
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
  <meta name="apple-mobile-web-app-title" content="Empire AI" />
  <title>Income Empire AI</title>
  <link href="https://fonts.googleapis.com/css2?family=Syne:wght@700;800&family=DM+Sans:wght@400;500&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet" />
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    :root {
      --bg: #080808; --surface: #0d0d0d; --border: #1e1e1e;
      --text: #e8e8e8; --muted: #444; --gold: #FFD700;
    }
    html, body { height: 100%; background: var(--bg); color: var(--text); font-family: 'DM Sans', sans-serif; overflow-x: hidden; }
    ::-webkit-scrollbar { width: 3px; }
    ::-webkit-scrollbar-thumb { background: #2a2a2a; border-radius: 2px; }

    @keyframes fadeUp { from { opacity:0; transform:translateY(16px); } to { opacity:1; transform:translateY(0); } }
    @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.3} }
    @keyframes shimmer { 0%{transform:translateX(-100%)} 100%{transform:translateX(300%)} }
    @keyframes spin { to { transform: rotate(360deg); } }
    @keyframes borderGlow { 0%,100%{box-shadow:0 0 8px var(--card-color)} 50%{box-shadow:0 0 20px var(--card-color)} }

    /* SETUP */
    #setup-screen { display:flex; flex-direction:column; align-items:center; justify-content:center; min-height:100vh; padding:32px 20px; text-align:center; }
    .setup-logo { font-size:52px; margin-bottom:16px; }
    .setup-title { font-family:'Syne',sans-serif; font-size:30px; font-weight:800; background:linear-gradient(135deg,#FFD700,#FF6B35,#FF3EAC); -webkit-background-clip:text; -webkit-text-fill-color:transparent; margin-bottom:8px; line-height:1.1; }
    .setup-sub { color:var(--muted); font-size:13px; margin-bottom:28px; line-height:1.6; max-width:300px; }
    .setup-steps { width:100%; max-width:360px; margin-bottom:24px; }
    .setup-step { background:var(--surface); border:1px solid var(--border); border-radius:12px; padding:14px 16px; margin-bottom:10px; text-align:left; display:flex; gap:12px; align-items:flex-start; }
    .step-num { background:var(--gold); color:#000; border-radius:50%; width:22px; height:22px; display:flex; align-items:center; justify-content:center; font-family:'Syne',sans-serif; font-weight:700; font-size:11px; flex-shrink:0; margin-top:1px; }
    .step-text { font-size:13px; line-height:1.5; color:#bbb; }
    .step-text a { color:var(--gold); text-decoration:none; }
    .setup-input { width:100%; max-width:360px; background:var(--surface); border:1px solid #333; border-radius:12px; padding:14px 16px; color:var(--text); font-size:14px; font-family:'JetBrains Mono',monospace; margin-bottom:12px; }
    .setup-input:focus { outline:none; border-color:var(--gold); }
    .setup-btn { width:100%; max-width:360px; padding:14px; background:linear-gradient(135deg,#FFD700,#FF6B35); border:none; border-radius:12px; color:#000; font-family:'Syne',sans-serif; font-weight:800; font-size:14px; letter-spacing:0.05em; cursor:pointer; }
    .setup-note { color:#333; font-size:11px; margin-top:12px; max-width:300px; }

    /* APP */
    #app { display:none; flex-direction:column; min-height:100vh; }

    /* HEADER */
    .header { background:#0a0a0a; border-bottom:1px solid var(--border); padding:14px 16px; position:sticky; top:0; z-index:100; display:flex; align-items:center; justify-content:space-between; }
    .header-title { font-family:'Syne',sans-serif; font-size:17px; font-weight:800; background:linear-gradient(135deg,#FFD700,#FF6B35); -webkit-background-clip:text; -webkit-text-fill-color:transparent; }
    .header-right { display:flex; gap:6px; align-items:center; }
    .mode-btn { padding:6px 12px; border-radius:20px; border:1px solid var(--border); font-family:'Syne',sans-serif; font-weight:700; font-size:10px; letter-spacing:0.05em; cursor:pointer; transition:all 0.2s; }
    .mode-btn.active { background:var(--gold); color:#000; border-color:var(--gold); }
    .mode-btn:not(.active) { background:transparent; color:#555; }
    .settings-btn { width:30px; height:30px; border-radius:50%; border:1px solid var(--border); background:transparent; color:var(--muted); cursor:pointer; font-size:13px; display:flex; align-items:center; justify-content:center; }

    /* MAIN */
    .main { flex:1; padding:16px 14px 100px; max-width:700px; margin:0 auto; width:100%; }

    /* GRID */
    .agent-grid { display:grid; grid-template-columns:repeat(2,1fr); gap:10px; margin-bottom:18px; }
    .agent-card { background:var(--surface); border:1px solid var(--border); border-radius:14px; padding:14px; cursor:pointer; transition:all 0.2s; position:relative; overflow:hidden; }
    .agent-card:active { transform:scale(0.97); }
    .agent-card.selected { border-color:var(--card-color); background:color-mix(in srgb,var(--card-color) 8%,var(--surface)); }
    .agent-card.running::after { content:''; position:absolute; top:0; left:-100%; width:60%; height:2px; background:linear-gradient(90deg,transparent,var(--card-color),transparent); animation:shimmer 1.2s linear infinite; }
    .agent-card.standby { opacity:0.5; cursor:default; }
    .card-emoji { font-size:20px; margin-bottom:6px; }
    .card-tag { font-family:'JetBrains Mono',monospace; font-size:9px; color:var(--card-color); letter-spacing:0.15em; margin-bottom:4px; }
    .card-name { font-family:'Syne',sans-serif; font-weight:700; font-size:12px; color:var(--text); margin-bottom:4px; line-height:1.2; }
    .card-desc { font-size:10px; color:var(--muted); line-height:1.4; }
    .standby-badge { display:inline-block; background:#1a1a1a; border:1px solid #2a2a2a; border-radius:8px; padding:2px 8px; font-size:9px; color:#333; font-family:'JetBrains Mono',monospace; margin-top:4px; }

    /* INPUT */
    .input-section { background:var(--surface); border:1px solid var(--border); border-radius:16px; padding:16px; margin-bottom:14px; animation:fadeUp 0.3s ease; }
    .input-agent-header { display:flex; align-items:center; gap:10px; margin-bottom:12px; }
    .input-textarea { width:100%; background:#111; border:1px solid #2a2a2a; border-radius:10px; padding:12px; color:var(--text); font-size:14px; resize:none; font-family:'DM Sans',sans-serif; line-height:1.5; min-height:80px; }
    .input-textarea:focus { outline:none; border-color:#444; }
    .run-btn { margin-top:10px; padding:12px 24px; border:none; border-radius:10px; font-family:'Syne',sans-serif; font-weight:700; font-size:12px; letter-spacing:0.05em; cursor:pointer; width:100%; transition:all 0.2s; }
    .run-btn:disabled { background:#1a1a1a !important; color:#333 !important; cursor:not-allowed; }

    /* OUTPUT */
    .output-card { background:var(--surface); border-radius:14px; padding:16px; margin-bottom:12px; animation:fadeUp 0.4s ease; border-left:3px solid var(--card-color,var(--gold)); }
    .output-header { display:flex; align-items:center; gap:8px; margin-bottom:12px; flex-wrap:wrap; }
    .output-tag { font-family:'JetBrains Mono',monospace; font-size:9px; letter-spacing:0.2em; color:var(--muted); }
    .output-text { font-size:13px; line-height:1.8; color:#ccc; white-space:pre-wrap; word-break:break-word; }

    /* EMPIRE */
    .empire-banner { background:linear-gradient(135deg,#FFD70011,#FF6B3511); border:1px solid #FFD70033; border-radius:16px; padding:16px; margin-bottom:14px; text-align:center; }
    .empire-title { font-family:'Syne',sans-serif; font-weight:800; font-size:15px; color:var(--gold); margin-bottom:4px; }
    .empire-sub { color:#555; font-size:12px; }

    .spinner { width:14px; height:14px; border:2px solid #333; border-top-color:var(--gold); border-radius:50%; animation:spin 0.7s linear infinite; display:inline-block; vertical-align:middle; }
    .empty { text-align:center; padding:40px 20px; color:#2a2a2a; font-family:'Syne',sans-serif; font-size:12px; letter-spacing:0.1em; }
    .empty-icon { font-size:36px; margin-bottom:12px; opacity:0.3; }
    .toast { position:fixed; bottom:24px; left:50%; transform:translateX(-50%); background:#1a1a1a; border:1px solid #333; border-radius:20px; padding:10px 20px; font-size:12px; color:var(--text); z-index:999; animation:fadeUp 0.3s ease; white-space:nowrap; }

    /* ASSIGN STANDBY */
    .assign-box { background:#111; border:1px solid #2a2a2a; border-radius:12px; padding:16px; margin-top:12px; }
    .assign-title { font-family:'Syne',sans-serif; font-weight:700; font-size:12px; color:#555; margin-bottom:10px; letter-spacing:0.05em; }
    .assign-input { width:100%; background:#0a0a0a; border:1px solid #2a2a2a; border-radius:8px; padding:10px; color:var(--text); font-size:13px; margin-bottom:8px; }
    .assign-input:focus { outline:none; border-color:#444; }
    .assign-btn { padding:8px 16px; border:none; border-radius:8px; background:#FFD700; color:#000; font-family:'Syne',sans-serif; font-weight:700; font-size:11px; cursor:pointer; }
  </style>
</head>
<body>

<!-- SETUP -->
<div id="setup-screen">
  <div class="setup-logo">👑</div>
  <div class="setup-title">Income Empire AI</div>
  <p class="setup-sub">9 AI agents for dropshipping, trading, TikTok, YouTube & online income. Free to run.</p>
  <div class="setup-steps">
    <div class="setup-step">
      <div class="step-num">1</div>
      <div class="step-text">Go to <a href="https://aistudio.google.com" target="_blank">aistudio.google.com</a> → sign in with Gmail</div>
    </div>
    <div class="setup-step">
      <div class="step-num">2</div>
      <div class="step-text">Tap <strong style="color:#fff">"Get API Key"</strong> → <strong style="color:#fff">"Create API key"</strong> → copy it</div>
    </div>
    <div class="setup-step">
      <div class="step-num">3</div>
      <div class="step-text">Paste it below. Saved on your device only. Never shared.</div>
    </div>
  </div>
  <input class="setup-input" id="api-key-input" type="password" placeholder="Paste Gemini API key (AIza...)" />
  <button class="setup-btn" onclick="saveKey()">🚀 Launch My Empire</button>
  <p class="setup-note">Key stays on your phone only. Free tier = hundreds of uses per day.</p>
</div>

<!-- APP -->
<div id="app">
  <div class="header">
    <div class="header-title">👑 Empire AI</div>
    <div class="header-right">
      <button class="mode-btn active" id="btn-single" onclick="setMode('single')">SINGLE</button>
      <button class="mode-btn" id="btn-empire" onclick="setMode('empire')">⚡ EMPIRE</button>
      <button class="settings-btn" onclick="resetKey()">⚙️</button>
    </div>
  </div>
  <div class="main" id="main-content"></div>
</div>

<script>
const AGENTS = [
  {
    id:'orchestrator', name:'Empire Orchestrator', emoji:'👑', color:'#FFD700', tag:'MASTER AI',
    desc:'Watches over all agents, plans & coordinates the full empire',
    standby: false,
    prompt:`You are the Master Orchestrator of a 9-agent online income empire. Your agents are: Risk Manager, Dropship Store, Video Content (TikTok/YouTube), Trading Advisor, Innovation, Trend Spotter, and 2 Standby agents. When given any goal or question: 1) Give a strategic empire-wide plan, 2) Assign which agents to activate, 3) Set priorities and timelines, 4) Flag risks immediately, 5) Suggest the next 3 moves. Be decisive, clear, and think like a billionaire CEO building from R100. Use emojis per section.`
  },
  {
    id:'risk', name:'Risk Manager', emoji:'⚠️', color:'#FF4444', tag:'RISK',
    desc:'Flags dangers in trading, spending & business decisions',
    standby: false,
    prompt:`You are a risk management expert for a young South African entrepreneur building an online income empire. When given a business decision, trade, investment, or plan: 1) Rate the overall risk (Low/Medium/High/Critical), 2) List the top 5 specific risks, 3) Give a worst-case scenario, 4) Give a best-case scenario, 5) Suggest 3 risk mitigation strategies, 6) Give a clear GO / WAIT / NO-GO recommendation. Be brutally honest. Lives and money are on the line.`
  },
  {
    id:'dropship', name:'Dropship Store Agent', emoji:'🛒', color:'#00E5FF', tag:'STORE',
    desc:'Products, listings, pricing for your dropshipping store',
    standby: false,
    prompt:`You are an expert dropshipping store agent for 2025. Given a niche or product: 1) Top 3 winning products with demand signals, 2) Pricing strategy (cost, sell price, margin), 3) Ready-to-use Shopify product listing (title, description, 5 bullet points), 4) Upsell & bundle opportunities, 5) Best platform to sell on (Shopify, WooCommerce, TikTok Shop). Focus on products that work in South Africa and globally.`
  },
  {
    id:'video', name:'Video Content Agent', emoji:'🎬', color:'#FF3EAC', tag:'TIKTOK & YOUTUBE',
    desc:'AI-generated scripts + free tools to make the actual videos',
    standby: false,
    prompt:`You are a viral video content strategist for TikTok and YouTube in 2025. Given a product, niche, or topic: 1) 3 TikTok video concepts (format, hook, script, caption, hashtags), 2) 2 YouTube video concepts (title, thumbnail idea, full script outline, SEO tags), 3) FREE AI video tools to actually produce the content — include: CapCut AI, Pictory, InVideo AI, HeyGen, D-ID, ElevenLabs (voiceover), RunwayML — explain what each is used for, 4) Posting schedule for maximum reach, 5) Monetization path (TikTok Shop, YouTube AdSense, sponsorships). Write hooks that stop the scroll immediately.`
  },
  {
    id:'trading', name:'Trading Advisor', emoji:'📈', color:'#A8FF3E', tag:'TRADING',
    desc:'Crypto, forex & stocks analysis and strategy',
    standby: false,
    prompt:`You are a trading analyst covering crypto, forex, and stocks. IMPORTANT DISCLAIMER: Always remind the user you are an AI and not a licensed financial advisor — all analysis is for educational purposes only. When given a trading question or asset: 1) Market overview & current sentiment, 2) Key support and resistance levels, 3) Potential entry points and targets, 4) Stop-loss suggestions, 5) Risk/reward ratio, 6) Relevant news or catalysts, 7) Strategy recommendation (scalp/swing/hold). Always emphasize risk management. Never guarantee profits. Be analytical and data-aware.`
  },
  {
    id:'innovation', name:'Innovation Agent', emoji:'💡', color:'#BF5FFF', tag:'INNOVATION',
    desc:'New money-making ideas & untapped opportunities',
    standby: false,
    prompt:`You are an innovation and new income stream expert for digital entrepreneurs in 2025, specifically thinking about someone in South Africa with a phone and internet access. When asked for ideas or given a context: 1) Generate 5 fresh, unconventional money-making ideas, 2) For each idea: explain the concept, startup cost (in Rands), time to first income, difficulty level (Easy/Medium/Hard), and income potential, 3) Highlight the single best idea for someone starting with almost nothing, 4) Give a 7-day action plan for that idea, 5) Mention any free tools or platforms needed. Think creatively — AI tools, digital products, local opportunities, arbitrage, content, services.`
  },
  {
    id:'trends', name:'Trend Spotter', emoji:'🔎', color:'#00FFB3', tag:'TRENDS',
    desc:'Finds trending products for your store before they blow up',
    standby: false,
    prompt:`You are a product trend analyst for dropshipping and ecommerce in 2025. When given a niche or asked for trends: 1) Top 7 trending products RIGHT NOW with evidence (TikTok views, search volume signals, seasonal demand), 2) For each: trend stage (Early/Growing/Peak/Declining), target audience, estimated sell price, and why it's hot, 3) Top 3 products to add to a dropshipping store immediately, 4) Upcoming trends to get ahead of in the next 30-60 days, 5) Platforms where these trends are biggest (TikTok, Instagram, Pinterest, Google). Be specific with product names, not just categories.`
  },
  {
    id:'standbyA', name:'Standby Agent A', emoji:'🔒', color:'#333333', tag:'STANDBY',
    desc:'Not assigned yet — tap to set a role',
    standby: true,
    prompt:''
  },
  {
    id:'standbyB', name:'Standby Agent B', emoji:'🔒', color:'#333333', tag:'STANDBY',
    desc:'Not assigned yet — tap to set a role',
    standby: true,
    prompt:''
  },
];

let apiKey = localStorage.getItem('gemini_key') || '';
let currentMode = 'single';
let selectedAgent = null;
let customAgents = JSON.parse(localStorage.getItem('custom_agents') || '{}');

// Apply any saved custom agents
AGENTS.forEach(a => {
  if (customAgents[a.id]) {
    a.name = customAgents[a.id].name;
    a.desc = customAgents[a.id].desc;
    a.prompt = customAgents[a.id].prompt;
    a.emoji = customAgents[a.id].emoji;
    a.color = customAgents[a.id].color;
    a.standby = false;
  }
});

window.onload = () => { if (apiKey) showApp(); };

function saveKey() {
  const val = document.getElementById('api-key-input').value.trim();
  if (!val || !val.startsWith('AIza')) { showToast('⚠️ Key must start with AIza...'); return; }
  localStorage.setItem('gemini_key', val);
  apiKey = val;
  showApp();
}

function resetKey() {
  if (confirm('Reset API key?')) { localStorage.removeItem('gemini_key'); location.reload(); }
}

function showApp() {
  document.getElementById('setup-screen').style.display = 'none';
  document.getElementById('app').style.display = 'flex';
  renderMode();
}

function setMode(m) {
  currentMode = m; selectedAgent = null;
  document.getElementById('btn-single').classList.toggle('active', m==='single');
  document.getElementById('btn-empire').classList.toggle('active', m==='empire');
  renderMode();
}

function renderMode() {
  document.getElementById('main-content').innerHTML =
    currentMode === 'single' ? renderSingle() : renderEmpire();
}

function renderSingle() {
  const grid = AGENTS.map(a => `
    <div class="agent-card${selectedAgent?.id===a.id?' selected':''}${a.standby?' standby':''}"
      style="--card-color:${a.color}"
      onclick="${a.standby ? `selectStandby('${a.id}')` : `selectAgent('${a.id}')`}">
      <div class="card-emoji">${a.emoji}</div>
      <div class="card-tag">${a.tag}</div>
      <div class="card-name">${a.name}</div>
      <div class="card-desc">${a.desc}</div>
      ${a.standby ? '<div class="standby-badge">TAP TO ASSIGN</div>' : ''}
    </div>`).join('');

  const inputArea = selectedAgent
    ? selectedAgent.standby
      ? renderAssignBox(selectedAgent)
      : renderInputBox(selectedAgent)
    : `<div class="empty"><div class="empty-icon">👆</div>TAP AN AGENT TO BEGIN</div>`;

  return `<div class="agent-grid">${grid}</div>${inputArea}<div id="output-area"></div>`;
}

function renderInputBox(agent) {
  return `
    <div class="input-section" style="border-color:${agent.color}44">
      <div class="input-agent-header">
        <span style="font-size:22px">${agent.emoji}</span>
        <div>
          <div style="font-family:'Syne',sans-serif;font-weight:700;font-size:13px;color:${agent.color}">${agent.name}</div>
          <div style="font-size:11px;color:var(--muted)">${agent.desc}</div>
        </div>
      </div>
      <textarea class="input-textarea" id="user-input"
        placeholder="Ask ${agent.name} anything..." rows="4"></textarea>
      <button class="run-btn" id="run-btn"
        style="background:${agent.color};color:#000"
        onclick="runSingle()">Run ${agent.name} →</button>
    </div>`;
}

function renderAssignBox(agent) {
  return `
    <div class="input-section" style="border-color:#333">
      <div class="input-agent-header">
        <span style="font-size:22px">🔒</span>
        <div>
          <div style="font-family:'Syne',sans-serif;font-weight:700;font-size:13px;color:#555">Assign ${agent.name}</div>
          <div style="font-size:11px;color:var(--muted)">Give this agent a role and it'll be ready to use</div>
        </div>
      </div>
      <div class="assign-box">
        <div class="assign-title">WHAT SHOULD THIS AGENT DO?</div>
        <input class="assign-input" id="assign-name" placeholder="Agent name (e.g. Email Marketing Agent)" />
        <input class="assign-input" id="assign-desc" placeholder="Short description (e.g. Write email sequences that convert)" />
        <textarea class="input-textarea" id="assign-role" rows="3"
          placeholder="Describe its role in detail... (e.g. You are an email marketing expert. When given a product, write a 5-email welcome sequence...)"></textarea>
        <div style="margin-top:10px;display:flex;gap:8px;flex-wrap:wrap">
          <button class="assign-btn" onclick="assignAgent('${agent.id}','💌','#FF6B35')">💌 Marketing</button>
          <button class="assign-btn" onclick="assignAgent('${agent.id}','🖨️','#00E5FF')">🖨️ Print on Demand</button>
          <button class="assign-btn" onclick="assignAgent('${agent.id}','🎧','#BF5FFF')">🎧 Freelance</button>
          <button class="assign-btn" onclick="assignAgent('${agent.id}','📦','#A8FF3E')">📦 Custom</button>
        </div>
      </div>
    </div>`;
}

function assignAgent(agentId, emoji, color) {
  const name = document.getElementById('assign-name')?.value?.trim();
  const desc = document.getElementById('assign-desc')?.value?.trim();
  const role = document.getElementById('assign-role')?.value?.trim();
  if (!name || !role) { showToast('Fill in the name and role first!'); return; }
  const agent = AGENTS.find(a => a.id === agentId);
  agent.name = name;
  agent.desc = desc || name;
  agent.prompt = role;
  agent.emoji = emoji;
  agent.color = color;
  agent.standby = false;
  customAgents[agentId] = { name, desc: agent.desc, prompt: role, emoji, color };
  localStorage.setItem('custom_agents', JSON.stringify(customAgents));
  selectedAgent = agent;
  showToast(`✅ ${name} is now active!`);
  renderMode();
}

function renderEmpire() {
  return `
    <div class="empire-banner">
      <div class="empire-title">⚡ Empire Mode</div>
      <div class="empire-sub">One goal. All active agents deploy together.</div>
    </div>
    <div class="input-section" style="border-color:#FFD70033">
      <textarea class="input-textarea" id="empire-input"
        placeholder="e.g. I want to start selling wireless earbuds via dropshipping and promote on TikTok and YouTube while managing risk carefully"
        rows="4"></textarea>
      <button class="run-btn" id="empire-btn"
        style="background:linear-gradient(135deg,#FFD700,#FF6B35);color:#000"
        onclick="runEmpire()">⚡ Deploy All Agents →</button>
    </div>
    <div id="empire-output"></div>`;
}

function selectAgent(id) {
  selectedAgent = AGENTS.find(a => a.id === id);
  renderMode();
  setTimeout(() => document.getElementById('user-input')?.focus(), 100);
}

function selectStandby(id) {
  selectedAgent = AGENTS.find(a => a.id === id);
  renderMode();
}

async function callGemini(prompt, message) {
  const res = await fetch(
    `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`,
    {
      method:'POST',
      headers:{'Content-Type':'application/json'},
      body: JSON.stringify({
        contents:[{parts:[{text:`${prompt}\n\nUser: ${message}`}]}],
        generationConfig:{maxOutputTokens:1400, temperature:0.8}
      })
    }
  );
  const d = await res.json();
  if (d.error) throw new Error(d.error.message);
  return d.candidates?.[0]?.content?.parts?.[0]?.text || 'No response.';
}

async function runSingle() {
  const input = document.getElementById('user-input')?.value?.trim();
  if (!input) { showToast('Type your request first!'); return; }
  const btn = document.getElementById('run-btn');
  btn.disabled = true;
  btn.innerHTML = '<span class="spinner"></span> Working...';
  const out = document.getElementById('output-area');
  out.innerHTML = `
    <div class="output-card" style="--card-color:${selectedAgent.color};border-left-color:${selectedAgent.color}">
      <div class="output-header">
        <span>${selectedAgent.emoji}</span>
        <span class="output-tag">THINKING...</span>
      </div>
      <div style="color:#333;animation:pulse 1s infinite;font-size:13px">...</div>
    </div>`;
  try {
    const result = await callGemini(selectedAgent.prompt, input);
    out.innerHTML = `
      <div class="output-card" style="--card-color:${selectedAgent.color};border-left-color:${selectedAgent.color}">
        <div class="output-header">
          <span>${selectedAgent.emoji}</span>
          <span style="font-family:'Syne',sans-serif;font-weight:700;font-size:12px;color:${selectedAgent.color}">${selectedAgent.name}</span>
          <span class="output-tag" style="margin-left:auto">DONE ✓</span>
        </div>
        <div class="output-text">${esc(result)}</div>
      </div>`;
  } catch(e) {
    out.innerHTML = `
      <div class="output-card" style="border-left-color:#ff4444">
        <div class="output-text" style="color:#ff6666">❌ ${e.message}<br><br>Tap ⚙️ to check your API key.</div>
      </div>`;
  }
  btn.disabled = false;
  btn.innerHTML = `Run ${selectedAgent.name} →`;
}

async function runEmpire() {
  const input = document.getElementById('empire-input')?.value?.trim();
  if (!input) { showToast('Describe your goal first!'); return; }
  const btn = document.getElementById('empire-btn');
  btn.disabled = true;
  btn.innerHTML = '⚡ Deploying...';
  const out = document.getElementById('empire-output');
  out.innerHTML = '';
  const activeAgents = AGENTS.filter(a => !a.standby);
  let ctx = '';
  for (let i = 0; i < activeAgents.length; i++) {
    const a = activeAgents[i];
    const id = `out-${a.id}`;
    out.innerHTML += `
      <div class="output-card running" id="${id}" style="--card-color:${a.color};border-left-color:${a.color}">
        <div class="output-header">
          <span>${a.emoji}</span>
          <span style="font-family:'Syne',sans-serif;font-weight:700;font-size:12px;color:${a.color}">${a.name}</span>
          <span style="margin-left:auto"><span class="spinner"></span></span>
        </div>
        <div style="color:#333;font-size:13px;animation:pulse 1s infinite">Working...</div>
      </div>`;
    document.getElementById(id).scrollIntoView({behavior:'smooth',block:'nearest'});
    try {
      const result = await callGemini(a.prompt, (ctx ? `Strategic context:\n${ctx}\n\n` : '') + input);
      if (i === 0) ctx = result;
      document.getElementById(id).innerHTML = `
        <div class="output-header">
          <span>${a.emoji}</span>
          <span style="font-family:'Syne',sans-serif;font-weight:700;font-size:12px;color:${a.color}">${a.name}</span>
          <span class="output-tag" style="margin-left:auto;color:${a.color}">DONE ✓</span>
        </div>
        <div class="output-text">${esc(result)}</div>`;
      document.getElementById(id).classList.remove('running');
    } catch(e) {
      document.getElementById(id).innerHTML = `
        <div class="output-header">
          <span>${a.emoji}</span>
          <span style="font-family:'Syne',sans-serif;font-weight:700;font-size:12px;color:${a.color}">${a.name}</span>
        </div>
        <div class="output-text" style="color:#ff6666">❌ ${e.message}</div>`;
    }
  }
  btn.disabled = false;
  btn.innerHTML = '⚡ Deploy All Agents →';
}

function esc(s) {
  return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
}

function showToast(msg) {
  const t = document.createElement('div');
  t.className = 'toast';
  t.textContent = msg;
  document.body.appendChild(t);
  setTimeout(() => t.remove(), 2500);
}
</script>
</body>
</html>
