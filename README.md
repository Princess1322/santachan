# santachan
ai agent for daosdotfun
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Agent</title>

    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            background: #0b0f19;
            color: white;
        }

        header {
            padding: 20px;
            background: #141a2b;
            text-align: center;
            font-size: 24px;
            border-bottom: 2px solid #232c46;
        }

        .chat-container {
            max-width: 850px;
            margin: 30px auto;
            padding: 20px;
            background: #141a2b;
            border-radius: 12px;
            height: 70vh;
            overflow-y: auto;
            box-shadow: 0 0 20px rgba(0,0,0,0.4);
        }

        .message {
            padding: 12px 16px;
            margin: 10px 0;
            border-radius: 10px;
            max-width: 80%;
            line-height: 1.5;
        }

        .user {
            background: #1f6feb;
            align-self: flex-end;
            margin-left: auto;
        }

        .ai {
            background: #2d354d;
            align-self: flex-start;
        }

        .input-area {
            display: flex;
            max-width: 850px;
            margin: 0 auto 20px;
        }

        input {
            flex: 1;
            padding: 14px;
            border: none;
            outline: none;
            border-radius: 10px 0 0 10px;
            background: #1b2236;
            color: white;
            font-size: 16px;
        }

        button {
            padding: 14px 20px;
            border: none;
            cursor: pointer;
            background: #1f6feb;
            color: white;
            border-radius: 0 10px 10px 0;
            font-size: 16px;
        }

        button:hover {
            background: #3584ff;
        }
    </style>
</head>
<body>

<header>AI Agent</header>

<div class="chat-container" id="chatBox"></div>

<div class="input-area">
    <input type="text" id="userInput" placeholder="Ask something...">
    <button onclick="sendMessage()">Send</button>
</div>

<script>
    const API_KEY = "YOUR_OPENAI_API_KEY";  // <-- PUT YOUR KEY HERE

    async function sendMessage() {
        const input = document.getElementById("userInput");
        const text = input.value.trim();
        if (!text) return;

        addMessage("user", text);
        input.value = "";

        const reply = await askAI(text);
        addMessage("ai", reply);
    }

    function addMessage(type, text) {
        const box = document.getElementById("chatBox");
        const div = document.createElement("div");
        div.className = "message " + type;
        div.innerText = text;
        box.appendChild(div);
        box.scrollTop = box.scrollHeight;
    }

    async function askAI(prompt) {
        try {
            const response = await fetch("https://api.openai.com/v1/chat/completions", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "Authorization": "Bearer " + API_KEY
                },
                body: JSON.stringify({
                    model: "gpt-4o-mini",
                    messages: [{ role: "user", content: prompt }]
                })
            });

            const data = await response.json();
            return data.choices[0].message.content;
        } catch (err) {
            return "Error: Unable to reach AI.";
        }
    }
</script>

</body>
</html>
