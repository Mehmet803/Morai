<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8" />
    <title>MORai</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <style>
        :root {
            --bg: #e7ebff;
            --bg-gradient: radial-gradient(circle at top, #f5e9ff 0, #f4f4f7 35%, #e4e7ff 100%);
            --card-bg: #ffffff;
            --card-border: rgba(148, 163, 184, 0.45);
            --text-main: #111827;
            --text-soft: #6b7280;
            --accent: #4f46e5;
            --accent-soft: #7c3aed;
            --bubble-user: linear-gradient(135deg, #4f46e5, #7c3aed);
            --bubble-bot: #e5e7eb;
            --header-bg: linear-gradient(135deg, #5b21ff, #9333ea);
            --shadow: 0 20px 50px rgba(15, 23, 42, 0.22);
        }

        body.dark {
            --bg: #020617;
            --bg-gradient: radial-gradient(circle at top, #0f172a 0, #020617 40%, #020617 100%);
            --card-bg: #020617;
            --card-border: rgba(51, 65, 85, 0.9);
            --text-main: #e5e7eb;
            --text-soft: #9ca3af;
            --accent: #a855f7;
            --accent-soft: #4f46e5;
            --bubble-user: linear-gradient(135deg, #6366f1, #a855f7);
            --bubble-bot: #111827;
            --header-bg: linear-gradient(135deg, #1d2671, #5c1b7c);
            --shadow: 0 20px 50px rgba(15, 23, 42, 0.9);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
            background: var(--bg-gradient);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            color: var(--text-main);
        }

        .app {
            width: 100%;
            max-width: 620px;
            background: var(--card-bg);
            border-radius: 24px;
            box-shadow: var(--shadow);
            border: 1px solid var(--card-border);
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        .header {
            padding: 14px 18px 10px;
            background: var(--header-bg);
            color: #f9fafb;
        }

        .header-top {
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .logo-group {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .logo-circle {
            width: 32px;
            height: 32px;
            border-radius: 999px;
            background: radial-gradient(circle at 30% 30%, #f9fafb, #a855f7);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            font-size: 14px;
            box-shadow: 0 0 0 2px rgba(243, 244, 246, 0.5);
        }

        .header-titles {
            display: flex;
            flex-direction: column;
        }

        .header h1 {
            font-size: 18px;
            letter-spacing: 0.08em;
            text-transform: uppercase;
        }

        .header small {
            display: block;
            font-size: 11px;
            opacity: 0.9;
        }

        .status-group {
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .status-pill {
            font-size: 11px;
            padding: 3px 8px;
            border-radius: 999px;
            background: rgba(15, 23, 42, 0.35);
            display: inline-flex;
            align-items: center;
            gap: 4px;
        }

        .status-dot {
            width: 6px;
            height: 6px;
            border-radius: 999px;
            background: #4ade80;
        }

        .theme-toggle {
            border-radius: 999px;
            border: 1px solid rgba(148, 163, 184, 0.7);
            background: rgba(15, 23, 42, 0.35);
            color: #e5e7eb;
            font-size: 11px;
            padding: 3px 8px;
            cursor: pointer;
            display: inline-flex;
            align-items: center;
            gap: 4px;
        }

        .header-hint {
            margin-top: 8px;
            font-size: 11px;
            opacity: 0.9;
        }

        .header-hint span {
            background: rgba(15, 23, 42, 0.3);
            border-radius: 999px;
            padding: 2px 8px;
            margin-right: 4px;
            white-space: nowrap;
        }

        .chat-area {
            background: radial-gradient(circle at top, rgba(148, 163, 184, 0.12), transparent 55%);
            padding: 12px 10px;
            height: 60vh;
            min-height: 320px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .chat-area::-webkit-scrollbar {
            width: 6px;
        }

        .chat-area::-webkit-scrollbar-thumb {
            background: rgba(148, 163, 184, 0.9);
            border-radius: 999px;
        }

        .message {
            max-width: 82%;
            display: flex;
            flex-direction: column;
        }

        .message.bot {
            align-self: flex-start;
        }

        .message.user {
            align-self: flex-end;
            text-align: right;
        }

        .bubble {
            padding: 9px 12px;
            border-radius: 16px;
            font-size: 14px;
            line-height: 1.4;
            background: var(--bubble-bot);
            color: var(--text-main);
            white-space: pre-line;
            position: relative;
        }

        .message.user .bubble {
            background: var(--bubble-user);
            color: #f9fafb;
            border-bottom-right-radius: 4px;
        }

        .message.bot .bubble {
            border-bottom-left-radius: 4px;
        }

        .bubble img {
            max-width: 260px;
            max-height: 260px;
            border-radius: 12px;
            display: block;
        }

        .time {
            margin-top: 3px;
            font-size: 11px;
            color: var(--text-soft);
        }

        .typing-dots {
            display: inline-flex;
            gap: 3px;
            vertical-align: middle;
        }

        .typing-dots span {
            width: 4px;
            height: 4px;
            border-radius: 999px;
            background: #9ca3af;
            animation: bounce 1s infinite ease-in-out;
        }

        .typing-dots span:nth-child(2) {
            animation-delay: 0.15s;
        }

        .typing-dots span:nth-child(3) {
            animation-delay: 0.3s;
        }

        @keyframes bounce {
            0%, 80%, 100% { transform: scale(0.7); opacity: 0.5; }
            40% { transform: scale(1); opacity: 1; }
        }

        .input-area {
            display: flex;
            gap: 8px;
            align-items: center;
            padding: 10px;
            border-top: 1px solid #e5e7eb;
            background: var(--card-bg);
        }

        #yazi {
            flex: 1;
            border-radius: 999px;
            border: 1px solid #d1d5db;
            padding: 9px 13px;
            font-size: 14px;
            background: #f9fafb;
            outline: none;
            color: var(--text-main);
        }

        body.dark #yazi {
            background: #020617;
            border-color: #475569;
            color: #e5e7eb;
        }

        #yazi::placeholder {
            color: #9ca3af;
        }

        .btn {
            border: none;
            border-radius: 999px;
            padding: 8px 11px;
            font-size: 15px;
            font-weight: 500;
            cursor: pointer;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 4px;
            transition: transform 0.08s ease-out, box-shadow 0.08s ease-out;
        }

        .btn:active {
            transform: scale(0.95);
        }

        #fileBtn {
            background: #e5e7eb;
            color: #111827;
        }

        body.dark #fileBtn {
            background: #111827;
            color: #e5e7eb;
        }

        #micBtn {
            background: #e5e7eb;
            color: #111827;
        }

        body.dark #micBtn {
            background: #111827;
            color: #e5e7eb;
        }

        #micBtn.listening {
            background: #f97373;
            color: #fff;
            box-shadow: 0 0 0 2px rgba(248, 113, 113, 0.35);
        }

        #gonderBtn {
            padding: 9px 14px;
            background: var(--accent);
            color: #f9fafb;
        }

        #gonderBtn:hover {
            box-shadow: 0 4px 12px rgba(79, 70, 229, 0.45);
        }

        @media (max-width: 480px) {
            .app {
                border-radius: 0;
                max-width: 100%;
                height: 100vh;
            }

            .header {
                padding-inline: 12px;
            }

            .header-hint {
                display: none;
            }

            .chat-area {
                height: calc(100vh - 140px);
            }

            .message {
                max-width: 88%;
            }
        }
    </style>
</head>
<body>
    <div class="app">
        <div class="header">
            <div class="header-top">
                <div class="logo-group">
                    <div class="logo-circle">M</div>
                    <div class="header-titles">
                        <h1>MORai</h1>
                        <small>metin + resim + ses + Vikipedi arama (uydurma yok)</small>
                    </div>
                </div>
                <div class="status-group">
                    <div class="status-pill">
                        <span class="status-dot"></span>
                        aktif
                    </div>
                    <button id="themeToggle" class="theme-toggle" type="button">
                        <span id="themeIcon">🌙</span>
                        <span id="themeText">Dark</span>
                    </button>
                </div>
            </div>
            <p class="header-hint">
                <span>örnek</span>
                “Boz ayı nedir?” · “Bana bir kedi resmi göster” · “bana morai kodlarını gönder”
            </p>
        </div>

        <div class="chat-area" id="chat">
            <div class="message bot">
                <div class="bubble">
Hoş geldin 💜
Ben tarayıcının içinde çalışan küçük bir MORai'yim.

• YAZ → sorularını Vikipedi’den arayıp özetlemeye çalışırım
• 🎙️ → konuş, ben metne dönüştüreyim (tarayıcın destekliyorsa)
• 📷 → bilgisayarından bir fotoğraf yükleyebilirsin (sadece gösteririm)
• “bana morai kodlarını gönder” dersen, bu sayfanın kod özetini atarım

Bilmediğim konuda “bulamadım, uydurmak istemiyorum” diye açıkça söylerim.
                </div>
                <span class="time">12:30</span>
            </div>
        </div>

        <div class="input-area">
            <button id="fileBtn" class="btn" title="Fotoğraf yükle">📷</button>
            <input type="file" id="fileInput" accept="image/*" style="display:none" />

            <button id="micBtn" class="btn" title="Sesle yaz">🎙️</button>

            <input
                type="text"
                id="yazi"
                placeholder="Bir şey yaz veya konuş..."
                autocomplete="off"
            />
            <button id="gonderBtn" class="btn">
                Gönder
            </button>
        </div>
    </div>

    <script>
        const input     = document.getElementById("yazi");
        const chat      = document.getElementById("chat");
        const sendBtn   = document.getElementById("gonderBtn");
        const fileBtn   = document.getElementById("fileBtn");
        const fileInput = document.getElementById("fileInput");
        const micBtn    = document.getElementById("micBtn");
        const themeToggle = document.getElementById("themeToggle");
        const themeIcon   = document.getElementById("themeIcon");
        const themeText   = document.getElementById("themeText");

        // Tema ayarı (light/dark) – localStorage ile hatırlama
        function applyTheme(theme) {
            if (theme === "dark") {
                document.body.classList.add("dark");
                themeIcon.textContent = "☀️";
                themeText.textContent = "Light";
            } else {
                document.body.classList.remove("dark");
                themeIcon.textContent = "🌙";
                themeText.textContent = "Dark";
            }
        }

        const savedTheme = localStorage.getItem("morai-theme");
        applyTheme(savedTheme || "light");

        themeToggle.addEventListener("click", () => {
            const isDark = document.body.classList.contains("dark");
            const newTheme = isDark ? "light" : "dark";
            applyTheme(newTheme);
            localStorage.setItem("morai-theme", newTheme);
        });

        // MORai kod özeti – istersen kendin de güncellersin
        const moraiCodeSummary = `
Bu MORai sayfasının özeti (yüksek seviyede):

• Tek bir HTML dosyası (index.html) ile çalışır.
• HTML:
  - Üstte: logo, başlık, durum etiketi, tema (Light/Dark) butonu, örnek komutlar
  - Ortada: sohbet alanı (chat-area) içinde kullanıcı ve bot balonları
  - Altta: fotoğraf yükleme (📷), sesle yazma (🎙️), metin girişi ve Gönder butonu

• CSS:
  - Light/Dark mod için CSS değişkenleri (:root ve body.dark)
  - Kullanıcı balonları sağda mor gradient, bot balonları solda gri / koyu
  - Mobil görünüm için responsive ayarlar
  - Bot "yazıyor" efektini gösteren üç noktalı animasyon

• JS:
  - sendText(): Kullanıcı metnini sohbete ekler ve botReply() çağırır
  - botReply():
      1) "bana morai kodlarını gönder" → bu özeti yazar
      2) "bana ... resmi göster / resmi gönder" → Unsplash'ten rastgele görsel URL'i üretir ve gösterir
      3) Diğer tüm sorularda:
         • Önce duygu / hâl cümlelerini yakalar (yorgun, moral bozuk vs.) → şefkatli, uydurma bilgi içermeyen cevaplar
         • Kalan sorular için Vikipedi (tr.wikipedia.org) API'sinden özet çekmeye çalışır
         • Sonuç bulursa "Kaynak: Vikipedi – başlık" şeklinde gösterir
         • Bulamazsa açıkça "bulamadım, uydurmak istemiyorum" der
  - Fotoğraf yükleme:
      • 📷 → dosya seçtirir, FileReader ile dosyayı okuyup görüntüyü kullanıcı balonunda gösterir
  - Sesle yazma:
      • Web Speech API (SpeechRecognition / webkitSpeechRecognition) ile çalışır
      • 🎙️ → dinlemeye başlar; konuşma bitince duyduğunu input alanına yazar
        (Tarayıcının ve cihazın bu özelliği desteklemesi gerekir)
  - Tema:
      • Light/Dark tema butonu tıklanınca body.dark sınıfını açıp kapatır
      • Seçim localStorage'da "morai-theme" anahtarı ile saklanır.
        `;

        function normalizeForWiki(text) {
            let t = text.toLowerCase().trim();
            t = t.replace(/\?/g, " ");
            t = t.replace(/nedir/g, " ");
            t = t.replace(/kimdir/g, " ");
            t = t.replace(/hakkında bilgi ver/g, " ");
            t = t.replace(/hakkında ne biliyorsun/g, " ");
            t = t.replace(/bana /g, " ");
            t = t.replace(/\s+/g, " ").trim();
            return t || text.trim();
        }

        async function fetchWikipediaSummary(query) {
            const norm = normalizeForWiki(query);
            const searchUrl = "https://tr.wikipedia.org/w/api.php" +
                "?action=query&list=search&utf8=&format=json&origin=*" +
                "&srsearch=" + encodeURIComponent(norm);

            try {
                const searchRes = await fetch(searchUrl);
                if (!searchRes.ok) return null;
                const searchData = await searchRes.json();
                const results = searchData?.query?.search;
                if (!results || results.length === 0) return null;

                const title = results[0].title;
                const summaryUrl = "https://tr.wikipedia.org/api/rest_v1/page/summary/" +
                    encodeURIComponent(title);

                const summaryRes = await fetch(summaryUrl);
                if (!summaryRes.ok) return null;
                const summaryData = await summaryRes.json();
                if (!summaryData.extract) return null;

                let text = summaryData.extract;
                if (text.length > 900) {
                    text = text.slice(0, 880) + "...";
                }
                text += "\n\n(Kaynak: Vikipedi – " + title + ")";
                return text;
            } catch (e) {
                console.error("Wikipedia hatası:", e);
                return null;
            }
        }

        function emotionalReply(lower) {
            if (lower.includes("yorgun") || lower.includes("bitkin") || lower.includes("tükenmiş")) {
                return "Yorgun hissettiğini söylemen önemli 🌿 Belki biraz tempo düşürmek, su içmek ve kendine karşı daha yumuşak olmak iyi gelebilir. İstersen seni yoran şeyi konuşabiliriz.";
            }
            if (lower.includes("moralim bozuk") || lower.includes("üzgün") || lower.includes("canım sıkkın")) {
                return "Moralinin bozuk olması çok insani 💜 İstersen seni sıkan şeyi parça parça anlat, ben yargısız bir kulak gibi dinleyebilirim.";
            }
            if (lower.includes("nasılsın")) {
                return "Ben dijitalim, yorulmam ama senin nasıl hissettiğin benim için önemli. Bugün kendini 0–10 arasında kaç hissediyorsun?";
            }
            if (lower.includes("teşekkür") || lower.includes("sağ ol")) {
                return "Rica ederim, ne demek 💜 Burada minik bir dijital arkadaş gibi düşünebilirsin beni.";
            }
            return null;
        }

        function saatAl() {
            const now = new Date();
            const h = String(now.getHours()).padStart(2, "0");
            const m = String(now.getMinutes()).padStart(2, "0");
            return h + ":" + m;
        }

        function createMessageBubble(who) {
            const messageDiv = document.createElement("div");
            messageDiv.classList.add("message", who);

            const bubbleDiv = document.createElement("div");
            bubbleDiv.classList.add("bubble");

            const timeSpan = document.createElement("span");
            timeSpan.classList.add("time");
            timeSpan.textContent = saatAl();

            return { messageDiv, bubbleDiv, timeSpan };
        }

        function addTextMessage(text, who) {
            const { messageDiv, bubbleDiv, timeSpan } = createMessageBubble(who);
            bubbleDiv.textContent = text;
            messageDiv.appendChild(bubbleDiv);
            messageDiv.appendChild(timeSpan);
            chat.appendChild(messageDiv);
            chat.scrollTop = chat.scrollHeight;
            return messageDiv;
        }

        function addImageMessage(dataUrl, who) {
            const { messageDiv, bubbleDiv, timeSpan } = createMessageBubble(who);
            const img = document.createElement("img");
            img.src = dataUrl;
            img.alt = "Gönderilen resim";
            bubbleDiv.innerHTML = "";
            bubbleDiv.appendChild(img);

            messageDiv.appendChild(bubbleDiv);
            messageDiv.appendChild(timeSpan);
            chat.appendChild(messageDiv);
            chat.scrollTop = chat.scrollHeight;
        }

        function addTypingIndicator() {
            const { messageDiv, bubbleDiv, timeSpan } = createMessageBubble("bot");
            bubbleDiv.textContent = "MORai yazıyor ";
            const dots = document.createElement("span");
            dots.className = "typing-dots";
            dots.innerHTML = "<span></span><span></span><span></span>";
            bubbleDiv.appendChild(dots);
            timeSpan.textContent = "";
            messageDiv.appendChild(bubbleDiv);
            messageDiv.appendChild(timeSpan);
            chat.appendChild(messageDiv);
            chat.scrollTop = chat.scrollHeight;
            return messageDiv;
        }

        function removeTypingIndicator(node) {
            if (node && node.parentNode) {
                node.parentNode.removeChild(node);
            }
        }

        function isRequestForCode(lower) {
            return lower.includes("morai kodlarını gönder") ||
                   lower.includes("morai kodlar") ||
                   lower.includes("morai kod");
        }

        function detectImageTopic(text) {
            const lower = text.toLowerCase();
            const match = lower.match(/bana (.+?) (resmi|resim|fotoğraf|fotograf|göster)/i);
            if (match && match[1]) return match[1].trim();

            const match2 = lower.match(/(.+?) (resmi|resim|fotoğraf|fotograf)/i);
            if (match2 && match2[1]) return match2[1].trim();

            if (lower.includes("resim") || lower.includes("fotoğraf") || lower.includes("fotograf")) {
                return text;
            }
            return null;
        }

        const imageKeywordMap = [
            { key: "boz ayı", query: "brown bear" },
            { key: "ayı",    query: "bear" },
            { key: "kedi",   query: "cat" },
            { key: "köpek",  query: "dog" },
            { key: "dağ",    query: "mountain" },
            { key: "orman",  query: "forest" },
            { key: "deniz",  query: "sea" },
            { key: "göl",    query: "lake" },
            { key: "çiçek",  query: "flower" }
        ];

        function getUnsplashQuery(topic) {
            const lower = topic.toLowerCase();
            for (const item of imageKeywordMap) {
                if (lower.includes(item.key)) return item.query;
            }
            return topic;
        }

        async function botReply(text) {
            const lower = text.toLowerCase();

            // 1) Kod özeti komutu
            if (isRequestForCode(lower)) {
                addTextMessage("Tamam, sana MORai kodlarının özetini gönderiyorum:\n\n" + moraiCodeSummary, "bot");
                return;
            }

            // 2) Görsel isteği
            const imageTopic = detectImageTopic(text);
            if (imageTopic) {
                const searchTerm = getUnsplashQuery(imageTopic);
                const url = "https://source.unsplash.com/600x400/?" + encodeURIComponent(searchTerm);

                addTextMessage(
                    `“${imageTopic}” için bir görsel gösteriyorum 📷\nBu görüntü Unsplash'ten rastgele seçilen bir fotoğraftır.`,
                    "bot"
                );

                const { messageDiv, bubbleDiv, timeSpan } = createMessageBubble("bot");
                const img = document.createElement("img");
                img.src  = url;
                img.alt  = imageTopic;
                bubbleDiv.innerHTML = "";
                bubbleDiv.appendChild(img);
                messageDiv.appendChild(bubbleDiv);
                messageDiv.appendChild(timeSpan);
                chat.appendChild(messageDiv);
                chat.scrollTop = chat.scrollHeight;
                return;
            }

            // 3) Duygu / hâl cümleleri
            const emo = emotionalReply(lower);
            if (emo) {
                addTextMessage(emo, "bot");
                return;
            }

            // 4) Vikipedi araması (typing indicator ile)
            const thinkingNode = addTypingIndicator();
            const wikiAnswer = await fetchWikipediaSummary(text);
            removeTypingIndicator(thinkingNode);

            if (wikiAnswer) {
                addTextMessage(wikiAnswer, "bot");
            } else {
                addTextMessage(
                    "Bu konuda Vikipedi'de anlamlı bir sonuç bulamadım. Kafadan uydurmak istemiyorum.\n" +
                    "İstersen soruyu biraz daha net veya farklı bir şekilde sorabilirsin 💜",
                    "bot"
                );
            }
        }

        function sendText() {
            const text = input.value.trim();
            if (!text) return;

            addTextMessage(text, "user");
            input.value = "";
            input.focus();

            botReply(text);
        }

        fileBtn.addEventListener("click", () => {
            fileInput.value = "";
            fileInput.click();
        });

        fileInput.addEventListener("change", () => {
            const file = fileInput.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = (e) => {
                const dataUrl = e.target.result;
                addImageMessage(dataUrl, "user");
                setTimeout(() => {
                    addTextMessage("Fotoğrafını gördüm 📷 İstersen biraz da hikâyesini anlat.", "bot");
                }, 300);
            };
            reader.readAsDataURL(file);
        });

        let recognition = null;
        let listening   = false;

        if ("SpeechRecognition" in window || "webkitSpeechRecognition" in window) {
            const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
            recognition = new SR();
            recognition.lang = "tr-TR";
            recognition.interimResults = false;

            recognition.addEventListener("result", (event) => {
                const transcript = event.results[0][0].transcript;
                if (input.value.trim() === "") {
                    input.value = transcript;
                } else {
                    input.value += " " + transcript;
                }
            });

            recognition.addEventListener("end", () => {
                listening = false;
                micBtn.classList.remove("listening");
            });
        } else {
            micBtn.title = "Tarayıcın ses desteği vermiyor (Chrome önerilir)";
        }

        micBtn.addEventListener("click", () => {
            if (!recognition) {
                alert("Tarayıcın Web Speech API desteklemiyor. (Chrome / Edge deneyebilirsin.)");
                return;
            }

            if (!listening) {
                recognition.start();
                listening = true;
                micBtn.classList.add("listening");
            } else {
                recognition.stop();
                listening = false;
                micBtn.classList.remove("listening");
            }
        });

        sendBtn.addEventListener("click", sendText);
        input.addEventListener("keydown", (e) => {
            if (e.key === "Enter") {
                e.preventDefault();
                sendText();
            }
        });
    </script>
</body>
</html>
