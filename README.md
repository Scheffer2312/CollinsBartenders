<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Orçamento - Collins Bartenders</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f9f9f9;
      padding: 20px;
    }

    .container {
      max-width: 600px;
      margin: auto;
      background: #fff;
      padding: 25px;
      border-radius: 8px;
      box-shadow: 0 0 15px rgba(0,0,0,0.1);
    }

    h1 {
      text-align: center;
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
      margin-bottom: 10px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }

    .radio-group {
      display: flex;
      gap: 10px;
      margin-top: 5px;
    }

    .radio-group label {
      font-weight: normal;
    }

    button {
      margin-top: 15px;
      width: 100%;
      padding: 12px;
      background-color: #000;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
    }

    .button-secondary {
      background-color: #555;
      margin-top: 10px;
    }

    #resultado {
      margin-top: 20px;
      padding: 15px;
      background-color: #eee;
      border-radius: 5px;
    }

    #justificativa-container {
      display: none;
      margin-top: 15px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Orçamento de Bar</h1>

    <label for="data">Data do evento:</label>
    <input type="date" id="data" required>

    <label for="local">Local do evento:</label>
    <input type="text" id="local" placeholder="Ex: Salão do Edifício, Chácara do Sol..." required>

    <label for="tipoEvento">Tipo de evento:</label>
    <select id="tipoEvento">
      <option value="Casamento">Casamento</option>
      <option value="Aniversário">Aniversário</option>
      <option value="Corporativo">Corporativo</option>
      <option value="Outro">Outro</option>
    </select>

    <label for="convidados">Quantidade de convidados:</label>
    <input type="number" id="convidados" min="10" max="150" step="10" required>

    <label for="pacote">Escolha o pacote:</label>
    <select id="pacote">
      <option value="basico">Básico</option>
      <option value="padrao">Padrão</option>
      <option value="premium">Premium</option>
    </select>

    <label>Tipo de drinks desejado:</label>
    <div class="radio-group">
      <label><input type="radio" name="tipoDrink" value="Com álcool" checked> Com álcool</label>
      <label><input type="radio" name="tipoDrink" value="Sem álcool"> Sem álcool</label>
      <label><input type="radio" name="tipoDrink" value="Ambos"> Ambos</label>
    </div>

    <label>Haverá outras bebidas no local?</label>
    <div class="radio-group">
      <label><input type="radio" name="outrasBebidas" value="Sim" checked> Sim</label>
      <label><input type="radio" name="outrasBebidas" value="Não"> Não</label>
    </div>

    <label for="whatsCliente">Seu número do WhatsApp:</label>
    <input type="tel" id="whatsCliente" placeholder="(DDD) 99999-9999" required>

    <button onclick="gerarOrcamento()">Gerar orçamento</button>

    <div id="resultado"></div>

    <button id="btnAceitar" onclick="aceitarProposta()">Aceitar Proposta</button>
    <button class="button-secondary" onclick="mostrarJustificativa()">Recusar Proposta</button>

    <div id="justificativa-container">
      <label for="justificativa">O que podemos melhorar para fechar com você?</label>
      <textarea id="justificativa" rows="4" placeholder="Escreva aqui..."></textarea>
      <button class="button-secondary" onclick="enviarJustificativa()">Enviar Resposta</button>
    </div>
  </div>

  <script>
    const pacotes = {
      basico: [
        {min: 10, max: 30, valor: 1100, detalhes: "4h Open, 1 bartender, até 8 drinks, copos descartáveis"},
        {min: 40, max: 50, valor: 1400, detalhes: "4h Open, 2 bartenders, até 8 drinks, copos descartáveis"},
        {min: 60, max: 70, valor: 1600, detalhes: "4h Open, 2 bartenders, até 8 drinks, copos descartáveis"},
        {min: 80, max: 90, valor: 1800, detalhes: "4h Open, 2 bartenders, até 8 drinks, copos descartáveis"},
        {min: 100, max: 110, valor: 2000, detalhes: "4h Open, 2 bartenders, até 8 drinks, copos descartáveis"},
        {min: 120, max: 130, valor: 2400, detalhes: "4h Open, 3 bartenders, até 8 drinks, copos descartáveis"},
        {min: 140, max: 150, valor: 2800, detalhes: "4h Open, 4 bartenders, até 8 drinks, copos descartáveis"},
      ],
      padrao: [
        {min: 10, max: 30, valor: 1300, detalhes: "4h Open, 1 bartender, 1 bar-back, até 8 drinks, copos/taças de vidro e canecas"},
        {min: 40, max: 50, valor: 1600, detalhes: "4h Open, 2 bartenders, 1 bar-back, até 8 drinks, copos/taças de vidro e canecas"},
        {min: 60, max: 70, valor: 1800, detalhes: "4h Open, 2 bartenders, 1 bar-back, até 8 drinks, copos/taças de vidro e canecas"},
        {min: 80, max: 90, valor: 2000, detalhes: "4h Open, 2 bartenders, 1 bar-back, até 8 drinks, copos/taças de vidro e canecas"},
        {min: 100, max: 110, valor: 2200, detalhes: "4h Open, 2 bartenders, 1 bar-back, até 8 drinks, copos/taças de vidro e canecas"},
        {min: 120, max: 130, valor: 2600, detalhes: "4h Open, 3 bartenders, 1 bar-back, até 8 drinks, copos/taças de vidro e canecas"},
        {min: 140, max: 150, valor: 3200, detalhes: "4h Open, 4 bartenders, 2 bar-back, até 8 drinks, copos/taças de vidro e canecas"},
      ],
      premium: [
        {min: 10, max: 30, valor: 1900, detalhes: "4h Open, 1 bartender, 1 bar-back, até 8 drinks, copos/taças de vidro e canecas"},
        {min: 40, max: 50, valor: 2600, detalhes: "4h Open, 2 bartenders, 1 bar-back, até 8 drinks, copos/taças de vidro e canecas"},
        {min: 60, max: 70, valor: 2800, detalhes: "4h Open, 2 bartenders, 1 bar-back, até 8 drinks, copos/taças de vidro e canecas"},
        {min: 80, max: 90, valor: 3000, detalhes: "4h Open, 2 bartenders, 1 bar-back, até 8 drinks, copos/taças de vidro e canecas"},
        {min: 100, max: 110, valor: 3300, detalhes: "4h Open, 2 bartenders, 1 bar-back, até 8 drinks, copos/taças de vidro e canecas"},
        {min: 120, max: 130, valor: 3900, detalhes: "4h Open, 3 bartenders, 1 bar-back, até 8 drinks, copos/taças de vidro e canecas"},
        {min: 140, max: 150, valor: 4700, detalhes: "4h Open, 4 bartenders, 2 bar-back, até 8 drinks, copos/taças de vidro e canecas"},
      ]
    };

    let ultimoOrcamento = "";

    function gerarOrcamento() {
      const data = document.getElementById("data").value;
      const local = document.getElementById("local").value;
      const tipoEvento = document.getElementById("tipoEvento").value;
      const convidados = parseInt(document.getElementById("convidados").value);
      const pacote = document.getElementById("pacote").value;
      const tipoDrink = document.querySelector('input[name="tipoDrink"]:checked').value;
      const outrasBebidas = document.querySelector('input[name="outrasBebidas"]:checked').value;
      const numeroCliente = document.getElementById("whatsCliente").value;

      const opcoes = pacotes[pacote];
      const faixa = opcoes.find(o => convidados >= o.min && convidados <= o.max);

      const resultado = document.getElementById("resultado");

      if (faixa) {
        ultimoOrcamento = `Data: ${data}\nLocal: ${local}\nTipo de Evento: ${tipoEvento}\nPacote: ${pacote}\nConvidados: ${convidados}\nTipo de drinks: ${tipoDrink}\nOutras bebidas: ${outrasBebidas}\nWhatsApp do cliente: ${numeroCliente}\nValor: R$ ${faixa.valor.toFixed(2).replace(".", ",")}\nDetalhes: ${faixa.detalhes}`;
        resultado.innerHTML = `
          <strong>Data:</strong> ${data}<br>
          <strong>Local:</strong> ${local}<br>
          <strong>Tipo de Evento:</strong> ${tipoEvento}<br>
          <strong>Pacote:</strong> ${pacote}<br>
          <strong>Convidados:</strong> ${convidados}<br>
          <strong>Tipo de drinks:</strong> ${tipoDrink}<br>
          <strong>Outras bebidas:</strong> ${outrasBebidas}<br>
          <strong>WhatsApp:</strong> ${numeroCliente}<br>
          <strong>Valor:</strong> R$ ${faixa.valor.toFixed(2).replace(".", ",")}<br>
          <strong>Detalhes:</strong> ${faixa.detalhes}
        `;
      } else {
        resultado.innerHTML = "Número de convidados fora da faixa atendida.";
        ultimoOrcamento = "";
      }
    }

    function aceitarProposta() {
      if (!ultimoOrcamento) {
        alert("Por favor, gere o orçamento primeiro.");
        return;
      }
      const url = `https://wa.me/5599999999999?text=${encodeURIComponent("Proposta aceita!\n\n" + ultimoOrcamento)}`;
      window.open(url, "_blank");
    }

    function mostrarJustificativa() {
      document.getElementById("justificativa-container").style.display = "block";
    }

    function enviarJustificativa() {
      const justificativa = document.getElementById("justificativa").value.trim();
      if (!justificativa) {
        alert("Por favor, preencha o campo com sua sugestão.");
        return;
      }
      const url = `https://wa.me/5599999999999?text=${encodeURIComponent("Proposta recusada.\n\n" + ultimoOrcamento + "\n\nMotivo do cliente:\n" + justificativa)}`;
      window.open(url, "_blank");
    }
  </script>
</body>
</html>
