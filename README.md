<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Formulário de Cadastro</title>
  <!DOCTYPE html>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #015958;
      margin: 0;
      padding: 20px;
      display: flex;
      justify-content: center;
    }
    .container {
      background: #008F8C;
      padding: 30px;
      border-radius: 14px;
      max-width: 620px;
      width: 100%;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    }
    h2 {
      text-align: center;
    }
    input {
      width: 80%;
      padding: 14px;
      border: 3px solid #928484;
      border-radius: 10px;
    }
    button {
      padding: 12px;
      border: none;
      cursor: pointer;
      border-radius: 8px;
    }
    .addBtn {
      background: #28a745;
      color: white;
    }
    .editBtn {
      background: #0177f5;
      color: white;
      margin-left: 5px;
    }
    .deleteBtn {
      background: #ff0019;
      color: white;
      margin-left: 6px;
    }
    ul {
      list-style: none;
      padding: 0;
      margin-top: 23px;
    }
    li {
      background: #ffffff;
      padding: 12px;
      margin-bottom: 13px;
      border-radius: 9px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .feedback {
      margin-top: 12px;
      text-align: center;
      font-weight: bold;
      color: rgb(0, 255, 0);
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>LISTA DE TAREFAS</h2>
    <input type="text" id="itemInput" placeholder="Digite uma tarefa...">
    <button class="addBtn" onclick="adicionarItem()">Adicionar</button>
    <p id="feedback" class="feedback"></p>
    <ul id="lista"></ul>
  </div>

  <script>
    let itens = [];

    // Carregar dados ao iniciar
    window.onload = function() {
      carregarLocalStorage();
      listarItens();
    }

    // Função para salvar no localStorage
    function salvarLocalStorage() {
      localStorage.setItem("itens", JSON.stringify(itens));
    }

    // Função para carregar do localStorage
    function carregarLocalStorage() {
      let dados = localStorage.getItem("itens");
      if (dados) {
        itens = JSON.parse(dados);
      }
    }

    // Função para listar itens na tela
    function listarItens() {
      let lista = document.getElementById("lista");
      lista.innerHTML = "";
      itens.forEach((item, index) => {
        let li = document.createElement("li");
        li.innerHTML = `
          <span>${item}</span>
          <div>
            <button class="editBtn" onclick="editarItem(${index})">Editar</button>
            <button class="deleteBtn" onclick="removerItem(${index})">Excluir</button>
          </div>
        `;
        lista.appendChild(li);
      });
    }

    // Função para adicionar item
    function adicionarItem() {
      let input = document.getElementById("itemInput");
      let valor = input.value.trim();

      if (valor === "") {
        mostrarFeedback("⚠️ Digite um item válido!", "red");
        return;
      }
      if (itens.includes(valor)) {
        mostrarFeedback("⚠️ Esse item já existe!", "red");
        return;
      }

      itens.push(valor);
      salvarLocalStorage();
      listarItens();
      mostrarFeedback("✅ Item adicionado com sucesso!");
      input.value = "";
    }

    // Função para remover item
    function removerItem(index) {
      itens.splice(index, 1);
      salvarLocalStorage();
      listarItens();
      mostrarFeedback("Item removido!");
    }

    // Função para editar item
    function editarItem(index) {
      let novoValor = prompt("Edite o item:", itens[index]);
      if (novoValor !== null && novoValor.trim() !== "") {
        itens[index] = novoValor.trim();
        salvarLocalStorage();
        listarItens();
        mostrarFeedback("Item editado com sucesso!");
      }
    }

    // Função para mostrar feedback
    function mostrarFeedback(msg, cor = "green") {
      let feedback = document.getElementById("feedback");
      feedback.style.color = cor;
      feedback.textContent = msg;
      setTimeout(() => { feedback.textContent = ""; }, 2000);
    }
  </script>
</body>
</html>
