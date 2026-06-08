<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI O‘qituvchi</title>

<style>
body {
  margin: 0;
  font-family: Arial;
  background: linear-gradient(135deg, #141e30, #243b55);
  color: white;
}

header {
  text-align: center;
  padding: 15px;
  font-size: 22px;
  background: rgba(0,0,0,0.5);
}

#chat {
  height: 70vh;
  overflow-y: auto;
  padding: 10px;
}

.message {
  margin: 8px;
  padding: 12px;
  border-radius: 12px;
  max-width: 80%;
  animation: fadeIn 0.3s ease;
}

.user {
  background: #00c6ff;
  color: black;
  margin-left: auto;
}

.ai {
  background: #2b2b2b;
}

@keyframes fadeIn {
  from {opacity: 0; transform: translateY(10px);}
  to {opacity: 1;}
}

#inputArea {
  display: flex;
  padding: 10px;
  background: rgba(0,0,0,0.6);
}

input {
  flex: 1;
  padding: 12px;
  border-radius: 10px;
  border: none;
  font-size: 16px;
}

button {
  padding: 12px;
  margin-left: 5px;
  border: none;
  border-radius: 10px;
  background: #00c6ff;
  font-weight: bold;
}
</style>
</head>

<body>

<header>🤖 AI O‘qituvchi PRO</header>

<div id="chat"></div>

<div id="inputArea">
  <input id="input" placeholder="Savol yoz...">
  <button onclick="send()">Yubor</button>
</div>

<script>
const chat = document.getElementById("chat");

function addMessage(text, type) {
  let div = document.createElement("div");
  div.className = "message " + type;
  div.innerText = text;
  chat.appendChild(div);
  chat.scrollTop = chat.scrollHeight;
}

async function send() {
  let input = document.getElementById("input");
  let text = input.value;
  if(!text) return;

  addMessage(text, "user");
  input.value = "";

  let loading = document.createElement("div");
  loading.className = "message ai";
  loading.innerText = "Yozilmoqda...";
  chat.appendChild(loading);

  try {
    const response = await fetch("https://SENING_SERVERING.onrender.com/ai", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify({ text: text })
    });

    const data = await response.json();
    chat.removeChild(loading);

    let reply = data.choices?.[0]?.message?.content || "Xatolik yuz berdi";
    addMessage(reply, "ai");

  } catch (err) {
    chat.removeChild(loading);
    addMessage("Server bilan bog‘lanishda xatolik!", "ai");
  }
}
</script>

</body>
</html>
