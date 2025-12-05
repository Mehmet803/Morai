<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<title>MORai v3</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body {
    background: #2b2d42;
    margin: 0;
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
}
#app {
    width: 100%;
    max-width: 500px;
    height: 100vh;
    background: #ffffff;
    display: flex;
    flex-direction: column;
}
header {
    background: #6a00ff;
    color: #fff;
    padding: 12px;
    text-align: center;
    font-size: 18px;
    font-weight: bold;
}
#messages {
    flex: 1;
    overflow-y: auto;
    padding: 10px;
}
.msg {
    max-width: 80%;
    padding: 8px;
    border-radius: 10px;
    margin: 6px 0;
    font-size: 15px;
    background: #ececec;
}
.user { background: #6a00ff; color: #fff; margin-left: auto; }
.msg img {
    width: 200px;
    border-radius: 10px;
}
.controls {
    padding: 8px;
    background: #eee;
    display: flex;
    gap: 6px;
}
input {
    flex: 1;
    padding: 8px;
    border-radius: 6px;
    border: 1px solid #bbb;
}
button {
    padding: 8px 14px;
    border: none;
    cursor: pointer;
    background: #6a00ff;
    color: #fff;
    border-radius: 6px;
}
</style>
</head>

<body>
<div id="app">
    <header>MORai 🤖</header>
    <div id="messages"></div>

    <div class="controls">
        <input type="file" id="fileInput" accept="image/*" style="display:none">
        <button id="fileBtn">📷</button>
        <input type="text" id="textInput" placeholder="Mesaj yaz...">
        <button onclick="sendMessage()">➡️</button>
    </div>
</div>

<script>
const messages = document.getElementById("messages");
const textInput = document.getElementById("textInput");
const fileInput = document.getElementById("fileInput");

function addMessage(content, isUser = false, isImage = false) {
    const div = document.createElement("div");
    div.classList.add("msg");
    if (isUser) div.classList.add("user");
    
    if (isImage) {
        const img = document.createElement("img");
        img.src = content;
        div.appendChild(img);
    } else {
        div.innerText = content;
    }
    
    messages.appendChild(div);
    messages.scrollTop = messages.scrollHeight;
}

// Bot internetten görüntü bulsun 📷
function searchImage(q) {
    // Proxy kullandık ➜ resim kesin görünür
    const url = `https://images.weserv.nl/?url=source.unsplash.com/600x400/?${encodeURIComponent(q)}`;
    addMessage(url, false, true);
}

fileBtn.onclick = () => fileInput.click();

fileInput.onchange = () => {
    const file = fileInput.files[0];
    const url = URL.createObjectURL(file);
    addMessage(url, true, true);
    setTimeout(() => addMessage("Fotoğrafın çok iyi! 📸"), 500);
};

async function sendMessage() {
    const text = textInput.value.trim();
    if (!text) return;

    addMessage(text, true);
    textInput.value = "";

    // Fotoğraf isteği algılama
    if (
        text.includes("resim") ||
        text.includes("foto") ||
        text.includes("fotoğraf")
    ) {
        addMessage("Tamam! Sana internetten bir görsel buluyorum… 🔍");

        let konu = text.replace("bana", "")
                       .replace("resmi", "")
                       .replace("resim", "")
                       .replace("fotoğraf", "")
                       .trim();

        if (konu.length < 2) konu = "nature";

        searchImage(konu);
        return;
    }

    // Wikipedia API
    addMessage("Bunu araştırıyorum… ⏳");
    const api = `https://tr.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(text)}`;
    const res = await fetch(api);

    if (res.ok) {
        const data = await res.json();
        if (data.extract) {
            addMessage(data.extract + "\n\n(Kaynak: Vikipedi)");
            return;
        }
    }

    addMessage("Buna dair net bir şey bulamadım. Uydurmak istemiyorum 😇");
}
</script>

</body>
</html>
