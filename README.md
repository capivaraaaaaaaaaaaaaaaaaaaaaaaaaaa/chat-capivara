# chat-capivara
chat ia
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IA Conversacional com Google</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f4f4f9;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .chat-container {
            width: 400px;
            background: white;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        .messages {
            flex-grow: 1;
            overflow-y: auto;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            max-height: 300px;
        }
        .message {
            margin: 5px 0;
        }
        .user {
            text-align: right;
            color: blue;
        }
        .bot {
            text-align: left;
            color: green;
        }
        input[type="text"] {
            padding: 10px;
            width: calc(100% - 22px);
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        button {
            padding: 10px;
            background: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background: #0056b3;
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <div class="messages" id="messages"></div>
        <input type="text" id="userInput" placeholder="Digite sua mensagem">
        <button onclick="sendMessage()">Enviar</button>
    </div>

    <script>
        const messagesDiv = document.getElementById("messages");
        const userInput = document.getElementById("userInput");

        // Função para enviar mensagens
        function sendMessage() {
            const userMessage = userInput.value.trim();
            if (!userMessage) return;

            addMessage(userMessage, "user");
            userInput.value = "";

            // Enviar pergunta para a IA
            getResponseFromAPI(userMessage);
        }

        // Adiciona mensagens ao chat
        function addMessage(text, sender) {
            const messageDiv = document.createElement("div");
            messageDiv.className = `message ${sender}`;
            messageDiv.textContent = text;
            messagesDiv.appendChild(messageDiv);
            messagesDiv.scrollTop = messagesDiv.scrollHeight;
        }

        // Busca resposta do Google Custom Search API
        async function getResponseFromAPI(query) {
            const apiKey = "SUA_CHAVE_DE_API_AQUI"; // Substitua pela sua chave
            const cx = "SEU_ID_DO_MOTOR_DE_BUSCA_AQUI"; // Substitua pelo ID do mecanismo
            const apiURL = `https://www.googleapis.com/customsearch/v1?q=${encodeURIComponent(query)}&key=${apiKey}&cx=${cx}`;

            try {
                const response = await fetch(apiURL);
                const data = await response.json();

                if (data.items && data.items.length > 0) {
                    const topResult = data.items[0].snippet;
                    addMessage(topResult, "bot");
                } else {
                    addMessage("Desculpe, não encontrei nada relevante.", "bot");
                }
            } catch (error) {
                addMessage("Erro ao buscar informações. Tente novamente mais tarde.", "bot");
            }
        }
    </script>
</body>
</html>
