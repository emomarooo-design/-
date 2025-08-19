<img width="512" height="512" alt="icon" src="https://github.com/user-attachments/assets/ff383012-4642-4f68-a7df-8b60ac4928be" />
[index.html](https://github.com/user-attachments/files/21852008/index.html)
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>حرب المعلومات - لعب ثنائي</title>
  <link rel="manifest" href="manifest.json">
  <link rel="icon" href="assets/icon.png">
  <meta name="theme-color" content="#2a9df4">
  <style>
    :root{
      --p1:#2a9df4; /* أزرق واضح */
      --p2:#f4a261; /* برتقالي دافئ */
      --ok:#2ecc71;
      --bad:#e74c3c;
      --bg:#0f172a; /* كحلي داكن */
      --card:#111827ee;
      --muted:#94a3b8;
      --chip:#1f2937;
      --accent:#ffd166;
      --text:#e5e7eb;
    }
    *{box-sizing:border-box}
    html,body{margin:0;height:100%;font-family: system-ui, -apple-system, Segoe UI, Roboto, Noto Sans Arabic, Arial}
    body{background:linear-gradient(120deg,#0f172a 0%, #0b1222 100%); color:var(--text)}
    .container{max-width:980px;margin:0 auto;padding:16px}
    header{display:flex;justify-content:space-between;align-items:center;gap:12px}
    .brand{display:flex;align-items:center;gap:10px}
    .brand img{width:36px;height:36px}
    .brand h1{font-size:22px;margin:0}
    .pill{background:var(--chip);padding:8px 12px;border-radius:24px;font-size:13px;color:var(--muted)}
    .card{background:var(--card);border:1px solid #1f2937;border-radius:16px;padding:16px;backdrop-filter: blur(6px);}
    .row{display:flex;gap:12px;flex-wrap:wrap}
    .col{flex:1 1 300px}
    .hud{display:grid;grid-template-columns:1fr auto 1fr;gap:8px;align-items:center}
    .player{display:flex;align-items:center;gap:10px}
    .name{font-weight:700}
    .state{display:flex;gap:4px;align-items:center}
    .segment{width:14px;height:22px;border-radius:3px;background:#1f2937;overflow:hidden;position:relative;border:1px solid #263142}
    .segment .fill{position:absolute;left:0;top:0;height:100%;width:0;background:linear-gradient(90deg,var(--p1),#7cc6ff)}
    .p2 .segment .fill{background:linear-gradient(90deg,var(--p2),#ffd6a1)}
    .timer{font-weight:800;font-size:20px;padding:8px 14px;border-radius:12px;background:#162032;min-width:70px;text-align:center}
    .question{font-size:20px;line-height:1.7}
    .category{font-size:12px;color:var(--muted);margin-bottom:6px}
    .answers{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:12px}
    button{cursor:pointer}
    .btn{border:none;padding:12px;border-radius:12px;background:#1f2937;color:var(--text);font-weight:600}
    .btn:hover{filter:brightness(1.1)}
    .btn.primary{background:linear-gradient(90deg,var(--p1),#7cc6ff);color:#06131f}
    .btn.orange{background:linear-gradient(90deg,var(--p2),#ffd6a1);color:#3c1e00}
    .lifelines{display:flex;gap:8px;flex-wrap:wrap;margin-top:8px}
    .chip{padding:8px 10px;border-radius:10px;background:#172033;border:1px dashed #2a3a55;font-size:13px}
    .chip.used{opacity:.45;text-decoration:line-through}
    .log{max-height:160px;overflow:auto;font-size:13px;color:var(--muted)}
    .center{display:flex;justify-content:center;align-items:center;gap:10px}
    .winner{font-size:26px;text-align:center}
    .footer{margin-top:18px;font-size:12px;color:var(--muted);text-align:center}
    .toggle{display:inline-flex;background:#122039;border-radius:12px;overflow:hidden;border:1px solid #22324b}
    .toggle button{padding:8px 12px;background:transparent;border:none;color:var(--muted)}
    .toggle .active{background:#1a2b4d;color:#fff}
    .help{font-size:13px;color:var(--muted)}
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="brand">
        <img src="assets/icon.png" alt="حرب المعلومات">
        <h1>حرب المعلومات</h1>
        <span class="pill">لعب ثنائي • عربي</span>
      </div>
      <div class="toggle">
        <button id="newGameBtn" class="active">لعبة جديدة</button>
        <button id="howBtn">طريقة اللعب</button>
      </div>
    </header>

    <div id="how" class="card" style="display:none">
      <b>القواعد</b>
      <ul>
        <li>لاعبان يتناوبان الإجابة على أسئلة عامة وثقافية وإسلامية.</li>
        <li>لكل لاعب دولة مكوّنة من 10 أجزاء. الخطأ يسبب فقدان <b>جزئين</b> (يعادل خسارة واحدة)، ومن يخسر 10 أجزاء يخسر الحرب (أي بعد <b>5</b> أخطاء).</li>
        <li>ثلاث وسائل مساعدة (مرة واحدة لكل لاعب): 
          <ul>
            <li><b>تغيير السؤال</b></li>
            <li><b>تقريب الإجابة</b> (تلميح ذكي مرتبط بالسؤال)</li>
            <li><b>تنازل جزئي</b> (تخسر جزءاً واحداً من دولتك دون أن يكسب الخصم، وتتجاوز السؤال)</li>
          </ul>
        </li>
        <li>الوقت لكل سؤال: <b id="ruleTime">20</b> ثانية.</li>
      </ul>
    </div>

    <div id="game" class="card">
      <div class="hud">
        <div class="player" id="p1">
          <div class="name">اللاعب ١</div>
          <div class="state" id="state1"></div>
        </div>
        <div class="timer" id="timer">20</div>
        <div class="player p2" id="p2" style="justify-self:end">
          <div class="name">اللاعب ٢</div>
          <div class="state" id="state2"></div>
        </div>
      </div>

      <div class="center" style="margin-top:10px">
        الدور الحالي: <span id="turnLabel" class="pill">اللاعب ١</span>
      </div>

      <div class="category" id="category"></div>
      <div class="question" id="question">اضغط "لعبة جديدة" للبدء</div>
      <div class="answers" id="answers"></div>

      <div class="lifelines" id="lifelines"></div>

      <div class="row" style="margin-top:12px">
        <div class="col">
          <button class="btn primary" id="startBtn">بدء الجولة</button>
        </div>
        <div class="col" style="text-align:end">
          <button class="btn orange" id="resetBtn">إعادة ضبط</button>
        </div>
      </div>

      <div class="card" style="margin-top:12px">
        <div class="log" id="log"></div>
      </div>

      <div id="winner" class="winner" style="display:none"></div>
    </div>

    <div class="footer">© 2025 لعبة حرب المعلومات – تعمل على المتصفح ويمكن إضافتها للشاشة الرئيسية كـ PWA عند رفعها على استضافة آمنة.</div>
  </div>

  <script>
    // بيانات الأسئلة (مختصرة ويمكنك توسيعها لاحقاً)
    const QUESTIONS = [
      {q:"ما هما المصدران الأساسيان للتشريع في الإسلام؟", a:["القرآن والسنة","الإجماع والقياس","الحديث والفقه","القرآن والتاريخ"], correct:0, cat:"إسلاميات", hint:"كتاب الله وسنة نبيّه ﷺ."},
      {q:"كم عدد أركان الإسلام؟", a:["خمسة","أربعة","ستة","سبعة"], correct:0, cat:"إسلاميات", hint:"عدد أصابع اليد الواحدة."},
      {q:"ما عاصمة المغرب؟", a:["الرباط","الدار البيضاء","فاس","مراكش"], correct:0, cat:"جغرافيا", hint:"مدينة على ساحل الأطلسي قرب مصب أبي رقراق."},
      {q:"العالم الذي وضع قانون الجاذبية هو؟", a:["آينشتاين","نيوتن","غاليلو","كوبرنيكوس"], correct:1, cat:"علوم", hint:"قصة التفاحة مشهورة معه."},
      {q:"سورة تبدأ بـ (اقرأ باسم ربك) هي؟", a:["العلق","الفاتحة","الرحمن","القدر"], correct:0, cat:"إسلاميات", hint:"من أوائل ما نزل من القرآن."},
      {q:"ما أكبر قارة في العالم مساحةً؟", a:["إفريقيا","آسيا","أوروبا","أمريكا الجنوبية"], correct:1, cat:"جغرافيا", hint:"تضم الصين والهند."},
      {q:"من هو مؤلف 'حي بن يقظان'؟", a:["ابن سينا","ابن رشد","ابن طفيل","أبو حيان"], correct:2, cat:"أدب", hint:"فيلسوف أندلسي."},
      {q:"عدد الخلفاء الراشدين؟", a:["ثلاثة","أربعة","خمسة","ستة"], correct:1, cat:"تاريخ إسلامي", hint:"أبو بكر، عمر، عثمان، علي."},
      {q:"العنصر الرئيسي في تكوّن الشمس؟", a:["الهيدروجين","الأكسجين","الهيليوم","النيتروجين"], correct:0, cat:"علوم", hint:"أخف العناصر."},
      {q:"لغة القرآن الكريم؟", a:["العربية","العبرية","السريانية","الفارسية"], correct:0, cat:"إسلاميات", hint:"لسان مبين."},
      {q:"أطول أنهار العالم؟", a:["الأمازون","النيل","اليانغتسي","الكونغو"], correct:1, cat:"جغرافيا", hint:"يمر بمصر والسودان."},
      {q:"الكعبة تقع في مدينة؟", a:["المدينة","مكة","الطائف","جدة"], correct:1, cat:"إسلاميات", hint:"بلد الله الحرام."},
      {q:"كم عدد الحروف العربية؟", a:["28","29","27","30"], correct:0, cat:"لغة", hint:"أقل من الثلاثين باثنين."},
      {q:"أول الخلفاء الراشدين؟", a:["عمر","عثمان","علي","أبو بكر"], correct:3, cat:"تاريخ إسلامي", hint:"صديق هذه الأمة."},
      {q:"مَن مكتشف جرثومة السل؟", a:["باستور","كوخ","فلمنغ","كوري"], correct:1, cat:"علوم", hint:"روبرت ..."},
      {q:"عاصمة فرنسا؟", a:["نيس","ليون","باريس","مرسيليا"], correct:2, cat:"جغرافيا", hint:"مدينة النور."},
      {q:"أركان الإيمان عددها؟", a:["خمسة","ستة","سبعة","أربعة"], correct:1, cat:"إسلاميات", hint:"تزيد عن أركان الإسلام بواحد."},
      {q:"اليوم الوطني للمملكة العربية السعودية في أي تاريخ؟", a:["23 سبتمبر","1 يوليو","2 ديسمبر","1 نوفمبر"], correct:0, cat:"ثقافة عامة", hint:"يوافق 23/9."},
      {q:"سورة يُطلق عليها عروس القرآن؟", a:["يس","الرحمن","يوسف","الواقعة"], correct:1, cat:"إسلاميات", hint:"تكرر فيها 'فبأي آلاء ربكما تكذبان'."},
      {q:"العاصمة الإدارية لمصر (حتى وقتنا)؟", a:["القاهرة","العاصمة الإدارية الجديدة","الإسكندرية","الجيزة"], correct:1, cat:"جغرافيا", hint:"مدينة حديثة شرق القاهرة."},
      {"q": "في أي عام بدأت الحرب العالمية الأولى؟", "a": ["1914", "1918", "1939", "1920"], "correct": 0, "cat": "تاريخ عالمي", "hint": "قبل الحرب العالمية الثانية بـ 25 سنة."},
      {"q": "في أي عام انتهت الحرب العالمية الثانية؟", "a": ["1939", "1945", "1950", "1948"], "correct": 1, "cat": "تاريخ عالمي", "hint": "بعد إلقاء القنبلتين الذريتين على اليابان."},
      {"q": "أول رئيس لجمهورية مصر العربية؟", "a": ["جمال عبد الناصر", "محمد نجيب", "أنور السادات", "حسني مبارك"], "correct": 1, "cat": "سياسة مصرية", "hint": "قاد ثورة 1952 مع الضباط الأحرار."},
      {"q": "ما هي عاصمة كازاخستان الحالية؟", "a": ["نور سلطان", "ألماتي", "أستانا", "طشقند"], "correct": 0, "cat": "جغرافيا", "hint": "سميت سابقاً أستانا."},
      {"q": "من هو أول أمين عام لجامعة الدول العربية؟", "a": ["عمرو موسى", "عبد الرحمن عزام", "محمود رياض", "كامل أبو السعادات"], "correct": 1, "cat": "سياسة عربية", "hint": "مصري تولى المنصب عام 1945."},
      {"q": "ما أطول حرب في التاريخ الحديث؟", "a": ["الحرب الفيتنامية", "الحرب الأهلية اللبنانية", "الحرب العراقية الإيرانية", "حرب المئة عام"], "correct": 3, "cat": "تاريخ حروب", "hint": "رغم اسمها فهي استمرت 116 سنة بين إنجلترا وفرنسا."},
      {"q": "ما أكبر دولة عربية من حيث المساحة؟", "a": ["مصر", "الجزائر", "السعودية", "السودان"], "correct": 1, "cat": "جغرافيا", "hint": "تقع في شمال أفريقيا."},
      {"q": "في أي عام توحّدت ألمانيا الشرقية والغربية؟", "a": ["1989", "1990", "1985", "1991"], "correct": 1, "cat": "تاريخ سياسي", "hint": "بعد سقوط جدار برلين بعام واحد."},
      {"q": "ما هي القناة الإخبارية التي أُسست في قطر عام 1996؟", "a": ["الجزيرة", "العربية", "سكاي نيوز عربية", "BBC Arabic"], "correct": 0, "cat": "إعلام", "hint": "شعارها 'الرأي والرأي الآخر'."},
      {"q": "ما هو البحر الذي يربط بين أوروبا وآسيا؟", "a": ["الأحمر", "الأسود", "المتوسط", "الكاسبي"], "correct": 1, "cat": "جغرافيا", "hint": "تطل عليه تركيا وأوكرانيا."},
      {"q": "في أي مدينة جرى توقيع معاهدة السلام بين مصر وإسرائيل عام 1979؟", "a": ["واشنطن", "القدس", "كامب ديفيد", "القاهرة"], "correct": 0, "cat": "سياسة دولية", "hint": "في عاصمة الولايات المتحدة."},
      {"q": "من هو الصحابي الذي لُقّب بسيف الله المسلول؟", "a": ["خالد بن الوليد", "علي بن أبي طالب", "عمر بن الخطاب", "الزبير بن العوام"], "correct": 0, "cat": "تاريخ إسلامي", "hint": "قائد معركة مؤتة."},
      {"q": "ما اسم الاتفاقية التي أنهت الحرب الأهلية في لبنان عام 1989؟", "a": ["اتفاق أوسلو", "اتفاق الطائف", "اتفاق مدريد", "اتفاق الدوحة"], "correct": 1, "cat": "سياسة عربية", "hint": "جرى توقيعها في السعودية."},
      {"q": "في أي عام أُعلنت دولة الإمارات العربية المتحدة؟", "a": ["1970", "1971", "1973", "1975"], "correct": 1, "cat": "تاريخ عربي", "hint": "في بداية السبعينيات."},
      {"q": "أول دولة اعترفت باستقلال الولايات المتحدة؟", "a": ["فرنسا", "المغرب", "إسبانيا", "بريطانيا"], "correct": 1, "cat": "تاريخ عالمي", "hint": "دولة عربية في شمال أفريقيا."},
      {"q": "ما الدولة التي استعمرت الجزائر لأكثر من 130 عاماً؟", "a": ["إيطاليا", "إسبانيا", "فرنسا", "بريطانيا"], "correct": 2, "cat": "تاريخ استعماري", "hint": "دولة أوروبية عاصمتها باريس."},
      {"q": "في أي عام وقعت نكبة فلسطين؟", "a": ["1945", "1947", "1948", "1956"], "correct": 2, "cat": "تاريخ عربي", "hint": "بعد انتهاء الانتداب البريطاني."},
      {"q": "أطول نهر في أوروبا؟", "a": ["الدانوب", "الفولغا", "الراين", "اللوار"], "correct": 1, "cat": "جغرافيا", "hint": "يمر بروسيا ويصب في بحر قزوين."},
      {"q": "ما اسم أول قمر صناعي أُطلق إلى الفضاء؟", "a": ["أبوللو", "سبوتنك", "فوستوك", "كوزموس"], "correct": 1, "cat": "تاريخ علوم", "hint": "أطلقه الاتحاد السوفيتي عام 1957."},
      {"q": "من هو صاحب مقولة 'أنا الدولة والدولة أنا'؟", "a": ["لويس الرابع عشر", "نابليون", "هتلر", "جورج واشنطن"], "correct": 0, "cat": "تاريخ سياسي", "hint": "ملك فرنسي يُعرف بالملك الشمس."}
    ];

    // الحالة العامة
    const state = {
      turn: 1, // 1 أو 2
      p1:{parts:0, lifelines:{swap:false,hint:false,concede:false}},
      p2:{parts:0, lifelines:{swap:false,hint:false,concede:false}},
      current:null,
      timePerQ:20,
      timer:null,
      pool:[]
    };

    const els = {
      state1:document.getElementById('state1'),
      state2:document.getElementById('state2'),
      timer:document.getElementById('timer'),
      question:document.getElementById('question'),
      answers:document.getElementById('answers'),
      category:document.getElementById('category'),
      lifelines:document.getElementById('lifelines'),
      turnLabel:document.getElementById('turnLabel'),
      log:document.getElementById('log'),
      winner:document.getElementById('winner'),
      startBtn:document.getElementById('startBtn'),
      resetBtn:document.getElementById('resetBtn'),
      newGameBtn:document.getElementById('newGameBtn'),
      howBtn:document.getElementById('how'),
    };

    document.getElementById('newGameBtn').onclick = () => {
      document.getElementById('how').style.display='none';
      newGame();
    };
    document.getElementById('howBtn').onclick = () => {
      const h = document.getElementById('how');
      h.style.display = h.style.display==='none' ? 'block' : 'none';
    };

    function renderStates(){
      const make = (parts, player) => {
        // 10 أجزاء لكل لاعب
        let html='';
        for(let i=0;i<10;i++){
          // كل جزء عبارة عن مستطيل يمكن ملؤه (0،1 أو 2)
          const filled = Math.min(2, Math.max(0, parts - i*1)); // but we store as count segments captured?
        }
      }
    }

    function buildSegments(container, parts, colorVar){
      container.innerHTML='';
      for(let i=0;i<10;i++){
        const seg = document.createElement('div');
        seg.className='segment';
        const fill = document.createElement('div');
        fill.className='fill';
        // width represents captured amount (each wrong answer = +2, concede = +1). parts = captured count out of 20.
        const capturedForThis = Math.max(0, Math.min(2, parts - i*2));
        fill.style.width = (capturedForThis/2*100)+'%';
        seg.appendChild(fill);
        container.appendChild(seg);
      }
    }

    function log(msg){
      const p=document.createElement('div'); p.textContent=msg;
      els.log.prepend(p);
    }

    function setTurn(t){
      state.turn = t;
      els.turnLabel.textContent = 'اللاعب ' + t;
      els.turnLabel.style.background = t===1 ? 'linear-gradient(90deg,var(--p1),#7cc6ff)' : 'linear-gradient(90deg,var(--p2),#ffd6a1)';
      els.turnLabel.style.color = '#00101a';
    }

    function randomQuestion(){
      if(state.pool.length===0){
        state.pool = QUESTIONS.map((_,i)=>i).sort(()=>Math.random()-0.5);
      }
      const idx = state.pool.pop();
      const q = JSON.parse(JSON.stringify(QUESTIONS[idx]));
      q.id = idx;
      return q;
    }

    function renderQuestion(){
      els.answers.innerHTML='';
      els.lifelines.innerHTML='';
      els.winner.style.display='none';
      if(!state.current){ els.question.textContent=''; els.category.textContent=''; return; }
      els.category.textContent = 'الفئة: ' + state.current.cat;
      els.question.textContent = state.current.q;
      state.current.a.forEach((ans, i)=>{
        const b=document.createElement('button');
        b.className='btn';
        b.textContent = ans;
        b.onclick = ()=> answer(i);
        els.answers.appendChild(b);
      });
      // lifelines
      const player = state.turn===1?state.p1:state.p2;
      const makeChip = (key, label, handler) => {
        const c = document.createElement('button');
        c.className = 'chip';
        c.textContent = label;
        if(player.lifelines[key]){ c.classList.add('used'); c.disabled=true; }
        c.onclick = handler;
        els.lifelines.appendChild(c);
      };
      makeChip('swap','🔄 تغيير السؤال', ()=>useSwap());
      makeChip('hint','💡 تقريب الإجابة', ()=>useHint());
      makeChip('concede','🛡️ تنازل جزئي', ()=>useConcede());
    }

    function startTimer(){
      clearInterval(state.timer);
      let t = state.timePerQ;
      els.timer.textContent = t;
      state.timer = setInterval(()=>{
        t--;
        els.timer.textContent = t;
        if(t<=0){
          clearInterval(state.timer);
          timeUp();
        }
      }, 1000);
    }

    function timeUp(){
      log(`انتهى وقت اللاعب ${state.turn}!`);
      wrong();
    }

    function answer(i){
      clearInterval(state.timer);
      const correct = i===state.current.correct;
      if(correct){
        log(`أصاب اللاعب ${state.turn}! ✅`);
        nextTurn();
      }else{
        log(`أخطأ اللاعب ${state.turn}. ❌`);
        wrong();
      }
    }

    function wrong(){
      // خطأ = الخصم يكسب جزئين (من 20)
      const opp = state.turn===1? 'p2':'p1';
      state[opp].parts = Math.min(20, state[opp].parts + 2);
      updateStatesUI();
      checkWin() || nextTurn();
    }

    function useSwap(){
      const p = state.turn===1?state.p1:state.p2;
      if(p.lifelines.swap) return;
      p.lifelines.swap = true;
      log(`اللاعب ${state.turn} استخدم: تغيير السؤال.`);
      state.current = randomQuestion();
      renderQuestion();
      startTimer();
      renderLifelinesOnly();
    }
    function useHint(){
      const p = state.turn===1?state.p1:state.p2;
      if(p.lifelines.hint) return;
      p.lifelines.hint = true;
      log(`اللاعب ${state.turn} استخدم: تقريب الإجابة.`);
      // عرض تلميح + تعطيل خيارين خاطئين (إن أمكن)
      const hint = state.current.hint || 'فكّر في الكلمات المفتاحية.';
      els.question.textContent = state.current.q + "  — تلميح: " + hint;
      // تعطيل خيارين عشوائيين خاطئين
      const wrongIdx = state.current.a.map((_,i)=>i).filter(i=>i!==state.current.correct);
      wrongIdx.sort(()=>Math.random()-0.5);
      const toDisable = wrongIdx.slice(0,2);
      Array.from(els.answers.children).forEach((btn, i)=>{
        if(toDisable.includes(i)){ btn.disabled=true; btn.style.opacity=.6; }
      });
      renderLifelinesOnly();
    }
    function useConcede(){
      const p = state.turn===1?state.p1:state.p2;
      if(p.lifelines.concede) return;
      p.lifelines.concede = true;
      log(`اللاعب ${state.turn} استخدم: تنازل جزئي (خسارة جزء واحد).`);
      // يخسر اللاعب الحالي جزءاً صغيراً (1 من 20)، دون أن يكسب الخصم، ويتجاوز السؤال
      p.parts = Math.min(20, p.parts + 1);
      updateStatesUI();
      checkWin() || nextTurn(true);
      renderLifelinesOnly();
    }

    function renderLifelinesOnly(){
      // لتحديث حالة الزرار (used)
      const player = state.turn===1?state.p1:state.p2;
      Array.from(els.lifelines.children).forEach(ch=>{
        const label = ch.textContent;
        if(label.includes('تغيير') && player.lifelines.swap){ ch.classList.add('used'); ch.disabled=true; }
        if(label.includes('تقريب') && player.lifelines.hint){ ch.classList.add('used'); ch.disabled=true; }
        if(label.includes('تنازل') && player.lifelines.concede){ ch.classList.add('used'); ch.disabled=true; }
      });
    }

    function nextTurn(skipNewQuestion){
      setTurn(state.turn===1?2:1);
      if(!skipNewQuestion){
        state.current = randomQuestion();
      }
      renderQuestion();
      startTimer();
    }

    function checkWin(){
      // إذا وصلت أجزاء لاعب إلى 20 فهو خاسر (أي خصمه فاز)
      if(state.p1.parts>=20){
        gameOver(2);
        return true;
      }
      if(state.p2.parts>=20){
        gameOver(1);
        return true;
      }
      return false;
    }

    function gameOver(winner){
      clearInterval(state.timer);
      els.answers.innerHTML='';
      els.lifelines.innerHTML='';
      els.winner.style.display='block';
      els.winner.textContent = `انتهت الحرب! الفائز: اللاعب ${winner} 🎉`;
      log(`انتهت اللعبة بفوز اللاعب ${winner}.`);
    }

    function updateStatesUI(){
      buildSegments(els.state1, state.p2.parts, '--p2'); // ما يملأ دولة اللاعب 1 هو أجزاء اللاعب 2
      buildSegments(els.state2, state.p1.parts, '--p1');
    }

    function newGame(){
      state.turn = 1;
      state.p1 = {parts:0, lifelines:{swap:false,hint:false,concede:false}};
      state.p2 = {parts:0, lifelines:{swap:false,hint:false,concede:false}};
      state.pool = [];
      setTurn(1);
      state.current = randomQuestion();
      updateStatesUI();
      renderQuestion();
      startTimer();
      els.log.innerHTML='';
      els.winner.style.display='none';
    }

    document.getElementById('resetBtn').onclick = newGame;
    document.getElementById('startBtn').onclick = ()=>{
      state.current = randomQuestion();
      renderQuestion();
      startTimer();
    };

    // أول تحميل
    updateStatesUI();
    document.getElementById('ruleTime').textContent = state.timePerQ;

    // PWA (اختياري عند الرفع لاستضافة HTTPS)
    if('serviceWorker' in navigator){
      navigator.serviceWorker.register('service-worker.js').catch(()=>{});
    }
  </script>{
  "name": "حرب المعلومات",
  "short_name": "حرب المعلومات",
  "start_url": "./index.html",
  "display": "standalone",
  "background_color": "#0f172a",
  "theme_color": "#2a9df4",
  "icons": [
    {
      "src": "assets/icon.png",
      "sizes": "512x512",
      "type": "image/png"
    }[Uploading service-worker.js…]()self.addEventListener('install', e=>{
  e.waitUntil(caches.open('quizwar-v1').then(c=>c.addAll([
    './index.html','./manifest.json','./assets/icon.png'
  ])));
});
self.addEventListener('fetch', e=>{
  e.respondWith(caches.match(e.request).then(r=> r || fetch(e.request)));
});

  ]
}
</body>[manifest.json](https://github.com/user-attachments/files/21852016/manifest.json)

</html>
