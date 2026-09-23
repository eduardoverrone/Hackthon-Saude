# Hackthon-Saude
!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MediFila • Cadastro de Pacientes</title>
<style>
  :root{
    --primary:#0ea5e9; --primary-dark:#0284c7; --primary-light:#e0f2fe;
    --accent:#14b8a6; --dark:#0f172a; --gray:#64748b; --light:#f8fafc;
    --border:#e2e8f0; --success:#10b981; --danger:#ef4444;
    --radius:14px; --shadow:0 4px 20px rgba(15,23,42,.08);
    --shadow-lg:0 10px 40px rgba(15,23,42,.14);
  }
  *{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent}
  html,body{height:100%}
  body{
    font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,sans-serif;
    color:var(--dark); min-height:100vh; line-height:1.5;
    background:
      radial-gradient(circle at 0% 0%, #e0f2fe 0%, transparent 45%),
      radial-gradient(circle at 100% 100%, #ccfbf1 0%, transparent 45%),
      #f8fafc;
    -webkit-font-smoothing:antialiased;
  }
  .app-header{
    position:sticky; top:0; z-index:50;
    background:rgba(255,255,255,.85); backdrop-filter:blur(12px);
    border-bottom:1px solid var(--border);
    padding:14px 22px; display:flex; align-items:center; justify-content:space-between;
  }
  .logo{display:flex; align-items:center; gap:12px}
  .logo h1{font-size:18px; font-weight:800; letter-spacing:-.4px; line-height:1.1}
  .logo span{font-size:11px; color:var(--gray); font-weight:500}
  .header-right{display:flex; align-items:center; gap:12px}
  .clock{font-size:12px; color:var(--gray); font-weight:600; background:var(--light); padding:6px 12px; border-radius:20px; border:1px solid var(--border)}
  .badge{background:var(--primary-light); color:var(--primary-dark); padding:6px 12px; border-radius:20px; font-size:12px; font-weight:700}
  @media(max-width:640px){ .clock{display:none} }
  main{max-width:1240px; margin:0 auto; padding:26px 22px 60px}
  .hero{text-align:center; margin-bottom:24px}
  .hero h2{font-size:26px; font-weight:800; letter-spacing:-.6px; margin-bottom:6px}
  .hero p{color:var(--gray); font-size:15px}
  .layout{display:grid; grid-template-columns:1.2fr 1fr; gap:22px; align-items:start}
  @media(max-width:960px){ .layout{grid-template-columns:1fr} }
  .card{background:#fff; border-radius:var(--radius); box-shadow:var(--shadow); padding:24px; border:1px solid rgba(226,232,240,.7)}
  h3{font-size:16px; font-weight:700; margin-bottom:14px; letter-spacing:-.2px}
  .form-title{display:flex; align-items:center; gap:10px; margin-bottom:18px; padding-bottom:14px; border-bottom:1px solid var(--border)}
  .form-title .ico{width:38px; height:38px; border-radius:10px; background:linear-gradient(135deg,var(--primary),var(--accent)); display:flex; align-items:center; justify-content:center; color:#fff; flex-shrink:0}
  .form-title h3{margin:0; font-size:17px}
  .form-title p{font-size:12px; color:var(--gray); margin-top:2px}
  .info-banner{
    background:var(--primary-light); border:1px solid #bae6fd;
    border-radius:10px; padding:10px 12px; font-size:12px;
    color:#075985; margin-bottom:18px; line-height:1.5;
    display:flex; gap:8px; align-items:flex-start;
  }
  .info-banner svg{flex-shrink:0; margin-top:1px}
  .field{margin-bottom:15px}
  .field label{display:block; font-size:13px; font-weight:600; margin-bottom:6px; color:var(--dark)}
  .field input,.field select{
    width:100%; padding:13px 14px; border:1.5px solid var(--border);
    border-radius:10px; font-size:15px; font-family:inherit; color:var(--dark);
    background:#fff; transition:border-color .2s, box-shadow .2s; min-height:48px;
  }
  .field input::placeholder{color:#94a3b8}
  .field input:focus,.field select:focus{outline:none; border-color:var(--primary); box-shadow:0 0 0 3px rgba(14,165,233,.15)}
  .field select{appearance:none; background-image:url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%2364748b' stroke-width='2'%3e%3cpolyline points='6 9 12 15 18 9'/%3e%3c/svg%3e"); background-repeat:no-repeat; background-position:right 12px center; background-size:18px; padding-right:40px}
  .field .err{display:none; color:var(--danger); font-size:12px; margin-top:5px; font-weight:500}
  .field.error input,.field.error select{border-color:var(--danger); box-shadow:0 0 0 3px rgba(239,68,68,.1)}
  .field.error .err{display:block}
  .row{display:grid; grid-template-columns:1fr 1fr; gap:14px}
  @media(max-width:560px){ .row{grid-template-columns:1fr; gap:0} }
  .upload-grid{display:grid; grid-template-columns:repeat(3,1fr); gap:10px}
  @media(max-width:560px){ .upload-grid{grid-template-columns:1fr; gap:8px} }
  .upload-wrap{position:relative}
  .upload-box{
    display:block; position:relative; border:2px dashed var(--border); border-radius:12px;
    background:var(--light); min-height:120px; overflow:hidden;
    cursor:pointer; transition:all .2s;
  }
  .upload-box:hover{border-color:var(--primary); background:var(--primary-light)}
  .upload-box input[type="file"]{
    position:absolute; width:1px; height:1px; opacity:0; pointer-events:none;
  }
  .upload-box .placeholder{
    position:absolute; inset:0;
    display:flex; flex-direction:column; align-items:center; justify-content:center;
    text-align:center; padding:10px; gap:6px;
  }
  .upload-box .icon{width:26px; height:26px; color:var(--primary)}
  .upload-box .ph-title{font-size:13px; font-weight:600}
  .upload-box .ph-sub{font-size:11px; color:var(--gray)}
  .upload-box .preview{display:none; position:absolute; inset:0; background:#fff}
  .upload-box .preview img{width:100%; height:100%; object-fit:cover; display:block}
  .upload-box.has-file{border-style:solid; border-color:var(--success)}
  .upload-box.has-file .placeholder{display:none}
  .upload-box.has-file .preview{display:block}
  .upload-box .check{
    position:absolute; bottom:6px; left:6px; z-index:2;
    background:var(--success); color:#fff; font-size:10px; font-weight:700;
    padding:3px 8px; border-radius:20px; display:none; pointer-events:none;
  }
  .upload-box.has-file .check{display:block}
  .remove{
    position:absolute; top:6px; right:6px; width:26px; height:26px;
    border-radius:50%; background:rgba(239,68,68,.95); color:#fff;
    border:none; cursor:pointer; font-size:13px; font-weight:700;
    display:none; align-items:center; justify-content:center; z-index:5;
    font-family:inherit;
  }
  .upload-wrap.has-file .remove{display:flex}
  .btn-primary{
    width:100%; padding:14px 20px; border:none; border-radius:11px;
    background:linear-gradient(135deg,var(--primary),var(--accent));
    color:#fff; font-size:15px; font-weight:700; cursor:pointer;
    font-family:inherit; transition:transform .15s, box-shadow .2s;
    box-shadow:0 6px 18px rgba(14,165,233,.28);
  }
  .btn-primary:hover{transform:translateY(-1px); box-shadow:0 10px 24px rgba(14,165,233,.36)}
  .btn-primary:active{transform:translateY(0)}
  .btn-primary:disabled{opacity:.6; cursor:wait}
  .btn-ghost{
    padding:11px 16px; border:1.5px solid var(--border); border-radius:10px;
    background:#fff; color:var(--dark); font-weight:600; font-size:13px;
    cursor:pointer; font-family:inherit; transition:all .18s;
  }
  .btn-ghost:hover{border-color:var(--primary); color:var(--primary-dark); background:var(--primary-light)}
  .btn-ghost.danger:hover{border-color:var(--danger); color:var(--danger); background:#fef2f2}
  .form-actions{display:flex; gap:10px; margin-top:8px}
  .form-actions .btn-primary{flex:2}
  .form-actions .btn-ghost{flex:1}
  .list-header{display:flex; align-items:center; justify-content:space-between; margin-bottom:14px}
  .list-header .left{display:flex; align-items:center; gap:10px}
  .count{background:var(--primary); color:#fff; font-size:12px; font-weight:700; padding:3px 10px; border-radius:20px; min-width:26px; text-align:center}
  .search{
    width:100%; padding:11px 13px 11px 38px; border:1.5px solid var(--border);
    border-radius:10px; font-size:14px; font-family:inherit; margin-bottom:12px;
    background:#fff url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%2364748b' stroke-width='2'%3e%3ccircle cx='11' cy='11' r='8'/%3e%3cpath d='m21 21-4.3-4.3'/%3e%3c/svg%3e") no-repeat 12px center; background-size:16px;
    transition:border-color .2s, box-shadow .2s;
  }
  .search:focus{outline:none; border-color:var(--primary); box-shadow:0 0 0 3px rgba(14,165,233,.15)}
  .patient-list{max-height:520px; overflow-y:auto; margin:0 -6px 12px}
  .patient-list::-webkit-scrollbar{width:6px}
  .patient-list::-webkit-scrollbar-thumb{background:#cbd5e1; border-radius:3px}
  .empty{text-align:center; color:var(--gray); font-size:13px; padding:36px 12px}
  .empty svg{width:44px; height:44px; color:#cbd5e1; margin-bottom:10px}
  .patient-item{display:flex; align-items:center; gap:12px; padding:11px; border-radius:10px; transition:background .15s; cursor:pointer; border:1px solid transparent}
  .patient-item:hover{background:var(--light); border-color:var(--border)}
  .avatar{
    width:46px; height:46px; border-radius:50%; flex-shrink:0;
    background:linear-gradient(135deg,var(--primary),var(--accent));
    color:#fff; font-weight:800; font-size:15px; letter-spacing:.5px;
    display:flex; align-items:center; justify-content:center;
  }
  .patient-item .info{flex:1; min-width:0}
  .patient-item strong{display:block; font-size:14px; font-weight:700; white-space:nowrap; overflow:hidden; text-overflow:ellipsis}
  .patient-item .meta{font-size:12px; color:var(--gray); white-space:nowrap; overflow:hidden; text-overflow:ellipsis}
  .patient-item .ticket-mini{
    font-size:11px; font-weight:700; color:var(--primary-dark);
    background:var(--primary-light); padding:4px 9px; border-radius:6px; flex-shrink:0;
  }
  .list-footer{display:flex; gap:10px; padding-top:12px; border-top:1px solid var(--border)}
  .list-footer button{flex:1}
  .storage-info{
    font-size:11px; color:var(--gray); margin-top:10px; text-align:center;
    padding:6px; background:var(--light); border-radius:8px;
    border:1px solid var(--border);
  }
  .storage-info strong{color:var(--primary-dark)}
  .modal{display:none; position:fixed; inset:0; z-index:100; background:rgba(15,23,42,.6); backdrop-filter:blur(4px); align-items:center; justify-content:center; padding:20px}
  .modal.open{display:flex; animation:fade .2s}
  @keyframes fade{from{opacity:0}to{opacity:1}}
  .modal-content{background:#fff; border-radius:16px; max-width:560px; width:100%; max-height:90vh; overflow-y:auto; padding:26px; position:relative; box-shadow:var(--shadow-lg); animation:slideUp .3s ease}
  @keyframes slideUp{from{transform:translateY(24px);opacity:0}to{transform:translateY(0);opacity:1}}
  .modal-close{position:absolute; top:14px; right:14px; width:34px; height:34px; border-radius:50%; border:none; background:var(--light); cursor:pointer; font-size:16px; color:var(--gray); display:flex; align-items:center; justify-content:center; font-family:inherit}
  .modal-close:hover{background:#f1f5f9; color:var(--dark)}
  .detail-row{display:flex; padding:9px 0; border-bottom:1px solid var(--border); font-size:14px}
  .detail-row:last-child{border-bottom:none}
  .detail-row .k{color:var(--gray); width:130px; flex-shrink:0; font-size:13px}
  .detail-row .v{font-weight:600; word-break:break-word}
  .docs-grid{display:grid; grid-template-columns:repeat(3,1fr); gap:8px; margin-top:14px}
  .docs-grid img{width:100%; height:100px; object-fit:cover; border-radius:8px; border:1px solid var(--border); cursor:zoom-in; transition:transform .2s; display:block}
  .docs-grid img:hover{transform:scale(1.03)}
  .docs-grid .no-doc{height:100px; border-radius:8px; border:1px dashed var(--border); display:flex; align-items:center; justify-content:center; font-size:11px; color:var(--gray); text-align:center; padding:6px}
  .toast{position:fixed; bottom:24px; left:50%; z-index:200; transform:translateX(-50%) translateY(120px); background:var(--dark); color:#fff; padding:12px 22px; border-radius:10px; font-size:14px; font-weight:500; opacity:0; transition:all .3s ease; box-shadow:var(--shadow-lg); max-width:90vw; text-align:center}
  .toast.show{transform:translateX(-50%) translateY(0); opacity:1}
  .toast.success{background:var(--success)}
  .toast.error{background:var(--danger)}
</style>
</head>
<body>

<header class="app-header">
  <div class="logo">
    <svg width="40" height="40" viewBox="0 0 40 40" fill="none" aria-hidden="true">
      <defs>
        <linearGradient id="lg" x1="0" y1="0" x2="40" y2="40">
          <stop stop-color="#0ea5e9"/><stop offset="1" stop-color="#14b8a6"/>
        </linearGradient>
      </defs>
      <rect width="40" height="40" rx="11" fill="url(#lg)"/>
      <path d="M20 11v18M11 20h18" stroke="#fff" stroke-width="3.2" stroke-linecap="round"/>
      <circle cx="28.5" cy="11.5" r="3" fill="#fff" opacity=".9"/>
    </svg>
    <div>
      <h1>MediFila</h1>
      <span>Cadastro de Pacientes</span>
    </div>
  </div>
  <div class="header-right">
    <span class="clock" id="clock">--:--</span>
    <span class="badge">Recepção</span>
  </div>
</header>

<main>
  <div class="hero">
    <h2>Cadastro rápido de pacientes</h2>
    <p>Preencha CPF e convênio, anexe as fotos e gere a senha de atendimento.</p>
  </div>

  <div class="layout">
    <div class="card">
      <div class="form-title">
        <div class="ico">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/>
            <circle cx="9" cy="7" r="4"/>
            <path d="M19 8v6M22 11h-6"/>
          </svg>
        </div>
        <div>
          <h3>Novo cadastro</h3>
          <p>Campos com * são obrigatórios</p>
        </div>
      </div>

      <div class="info-banner">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M12 16v-4M12 8h.01"/></svg>
        <span>Preencha o <strong>CPF</strong> e o <strong>convênio</strong>. O resto das informações pode vir pelas <strong>fotos dos documentos</strong>.</span>
      </div>

      <div id="patientForm">
        <div class="field" data-field="cpf">
          <label for="cpf">CPF *</label>
          <input id="cpf" type="text" inputmode="numeric" placeholder="000.000.000-00" maxlength="14" autocomplete="off">
          <div class="err">CPF inválido — verifique os dígitos</div>
        </div>

        <div class="field" data-field="rg">
          <label for="rg">RG</label>
          <input id="rg" type="text" placeholder="00.000.000-0" autocomplete="off">
        </div>

        <div class="row">
          <div class="field" data-field="nascimento">
            <label for="nascimento">Data de nascimento</label>
            <input id="nascimento" type="date">
          </div>
          <div class="field" data-field="convenio">
            <label for="convenio">Convênio *</label>
            <select id="convenio">
              <option value="">Selecione...</option>
              <option>SUS</option>
              <option>Unimed</option>
              <option>Bradesco Saúde</option>
              <option>SulAmérica</option>
              <option>Amil</option>
              <option>NotreDame Intermédica</option>
              <option>Hapvida</option>
              <option>Particular</option>
              <option>Outro</option>
            </select>
            <div class="err">Selecione o convênio</div>
          </div>
        </div>

        <div class="field">
          <label>Documentos (fotos)</label>
          <div class="upload-grid">
            <div class="upload-wrap" data-key="cpf">
              <label class="upload-box">
                <input type="file" accept="image/*">
                <div class="placeholder">
                  <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="16" rx="2"/><circle cx="9" cy="11" r="2.5"/><path d="M5 18c1.5-3 6-3 7.5 0"/><path d="M16 9h3M16 13h3"/></svg>
                  <div class="ph-title">Foto do CPF</div>
                  <div class="ph-sub">Clique para enviar</div>
                </div>
                <div class="preview"></div>
                <span class="check">✓ Enviado</span>
              </label>
              <button type="button" class="remove" title="Remover">✕</button>
            </div>

            <div class="upload-wrap" data-key="rg">
              <label class="upload-box">
                <input type="file" accept="image/*">
                <div class="placeholder">
                  <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="16" rx="2"/><circle cx="9" cy="11" r="2.5"/><path d="M5 18c1.5-3 6-3 7.5 0"/><path d="M16 9h3M16 13h3"/></svg>
                  <div class="ph-title">Foto do RG</div>
                  <div class="ph-sub">Clique para enviar</div>
                </div>
                <div class="preview"></div>
                <span class="check">✓ Enviado</span>
              </label>
              <button type="button" class="remove" title="Remover">✕</button>
            </div>

            <div class="upload-wrap" data-key="convenio">
              <label class="upload-box">
                <input type="file" accept="image/*">
                <div class="placeholder">
                  <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2.5" y="6" width="19" height="13" rx="2"/><path d="M2.5 10h19"/><path d="M6 15h4"/></svg>
                  <div class="ph-title">Convênio</div>
                  <div class="ph-sub">Clique para enviar</div>
                </div>
                <div class="preview"></div>
                <span class="check">✓ Enviado</span>
              </label>
              <button type="button" class="remove" title="Remover">✕</button>
            </div>
          </div>
        </div>

        <div class="form-actions">
          <button type="button" class="btn-primary" id="btnSubmit">Cadastrar paciente</button>
          <button type="button" class="btn-ghost" id="btnReset">Limpar</button>
        </div>
      </div>
    </div>

    <div class="card">
      <div class="list-header">
        <div class="left">
          <h3 style="margin:0">Pacientes cadastrados</h3>
          <span class="count" id="countBadge">0</span>
        </div>
      </div>

      <input type="text" class="search" id="searchInput" placeholder="Buscar por CPF, RG, convênio ou senha...">

      <div class="patient-list" id="patientList">
        <div class="empty">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
            <path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/>
            <circle cx="9" cy="7" r="4"/>
            <path d="M22 21v-2a4 4 0 0 0-3-3.87"/>
            <path d="M16 3.13a4 4 0 0 1 0 7.75"/>
          </svg>
          <div>Nenhum paciente cadastrado</div>
        </div>
      </div>

      <div class="list-footer">
        <button class="btn-ghost" id="btnExport">Exportar JSON</button>
        <button class="btn-ghost danger" id="btnClear">Limpar tudo</button>
      </div>

      <div class="storage-info" id="storageInfo">Carregando...</div>
    </div>
  </div>
</main>

<div class="modal" id="modal">
  <div class="modal-content">
    <button class="modal-close" id="modalClose">✕</button>
    <div id="modalBody"></div>
  </div>
</div>

<script>
(function(){
  'use strict';

  /* ========== UTIL ========== */
  function $(sel, root){ return (root || document).querySelector(sel); }
  function $$(sel, root){ return Array.prototype.slice.call((root || document).querySelectorAll(sel)); }

  function escapeHtml(s){
    return String(s == null ? '' : s).replace(/[&<>"']/g, function(c){
      return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c];
    });
  }

  function pad3(n){
    var s = String(n);
    while (s.length < 3) s = '0' + s;
    return s;
  }

  function toast(msg, type){
    var t = document.createElement('div');
    t.className = 'toast' + (type ? ' ' + type : '');
    t.textContent = msg;
    document.body.appendChild(t);
    setTimeout(function(){ t.classList.add('show'); }, 10);
    setTimeout(function(){
      t.classList.remove('show');
      setTimeout(function(){
        if (t.parentNode) t.parentNode.removeChild(t);
      }, 320);
    }, 2600);
  }

  function formatTime(ts){
    try {
      return new Date(ts).toLocaleString('pt-BR', {
        day:'2-digit', month:'2-digit', year:'2-digit', hour:'2-digit', minute:'2-digit'
      });
    } catch(e){ return '—'; }
  }

  function maskCPF(v){
    var d = String(v).replace(/\D/g,'').slice(0,11);
    if (d.length <= 3) return d;
    if (d.length <= 6) return d.slice(0,3) + '.' + d.slice(3);
    if (d.length <= 9) return d.slice(0,3) + '.' + d.slice(3,6) + '.' + d.slice(6);
    return d.slice(0,3) + '.' + d.slice(3,6) + '.' + d.slice(6,9) + '-' + d.slice(9);
  }

  function isValidCPF(cpf){
    var d = String(cpf || '').replace(/\D/g,'');
    if (d.length !== 11) return false;
    if (/^(\d)\1+$/.test(d)) return false;
    var sum = 0, i, r;
    for (i = 0; i < 9; i++) sum += parseInt(d.charAt(i),10) * (10 - i);
    r = (sum * 10) % 11;
    if (r === 10) r = 0;
    if (r !== parseInt(d.charAt(9),10)) return false;
    sum = 0;
    for (i = 0; i < 10; i++) sum += parseInt(d.charAt(i),10) * (11 - i);
    r = (sum * 10) % 11;
    if (r === 10) r = 0;
    return r === parseInt(d.charAt(10),10);
  }

  /* ========== STORAGE BLINDADO ========== */
  var memoryFallback = {};
  var storageAvailable = true;

  // Testa se localStorage funciona
  try {
    var testKey = '__medifila_test__';
    localStorage.setItem(testKey, '1');
    localStorage.removeItem(testKey);
  } catch(e){
    storageAvailable = false;
    console.warn('localStorage indisponível, usando memória:', e);
  }

  function storeGet(key, fallback){
    try {
      if (storageAvailable){
        var v = localStorage.getItem(key);
        return v == null ? fallback : v;
      }
      return memoryFallback[key] != null ? memoryFallback[key] : fallback;
    } catch(e){
      return fallback;
    }
  }

  function storeSet(key, val){
    try {
      if (storageAvailable){
        localStorage.setItem(key, val);
        return true;
      }
      memoryFallback[key] = val;
      return true;
    } catch(e){
      console.error('Erro ao salvar:', e);
      return false;
    }
  }

  function storeRemove(key){
    try {
      if (storageAvailable) localStorage.removeItem(key);
      delete memoryFallback[key];
    } catch(e){}
  }

  /* ========== ESTADO ========== */
  var patients = [];
  var lastTicket = 0;
  var searchTerm = '';
  var currentFiles = { cpf: null, rg: null, convenio: null };

  function loadState(){
    try {
      var raw = storeGet('medifila_patients', '[]');
      patients = JSON.parse(raw) || [];
      if (!Array.isArray(patients)) patients = [];
    } catch(e){
      console.error('Erro ao carregar pacientes:', e);
      patients = [];
    }

    var lt = parseInt(storeGet('medifila_lastTicket', '0'), 10);
    lastTicket = isNaN(lt) ? 0 : lt;
  }

  function saveState(){
    var json = JSON.stringify(patients);
    var ok = storeSet('medifila_patients', json);
    if (ok) storeSet('medifila_lastTicket', String(lastTicket));
    return ok;
  }

  /* ========== RELÓGIO ========== */
  function updateClock(){
    var el = $('#clock');
    if (!el) return;
    var now = new Date();
    el.textContent = now.toLocaleString('pt-BR', {
      day:'2-digit', month:'2-digit', hour:'2-digit', minute:'2-digit'
    });
  }
  setInterval(updateClock, 30000);
  updateClock();

  /* ========== STORAGE INFO ========== */
  function updateStorageInfo(){
    var el = $('#storageInfo');
    if (!el) return;
    try {
      var json = JSON.stringify(patients);
      var bytes = new Blob([json]).size;
      var kb = (bytes / 1024).toFixed(1);
      var limitKB = 5000;
      var pct = Math.min(100, (bytes / 1024 / limitKB * 100)).toFixed(0);
      var mode = storageAvailable ? 'localStorage' : 'memória (temporário)';
      el.innerHTML = 'Armazenamento: <strong>' + mode + '</strong> • ' +
                     patients.length + ' pacientes • <strong>' + kb + ' KB</strong> (~' + pct + '% de 5MB)';
    } catch(e){
      el.textContent = patients.length + ' pacientes cadastrados';
    }
  }

  /* ========== RENDER ========== */
  function renderList(){
    var list = $('#patientList');
    if (!list) return;

    var countBadge = $('#countBadge');
    if (countBadge) countBadge.textContent = patients.length;

    var filtered = patients.slice().sort(function(a,b){ return b.createdAt - a.createdAt; });

    if (searchTerm){
      var q = searchTerm.toLowerCase();
      filtered = filtered.filter(function(p){
        return (p.cpf || '').toLowerCase().indexOf(q) !== -1 ||
               (p.rg || '').toLowerCase().indexOf(q) !== -1 ||
               (p.ticket || '').toLowerCase().indexOf(q) !== -1 ||
               (p.convenio || '').toLowerCase().indexOf(q) !== -1;
      });
    }

    if (!filtered.length){
      list.innerHTML =
        '<div class="empty">' +
          '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">' +
            '<path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/>' +
            '<circle cx="9" cy="7" r="4"/>' +
            '<path d="M22 21v-2a4 4 0 0 0-3-3.87"/>' +
            '<path d="M16 3.13a4 4 0 0 1 0 7.75"/>' +
          '</svg>' +
          '<div>' + (searchTerm ? 'Nenhum resultado encontrado' : 'Nenhum paciente cadastrado') + '</div>' +
        '</div>';
      updateStorageInfo();
      return;
    }

    var html = '';
    filtered.forEach(function(p){
      var digits = String(p.ticket || '').replace(/\D/g,'').slice(-2) || '?';
      html += '<div class="patient-item" data-id="' + escapeHtml(p.id) + '">' +
        '<div class="avatar">' + escapeHtml(digits) + '</div>' +
        '<div class="info">' +
          '<strong>Paciente ' + escapeHtml(p.ticket) + '</strong>' +
          '<span class="meta">CPF: ' + escapeHtml(p.cpf) + ' • ' + escapeHtml(p.convenio || '—') + '</span>' +
        '</div>' +
        '<span class="ticket-mini">' + escapeHtml(p.ticket) + '</span>' +
      '</div>';
    });
    list.innerHTML = html;

    $$('.patient-item', list).forEach(function(item){
      item.addEventListener('click', function(){
        openDetails(item.getAttribute('data-id'));
      });
    });

    updateStorageInfo();
  }

  var searchInput = $('#searchInput');
  if (searchInput){
    searchInput.addEventListener('input', function(e){
      searchTerm = e.target.value.trim();
      renderList();
    });
  }

  /* ========== MODAL ========== */
  function openDetails(id){
    var p = null;
    for (var i = 0; i < patients.length; i++){
      if (patients[i].id === id){ p = patients[i]; break; }
    }
    if (!p) return;

    var docs = p.documentos || {};
    function docBox(key, label){
      return docs[key]
        ? '<div><img src="' + docs[key] + '" alt="' + label + '"></div>'
        : '<div class="no-doc">' + label + '<br>não enviado</div>';
    }

    $('#modalBody').innerHTML =
      '<h3 style="margin-bottom:16px">Detalhes do Paciente</h3>' +
      '<div class="detail-row"><span class="k">Senha</span><span class="v" style="color:var(--primary-dark)">' + escapeHtml(p.ticket) + '</span></div>' +
      '<div class="detail-row"><span class="k">CPF</span><span class="v">' + escapeHtml(p.cpf) + '</span></div>' +
      '<div class="detail-row"><span class="k">RG</span><span class="v">' + escapeHtml(p.rg || '—') + '</span></div>' +
      '<div class="detail-row"><span class="k">Nascimento</span><span class="v">' + escapeHtml(p.nascimento || '—') + '</span></div>' +
      '<div class="detail-row"><span class="k">Convênio</span><span class="v">' + escapeHtml(p.convenio || '—') + '</span></div>' +
      '<div class="detail-row"><span class="k">Cadastrado em</span><span class="v">' + formatTime(p.createdAt) + '</span></div>' +
      '<h3 style="margin-top:20px;margin-bottom:6px;font-size:14px">Documentos</h3>' +
      '<div class="docs-grid">' +
        docBox('cpf','CPF') +
        docBox('rg','RG') +
        docBox('convenio','Convênio') +
      '</div>' +
      '<button type="button" class="btn-primary" id="btnRemove" style="margin-top:22px">Remover cadastro</button>';

    $$('#modalBody .docs-grid img').forEach(function(img){
      img.addEventListener('click', function(){
        var src = img.getAttribute('src');
        var w = window.open('');
        if (w){
          w.document.write('<title>Documento</title><img src="' + src + '" style="max-width:100%;display:block;margin:0 auto">');
        }
      });
    });

    var btn = $('#btnRemove');
    if (btn){
      btn.addEventListener('click', function(){ removePatient(p.id); });
    }

    $('#modal').classList.add('open');
  }

  function closeModal(){
    var m = $('#modal');
    if (m) m.classList.remove('open');
  }

  var modalClose = $('#modalClose');
  if (modalClose) modalClose.addEventListener('click', closeModal);

  var modalEl = $('#modal');
  if (modalEl){
    modalEl.addEventListener('click', function(e){
      if (e.target === modalEl) closeModal();
    });
  }

  function removePatient(id){
    patients = patients.filter(function(p){ return p.id !== id; });
    saveState();
    renderList();
    closeModal();
    toast('Cadastro removido', 'success');
  }

  /* ========== EXPORTAR / LIMPAR ========== */
  var btnExport = $('#btnExport');
  if (btnExport){
    btnExport.addEventListener('click', function(){
      if (!patients.length) return toast('Nenhum dado para exportar', 'error');
      try {
        var blob = new Blob([JSON.stringify(patients, null, 2)], { type: 'application/json' });
        var url = URL.createObjectURL(blob);
        var a = document.createElement('a');
        a.href = url;
        a.download = 'medifila-pacientes-' + Date.now() + '.json';
        document.body.appendChild(a);
        a.click();
        setTimeout(function(){
          document.body.removeChild(a);
          URL.revokeObjectURL(url);
        }, 100);
        toast('Dados exportados', 'success');
      } catch(e){
        toast('Falha ao exportar', 'error');
      }
    });
  }

  var btnClear = $('#btnClear');
  if (btnClear){
    btnClear.addEventListener('click', function(){
      if (!patients.length) return toast('Nada para limpar', 'error');
      if (!confirm('Tem certeza que deseja apagar todos os cadastros?')) return;
      patients = [];
      lastTicket = 0;
      storeRemove('medifila_patients');
      storeRemove('medifila_lastTicket');
      renderList();
      toast('Todos os dados foram apagados', 'success');
    });
  }

  /* ========== MÁSCARA ========== */
  var cpfInput = $('#cpf');
  if (cpfInput){
    cpfInput.addEventListener('input', function(e){
      e.target.value = maskCPF(e.target.value);
    });
  }

  /* ========== COMPRESSÃO (agressiva pra caber no localStorage) ========== */
  function compressImage(file, maxW, quality){
    maxW = maxW || 320;      // menor que antes pra caber mais
    quality = quality || 0.5;
    return new Promise(function(resolve, reject){
      var reader = new FileReader();
      reader.onload = function(ev){
        var img = new Image();
        img.onload = function(){
          try {
            var canvas = document.createElement('canvas');
            var w = img.width, h = img.height;
            if (w > maxW){
              h = Math.round(h * maxW / w);
              w = maxW;
            }
            canvas.width = w;
            canvas.height = h;
            var ctx = canvas.getContext('2d');
            ctx.fillStyle = '#ffffff';
            ctx.fillRect(0, 0, w, h);
            ctx.drawImage(img, 0, 0, w, h);
            resolve(canvas.toDataURL('image/jpeg', quality));
          } catch(err){
            reject(err);
          }
        };
        img.onerror = function(){ reject(new Error('Falha ao carregar imagem')); };
        img.src = ev.target.result;
      };
      reader.onerror = function(){ reject(new Error('Falha ao ler arquivo')); };
      reader.readAsDataURL(file);
    });
  }

  /* ========== UPLOAD ========== */
  $$('.upload-wrap').forEach(function(wrap){
    var key = wrap.getAttribute('data-key');
    var box = wrap.querySelector('.upload-box');
    var input = wrap.querySelector('input[type="file"]');
    var preview = wrap.querySelector('.preview');
    var removeBtn = wrap.querySelector('.remove');

    if (!input || !box) return;

    input.addEventListener('change', function(e){
      var file = e.target.files && e.target.files[0];
      if (!file) return;

      if (file.type.indexOf('image/') !== 0){
        input.value = '';
        return toast('Selecione um arquivo de imagem', 'error');
      }
      if (file.size > 20 * 1024 * 1024){
        input.value = '';
        return toast('Imagem muito grande (máx. 20MB)', 'error');
      }

      var objectUrl = null;
      try { objectUrl = URL.createObjectURL(file); } catch(err){ objectUrl = null; }

      if (objectUrl){
        preview.innerHTML = '<img src="' + objectUrl + '" alt="">';
        box.classList.add('has-file');
        wrap.classList.add('has-file');
      }

      toast('Comprimindo imagem...');

      compressImage(file).then(function(dataUrl){
        currentFiles[key] = dataUrl;
        if (!objectUrl){
          preview.innerHTML = '<img src="' + dataUrl + '" alt="">';
          box.classList.add('has-file');
          wrap.classList.add('has-file');
        }
        var sizeKB = Math.round(dataUrl.length * 0.75 / 1024);
        toast('Documento anexado (' + sizeKB + ' KB)', 'success');
      }).catch(function(){
        currentFiles[key] = null;
        preview.innerHTML = '';
        box.classList.remove('has-file');
        wrap.classList.remove('has-file');
        input.value = '';
        toast('Falha ao processar imagem', 'error');
      });
    });

    if (removeBtn){
      removeBtn.addEventListener('click', function(e){
        e.preventDefault();
        e.stopPropagation();
        currentFiles[key] = null;
        preview.innerHTML = '';
        input.value = '';
        box.classList.remove('has-file');
        wrap.classList.remove('has-file');
        toast('Documento removido');
      });
    }
  });

  /* ========== VALIDAÇÃO ========== */
  function markError(field){
    var el = document.querySelector('.field[data-field="' + field + '"]');
    if (el) el.classList.add('error');
  }

  function clearErrors(){
    $$('.field.error').forEach(function(f){ f.classList.remove('error'); });
  }

  /* ========== SUBMIT ========== */
  var isSubmitting = false;

  function handleSubmit(){
    if (isSubmitting) return;
    clearErrors();

    var cpfEl = $('#cpf');
    var rgEl = $('#rg');
    var nascEl = $('#nascimento');
    var convEl = $('#convenio');

    if (!cpfEl || !convEl) return;

    var cpf = cpfEl.value.trim();
    var rg = rgEl ? rgEl.value.trim() : '';
    var nasc = nascEl ? nascEl.value : '';
    var conv = convEl.value;

    var hasError = false;
    if (!isValidCPF(cpf)){ markError('cpf'); hasError = true; }
    if (!conv){ markError('convenio'); hasError = true; }

    if (hasError){
      var first = document.querySelector('.field.error');
      if (first && first.scrollIntoView){
        first.scrollIntoView({ behavior:'smooth', block:'center' });
      }
      return toast('Verifique os campos destacados', 'error');
    }

    var dup = false;
    for (var i = 0; i < patients.length; i++){
      if (patients[i].cpf === cpf){ dup = true; break; }
    }
    if (dup){
      if (!confirm('Já existe um paciente com este CPF. Deseja cadastrar mesmo assim?')) return;
    }

    isSubmitting = true;
    var btn = $('#btnSubmit');
    if (btn){ btn.disabled = true; btn.textContent = 'Salvando...'; }

    // Timeout pra não travar a UI
    setTimeout(function(){
      try {
        var nextTicket = lastTicket + 1;
        var ticket = 'A-' + pad3(nextTicket);

        var patient = {
          id: Date.now().toString(36) + Math.random().toString(36).slice(2,7),
          cpf: cpf,
          rg: rg,
          nascimento: nasc,
          convenio: conv,
          documentos: {
            cpf: currentFiles.cpf,
            rg: currentFiles.rg,
            convenio: currentFiles.convenio
          },
          createdAt: Date.now(),
          ticket: ticket
        };

        // Tenta salvar com documentos
        var backupPatients = patients.slice();
        var backupTicket = lastTicket;

        patients.push(patient);
        lastTicket = nextTicket;

        var ok = saveState();

        // Se falhou, tenta salvar sem documentos
        if (!ok){
          patient.documentos = { cpf: null, rg: null, convenio: null };
          patients = backupPatients.slice();
          patients.push(patient);
          lastTicket = nextTicket;
          ok = saveState();

          if (ok){
            toast('Salvo sem fotos (armazenamento cheio). Exporte e limpe.', 'error');
          } else {
            // Última tentativa: salvar só o paciente sem docs
            patients = backupPatients.concat([patient]);
            lastTicket = backupTicket + 1;
            ok = saveState();
          }
        }

        if (!ok){
          patients = backupPatients;
          lastTicket = backupTicket;
          isSubmitting = false;
          if (btn){ btn.disabled = false; btn.textContent = 'Cadastrar paciente'; }
          return toast('Armazenamento totalmente cheio. Exporte e limpe os dados antigos.', 'error');
        }

        renderList();
        resetForm();

        if (storageAvailable){
          toast('Paciente cadastrado • Senha ' + ticket, 'success');
        } else {
          toast('Cadastrado (memória temporária) • Senha ' + ticket, 'success');
        }

        setTimeout(function(){
          var first = document.querySelector('.patient-item');
          if (first){
            if (first.scrollIntoView) first.scrollIntoView({ behavior:'smooth', block:'center' });
            first.style.transition = 'background .4s';
            first.style.background = 'var(--primary-light)';
            setTimeout(function(){ first.style.background = ''; }, 1400);
          }
        }, 80);

      } catch(err){
        console.error('Erro no cadastro:', err);
        toast('Erro ao cadastrar: ' + (err && err.message ? err.message : 'desconhecido'), 'error');
      } finally {
        isSubmitting = false;
        if (btn){ btn.disabled = false; btn.textContent = 'Cadastrar paciente'; }
      }
    }, 20);
  }

  var btnSubmit = $('#btnSubmit');
  if (btnSubmit){
    btnSubmit.addEventListener('click', function(e){
      e.preventDefault();
      e.stopPropagation();
      handleSubmit();
    });
  }

  /* ========== RESET ========== */
  function resetForm(){
    var cpfEl = $('#cpf');
    var rgEl = $('#rg');
    var nascEl = $('#nascimento');
    var convEl = $('#convenio');

    if (cpfEl) cpfEl.value = '';
    if (rgEl) rgEl.value = '';
    if (nascEl) nascEl.value = '';
    if (convEl) convEl.value = '';

    currentFiles.cpf = null;
    currentFiles.rg = null;
    currentFiles.convenio = null;

    $$('.upload-wrap').forEach(function(wrap){
      wrap.classList.remove('has-file');
      var box = wrap.querySelector('.upload-box');
      if (box) box.classList.remove('has-file');
      var p = wrap.querySelector('.preview');
      if (p) p.innerHTML = '';
      var inp = wrap.querySelector('input[type="file"]');
      if (inp) inp.value = '';
    });

    clearErrors();
    if (cpfEl) cpfEl.focus();
  }

  var btnReset = $('#btnReset');
  if (btnReset){
    btnReset.addEventListener('click', function(){
      resetForm();
      toast('Formulário limpo');
    });
  }

  /* ========== ATALHOS ========== */
  document.addEventListener('keydown', function(e){
    if (e.key === 'Escape'){
      var m = $('#modal');
      if (m && m.classList.contains('open')) closeModal();
    }
    if (e.ctrlKey && e.key === 'Enter'){
      handleSubmit();
    }
  });

  /* ========== INIT ========== */
  loadState();
  renderList();
  if (cpfInput) cpfInput.focus();

  if (!storageAvailable){
    toast('Modo temporário: dados serão perdidos ao fechar', 'error');
  }

})();
</script>
</body>
</html>
