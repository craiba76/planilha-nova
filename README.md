<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Cadastro de Fornecedores</title>
  <style>
    table {
      border-collapse: collapse;
      width: 100%;
      margin-top: 20px;
    }
    th, td {
      border: 1px solid #ccc;
      padding: 10px;
      text-align: left;
    }
    th {
      background-color: #f4f4f4;
    }
    td[contenteditable="true"] {
      background-color: #fffbea;
    }
    button {
      padding: 6px 12px;
      margin: 5px 0;
    }
  </style>
</head>
<body>
  <h1>Cadastro de Fornecedores</h1>
  <button onclick="adicionarFornecedor()">Adicionar Fornecedor</button>

  <table id="tabelaFornecedores">
    <thead>
      <tr>
        <th>Nome</th>
        <th>Telefone</th>
        <th>E-mail</th>
        <th>Obs</th>
        <th>Ações</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <script>
    function adicionarFornecedor(nome = '', telefone = '', email = '', obs = '') {
      const tabela = document.getElementById("tabelaFornecedores").getElementsByTagName('tbody')[0];
      const novaLinha = tabela.insertRow();

      const celNome = novaLinha.insertCell();
      celNome.contentEditable = true;
      celNome.innerText = nome;

      const celTelefone = novaLinha.insertCell();
      celTelefone.contentEditable = true;
      celTelefone.innerText = telefone;

      const celEmail = novaLinha.insertCell();
      celEmail.contentEditable = true;
      celEmail.innerText = email;

      const celObs = novaLinha.insertCell();
      celObs.contentEditable = true;
      celObs.innerText = obs;

      const celAcoes = novaLinha.insertCell();
      const botaoExcluir = document.createElement("button");
      botaoExcluir.innerText = "Excluir";
      botaoExcluir.onclick = () => {
        novaLinha.remove();
        salvarFornecedores();
      };
      celAcoes.appendChild(botaoExcluir);

      novaLinha.addEventListener('input', salvarFornecedores);
      salvarFornecedores();
    }

    function salvarFornecedores() {
      const linhas = document.getElementById("tabelaFornecedores").getElementsByTagName("tbody")[0].rows;
      const dados = [];

      for (let i = 0; i < linhas.length; i++) {
        const celulas = linhas[i].cells;
        dados.push({
          nome: celulas[0].innerText,
          telefone: celulas[1].innerText,
          email: celulas[2].innerText,
          obs: celulas[3].innerText,
        });
      }

      localStorage.setItem("fornecedores", JSON.stringify(dados));
    }

    function carregarFornecedores() {
      const dados = JSON.parse(localStorage.getItem("fornecedores")) || [];
      dados.forEach(f => adicionarFornecedor(f.nome, f.telefone, f.email, f.obs));
    }

    window.addEventListener("load", carregarFornecedores);
  </script>
</body>
</html>
