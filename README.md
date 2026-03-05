<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>마이페이지 · 음식 룰렛 (Web Prototype)</title>
  <style>
    :root{
      --mint:#2AC1BC;
      --sky:#6ED3E8;
      --sky2:#AEEAF4;
      --bg:#F6F7F9;
      --text:#111827;
      --muted:#6B7280;
      --card:#ffffff;
      --line:#E5E7EB;
      --shadow: 0 10px 24px rgba(0,0,0,.08);
      --shadow2: 0 6px 18px rgba(0,0,0,.06);
      --radius: 18px;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      font-family: system-ui,-apple-system,"Segoe UI",Roboto,"Noto Sans KR",sans-serif;
      background:var(--bg);
      color:var(--text);
    }
    .app{
      max-width: 420px;
      margin: 0 auto;
      min-height: 100vh;
      padding-bottom: 82px; /* tabbar space */
    }

    /* Top */
    .top{
      padding: 16px 16px 12px;
      background: linear-gradient(180deg, rgba(42,193,188,.12), rgba(42,193,188,0));
      position: sticky;
      top:0;
      backdrop-filter: blur(10px);
      z-index:10;
    }
    .top-row{
      display:flex; align-items:center; justify-content:space-between;
      gap:12px;
    }
    .title{
      font-weight: 800;
      letter-spacing: -0.02em;
      font-size: 18px;
    }
    .pill{
      display:flex; gap:8px; align-items:center;
      padding: 10px 12px;
      border-radius: 999px;
      background:#fff;
      border:1px solid var(--line);
      box-shadow: var(--shadow2);
      flex:1;
    }
    .pill input{
      border:none; outline:none; width:100%;
      font-size: 14px;
      background: transparent;
    }
    .icon-btn{
      width: 42px; height: 42px;
      border-radius: 12px;
      border:1px solid var(--line);
      background:#fff;
      box-shadow: var(--shadow2);
      cursor:pointer;
      display:grid; place-items:center;
      user-select:none;
    }

    /* Profile card */
    .profile{
      margin: 10px 16px 14px;
      background: var(--card);
      border: 1px solid var(--line);
      border-radius: var(--radius);
      box-shadow: var(--shadow2);
      overflow:hidden;
      position: relative;
    }
    .profile-head{
      padding: 14px 14px 0;
      display:flex; align-items:center; justify-content:space-between;
    }
    .user{
      display:flex; gap:10px; align-items:center;
    }
    .avatar{
      width:44px; height:44px; border-radius: 14px;
      background: linear-gradient(135deg, rgba(42,193,188,.25), rgba(110,211,232,.25));
      border:1px solid rgba(42,193,188,.25);
      display:grid; place-items:center;
      font-weight: 900;
      color:#0f766e;
    }
    .user .meta{
      display:flex; flex-direction:column; gap:2px;
    }
    .user .name{ font-weight: 800; letter-spacing:-0.02em;}
    .user .sub{ font-size:12px; color: var(--muted);}

    /* Roulette block */
    .roulette-wrap{
      padding: 10px 14px 14px;
      display:grid;
      grid-template-columns: 1fr;
      gap: 10px;
    }
    .roulette-card{
      background: linear-gradient(180deg, rgba(110,211,232,.14), rgba(110,211,232,0));
      border:1px solid rgba(110,211,232,.25);
      border-radius: 16px;
      padding: 14px;
      position: relative;
    }
    .roulette-top{
      display:flex; align-items:flex-end; justify-content:space-between;
      gap:10px;
      margin-bottom: 12px;
    }
    .roulette-top .h1{
      font-weight: 900;
      letter-spacing:-0.03em;
      font-size: 16px;
      line-height:1.2;
    }
    .roulette-top .desc{
      font-size: 12px;
      color: var(--muted);
      margin-top:4px;
    }
    .spin-btn{
      border:none;
      cursor:pointer;
      background: var(--mint);
      color:#fff;
      font-weight: 800;
      padding: 10px 12px;
      border-radius: 12px;
      box-shadow: 0 10px 16px rgba(42,193,188,.22);
      white-space:nowrap;
    }
    .spin-btn:active{ transform: translateY(1px); }

    .roulette-stage{
      display:grid;
      place-items:center;
      position: relative;
      padding: 6px 0 2px;
    }

    /* Pointer */
    .pointer{
      position:absolute;
      top: 2px;
      width: 0; height: 0;
      border-left: 10px solid transparent;
      border-right: 10px solid transparent;
      border-top: 16px solid #F4C542; /* yellow */
      filter: drop-shadow(0 6px 6px rgba(0,0,0,.12));
      z-index: 5;
    }

    /* Canvas */
    canvas{
      width: 290px;
      height: 290px;
      max-width: 100%;
      display:block;
      filter: drop-shadow(0 16px 22px rgba(0,0,0,.10));
    }

    /* Center badge */
    .center-badge{
      position:absolute;
      width: 78px; height: 78px;
      border-radius: 999px;
      background: #fff;
      border: 1px solid rgba(0,0,0,.06);
      box-shadow: var(--shadow2);
      display:grid; place-items:center;
      font-weight: 900;
      color:#111827;
      z-index: 4;
    }
    .center-badge span{
      display:block;
      font-size:12px;
      color: var(--muted);
      font-weight: 800;
      margin-top:2px;
    }

    /* Pop food */
    .pop{
      position:absolute;
      bottom: 120px;
      transform: translateY(18px) scale(.92);
      opacity: 0;
      pointer-events:none;
      z-index: 6;
      transition: transform .35s ease, opacity .25s ease;
      text-align:center;
    }
    .pop.on{
      opacity:1;
      transform: translateY(0) scale(1);
    }
    .pop .emoji{
      font-size: 44px;
      filter: drop-shadow(0 10px 14px rgba(0,0,0,.12));
    }
    .pop .label{
      margin-top: 6px;
      display:inline-block;
      background:#fff;
      border:1px solid var(--line);
      border-radius: 999px;
      padding: 6px 10px;
      font-weight: 900;
      font-size: 13px;
      box-shadow: var(--shadow2);
    }

    /* Quick cards */
    .grid{
      display:grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;
      padding: 0 16px 18px;
    }
    .mini{
      background:#fff;
      border:1px solid var(--line);
      border-radius: 16px;
      padding: 12px 10px;
      box-shadow: var(--shadow2);
    }
    .mini .t{font-weight:900; font-size:13px; letter-spacing:-0.02em;}
    .mini .s{color:var(--muted); font-size:12px; margin-top:4px;}

    /* Promo */
    .promo{
      margin: 0 16px 16px;
      background: #fff;
      border:1px solid var(--line);
      border-radius: var(--radius);
      box-shadow: var(--shadow2);
      overflow:hidden;
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:10px;
      padding: 12px 14px;
    }
    .promo .left .t{font-weight:900; letter-spacing:-0.02em;}
    .promo .left .s{font-size:12px; color:var(--muted); margin-top:4px;}
    .promo .tag{
      background: rgba(42,193,188,.14);
      color:#0f766e;
      font-weight: 900;
      padding: 8px 10px;
      border-radius: 999px;
      border: 1px solid rgba(42,193,188,.24);
      white-space: nowrap;
    }

    /* Tab bar */
    .tabbar{
      position: fixed;
      left: 50%;
      transform: translateX(-50%);
      bottom: 10px;
      width: min(420px, calc(100% - 18px));
      background:#fff;
      border:1px solid var(--line);
      border-radius: 18px;
      box-shadow: var(--shadow);
      padding: 10px 10px;
      display:grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 6px;
      z-index: 99;
    }
    .tab{
      display:grid;
      place-items:center;
      gap:4px;
      color: var(--muted);
      font-size: 11px;
      font-weight: 800;
      user-select:none;
    }
    .tab .dot{
      width: 28px; height: 28px;
      border-radius: 10px;
      background: rgba(17,24,39,.05);
      display:grid; place-items:center;
      font-size: 14px;
    }
    .tab.active{ color: var(--mint); }
    .tab.active .dot{
      background: rgba(42,193,188,.14);
      border: 1px solid rgba(42,193,188,.22);
    }

    /* Small note */
    .note{
      padding: 0 16px 20px;
      color: var(--muted);
      font-size: 12px;
      line-height: 1.5;
    }
  </style>
