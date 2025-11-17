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
      font-family: Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;
      background: var(--bg);
      margin: 0;
      color: #0f172a;
    }

    header {
      background: linear-gradient(90deg, #0072ce15, #0072ce05);
      padding: 24px 28px;
      border-bottom: 1px solid var(--border);
      display: flex;
      align-items: center;
      justify-content: space-between;
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
      border-radius: 12px;
      padding: 20px;
      box-shadow: 0 6px 18px rgba(15, 23, 42, 0.06);
      display: flex;
      gap: 16px;
      align-items: center;
    }

    .search input {
      flex: 1;
      padding: 14px 16px;
      border: 1px solid var(--border);
      border-radius: 8px;
      font-size: 15px;
    }

    .search button {
      background: var(--accent);
      color: white;
      border: none;
      padding: 12px 16px;
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
      border-radius: 12px;
      padding: 22px;
      box-shadow: 0 6px 18px rgba(15, 23, 42, 0.06);
    }

    .muted {
      color: var(--muted);
      font-size: 14px;
    }

    .img-wrap {
      background: #f9fbff;
      padding: 18px;
      border-radius: 10px;
      border: 1px solid var(--border);
    }

    .meta {
      margin-top: 12px;
      font-size: 15px;
    }

    .label {
      display: inline-block;
      padding: 6px 10px;
      border-radius: 8px;
      background: #e0f0ff;
      color: #004a99;
      font-weight: 700;
    }

    footer {
      max-width: 980px;
      margin: 28px auto;
      color: var(--muted);
      font-size: 13px;
      text-align: center;
    }

    @media (max-width:820px) {
      .content {
        flex-direction: column;
      }

      .right {
        width: 100%;
      }
    }

    #previewCanvas {
      max-width: 100%;
      border-radius: 8px;
      box-shadow: 0 8px 30px rgba(12, 20, 40, 0.06);
      background: white;
    }
  </style>
</head>

<body>
  <header>
    <div class="brand">UNICID — Universidade Cidade de São Paulo</div>
    <div class="muted">Portal de Autenticidade Acadêmica</div>
  </header>

  <div class="container">
    <div class="search" role="search" aria-label="Pesquisar diploma">
      <input id="q" placeholder="Consultar por número do diploma (ex: 46456-64)" aria-label="Número do diploma" />
      <button id="btn">Consultar</button>
    </div>

    <div class="content">
      <div class="left">
        <div class="card" id="result">
          <div class="muted">Resultado da verificação</div>
          <h2 id="name">—</h2>
          <div class="meta" id="degree">—</div>
          <div class="meta" id="inst">—</div>
          <div class="meta" id="date">—</div>
          <div style="margin-top:14px;">
            <span class="label" id="status">—</span>
          </div>
          <hr style="margin:18px 0;border:none;border-top:1px solid #eef2f6" />
          <div class="muted">Número do diploma</div>
          <div id="dnum" style="font-weight:700;margin-top:6px">—</div>
        </div>
      </div>

      <div class="right">
        <div class="card">
          <div class="muted">Visualização</div>
          <div class="img-wrap" style="margin-top:12px;display:flex;justify-content:center">
            <canvas id="previewCanvas" width="420" height="260" role="img"
              aria-label="Pré-visualização do diploma"></canvas>
          </div>
        </div>
      </div>
    </div>
  </div>

  <footer>
    UNICID — Universidade Cidade de São Paulo © 2025  
    <br>Parte do grupo Cruzeiro do Sul Educacional ⭐
  </footer>

  <script>
    const records = {
      '46456-64': {
        diploma: '46456-64',
        name: 'Zilda Felipe da Silva',
        degree: 'Mestrado em Educação',
        inst: 'Universidade de São Paulo — UNICID',
        date: 'Conclusão em 15/03/2025',
        status: 'Ativo'
      },
      '26644-64': {
        diploma: '26644-64',
        name: 'Ivani Felipe da Silva',
        degree: 'Mestrado em Educação',
        inst: 'Universidade de São Paulo — UNICID',
        date: 'Conclusão em 10/03/2025',
        status: 'Ativo'
      },
      '67865-76': {
        diploma: '67865-76',
        name: 'Elisangela Aparecida Ramos',
        degree: 'Bacharel em Fonoaudiologia',
        inst: 'Cruzeiro do Sul Virtual',
        date: 'Conclusão em 16/08/2025',
        status: 'Ativo'
      },
      '54577-75': {
        diploma: '54577-75',
        name: 'Jaqueline Kisandre da Silva Santana',
        degree: 'Mestrado em Educação',
        inst: 'Universidade Unicid São Paulo',
        date: 'Conclusão em 10/03/2025',
        status: 'Ativo'
      },
      '57755-42': {
        diploma: '57755-42',
        name: 'Josilene Santos Ribeiro',
        degree: 'Bacharel em Enfermagem',
        inst: 'Universidade de São Paulo — UNICID',
        date: 'Conclusão em 07/10/2024',
        status: 'Ativo'
      },

      /* Registro adicionado */
      '56765-86': {
        diploma: '56765-86',
        name: 'Tatiane Carla Valdivino Gomes Silva',
        degree: 'Pós-Graduação em Análise do Comportamento Aplicada',
        inst: 'Universidade de São Paulo — UNICID',
        date: 'Conclusão em 04/02/2024',
        status: 'Ativo'
      }
    };

    document.getElementById('btn').addEventListener('click', function () {
      const q = document.getElementById('q').value.trim();
      const result = records[q];

      if (result) {
        document.getElementById('name').innerText = result.name;
        document.getElementById('degree').innerText = result.degree;
        document.getElementById('inst').innerText = result.inst;
        document.getElementById('date').innerText = result.date;
        document.getElementById('status').innerText = result.status;
        document.getElementById('dnum').innerText = result.diploma;
      } else {
        document.getElementById('name').innerText = '—';
        document.getElementById('degree').innerText = '—';
        document.getElementById('inst').innerText = '—';
        document.getElementById('date').innerText = '—';
        document.getElementById('status').innerText = '—';
        document.getElementById('dnum').innerText = '—';
        alert('Diploma não encontrado. Verifique o número digitado.');
      }
    });

    // Desenho do diploma no canvas
    const canvas = document.getElementById('previewCanvas');
    const ctx = canvas.getContext('2d');

    function drawStar(cx, cy, spikes, outerRadius, innerRadius) {
      let rot = Math.PI / 2 * 3;
      let x = cx;
      let y = cy;
      const step = Math.PI / spikes;

      ctx.beginPath();
      ctx.moveTo(cx, cy - outerRadius);
      for (let i = 0; i < spikes; i++) {
        x = cx + Math.cos(rot) * outerRadius;
        y = cy + Math.sin(rot) * outerRadius;
        ctx.lineTo(x, y);
        rot += step;

        x = cx + Math.cos(rot) * innerRadius;
        y = cy + Math.sin(rot) * innerRadius;
        ctx.lineTo(x, y);
        rot += step;
      }
      ctx.lineTo(cx, cy - outerRadius);
      ctx.closePath();
      ctx.fillStyle = "#0072ce";
      ctx.fill();
    }

    // Fundo e texto do diploma
    ctx.fillStyle = "#ffffff";
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    ctx.font = "bold 20px Arial";
    ctx.fillStyle = "#003b73";
    ctx.fillText("UNICID", 160, 50);
    ctx.font = "14px Arial";
    ctx.fillText("Universidade Cidade de São Paulo", 80, 75);
    drawStar(370, 220, 5, 15, 6);
  </script>
</body>

</html>
