<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8" />
  <title>MØR•AI</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />

  <style>
    :root {
      --bg: #0f172a;
      --bg-grad: radial-gradient(circle at top, #1f2937 0, #020617 55%, #020617 100%);
      --card-bg: #020617;
      --border: rgba(148, 163, 184, 0.4);
      --accent: #8b5cf6;
      --accent-soft: rgba(139, 92, 246, 0.12);
      --user-bubble: linear-gradient(135deg, #6366f1, #a855f7);
      --text-main: #e5e7eb;
      --text-soft: #9ca3af;
      --input-bg: rgba(15, 23, 42, 0.85);
      --header-bg: radial-gradient(circle at top left, #1d283a, #020617 65%);
      --hint-bg: rgba(15, 23, 42, 0.9);
      --panel-bg: rgba(15, 23, 42, 0.95);
    }

    body.light {
      --bg: #e5e7eb;
      --bg-grad: radial-gradient(circle at top, #f3f4f6 0, #e5e7eb 40%, #d1d5db 100%);
      --card-bg: #f9fafb;
      --border: rgba(148, 163, 184, 0.7);
      --accent: #6366f1;
      --accent-soft: rgba(99, 102, 241, 0.16);
      --user-bubble: linear-gradient(135deg, #6366f1, #8b5cf6);
      --text-main: #111827;
      --text-soft: #6b7280;
      --input-bg: #ffffff;
      --header-bg: linear-gradient(135deg, #6366f1, #8b5cf6);
      --hint-bg: #e5e7eb;
      --panel-bg: #f3f4f6;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      background: var(--bg-grad);
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
        sans-serif;
      color: var(--text-main);
    }

    .app {
      width: 100%;
      max-width: 540px;
      height: 100vh;
      max-height: 820px;
      background: var(--card-bg);
      border-radius: 22px;
      border: 1px solid var(--border);
      box-shadow: 0 25px 60px rgba(15, 23, 42, 0.75);
      display: flex;
      flex-direction: column;
      overflow: hidden;
    }

    .header {
      padding: 12px 16px;
      border-bottom: 1px solid var(--border);
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: var(--header-bg);
    }

    .brand {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .logo {
      width: 32px;
      height: 32px;
      border-radius: 999px;
      background: radial-gradient(circle at 30% 30%, #f9fafb, #8b5cf6);
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: 800;
      font-size: 15px;
      color: #0f172a;
    }

    .titles h1 {
      font-size: 16px;
      letter-spacing: 0.14em;
    }

    .titles span {
      font-size: 11px;
      color: var(--text-soft);
    }

    .header-right {
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .pill {
      font-size: 11px;
      background: rgba(15, 23, 42, 0.8);
      border-radius: 999px;
      padding: 4px 8px;
      display: inline-flex;
      align-items: center;
      gap: 5px;
      color: #e5e7eb;
    }

    body.light .pill {
      background: rgba(15, 23, 42, 0.12);
      color: #111827;
    }

    .pill-dot {
      width: 6px;
      height: 6px;
      border-radius: 999px;
      background: #22c55e;
    }

    .theme-btn {
      width: 30px;
      height: 30px;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.7);
      background: rgba(15, 23, 42, 0.85);
      color: #e5e7eb;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 15px;
    }

    body.light .theme-btn {
      background: #f9fafb;
      color: #111827;
      border-color: rgba(148, 163, 184, 0.7);
    }

    .hint {
      padding: 6px 14px 8px;
      border-bottom: 1px solid var(--border);
      font-size: 11px;
      color: var(--text-soft);
      background: var(--hint-bg);
    }

    .hint span {
      background: rgba(55, 65, 81, 0.8);
      border-radius: 999px;
      padding: 2px 8px;
      font-size: 10px;
      margin-right: 4px;
      color: #e5e7eb;
    }

    body.light .hint span {
      background: rgba(55, 65, 81, 0.1);
      color: #111827;
    }

    .chat {
      flex: 1;
      padding: 10px 10px 12px;
      overflow-y: auto;
      display: flex;
      flex-direction: column;
      gap: 8px;
      background: radial-gradient(circle at top, rgba(148, 163, 184, 0.18), transparent 55%);
    }

    .chat::-webkit-scrollbar {
      width: 6px;
    }
    .chat::-webkit-scrollbar-thumb {
      background: rgba(148, 163, 184, 0.8);
      border-radius: 999px;
    }

    .msg-row {
      display: flex;
      flex-direction: column;
      max-width: 78%;
    }

    .msg-row.user {
      align-self: flex-end;
    }

    .msg-row.bot {
      align-self: flex-start;
    }

    .bubble {
      padding: 9px 12px;
      border-radius: 14px;
      background: transparent; /* bot için default şeffaf */
      color: var(--text-main);
      font-size: 14px;
      line-height: 1.35;
      white-space: pre-line;
    }

    /* Sadece KULLANICI balonlu olsun */
    .msg-row.user .bubble {
      background: var(--user-bubble);
      color: #f9fafb;
      border-radius: 16px;
      border: none;
      border-bottom-right-radius: 4px;
    }

    /* Bot mesajı: düz metin, balonsuz */
    .msg-row.bot .bubble {
      padding: 0;
      border: none;
      border-radius: 0;
    }

    .bubble img {
      display: block;
      max-width: 260px;
      max-height: 260px;
      border-radius: 12px;
      object-fit: cover;
    }

    .meta {
      font-size: 10px;
      color: var(--text-soft);
      margin-top: 3px;
    }

    .meta.right {
      text-align: right;
    }

    .input-bar {
      padding: 8px 10px 10px;
      border-top: 1px solid var(--border);
      background: var(--panel-bg);
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .icon-btn {
      width: 34px;
      height: 34px;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.7);
      background: rgba(15, 23, 42, 0.8);
      color: var(--text-main);
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 17px;
    }

    body.light .icon-btn {
      background: #ffffff;
      color: #111827;
      border-color: rgba(148, 163, 184, 0.7);
    }

    #input {
      flex: 1;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.7);
      background: var(--input-bg);
      color: var(--text-main);
      padding: 8px 11px;
      font-size: 14px;
      outline: none;
    }

    #input::placeholder {
      color: rgba(148, 163, 184, 0.9);
    }

    body.light #input {
      border-color: rgba(148, 163, 184, 0.7);
    }

    #sendBtn {
      width: 40px;
      height: 34px;
      border-radius: 999px;
      border: none;
      background: var(--accent);
      color: #f9fafb;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 17px;
    }

    @media (max-width: 480px) {
      .app {
        max-height: 100vh;
        border-radius: 0;
      }
    }
  </style>
</head>

<body>
  <div class="app">
    <header class="header">
      <div class="brand">
        <div class="logo">Ø</div>
        <div class="titles">
          <h1>MØR•AI</h1>
          <span>TR bot · wiki + görsel</span>
        </div>
      </div>
      <div class="header-right">
        <div class="pill">
          <span class="pill-dot"></span>
          çevrimiçi
        </div>
        <button class="theme-btn" id="themeBtn">🌙</button>
      </div>
    </header>

    <div class="hint">
      <span>örnek</span>
      “Boz ayı nedir?” · “Bana kedi resmi gönder” · “İstanbul”
    </div>

    <div class="chat" id="chat">
      <div class="msg-row bot">
        <div class="bubble">
Hoş geldin 💜
Ben tarayıcı içinde çalışan küçük bir MØR•AI’yim.

• Metin yaz → Vikipedi’den özet bulmaya çalışırım  
• “bana kedi resmi gönder” → internetten görsel çekerim  
• 📷 tuşuyla → kendi fotoğrafını gönderebilirsin (sadece gösteririm)

Bilmediğim konuda “bulamadım, sallamıyorum” derim; uydurma yok.
        </div>
        <div class="meta">şimdi</div>
      </div>
    </div>

    <div class="input-bar">
      <input type="file" id="fileInput" accept="image/*" style="display:none" />
      <button class="icon-btn" id="fileBtn">📷</button>
      <input id="input" type="text" placeholder="Bir şey yaz..." />
      <button id="sendBtn">➡️</button>
    </div>
  </div>

  <script>
    const chatEl = document.getElementById("chat");
    const inputEl = document.getElementById("input");
    const sendBtn = document.getElementById("sendBtn");
    const fileBtn = document.getElementById("fileBtn");
    const fileInput = document.getElementById("fileInput");
    const themeBtn = document.getElementById("themeBtn");

    // Tema switch
    let dark = true;
    themeBtn.addEventListener("click", () => {
      dark = !dark;
      document.body.classList.toggle("light", !dark);
      themeBtn.textContent = dark ? "🌙" : "☀️";
    });

    function nowTime() {
      const d = new Date();
      return String(d.getHours()).padStart(2, "0") + ":" +
             String(d.getMinutes()).padStart(2, "0");
    }

    function addMessage({ text = "", isUser = false, imageUrl = null }) {
      const row = document.createElement("div");
      row.className = "msg-row " + (isUser ? "user" : "bot");

      const bubble = document.createElement("div");
      bubble.className = "bubble";

      if (imageUrl) {
        const img = document.createElement("img");
        img.src = imageUrl;
        img.alt = "görsel";
        bubble.appendChild(img);
      }

      if (text) {
        if (imageUrl) {
          const p = document.createElement("div");
          p.style.marginTop = "6px";
          p.textContent = text;
          bubble.appendChild(p);
        } else {
          bubble.textContent = text;
        }
      }

      const meta = document.createElement("div");
      meta.className = "meta" + (isUser ? " right" : "");
      meta.textContent = nowTime();

      row.appendChild(bubble);
      row.appendChild(meta);
      chatEl.appendChild(row);
      chatEl.scrollTop = chatEl.scrollHeight;
    }

    function detectImageTopic(text) {
      const lower = text.toLowerCase();
      const hasImageWord =
        lower.includes("resim") ||
        lower.includes("resmi") ||
        lower.includes("foto") ||
        lower.includes("fotoğraf") ||
        lower.includes("fotograf") ||
        lower.includes("görsel") ||
        lower.includes("gorsel");

      if (!hasImageWord) return null;

      const cleaned = lower
        .replace("bana", "")
        .replace("resmi", "")
        .replace("resim", "")
        .replace("fotoğrafını", "")
        .replace("fotoğraf", "")
        .replace("fotograf", "")
        .replace("görselini", "")
        .replace("görsel", "")
        .trim();

      return cleaned || "nature";
    }

    function normalizeWikiQuery(text) {
      let t = text.toLowerCase();
      t = t.replace(/\?/g, " ");
      t = t.replace(/ nedir/g, " ");
      t = t.replace(/ kimdir/g, " ");
      t = t.replace(/ hakkında bilgi ver/g, " ");
      t = t.replace(/bana /g, " ");
      t = t.replace(/\s+/g, " ").trim();
      return t || text.trim();
    }

    async function fetchWikiSummary(query) {
      const q = normalizeWikiQuery(query);
      const url = "https://tr.wikipedia.org/api/rest_v1/page/summary/" +
                  encodeURIComponent(q);

      try {
        const res = await fetch(url);
        if (!res.ok) return null;
        const data = await res.json();
        if (!data.extract) return null;

        let text = data.extract;
        if (text.length > 900) {
          text = text.slice(0, 880) + "...";
        }
        text += "\n\nKaynak: Vikipedi";
        return text;
      } catch (e) {
        console.error("Wiki hatası:", e);
        return null;
      }
    }

    function searchImage(topic) {
      const url =
        "https://images.weserv.nl/?url=source.unsplash.com/600x400/?" +
        encodeURIComponent(topic);

      addMessage({
        text: `"${topic}" için bir görsel gösteriyorum 📷 (kaynak: Unsplash)`,
        isUser: false,
        imageUrl: url,
      });
    }

    async function handleBotReply(userText) {
      const topic = detectImageTopic(userText);
      if (topic) {
        searchImage(topic);
        return;
      }

      addMessage({
        text: "Sorunu Vikipedi'de arıyorum, uygun bir özet bulmaya çalışıyorum…",
        isUser: false,
      });

      const wikiText = await fetchWikiSummary(userText);
      if (wikiText) {
        addMessage({ text: wikiText, isUser: false });
      } else {
        addMessage({
          text:
            "Bu konuda anlamlı bir kaynak bulamadım. Uydurmak istemiyorum 😇\n" +
            "Soruyu biraz daha farklı veya ayrıntılı sorabilirsin.",
          isUser: false,
        });
      }
    }

    function sendTextMessage() {
      const text = inputEl.value.trim();
      if (!text) return;
      addMessage({ text, isUser: true });
      inputEl.value = "";
      inputEl.focus();
      handleBotReply(text);
    }

    sendBtn.addEventListener("click", sendTextMessage);
    inputEl.addEventListener("keydown", (e) => {
      if (e.key === "Enter") {
        e.preventDefault();
        sendTextMessage();
      }
    });

    fileBtn.addEventListener("click", () => {
      fileInput.value = "";
      fileInput.click();
    });

    fileInput.addEventListener("change", () => {
      const file = fileInput.files[0];
      if (!file) return;
      const url = URL.createObjectURL(file);

      addMessage({
        imageUrl: url,
        isUser: true,
        text: "",
      });

      setTimeout(() => {
        addMessage({
          text: "Fotoğrafını gördüm 📷 İstersen biraz da hikâyesini anlat.",
          isUser: false,
        });
      }, 400);

      setTimeout(() => URL.revokeObjectURL(url), 15000);
    });
  </script>
</body>
</html>
