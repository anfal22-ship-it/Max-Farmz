bash

cat > /mnt/user-data/outputs/maxfarmz_ghee_dashboard.html << 'HTMLEOF'
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Maxfarmz — Butter Ghee Sales Dashboard</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<style>
  *{box-sizing:border-box;margin:0;padding:0}
  body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;background:#F7F6F2;color:#1a1a1a;min-height:100vh;padding:2rem}
  .container{max-width:1100px;margin:0 auto}
  .hdr{display:flex;justify-content:space-between;align-items:center;margin-bottom:1.5rem;padding-bottom:1rem;border-bottom:1px solid #E0DED6;flex-wrap:wrap;gap:10px}
  .brand{display:flex;align-items:center;gap:12px}
  .logo{width:38px;height:38px;border-radius:8px;background:#1D9E75;display:flex;align-items:center;justify-content:center;color:#fff;font-size:13px;font-weight:600}
  .hdr-title{font-size:16px;font-weight:600;color:#1a1a1a}
  .hdr-sub{font-size:12px;color:#6b6b6b;margin-top:2px}
  .period-row{display:flex;gap:6px;align-items:center}
  .pb{font-size:12px;padding:5px 12px;border-radius:6px;border:1px solid #D0CEC6;background:#fff;color:#6b6b6b;cursor:pointer;transition:all .15s}
  .pb:hover{background:#f0efe9}
  .pb.active{background:#1a1a1a;color:#fff;border-color:#1a1a1a}
  .channel-cards{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-bottom:1.25rem}
  .cc{border-radius:10px;padding:1rem 1.25rem;border:1px solid transparent}
  .cc-b2b{border-color:#1D9E75;background:#F0FBF7}
  .cc-ws{border-color:#378ADD;background:#EEF6FD}
  .cc-rt{border-color:#EF9F27;background:#FEF7EC}
  .cc-label{font-size:11px;font-weight:600;text-transform:uppercase;letter-spacing:.05em;margin-bottom:4px}
  .cc-b2b .cc-label{color:#0F6E56}
  .cc-ws .cc-label{color:#185FA5}
  .cc-rt .cc-label{color:#854F0B}
  .cc-val{font-size:22px;font-weight:600;color:#1a1a1a;margin-bottom:2px}
  .cc-sub{font-size:11px;color:#6b6b6b}
  .band-wrap{background:#fff;border:1px solid #E0DED6;border-radius:10px;padding:1rem 1.25rem;margin-bottom:1.25rem}
  .band-title{font-size:11px;font-weight:600;color:#6b6b6b;text-transform:uppercase;letter-spacing:.05em;margin-bottom:.75rem}
  .channel-band{display:flex;border-radius:6px;overflow:hidden;height:26px;margin-bottom:.5rem}
  .cb-seg{display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:600;color:#fff}
  .band-legend{display:flex;gap:16px;font-size:11px;color:#6b6b6b}
  .band-legend span{display:flex;align-items:center;gap:4px}
  .bl-dot{width:8px;height:8px;border-radius:2px}
  .tabs{display:flex;gap:0;border-bottom:1px solid #E0DED6;margin-bottom:1.25rem}
  .tab{font-size:13px;padding:8px 18px;border:none;background:transparent;color:#6b6b6b;cursor:pointer;border-bottom:2px solid transparent;margin-bottom:-1px}
  .tab:hover{color:#1a1a1a}
  .tab.active{color:#1a1a1a;font-weight:600;border-bottom-color:#1a1a1a}
  .view{display:none}
  .view.active{display:block}
  .kpis{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-bottom:1.25rem}
  .kc{background:#fff;border:1px solid #E0DED6;border-radius:8px;padding:.875rem 1rem}
  .kc-l{font-size:11px;color:#6b6b6b;text-transform:uppercase;letter-spacing:.04em;margin-bottom:4px}
  .kc-v{font-size:20px;font-weight:600;color:#1a1a1a}
  .kc-c{font-size:11px;margin-top:3px}
  .up{color:#0F6E56}.dn{color:#A32D2D}.nt{color:#6b6b6b}
  .g2{display:grid;grid-template-columns:1.3fr 1fr;gap:1rem;margin-bottom:1rem}
  .g2b{display:grid;grid-template-columns:1fr 1fr;gap:1rem;margin-bottom:1rem}
  .card{background:#fff;border:1px solid #E0DED6;border-radius:10px;padding:1rem 1.25rem}
  .ct{font-size:11px;font-weight:600;color:#6b6b6b;text-transform:uppercase;letter-spacing:.05em;margin-bottom:.875rem}
  .pipe-item{display:flex;align-items:flex-start;gap:8px;padding:.55rem 0;border-bottom:1px solid #F0EDE6}
  .pipe-item:last-child{border-bottom:none}
  .dot{width:7px;height:7px;border-radius:50%;flex-shrink:0;margin-top:4px}
  .pn{font-size:12px;font-weight:600;color:#1a1a1a;margin-bottom:1px}
  .pm{font-size:11px;color:#6b6b6b}
  .pr{margin-left:auto;text-align:right;flex-shrink:0}
  .pv{font-size:12px;font-weight:600;color:#1a1a1a}
  .badge{display:inline-block;font-size:10px;padding:2px 6px;border-radius:4px;font-weight:600;margin-top:2px}
  .bs{background:#D1FAE5;color:#065F46}
  .bw{background:#FEF3C7;color:#92400E}
  .bd{background:#FEE2E2;color:#991B1B}
  .bi{background:#DBEAFE;color:#1E40AF}
  .prog-row{display:flex;align-items:center;gap:8px;margin-bottom:9px}
  .pl{font-size:11px;color:#1a1a1a;width:100px;flex-shrink:0;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
  .pt{flex:1;height:5px;background:#F0EDE6;border-radius:3px}
  .pf{height:5px;border-radius:3px}
  .pp{font-size:11px;color:#6b6b6b;width:28px;text-align:right;flex-shrink:0}
  .bottom-grid{display:grid;grid-template-columns:1fr 1fr;gap:1rem;margin-bottom:1.5rem}
  .leg-row{display:flex;gap:14px;margin-bottom:8px;font-size:11px;color:#6b6b6b;flex-wrap:wrap}
  .leg-row span{display:flex;align-items:center;gap:4px}
  .leg-dot{width:8px;height:8px;border-radius:2px;display:inline-block}
  .ftr{display:flex;justify-content:space-between;align-items:center;padding-top:1rem;border-top:1px solid #E0DED6;flex-wrap:wrap;gap:8px}
  .ftr-note{font-size:11px;color:#6b6b6b}
  .ftr-btns{display:flex;gap:8px}
  .edit-bar{background:#1D9E75;color:#fff;border-radius:8px;padding:.75rem 1.25rem;margin-bottom:1.5rem;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px}
  .edit-bar-text{font-size:13px;font-weight:500}
  .edit-toggle{font-size:12px;padding:5px 12px;border-radius:6px;border:1px solid rgba(255,255,255,0.4);background:transparent;color:#fff;cursor:pointer}
  .edit-toggle:hover{background:rgba(255,255,255,0.15)}
  [contenteditable="true"]{outline:2px dashed #1D9E75;border-radius:3px;padding:1px 3px;min-width:20px;display:inline-block}
  @media(max-width:768px){
    .channel-cards{grid-template-columns:1fr}
    .kpis{grid-template-columns:repeat(2,1fr)}
    .g2,.g2b,.bottom-grid{grid-template-columns:1fr}
    body{padding:1rem}
  }
  @media print{
    body{background:#fff;padding:0}
    .edit-bar,.ftr-btns{display:none}
  }
</style>
</head>
<body>
<div class="container">

  <div class="edit-bar">
    <div class="edit-bar-text">✏️ Edit mode — click any number or name on the dashboard to update it directly</div>
    <button class="edit-toggle" onclick="toggleEdit()">Enable editing</button>
  </div>

  <div class="hdr">
    <div class="brand">
      <div class="logo">MF</div>
      <div>
        <div class="hdr-title">Maxfarmz · Butter Ghee · Sales Dashboard</div>
        <div class="hdr-sub" id="hdr-sub">Jeddah · B2B · Wholesale · Retail</div>
      </div>
    </div>
    <div class="period-row">
      <button class="pb active" onclick="setPeriod('daily',this)">Daily</button>
      <button class="pb" onclick="setPeriod('weekly',this)">Weekly</button>
      <button class="pb" onclick="setPeriod('monthly',this)">Monthly</button>
    </div>
  </div>

  <div class="channel-cards">
    <div class="cc cc-b2b">
      <div class="cc-label">B2B · HORECA</div>
      <div class="cc-val" id="b2b-rev">SAR 43,500</div>
      <div class="cc-sub" id="b2b-sub">↑ 8.2% vs last week</div>
    </div>
    <div class="cc cc-ws">
      <div class="cc-label">Wholesale</div>
      <div class="cc-val" id="ws-rev">SAR 28,200</div>
      <div class="cc-sub" id="ws-sub">↑ 5.1% vs last week</div>
    </div>
    <div class="cc cc-rt">
      <div class="cc-label">Retail</div>
      <div class="cc-val" id="rt-rev">SAR 18,600</div>
      <div class="cc-sub" id="rt-sub">↑ 3.8% vs last week</div>
    </div>
  </div>

  <div class="band-wrap">
    <div class="band-title">Revenue split by channel</div>
    <div class="channel-band">
      <div class="cb-seg" style="background:#1D9E75;flex:43.5">B2B 48%</div>
      <div class="cb-seg" style="background:#378ADD;flex:28.2">WS 31%</div>
      <div class="cb-seg" style="background:#EF9F27;flex:18.6">Retail 21%</div>
    </div>
    <div class="band-legend">
      <span><span class="bl-dot" style="background:#1D9E75"></span>B2B / HORECA</span>
      <span><span class="bl-dot" style="background:#378ADD"></span>Wholesale</span>
      <span><span class="bl-dot" style="background:#EF9F27"></span>Retail</span>
    </div>
  </div>

  <div class="tabs">
    <button class="tab active" onclick="setTab('b2b',this)">B2B / HORECA</button>
    <button class="tab" onclick="setTab('wholesale',this)">Wholesale</button>
    <button class="tab" onclick="setTab('retail',this)">Retail</button>
  </div>

  <!-- B2B VIEW -->
  <div id="view-b2b" class="view active">
    <div class="kpis" id="kpis-b2b"></div>
    <div class="g2">
      <div class="card">
        <div class="ct">B2B revenue trend</div>
        <div style="position:relative;height:150px"><canvas id="b2b-trend" role="img" aria-label="B2B revenue trend chart"></canvas></div>
      </div>
      <div class="card">
        <div class="ct">Deal pipeline</div>
        <div id="b2b-pipe"></div>
      </div>
    </div>
    <div class="g2b">
      <div class="card">
        <div class="ct">Top accounts</div>
        <div id="b2b-accounts"></div>
      </div>
      <div class="card">
        <div class="ct">Segment breakdown</div>
        <div id="b2b-segs"></div>
      </div>
    </div>
  </div>

  <!-- WHOLESALE VIEW -->
  <div id="view-wholesale" class="view">
    <div class="kpis" id="kpis-ws"></div>
    <div class="g2">
      <div class="card">
        <div class="ct">Wholesale revenue trend</div>
        <div style="position:relative;height:150px"><canvas id="ws-trend" role="img" aria-label="Wholesale revenue trend"></canvas></div>
      </div>
      <div class="card">
        <div class="ct">Active orders</div>
        <div id="ws-orders"></div>
      </div>
    </div>
    <div class="g2b">
      <div class="card">
        <div class="ct">Key distributors</div>
        <div id="ws-clients"></div>
      </div>
      <div class="card">
        <div class="ct">Volume by SKU (units)</div>
        <div style="position:relative;height:160px"><canvas id="ws-sku" role="img" aria-label="Volume by SKU bar chart"></canvas></div>
      </div>
    </div>
  </div>

  <!-- RETAIL VIEW -->
  <div id="view-retail" class="view">
    <div class="kpis" id="kpis-rt"></div>
    <div class="g2">
      <div class="card">
        <div class="ct">Retail revenue trend</div>
        <div style="position:relative;height:150px"><canvas id="rt-trend" role="img" aria-label="Retail revenue trend"></canvas></div>
      </div>
      <div class="card">
        <div class="ct">Store performance</div>
        <div id="rt-stores"></div>
      </div>
    </div>
    <div class="g2b">
      <div class="card">
        <div class="ct">Product sell-through</div>
        <div id="rt-products"></div>
      </div>
      <div class="card">
        <div class="ct">Retail channel mix</div>
        <div id="rt-mix-leg" style="display:flex;flex-direction:column;gap:6px;margin-bottom:8px;font-size:11px;color:#6b6b6b"></div>
        <div style="position:relative;height:110px"><canvas id="rt-mix" role="img" aria-label="Retail channel mix donut"></canvas></div>
      </div>
    </div>
  </div>

  <!-- BOTTOM SUMMARY -->
  <div class="bottom-grid" style="margin-top:1rem">
    <div class="card">
      <div class="ct">All channels — target vs actual</div>
      <div class="leg-row">
        <span><span class="leg-dot" style="background:rgba(128,128,128,0.3)"></span>Target</span>
        <span><span class="leg-dot" style="background:#1D9E75"></span>Actual</span>
      </div>
      <div style="position:relative;height:150px"><canvas id="all-target" role="img" aria-label="Target vs actual all channels"></canvas></div>
    </div>
    <div class="card">
      <div class="ct">Combined monthly trend</div>
      <div class="leg-row">
        <span><span class="leg-dot" style="background:#1D9E75"></span>B2B</span>
        <span><span class="leg-dot" style="background:#378ADD"></span>Wholesale</span>
        <span><span class="leg-dot" style="background:#EF9F27"></span>Retail</span>
      </div>
      <div style="position:relative;height:150px"><canvas id="combined-trend" role="img" aria-label="Combined monthly trend all channels"></canvas></div>
    </div>
  </div>

  <div class="ftr">
    <div class="ftr-note" id="ftr-note"></div>
    <div class="ftr-btns">
      <button class="pb" onclick="window.print()">Print / Save PDF</button>
    </div>
  </div>

</div>

<script>
document.getElementById('ftr-note').textContent = 'Maxfarmz · Jeddah Sales Report · Generated ' + new Date().toLocaleDateString('en-GB',{day:'numeric',month:'short',year:'numeric'});

// ─── EDIT MODE ───
let editMode = false;
function toggleEdit(){
  editMode = !editMode;
  const btn = document.querySelector('.edit-toggle');
  const bar = document.querySelector('.edit-bar');
  btn.textContent = editMode ? 'Disable editing' : 'Enable editing';
  bar.style.background = editMode ? '#0F6E56' : '#1D9E75';
  document.querySelectorAll('.kc-v, .pv, .pn, .pm, .cc-val, .cc-sub').forEach(el => {
    el.contentEditable = editMode ? 'true' : 'false';
  });
}

// ─── DATA ───
const periods = {
  daily:{
    b2b:{rev:'SAR 8,400',sub:'↑ 12% vs yesterday',kpis:[{l:'Revenue today',v:'SAR 8,400',c:'↑ 12%',t:'up'},{l:'Meetings',v:'3',c:'2 tomorrow',t:'nt'},{l:'Quotes sent',v:'4',c:'SAR 31k value',t:'nt'},{l:'Closed today',v:'1 deal',c:'SAR 8,400',t:'up'}],trend:{l:['Mon','Tue','Wed','Thu','Fri','Today'],d:[5800,6400,5200,7800,6900,8400]}},
    ws:{rev:'SAR 5,100',sub:'↑ 6% vs yesterday',kpis:[{l:'Revenue today',v:'SAR 5,100',c:'↑ 6%',t:'up'},{l:'Orders placed',v:'6',c:'3 dispatched',t:'nt'},{l:'Pallets moved',v:'14',c:'this day',t:'nt'},{l:'New inquiry',v:'2',c:'pending quote',t:'nt'}],trend:{l:['Mon','Tue','Wed','Thu','Fri','Today'],d:[3900,4600,3700,5200,4800,5100]}},
    rt:{rev:'SAR 3,200',sub:'↑ 4% vs yesterday',kpis:[{l:'Revenue today',v:'SAR 3,200',c:'↑ 4%',t:'up'},{l:'Units sold',v:'48',c:'across outlets',t:'nt'},{l:'Outlets active',v:'9',c:'of 12',t:'nt'},{l:'Returns',v:'0',c:'today',t:'up'}],trend:{l:['Mon','Tue','Wed','Thu','Fri','Today'],d:[2800,3100,2600,3400,3000,3200]}}
  },
  weekly:{
    b2b:{rev:'SAR 43,500',sub:'↑ 8.2% vs last week',kpis:[{l:'WTD revenue',v:'SAR 43,500',c:'↑ 8.2%',t:'up'},{l:'New clients',v:'2',c:'this week',t:'up'},{l:'Win rate',v:'44%',c:'4 of 9 quotes',t:'up'},{l:'Pipeline',v:'SAR 175k',c:'5 active deals',t:'nt'}],trend:{l:['Wk1','Wk2','Wk3','Wk4','Wk5','This'],d:[34000,38500,41000,36000,40200,43500]}},
    ws:{rev:'SAR 28,200',sub:'↑ 5.1% vs last week',kpis:[{l:'WTD revenue',v:'SAR 28,200',c:'↑ 5.1%',t:'up'},{l:'Orders',v:'31',c:'this week',t:'nt'},{l:'Avg order',v:'SAR 910',c:'per order',t:'nt'},{l:'New traders',v:'1',c:'onboarded',t:'up'}],trend:{l:['Wk1','Wk2','Wk3','Wk4','Wk5','This'],d:[22000,25500,23000,26800,26800,28200]}},
    rt:{rev:'SAR 18,600',sub:'↑ 3.8% vs last week',kpis:[{l:'WTD revenue',v:'SAR 18,600',c:'↑ 3.8%',t:'up'},{l:'Outlets',v:'12',c:'active this week',t:'nt'},{l:'Sell-through',v:'71%',c:'of stocked units',t:'up'},{l:'Reorders',v:'4',c:'stores reordered',t:'up'}],trend:{l:['Wk1','Wk2','Wk3','Wk4','Wk5','This'],d:[15000,16200,14800,17900,17900,18600]}}
  },
  monthly:{
    b2b:{rev:'SAR 97,500',sub:'↑ 10.8% vs last month',kpis:[{l:'MTD revenue',v:'SAR 97,500',c:'↑ 10.8%',t:'up'},{l:'Target',v:'65%',c:'of SAR 150k',t:'nt'},{l:'Active accounts',v:'24',c:'+4 new',t:'up'},{l:'Pipeline',v:'SAR 175k',c:'5 deals open',t:'nt'}],trend:{l:['Jan','Feb','Mar','Apr','May','Jun'],d:[62000,71000,68000,80000,88000,97500]}},
    ws:{rev:'SAR 64,800',sub:'↑ 7.2% vs last month',kpis:[{l:'MTD revenue',v:'SAR 64,800',c:'↑ 7.2%',t:'up'},{l:'Target',v:'72%',c:'of SAR 90k',t:'nt'},{l:'Active traders',v:'11',c:'+2 new',t:'up'},{l:'Avg order',v:'SAR 5,890',c:'per account',t:'nt'}],trend:{l:['Jan','Feb','Mar','Apr','May','Jun'],d:[42000,48000,51000,57000,60500,64800]}},
    rt:{rev:'SAR 38,400',sub:'↑ 5.5% vs last month',kpis:[{l:'MTD revenue',v:'SAR 38,400',c:'↑ 5.5%',t:'up'},{l:'Target',v:'77%',c:'of SAR 50k',t:'nt'},{l:'Outlets',v:'12',c:'across Jeddah',t:'nt'},{l:'Shelf share',v:'18%',c:'vs 14% last mo',t:'up'}],trend:{l:['Jan','Feb','Mar','Apr','May','Jun'],d:[28000,30500,32000,34800,36400,38400]}}
  }
};

let curPeriod = 'weekly';
let b2bChart, wsChart, rtChart;

function setPeriod(p, btn){
  curPeriod = p;
  document.querySelectorAll('.pb').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  const d = periods[p];
  document.getElementById('b2b-rev').textContent = d.b2b.rev;
  document.getElementById('b2b-sub').textContent = d.b2b.sub;
  document.getElementById('ws-rev').textContent = d.ws.rev;
  document.getElementById('ws-sub').textContent = d.ws.sub;
  document.getElementById('rt-rev').textContent = d.rt.rev;
  document.getElementById('rt-sub').textContent = d.rt.sub;
  renderKPIs('kpis-b2b', d.b2b.kpis);
  renderKPIs('kpis-ws', d.ws.kpis);
  renderKPIs('kpis-rt', d.rt.kpis);
  if(b2bChart){b2bChart.data.labels=d.b2b.trend.l;b2bChart.data.datasets[0].data=d.b2b.trend.d;b2bChart.update();}
  if(wsChart){wsChart.data.labels=d.ws.trend.l;wsChart.data.datasets[0].data=d.ws.trend.d;wsChart.update();}
  if(rtChart){rtChart.data.labels=d.rt.trend.l;rtChart.data.datasets[0].data=d.rt.trend.d;rtChart.update();}
}

function setTab(t, btn){
  document.querySelectorAll('.tab').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
  document.getElementById('view-'+t).classList.add('active');
}

function renderKPIs(id, data){
  document.getElementById(id).innerHTML = data.map(k =>
    `<div class="kc"><div class="kc-l">${k.l}</div><div class="kc-v">${k.v}</div><div class="kc-c ${k.t}">${k.c}</div></div>`
  ).join('');
}

renderKPIs('kpis-b2b', periods.weekly.b2b.kpis);
renderKPIs('kpis-ws', periods.weekly.ws.kpis);
renderKPIs('kpis-rt', periods.weekly.rt.kpis);

// ─── PIPELINE DATA ───
const b2bPipe = [
  {name:'Bidfood KSA',seg:'Distributor',val:'SAR 35k',stage:'Pitched',dot:'#EF9F27',badge:'bw'},
  {name:'CATRION',seg:'Catering',val:'SAR 28k',stage:'Negotiating',dot:'#378ADD',badge:'bi'},
  {name:'Unique Catering',seg:'Catering',val:'SAR 22k',stage:'Negotiating',dot:'#378ADD',badge:'bi'},
  {name:'Waldorf Astoria',seg:'Hotel',val:'SAR 16k',stage:'Pitched',dot:'#EF9F27',badge:'bw'},
  {name:'Sunbulah Group',seg:'Mfg',val:'SAR 18.5k',stage:'Pending',dot:'#E24B4A',badge:'bd'}
];
document.getElementById('b2b-pipe').innerHTML = b2bPipe.map(p =>
  `<div class="pipe-item"><div class="dot" style="background:${p.dot}"></div><div><div class="pn">${p.name}</div><div class="pm">${p.seg}</div></div><div class="pr"><div class="pv">${p.val}</div><span class="badge ${p.badge}">${p.stage}</span></div></div>`
).join('');

const b2bAccounts = [
  {name:'Saudi Airlines Catering',val:'SAR 18,500',badge:'bs',stage:'Active'},
  {name:'Ritz-Carlton Jeddah',val:'SAR 14,200',badge:'bs',stage:'Active'},
  {name:'Halwani Brothers',val:'SAR 12,800',badge:'bs',stage:'Active'},
  {name:'Emad Bakeries',val:'SAR 9,400',badge:'bw',stage:'Follow-up'},
  {name:'Grand Hyatt Jeddah',val:'SAR 8,600',badge:'bs',stage:'Active'}
];
document.getElementById('b2b-accounts').innerHTML = b2bAccounts.map(a =>
  `<div class="pipe-item"><div><div class="pn">${a.name}</div></div><div class="pr"><div class="pv">${a.val}</div><span class="badge ${a.badge}">${a.stage}</span></div></div>`
).join('');

const b2bSegs = [
  {l:'Hotels',pct:34,c:'#1D9E75'},{l:'Catering',pct:28,c:'#5DCAA5'},{l:'Bakeries',pct:18,c:'#EF9F27'},
  {l:'Food mfg',pct:13,c:'#534AB7'},{l:'Distributors',pct:7,c:'#888780'}
];
document.getElementById('b2b-segs').innerHTML = b2bSegs.map(s =>
  `<div class="prog-row"><div class="pl">${s.l}</div><div class="pt"><div class="pf" style="width:${s.pct}%;background:${s.c}"></div></div><div class="pp">${s.pct}%</div></div>`
).join('');

const wsOrders = [
  {name:'Almunajem Foods',val:'SAR 12,400',badge:'bs',stage:'Dispatched'},
  {name:'Khairat Foods',val:'SAR 8,200',badge:'bi',stage:'Processing'},
  {name:'Nahla Al Wadi',val:'SAR 5,600',badge:'bw',stage:'Pending'},
  {name:'Fazco Trading',val:'SAR 4,800',badge:'bs',stage:'Delivered'},
  {name:'Premier Foods',val:'SAR 3,900',badge:'bi',stage:'Processing'}
];
document.getElementById('ws-orders').innerHTML = wsOrders.map(o =>
  `<div class="pipe-item"><div><div class="pn">${o.name}</div></div><div class="pr"><div class="pv">${o.val}</div><span class="badge ${o.badge}">${o.stage}</span></div></div>`
).join('');

const wsClients = [
  {name:'Almunajem Foods',val:'20,000+ outlets reach',badge:'bs',stage:'Key account'},
  {name:'Bidfood KSA',val:'HORECA focus',badge:'bs',stage:'Key account'},
  {name:'Khairat Foods',val:'Dairy specialist',badge:'bi',stage:'Growing'},
  {name:'Fazco Trading',val:'Dry goods',badge:'bi',stage:'Growing'},
  {name:'Nahla Al Wadi',val:'Frozen & grocery',badge:'bw',stage:'New'}
];
document.getElementById('ws-clients').innerHTML = wsClients.map(c =>
  `<div class="pipe-item"><div><div class="pn">${c.name}</div><div class="pm">${c.val}</div></div><div class="pr"><span class="badge ${c.badge}">${c.stage}</span></div></div>`
).join('');

const rtStores = [
  {name:'Panda — Al Rawdah',val:'SAR 4,200',badge:'bs',stage:'Top store'},
  {name:'Danube — Corniche',val:'SAR 3,800',badge:'bs',stage:'Top store'},
  {name:'Carrefour — Mall',val:'SAR 3,500',badge:'bs',stage:'Active'},
  {name:'Lulu — Jeddah',val:'SAR 3,100',badge:'bi',stage:'Growing'},
  {name:'Othaim — North',val:'SAR 2,400',badge:'bw',stage:'Low stock'}
];
document.getElementById('rt-stores').innerHTML = rtStores.map(s =>
  `<div class="pipe-item"><div><div class="pn">${s.name}</div></div><div class="pr"><div class="pv">${s.val}</div><span class="badge ${s.badge}">${s.stage}</span></div></div>`
).join('');

const rtProds = [
  {l:'Pure butter ghee 1kg',pct:52,c:'#1D9E75'},{l:'Clarified ghee 500g',pct:30,c:'#5DCAA5'},{l:'Blended ghee 250g',pct:18,c:'#EF9F27'}
];
document.getElementById('rt-products').innerHTML = rtProds.map(p =>
  `<div class="prog-row"><div class="pl">${p.l}</div><div class="pt"><div class="pf" style="width:${p.pct}%;background:${p.c}"></div></div><div class="pp">${p.pct}%</div></div>`
).join('');

const rtMix = [{label:'Supermarkets',val:54,color:'#1D9E75'},{label:'Grocery / hyper',val:29,color:'#5DCAA5'},{label:'Online',val:17,color:'#EF9F27'}];
document.getElementById('rt-mix-leg').innerHTML = rtMix.map(m =>
  `<span style="display:flex;align-items:center;gap:5px"><span style="width:8px;height:8px;border-radius:2px;background:${m.color};flex-shrink:0"></span><span>${m.label}</span><span style="font-weight:600;color:#1a1a1a;margin-left:auto">${m.val}%</span></span>`
).join('');

// ─── CHARTS ───
const tOpts = {
  responsive:true, maintainAspectRatio:false,
  plugins:{legend:{display:false}},
  scales:{
    y:{ticks:{callback:v=>'SAR '+(v/1000).toFixed(0)+'k',font:{size:10}},grid:{color:'rgba(0,0,0,0.05)'}},
    x:{ticks:{font:{size:10}},grid:{display:false}}
  }
};

b2bChart = new Chart(document.getElementById('b2b-trend'),{type:'line',data:{labels:periods.weekly.b2b.trend.l,datasets:[{data:periods.weekly.b2b.trend.d,borderColor:'#1D9E75',backgroundColor:'rgba(29,158,117,0.07)',fill:true,tension:0.4,pointRadius:3,borderWidth:2}]},options:tOpts});
wsChart  = new Chart(document.getElementById('ws-trend'),{type:'line',data:{labels:periods.weekly.ws.trend.l,datasets:[{data:periods.weekly.ws.trend.d,borderColor:'#378ADD',backgroundColor:'rgba(55,138,221,0.07)',fill:true,tension:0.4,pointRadius:3,borderWidth:2}]},options:tOpts});
rtChart  = new Chart(document.getElementById('rt-trend'),{type:'line',data:{labels:periods.weekly.rt.trend.l,datasets:[{data:periods.weekly.rt.trend.d,borderColor:'#EF9F27',backgroundColor:'rgba(239,159,39,0.07)',fill:true,tension:0.4,pointRadius:3,borderWidth:2}]},options:tOpts});

new Chart(document.getElementById('ws-sku'),{type:'bar',data:{labels:['Pure 1kg','Pure 2kg','Clarified 500g','Blended 1kg','Bulk 5kg'],datasets:[{data:[320,215,180,140,95],backgroundColor:['#1D9E75','#5DCAA5','#378ADD','#EF9F27','#888780'],borderRadius:3,barThickness:14}]},options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{ticks:{font:{size:10}},grid:{color:'rgba(0,0,0,0.05)'}},y:{ticks:{font:{size:10}},grid:{display:false}}}}});

new Chart(document.getElementById('rt-mix'),{type:'doughnut',data:{labels:rtMix.map(m=>m.label),datasets:[{data:rtMix.map(m=>m.val),backgroundColor:rtMix.map(m=>m.color),borderWidth:0,hoverOffset:3}]},options:{responsive:true,maintainAspectRatio:false,cutout:'70%',plugins:{legend:{display:false}}}});

new Chart(document.getElementById('all-target'),{type:'bar',data:{labels:['B2B','Wholesale','Retail'],datasets:[{label:'Target',data:[150000,90000,50000],backgroundColor:'rgba(0,0,0,0.1)',borderRadius:3,barThickness:22},{label:'Actual',data:[97500,64800,38400],backgroundColor:['#1D9E75','#378ADD','#EF9F27'],borderRadius:3,barThickness:22}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{y:{ticks:{callback:v=>'SAR '+(v/1000).toFixed(0)+'k',font:{size:10}},grid:{color:'rgba(0,0,0,0.05)'}},x:{ticks:{font:{size:10}},grid:{display:false}}}}});

new Chart(document.getElementById('combined-trend'),{type:'line',data:{labels:['Jan','Feb','Mar','Apr','May','Jun'],datasets:[
  {label:'B2B',data:[62000,71000,68000,80000,88000,97500],borderColor:'#1D9E75',backgroundColor:'transparent',tension:0.4,pointRadius:2,borderWidth:2},
  {label:'Wholesale',data:[42000,48000,51000,57000,60500,64800],borderColor:'#378ADD',backgroundColor:'transparent',tension:0.4,pointRadius:2,borderWidth:2,borderDash:[4,3]},
  {label:'Retail',data:[28000,30500,32000,34800,36400,38400],borderColor:'#EF9F27',backgroundColor:'transparent',tension:0.4,pointRadius:2,borderWidth:2,borderDash:[2,2]}
]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{y:{ticks:{callback:v=>'SAR '+(v/1000).toFixed(0)+'k',font:{size:10}},grid:{color:'rgba(0,0,0,0.05)'}},x:{ticks:{font:{size:10}},grid:{display:false}}}}});
</script>
</body>
</html>
HTMLEOF
echo "Done"
