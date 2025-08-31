<!DOCTYPE html><html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
  <title>Bookkeeping Pro</title>
  <meta name="theme-color" content="#0b1220" />
  <style>
    :root{
      --bg:#0b1220;--card:#0e1730;--muted:#101b3a;--text:#e6eefc;--sub:#a7b3d1;--line:#1c2747;--accent1:#60a5fa;--accent2:#22c55e;--danger:#ef4444;
      --pill:#0f1c3d;--radius:16px;--radius-sm:12px;--shadow:0 6px 24px rgba(0,0,0,.35);
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{margin:0;background:linear-gradient(180deg,#0b1024,#0a1528);color:var(--text);font:14px/1.4 system-ui, -apple-system, Segoe UI, Roboto, Inter, Arial}
    h1,h2,h3{margin:0 0 8px}
    .sub{color:var(--sub);font-size:12px}
    .wrap{max-width:1100px;margin:0 auto;padding:14px;}
    .card{background:var(--card);border:1px solid var(--line);border-radius:var(--radius);box-shadow:var(--shadow)}
    .row{display:flex;gap:10px;align-items:center}
    .col{flex:1}
    .pill{background:var(--pill);border:1px solid var(--line);border-radius:999px;padding:4px 8px;font-size:12px;color:var(--text)}
    .btn{border:1px solid var(--line);background:rgba(255,255,255,.04);color:var(--text);padding:10px 12px;border-radius:12px;cursor:pointer}
    .btn.primary{background:linear-gradient(135deg,var(--accent1),var(--accent2));color:#06131a;border:none}
    .btn.ghost{background:transparent}
    .btn.danger{background:rgba(239,68,68,.1);border-color:#7f1d1d;color:#fecaca}
    .input, select, textarea{width:100%;background:#0b142b;border:1px solid var(--line);color:var(--text);border-radius:12px;padding:12px;outline:none}
    label{display:block;margin:4px 0 6px;color:var(--sub);font-size:12px}
    table{width:100%;border-collapse:separate;border-spacing:0;}
    thead th{background:var(--muted);position:sticky;top:0;z-index:1}
    th,td{padding:10px;border-bottom:1px solid var(--line)}
    tr:last-child td{border-bottom:none}
    .section{margin:12px 0;padding:14px}
    header{position:sticky;top:0;z-index:20;background:rgba(7,11,24,.75);backdrop-filter:blur(10px);border-bottom:1px solid var(--line)}
    header .wrap{display:flex;justify-content:space-between;align-items:center;padding:10px 14px}
    nav{position:sticky;bottom:0;background:rgba(7,11,24,.85);backdrop-filter:blur(10px);border-top:1px solid var(--line);z-index:20}
    nav .tabs{display:grid;grid-template-columns:repeat(7,1fr);gap:8px;padding:8px;max-width:700px;margin:0 auto}
    nav .tab{display:flex;flex-direction:column;align-items:center;gap:4px;padding:8px;border-radius:12px;color:var(--sub);text-decoration:none}
    nav .tab.active{background:var(--muted);color:var(--text)}
    .hide{display:none !important}
    .right{margin-left:auto}
    .chip{font-size:12px;padding:2px 6px;border-radius:6px;border:1px solid var(--line);background:#0b142b;color:var(--sub)}
    .avatar{width:28px;height:28px;border-radius:8px;background:linear-gradient(135deg,var(--accent1),var(--accent2))}
    @media(min-width:900px){ body{font-size:15px} }
  </style>
</head>
<body>
  <!-- Auth Gate -->
  <div id="authGate" style="position:fixed;inset:0;display:flex;align-items:center;justify-content:center;background:linear-gradient(180deg,#0b1024,#0a1528);z-index:50">
    <div class="card section" style="width:min(520px,92vw)">
      <div class="row" style="gap:10px">
        <div class="avatar"></div>
        <div>
          <h2>Bookkeeping Pro</h2>
          <div class="sub">Sign in to access your books</div>
        </div>
        <span class="right pill" id="syncBadge">Offline</span>
      </div>
      <div class="row" style="margin-top:10px">
        <div class="col"><label>Email</label><input id="authEmail" class="input" type="email" placeholder="you@example.com"/></div>
      </div>
      <div class="row">
        <div class="col"><label>Password</label><input id="authPass" class="input" type="password" placeholder="••••••••"/></div>
      </div>
      <div class="row" style="gap:8px;margin-top:10px">
        <button id="btnLogin" class="btn primary">Log in</button>
        <button id="btnSignup" class="btn">Sign up</button>
      </div>
      <div id="authError" class="sub" style="color:#ffb4b4;margin-top:8px;display:none">—</div>
    </div>
  </div>  <!-- App Bar -->  <header>
    <div class="wrap">
      <div class="row">
        <div class="avatar"></div>
        <strong style="margin-left:8px">Bookkeeping Pro</strong>
        <span id="companyBadge" class="pill hide" style="margin-left:8px">Company</span>
      </div>
      <div class="row">
        <span id="syncBadgeTop" class="pill">Offline</span>
        <button id="btnAccount" class="btn ghost">Account ▾</button>
      </div>
    </div>
  </header>  <!-- Company Modal -->  <div id="companyModal" class="hide" style="position:fixed;inset:0;display:flex;align-items:center;justify-content:center;background:rgba(0,0,0,.5);z-index:60">
    <div class="card section" style="width:min(520px,92vw)">
      <div class="row" style="justify-content:space-between">
        <h3>Choose Company</h3>
        <button id="closeCompany" class="btn">✕</button>
      </div>
      <div id="companyList" style="margin-top:6px"></div>
      <div class="sub" style="margin-top:10px">Select an existing company. Company creation is disabled.</div>
    </div>
  </div>  <main class="wrap" style="padding-bottom:90px">
    <!-- HOME -->
    <section id="view-home" class="section card">
      <div class="row" style="justify-content:space-between;align-items:center">
        <h2>Dashboard</h2>
        <div class="row">
          <span class="chip">AR: <span id="kpiAR">0.00</span></span>
          <span class="chip">Revenue: <span id="kpiREV">0.00</span></span>
        </div>
      </div>
      <div class="sub" style="margin-top:6px">Quick actions</div>
      <div class="row" style="flex-wrap:wrap;gap:8px;margin-top:8px">
        <button id="qaNewClient" class="btn">New Client</button>
        <button id="qaNewInvoice" class="btn">New Invoice</button>
        <button id="qaNewJE" class="btn">New Journal</button>
      </div>
      <div class="row" style="gap:12px;flex-wrap:wrap;margin-top:12px">
        <div class="card section" style="flex:1;min-width:280px">
          <h3>Income vs Expenses</h3>
          <canvas id="chartIE" height="140"></canvas>
        </div>
        <div class="card section" style="flex:1;min-width:280px">
          <h3>AR Aging</h3>
          <canvas id="chartAging" height="140"></canvas>
        </div>
      </div>
    </section><!-- CLIENTS & LEDGER -->
<section id="view-clients" class="section card hide">
  <div class="row" style="justify-content:space-between"><h2>Clients</h2>
    <div class="row"><input id="clientSearch" class="input" placeholder="Search" style="max-width:220px"/></div>
  </div>
  <div class="row" style="margin-top:10px;gap:8px">
    <input id="clientName" class="input" placeholder="Client name"/>
    <input id="clientEmail" class="input" placeholder="Email"/>
    <input id="clientPhone" class="input" placeholder="Phone"/>
    <button id="btnAddClient" class="btn primary">Add</button>
  </div>
  <div style="margin-top:8px;overflow:auto;max-height:45vh">
    <table id="clientTable">
      <thead><tr><th>Name</th><th>Email</th><th>Phone</th><th>Balance</th><th>Aging</th><th></th></tr></thead>
      <tbody></tbody>
    </table>
  </div>

  <div id="clientLedgerCard" class="card section hide" style="margin-top:12px">
    <div class="row" style="justify-content:space-between;align-items:center;gap:8px;flex-wrap:wrap">
      <h3 id="clName">Client Ledger</h3>
      <div class="row" style="gap:6px;flex-wrap:wrap">
        <input id="clFrom" class="input" type="date" style="max-width:150px"/>
        <input id="clTo" class="input" type="date" style="max-width:150px"/>
        <span id="clAging" class="row" style="gap:6px"></span>
        <span class="pill">Balance <span id="clBalance">0.00</span></span>
        <button id="btnPrintClientLedger" class="btn">Print</button>
        <button id="btnClPDF" class="btn">Export PDF</button>
        <button id="btnClXLSX" class="btn">Export Excel</button>
      </div>
    </div>
    <div style="max-height:50vh;overflow:auto;margin-top:8px">
      <table id="clientLedgerTable">
        <thead><tr><th>Date</th><th>Type</th><th>No.</th><th>Description</th><th>Debit</th><th>Credit</th><th>Balance</th></tr></thead>
        <tbody></tbody>
      </table>
    </div>
  </div>
</section>

<!-- INVOICES -->
<section id="view-invoices" class="section card hide">
  <div class="row" style="justify-content:space-between;align-items:center"><h2>Invoices</h2>
    <div class="row"><select id="invFilterClient" class="input" style="max-width:180px"></select>
      <select id="invFilterStatus" class="input" style="max-width:140px"><option value="">All</option><option>Unpaid</option><option>Partial</option><option>Paid</option></select>
      <input id="invSearch" class="input" style="max-width:160px" placeholder="Search"/></div>
  </div>
  <div class="row" style="margin-top:10px;gap:8px">
    <input id="invDate" class="input" type="date"/>
    <input id="invNo" class="input" placeholder="Invoice No."/>
    <select id="invClient" class="input" style="min-width:200px"></select>
    <input id="invDesc" class="input" placeholder="Description"/>
    <input id="invQty" class="input" type="number" min="0" step="1" placeholder="Qty" style="max-width:100px"/>
    <input id="invRate" class="input" type="number" min="0" step="0.01" placeholder="Rate" style="max-width:120px"/>
    <button id="btnSaveInvoice" class="btn primary">Save Invoice</button>
  </div>
  <div class="row" style="gap:8px;align-items:center;margin-top:6px">
    <div class="sub">Saved Invoices</div>
    <span class="right"></span>
    <button id="btnInvPDF" class="btn">Export PDF</button>
    <button id="btnInvXLSX" class="btn">Export Excel</button>
  </div>
  <div style="max-height:48vh;overflow:auto;margin-top:6px">
    <table id="invoiceTable"><thead><tr><th>Date</th><th>No.</th><th>Client</th><th>Amount</th><th>Status</th><th></th></tr></thead><tbody></tbody></table>
  </div>
</section>

<!-- JOURNAL -->
<section id="view-journal" class="section card hide">
  <div class="row" style="justify-content:space-between"><h2>Journal</h2>
    <button id="btnAddLine" class="btn">Add Line</button></div>
  <div class="row" style="gap:8px;margin-top:10px">
    <input id="jeDate" class="input" type="date"/>
    <input id="jeRef" class="input" placeholder="Ref No."/>
    <input id="jeMemo" class="input" placeholder="Memo"/>
    <span class="right"></span>
    <button id="btnJrnlPDF" class="btn">Export PDF</button>
    <button id="btnJrnlXLSX" class="btn">Export Excel</button>
  </div>
  <div id="jeLines" class="section card" style="margin-top:10px"></div>
  <div class="row" style="justify-content:flex-end;gap:8px;margin-top:10px">
    <span class="pill">Debit: <span id="jeDebit">0.00</span></span>
    <span class="pill">Credit: <span id="jeCredit">0.00</span></span>
    <button id="btnPostJE" class="btn primary">Post Entry</button>
  </div>
  <div style="max-height:40vh;overflow:auto;margin-top:8px">
    <table id="journalTable"><thead><tr><th>Date</th><th>Ref</th><th>Memo</th><th></th></tr></thead><tbody></tbody></table>
  </div>
</section>

<!-- ACCOUNTS -->
<section id="view-accounts" class="section card hide">
  <div class="row" style="justify-content:space-between;align-items:center">
    <h2>Accounts</h2>
    <div class="row" style="gap:8px">
      <input id="accSearch" class="input" placeholder="Search accounts" style="max-width:180px"/>
      <select id="accTypeFilter" class="input" style="max-width:160px">
        <option value="">All Types</option>
        <option>Asset</option>
        <option>Liability</option>
        <option>Equity</option>
        <option>Income</option>
        <option>Expense</option>
      </select>
    </div>
  </div>
  <div class="row" style="gap:8px;margin-top:10px">
    <input id="accName" class="input" placeholder="Account name"/>
    <select id="accType" class="input" style="max-width:180px">
      <option>Asset</option>
      <option>Liability</option>
      <option>Equity</option>
      <option>Income</option>
      <option>Expense</option>
    </select>
    <button id="btnAddAccount" class="btn primary">Add</button>
  </div>
  <div style="max-height:48vh;overflow:auto;margin-top:8px">
    <table id="accTable"><thead><tr><th>Name</th><th>Type</th><th></th></tr></thead><tbody></tbody></table>
  </div>
</section>

<!-- REPORTS -->
<section id="view-reports" class="section card hide">
  <div class="row" style="justify-content:space-between;align-items:center">
    <h2>Reports</h2>
    <div class="row" style="gap:8px">
      <input id="repAsOf" class="input" type="date" />
      <button id="btnRefreshReports" class="btn">Refresh</button>
      <button id="btnAgingPDF" class="btn">Aging PDF</button>
      <button id="btnAgingXLSX" class="btn">Aging Excel</button>
    </div>
  </div>
  <div class="row" style="gap:12px;flex-wrap:wrap;margin-top:10px">
    <div class="card section" style="flex:1;min-width:280px">
      <h3>Balance Sheet</h3>
      <div id="bsHTML" class="sub" style="margin-top:6px"></div>
    </div>
    <div class="card section" style="flex:1;min-width:280px">
      <h3>Profit & Loss</h3>
      <div id="plHTML" class="sub" style="margin-top:6px"></div>
    </div>
    <div class="card section" style="flex:1;min-width:280px">
      <h3>Invoice Aging</h3>
      <div style="max-height:42vh;overflow:auto;margin-top:6px">
        <table id="agingTable">
          <thead><tr><th>Client</th><th>Invoice No</th><th>Date</th><th>Due</th><th>0-30</th><th>31-60</th><th>61-90</th><th>90+</th><th>Status</th></tr></thead>
          <tbody></tbody>
        </table>
      </div>
    </div>
  </div>
</section>

<!-- SETTINGS -->
<section id="view-settings" class="section card hide">
  <h2>Settings</h2>
  <div class="row" style="gap:8px;margin-top:10px">
    <input id="setCurrency" class="input" placeholder="Currency (e.g., USD)"/>
    <input id="setAR" class="input" placeholder="AR Account Name"/>
    <input id="setREV" class="input" placeholder="Revenue Account Name"/>
    <input id="setCASH" class="input" placeholder="Cash Account Name"/>
    <button id="btnSaveSettings" class="btn primary">Save</button>
    <span class="right"></span>
    <button id="btnBackupNow" class="btn">Backup Now</button>
  </div>
</section>
  </div>
</section>

  </main>  <!-- Tabs -->  <nav>
    <div class="tabs">
      <a data-view="home" class="tab active">🏠<span class="sub">Home</span></a>
      <a data-view="invoices" class="tab">🧾<span class="sub">Invoices</span></a>
      <a data-view="journal" class="tab">📒<span class="sub">Journal</span></a>
      <a data-view="clients" class="tab">👤<span class="sub">Clients</span></a>
      <a data-view="accounts" class="tab">🏷️<span class="sub">Accounts</span></a>
      <a data-view="reports" class="tab">📊<span class="sub">Reports</span></a>
      <a data-view="settings" class="tab">⚙️<span class="sub">Settings</span></a>
    </div>
  </nav>  <!-- Chart.js -->  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>  <script>
    // Utilities
    const $ = s=>document.querySelector(s); const $$=s=>Array.from(document.querySelectorAll(s));
    const fmt = n => (Number(n||0)).toFixed(2);

    // App state (online-only mirror; source of truth is Firebase)
    window.DB = { clients: [], invoices: [], journal: [], accounts: [], settings: { arAccount:'Accounts Receivable', revAccount:'Sales Revenue', cashAccount:'Cash' }, currency:'USD' };

    // Navigation
    window.currentRoute='home';
    function show(view){
      $$('section[id^=view-]').forEach(s=> s.classList.add('hide'));
      const target = document.getElementById('view-'+view);
      if(target) target.classList.remove('hide');
      $$('.tab').forEach(t=> t.classList.toggle('active', t.dataset.view===view));
      window.currentRoute=view;
      if(view==='clients') renderClients();
      if(view==='invoices') renderInvoices();
      if(view==='journal') renderJournal();
      if(view==='home'){ renderKPIs(); renderDashboardCharts(); }
      if(view==='accounts') renderAccounts();
      if(view==='reports') renderReports();
    }
    $$('.tab').forEach(t=> t.addEventListener('click', ()=> show(t.dataset.view)));

    // ===== Accounts =====
    function accountOptionsHTML(){
      if(!DB.accounts || DB.accounts.length===0) return `<option value=''>No accounts</option>`;
      return DB.accounts.map(a=> `<option value='${a.name}'>${a.name} (${a.type})</option>`).join('');
    }
    function renderAccounts(){
      const q = ($('#accSearch')?.value||'').toLowerCase();
      const t = $('#accTypeFilter')?.value||'';
      const items = (DB.accounts||[])
        .filter(a=>!t || a.type===t)
        .filter(a=> (a.name||'').toLowerCase().includes(q));
      const rows = items.map(a=> `<tr>
        <td>${a.name}</td>
        <td><span class='pill'>${a.type}</span></td>
        <td>
          <button class='btn ghost' data-act='rename' data-id='${a.id}'>Rename</button>
          <button class='btn danger' data-act='del' data-id='${a.id}'>Delete</button>
        </td>
      </tr>`).join('');
      const tbody = document.querySelector('#accTable tbody');
      if(tbody) tbody.innerHTML = rows || `<tr><td colspan='3'>No accounts.</td></tr>`;
      refreshJournalAccountDropdowns();
    }
    document.addEventListener('input', (e)=>{
      if(e.target && (e.target.id==='accSearch' || e.target.id==='accTypeFilter')) renderAccounts();
    });
    document.addEventListener('click', (e)=>{
      if(e.target && e.target.id==='btnAddAccount'){
        const name = ($('#accName').value||'').trim();
        const type = $('#accType').value||'Asset';
        if(!name) return alert('Enter an account name');
        DB.accounts.push({ id: crypto.randomUUID(), name, type });
        $('#accName').value=''; save(); return;
      }
      const btn = e.target.closest && e.target.closest('#accTable button[data-act]');
      if(!btn) return;
      const id = btn.getAttribute('data-id');
      if(btn.dataset.act==='del'){
        if(!confirm('Delete this account?')) return;
        DB.accounts = DB.accounts.filter(a=>a.id!==id); save(); return;
      }
      if(btn.dataset.act==='rename'){
        const acc = DB.accounts.find(a=>a.id===id); if(!acc) return;
        const name = prompt('New account name', acc.name); if(!name) return;
        acc.name = name.trim(); save(); return;
      }
    });
    function refreshJournalAccountDropdowns(){
      $$('#jeLines select.accSel').forEach(sel=>{
        const val = sel.value;
        sel.innerHTML = `<option value=''>Select account</option>` + accountOptionsHTML();
        if(val) sel.value = val;
      });
    }

    // ===== Clients =====
    $('#btnAddClient').addEventListener('click', ()=>{
      const name=$('#clientName').value.trim(); if(!name) return;
      DB.clients.push({ id: crypto.randomUUID(), name, email:$('#clientEmail').value.trim(), phone:$('#clientPhone').value.trim() });
      $('#clientName').value=$('#clientEmail').value=$('#clientPhone').value='';
      save();
    });
    $('#clientSearch').addEventListener('input', renderClients);

    function clientAging(clientId){
      const invs = DB.invoices.filter(i=>i.clientId===clientId && (i.status!=='Paid'));
      const now = new Date();
      let current=0,d30=0,d60=0,d90=0;
      for(const i of invs){
        const d = new Date(i.date || now).getTime();
        const days = Math.floor((now - d)/86400000);
        const bal = (i.amount||0) - (i.paid||0);
        if(days<=30) current+=bal; else if(days<=60) d30+=bal; else if(days<=90) d60+=bal; else d90+=bal;
      }
      return {current,d30,d60,d90};
    }
    function calcClientBalance(clientId){
      const invs = DB.invoices.filter(i=>i.clientId===clientId);
      return invs.reduce((s,i)=> s + (i.amount||0) - (i.paid||0), 0);
    }
    function renderClients(){
      // populate selects for invoices
      const cs=$('#invClient'); if(cs){ cs.innerHTML = `<option value=''>Select client</option>` + DB.clients.map(c=>`<option value="${c.id}">${c.name}</option>`).join(''); }
      $('#invFilterClient').innerHTML = `<option value=''>All</option>` + DB.clients.map(c=>`<option value='${c.id}'>${c.name}</option>`).join('');
      // table
      const q = ($('#clientSearch').value||'').toLowerCase();
      const rows = DB.clients.filter(c=> (c.name+c.email+c.phone).toLowerCase().includes(q)).map(c=>{
        const age=clientAging(c.id);
        const agingHTML = `<span class='pill'>C ${fmt(age.current)}</span> <span class='pill'>30 ${fmt(age.d30)}</span> <span class='pill'>60 ${fmt(age.d60)}</span> <span class='pill'>90+ ${fmt(age.d90)}</span>`;
        return `<tr>
          <td>${c.name}</td><td>${c.email||''}</td><td>${c.phone||''}</td>
          <td>${fmt(calcClientBalance(c.id))}</td>
          <td>${agingHTML}</td>
          <td><button class='btn' data-act='ledger' data-id='${c.id}'>Ledger</button></td>
        </tr>`;
      }).join('');
      $('#clientTable tbody').innerHTML = rows || `<tr><td colspan='6'>No clients yet.</td></tr>`;
    }

    // Client Ledger
    $('#clientTable').addEventListener('click', (e)=>{
      const b=e.target.closest('button[data-act="ledger"]'); if(!b) return; renderClientLedger(b.dataset.id); $('#clientLedgerCard').classList.remove('hide');
    });
    window.__currentClientId = null;
    function renderClientLedger(clientId){
      window.__currentClientId = clientId;
      const c = DB.clients.find(x=>x.id===clientId); if(!c) return;
      $('#clName').textContent = c.name;
      $('#clBalance').textContent = fmt(calcClientBalance(clientId));
      const age=clientAging(clientId);
      $('#clAging').innerHTML = `<span class='pill'>Current ${fmt(age.current)}</span><span class='pill'>30 ${fmt(age.d30)}</span><span class='pill'>60 ${fmt(age.d60)}</span><span class='pill'>90+ ${fmt(age.d90)}</span>`;

      const entries=[];
      for(const inv of DB.invoices.filter(i=>i.clientId===clientId)){
        entries.push({date:inv.date, type:'Invoice', no:inv.number||'', desc:inv.desc||'', debit:inv.amount||0, credit:0});
        if(inv.paid){ entries.push({date:inv.date, type:'Payment', no:inv.number||'', desc:'Payment received', debit:0, credit:inv.paid}); }
      }
      for(const je of DB.journal){
        for(const ln of (je.lines||[])){
          if(ln.clientId===clientId){ entries.push({date:je.date, type:'Journal', no:je.ref||'', desc:je.memo||'', debit:ln.debit||0, credit:ln.credit||0}); }
        }
      }
      entries.sort((a,b)=> (a.date||'').localeCompare(b.date));
      let run=0; const rows = entries.map(e=>{ run += (e.debit||0)-(e.credit||0); return `<tr><td>${e.date||''}</td><td>${e.type}</td><td>${e.no}</td><td>${e.desc}</td><td>${e.debit?fmt(e.debit):''}</td><td>${e.credit?fmt(e.credit):''}</td><td>${fmt(run)}</td></tr>`; }).join('');
      $('#clientLedgerTable tbody').innerHTML = rows || `<tr><td colspan='7'>No activity yet.</td></tr>`;
    }

    $('#btnPrintClientLedger').addEventListener('click', ()=>{
      const html = `<!doctype html><html><head><meta charset='utf-8'><title>Client Ledger</title>
        <style>body{font:13px Arial;color:#111} h2{margin:0 0 8px} table{border-collapse:collapse;width:100%} th,td{border:1px solid #ccc;padding:6px}</style></head><body>
    <h2>${$('#clName').textContent} — Client Ledger</h2>
    ${$('#clientLedgerTable').outerHTML}
  </body></html>`;
  const w=window.open('', '_blank'); w.document.write(html); w.document.close(); w.print();
});

// ===== Invoices =====
function renderInvoices(){
  $('#invClient').innerHTML = `<option value=''>Select client</option>` + DB.clients.map(c=>`<option value='${c.id}'>${c.name}</option>`).join('');
  $('#invFilterClient').innerHTML = `<option value=''>All</option>` + DB.clients.map(c=>`<option value='${c.id}'>${c.name}</option>`).join('');
  const fC=$('#invFilterClient').value||''; const fS=$('#invFilterStatus').value||''; const q=($('#invSearch').value||'').toLowerCase();
  const rows = DB.invoices
    .filter(i=>!fC||i.clientId===fC)
    .filter(i=>!fS||i.status===fS)
    .filter(i=> ((i.number||'')+ (DB.clients.find(c=>c.id===i.clientId)?.name||'') + (i.desc||'')).toLowerCase().includes(q))
    .sort((a,b)=> (a.date||'').localeCompare(b.date))
    .map(i=>{
      const client = DB.clients.find(c=>c.id===i.clientId);
      return `<tr>
        <td>${i.date||''}</td>
        <td>${i.number||''}</td>
        <td>${client?client.name:'—'}</td>
        <td>${fmt(i.amount||0)}</td>
        <td><span class='pill'>${i.status||'Unpaid'}</span></td>
        <td>
          <button class='btn' data-act='pay' data-id='${i.id}'>Record Payment</button>
          <button class='btn ghost' data-act='del' data-id='${i.id}'>Delete</button>
        </td>
      </tr>`;
    }).join('');
  $('#invoiceTable tbody').innerHTML = rows || `<tr><td colspan='6'>No invoices yet.</td></tr>`;
}
;['change','input'].forEach(ev=>['#invFilterClient','#invFilterStatus','#invSearch'].forEach(sel=> $(sel).addEventListener(ev, renderInvoices)));

$('#btnSaveInvoice').addEventListener('click', ()=>{
  const id = crypto.randomUUID();
  const qty = Number($('#invQty').value||0); const rate = Number($('#invRate').value||0);
  const amount = qty*rate;
  const clientId = $('#invClient').value;
  if(!clientId) return alert('Select a client');
  const date = $('#invDate').value||new Date().toISOString().slice(0,10);
  const number = $('#invNo').value||('INV-'+String(DB.invoices.length+1).padStart(4,'0'));
  const desc = $('#invDesc').value;
  DB.invoices.push({ id, date, number, clientId, desc, qty, rate, amount, status:'Unpaid', paid:0 });
  // Auto Journal: DR A/R, CR Revenue
  DB.journal.push({ id:crypto.randomUUID(), date, ref:number, memo:`Invoice ${number} — ${desc||''}`,
    lines:[ { accountName:DB.settings.arAccount, debit:amount, credit:0, clientId }, { accountName:DB.settings.revAccount, debit:0, credit:amount } ] });
  $('#invQty').value=$('#invRate').value=$('#invDesc').value='';
  save();
});

$('#invoiceTable').addEventListener('click', (e)=>{
  const b=e.target.closest('button[data-act]'); if(!b) return; const id=b.dataset.id;
  const inv = DB.invoices.find(x=>x.id===id); if(!inv) return;
  if(b.dataset.act==='del'){ DB.invoices = DB.invoices.filter(x=>x.id!==id); save(); return; }
  if(b.dataset.act==='pay'){
    const due = (inv.amount||0)-(inv.paid||0);
    const amt = Number(prompt('Payment amount', String(due))||0);
    if(!amt) return;
    inv.paid = (inv.paid||0) + amt; inv.status = inv.paid>=inv.amount? 'Paid' : 'Partial';
    DB.journal.push({ id:crypto.randomUUID(), date:new Date().toISOString().slice(0,10), ref:inv.number, memo:`Payment for ${inv.number}`,
      lines:[ { accountName:DB.settings.cashAccount, debit:amt, credit:0 }, { accountName:DB.settings.arAccount, debit:0, credit:amt, clientId:inv.clientId } ] });
    save();
  }
});

// ===== Journal =====
function renderJournal(){
  const tbody=$('#journalTable tbody');
  tbody.innerHTML = DB.journal.map(j=> `<tr><td>${j.date||''}</td><td>${j.ref||''}</td><td>${j.memo||''}</td><td><button class='btn danger' data-id='${j.id}'>Delete</button></td></tr>`).join('') || `<tr><td colspan='4'>No entries.</td></tr>`;
  const wrap = $('#jeLines'); wrap.innerHTML = '';
  const line = ()=> `<div class='row' style='gap:8px;margin:6px 0'>
    <select class='input accSel'><option value=''>Select account</option>${accountOptionsHTML()}</select>
    <select class='input clientSel'><option value=''>No client</option>${DB.clients.map(c=>`<option value='${c.id}'>${c.name}</option>`).join('')}</select>
    <input class='input debit' type='number' step='0.01' placeholder='Debit' style='max-width:120px'/>
    <input class='input credit' type='number' step='0.01' placeholder='Credit' style='max-width:120px'/>
    <button class='btn danger btnDelLine'>✕</button>
  </div>`;
  wrap.insertAdjacentHTML('beforeend', line());
  updateTotals();
  wrap.addEventListener('click', (e)=>{ if(e.target.classList.contains('btnDelLine')){ e.target.parentElement.remove(); updateTotals(); } });
  wrap.addEventListener('input', updateTotals);
  function updateTotals(){ let d=0,c=0; $$('#jeLines .debit').forEach(x=> d+=Number(x.value||0)); $$('#jeLines .credit').forEach(x=> c+=Number(x.value||0)); $('#jeDebit').textContent = fmt(d); $('#jeCredit').textContent = fmt(c); }
}
$('#btnAddLine').addEventListener('click', ()=>{
  const wrap=$('#jeLines');
  wrap.insertAdjacentHTML('beforeend', `<div class='row' style='gap:8px;margin:6px 0'>
    <select class='input accSel'><option value=''>Select account</option>${accountOptionsHTML()}</select>
    <select class='input clientSel'><option value=''>No client</option>${DB.clients.map(c=>`<option value='${c.id}'>${c.name}</option>`).join('')}</select>
    <input class='input debit' type='number' step='0.01' placeholder='Debit' style='max-width:120px'/>
    <input class='input credit' type='number' step='0.01' placeholder='Credit' style='max-width:120px'/>
    <button class='btn danger btnDelLine'>✕</button>
  </div>`);
});

$('#btnPostJE').addEventListener('click', ()=>{
  if(!DB.accounts || DB.accounts.length===0){ return alert('No accounts yet. Add accounts first.'); }
  const lines=[]; $$('#jeLines .row').forEach(r=>{ lines.push({ accountName:r.querySelector('.accSel').value.trim(), debit:Number(r.querySelector('.debit').value||0), credit:Number(r.querySelector('.credit').value||0), clientId:r.querySelector('.clientSel').value||undefined }); });
  const d=Number($('#jeDebit').textContent), c=Number($('#jeCredit').textContent);
  if(!(d>0 && d===c)) return alert('Debits and credits must balance');
  if(lines.some(l=>!l.accountName)) return alert('Please choose an account for every line.');
  DB.journal.push({ id:crypto.randomUUID(), date:$('#jeDate').value||new Date().toISOString().slice(0,10), ref:$('#jeRef').value, memo:$('#jeMemo').value, lines });
  save();
});

$('#journalTable').addEventListener('click', (e)=>{
  const b=e.target.closest('button[data-id]'); if(!b) return; const id=b.dataset.id; DB.journal = DB.journal.filter(j=>j.id!==id); save();
});

// ===== Settings & KPIs =====
function renderKPIs(){
  const ar = DB.invoices.reduce((s,i)=> s + (i.amount||0)-(i.paid||0), 0);
  const rev = DB.invoices.reduce((s,i)=> s + (i.paid||0), 0);
  document.getElementById('kpiAR').textContent = fmt(ar);
  document.getElementById('kpiREV').textContent = fmt(rev);
}
let chartIE=null, chartAging=null;
function monthKeys(n){ const d=new Date(); const arr=[]; for(let i=n-1;i>=0;i--){ const t=new Date(d.getFullYear(), d.getMonth()-i, 1); arr.push(`${t.getFullYear()}-${String(t.getMonth()+1).padStart(2,'0')}`);} return arr; }
function renderDashboardCharts(){ try{
  const labels = monthKeys(6);
  const byMonth = (dateStr)=> (dateStr||'').slice(0,7);
  const accType = (name)=> (DB.accounts.find(a=>a.name===name)||{}).type||'';
  // Income: paid invoices month
  const incomeMap = Object.fromEntries(labels.map(k=>[k,0]));
  for(const i of DB.invoices){ const m=byMonth(i.date); if(labels.includes(m)) incomeMap[m] += Number(i.paid||0); }
  // Expenses: sum JE lines where account type Expense
  const expenseMap = Object.fromEntries(labels.map(k=>[k,0]));
  for(const je of DB.journal){ const m=byMonth(je.date); if(!labels.includes(m)) continue; for(const ln of (je.lines||[])){ if(accType(ln.accountName)==='Expense'){ expenseMap[m] += Number(ln.debit||0) - Number(ln.credit||0); } } }
  const incomeData = labels.map(k=> incomeMap[k]);
  const expenseData = labels.map(k=> expenseMap[k]);
  const ctx1=document.getElementById('chartIE'); if(ctx1){ if(chartIE) chartIE.destroy(); chartIE=new Chart(ctx1,{ type:'line', data:{ labels, datasets:[{ label:'Income', data:incomeData }, { label:'Expenses', data:expenseData }] }, options:{ responsive:true, maintainAspectRatio:false } }); }
  // AR Aging totals
  let cur=0,d30=0,d60=0,d90=0; const now=new Date();
  for(const i of DB.invoices.filter(x=> (x.amount||0)>(x.paid||0))){ const d=new Date(i.date||now).getTime(); const days=Math.floor((now-d)/86400000); const bal=(i.amount||0)-(i.paid||0); if(days<=30) cur+=bal; else if(days<=60) d30+=bal; else if(days<=90) d60+=bal; else d90+=bal; }
  const ctx2=document.getElementById('chartAging'); if(ctx2){ if(chartAging) chartAging.destroy(); chartAging=new Chart(ctx2,{ type:'bar', data:{ labels:['Current','31-60','61-90','90+'], datasets:[{ label:'A/R', data:[cur,d30,d60,d90] }] }, options:{ responsive:true, maintainAspectRatio:false } }); }
}catch(e){} }
$('#btnSaveSettings').addEventListener('click', ()=>{
  DB.currency = $('#setCurrency').value || DB.currency;
  DB.settings.arAccount = $('#setAR').value || DB.settings.arAccount;
  DB.settings.revAccount = $('#setREV').value || DB.settings.revAccount;
  DB.settings.cashAccount = $('#setCASH').value || DB.settings.cashAccount;
  save();
});
// QA shortcuts
$('#qaNewClient').addEventListener('click', ()=> show('clients'));
$('#qaNewInvoice').addEventListener('click', ()=> show('invoices'));
$('#qaNewJE').addEventListener('click', ()=> show('journal'));

// ===== Reports =====
function computeTrialBalance(asOf){
  const byAcc = new Map();
  const push = (name, type, debit, credit)=>{
    if(!name) return; const key=name; if(!byAcc.has(key)) byAcc.set(key,{ name, type, debit:0, credit:0 });
    const row = byAcc.get(key); row.debit += Number(debit||0); row.credit += Number(credit||0);
  };
  // From Journal
  for(const je of DB.journal){ if(asOf && je.date>asOf) continue; for(const ln of (je.lines||[])){ const acc = DB.accounts.find(a=>a.name===ln.accountName); push(ln.accountName, acc?.type||'Asset', ln.debit, ln.credit); } }
  return Array.from(byAcc.values());
}
function renderReports(){
  const asOf = $('#repAsOf').value || '9999-12-31';
  const tb = computeTrialBalance(asOf);
  const sum = (f)=> tb.filter(f).reduce((s,r)=> s + (r.debit - r.credit), 0);
  const sumCreditSide = (f)=> tb.filter(f).reduce((s,r)=> s + (r.credit - r.debit), 0);
  const assets = sum(r=> r.type==='Asset');
  const liab = sumCreditSide(r=> r.type==='Liability');
  const equity = sumCreditSide(r=> r.type==='Equity');
  const income = sumCreditSide(r=> r.type==='Income');
  const expense = sum(r=> r.type==='Expense');
  const netInc = income - expense;
  const retained = netInc; // current period earnings
  const bsHTML = `
    <div>Assets: <strong>${fmt(assets)}</strong></div>
    <div>Liabilities: <strong>${fmt(liab)}</strong></div>
    <div>Equity (excl. Retained): <strong>${fmt(equity)}</strong></div>
    <div>Retained Earnings (current): <strong>${fmt(retained)}</strong></div>
    <hr/>
    <div>Total Liab + Equity: <strong>${fmt(liab + equity + retained)}</strong></div>`;
  const plHTML = `
    <div>Income: <strong>${fmt(income)}</strong></div>
    <div>Expenses: <strong>${fmt(expense)}</strong></div>
    <hr/>
    <div>Net Income: <strong>${fmt(netInc)}</strong></div>`;
  $('#bsHTML').innerHTML = bsHTML; $('#plHTML').innerHTML = plHTML;
}
$('#btnRefreshReports').addEventListener('click', renderReports);

// Save hook (patched by Firebase module below)
window.save = function(){ /* replaced by Firebase sync */ };

  </script>  <!-- Export libs -->  <script src="https://cdn.jsdelivr.net/npm/jspdf@2.5.1/dist/jspdf.umd.min.js"></script>  <script src="https://cdn.jsdelivr.net/npm/jspdf-autotable@3.8.2/dist/jspdf.plugin.autotable.min.js"></script>  <script src="https://cdn.jsdelivr.net/npm/xlsx@0.19.3/dist/xlsx.full.min.js"></script>  <!-- Firebase via CDN modules (online-only) -->  <script type="module">
    import { initializeApp } from 'https://www.gstatic.com/firebasejs/10.12.3/firebase-app.js';
    import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut } from 'https://www.gstatic.com/firebasejs/10.12.3/firebase-auth.js';
    import { getDatabase, ref, onValue, set, update, push, get, serverTimestamp } from 'https://www.gstatic.com/firebasejs/10.12.3/firebase-database.js';

    // ==== Firebase Config ====
    const firebaseConfig = {
      apiKey: 'AIzaSyB0GBOkpFyLIp8RJDARHEgb_W8q1W40v40',
      authDomain: 'lizardo-sea-oil.firebaseapp.com',
      databaseURL: 'https://lizardo-sea-oil-default-rtdb.firebaseio.com',
      projectId: 'lizardo-sea-oil',
      storageBucket: 'lizardo-sea-oil.firebasestorage.app',
      messagingSenderId: '594882681824',
      appId: '1:594882681824:web:6d70cc3c27be2946632985',
      measurementId: 'G-QW08V2PW52'
    };

    const app = initializeApp(firebaseConfig);
    const auth = getAuth(app);
    const db = getDatabase(app);

    // ==== Offline-first helpers ====
    const LS_KEY='bkpro.cache';
    const QUEUE_KEY='bkpro.queue';
    function loadLocal(){ try{ const j=JSON.parse(localStorage.getItem(LS_KEY)||'null'); if(j&&typeof j==='object'){ Object.assign(DB,j); } }catch(_){} }
    function persistLocal(){ try{ localStorage.setItem(LS_KEY, JSON.stringify(DB)); }catch(_){} }
    function enqueueSync(payload){ try{ const q=JSON.parse(localStorage.getItem(QUEUE_KEY)||'[]'); q.push({t:Date.now(), payload}); localStorage.setItem(QUEUE_KEY, JSON.stringify(q)); }catch(_){} }
    async function flushQueue(){ if(!currentUser||!currentCompany||!navigator.onLine) return; try{ const q=JSON.parse(localStorage.getItem(QUEUE_KEY)||'[]'); if(!Array.isArray(q)||q.length===0) return; setSync(true); const base=`users/${currentUser.uid}/data/${currentCompany}`; for(const item of q){ await update(ref(db, base), item.payload).catch(()=>{}); } localStorage.setItem(QUEUE_KEY,'[]'); }finally{ setSync(false);} }

    const syncBadges=[document.getElementById('syncBadge'), document.getElementById('syncBadgeTop')].filter(Boolean);
    const setSync = (state)=> syncBadges.forEach(b=> b.textContent = state? 'Syncing…' : (navigator.onLine? 'Online':'Offline'));
    window.addEventListener('online', ()=> { setSync(false); flushQueue(); });
    window.addEventListener('offline', ()=> setSync(false));

    let currentUser=null, currentCompany=null; let unsub=[];
    function showAuthGate(show){ document.getElementById('authGate').classList.toggle('hide', !show); }
    function showCompanyModal(show){ document.getElementById('companyModal').classList.toggle('hide', !show); }

    // Load local cache at boot
    loadLocal(); renderKPIs(); renderDashboardCharts(); renderAccounts(); renderInvoices(); renderJournal(); renderReports();

    // ==== Auth UI ====
    document.getElementById('btnLogin').addEventListener('click', async ()=>{
      const email=document.getElementById('authEmail').value.trim();
      const pass=document.getElementById('authPass').value;
      try{ setSync(true); await signInWithEmailAndPassword(auth,email,pass); document.getElementById('authError').style.display='none'; }
      catch(e){ const el=document.getElementById('authError'); el.textContent=e.message; el.style.display='block'; setSync(false);} 
    });
    document.getElementById('btnSignup').addEventListener('click', async ()=>{
      const email=document.getElementById('authEmail').value.trim();
      const pass=document.getElementById('authPass').value;
      try{ setSync(true); await createUserWithEmailAndPassword(auth,email,pass); document.getElementById('authError').style.display='none'; }
      catch(e){ const el=document.getElementById('authError'); el.textContent=e.message; el.style.display='block'; setSync(false);} 
    });

    document.getElementById('btnAccount').addEventListener('click', ()=>{
      const m=document.createElement('div'); m.style.position='fixed'; m.style.right='10px'; m.style.top='58px'; m.style.zIndex='80';
      m.innerHTML = `<div class='card section' style='padding:8px;min-width:220px'>
        <div class='sub'>${currentUser? currentUser.email : 'Signed out'}</div>
        <div class='row' style='gap:6px;margin-top:8px'>
          <button class='btn' id='btnChooseCompany'>Companies</button>
          <button class='btn danger' id='btnLogout'>Logout</button>
        </div>
      </div>`;
      document.body.appendChild(m);
      function cleanup(ev){ if(!m.contains(ev.target) && ev.target!==document.getElementById('btnAccount')){ m.remove(); document.removeEventListener('click', cleanup);} }
      setTimeout(()=>document.addEventListener('click', cleanup), 0);
      m.querySelector('#btnLogout').addEventListener('click', async ()=>{ await signOut(auth); m.remove(); });
      m.querySelector('#btnChooseCompany').addEventListener('click', ()=>{ showCompanyModal(true); m.remove(); });
    });

    // ==== Companies (selection only; creation disabled) ====
    document.getElementById('closeCompany').addEventListener('click', ()=> showCompanyModal(false));
    document.getElementById('companyList').addEventListener('click', async (e)=>{
      const btn=e.target.closest('button[data-id]'); if(!btn) return; const id=btn.getAttribute('data-id');
      if(btn.dataset.act==='switch'){ await selectCompany(id); showCompanyModal(false); }
      if(btn.dataset.act==='rename'){ const name=prompt('New company name'); if(name) await update(ref(db,`users/${currentUser.uid}/companies/${id}`),{name}); }
    });

    async function selectCompany(companyId){
      currentCompany = companyId; localStorage.setItem('bkpro.company', companyId);
      const badge=document.getElementById('companyBadge'); badge.classList.remove('hide');
      onValue(ref(db,`users/${currentUser.uid}/companies/${companyId}`), s=> badge.textContent = s.val()?.name||'Company');
      bindDataListeners();
      await seedAccountsIfEmpty();
    }

    // ==== Seed default chart of accounts if empty ====
    const DEFAULT_ACCOUNTS = [
      { id: crypto.randomUUID(), name: 'Cash', type: 'Asset' },
      { id: crypto.randomUUID(), name: 'Accounts Receivable', type: 'Asset' },
      { id: crypto.randomUUID(), name: 'Inventory', type: 'Asset' },
      { id: crypto.randomUUID(), name: 'Prepaid Expenses', type: 'Asset' },
      { id: crypto.randomUUID(), name: 'Accounts Payable', type: 'Liability' },
      { id: crypto.randomUUID(), name: 'Loans Payable', type: 'Liability' },
      { id: crypto.randomUUID(), name: "Owner's Capital", type: 'Equity' },
      { id: crypto.randomUUID(), name: 'Retained Earnings', type: 'Equity' },
      { id: crypto.randomUUID(), name: 'Sales Revenue', type: 'Income' },
      { id: crypto.randomUUID(), name: 'Service Revenue', type: 'Income' },
      { id: crypto.randomUUID(), name: 'Salaries Expense', type: 'Expense' },
      { id: crypto.randomUUID(), name: 'Rent Expense', type: 'Expense' },
      { id: crypto.randomUUID(), name: 'Utilities Expense', type: 'Expense' },
      { id: crypto.randomUUID(), name: 'Office Supplies', type: 'Expense' }
    ];

    async function seedAccountsIfEmpty(){
      if(!currentUser || !currentCompany) return;
      const base = `users/${currentUser.uid}/data/${currentCompany}/accounts`;
      try{
        const snap = await get(ref(db, base));
        const val = snap.val();
        const isEmpty = !val || (Array.isArray(val) && val.length===0) || (typeof val==='object' && Object.keys(val).length===0);
        if(isEmpty){ await set(ref(db, base), DEFAULT_ACCOUNTS); }
      }catch(e){ /* ignore */ }
    }

    // ==== Apply snapshots to UI ====
    function apply(kind, data){
      const normalize = (d)=> Array.isArray(d)? d : (d? Object.values(d) : []);
      if(kind==='clients') DB.clients = normalize(data);
      if(kind==='invoices') DB.invoices = normalize(data);
      if(kind==='journal') DB.journal = normalize(data);
      if(kind==='accounts') DB.accounts = normalize(data).filter(Boolean);
      if(kind==='settings') DB.settings = Object.assign(DB.settings, data||{});
      if(window.currentRoute==='clients') renderClients();
      if(window.currentRoute==='invoices') renderInvoices();
      if(window.currentRoute==='journal') renderJournal();
      if(window.currentRoute==='accounts') renderAccounts();
      if(window.currentRoute==='reports') renderReports();
      renderKPIs(); renderDashboardCharts(); if(window.__currentClientId) renderClientLedger(window.__currentClientId);
    }

    function bindDataListeners(){
      if(!currentUser||!currentCompany) return; unsub.forEach(u=>u&&u()); unsub=[]; const base=`users/${currentUser.uid}/data/${currentCompany}`;
      unsub.push(onValue(ref(db,`${base}/clients`), s=> apply('clients', s.val())));
      unsub.push(onValue(ref(db,`${base}/invoices`), s=> apply('invoices', s.val())));
      unsub.push(onValue(ref(db,`${base}/journal`), s=> apply('journal', s.val())));
      unsub.push(onValue(ref(db,`${base}/accounts`), s=> apply('accounts', s.val())));
      unsub.push(onValue(ref(db,`${base}/settings`), s=> apply('settings', s.val())));
    }

    // ==== Global save (simple mirror) ====
    window.save = function(){
      persistLocal();
      if(!currentUser||!currentCompany||!navigator.onLine){
        enqueueSync({ clients:DB.clients||[], invoices:DB.invoices||[], journal:DB.journal||[], accounts:DB.accounts||[], settings:DB.settings||{} });
        return;
      }
      setSync(true);
      const base=`users/${currentUser.uid}/data/${currentCompany}`;
      Promise.all([
        set(ref(db,`${base}/clients`), DB.clients||[]),
        set(ref(db,`${base}/invoices`), DB.invoices||[]),
        set(ref(db,`${base}/journal`), DB.journal||[]),
        set(ref(db,`${base}/accounts`), DB.accounts||[]),
        set(ref(db,`${base}/settings`), DB.settings||{})
      ]).finally(()=> setSync(false));
    };

    // ==== Auth state changes ====
    onAuthStateChanged(auth, (user)=>{
      currentUser = user || null;
      if(!currentUser){
        showAuthGate(true); document.getElementById('companyBadge').classList.add('hide'); unsub.forEach(u=>u&&u()); unsub=[]; return;
      }
      showAuthGate(false);
      onValue(ref(db, `users/${currentUser.uid}/companies`), (snap)=>{
        const list=snap.val()||{}; const wrap=document.getElementById('companyList');
        wrap.innerHTML = (Object.keys(list).length? Object.entries(list).map(([id,c])=>`<div class='row' style='justify-content:space-between;margin:8px 0'><div><strong>${c.name||'Company'}</strong></div><div class='right'><button class='btn' data-id='${id}' data-act='switch'>Open</button><button class='btn ghost' data-id='${id}' data-act='rename'>Rename</button></div></div>`).join('') : `<div class='sub'>No companies available. Company creation is disabled.</div>`);
        const stored = localStorage.getItem('bkpro.company'); const first = Object.keys(list)[0]; if(!currentCompany && (stored||first)) selectCompany(stored||first);
        flushQueue();
      });
    });
  </script></body>
</html>
