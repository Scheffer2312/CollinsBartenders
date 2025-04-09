<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Orçamento - Collins Bartenders</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: linear-gradient(to right, #f2f2f2, #ffffff);
      padding: 30px;
      color: #333;
    }

    h1 {
      text-align: center;
      color: #444;
    }

    form {
      background-color: #fff;
      padding: 30px;
      max-width: 600px;
      margin: 30px auto;
      box-shadow: 0 0 15px rgba(0,0,0,0.1);
      border-radius: 10px;
    }

    label {
      margin-top: 15px;
      display: block;
      font-weight: bold;
    }

    input, select, textarea {
      width: 100%;
      padding: 10px;
      margin-top: 5px;
      border: 1px solid #ccc;
      border-radius: 5px;
      box-sizing: border-box;
    }

    .checkbox-group {
      display: flex;
      gap: 15px;
      margin-top: 10px;
    }

    button {
      padding: 10px 20px;
      margin-top: 20px;
      margin-right: 10px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-weight: bold;
    }

    .gerar {
      background-color: #3498db;
      color: white;
    }

    .aceitar {
      background-color: #2ecc71;
      color: white;
    }

    .recusar {
      background-color: #e74c3c;
      color: white;
    }

    .enviarJustificativa {
      background-color: #f39c12;
      color: white;
    }

    #justificativa-container {
      display: none;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>Solicitação de Orçamento - Collins Bartenders</h1>
  <form id="orcamentoForm">
    <label for="nome">Nome:</label>
    <input type="text" id="nome" required>

    <label for="data">Data do Evento:</label>
    <input type="date" id="data" required>

    <label for="tipoEvento">Tipo de Evento:</label>
    <input type="text" id="tipoEvento" placeholder="Casamento, Aniversário..." required>

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
    <div class="checkbox-group">
      <label><input type="checkbox" id="alcool"> Com Álcool</label>
      <label><input type="checkbox" id="semAlcool"> Sem Álcool</label>
      <label><input type="checkbox" id="ambos"> Ambos</label>
    </div>

    <label for="outrasBebidas">Haverá outras bebidas no local?</label>
    <select id="outrasBebidas">
      <option value="Sim">Sim</option>
      <option value="Não">Não</option>
    </select>

    <label for="whatsappCliente">Seu número de WhatsApp:</label>
    <input type="text" id="whatsappCliente" placeholder="Ex: 65 91234-5678" required>

    <button type="button" class="gerar" onclick="gerarOrcamento()">Gerar Orçamento</button>
    <button type="button" class="aceitar" onclick="enviarWhatsApp(true)">Aceitar Proposta</button>
    <button type="button" class="recusar" onclick="mostrarJustificativa()">Recusar Proposta</button>

    <div id="justificativa-container">
      <label for="justificativa">O que podemos melhorar?</label>
      <textarea id="justificativa" rows="4" placeholder="Digite aqui sua justificativa..."></textarea>
      <button type="button" class="enviarJustificativa" onclick="enviarWhatsApp(false)">Enviar Justificativa</button>
    </div>
  </form>

  <script>
    function gerarOrcamento() {
      alert("Seu orçamento será gerado com base nas informações preenchidas.");
    }

    function montarMensagem() {
      const nome = document.getElementById("nome").value;
      const data = document.getElementById("data").value;
      const tipoEvento = document.getElementById("tipoEvento").value;
      const localEvento = document.getElementById("localEvento").value;
      const convidados = document.getElementById("convidados").value;
      const pacote = document.getElementById("pacote").value;
      const alcool = document.getElementById("alcool").checked ? "Sim" : "Não";
      const semAlcool = document.getElementById("semAlcool").checked ? "Sim" : "Não";
      const ambos = document.getElementById("ambos").checked ? "Sim" : "Não";
      const outrasBebidas = document.getElementById("outrasBebidas").value;
      const whatsappCliente = document.getElementById("whatsappCliente").value;
      const justificativa = document.getElementById("justificativa").value;

      let mensagem = "Olá! Recebi uma solicitação de orçamento:\n\n";
      mensagem += `Nome: ${nome}\n`;
      mensagem += `Data do Evento: ${data}\n`;
      mensagem += `Tipo de Evento: ${tipoEvento}\n`;
      mensagem += `Local do Evento: ${localEvento}\n`;
      mensagem += `Número de Convidados: ${convidados}\n`;
      mensagem += `Pacote Escolhido: ${pacote}\n`;
      mensagem += `Drinks com Álcool: ${alcool}\n`;
      mensagem += `Drinks sem Álcool: ${semAlcool}\n`;
      mensagem += `Ambos: ${ambos}\n`;
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
  </script>
</body>
</html>
