<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tabela de Investimentos</title>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&family=Press+Start+2P&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Roboto', sans-serif;
      background-color: #1e1e1e;
      color: #f1f1f1;
      margin: 0;
      padding: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    /* Banner animado */
    .banner {
      width: 100%;
      background: linear-gradient(45deg, #ffcc00, #ff6600, #ffcc00);
      background-size: 300% 300%;
      animation: gradientMove 5s ease infinite;
      padding: 20px 0;
      text-align: center;
      color: white;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
    }
    .banner h1 {
      font-family: 'Press Start 2P', cursive;
      font-size: 2.5em;
      text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.7);
      margin: 0;
    }
    @keyframes gradientMove {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    /* Grid para números */
    .grid {
      display: grid;
      grid-template-columns: repeat(20, 1fr); /* 20 colunas */
      gap: 10px;
      width: 90%;
      max-width: 1200px;
      margin: 20px 0;
    }

    /* Blocos da tabela */
    .block {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 50px;
      border: 2px solid #444;
      border-radius: 5px;
      background-color: #333;
      font-size: 18px;
      font-weight: bold;
      color: #bbb;
      cursor: pointer;
      transition: transform 0.2s, background-color 0.2s, box-shadow 0.2s;
    }
    .block:hover {
      transform: scale(1.1);
      box-shadow: 0 4px 8px rgba(255, 204, 0, 0.4);
    }
    .block.marked {
      background-color: #ffcc00;
      color: #1e1e1e;
      border-color: #ffcc00;
    }

    /* Box de total */
    .total-box {
      margin-top: 20px;
      padding: 10px 20px;
      border: 2px solid #ffcc00;
      background-color: #333;
      color: #ffcc00;
      font-size: 1.2em;
      font-weight: bold;
      border-radius: 5px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }

    /* Rodapé */
    .footer {
      margin: 20px;
      text-align: center;
      color: #ccc;
    }

    /* Ajustes para dispositivos móveis */
    @media (max-width: 768px) {
      .grid {
        grid-template-columns: repeat(5, 1fr); /* 5 colunas no celular */
        gap: 8px;
      }
      .block {
        height: 40px;
        font-size: 16px;
      }
      .banner h1 {
        font-size: 2em;
      }
    }

    @media (max-width: 480px) {
      .grid {
        grid-template-columns: repeat(4, 1fr); /* 4 colunas em telas bem pequenas */
        gap: 5px;
      }
      .block {
        height: 35px;
        font-size: 14px;
      }
      .banner h1 {
        font-size: 1.8em;
      }
    }
  </style>
</head>
<body>
  <div class="banner">
    <h1>Tabela de Investimentos</h1>
  </div>
  <div class="grid" id="grid"></div>
  <div class="total-box" id="totalBox">Total: R$0,00</div>
  <div class="footer">
    Clique nos blocos para marcar ou desmarcar. O total será atualizado automaticamente e salvo.
  </div>

  <script>
    const grid = document.getElementById('grid');
    const totalBox = document.getElementById('totalBox');
    const markedBlocks = JSON.parse(localStorage.getItem('markedBlocks')) || []; // Recupera marcações salvas
    let totalValue = 0;

    // Atualiza o total no box
    const updateTotal = () => {
      totalValue = markedBlocks.reduce((sum, value) => sum + value, 0);
      totalBox.textContent = `Total: R$${totalValue.toFixed(2).replace('.', ',')}`;
    };

    // Criação dinâmica dos blocos de 1 a 200
    for (let i = 1; i <= 200; i++) {
      const block = document.createElement('div');
      block.classList.add('block');
      block.textContent = i;

      // Restaura os blocos marcados ao carregar
      if (markedBlocks.includes(i)) {
        block.classList.add('marked');
      }

      // Evento de clique para marcar/desmarcar
      block.addEventListener('click', () => {
        if (block.classList.contains('marked')) {
          block.classList.remove('marked');
          const index = markedBlocks.indexOf(i);
          markedBlocks.splice(index, 1); // Remove o número do array
        } else {
          block.classList.add('marked');
          markedBlocks.push(i); // Adiciona o número ao array
        }

        // Salva as mudanças e atualiza o total
        localStorage.setItem('markedBlocks', JSON.stringify(markedBlocks));
        updateTotal();
      });

      grid.appendChild(block);
    }

    // Atualiza o total ao carregar a página
    updateTotal();
  </script>
</body>
</html>
