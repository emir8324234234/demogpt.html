

<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Demo GPT - Gerçek AI Sohbet</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 10px;
        }

        .container {
            width: 100%;
            max-width: 800px;
            height: 90vh;
            background: white;
            border-radius: 15px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            text-align: center;
            font-size: 24px;
            font-weight: bold;
            position: relative;
        }

        .api-status {
            font-size: 12px;
            position: absolute;
            top: 5px;
            right: 10px;
            padding: 5px 10px;
            background: rgba(255,255,255,0.2);
            border-radius: 5px;
        }

        .api-status.connected {
            background: #4caf50;
        }

        .api-status.disconnected {
            background: #f44336;
        }

        .chat-messages {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .message {
            display: flex;
            gap: 10px;
            animation: slideIn 0.3s ease-out;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .message.user {
            justify-content: flex-end;
        }
sdfsdfsdfcvjl
        .message-content {
            max-width: 70%;
            padding: 12px 16px;
            border-radius: 12px;
            word-wrap: break-word;
            line-height: 1.5;
        }

        .message.user .message-content {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border-bottom-right-radius: 4px;
        }

        .message.bot .message-content {
            background: #f0f0f0;
            color: #333;
            border-bottom-left-radius: 4px;
        }

        .avatar {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: white;
            flex-shrink: 0;
        }

        .message.user .avatar {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }

        .message.bot .avatar {
            background: #764ba2;
        }

        .input-area {
            padding: 15px;
            border-top: 1px solid #e0e0e0;
            display: flex;
            gap: 10px;
        }

        input {
            flex: 1;
            padding: 12px 15px;
            border: 1px solid #e0e0e0;
            border-radius: 25px;
            font-size: 14px;
            outline: none;
            transition: border-color 0.3s;
        }

        input:focus {
            border-color: #667eea;
        }

        input:disabled {
            background: #f5f5f5;
            cursor: not-allowed;
        }

        button {
            padding: 12px 25px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-weight: bold;
            transition: transform 0.2s;
        }

        button:hover:not(:disabled) {
            transform: scale(1.05);
        }

        button:active:not(:disabled) {
            transform: scale(0.95);
        }

        button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }

        .typing-indicator {
            display: flex;
            gap: 5px;
            padding: 10px;
        }

        .typing-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: #764ba2;
            animation: typing 1.4s infinite;
        }

        .typing-dot:nth-child(2) {
            animation-delay: 0.2s;
        }

        .typing-dot:nth-child(3) {
            animation-delay: 0.4s;
        }

        @keyframes typing {
            0%, 60%, 100% {
                opacity: 0.5;
                transform: translateY(0);
            }
            30% {
                opacity: 1;
                transform: translateY(-10px);
            }
        }

        .setup-modal {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.5);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        .setup-content {
            background: white;
            padding: 30px;
            border-radius: 15px;
            max-width: 500px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
        }

        .setup-content h2 {
            margin-bottom: 20px;
            color: #667eea;
        }

        .setup-content p {
            margin-bottom: 15px;
            color: #666;
            line-height: 1.6;
        }

        .setup-content a {
            color: #667eea;
            text-decoration: none;
            font-weight: bold;
        }

        .setup-content input {
            width: 100%;
            padding: 12px 15px;
            margin-bottom: 15px;
            border: 1px solid #e0e0e0;
            border-radius: 8px;
            font-size: 14px;
        }

        .setup-content button {
            width: 100%;
            padding: 12px;
            margin-bottom: 10px;
        }

        .error-message {
            color: #f44336;
            padding: 10px;
            background: #ffebee;
            border-radius: 5px;
            margin-bottom: 10px;
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            💬 Demo GPT
            <div class="api-status disconnected" id="apiStatus">Bağlı Değil</div>
        </div>
        <div class="chat-messages" id="chatMessages"></div>
        <div class="input-area">
            <input 
                type="text" 
                id="messageInput" 
                placeholder="Bir şeyler yazın..."
                autocomplete="off"
            >
            <button id="sendBtn">Gönder</button>
        </div>
    </div>

    <!-- API Key Setup Modal -->
    <div class="setup-modal" id="setupModal">
        <div class="setup-content">
            <h2>🔑 Gemini API Key Gerekli</h2>
            <p>Demo GPT'yi kullanmak için Google Gemini API Key gerekir.</p>
            
            <p><strong>Adım 1:</strong> Şu linke git ve API Key al:</p>
            <p><a href="https://makersuite.google.com/app/apikeys" target="_blank">makersuite.google.com/app/apikeys</a></p>
            
            <p><strong>Adım 2:</strong> API Key'i aşağıya yapıştır:</p>
            
            <div class="error-message" id="errorMessage"></div>
            
            <input 
                type="password" 
                id="apiKeyInput" 
                placeholder="API Key'i buraya yapıştır..."
            >
            
            <button id="saveApiKeyBtn">Kaydet ve Başla</button>
            <small style="color: #999;">API Key'in güvenli şekilde tarayıcında saklanacak.</small>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/@google/generative-ai@0.3.0/dist/index.min.js"></script>

    <script>
        const chatMessages = document.getElementById('chatMessages');
        const messageInput = document.getElementById('messageInput');
        const sendBtn = document.getElementById('sendBtn');
        const setupModal = document.getElementById('setupModal');
        const apiKeyInput = document.getElementById('apiKeyInput');
        const saveApiKeyBtn = document.getElementById('saveApiKeyBtn');
        const apiStatus = document.getElementById('apiStatus');
        const errorMessage = document.getElementById('errorMessage');

        let genAI = null;
        let model = null;
        let conversationHistory = [];

        // API Key'i localStorage'dan al veya iste
        function initializeAPI() {
            const savedApiKey = localStorage.getItem('geminiApiKey');
            if (savedApiKey) {
                setupAPI(savedApiKey);
            } else {
                setupModal.style.display = 'flex';
            }
        }

        // API'yi başlat
        function setupAPI(apiKey) {
            try {
                genAI = new google.generativeai.GoogleGenerativeAI(apiKey);
                model = genAI.getGenerativeModel({ model: 'gemini-pro' });
                localStorage.setItem('geminiApiKey', apiKey);
                setupModal.style.display = 'none';
                apiStatus.textContent = '✓ Bağlı';
                apiStatus.className = 'api-status connected';
                addMessage('Merhaba! 👋 Ben Demo GPT. Google Gemini AI ile çalış��yorum. Sana nasıl yardımcı olabilirim?', false);
                messageInput.disabled = false;
                sendBtn.disabled = false;
            } catch (error) {
                showError('Geçersiz API Key! Lütfen tekrar dene.');
                console.error('API Setup Error:', error);
            }
        }

        // Hata mesajı göster
        function showError(message) {
            errorMessage.textContent = message;
            errorMessage.style.display = 'block';
            setTimeout(() => {
                errorMessage.style.display = 'none';
            }, 5000);
        }

        // Mesaj ekle
        function addMessage(text, isUser) {
            const messageDiv = document.createElement('div');
            messageDiv.className = `message ${isUser ? 'user' : 'bot'}`;
            
            const avatar = document.createElement('div');
            avatar.className = 'avatar';
            avatar.textContent = isUser ? '👤' : '🤖';
            
            const content = document.createElement('div');
            content.className = 'message-content';
            content.textContent = text;
            
            if (isUser) {
                messageDiv.appendChild(content);
                messageDiv.appendChild(avatar);
            } else {
                messageDiv.appendChild(avatar);
                messageDiv.appendChild(content);
            }
            
            chatMessages.appendChild(messageDiv);
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }

        // Yazma göstergesi
        function showTypingIndicator() {
            const messageDiv = document.createElement('div');
            messageDiv.className = 'message bot';
            messageDiv.id = 'typingIndicator';
            
            const avatar = document.createElement('div');
            avatar.className = 'avatar';
            avatar.textContent = '🤖';
            
            const content = document.createElement('div');
            content.className = 'message-content';
            
            const dot1 = document.createElement('div');
            dot1.className = 'typing-dot';
            const dot2 = document.createElement('div');
            dot2.className = 'typing-dot';
            const dot3 = document.createElement('div');
            dot3.className = 'typing-dot';
            
            const typingDiv = document.createElement('div');
            typingDiv.className = 'typing-indicator';
            typingDiv.appendChild(dot1);
            typingDiv.appendChild(dot2);
            typingDiv.appendChild(dot3);
            
            content.appendChild(typingDiv);
            messageDiv.appendChild(avatar);
            messageDiv.appendChild(content);
            
            chatMessages.appendChild(messageDiv);
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }

        function removeTypingIndicator() {
            const typingIndicator = document.getElementById('typingIndicator');
            if (typingIndicator) {
                typingIndicator.remove();
            }
        }

        // Gemini'den cevap al
        async function getAiResponse(userMessage) {
            try {
                conversationHistory.push({
                    role: "user",
                    parts: [{ text: userMessage }]
                });

                const chat = model.startChat({
                    history: conversationHistory
                });

                const result = await chat.sendMessage(userMessage);
                const responseText = result.response.text();

                conversationHistory.push({
                    role: "model",
                    parts: [{ text: responseText }]
                });

                return responseText;
            } catch (error) {
                console.error('AI Response Error:', error);
                return 'Üzgünüm, bir hata oluştu. Lütfen tekrar dene.';
            }
        }

        // Mesaj gönder
        async function sendMessage() {
            const message = messageInput.value.trim();
            
            if (message === '' || !model) return;
            
            addMessage(message, true);
            messageInput.value = '';
            messageInput.focus();
            
            showTypingIndicator();
            
            const aiResponse = await getAiResponse(message);
            removeTypingIndicator();
            addMessage(aiResponse, false);
        }

        // Event listeners
        sendBtn.addEventListener('click', sendMessage);

        messageInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                sendMessage();
            }
        });

        saveApiKeyBtn.addEventListener('click', () => {
            const apiKey = apiKeyInput.value.trim();
            if (apiKey) {
                setupAPI(apiKey);
            } else {
                showError('Lütfen API Key gir!');
            }
        });

        // Başlat
        window.addEventListener('load', initializeAPI);
    </script>
</body>
</html>
