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

        body {
            font-family: system-ui, sans-serif;
            background: var(--bg-gradient);
            margin: 0;
            height: 100vh;
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
            height: 100vh;
        }

        .header {
            padding: 14px 18px 10px;
            background: var(--header-bg);
            color: #f9fafb;
        }

        .chat-area {
            flex: 1;
            padding: 12px 10px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .message {
            max-width: 80%;
            display: flex;
            flex-direction: column;
        }

        .message.user {
            align-self: flex-end;
            text-align: right;
        }

        .message.bot {
            align-self: flex-start;
        }

        .bubble {
            padding: 10px 13px;
            background: var(--bubble-bot);
            border-radius: 16px;
            white-space: pre-line;
        }

        .message.user .bubble {
            background: var(--bubble-user);
            color: #ffffff;
        }

        .bubble img {
            max-width: 260px;
            border-radius: 12px;
        }

        .time {
            font-size: 11px;
            margin-top: 2px;
            opacity: 0.7;
        }

        .input-area {
            display: flex;
            gap: 8px;
            padding: 10px;
            border-top: 1px solid #ddd;
        }

        input[type=text] {
            flex: 1;
            padding: 10px;
            border-radius: 50px;
            border: 1px solid #aaa;
            font-size: 15px;
        }

        .btn {
            border-radius: 50%;
            width: 42px;
            height: 42px;
            border: none;
            cursor: pointer;
            font-size: 18px;
        }

        #gonderBtn {
            background: var(--accent);
            color: #fff;
        }
    </style>
</head>
<body>
<div class="app">
    <div class="header">
        <h2>MORai v2.1</h2>
        <small>Ses + Fotoğraf + Vikipedi</small>
    </div>

    <div class="chat-area" id="chat">
        <div class="message bot">
            <div class="bubble">Hey! 🤖  
Ben **MORai** senin yapay zekânım.  
Bana bir şey yaz ya da sor:
• Boz ayı nedir?
• Bana bir kedi resmi göster
📷 Fotoğraf at, 🎙️ Konuş → Anlarım 😎</div>
            <div class="time">--:--</div>
        </div>
    </div>

    <div class="input-area">
        <button id="fileBtn" class="btn">📷</button>
        <input type="file" id="fileInput" accept="image/*" style="display:none" />

        <button id="micBtn" class="btn">🎙️</button>
        <input type="text" id="yazi" placeholder="Mesaj yaz..." />
        <button id="gonderBtn" class="btn">➡️</button>
    </div>
