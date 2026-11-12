# Yurak — tanishuv demo

Mobil va kompyuterga mos statik prototip. Sayt GitHub Pages’da joylashishi mumkin.

## Demo ichida ishlaydigan qismlar

- Tanishuv profillarini ko‘rish, o‘tkazib yuborish va yoqtirish.
- Matchlar va demo suhbatlar.
- Profil tahrirlash.
- Zar tashlash va xotira mini-o‘yini.
- Brauzer `localStorage`’ida saqlash.

## Muhim: bu production xizmat emas

Bu versiya serverga ulanmagan. Profil, match va chat ma’lumotları **faqat shu brauzer va qurilmada** saqlanadi; boshqa foydalanuvchilar bilan onlayn almashilmaydi. Haqiqiy login, telefon/Google autentifikatsiyasi, backend chat, admin panel va global statistika yo‘q. Haqiqiy parol yoki shaxsiy ma’lumotlarni kiritmang.

Haqiqiy ko‘p foydalanuvchili xizmat uchun Firebase/Supabase kabi backend, server tomonda ruxsat tekshiruvi, xavfsizlik qoidalari va maxfiylik siyosati alohida sozlanishi kerak. Admin paroli yoki service-account kalitlarini HTML yoki repozitoriyaga qo‘shmang.

## GitHub Pages’ni yoqish

1. Repozitoriyada **Settings → Pages** ni oching.
2. **Build and deployment → Source** dan **GitHub Actions** ni tanlang.
3. **Actions** bo‘limida `Deploy static site to GitHub Pages` workflow muvaffaqiyatтай дууссаны дараа URL нь `https://baburshakh.github.io/Tanishuvlar/` байх ёстой. URL зөв эсэхийг Pages тохиргооноос баталгаажуулна уу.
4. Workflow автоматаар `main` branch-д push хийхэд ажиллана; мөн Actions tab-аас гараар эхлүүлж болно.

## Xavfsizlik eslatmasi

`localStorage` demo login yoki maxfiy ma’lumot saqlash uchun xavfsiz emas. Ushbu demo login/parol so‘ramaydi. Production’da parollarni faqat ishonchli autentifikatsiya provayderi orqali boshqaring.
# Yurak — tanishuv demo

Mobilga mos, GitHub Pages’da ishlaydigan tanishuv sayti prototipi. GitHub Actions workflow `main` branch-dagi o‘zgarishlarni avtomatik Pages deploy qiladi.

## Nima ishlaydi

- Tanishuv profillarini ko‘rish va yoqtirish.
- Matchlar va chat xabarlarining brauzerda saqlanishi (`localStorage`).
- O‘yinlar: zar tashlash va xotira mini-o‘yini.
- Profilni saqlash.

## Muhim cheklov

Bu frontend demo bo‘lib, haqiqiy foydalanuvchi autentifikatsiyasi, telefon/Google kirish, real-time chatlar, admin panel va online/offline statistikasi hali mavjud emas. Ma’lumotlar faqat bitta qurilmada saqlanadi.

Haqiqiy production versiyasi uchun Firebase/Auth/Firestore yoki Supabase + admin panel + security rules kerak bo‘ladi.

## GitHub Pages

`main` branchga push qilingandan so‘ng `.github/workflows/pages.yml` avtomatik ishlaydi. Repozitoriyning **Settings → Pages** bo‘limidan **GitHub Actions** tanlagan bo‘lishi lozim.

Agar deployment muvaffaqiyatli bo‘lsa, sayt URL ko‘rinishi:

