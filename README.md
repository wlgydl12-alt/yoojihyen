<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>마이페이지 · 음식룰렛 (UT Prototype)</title>
  <style>
    :root{
      --bg:#F3F4F6;
      --card:#fff;
      --text:#111827;
      --muted:#6B7280;
      --line:rgba(0,0,0,.06);

      --mint:#2AC1BC;

      --gold:#B9852B;
      --gold2:#F4C542;
      --shadow:0 12px 26px rgba(0,0,0,.10);
      --shadow2:0 8px 18px rgba(0,0,0,.08);
      --radius:16px;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      font-family: system-ui,-apple-system,"Segoe UI",Roboto,"Noto Sans KR",sans-serif;
      background:#fff;
      color:var(--text);
    }
    .app{
      width:min(390px, 100%);
      margin:0 auto;
      background:var(--bg);
      min-height:100vh;
      padding-bottom:88px;
    }

    /* header */
    .header{
      padding:14px 16px 8px;
      display:flex; align-items:center; justify-content:space-between;
    }
    .h-title{font-size:18px; font-weight:900; letter-spacing:-.03em;}
    .h-icons{display:flex; gap:10px;}
    .h-dot{width:18px;height:18px;border-radius:999px;background:rgba(0,0,0,.22);opacity:.35;}

    /* search pill */
    .search{
      margin:0 16px 10px;
      padding:10px 12px;
      background:rgba(0,0,0,.06);
      border-radius:999px;
      font-size:12px;
      font-weight:800;
      display:flex; align-items:center; justify-content:space-between;
      color:#374151;
    }
    .search .chev{opacity:.55}

    /* roulette block */
    .rouletteArea{
      margin:0 16px 10px;
      position:relative;
    }
    .stepNo{
      position:absolute; left:0; top:0;
      font-size:22px; font-weight:900;
    }
    .wheelWrap{
      display:flex; justify-content:center; align-items:center;
      position:relative;
      margin-top:8px;
    }
    canvas{
      width:290px; height:290px;
      display:block;
      filter: drop-shadow(0 18px 22px rgba(0,0,0,.12));
    }

    /* ring */
    .ring{
      position:absolute;
      width:312px; height:312px;
      border-radius:999px;
      border:10px solid var(--gold);
      box-shadow: 0 10px 18px rgba(0,0,0,.10) inset;
      pointer-events:none;
    }
    .bulbs{
      position:absolute;
      width:312px;height:312px;border-radius:999px;
      pointer-events:none;
    }
    .bulb{
      position:absolute;
      width:10px;height:10px;border-radius:999px;
      background:#fff;
      box-shadow:0 0 10px rgba(255,255,255,.65);
      opacity:.85;
    }

    /* center image */
    .centerImg{
      position:absolute;
      width:86px;height:86px;border-radius:999px;
      background:#fff;
      border:6px solid #fff;
      box-shadow:var(--shadow2);
      overflow:hidden;
      z-index:3;
      display:grid; place-items:center;
    }
    .centerImg img{
      width:100%;height:100%;
      object-fit:cover;
      display:block;
    }

    /* sparkles */
    .spark{
      position:absolute;
      width:12px;height:12px;
      background:#111827;
      border-radius:4px;
      transform:rotate(45deg);
      opacity:.9;
      z-index:4;
    }
    .spark.s1{left:66px; top:122px}
    .spark.s2{right:66px; top:144px; transform:rotate(18deg)}

    /* thought bubble (emoji result) */
    .bubble{
      position:absolute;
      right:18px; top:70px;
      width:96px;height:62px;
      background:#fff;
      border-radius:999px;
      box-shadow:var(--shadow2);
      display:flex; align-items:center; justify-content:center;
      font-size:26px;
      z-index:5;
    }
    .bubble:after{
      content:"";
      position:absolute;
      left:14px;
      bottom:-10px;
      width:18px;height:18px;
      background:#fff;
      transform:rotate(45deg);
      border-radius:4px;
      box-shadow:var(--shadow2);
    }

    /* right hint */
    .rightHint{
      position:absolute;
      right:0; top:205px;
      width:90px;
      text-align:center;
      font-size:12px;
      font-weight:900;
      line-height:1.2;
    }
    .arrow{
      margin:6px auto 0;
      width:0;height:0;
      border-left:10px solid transparent;
      border-right:10px solid transparent;
      border-top:14px solid #EF4444;
    }

    /* button row */
    .btnRow{
      margin:8px 16px 8px;
      display:grid;
      grid-template-columns: 1fr 1fr 46px;
      gap:10px;
      align-items:stretch;
    }
    .boxBtn{
      background:#FDE68A;
      border:2px solid var(--gold);
      border-radius:10px;
      box-shadow:0 6px 0 rgba(0,0,0,.08);
      padding:10px;
      font-weight:900;
      font-size:12px;
      display:flex; align-items:center; justify-content:center;
      text-align:center;
      line-height:1.2;
      color:#111827;
      user-select:none;
    }
    .boxBtn.small{
      justify-content:flex-start;
      font-weight:800;
      font-size:11px;
      padding:8px 10px;
    }
    .gearBtn{
      background:#FDE68A;
      border:2px solid var(--gold);
      border-radius:10px;
      box-shadow:0 6px 0 rgba(0,0,0,.08);
      display:grid; place-items:center;
      cursor:pointer;
      user-select:none;
      overflow:hidden;
    }
    .gearBtn .inner{
      width:28px;height:28px;border-radius:8px;
      background:#EF4444;
      color:#fff;
      display:grid; place-items:center;
      font-weight:900;
    }

    /* club card */
    .club{
      margin:0 16px 10px;
      background:#fff;
      border-radius:14px;
      border:1px solid var(--line);
      box-shadow:var(--shadow2);
      padding:12px;
    }
    .club .title{display:flex; align-items:center; gap:8px; font-weight:900; font-size:13px;}
    .mintDot{width:10px;height:10px;border-radius:999px;background:var(--mint);}
    .club .desc{margin-top:6px;font-size:12px;font-weight:800;color:#0EA5A4;}

    /* list rows (쿠폰함/선물함/배민페이) */
    .list{
      margin:0 16px;
      background:#fff;
      border-radius:14px;
      border:1px solid var(--line);
      box-shadow:var(--shadow2);
      overflow:hidden;
    }
    .li{
      padding:14px 12px;
      display:flex; align-items:center; justify-content:space-between;
      border-top:1px solid var(--line);
      gap:10px;
      font-size:13px;
      font-weight:900;
    }
    .li:first-child{border-top:none}
    .li .left{display:flex; align-items:center; gap:10px;}
    .bDot{width:10px;height:10px;border-radius:999px;background:#111827;}
    .bDot.purple{background:#7C3AED}
    .bDot.yellow{background:#F59E0B}
    .li .right{opacity:.9}

    /* section list */
    .section{
      margin:12px 16px 0;
      background:#fff;
      border-radius:14px;
      border:1px solid var(--line);
      box-shadow:var(--shadow2);
      overflow:hidden;
    }
    .secRow{
      padding:13px 12px;
      display:flex; align-items:center; justify-content:space-between;
      border-top:1px solid var(--line);
      font-weight:900;
      font-size:13px;
    }
    .secRow:first-child{border-top:none}
    .secLeft{display:flex; align-items:center; gap:10px;}
    .circle{
      width:14px;height:14px;border-radius:999px;
      border:2px solid #111827;
      background:#fff;
    }
    .chev{opacity:.5}

    /* banner (no external image) */
    .banner{
      margin:12px 16px 0;
      border-radius:14px;
      overflow:hidden;
      border:1px solid var(--line);
      box-shadow:var(--shadow2);
      background: linear-gradient(90deg,#FF7A00,#FF3D00);
      color:#fff;
      padding:16px 14px;
      position:relative;
    }
    .banner .kicker{font-weight:900; font-size:12px; opacity:.92}
    .banner .big{margin-top:6px; font-weight:900; font-size:20px; letter-spacing:-.02em}
    .banner .btn{
      display:inline-block;
      margin-top:10px;
      background:rgba(0,0,0,.55);
      padding:8px 10px;
      border-radius:999px;
      font-size:12px;
      font-weight:900;
    }
    .banner:after{
      content:"🍖";
      position:absolute;
      right:14px; bottom:10px;
      font-size:54px;
      opacity:.9;
      filter: drop-shadow(0 10px 12px rgba(0,0,0,.22));
    }

    /* mini grid */
    .miniGrid{
      margin:12px 16px 0;
      display:grid;
      grid-template-columns: repeat(4, 1fr);
      gap:10px;
    }
    .miniCard{
      background:#fff;
      border-radius:14px;
      border:1px solid var(--line);
      box-shadow:var(--shadow2);
      padding:10px 8px;
      text-align:center;
    }
    .cap{
      font-size:10px;
      font-weight:900;
      color:#6366F1;
      background:rgba(99,102,241,.10);
      border-radius:999px;
      display:inline-block;
      padding:2px 6px;
      margin-bottom:6px;
    }
    .miniEmoji{
      width:44px;height:44px;border-radius:16px;
      margin:0 auto 6px;
      background: linear-gradient(135deg, rgba(103,215,234,.35), rgba(42,193,188,.15));
      display:grid; place-items:center;
      font-size:24px;
    }
    .miniCard .t{font-size:12px;font-weight:900;letter-spacing:-.02em;}
    .miniCard .s{font-size:11px;font-weight:800;color:var(--muted);margin-top:2px;}

    /* bottom tab */
    .tabbar{
      position:fixed;
      left:50%;
      transform:translateX(-50%);
      bottom:10px;
      width:min(390px, calc(100% - 18px));
      background:#fff;
      border-radius:18px;
      box-shadow:var(--shadow);
      border:1px solid var(--line);
      padding:10px 10px;
      display:grid;
      grid-template-columns: repeat(5, 1fr);
      gap:6px;
      z-index:99;
    }
    .tab{
      display:grid; place-items:center; gap:4px;
      font-size:11px; font-weight:900;
      color:#6B7280;
      user-select:none;
    }
    .tab .dot{
      width:28px;height:28px;border-radius:10px;
      background:rgba(17,24,39,.06);
      display:grid; place-items:center;
      font-size:14px;
    }
    .tab.active{color:#111827}
    .tab.active .dot{
      background: rgba(42,193,188,.16);
      border:1px solid rgba(42,193,188,.22);
    }

    /* upload (hidden but usable) */
    .upload{
      margin:10px 16px 0;
      font-size:12px;
      color:var(--muted);
      font-weight:800;
      display:flex;
      gap:10px;
      align-items:center;
      justify-content:space-between;
    }
    .upload input{max-width:220px;}
    .tiny{opacity:.8;font-weight:800;}
  </style>
</head>

<body>
  <div class="app">
    <div class="header">
      <div class="h-title">마이배민</div>
      <div class="h-icons" aria-hidden="true">
        <div class="h-dot"></div>
        <div class="h-dot"></div>
      </div>
    </div>

    <div class="search">
      <div>배민의 음식주문 경험, 어떠셨나요?</div>
      <div class="chev">›</div>
    </div>

    <div class="rouletteArea">
      <div class="stepNo">1</div>

      <div class="wheelWrap">
        <canvas id="wheel" width="600" height="600" aria-label="음식 룰렛"></canvas>
        <div class="ring" aria-hidden="true"></div>
        <div class="bulbs" id="bulbs" aria-hidden="true"></div>

        <!-- ✅ 내 이미지: 아래 업로드로 바꿀 수 있음 -->
        <div class="centerImg" title="내 이미지">
          <img id="myImg" alt="my image"
               src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='200' height='200'><rect width='200' height='200' fill='%23fff'/><text x='50%25' y='54%25' text-anchor='middle' dominant-baseline='middle' font-size='34' fill='%236b7280' font-family='Arial'>MY</text></svg>">
        </div>

        <div class="spark s1" aria-hidden="true"></div>
        <div class="spark s2" aria-hidden="true"></div>

        <div class="bubble" id="bubble" title="결과">🍗</div>
      </div>

      <div class="rightHint">
        지금 땡기는<br/>메뉴는?
        <div class="arrow" aria-hidden="true"></div>
      </div>
    </div>

    <div class="btnRow">
      <div class="boxBtn small">Lv.37 골라먹기<br/>냉삼곱쌈</div>
      <div class="boxBtn">맞춤형 메뉴 배치</div>
      <div class="gearBtn" id="gearBtn" title="룰렛 돌리기">
        <div class="inner">⚙️</div>
      </div>
    </div>

    <!-- 내 이미지 업로드(압축/파일 없이 해결) -->
    <div class="upload">
      <div class="tiny">내 이미지 넣기</div>
      <input id="imgInput" type="file" accept="image/*" />
    </div>

    <div class="club">
      <div class="title"><span class="mintDot"></span>배민클럽 이용중</div>
      <div class="desc">스타벅스 아메리카노 + 처갓집양념치킨 쿠폰 받아요!</div>
    </div>

    <div class="list">
      <div class="li">
        <div class="left"><span class="bDot purple"></span>쿠폰함</div>
        <div class="right">10장 ›</div>
      </div>
      <div class="li">
        <div class="left"><span class="bDot"></span>선물함</div>
        <div class="right">0원 ›</div>
      </div>
      <div class="li">
        <div class="left"><span class="bDot yellow"></span>배민페이</div>
        <div class="right">등록하기 ›</div>
      </div>
    </div>

    <div class="section">
      <div class="secRow"><div class="secLeft"><span class="circle"></span>진행중인 이벤트</div><div class="chev">›</div></div>
      <div class="secRow"><div class="secLeft"><span class="circle"></span>혜택 모아보기</div><div class="chev">›</div></div>
      <div class="secRow"><div class="secLeft"><span class="circle"></span>배민클럽</div><div class="chev">›</div></div>
      <div class="secRow"><div class="secLeft"><span class="circle"></span>가족계정</div><div class="chev">›</div></div>
    </div>

    <div class="banner">
      <div class="kicker">328회 칼질로 육즙 가득</div>
      <div class="big">육식사관학교<br/>최대 6,000원 할인</div>
      <div class="btn">즉시할인받기 ›</div>
    </div>

    <div class="section">
      <div class="secRow"><div class="secLeft"><span class="circle"></span>진행중인 이벤트</div><div class="chev">›</div></div>
      <div class="secRow"><div class="secLeft"><span class="circle"></span>혜택 모아보기</div><div class="chev">›</div></div>
      <div class="secRow"><div class="secLeft"><span class="circle"></span>배민 신한카드 발친구</div><div class="chev">›</div></div>
      <div class="secRow"><div class="secLeft"><span class="circle"></span>샌드위치 쌓고 혜택 받기</div><div class="chev">›</div></div>
    </div>

    <div class="miniGrid">
      <div class="miniCard"><div class="cap">최대 70만원</div><div class="miniEmoji">🎧</div><div class="t">배민리서치</div><div class="s">리서치 참여해요</div></div>
      <div class="miniCard"><div class="cap">최대 70만원</div><div class="miniEmoji">🎁</div><div class="t">배민리서치</div><div class="s">리서치 참여해요</div></div>
      <div class="miniCard"><div class="cap">최대 70만원</div><div class="miniEmoji">📣</div><div class="t">배민리서치</div><div class="s">리서치 참여해요</div></div>
      <div class="miniCard"><div class="cap">최대 70만원</div><div class="miniEmoji">🙂</div><div class="t">배민리서치</div><div class="s">리서치 참여해요</div></div>

      <div class="miniCard"><div class="cap">최대 70만원</div><div class="miniEmoji">⚽</div><div class="t">배민리서치</div><div class="s">리서치 참여해요</div></div>
      <div class="miniCard"><div class="cap">최대 70만원</div><div class="miniEmoji">🍱</div><div class="t">배민리서치</div><div class="s">리서치 참여해요</div></div>
      <div class="miniCard"><div class="cap">최대 70만원</div><div class="miniEmoji">🧾</div><div class="t">배민리서치</div><div class="s">리서치 참여해요</div></div>
      <div class="miniCard"><div class="cap">최대 70만원</div><div class="miniEmoji">♾️</div><div class="t">배민리서치</div><div class="s">리서치 참여해요</div></div>
    </div>

    <div class="section" style="margin-bottom:14px;">
      <div class="secRow"><div class="secLeft"><span class="circle"></span>고객센터</div><div class="chev">›</div></div>
      <div class="secRow"><div class="secLeft"><span class="circle"></span>자주 묻는 질문</div><div class="chev">›</div></div>
      <div class="secRow"><div class="secLeft"><span class="circle"></span>공지사항</div><div class="chev">›</div></div>
      <div class="secRow"><div class="secLeft"><span class="circle"></span>약관 및 정책</div><div class="chev">›</div></div>
      <div class="secRow"><div class="secLeft"><span class="circle"></span>현재 버전 1524.1</div><div class="chev">›</div></div>
    </div>
  </div>

  <nav class="tabbar" aria-label="하단 탭">
    <div class="tab"><div class="dot">🏠</div>홈</div>
    <div class="tab"><div class="dot">🧺</div>장보기·쇼핑</div>
    <div class="tab"><div class="dot">♡</div>찜</div>
    <div class="tab"><div class="dot">🧾</div>주문내역</div>
    <div class="tab active"><div class="dot">🙂</div>마이배민</div>
  </nav>

<script>
  // 메뉴 6개 (이모지 고정)
  const menus = [
    { label:"치킨", emoji:"🍗" },
    { label:"피자", emoji:"🍕" },
    { label:"초밥", emoji:"🍣" },
    { label:"버거", emoji:"🍔" },
    { label:"떡볶이", emoji:"🍢" },
    { label:"국밥", emoji:"🍲" },
  ];

  const canvas = document.getElementById("wheel");
  const ctx = canvas.getContext("2d");
  const cx = canvas.width/2, cy = canvas.height/2;
  const radius = 250;

  const bubble = document.getElementById("bubble");
  const gearBtn = document.getElementById("gearBtn");

  // bulbs 생성
  const bulbsEl = document.getElementById("bulbs");
  const bulbCount = 28;
  for(let i=0;i<bulbCount;i++){
    const b=document.createElement("div");
    b.className="bulb";
    const ang=(i/bulbCount)*Math.PI*2;
    const x=156 + Math.cos(ang)*132 - 5;
    const y=156 + Math.sin(ang)*132 - 5;
    b.style.left=x+"px";
    b.style.top=y+"px";
    b.style.opacity=(i%2?0.92:0.62);
    bulbsEl.appendChild(b);
  }

  // 룰렛 그리기 (캡쳐처럼 흰 섹션 + 얇은 라인)
  let spinning=false;
  let currentRotation=0;

  function drawWheel(rot){
    ctx.clearRect(0,0,canvas.width,canvas.height);
    ctx.save();
    ctx.translate(cx, cy);
    ctx.rotate(rot);

    const slice=(Math.PI*2)/menus.length;

    for(let i=0;i<menus.length;i++){
      const start=i*slice, end=start+slice;

      ctx.beginPath();
      ctx.moveTo(0,0);
      ctx.arc(0,0,radius,start,end);
      ctx.closePath();
      ctx.fillStyle = (i%2===0) ? "#FFFFFF" : "#F7F7F7";
      ctx.fill();

      ctx.lineWidth=4;
      ctx.strokeStyle="rgba(0,0,0,.10)";
      ctx.stroke();

      const mid=start+slice/2;
      ctx.save();
      ctx.rotate(mid);

      // emoji
      ctx.font="900 44px system-ui, Apple Color Emoji, Segoe UI Emoji";
      ctx.textAlign="center";
      ctx.textBaseline="middle";
      ctx.fillText(menus[i].emoji, radius*0.60, 0);

      ctx.restore();
    }

    // inner ring
    ctx.beginPath();
    ctx.arc(0,0,radius-8,0,Math.PI*2);
    ctx.lineWidth=10;
    ctx.strokeStyle="rgba(0,0,0,.12)";
    ctx.stroke();

    ctx.restore();
  }

  function pickIndex(rot){
    const slice=(Math.PI*2)/menus.length;
    const pointerAngle=Math.PI*1.5; // top
    const angle=(pointerAngle - (rot%(Math.PI*2)) + Math.PI*2)%(Math.PI*2);
    return Math.floor(angle/slice)%menus.length;
  }

  function spin(){
    if(spinning) return;
    spinning=true;

    const targetIndex=Math.floor(Math.random()*menus.length);
    const slice=(Math.PI*2)/menus.length;
    const pointerAngle=Math.PI*1.5;
    const targetCenter=(targetIndex+0.5)*slice;

    let desired=pointerAngle - targetCenter;
    const base=currentRotation;
    while(desired<base) desired += Math.PI*2;

    const spins=7;
    const finalRot=desired + spins*Math.PI*2;

    const duration=2600;
    const start=performance.now();
    const easeOut=t=>1-Math.pow(1-t,3);

    function frame(now){
      const t=Math.min(1,(now-start)/duration);
      const e=easeOut(t);
      currentRotation=base + (finalRot-base)*e;
      drawWheel(currentRotation);
      if(t<1) requestAnimationFrame(frame);
      else{
        const idx=pickIndex(currentRotation);
        bubble.textContent=menus[idx].emoji;
        spinning=false;
      }
    }
    requestAnimationFrame(frame);
  }

  gearBtn.addEventListener("click", spin);

  // 내 이미지 업로드
  const imgInput=document.getElementById("imgInput");
  const myImg=document.getElementById("myImg");
  const saved=localStorage.getItem("myCenterImage");
  if(saved) myImg.src=saved;

  imgInput.addEventListener("change",(e)=>{
    const f=e.target.files?.[0];
    if(!f) return;
    const reader=new FileReader();
    reader.onload=()=>{
      myImg.src=reader.result;
      localStorage.setItem("myCenterImage", reader.result);
    };
    reader.readAsDataURL(f);
  });

  drawWheel(currentRotation);
</script>
</body>
</html>