</head>
<body>
  <div class="app">
    <div class="top">
      <div class="top-row">
        <div class="title">마이페이지</div>
        <div class="icon-btn" title="알림">🔔</div>
      </div>
      <div style="height:10px"></div>
      <div class="pill" aria-label="검색">
        <span style="font-size:14px">🔎</span>
        <input placeholder="가게/메뉴 검색" />
      </div>
    </div>

    <section class="profile">
      <div class="profile-head">
        <div class="user">
          <div class="avatar" aria-hidden="true">J</div>
          <div class="meta">
            <div class="name">지현님</div>
            <div class="sub">포인트 · 쿠폰 · 내역을 한눈에</div>
          </div>
        </div>
        <button class="icon-btn" title="설정">⚙️</button>
      </div>

      <div class="roulette-wrap">
        <div class="roulette-card">
          <div class="roulette-top">
            <div>
              <div class="h1">뭘 먹을까? 🎲</div>
              <div class="desc">룰렛을 돌리면 메뉴가 뿅! 떠올라요</div>
            </div>
            <button class="spin-btn" id="spinBtn">룰렛 돌리기</button>
          </div>

          <div class="roulette-stage">
            <div class="pointer" aria-hidden="true"></div>
            <canvas id="wheel" width="580" height="580" aria-label="음식 룰렛"></canvas>

            <div class="center-badge" aria-hidden="true">
              룰렛
              <span>tap</span>
            </div>

            <div class="pop" id="pop">
              <div class="emoji" id="popEmoji">🍗</div>
              <div class="label" id="popLabel">치킨</div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <div class="grid">
      <div class="mini">
        <div class="t">포인트</div>
        <div class="s">0P</div>
      </div>
      <div class="mini">
        <div class="t">쿠폰</div>
        <div class="s">2장</div>
      </div>
      <div class="mini">
        <div class="t">선물함</div>
        <div class="s">0개</div>
      </div>
    </div>

    <div class="promo">
      <div class="left">
        <div class="t">오늘만 할인</div>
        <div class="s">추천 메뉴로 빠르게 골라봐요</div>
      </div>
      <div class="tag">최대 3천원</div>
    </div>

    <div class="note">
      * 과제/포트폴리오용 프로토타입입니다. 실제 서비스 UI와 동일하지 않으며, 룰렛은 메뉴 선택 피로를 줄이는 인터랙션 컨셉을 검증하기 위한 예시입니다.
    </div>
  </div>

  <nav class="tabbar" aria-label="하단 탭바">
    <div class="tab"><div class="dot">🏠</div>홈</div>
    <div class="tab"><div class="dot">🧺</div>장보기</div>
    <div class="tab"><div class="dot">🧾</div>주문내역</div>
    <div class="tab active"><div class="dot">🙂</div>마이</div>
    <div class="tab"><div class="dot">💬</div>문의</div>
  </nav>

  <script>
    // ====== Roulette data (6 menus) ======
    const menus = [
      { label: "치킨", emoji: "🍗" },
      { label: "피자", emoji: "🍕" },
      { label: "떡볶이", emoji: "🍢" },
      { label: "햄버거", emoji: "🍔" },
      { label: "초밥", emoji: "🍣" },
      { label: "국밥", emoji: "🍲" },
    ];

    // ====== Draw wheel ======
    const canvas = document.getElementById("wheel");
    const ctx = canvas.getContext("2d");

    const DPR = window.devicePixelRatio || 1;
    function fitCanvas(){
      // Keep internal size fixed for simplicity (already 580x580)
      drawWheel(currentRotation);
    }

    const cx = canvas.width / 2;
    const cy = canvas.height / 2;
    const radius = 250;

    // Cute sky palette (alternating shades)
    const sliceColors = ["#6ED3E8", "#AEEAF4"];

    function drawWheel(rotationRad){
      ctx.clearRect(0,0,canvas.width,canvas.height);

      // outer shadow ring
      ctx.save();
      ctx.translate(cx, cy);
      ctx.rotate(rotationRad);

      const slice = (Math.PI * 2) / menus.length;

      for(let i=0; i<menus.length; i++){
        const start = i * slice;
        const end = start + slice;

        // slice
        ctx.beginPath();
        ctx.moveTo(0,0);
        ctx.arc(0,0,radius,start,end);
        ctx.closePath();
        ctx.fillStyle = sliceColors[i%2];
        ctx.fill();

        // slice border
        ctx.lineWidth = 6;
        ctx.strokeStyle = "rgba(255,255,255,.85)";
        ctx.stroke();

        // label + emoji
        const mid = start + slice/2;
        ctx.save();
        ctx.rotate(mid);

        // emoji
        ctx.font = "900 44px system-ui, Apple Color Emoji, Segoe UI Emoji";
        ctx.textAlign = "center";
        ctx.textBaseline = "middle";
        ctx.fillText(menus[i].emoji, radius * 0.62, 0);

        // label pill
        const text = menus[i].label;
        ctx.font = "900 20px system-ui, -apple-system, 'Noto Sans KR', sans-serif";
        const w = ctx.measureText(text).width;
        const pillW = w + 26, pillH = 34;
        const px = radius * 0.62;
        const py = 44;

        // pill background
        ctx.fillStyle = "rgba(255,255,255,.92)";
        roundRect(ctx, px - pillW/2, py - pillH/2, pillW, pillH, 999);
        ctx.fill();

        // pill stroke
        ctx.strokeStyle = "rgba(0,0,0,.06)";
        ctx.lineWidth = 2;
        ctx.stroke();

        // pill text
        ctx.fillStyle = "rgba(17,24,39,.92)";
        ctx.fillText(text, px, py+1);

        ctx.restore();
      }

      // inner highlight ring
      ctx.beginPath();
      ctx.arc(0,0,radius-8,0,Math.PI*2);
      ctx.lineWidth = 10;
      ctx.strokeStyle = "rgba(207,246,255,.9)";
      ctx.stroke();

      ctx.restore();

      // center dot (cute)
      ctx.save();
      ctx.translate(cx, cy);
      ctx.beginPath();
      ctx.arc(0,0,10,0,Math.PI*2);
      ctx.fillStyle = "rgba(17,24,39,.08)";
      ctx.fill();
      ctx.restore();
    }

    function roundRect(ctx, x, y, w, h, r){
      const rr = Math.min(r, w/2, h/2);
      ctx.beginPath();
      ctx.moveTo(x+rr, y);
      ctx.arcTo(x+w, y, x+w, y+h, rr);
      ctx.arcTo(x+w, y+h, x, y+h, rr);
      ctx.arcTo(x, y+h, x, y, rr);
      ctx.arcTo(x, y, x+w, y, rr);
      ctx.closePath();
    }

    // ====== Spin logic ======
    let spinning = false;
    let currentRotation = 0; // radians

    const spinBtn = document.getElementById("spinBtn");
    const pop = document.getElementById("pop");
    const popEmoji = document.getElementById("popEmoji");
    const popLabel = document.getElementById("popLabel");

    function showPop(menu){
      popEmoji.textContent = menu.emoji;
      popLabel.textContent = menu.label;
      pop.classList.remove("on");
      // restart animation
      requestAnimationFrame(()=> pop.classList.add("on"));
      // hide after a bit
      setTimeout(()=> pop.classList.remove("on"), 1500);
    }

    function pickIndexFromRotation(rot){
      // Pointer is at top (angle -90deg in canvas coordinates).
      // Our slices start at 0 rad to the right; need mapping.
      const slice = (Math.PI * 2) / menus.length;
      // angle at pointer relative to wheel rotation
      const pointerAngle = (Math.PI * 1.5); // 270deg (top) in standard unit circle
      const angle = (pointerAngle - (rot % (Math.PI*2)) + Math.PI*2) % (Math.PI*2);
      const index = Math.floor(angle / slice) % menus.length;
      return index;
    }

    function spin(){
      if(spinning) return;
      spinning = true;
      spinBtn.disabled = true;
      spinBtn.textContent = "돌리는 중…";

      // choose result index
      const targetIndex = Math.floor(Math.random() * menus.length);

      // calculate target rotation so that targetIndex lands at pointer
      const slice = (Math.PI * 2) / menus.length;

      // We want slice center aligned to pointer
      const pointerAngle = Math.PI * 1.5; // top
      const targetCenter = (targetIndex + 0.5) * slice;

      // rotation such that (pointerAngle - rotation) hits targetCenter
      // => rotation = pointerAngle - targetCenter
      let desired = pointerAngle - targetCenter;

      // add multiple spins + keep continuity
      const spins = 6; // full turns
      const base = currentRotation;
      // normalize desired to be near next turn
      while(desired < base) desired += Math.PI * 2;
      const finalRotation = desired + spins * Math.PI * 2;

      const duration = 2800; // ms
      const start = performance.now();

      function easeOutCubic(t){ return 1 - Math.pow(1-t,3); }

      function frame(now){
        const t = Math.min(1, (now - start) / duration);
        const eased = easeOutCubic(t);

        currentRotation = base + (finalRotation - base) * eased;
        drawWheel(currentRotation);

        if(t < 1){
          requestAnimationFrame(frame);
        }else{
          // tiny settle wobble
          settle(targetIndex);
        }
      }
      requestAnimationFrame(frame);
    }

    function settle(targetIndex){
      const wobble = 10 * (Math.PI/180); // 10deg
      const base = currentRotation;
      const duration = 260;
      const start = performance.now();

      function frame(now){
        const t = Math.min(1, (now - start)/duration);
        // damped wobble
        const w = Math.cos(t * Math.PI * 2) * (1 - t);
        currentRotation = base + wobble * w;
        drawWheel(currentRotation);

        if(t < 1) requestAnimationFrame(frame);
        else{
          const idx = pickIndexFromRotation(currentRotation);
          showPop(menus[idx]);
          spinBtn.disabled = false;
          spinBtn.textContent = "룰렛 돌리기";
          spinning = false;
        }
      }
      requestAnimationFrame(frame);
    }

    // click center badge or button
    spinBtn.addEventListener("click", spin);
    document.querySelector(".center-badge").addEventListener("click", spin);

    // initial draw
    fitCanvas();
    window.addEventListener("resize", fitCanvas);
  </script>
</body>
</html>

