<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <meta name="theme-color" content="#9be15d">
  <title>Super Mind - AI Chatbot</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    :root {
      --primary-color: #9be15d;
      --secondary-color: #00e3ae;
      --user-bubble: #e3f2fd;
      --bot-bubble: #f5f5f5;
      --text-dark: #333;
      --text-light: #fff;
      --shadow: 0 1px 3px rgba(0,0,0,0.12);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
      background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-start;
      min-height: 100vh;
      color: var(--text-dark);
      line-height: 1.6;
    }

    .app-container {
      width: 100%;
      max-width: 800px;
      height: 100vh;
      max-height: 100vh;
      display: flex;
      flex-direction: column;
      background: #fff;
      box-shadow: 0 4px 20px rgba(0,0,0,0.1);
      border-radius: 0;
      overflow: hidden;
    }

    @media (min-width: 768px) {
      .app-container {
        height: 90vh;
        max-height: 900px;
        margin: 20px 0;
        border-radius: 12px;
      }
    }

    header {
      background: linear-gradient(120deg, var(--primary-color), var(--secondary-color));
      color: var(--text-light);
      padding: 12px 20px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      z-index: 10;
    }

    .logo {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .logo-img {
      height: 32px;
      width: auto;
    }

    .logo h1 {
      font-size: 1.5rem;
      font-weight: 600;
      margin: 0;
    }

    .chat-container {
      flex: 1;
      overflow-y: auto;
      padding: 20px;
      background-color: #fafafa;
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .message {
      max-width: 85%;
      padding: 12px 16px;
      position: relative;
      animation: fadeIn 0.3s ease;
      line-height: 1.5;
      font-size: 0.95rem;
      box-shadow: var(--shadow);
      border-radius: 8px;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .user-message {
      background-color: var(--user-bubble);
      align-self: flex-end;
      border-top-right-radius: 0;
      color: var(--text-dark);
      margin-left: 15%;
      border-bottom: 2px solid #bbdefb;
    }

    .bot-message {
      background-color: var(--bot-bubble);
      align-self: flex-start;
      border-top-left-radius: 0;
      margin-right: 15%;
      border-bottom: 2px solid #e0e0e0;
    }

    .typing-indicator {
      display: flex;
      align-items: center;
      gap: 5px;
      padding: 12px 16px;
      background-color: var(--bot-bubble);
      border-radius: 8px;
      align-self: flex-start;
      margin-right: 15%;
      border-top-left-radius: 0;
      box-shadow: var(--shadow);
      border-bottom: 2px solid #e0e0e0;
    }

    .typing-dot {
      width: 8px;
      height: 8px;
      background-color: #666;
      border-radius: 50%;
      animation: typingAnimation 1.4s infinite ease-in-out;
    }

    .typing-dot:nth-child(1) {
      animation-delay: 0s;
    }
    .typing-dot:nth-child(2) {
      animation-delay: 0.2s;
    }
    .typing-dot:nth-child(3) {
      animation-delay: 0.4s;
    }

    @keyframes typingAnimation {
      0%, 60%, 100% { transform: translateY(0); }
      30% { transform: translateY(-5px); }
    }

    .input-container {
      padding: 15px;
      background: #fff;
      border-top: 1px solid #e0e0e0;
      display: flex;
      gap: 10px;
      align-items: center;
    }

    .input-wrapper {
      flex: 1;
      display: flex;
      background: #f0f2f5;
      border-radius: 24px;
      padding: 5px 15px;
      align-items: center;
    }

    #userInput {
      flex: 1;
      border: none;
      background: transparent;
      padding: 10px 0;
      font-size: 0.95rem;
      outline: none;
      resize: none;
      max-height: 120px;
      line-height: 1.5;
    }

    .send-btn {
      background: linear-gradient(120deg, var(--primary-color), var(--secondary-color));
      color: white;
      border: none;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: all 0.2s;
    }

    .send-btn:disabled {
      opacity: 0.6;
      cursor: not-allowed;
    }

    .send-btn i {
      font-size: 1rem;
    }

    .send-btn:hover:not(:disabled) {
      transform: scale(1.05);
    }

    .message-time {
      font-size: 0.7rem;
      color: #666;
      margin-top: 4px;
      text-align: right;
    }

    .copy-btn {
      position: absolute;
      top: 5px;
      right: 5px;
      background: rgba(255,255,255,0.7);
      border: none;
      border-radius: 50%;
      width: 24px;
      height: 24px;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      opacity: 0;
      transition: opacity 0.2s;
    }

    .bot-message:hover .copy-btn {
      opacity: 1;
    }

    .copy-btn i {
      font-size: 0.7rem;
      color: #333;
    }

    /* Markdown-like styling */
    .bot-message pre {
      background: #2d2d2d;
      color: #f8f8f2;
      padding: 10px;
      border-radius: 6px;
      overflow-x: auto;
      margin: 10px 0;
      font-family: 'Courier New', monospace;
      font-size: 0.85rem;
    }

    .bot-message code {
      background: #f0f0f0;
      padding: 2px 4px;
      border-radius: 4px;
      font-family: 'Courier New', monospace;
      font-size: 0.85rem;
    }

    .bot-message strong {
      font-weight: 600;
    }

    .bot-message em {
      font-style: italic;
    }

    .bot-message ul, 
    .bot-message ol {
      padding-left: 20px;
      margin: 8px 0;
    }

    .bot-message li {
      margin-bottom: 4px;
    }

    .bot-message blockquote {
      border-left: 3px solid #ddd;
      padding-left: 12px;
      color: #666;
      margin: 8px 0;
    }

    /* Scrollbar styling */
    ::-webkit-scrollbar {
      width: 8px;
    }

    ::-webkit-scrollbar-track {
      background: #f1f1f1;
    }

    ::-webkit-scrollbar-thumb {
      background: #c1c1c1;
      border-radius: 4px;
    }

    ::-webkit-scrollbar-thumb:hover {
      background: #a8a8a8;
    }
  </style>
</head>
<body>
  <div class="app-container">
    <header>
      <div class="logo">
        <img src="supermind.png" alt="Super Mind Logo" class="logo-img">
        <h1>Super Mind</h1>
      </div>
    </header>

    <div class="chat-container" id="chatContainer">
      <div class="bot-message message">
        <button class="copy-btn" title="Copy to clipboard"><i class="far fa-copy"></i></button>
        Hello! I'm Super Mind, your AI assistant. How can I help you today?
        <div class="message-time">Just now</div>
      </div>
    </div>

    <div class="input-container">
      <div class="input-wrapper">
        <textarea id="userInput" placeholder="Message Super Mind..." rows="1"></textarea>
      </div>
      <button class="send-btn" id="sendBtn" disabled>
        <i class="fas fa-paper-plane"></i>
      </button>
    </div>
  </div>

  <script>
    // DOM Elements
    const chatContainer = document.getElementById('chatContainer');
    const userInput = document.getElementById('userInput');
    const sendBtn = document.getElementById('sendBtn');

    // Configuration
    const API_KEY = 'ddc-temp-free-e3b73cd814cc4f3ea79b5d4437912663';
    const BASE_URL = 'https://api.devsdocode.com/v1/chat/completions';
    const MODEL = 'provider-4/gpt-4.1';
    const CREATOR_NAME = 'Abdullah';

    // Special responses for creator questions
    const CREATOR_RESPONSES = {
      "who created you": `I was created by ${CREATOR_NAME}. He's a talented developer who built me to help people with their questions and tasks.`,
      "who made you": `My creator is ${CREATOR_NAME}. He developed me as an AI assistant to provide helpful information and support.`,
      "who developed you": `${CREATOR_NAME} is the developer behind Super Mind. He designed me to be a useful and intelligent assistant.`,
      "what is your creator's name": `My creator's name is ${CREATOR_NAME}. He put a lot of effort into making me helpful and responsive!`,
      "who is your creator": `${CREATOR_NAME} is my creator. He built me to assist users like you with various tasks and questions.`
    };

    // Conversation history
    let conversationHistory = [
      { role: 'system', content: `You are Super Mind, an intelligent and friendly AI assistant created by ${CREATOR_NAME}. Provide helpful, detailed responses. Use markdown formatting for code blocks, lists, and other structured content. If asked about your creator, respond naturally that you were created by ${CREATOR_NAME}.` }
    ];

    // Initialize the app
    function init() {
      setupEventListeners();
      adjustTextareaHeight();
    }

    // Set up event listeners
    function setupEventListeners() {
      sendBtn.addEventListener('click', sendMessage);
      userInput.addEventListener('keydown', handleTextareaKeydown);
      userInput.addEventListener('input', handleTextareaInput);
      chatContainer.addEventListener('click', handleChatContainerClick);
    }

    // Handle send button click or Enter key
    async function sendMessage() {
      const message = userInput.value.trim();
      if (!message) return;

      // Add user message to chat
      appendMessage(message, 'user');
      userInput.value = '';
      adjustTextareaHeight();
      sendBtn.disabled = true;

      // Add to conversation history
      conversationHistory.push({ role: 'user', content: message });

      // Check for creator questions
      const lowerMessage = message.toLowerCase();
      const creatorResponse = Object.keys(CREATOR_RESPONSES).find(key => lowerMessage.includes(key));
      
      if (creatorResponse) {
        // Show typing indicator
        const typingIndicator = showTypingIndicator();
        setTimeout(() => {
          chatContainer.removeChild(typingIndicator);
          appendMessage(CREATOR_RESPONSES[creatorResponse], 'bot');
          conversationHistory.push({ role: 'assistant', content: CREATOR_RESPONSES[creatorResponse] });
        }, 1000);
        return;
      }

      // Show typing indicator
      const typingIndicator = showTypingIndicator();

      try {
        const response = await fetch(BASE_URL, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${API_KEY}`
          },
          body: JSON.stringify({
            model: MODEL,
            messages: conversationHistory,
            temperature: 0.7
          })
        });

        if (!response.ok) {
          throw new Error(`API request failed with status ${response.status}`);
        }

        const data = await response.json();
        const reply = data.choices[0].message.content;

        // Remove typing indicator
        chatContainer.removeChild(typingIndicator);

        // Add bot response to chat and history
        appendMessage(reply, 'bot');
        conversationHistory.push({ role: 'assistant', content: reply });
      } catch (error) {
        console.error('Error:', error);
        chatContainer.removeChild(typingIndicator);
        appendMessage("Sorry, I'm having trouble connecting. Please try again later.", 'bot');
      }
    }

    // Append a message to the chat
    function appendMessage(text, sender) {
      const messageDiv = document.createElement('div');
      messageDiv.classList.add('message', `${sender}-message`);

      // Add copy button for bot messages
      if (sender === 'bot') {
        const copyBtn = document.createElement('button');
        copyBtn.classList.add('copy-btn');
        copyBtn.title = 'Copy to clipboard';
        copyBtn.innerHTML = '<i class="far fa-copy"></i>';
        messageDiv.appendChild(copyBtn);
      }

      // Process markdown-like formatting
      const processedText = processMarkdown(text);
      messageDiv.innerHTML += processedText;

      // Add timestamp
      const timeDiv = document.createElement('div');
      timeDiv.classList.add('message-time');
      timeDiv.textContent = getCurrentTime();
      messageDiv.appendChild(timeDiv);

      chatContainer.appendChild(messageDiv);
      scrollToBottom();
    }

    // Show typing indicator
    function showTypingIndicator() {
      const typingDiv = document.createElement('div');
      typingDiv.classList.add('typing-indicator');
      
      for (let i = 0; i < 3; i++) {
        const dot = document.createElement('div');
        dot.classList.add('typing-dot');
        typingDiv.appendChild(dot);
      }
      
      chatContainer.appendChild(typingDiv);
      scrollToBottom();
      return typingDiv;
    }

    // Process markdown-like formatting
    function processMarkdown(text) {
      // Simple markdown processing for demonstration
      return text
        .replace(/```([\s\S]*?)```/g, '<pre>$1</pre>')
        .replace(/`([^`]+)`/g, '<code>$1</code>')
        .replace(/\*\*(.+?)\*\*/g, '<strong>$1</strong>')
        .replace(/\*(.+?)\*/g, '<em>$1</em>')
        .replace(/^>\s?(.+)$/gm, '<blockquote>$1</blockquote>')
        .replace(/^- (.+)$/gm, '<li>$1</li>')
        .replace(/^1\. (.+)$/gm, '<li>$1</li>')
        .replace(/\n/g, '<br>');
    }

    // Handle textarea input
    function handleTextareaInput() {
      adjustTextareaHeight();
      sendBtn.disabled = userInput.value.trim() === '';
    }

    // Handle textarea keydown events
    function handleTextareaKeydown(e) {
      if (e.key === 'Enter' && !e.shiftKey) {
        e.preventDefault();
        sendMessage();
      }
    }

    // Adjust textarea height based on content
    function adjustTextareaHeight() {
      userInput.style.height = 'auto';
      userInput.style.height = `${Math.min(userInput.scrollHeight, 120)}px`;
    }

    // Handle chat container clicks (for copy buttons)
    function handleChatContainerClick(e) {
      if (e.target.closest('.copy-btn')) {
        const messageDiv = e.target.closest('.bot-message');
        const messageText = messageDiv.innerText.replace('Copy', '').trim();
        copyToClipboard(messageText);
        
        // Show feedback
        const copyBtn = e.target.closest('.copy-btn');
        copyBtn.innerHTML = '<i class="fas fa-check"></i>';
        setTimeout(() => {
          copyBtn.innerHTML = '<i class="far fa-copy"></i>';
        }, 2000);
      }
    }

    // Copy text to clipboard
    function copyToClipboard(text) {
      navigator.clipboard.writeText(text).then(() => {
        console.log('Copied to clipboard');
      }).catch(err => {
        console.error('Failed to copy:', err);
      });
    }

    // Get current time in HH:MM format
    function getCurrentTime() {
      const now = new Date();
      return now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
    }

    // Scroll chat to bottom
    function scrollToBottom() {
      chatContainer.scrollTop = chatContainer.scrollHeight;
    }

    // Initialize the app
    document.addEventListener('DOMContentLoaded', init);
  </script>
</body>
</html>
