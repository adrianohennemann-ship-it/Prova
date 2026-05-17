<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerador de Provas Automático</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; background-color: #f4f4f9; color: #333; }
        .container { max-width: 600px; margin: auto; background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        .form-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 5px; font-weight: bold; }
        input, select, textarea { width: 100%; padding: 8px; box-sizing: border-box; border: 1px solid #ccc; border-radius: 4px; }
        button { background-color: #007bff; color: white; padding: 10px 15px; border: none; border-radius: 4px; cursor: pointer; width: 100%; font-size: 16px; }
        button:hover { background-color: #0056b3; }
        #resultado { margin-top: 20px; white-space: pre-wrap; background: #e9ecef; padding: 15px; border-radius: 4px; display: none; }
        .btn-print { background-color: #28a745; margin-top: 10px; display: none; }
    </style>
</head>
<body>

<div class="container">
    <h2>📝 Gerador de Provas</h2>
    
    <div class="form-group">
        <label for="apiKey">Sua API Key do Gemini (Cole aqui):</label>
        <input type="password" id="apiKey" placeholder="AIzaSy...">
    </div>
    <hr>

    <div class="form-group">
        <label for="nomeEscola">Nome da Escola / Instituição:</label>
        <input type="text" id="nomeEscola" placeholder="Ex: Colégio Machado de Assis">
    </div>
    <div class="form-group">
        <label for="nomeProfessor">Nome do Professor(a):</label>
        <input type="text" id="nomeProfessor" placeholder="Ex: Prof. Carlos Silva">
    </div>
    <div class="form-group">
        <label for="assunto">Assunto Abordado:</label>
        <input type="text" id="assunto" placeholder="Ex: Fotossíntese e Respiração Celular">
    </div>

    <div class="form-group">
        <label for="qtdQuestoes">Quantidade de Questões:</label>
        <input type="number" id="qtdQuestoes" min="1" max="20" value="5">
    </div>
    <div class="form-group">
        <label for="pesoQuestao">Peso / Valor de cada questão:</label>
        <input type="number" id="pesoQuestao" step="0.1" value="2.0">
    </div>
    <div class="form-group">
        <label for="tipoQuestao">Tipo de Questão:</label>
        <select id="tipoQuestao">
            <option value="objetiva">Objetiva (Múltipla Escolha: A, B, C, D, E)</option>
            <option value="dissertativa">Dissertativa (Resposta Aberta)</option>
        </select>
    </div>

    <div class="form-group">
        <label for="conteudoBase">Cole aqui o texto/conteúdo base para criar as questões:</label>
        <textarea id="conteudoBase" rows="6" placeholder="Cole o texto de apoio, capítulos de livro ou anotações aqui..."></textarea>
    </div>

    <button onclick="gerarProva()">Gerar Prova com IA</button>
    <button class="btn-print" id="btnPrint" onclick="imprimirProva()">Imprimir Prova</button>

    <div id="resultado"></div>
</div>

<script src="script.js"></script>
</body>
</html>
