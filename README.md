<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Orçamento - Collins Bartenders</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      padding: 0;
      margin: 0;
      background-color: #f4f4f4;
      color: #333;
    }

    header {
      background-color: #000;
      padding: 20px;
      text-align: center;
    }

    header img.logo {
      height: 80px;
    }

    .hero {
      width: 100%;
      max-height: 400px;
      overflow: hidden;
    }

    .hero img {
      width: 100%;
      object-fit: cover;
    }

    .container {
      max-width: 700px;
      margin: 30px auto;
      background-color: white;
      padding: 30px;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }

    h1 {
      text-align: center;
      color: #000;
      margin-bottom: 20px;
    }

    label, select, input, textarea {
      display: block;
      margin-top: 15px;
      width: 100%;
    }

    input, select, textarea {
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 4px;
      font-size: 14px;
    }

    .radio-group {
      display: flex;
      gap: 10px;
      margin-top: 10px;
    }

    .radio-group label {
      display: flex;
      align-items: center;
      gap: 5px;
    }

    button {
      padding: 12px 20px;
      margin-top: 20px;
      margin-right: 10px;
      background-color: #000;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
    }

    button:hover {
      background-color: #444;
    }

    #justificativa-container {
      margin-top: 20px;
      display: none;
    }

    #resumoOrcamento {
      background-color: #f0f0f0;
      padding: 15px;
      border-radius: 5px;
      margin-top: 20px;
      display: none;
    }

    footer {
      text-align: center;
      margin: 40px 0 20px;
      font-size: 14px;
      color: #777;
    }
  </style>
</head>
<body>

  <header>
    <img src="/mnt/data/Logo.JPG" alt="Logo Collins Bartenders" class="logo" />
  </header>

  <div class="hero">
    <img src="/mnt/data/Balcao.JPG" alt="Balcão de Bar" />
  </div>

  <div class="container">
    <h1>Solicitação de Orçamento</h1>
    <form id="orcamentoForm">
      <label for="nome">Nome:</label>
      <input type="text" id="nome" required>

      <label for="data">Data do Evento:</label>
      <input type="date" id="data" required>

      <label for="tipoEvento">Tipo de Evento:</label>
      <input type="text" id="tipoEvento" placeholder="Casamento, Aniversário, etc." required>

      <label for="localEvento">Local do Evento:</label>
      <input type="text" id="localEvento" required>

      <label for="convidados">Número de Convidados:</label>
      <input type="number" id="convidados" required>

      <label for="pacote">Pacote:</label>
      <select id="pacote" required>
        <option value="Básico">Básico</option>
        <option value="Padrão">Padrão</option>
        <option value="Premium">Premium</option>
      </select>

      <label>Tipo de Drinks:</label>
      <div class="radio-group">
        <label><input type="radio" name="tipoDrink" value="Com Álcool" required>Com Álcool</label>
        <label><input type="radio" name="tipoDrink" value="Sem Álcool">Sem Álcool</label>
        <label><input type="radio" name="tipoDrink" value="Ambos">Ambos</label>
      </div>

      <label for="outrasBebidas">Haverá outras bebidas no local?</label>
      <select id="outrasBebidas">
        <option value="Sim">Sim</option>
        <option value="Não">Não</option>
      </select>

      <label for="whatsappCliente">Seu número de WhatsApp:</label>
      <input type="text" id="whatsappCliente" placeholder="Ex: 65 91234-5678" required>

      <button type="button" onclick="gerarResumo()">Gerar Orçamento</button>
      <button type="button" onclick="enviarWhatsApp(true)">Aceitar Proposta</button>
      <button type="button" onclick="mostrarJustificativa()">Recusar Proposta</button>

      <div id="justificativa-container">
        <label for="justificativa">O que podemos melhorar?</label>
        <textarea id="justificativa" rows="4" placeholder="Digite aqui sua justificativa..."></textarea>
        <button type="button" onclick="enviarWhatsApp(false)">Enviar Justificativa</button>
      </div>

      <div id="resumoOrcamento"></div>
    </form>
  </div>

  <footer>
    &copy; 2025 Collins Bartenders | contato: (65) 99288-7213
  </footer>

  <script>
    function montarMensagem() {
      const nome = document.getElementById("nome").value;
      const data = document.getElementById("data").value;
      const tipoEvento = document.getElementById("tipoEvento").value;
      const localEvento = document.getElementById("localEvento").value;
      const convidados = document.getElementById("convidados").value;
      const pacote = document.getElementById("pacote").value;
      const tipoDrink = document.querySelector('input[name="tipoDrink"]:checked')?.value || "Não informado";
      const outrasBebidas = document.getElementById("outrasBebidas").value;
      const whatsappCliente = document.getElementById("whatsappCliente").value;
      const justificativa = document.getElementById("justificativa").value;

      let mensagem = "Olá! Recebi uma solicitação de orçamento:\n\n";
      mensagem += `Nome: ${nome}\n`;
      mensagem += `Data do Evento: ${data}\n`;
      mensagem += `Tipo de Evento: ${tipoEvento}\n`;
      mensagem += `Local do Evento: ${localEvento}\n`;
      mensagem += `Convidados: ${convidados}\n`;
      mensagem += `Pacote: ${pacote}\n`;
      mensagem += `Tipo de Drinks: ${tipoDrink}\n`;
      mensagem += `Outras bebidas no local: ${outrasBebidas}\n`;
      mensagem += `WhatsApp do cliente: ${whatsappCliente}\n`;

      if (justificativa) {
        mensagem += `\nMotivo da recusa: ${justificativa}`;
      }

      return encodeURIComponent(mensagem);
    }

    function enviarWhatsApp(aceito) {
      const numeroDestino = "5565992887213";
      const mensagem = montarMensagem();
      const link = "https://wa.me/" + numeroDestino + "?text=" + mensagem;
      window.open(link, "_blank");
    }

    function mostrarJustificativa() {
      document.getElementById("justificativa-container").style.display = "block";
    }

    function gerarResumo() {
      const nome = document.getElementById("nome").value;
      const convidados = document.getElementById("convidados").value;
      const pacote = document.getElementById("pacote").value;

      // (Exemplo de valores fictícios — pode ser integrado com regras reais se quiser)
      let valor = 0;
      if (pacote === "Básico") {
        valor = convidados * 25;
      } else if (pacote === "Padrão") {
        valor = convidados * 40;
      } else if (pacote === "Premium") {
        valor = convidados * 60;
      }

      const resumo = document.getElementById("resumoOrcamento");
      resumo.style.display = "block";
      resumo.innerText = `Olá, ${nome}! O valor estimado para ${convidados} convidados no pacote ${pacote} é de R$ ${valor.toLocaleString('pt-BR', {minimumFractionDigits: 2})}.`;
    }
  </script>

</body>
</html>