`https://baburshakh.github.io/Tanishuvlar/`
<!doctype html>
<html lang="uz">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Yurak — tanishuv va o‘yinlar</title>
  <style>
    :root {
      --pink: #eb4d7b;
      --pink-2: #ffedf3;
      --ink: #1d1a22;
      --muted: #726a78;
      --line: #f0e8ee;
      --bg: #fff9fb;
      --green: #1e9b6d;
      --gold: #ffb545;
      --shadow: 0 18px 55px rgba(37, 16, 28, 0.12);
    }
    * { box-sizing: border-box; }
    html, body { margin: 0; padding: 0; font-family: Inter, "Segoe UI", sans-serif; background: linear-gradient(180deg,#fff5f8,#f8f4f7); color: var(--ink); }
    body { min-height: 100vh; }
    button, input, textarea { font: inherit; }
    button { cursor: pointer; }
    a { color: inherit; text-decoration: none; }
    .hidden { display: none !important; }
    .app-shell {
      max-width: 1280px; margin: 0 auto; min-height: 100vh; display: flex; background: rgba(255,255,255,0.7); backdrop-filter: blur(10px);
    }
    .sidebar {
      width: 280px; background: #fff; border-right: 1px solid var(--line); padding: 20px 18px; display: flex; flex-direction: column; gap: 18px;
    }
    .brand { display: flex; align-items: center; gap: 10px; font-size: 1.8rem; font-weight: 900; color: var(--pink); }
    .brand .dot { width: 12px; height: 12px; border-radius: 50%; background: var(--pink); box-shadow: 0 0 22px rgba(235,77,123,.5); }
    .nav { display: grid; gap: 8px; }
    .nav button {
      width: 100%; border: 0; background: transparent; text-align: left; border-radius: 12px; padding: 12px 12px; font-weight: 700; color: var(--muted); display: flex; align-items: center; gap: 10px; justify-content: flex-start;
    }
    .nav button.active, .nav button:hover { background: var(--pink-2); color: var(--pink); }
    .side-card {
      margin-top: auto; background: linear-gradient(135deg, #fff0f4, #fff9fb); border: 1px solid var(--line); border-radius: 18px; padding: 14px; color: var(--muted); font-size: 13px; line-height: 1.5;
    }
    .account-box { display: flex; align-items: center; gap: 12px; padding: 12px; border-radius: 14px; border: 1px solid var(--line); background: #fff; }
    .avatar {
      width: 42px; height: 42px; border-radius: 50%; object-fit: cover; background: linear-gradient(135deg,#ffe8f1,#ffdfe8); display: flex; align-items: center; justify-content: center; font-weight: 900; color: var(--pink);
    }
    .account-name { font-weight: 800; }
    .main {
      flex: 1; min-width: 0; padding: 24px; background: rgba(255,255,255,0.38);
    }
    .topbar {
      display: flex; align-items: center; justify-content: space-between; gap: 20px; padding: 8px 0 18px; border-bottom: 1px solid var(--line); margin-bottom: 18px;
    }
    .topbar h1 { margin: 0; font-size: clamp(1.6rem, 2vw, 2.2rem); }
    .status { display: inline-flex; align-items: center; gap: 8px; font-size: 13px; padding: 8px 12px; border-radius: 999px; background: #eafafa; color: var(--green); font-weight: 700; }
    .status .dot { width: 9px; height: 9px; border-radius: 50%; background: var(--green); display: inline-block; }
    .panel { display: none; }
    .panel.active { display: block; }
    .hero { background: linear-gradient(135deg,#fff2f6,#fffafc); border: 1px solid var(--line); border-radius: 20px; padding: 20px 22px; margin-bottom: 18px; }
    .hero h2 { margin: 0 0 6px; font-size: clamp(1.3rem, 1.5vw, 1.8rem); }
    .hero p { margin: 0; color: var(--muted); }
    .card-grid { display: grid; grid-template-columns: repeat(auto-fit,minmax(240px,1fr)); gap: 18px; }
    .profile-card { background: #fff; border: 1px solid var(--line); border-radius: 22px; overflow: hidden; box-shadow: var(--shadow); }
    .profile-card img { width: 100%; height: 340px; object-fit: cover; background: #f9eef2; }
    .profile-card .body { padding: 16px; }
    .profile-card h3 { margin: 0 0 6px; font-size: 1.5rem; }
    .profile-card p { margin: 0; color: var(--muted); line-height: 1.5; }
    .actions { display: flex; gap: 12px; margin-top: 16px; }
    .btn {
      border: 0; border-radius: 12px; padding: 11px 16px; font-weight: 800; transition: transform .15s ease; background: var(--pink); color: white;
    }
    .btn.secondary { background: var(--pink-2); color: var(--pink); }
    .btn.ghost { background: #fff; border: 1px solid var(--line); color: var(--ink); }
    .btn:hover { transform: translateY(-1px); }
    .list { display: grid; gap: 10px; }
    .match-item {
      display: flex; align-items: center; gap: 12px; background: white; padding: 12px 14px; border: 1px solid var(--line); border-radius: 14px;
    }
    .match-item img { width: 52px; height: 52px; border-radius: 50%; object-fit: cover; }
    .match-item .meta { flex: 1; }
    .match-item .meta h4 { margin: 0 0 2px; }
    .match-item .meta small { color: var(--muted); }
    .chat-box {
      display: grid; grid-template-columns: 290px 1fr; gap: 18px; min-height: 560px;
    }
    .chat-list { background: #fff; border: 1px solid var(--line); border-radius: 20px; padding: 12px; }
    .chat-item { display: flex; align-items: center; gap: 12px; padding: 10px 8px; border-radius: 12px; cursor: pointer; }
    .chat-item:hover, .chat-item.active { background: var(--pink-2); }
    .chat-item img { width: 42px; height: 42px; border-radius: 50%; object-fit: cover; }
    .chat-item .name { font-weight: 700; }
    .chat-item .tiny { color: var(--muted); font-size: 12px; }
    .chat-room {
      background: #fff; border: 1px solid var(--line); border-radius: 20px; display: flex; flex-direction: column; overflow: hidden;
    }
    .chat-head {
      display: flex; align-items: center; justify-content: space-between; padding: 12px 16px; border-bottom: 1px solid var(--line); font-weight: 800;
    }
    .messages {
      flex: 1; background: #fffdfd; padding: 16px; display: flex; flex-direction: column; gap: 10px; overflow: auto; min-height: 250px;
    }
    .bubble {
      max-width: 72%; padding: 10px 12px; border-radius: 14px; line-height: 1.5; background: #f2eef1; align-self: flex-start; color: var(--ink);
    }
    .bubble.mine { align-self: flex-end; background: var(--pink); color: white; }
    .composer { display: flex; gap: 8px; padding: 12px; border-top: 1px solid var(--line); }
    .composer input { flex: 1; border: 1px solid var(--line); border-radius: 12px; padding: 12px 14px; outline: none; }
    .grid-2 { display: grid; grid-template-columns: repeat(auto-fit,minmax(220px,1fr)); gap: 18px; }
    .game-card, .panel-card, .stats-box {
      background: white; border: 1px solid var(--line); border-radius: 18px; padding: 18px; box-shadow: 0 8px 18px rgba(32, 20, 30, 0.03);
    }
    .dice { font-size: 52px; line-height: 1; text-align: center; margin: 10px 0; }
    .memory-box { min-height: 80px; display: grid; place-items: center; font-size: 38px; }
    .form-grid { display: grid; gap: 12px; max-width: 560px; }
    .field { display: grid; gap: 6px; }
    .field label { font-size: 13px; font-weight: 700; color: var(--muted); }
    .field input, .field textarea {
      width: 100%; border: 1px solid var(--line); border-radius: 12px; padding: 11px 12px; background: #fff; outline: none;
    }
    .field textarea { min-height: 90px; resize: vertical; }
    .admin-table { width: 100%; border-collapse: collapse; margin-top: 12px; }
    .admin-table th, .admin-table td { text-align: left; padding: 10px 12px; border-bottom: 1px solid var(--line); font-size: 14px; }
    .badge { display: inline-block; padding: 5px 9px; border-radius: 999px; font-size: 11px; font-weight: 800; }
    .online { background: #eafaf2; color: var(--green); }
    .offline { background: #fff3ea; color: #d1762d; }
    .toast {
      position: fixed; right: 24px; bottom: 28px; background: #1d1a22; color: white; padding: 11px 16px; border-radius: 12px; box-shadow: var(--shadow); z-index: 999; font-weight: 700; opacity: 1; transition: opacity .2s ease;
    }
    .toast.hidden { opacity: 0; }
    .auth-screen { min-height: 100vh; display: grid; place-items: center; background: radial-gradient(circle at top,#fff2f6,#fff9fb 35%,#f3edf0); padding: 30px; }
    .auth-card {
      width: min(560px, 100%); background: rgba(255,255,255,0.9); border: 1px solid var(--line); border-radius: 26px; box-shadow: var(--shadow); padding: 26px;
    }
    .auth-top { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; }
    .auth-toggle { display: inline-flex; background: #f7eef2; padding: 6px; border-radius: 12px; }
    .auth-toggle button {
      border: 0; background: transparent; padding: 8px 16px; border-radius: 10px; font-weight: 800; color: var(--muted);
    }
    .auth-toggle button.active { background: white; color: var(--pink); box-shadow: 0 4px 14px rgba(235,77,123,.15); }
    .auth-form { display: grid; gap: 12px; margin-top: 18px; }
    .auth-form input, .auth-form button { width: 100%; }
    .text-center { text-align: center; }
    .tiny-muted { color: var(--muted); font-size: 12px; }
    .divider { display: flex; align-items: center; gap: 12px; color: var(--muted); font-size: 12px; margin: 18px 0 8px; }
    .divider:before, .divider:after { content: ""; flex: 1; height: 1px; background: var(--line); }
    @media (max-width: 980px) {
      .app-shell { display: block; }
      .sidebar { width: 100%; border-right: 0; border-bottom: 1px solid var(--line); }
      .chat-box { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>
  <div id="authScreen" class="auth-screen">
    <div class="auth-card">
      <div class="auth-top">
        <div class="brand"><span class="dot"></span> <span>Yurak</span></div>
        <div class="auth-toggle">
          <button id="loginTab" class="active">Kirish</button>
          <button id="registerTab">Ro‘yxatdan o‘tish</button>
        </div>
      </div>
      <h2 class="text-center" style="margin:0 0 6px;">Yulda bo‘lib qolganingizga hamma uchun o‘rindiq</h2>
      <p class="text-center tiny-muted">Tanishuv, chat, o‘yinlar va admin panel bir joyda.</p>

      <form id="authForm" class="auth-form">
        <div id="nameField" class="field hidden">
          <label for="nameInput">Ismingiz</label>
          <input id="nameInput" type="text" maxlength="60" placeholder="Ismingiz" />
        </div>
        <div class="field">
          <label for="emailInput">Elektron pochta</label>
          <input id="emailInput" type="email" placeholder="name@example.com" />
        </div>
        <div class="field">
          <label for="passwordInput">Parol</label>
          <input id="passwordInput" type="password" placeholder="Kamida 8 ta belgi" />
        </div>
        <button id="authSubmit" class="btn" type="submit">Kirish</button>
      </form>

      <div class="divider">yoki</div>
      <button id="googleBtn" class="btn secondary" type="button">Google bilan davom etish</button>

      <div class="divider">telefon orqali</div>
      <div class="auth-form">
        <input id="phoneInput" type="tel" placeholder="+998901234567" />
        <input id="phoneCodeInput" type="text" placeholder="SMS kodini kiriting" class="hidden" />
        <button id="phoneBtn" class="btn ghost" type="button">SMS kodini yuborish</button>
      </div>

      <p class="tiny-muted" style="margin-top:18px; line-height:1.5;">Davom etish orqali foydalanish qoidalariga rozilik bildirasiz. Bu demo versiyada ma’lumotlar brauzerda saqlanadi.</p>
      <div id="authError" class="tiny-muted" style="margin-top:12px; color:#c24d4d; min-height:18px;"></div>
    </div>
  </div>

  <div id="app" class="app-shell hidden">
    <aside class="sidebar">
      <div class="brand"><span class="dot"></span> <span>Yurak</span></div>
      <div class="account-box">
        <div id="sideAvatar" class="avatar">U</div>
        <div>
          <div id="sideName" class="account-name">Foydalanuvchi</div>
          <div class="tiny-muted" id="sideEmail">demo@example.com</div>
        </div>
      </div>
      <nav class="nav">
        <button class="active" data-tab="discover">🔥 Tanishuv</button>
        <button data-tab="chats">💬 Chatlar</button>
        <button data-tab="games">🎮 O‘yinlar</button>
        <button data-tab="profile">👤 Profil</button>
        <button data-tab="admin">🛡️ Admin</button>
      </nav>
      <div class="side-card">
        Hurmat, ishonch va xavfsizlik — bu platformaning asosiy tamoyillari. Nomaqbul xabarlarni yetkazib berish mumkin emas.
      </div>
      <button id="logoutBtn" class="btn secondary" type="button">Chiqish</button>
    </aside>

    <main class="main">
      <header class="topbar">
        <h1 id="pageTitle">Sizga mos insonlar</h1>
        <div class="status"><span class="dot"></span> Ulangan</div>
      </header>

      <section id="panel-discover" class="panel active">
        <div class="hero">
          <h2>Yangi tanishuvlar 💞</h2>
          <p>O‘zingizga mos insonlarni tanlang va suhbat boshlang.</p>
        </div>
        <div id="discoverList" class="card-grid"></div>
      </section>

      <section id="panel-chats" class="panel">
        <div class="hero">
          <h2>Suhbatlaringiz</h2>
          <p>Xabarlar saqlanadi. Qayta kirganingizda chat tarixi qoladi.</p>
        </div>

        <div class="chat-box">
          <div class="chat-list" id="chatList"></div>

          <div class="chat-room">
            <div class="chat-head">
              <span id="chatHead">Suhbatni tanlang</span>
              <span class="tiny-muted" id="chatStatus">Hech narsa yo‘q</span>
            </div>
            <div id="messages" class="messages"></div>
            <form id="messageForm" class="composer">
              <input id="messageInput" type="text" maxlength="1000" placeholder="Xabar yozing…" autocomplete="off" />
              <button class="btn" type="submit">Yuborish</button>
            </form>
          </div>
        </div>
      </section>

      <section id="panel-games" class="panel">
        <div class="hero">
          <h2>O‘yinlar maydoni ⚡</h2>
          <p>Qiziqarli mini o‘yinlar, tezkor rol oynash va random natijalar.</p>
        </div>

        <div class="grid-2">
          <div class="game-card">
            <h3>🎲 Omad jangi</h3>
            <div id="diceValue" class="dice">🎲</div>
            <button id="rollDice" class="btn" type="button">Zar tashlash</button>
            <p id="diceResult" class="tiny-muted" style="margin-top:12px; min-height:20px;">Bot bilan o‘ynash uchun bosing.</p>
          </div>

          <div class="game-card">
            <h3>🧠 Xotira sinovi</h3>
            <div id="memoryValue" class="memory-box">❔</div>
            <button id="startMemory" class="btn secondary" type="button">Boshlash</button>
            <p id="memoryResult" class="tiny-muted" style="margin-top:12px; min-height:20px;">Belgilarni yodda tuting.</p>
          </div>
        </div>
      </section>

      <section id="panel-profile" class="panel">
        <div class="hero">
          <h2>Profil sozlamalari</h2>
          <p>O‘zingiz haqingizda qisqacha ma’lumot yozing.</p>
        </div>

        <form id="profileForm" class="form-grid">
          <div class="field">
            <label for="profileName">Ismingiz</label>
            <input id="profileName" type="text" maxlength="60" />
          </div>
          <div class="field">
            <label for="profileAge">Yosh</label>
            <input id="profileAge" type="number" min="18" max="100" />
          </div>
          <div class="field">
            <label for="profileCity">Shahar</label>
            <input id="profileCity" type="text" maxlength="60" />
          </div>
          <div class="field">
            <label for="profilePhoto">Rasm URL</label>
            <input id="profilePhoto" type="url" placeholder="https://..." />
          </div>
          <div class="field">
            <label for="profileBio">Biografiya</label>
            <textarea id="profileBio" maxlength="500" placeholder="O‘zingiz haqingizda qisqacha..."></textarea>
          </div>
          <button class="btn" type="submit">Saqlash</button>
        </form>
      </section>

      <section id="panel-admin" class="panel">
        <div class="hero">
          <h2>Admin boshqaruv paneli</h2>
          <p>Ro‘yxatdan o‘tgan foydalanuvchilar va online/offline statistikasi.</p>
        </div>

        <div class="stats-box">
          <div class="grid-2">
            <div>
              <small class="tiny-muted">Jami foydalanuvchilar</small>
              <h3 id="statUsers">0</h3>
            </div>
            <div>
              <small class="tiny-muted">Online</small>
              <h3 id="statOnline">0</h3>
            </div>
          </div>
        </div>

        <div class="panel-card" style="margin-top:18px;">
          <table class="admin-table">
            <thead>
              <tr>
                <th>Ism</th>
                <th>Email</th>
                <th>Telefon</th>
                <th>Status</th>
                <th>Oxirgi aktivlik</th>
              </tr>
            </thead>
            <tbody id="adminTableBody"></tbody>
          </table>
        </div>
      </section>
    </main>
  </div>

  <div id="toast" class="toast hidden">Xabar</div>

  <script>
    const demoProfiles = [
      { id: "p1", name: "Malika", age: 22, city: "Toshkent", bio: "Kitob, qahva va sayr qilishni yaxshi ko‘raman.", photo: "https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=800&q=80" },
      { id: "p2", name: "Madina", age: 24, city: "Samarqand", bio: "Dizayn, sayohat va shashka yaxshi ko‘rishadi.", photo: "https://images.unsplash.com/photo-1517841905240-472988babdf9?auto=format&fit=crop&w=800&q=80" },
      { id: "p3", name: "Sardor", age: 25, city: "Toshkent", bio: "Sport, yangi joylarni kashf qilish va yaxshi suhbat ham yoqadi.", photo: "https://images.unsplash.com/photo-1500648767791-00dcc994a43e?auto=format&fit=crop&w=800&q=80" },
      { id: "p4", name: "Nilufar", age: 21, city: "Buxoro", bio: "Rasm chizish, musiqa va bayramlarda uchrashishni yaxshi ko‘raman.", photo: "https://images.unsplash.com/photo-1487412720507-e7ab37603c6f?auto=format&fit=crop&w=800&q=80" }
    ];

    const storageKeys = {
      users: "yurak_users",
      currentUser: "yurak_current_user",
      matches: "yurak_matches",
      chats: "yurak_chats",
      profile: "yurak_profile",
      seen: "yurak_seen"
    };

    let currentUser = null;
    let currentChatId = null;
    let currentMode = "login";

    const $ = (id) => document.getElementById(id);
    const safe = (v) => String(v ?? "").replace(/[&<>"']/g, c => ({"&":"&amp;","<":"&lt;", ">":"&gt;","\"":"&quot;","'":"&#39;"}[c]));

    function read(key, fallback) {
      try {
        const raw = localStorage.getItem(key);
        return raw ? JSON.parse(raw) : fallback;
      } catch {
        return fallback;
      }
    }

    function write(key, value) {
      localStorage.setItem(key, JSON.stringify(value));
    }

    function toast(msg) {
      const t = $("toast");
      t.textContent = msg;
      t.classList.remove("hidden");
      setTimeout(() => t.classList.add("hidden"), 2500);
    }

    function showAuthError(msg) {
      $("authError").textContent = msg;
    }

    function setAuthMode(mode) {
      currentMode = mode;
      const loginActive = mode === "login";
      $("loginTab").classList.toggle("active", loginActive);
      $("registerTab").classList.toggle("active", !loginActive);
      $("nameField").classList.toggle("hidden", !loginActive);
      $("authSubmit").textContent = loginActive ? "Kirish" : "Ro‘yxatdan o‘tish";
      $("authForm").dataset.mode = mode;
      showAuthError("");
    }

    function getUsers() {
      return read(storageKeys.users, []);
    }

    function saveUsers(users) {
      write(storageKeys.users, users);
    }

    function getUserByEmail(email) {
      return getUsers().find(u => u.email.toLowerCase() === email.toLowerCase());
    }

    function createDemoUser(payload) {
      const users = getUsers();
      const exists = users.some(u => u.email.toLowerCase() === payload.email.toLowerCase());
      if (exists) return { ok: false, error: "Bu email allaqachon ro‘yxatdan o‘tgan." };
      const user = {
        id: crypto.randomUUID ? crypto.randomUUID() : `u_${Date.now()}`,
        name: payload.name || "Yangi foydalanuvchi",
        email: payload.email,
        password: payload.password,
        phone: payload.phone || "",
        photo: payload.photo || "",
        bio: payload.bio || "",
        city: payload.city || "",
        age: Number(payload.age) || 18,
        role: "user",
        lastSeen: Date.now(),
        online: true
      };
      users.push(user);
      saveUsers(users);
      return { ok: true, user };
    }

    function loginUser(email, password) {
      const user = getUserByEmail(email);
      if (!user) return { ok: false, error: "Email yoki parol noto‘g‘ri." };
      if (user.password !== password) return { ok: false, error: "Email yoki parol noto‘g‘ri." };
      currentUser = user;
      user.lastSeen = Date.now();
      user.online = true;
      saveUsers(getUsers());
      write(storageKeys.currentUser, user.id);
      return { ok: true, user };
    }

    function logoutUser() {
      const users = getUsers();
      const target = users.find(u => u.id === currentUser?.id);
      if (target) {
        target.online = false;
        target.lastSeen = Date.now();
        saveUsers(users);
      }
      currentUser = null;
      localStorage.removeItem(storageKeys.currentUser);
      showAuthScreen();
      toast("Tizimdan chiqdingiz.");
    }

    function showAuthScreen() {
      $("authScreen").classList.remove("hidden");
      $("app").classList.add("hidden");
    }

    function showAppScreen() {
      $("authScreen").classList.add("hidden");
      $("app").classList.remove("hidden");
      renderSideUser();
      renderMatches();
      renderDiscover();
      renderProfileForm();
      renderAdmin();
      switchTab("discover");
    }

    function renderSideUser() {
      if (!currentUser) return;
      $("sideName").textContent = currentUser.name;
      $("sideEmail").textContent = currentUser.email;
      const avatar = currentUser.photo || "";
      $("sideAvatar").textContent = currentUser.name.charAt(0).toUpperCase();
      if (avatar) {
        $("sideAvatar").style.backgroundImage = `url(${avatar})`;
        $("sideAvatar").style.backgroundSize = "cover";
        $("sideAvatar").style.backgroundPosition = "center";
        $("sideAvatar").textContent = "";
      } else {
        $("sideAvatar").style.background = "linear-gradient(135deg,#ffe8f1,#ffdfe8)";
        $("sideAvatar").textContent = currentUser.name.charAt(0).toUpperCase();
      }
    }

    function renderDiscover() {
      const list = $("discoverList");
      const seen = read(storageKeys.seen, []);
      const candidate = demoProfiles.filter(p => !seen.includes(p.id) || seen.length === 0);
      const profile = candidate[0] || demoProfiles[0];
      if (!profile) {
        list.innerHTML = '<div class="panel-card"><h3>Barcha profilga qarash tugadi.</h3></div>';
        return;
      }
      list.innerHTML = `
        <article class="profile-card">
          <img src="${profile.photo}" alt="${safe(profile.name)}" />
          <div class="body">
            <h3>${safe(profile.name)}, ${profile.age}</h3>
            <p>📍 ${safe(profile.city)}</p>
            <p style="margin-top:10px;">${safe(profile.bio)}</p>
            <div class="actions">
              <button class="btn ghost" type="button" data-action="skip" data-profile="${profile.id}">O‘tkazib yuborish</button>
              <button class="btn" type="button" data-action="like" data-profile="${profile.id}">💚 Yoqdi</button>
            </div>
          </div>
        </article>
      `;

      list.querySelectorAll("button[data-action]").forEach(btn => {
        btn.addEventListener("click", () => {
          const pid = btn.dataset.profile;
          const seenList = read(storageKeys.seen, []);
          if (!seenList.includes(pid)) seenList.push(pid);
          write(storageKeys.seen, seenList);
          if (btn.dataset.action === "like") {
            const matches = read(storageKeys.matches, []);
            const profileData = demoProfiles.find(p => p.id === pid);
            if (profileData && !matches.some(m => m.id === profileData.id)) {
              matches.push(profileData);
              write(storageKeys.matches, matches);
              toast("Yangi match! 💞");
            }
          }
          renderDiscover();
          renderMatches();
        });
      });
    }

    function renderMatches() {
      const matches = read(storageKeys.matches, []);
      const list = $("chatList");
      if (!matches.length) {
        list.innerHTML = '<div class="tiny-muted" style="padding:16px;">Hozircha matchlar yo‘q.</div>';
        return;
      }
      list.innerHTML = matches.map(match => `
        <div class="chat-item ${currentChatId === match.id ? 'active' : ''}" data-chat-id="${match.id}">
          <img src="${match.photo}" alt="${safe(match.name)}" />
          <div>
            <div class="name">${safe(match.name)}</div>
            <div class="tiny">Suhbatni boshlash</div>
          </div>
        </div>
      `).join("");

      list.querySelectorAll(".chat-item").forEach(item => {
        item.addEventListener("click", () => {
          currentChatId = item.dataset.chatId;
          const match = matches.find(m => m.id === currentChatId);
          $("chatHead").textContent = match ? match.name : "Suhbat";
          $("chatStatus").textContent = match ? "online" : "Hech narsa yo‘q";
          renderMessages();
          renderMatches();
        });
      });

      if (!currentChatId && matches[0]) currentChatId = matches[0].id;
      const current = matches.find(m => m.id === currentChatId);
      if (current) {
        $("chatHead").textContent = current.name;
        $("chatStatus").textContent = "online";
      }
      renderMessages();
    }

    function renderMessages() {
      const matches = read(storageKeys.matches, []);
      const chats = read(storageKeys.chats, {});
      const panel = $("messages");
      const current = matches.find(m => m.id === currentChatId);
      if (!current) {
        panel.innerHTML = '<div class="tiny-muted" style="padding:18px;">Matchlarni toping va chat boshlang.</div>';
        return;
      }
      const list = chats[current.id] || [
        { sender: "them", text: "Salom! Tanishganimizdan xursandman 😊" },
        { sender: "me", text: "Salom! Qiziqarli suhbat bo‘lsin." }
      ];
      panel.innerHTML = list.map(msg => `
        <div class="bubble ${msg.sender === "me" ? "mine" : ""}">${safe(msg.text)}</div>
      `).join("");
      panel.scrollTop = panel.scrollHeight;
    }

    $("messageForm").addEventListener("submit", (e) => {
      e.preventDefault();
      const text = $("messageInput").value.trim();
      if (!text || !currentChatId) return;
      const chats = read(storageKeys.chats, {});
      if (!chats[currentChatId]) chats[currentChatId] = [];
      chats[currentChatId].push({ sender: "me", text });
      write(storageKeys.chats, chats);
      $("messageInput").value = "";
      setTimeout(() => {
        chats[currentChatId].push({ sender: "them", text: "Ajoyib! O‘zingizga ko‘p maqtov beraman 😊" });
        write(storageKeys.chats, chats);
        renderMessages();
      }, 700);
      renderMessages();
    });

    function renderProfileForm() {
      if (!currentUser) return;
      $("profileName").value = currentUser.name || "";
      $("profileAge").value = currentUser.age || 18;
      $("profileCity").value = currentUser.city || "";
      $("profilePhoto").value = currentUser.photo || "";
      $("profileBio").value = currentUser.bio || "";
    }

    $("profileForm").addEventListener("submit", (e) => {
      e.preventDefault();
      if (!currentUser) return;
      const users = getUsers();
      const idx = users.findIndex(u => u.id === currentUser.id);
      if (idx === -1) return;
      users[idx].name = $("profileName").value.trim() || currentUser.name;
      users[idx].age = Number($("profileAge").value || 18);
      users[idx].city = $("profileCity").value.trim();
      users[idx].photo = $("profilePhoto").value.trim();
      users[idx].bio = $("profileBio").value.trim();
      users[idx].lastSeen = Date.now();
      saveUsers(users);
      currentUser = users[idx];
      write(storageKeys.currentUser, currentUser.id);
      renderSideUser();
      toast("Profil saqlandi.");
    });

    function renderAdmin() {
      const users = getUsers();
      const onlineCount = users.filter(u => u.online).length;
      $("statUsers").textContent = users.length;
      $("statOnline").textContent = onlineCount;
      $("adminTableBody").innerHTML = users.map(user => `
        <tr>
          <td>${safe(user.name)}</td>
          <td>${safe(user.email)}</td>
          <td>${safe(user.phone || "—")}</td>
          <td><span class="badge ${user.online ? "online" : "offline"}">${user.online ? "Online" : "Offline"}</span></td>
          <td>${new Date(user.lastSeen || Date.now()).toLocaleString("uz-UZ")}</td>
        </tr>
      `).join("");
    }

    function switchTab(tab) {
      document.querySelectorAll(".nav button").forEach(btn => btn.classList.toggle("active", btn.dataset.tab === tab));
      document.querySelectorAll(".panel").forEach(panel => panel.classList.toggle("active", panel.id === `panel-${tab}`));
      $("pageTitle").textContent = {
        discover: "Sizga mos insonlar",
        chats: "Suhbatlaringiz",
        games: "O‘yinlar maydoni",
        profile: "Profil sozlamalari",
        admin: "Admin paneli"
      }[tab] || "Yurak";
      if (tab === "admin") renderAdmin();
    }

    document.querySelectorAll(".nav button").forEach(btn => {
      btn.addEventListener("click", () => switchTab(btn.dataset.tab));
    });

    $("loginTab").addEventListener("click", () => setAuthMode("login"));
    $("registerTab").addEventListener("click", () => setAuthMode("register"));

    $("authForm").addEventListener("submit", (e) => {
      e.preventDefault();
      const email = $("emailInput").value.trim();
      const password = $("passwordInput").value.trim();
      const name = $("nameInput").value.trim();
      if (!email || !password) {
        showAuthError("Email va parolni kiriting.");
        return;
      }
      if (currentMode === "register") {
        if (password.length < 8) {
          showAuthError("Parol kamida 8 ta belgidan iborat bo‘lishi kerak.");
          return;
        }
        const result = createDemoUser({ name: name || "Yangi foydalanuvchi", email, password, phone: "", age: 18, city: "", bio: "", photo: "" });
        if (!result.ok) {
          showAuthError(result.error);
          return;
        }
        currentUser = result.user;
        write(storageKeys.currentUser, currentUser.id);
        currentUser.online = true;
        saveUsers(getUsers());
        showAppScreen();
        toast("Ro‘yxatdan o‘tdingiz.");
        return;
      }

      const result = loginUser(email, password);
      if (!result.ok) {
        showAuthError(result.error);
        return;
      }
      currentUser = result.user;
      showAppScreen();
      toast("Xush kelibsiz!");
    });

    $("googleBtn").addEventListener("click", () => {
      const email = `${Math.random().toString(36).slice(2)}@gmail.com`;
      const fake = createDemoUser({
        name: "Google foydalanuvchi",
        email,
        password: "qwerty123",
        phone: "",
        age: 25,
        city: "Toshkent",
        bio: "Google orqali kirgan foydalanuvchi.",
        photo: "https://images.unsplash.com/photo-1544005313-94ddf0286df2?auto=format&fit=crop&w=800&q=80"
      });
      if (fake.ok) {
        currentUser = fake.user;
        write(storageKeys.currentUser, currentUser.id);
        currentUser.online = true;
        saveUsers(getUsers());
        showAppScreen();
        toast("Google orqali kirish muvaffaqiyatli.");
      }
    });

    $("phoneBtn").addEventListener("click", () => {
      const phone = $("phoneInput").value.trim();
      const codeInput = $("phoneCodeInput");
      if (!phone) {
        showAuthError("Telefon raqami kiriting.");
        return;
      }
      if (codeInput.classList.contains("hidden")) {
        codeInput.classList.remove("hidden");
        codeInput.value = "123456";
        showAuthError("Demo SMS kodi: 123456");
        $("phoneBtn").textContent = "Kodni tasdiqlash";
        return;
      }
      if (codeInput.value.trim() !== "123456") {
        showAuthError("Noto‘g‘ri SMS kodi.");
        return;
      }
      const email = `phone_${Date.now()}@demo.local`;
      const fake = createDemoUser({
        name: "Telefon foydalanuvchi",
        email,
        password: "qwerty123",
        phone,
        age: 27,
        city: "Andijon",
        bio: "Telefon orqali ro‘yxatdan o‘tgan foydalanuvchi.",
        photo: "https://images.unsplash.com/photo-1506794778202-cad84cf45f1d?auto=format&fit=crop&w=800&q=80"
      });
      if (fake.ok) {
        currentUser = fake.user;
        write(storageKeys.currentUser, currentUser.id);
        currentUser.online = true;
        saveUsers(getUsers());
        showAppScreen();
        toast("Telefon orqali kirish tayyor.");
      }
    });

    $("logoutBtn").addEventListener("click", logoutUser);

    $("rollDice").addEventListener("click", () => {
      const a = Math.floor(Math.random() * 6) + 1;
      const b = Math.floor(Math.random() * 6) + 1;
      $("diceValue").textContent = `${["⚀","⚁","⚂","⚃","⚄","⚅"][a-1]} — ${["⚀","⚁","⚂","⚃","⚄","⚅"][b-1]}`;
      $("diceResult").textContent = a > b ? "Siz yutdingiz! 🎉" : a < b ? "Bot yutdi — yana urinib ko‘ring." : "Durang!";
    });

    let memorySequence = [];
    $("startMemory").addEventListener("click", () => {
      const icons = ["🍓","🌙","⭐","🎈","🍀","🐱","🍉","🎵","🎯","🌸"];
      memorySequence = Array.from({ length: 4 }, () => icons[Math.floor(Math.random() * icons.length)]);
      $("memoryValue").textContent = memorySequence.join(" ");
      $("memoryResult").textContent = "Eslab qoling…";
      setTimeout(() => {
        $("memoryValue").textContent = "❔ ❔ ❔ ❔";
        const answer = prompt("Ketma-ketlikni yozing (bo‘sh joy bilan):");
        const given = (answer || "").trim();
        $("memoryResult").textContent = given === memorySequence.join(" ") ? "To‘g‘ri! 🧠" : `Noto‘g‘ri. To‘g‘ri ketma-ketlik: ${memorySequence.join(" ")}`;
      }, 1400);
    });

    function init() {
      setAuthMode("login");
      const savedId = localStorage.getItem(storageKeys.currentUser);
      if (savedId) {
        const users = getUsers();
        const savedUser = users.find(u => u.id === savedId);
        if (savedUser) {
          currentUser = savedUser;
          currentUser.online = true;
          saveUsers(users);
          showAppScreen();
          return;
        }
      }
      showAuthScreen();
    }

    init();
  </script>
</body>
</html>
