<!doctype html>
<html lang="pt-BR">

<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>UNICID — Sistema de Verificação de Diplomas</title>

  <style>
    :root {
      --bg: #f4f8ff;
      --card: #ffffff;
      --accent: #0072ce;
      --muted: #6b7280;
      --border: #d9e4f5;
    }

    body {
      font-family: Inter, system-ui, -apple-system, 'Segoe UI', Roboto, Arial;
      background: var(--bg);
      margin: 0;
      color: #0f172a;
    }

    header {
      background: linear-gradient(90deg, #0072ce15, #0072ce05);
      padding: 24px 28px;
      border-bottom: 1px solid var(--border);
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .brand {
      font-weight: 700;
      font-size: 18px;
      color: #003b73;
    }

    .container {
      max-width: 980px;
      margin: 36px auto;
      padding: 20px;
    }

    .search {
      background: var(--card);
      padding: 20px;
      border-radius: 12px;
      display: flex;
      gap: 16px;
      box-shadow: 0 6px 18px rgba(15, 23, 42, 0.06);
    }

    .search input {
      flex: 1;
      padding: 14px;
      border-radius: 8px;
      border: 1px solid var(--border);
      font-size: 15px;
    }

    .search button {
      background: var(--accent);
      color: #fff;
      border: none;
      padding: 14px 18px;
      border-radius: 8px;
      font-weight: 600;
      cursor: pointer;
    }

    .content {
      display: flex;
      gap: 20px;
      margin-top: 20px;
    }

    .left {
      flex: 1;
    }

    .right {
      width: 460px;
    }

    .card {
      background: var(--card);
      padding: 22px;
      border-radius: 12px;
      box-shadow: 0 6px 18px rgba(15, 23, 42, 0.06);
    }

    .muted {
      color: var(--muted);
      font-size: 14px;
    }

    .meta {
      margin-top: 10px;
      font-size: 15px;
    }

    .label {
      display: inline-block;
      margin-top: 12px;
      padding: 6px 12px;
      border-radius: 8px;
      background: #e0f0ff;
      color: #004a99;
      font-weight: 700;
    }

    .img-wrap {
      margin-top: 12px;
      background: #f9fbff;
      padding: 18px;
      border-radius: 10px;
      border: 1px solid var(--border);
      display: flex;
      justify-content: center;
    }

    canvas {
      background: #fff;
      border-radius: 8px;
      box-shadow: 0 8px 30px rgba(12, 20, 40, 0.06);
    }

    footer {
      text-align: center;
      font-size: 13px;
      color: var(--muted);
      margin: 30px auto;
      max-width: 980px;
    }

    @media (max-width: 820px) {
      .content {
        flex-direction: column;
      }
      .right {
        width: 100%;
      }
    }
  </style>
</head>

<body>

<header>
  <div class="brand">UNICID — Universidade Cidade de São Paulo</div>
  <div class="muted">Portal de Autenticidade Acadêmica</div>
</header>

<div class="container">

  <div class="search">
    <input id="q" placeholder="Consultar por número do diploma (ex: 55578-87)">
    <button id="btn">Consultar</button>
  </div>

  <div class="content">

    <div class="left">
      <div class="card">
        <div class="muted">Resultado da verificação</div>
        <h2 id="studentName">—</h2>
        <div class="meta" id="degree">—</div>
        <div class="meta" id="inst">—</div>
        <div class="meta" id="date">—</div>
        <span class="label" id="status">—</span>

        <hr style="margin:18px 0;border:none;border-top:1px solid #eef2f6">

        <div class="muted">Número do diploma</div>
        <div id="dnum" style="font-weight:700;margin-top:6px">—</div>
      </div>
    </div>

    <div class="right">
      <div class="card">
        <div class="muted">Visualização</div>
        <div class="img-wrap">
          <canvas id="previewCanvas" width="420" height="260"></canvas>
        </div>
      </div>
    </div>

  </div>
</div>

<footer>
  UNICID — Universidade Cidade de São Paulo © 2025  
  <br>Grupo Cruzeiro do Sul Educacional ⭐
</footer>

<script>
  const records = {

    '55578-87': {
      diploma: '55578-87',
      name: 'Cassio Augusto Rosa',
      degree: 'Bacharel em Engenharia Mecatrônica',
      inst: 'Universidade Cidade de São Paulo — UNICID',
      date: 'Conclusão em 10/08/2019',
      status: 'Ativo'
    },

    '46566-85': {
      diploma: '46566-85',
      name: 'Lucelene Cristina Rodrigues da Silva Siqueira',
      degree: 'Mestrado em Educação Especial',
      inst: 'Universidade Cidade de São Paulo — UNICID',
      date: 'Conclusão em 10/08/2024',
      status: 'Ativo'
    }

  };

  const canvas = document.getElementById('previewCanvas');
  const ctx = canvas.getContext('2d');

  function drawCanvas(data) {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.fillStyle = "#ffffff";
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    ctx.fillStyle = "#003b73";
    ctx.font = "bold 22px Arial";
    ctx.fillText("UNICID", 160, 40);

    ctx.font = "14px Arial";
    ctx.fillText("Universidade Cidade de São Paulo", 80, 65);

    ctx.font = "bold 16px Arial";
    ctx.fillText(data.name, 40, 120);

    ctx.font = "14px Arial";
    ctx.fillText(data.degree, 40, 145);
    ctx.fillText(data.date, 40, 170);
  }

  document.getElementById('btn').onclick = () => {
    const q = document.getElementById('q').value.trim();
    const r = records[q];

    if (!r) {
      alert('Diploma não encontrado.');
      return;
    }

    studentName.innerText = r.name;
    degree.innerText = r.degree;
    inst.innerText = r.inst;
    date.innerText = r.date;
    status.innerText = r.status;
    dnum.innerText = r.diploma;

    drawCanvas(r);
  };
</script>

</body>
</html>
