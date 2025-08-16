<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>Script Manager</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    /* ====== Base / Elite Neon Theme ====== */
    :root{
      --bg1:#0a0a0a; --bg2:#1e1e1e; --card:#151515;
      --neon:#00e6ff; --neon2:#0077ff; --gold:#ffd54d;
      --text:#f5f5f5;
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0; font-family:"Segoe UI",system-ui,-apple-system,Arial,sans-serif;
      background:linear-gradient(135deg,var(--bg1),var(--bg2));
      color:var(--text); text-align:center;
    }
    header{
      padding:18px 20px; background:#0a0a0a; border-bottom:2px solid var(--neon);
      display:flex; justify-content:space-between; align-items:center; position:sticky; top:0; z-index:10;
    }
    header h1{
      margin:0; font-size:28px;
      background:linear-gradient(90deg,var(--neon),var(--neon2));
      -webkit-background-clip:text; -webkit-text-fill-color:transparent;
    }
    .lang-switch{
      cursor:pointer; font-size:14px; background:none; color:var(--neon);
      border:1px solid var(--neon); border-radius:10px; padding:6px 10px; transition:.25s;
    }
    .lang-switch:hover{background:var(--neon); color:#000}
    /* Screens */
    .screen{display:none; padding:40px 20px; animation:fadeIn .5s ease}
    #welcome{display:flex; flex-direction:column; justify-content:center; align-items:center; min-height:80vh}
    button{
      margin:12px; padding:14px 30px; border:none; border-radius:14px; font-size:17px; cursor:pointer;
      background:linear-gradient(135deg,var(--neon),var(--neon2)); color:#001018; font-weight:700; transition:.25s;
      box-shadow:0 0 12px var(--neon);
    }
    button:hover{transform:translateY(-1px) scale(1.03); box-shadow:0 0 24px var(--neon)}
    .btn-ghost{background:#0f0f12; color:var(--text); border:1px solid #2a2a2a; box-shadow:none}
    .btn-ghost:hover{border-color:var(--neon)}
    /* Cards */
    .cards{display:grid; grid-template-columns:repeat(auto-fit,minmax(260px,420px)); gap:20px; justify-content:center; margin:20px auto; max-width:900px}
    .card{
      background:var(--card); border-radius:16px; padding:22px; text-align:left; cursor:pointer;
      box-shadow:0 0 10px rgba(0,255,255,.25); transition:.25s; border:1px solid transparent;
    }
    .card:hover{transform:translateY(-2px); box-shadow:0 0 18px rgba(0,255,255,.5)}
    .card .price{margin-top:10px; color:var(--neon); font-weight:700}
    .card.selected{border:2px solid var(--neon); box-shadow:0 0 22px var(--neon)}
    .card.premium{border:2px solid var(--gold); box-shadow:0 0 22px var(--gold), 0 0 60px rgba(255,213,77,.25); position:relative}
    .card.premium:after{
      content:"PREMIUM"; position:absolute; top:12px; right:14px; font-size:12px; letter-spacing:.12em;
      color:#1a1200; background:linear-gradient(135deg,#ffe27a,#ffc107); padding:4px 8px; border-radius:999px; font-weight:800;
    }
    @keyframes fadeIn{from{opacity:0; transform:translateY(6px)} to{opacity:1; transform:none}}
    input{
      width:280px; max-width:95%; padding:12px; border-radius:10px; border:1px solid var(--neon);
      background:#0f0f0f; color:var(--neon); font-size:16px; outline:none;
    }
    /* Result */
    #resultBox{
      white-space:pre-line; text-align:left; max-width:720px; margin:20px auto; background:#0f0f10;
      padding:20px; border-radius:14px; box-shadow:0 0 18px rgba(0,255,255,.35); border:1px solid #1d1f22;
      font-size:15.5px;
    }
    .actions{display:flex; flex-wrap:wrap; gap:12px; justify-content:center}
    .tg-btn{
      display:inline-block; padding:12px 22px; border-radius:10px; text-decoration:none; font-weight:800; font-size:16px;
      background:linear-gradient(135deg,var(--neon),var(--neon2)); color:#001018; box-shadow:0 0 12px var(--neon);
    }
    .tg-btn:hover{box-shadow:0 0 22px var(--neon)}
    a{color:var(--neon)}
  </style>
</head>
<body>
  <header>
    <h1 id="title">⚡ Скрипты</h1>
    <button class="lang-switch" id="langBtn" onclick="toggleLang()">🌐 EN</button>
  </header>

  <!-- Welcome -->
  <section id="welcome">
    <h2 id="welcomeText">👋 Добро пожаловать</h2>
    <div class="actions">
      <button id="chooseBtn" onclick="showScreen('scripts')">🚀 Выбрать скрипт</button>
      <button id="howBtn" class="btn-ghost" onclick="showScreen('how')">ℹ️ Как работает</button>
    </div>
  </section>

  <!-- Scripts -->
  <section id="scripts" class="screen">
    <h2 id="scriptChoose">Выберите скрипт</h2>
    <div class="cards" id="scriptCards">
      <div class="card" data-id="basic">
        <h3 id="basicName">🎁 Обычный</h3>
        <p id="basicDesc">Проверка подарков каждые <b>5 сек – ∞</b>. Стабильно и без лишней нагрузки.</p>
        <p class="price" id="basicPrice">Цена: 5000₽ / 55$ / 4000⭐</p>
      </div>
      <div class="card premium" data-id="premium">
        <h3 id="premiumName">💎 Премиум</h3>
        <p id="premiumDesc">Проверка подарков каждые <b>0.2 сек – ∞</b>. Максимальная скорость и приоритет.</p>
        <p class="price" id="premiumPrice">Цена: 13500₽ / 140$ / 12900⭐</p>
      </div>
    </div>
    <div class="actions">
      <button id="toPayBtn" onclick="showScreen('pay')">Продолжить к оплате</button>
      <button id="backBtn1" class="btn-ghost" onclick="back()">⬅️ Назад</button>
    </div>
  </section>

  <!-- Payment -->
  <section id="pay" class="screen">
    <h2 id="payTitle">💳 Оплата</h2>
    <p id="choosePayText">Выберите валюту:</p>
    <div class="cards" id="payments">
      <div class="card" data-method="₽" id="payRub">🇷🇺 Рубли (₽)</div>
      <div class="card" data-method="$" id="payUsd">💵 Доллары ($)</div>
      <div class="card" data-method="⭐" id="payStar">⭐ Stars (⭐)</div>
    </div>
    <input type="text" id="promo" placeholder="Введите промокод (если есть)" />
    <div class="actions">
      <button id="toResultBtn" onclick="goResult()">Продолжить</button>
      <button id="backBtn2" class="btn-ghost" onclick="back()">⬅️ Назад</button>
    </div>
  </section>

  <!-- Result -->
  <section id="resultScreen" class="screen">
    <h2 id="resultTitle">📩 Ваша информация</h2>
    <div id="resultBox"></div>
    <div class="actions">
      <button id="copyBtn" onclick="copyText()">📋 Скопировать</button>
      <a id="tgBtn1" class="tg-btn" target="_blank">✈️ Написать @arh_h</a>
      <a id="tgBtn2" class="tg-btn" target="_blank">✈️ Написать @juilly</a>
    </div>
    <div class="actions" style="margin-top:10px">
      <button id="backBtn3" class="btn-ghost" onclick="back()">⬅️ На главную</button>
    </div>
  </section>

  <!-- How it works -->
  <section id="how" class="screen">
    <h2 id="howTitle">ℹ️ Как всё работает</h2>
    <p id="howText" style="max-width:900px;margin:0 auto;line-height:1.6;text-align:left">
      <!-- Заполняется из переводов -->
    </p>
    <div class="actions">
      <button id="backBtn4" class="btn-ghost" onclick="back()">⬅️ Назад</button>
    </div>
  </section>

  <script>
    let lang = "ru";
    let selectedScript = null;   // { id, name, desc, prices }
    let selectedPayment = null;  // "₽" | "$" | "⭐"

    const translations = {
      ru: {
        title:"⚡ Скрипты",
        welcome:"👋 Добро пожаловать",
        choose:"🚀 Выбрать скрипт",
        how:"ℹ️ Как работает",
        scriptChoose:"Выберите скрипт",
        scripts:{
          basic:{
            name:"🎁 Обычный",
            desc:"Проверка подарков каждые 5 секунд – ∞. Стабильная работа без лишней нагрузки. Подходит для постоянного мониторинга каналов и минимальных рисков.",
            priceText:"Цена: 5000₽ / 55$ / 4000⭐",
            prices:{ "₽":5000, "$":55, "⭐":4000 }
          },
          premium:{
            name:"💎 Премиум",
            desc:"Проверка подарков каждые 0.2 секунды – ∞. Максимальная скорость отклика и приоритет при выкупе. Идеален для высококонкурентных раздач.",
            priceText:"Цена: 13500₽ / 140$ / 12900⭐",
            prices:{ "₽":13500, "$":140, "⭐":12900 }
          }
        },
        toPay:"Продолжить к оплате",
        back:"⬅️ Назад",
        payTitle:"💳 Оплата",
        choosePay:"Выберите валюту:",
        promoPH:"Введите промокод (если есть)",
        toResult:"Продолжить",
        resultTitle:"📩 Ваша информация",
        copy:"📋 Скопировать",
        tg1:"✈️ Написать @arh_h",
        tg2:"✈️ Написать @juilly",
        backHome:"⬅️ На главную",
        currencies:{ "₽":"🇷🇺 Рубли (₽)", "$":"💵 Доллары ($)", "⭐":"⭐ Stars (⭐)" },
        alerts:{ select:"Выберите скрипт и валюту!", copied:"Сообщение скопировано ✅" },
        howLong: `
🔹 <b>Что вы получаете</b><br>
• Личный сайт управления (после оплаты): добавление токена, запуск/остановка, настройка интервалов и уведомлений.<br>
• Поддержка: <a href="https://t.me/arh_h" target="_blank">@arh_h</a> и <a href="https://t.me/juilly" target="_blank">@juilly</a>.<br><br>
🔹 <b>Режимы работы</b><br>
• Обычный — интервал проверки <b>5 сек – ∞</b>: разумная нагрузка, стабильность.<br>
• Премиум — интервал проверки <b>0.2 сек – ∞</b>: ультра-частые проверки и мгновенная реакция.<br><br>
🔹 <b>Как использовать</b><br>
1) Выберите скрипт и валюту, при желании введите промокод.<br>
2) На странице итогов скопируйте сообщение и отправьте его в Telegram (<a href="https://t.me/arh_h" target="_blank">@arh_h</a> или <a href="https://t.me/juilly" target="_blank">@juilly</a>).<br>
3) После оплаты вы получите доступ к личному сайту управления и инструкцию по подключению токена.<br><br>
⚠️ <b>Примечание:</b> частота проверок влияет на нагрузку и расход лимитов. Премиум рассчитан на максимальную интенсивность.`,
        msgTemplate: (name,desc,method,price) =>
`Вы выбрали: ${name}
Описание: ${desc}
Оплата: ${method}
Цена: ${price}${method}

👉 Скопируйте это сообщение и отправьте владельцам: @arh_h или @juilly`
      },
      en: {
        title:"⚡ Scripts",
        welcome:"👋 Welcome",
        choose:"🚀 Choose Script",
        how:"ℹ️ How it works",
        scriptChoose:"Select a script",
        scripts:{
          basic:{
            name:"🎁 Basic",
            desc:"Checks gifts every 5 seconds – ∞. Stable operation with reasonable load. Great for continuous monitoring.",
            priceText:"Price: 5000₽ / 55$ / 4000⭐",
            prices:{ "₽":5000, "$":55, "⭐":4000 }
          },
          premium:{
            name:"💎 Premium",
            desc:"Checks gifts every 0.2 seconds – ∞. Ultra-fast response and top priority in competitive drops.",
            priceText:"Price: 13500₽ / 140$ / 12900⭐",
            prices:{ "₽":13500, "$":140, "⭐":12900 }
          }
        },
        toPay:"Proceed to payment",
        back:"⬅️ Back",
        payTitle:"💳 Payment",
        choosePay:"Select currency:",
        promoPH:"Enter promo code (optional)",
        toResult:"Continue",
        resultTitle:"📩 Your Information",
        copy:"📋 Copy",
        tg1:"✈️ Message @arh_h",
        tg2:"✈️ Message @juilly",
        backHome:"⬅️ Home",
        currencies:{ "₽":"🇷🇺 Rubles (₽)", "$":"💵 US Dollars ($)", "⭐":"⭐ Stars (⭐)" },
        alerts:{ select:"Please select a script and a currency!", copied:"Message copied ✅" },
        howLong: `
🔹 <b>What you get</b><br>
• Personal control site (after payment): add token, start/stop, interval & notifications.<br>
• Support: <a href="https://t.me/arh_h" target="_blank">@arh_h</a> and <a href="https://t.me/juilly" target="_blank">@juilly</a>.<br><br>
🔹 <b>Modes</b><br>
• Basic — check interval <b>5 s – ∞</b>: balanced load, stability.<br>
• Premium — check interval <b>0.2 s – ∞</b>: ultra-frequent checks and fastest reaction.<br><br>
🔹 <b>How to use</b><br>
1) Pick a script and currency, add a promo if you have one.<br>
2) On the summary page copy the message and send it via Telegram (<a href="https://t.me/arh_h" target="_blank">@arh_h</a> or <a href="https://t.me/juilly" target="_blank">@juilly</a>).<br>
3) After payment you’ll receive your personal control site and token setup instructions.<br><br>
⚠️ <b>Note:</b> higher frequency increases load and quota usage. Premium is built for maximum intensity.`,
        msgTemplate: (name,desc,method,price) =>
`You selected: ${name}
Description: ${desc}
Payment: ${method}
Price: ${price}${method}

👉 Copy this message and send to: @arh_h or @juilly`
      }
    };

    const promoCodes = { "VIP2025":0.8, "PREMIUMX":0.7, "FREEPASS":0.0 };

    function $(id){ return document.getElementById(id); }

    function applyTranslations(){
      const t = translations[lang];
      document.documentElement.lang = lang;
      $("title").innerHTML = t.title;
      $("welcomeText").innerHTML = t.welcome;
      $("chooseBtn").innerHTML = t.choose;
      $("howBtn").innerHTML = t.how;
      $("scriptChoose").innerHTML = t.scriptChoose;

      // Script cards
      $("basicName").innerHTML = t.scripts.basic.name;
      $("basicDesc").innerHTML = t.scripts.basic.desc.replaceAll("\n","<br>");
      $("basicPrice").innerHTML = t.scripts.basic.priceText;

      $("premiumName").innerHTML = t.scripts.premium.name;
      $("premiumDesc").innerHTML = t.scripts.premium.desc.replaceAll("\n","<br>");
      $("premiumPrice").innerHTML = t.scripts.premium.priceText;

      $("toPayBtn").innerHTML = t.toPay;
      $("backBtn1").innerHTML = t.back;

      $("payTitle").innerHTML = t.payTitle;
      $("choosePayText").innerHTML = t.choosePay;
      $("promo").placeholder = t.promoPH;
      $("toResultBtn").innerHTML = t.toResult;
      $("backBtn2").innerHTML = t.back;

      $("resultTitle").innerHTML = t.resultTitle;
      $("copyBtn").innerHTML = t.copy;
      $("tgBtn1").innerHTML = t.tg1;
      $("tgBtn2").innerHTML = t.tg2;
      $("backBtn3").innerHTML = t.backHome;

      $("howTitle").innerHTML = t.howTitle || t.how;
      $("howText").innerHTML = t.howLong;
      $("backBtn4").innerHTML = t.back;

      // Currencies text
      $("payRub").innerHTML = t.currencies["₽"];
      $("payUsd").innerHTML = t.currencies["$"];
      $("payStar").innerHTML = t.currencies["⭐"];

      // Update language button label
      $("langBtn").textContent = (lang==="ru") ? "🌐 EN" : "🌐 RU";

      // If user already selected a script, refresh it in current language
      if(selectedScript){
        const id = selectedScript.id; // "basic" | "premium"
        selectedScript = {
          id,
          name: t.scripts[id].name,
          desc: t.scripts[id].desc,
          prices: t.scripts[id].prices
        };
      }

      // If result screen is visible, rebuild the message text
      if(window.getComputedStyle($("resultScreen")).display !== "none"){
        buildResultMessage();
      }
    }

    function toggleLang(){
      lang = (lang === "ru") ? "en" : "ru";
      applyTranslations();
    }

    function showScreen(id){
      $("welcome").style.display = "none";
      document.querySelectorAll(".screen").forEach(s => s.style.display = "none");
      $(id).style.display = "block";
    }
    function back(){
      document.querySelectorAll(".screen").forEach(s => s.style.display = "none");
      $("welcome").style.display = "flex";
    }

    // Script selection
    document.querySelectorAll("#scriptCards .card").forEach(card=>{
      card.addEventListener("click", ()=>{
        document.querySelectorAll("#scriptCards .card").forEach(c=>c.classList.remove("selected"));
        card.classList.add("selected");
        const id = card.dataset.id; // basic | premium
        const t = translations[lang].scripts[id];
        selectedScript = { id, name: t.name, desc: t.desc, prices: t.prices };
      });
    });

    // Payment selection
    document.querySelectorAll("#payments .card").forEach(card=>{
      card.addEventListener("click", ()=>{
        document.querySelectorAll("#payments .card").forEach(c=>c.classList.remove("selected"));
        card.classList.add("selected");
        selectedPayment = card.dataset.method; // "₽" | "$" | "⭐"
      });
    });

    function goResult(){
      const tAll = translations[lang];
      if(!selectedScript || !selectedPayment){ alert(tAll.alerts.select); return; }
      buildResultMessage();
      showScreen("resultScreen");
    }

    function buildResultMessage(){
      const tAll = translations[lang];
      let discount = 1.0;
      const code = $("promo").value.trim();
      if(code && promoCodes[code] !== undefined) discount = promoCodes[code];

      const base = selectedScript.prices[selectedPayment]; // price in chosen currency
      const finalPrice = Math.round(base * discount);

      const message = tAll.msgTemplate(
        selectedScript.name,
        selectedScript.desc,
        selectedPayment,
        finalPrice
      );

      $("resultBox").textContent = message;
      // Telegram deeplinks
      const enc = encodeURIComponent(message);
      $("tgBtn1").href = "https://t.me/arh_h?text=" + enc;
      $("tgBtn2").href = "https://t.me/juilly?text=" + enc;
    }

    function copyText(){
      const tAll = translations[lang];
      navigator.clipboard.writeText($("resultBox").textContent).then(()=>{
        alert(tAll.alerts.copied);
      });
    }

    // Init
    applyTranslations();
  </script>
</body>
</html>