</div>
<script>
    const chat      = document.getElementById("chat");
    const input     = document.getElementById("yazi");
    const sendBtn   = document.getElementById("gonderBtn");
    const fileBtn   = document.getElementById("fileBtn");
    const fileInput = document.getElementById("fileInput");
    const micBtn    = document.getElementById("micBtn");

    // MORai kod özeti – "bana morai kodlarını gönder" için
    const moraiCodeSummary = `
Bu MORai v2.1 sayfasının özeti:

• Tek dosya: index.html
• Özellikler:
  - Metinle sohbet (kullanıcı mesajı + bot cevabı)
  - Vikipedi'den (tr.wikipedia.org) bilgi çekip özetleme
  - Fotoğraf yükleme (bilgisayardan seçilen resim balon içinde gösteriliyor)
  - Sesle yazma (Web Speech API, tarayıcı destekliyorsa)
  - Basit, modern arayüz

• JS:
  - sendText(): Kullanıcı metnini sohbete ekler, sonra botReply() çağırır
  - botReply():
      1) "bana morai kodlarını gönder" → bu açıklamayı geri yollar
      2) "bana ... resmi göster / resmi gönder" → Unsplash'ten rastgele görsel URL'i oluşturur
      3) Duygu/hâl cümlelerinde (yorgun, moral bozuk vb.) hazır şefkatli cevaplar verir
      4) Diğer sorular için:
          • Vikipedi'de arama yapar (search API + summary)
          • Sonuç bulursa özet + "Kaynak: Vikipedi – başlık"
          • Bulamazsa "bulamadım, uydurmak istemiyorum" diye dürüstçe söyler
  - Fotoğraf yükleme:
      • File input'tan seçilen resmi URL.createObjectURL ile gösterir
  - Sesle yazma:
      • SpeechRecognition / webkitSpeechRecognition (tr-TR) ile konuşmayı metne çevirir.
    `;

    function saatAl() {
        const now = new Date();
        const h = String(now.getHours()).padStart(2, "0");
        const m = String(now.getMinutes()).padStart(2, "0");
        return `${h}:${m}`;
    }

    function createMessage(who) {
        const wrapper = document.createElement("div");
        wrapper.className = `message ${who}`;

        const bubble = document.createElement("div");
        bubble.className = "bubble";

        const time = document.createElement("div");
        time.className = "time";
        time.textContent = saatAl();

        wrapper.appendChild(bubble);
        wrapper.appendChild(time);
        chat.appendChild(wrapper);
        chat.scrollTop = chat.scrollHeight;

        return { wrapper, bubble, time };
    }

    function addTextMessage(text, who) {
        const { bubble } = createMessage(who);
        bubble.textContent = text;
    }

    function addImageMessage(url, who) {
        const { bubble } = createMessage(who);
        const img = document.createElement("img");
        img.src = url;
        img.alt = "Gönderilen resim";
        bubble.innerHTML = "";
        bubble.appendChild(img);
    }

    // Metni Vikipedi araması için sadeleştirme
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

    // Vikipedi'den özet çekme
    async function fetchWikipediaSummary(query) {
        const norm = normalizeForWiki(query);
        const searchUrl =
            "https://tr.wikipedia.org/w/api.php" +
            "?action=query&list=search&utf8=&format=json&origin=*" +
            "&srsearch=" + encodeURIComponent(norm);

        try {
            const searchRes = await fetch(searchUrl);
            if (!searchRes.ok) return null;
            const searchData = await searchRes.json();
            const results = searchData?.query?.search;
            if (!results || results.length === 0) return null;

            const title = results[0].title;
            const summaryUrl =
                "https://tr.wikipedia.org/api/rest_v1/page/summary/" +
                encodeURIComponent(title);

            const summaryRes = await fetch(summaryUrl);
            if (!summaryRes.ok) return null;
            const summaryData = await summaryRes.json();
            if (!summaryData.extract) return null;

            let text = summaryData.extract;
            if (text.length > 900) {
                text = text.slice(0, 880) + "...";
            }
            text += `\n\n(Kaynak: Vikipedi – ${title})`;
            return text;
        } catch (e) {
            console.error("Vikipedi hatası:", e);
            return null;
        }
    }

    // Duygu / ruh hali bazlı cevaplar (uydurma bilgi yok)
    function emotionalReply(lower) {
        if (lower.includes("yorgun") || lower.includes("bitkin") || lower.includes("tükenmiş")) {
            return "Yorgun hissettiğini söylemen önemli 🌿 Belki biraz tempo düşürmek, su içmek ve kendine karşı daha yumuşak olmak iyi gelebilir. İstersen seni yoran şeyi konuşabiliriz.";
        }
        if (lower.includes("moralim bozuk") || lower.includes("üzgün") || lower.includes("canım sıkkın")) {
            return "Moralinin bozuk olması çok insani 💜 İstersen seni sıkan şeyi parça parça anlat, ben yargısız bir kulak gibi dinleyebilirim.";
        }
        if (lower.includes("nasılsın")) {
            return "Ben dijitalim, yorulmam ama senin nasıl hissettiğin önemli. Bugün kendini 0–10 arasında kaç hissediyorsun?";
        }
        if (lower.includes("teşekkür") || lower.includes("sağ ol")) {
            return "Rica ederim, ne demek 💜 Küçük de olsa yanında hissettirebiliyorsam ne mutlu bana.";
        }
        return null;
    }

    function isRequestForCode(lower) {
        return (
            lower.includes("morai kodlarını gönder") ||
            lower.includes("morai kodlar") ||
            lower.includes("morai kod")
        );
    }

    // Görsel isteklerini yakalama ("bana ... resmi göster" gibi)
    function detectImageTopic(text) {
    const lower = text.toLowerCase();

    // Eğer mesajda resim/foto/fotoğraf/görsel geçiyorsa
    // her koşulda bir görsel isteği olarak kabul et
    if (
        lower.includes("resim") ||
        lower.includes("fotoğraf") ||
        lower.includes("fotograf") ||
        lower.includes("foto") ||
        lower.includes("görsel") ||
        lower.includes("gorsel")
    ) {
        // Boz ayı, kedi vs gibi özel bir şey geçiyorsa onu yakalamaya çalış
        if (lower.includes("boz ayı")) return "brown bear";
        if (lower.includes("ayı"))     return "bear";
        if (lower.includes("kedi"))    return "cat";
        if (lower.includes("köpek"))   return "dog";
        if (lower.includes("dağ"))     return "mountain";
        if (lower.includes("orman"))   return "forest";
        if (lower.includes("deniz"))   return "sea";

        // Hiçbiri yoksa, yine de mesajın tamamını konu kabul et
        return text;
    }

    // Hiç resim/foto kelimesi yoksa → resim isteği değildir
    return null;
}
    const imageKeywordMap = [
        { key: "boz ayı", query: "brown bear" },
        { key: "ayı",     query: "bear" },
        { key: "kedi",    query: "cat" },
        { key: "köpek",   query: "dog" },
        { key: "dağ",     query: "mountain" },
        { key: "orman",   query: "forest" },
        { key: "deniz",   query: "sea" },
        { key: "göl",     query: "lake" },
        { key: "çiçek",   query: "flower" }
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

        // 1) Kod özeti isteği
        if (isRequestForCode(lower)) {
            addTextMessage(
                "Tamam, sana MORai kodlarının özetini gönderiyorum:\n\n" + moraiCodeSummary,
                "bot"
            );
            return;
        }

        // 2) Görsel isteği
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

        // 3) Duygu / hâl cevapları
        const emo = emotionalReply(lower);
        if (emo) {
            addTextMessage(emo, "bot");
            return;
        }

        // 4) Vikipedi'den bilgi çekme
        addTextMessage("Sorunu Vikipedi'de arıyorum, uygun bir özet bulmaya çalışıyorum…", "bot");

        const wikiAnswer = await fetchWikipediaSummary(text);
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

    // 📷 Fotoğraf yükleme (blob URL ile)
    fileBtn.addEventListener("click", () => {
        fileInput.value = "";
        fileInput.click();
    });

    fileInput.addEventListener("change", () => {
        const file = fileInput.files[0];
        if (!file) return;

        const objectUrl = URL.createObjectURL(file);
        addImageMessage(objectUrl, "user");

        setTimeout(() => {
            addTextMessage(
                "Fotoğrafını gördüm 📷 İstersen biraz da hikâyesini anlat.",
                "bot"
            );
        }, 300);

        // Bir süre sonra RAM'i rahatlatmak için serbest bırak
        setTimeout(() => URL.revokeObjectURL(objectUrl), 10000);
    });

    // 🎙️ Sesle yazma (Web Speech API)
    let recognition = null;
    let listening = false;

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
        } else {
            recognition.stop();
            listening = false;
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
