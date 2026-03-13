# Interactive Quizzes Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Enhance all 4 static mini-quizzes in `Aarika_Maths_Dashboard.html` with randomised question banks (MCQ/TF/FIB), running score tallies, and a full-screen Practice Test mode with confetti celebration.

**Architecture:** All changes are in-place inside `Aarika_Maths_Dashboard.html`. New CSS is inserted before `</style>` (line 379). New HTML (Practice Test overlay + floating button) is inserted before `</body>` (line 1364). New JS replaces the `checkQ` function and adds quiz engine + practice test logic before `</script>` (line 1363).

**Tech Stack:** Vanilla HTML/CSS/JS, no libraries. Fredoka One + Nunito fonts (already loaded). GitHub Pages for deployment.

---

## Chunk 1: CSS for New Quiz Components

### Task 1: Add CSS for score tally, TF buttons, FIB input, Next button

**Files:**
- Modify: `Aarika_Maths_Dashboard.html` — insert CSS before `</style>` (line 379)

- [ ] **Step 1: Insert new CSS block**

Find the line `</style>` (line 379) and insert the following CSS block **immediately before it**:

```css
/* ═══ ENHANCED QUIZ ══════════════════════════════ */
.quiz-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:10px}
.quiz-tally{font-family:'Fredoka One',cursive;font-size:13px;color:var(--green);background:var(--green-l);padding:3px 10px;border-radius:20px}
.quiz-tf-opts{display:flex;gap:10px;margin-top:8px}
.quiz-tf-btn{padding:8px 28px;border-radius:30px;border:2.5px solid #ddd;background:#fff;font-weight:700;font-size:14px;cursor:pointer;transition:all .2s}
.quiz-tf-btn:hover{transform:scale(1.06)}
.quiz-tf-btn.correct{background:#2ECC71;color:#fff;border-color:#2ECC71}
.quiz-tf-btn.wrong{background:var(--red);color:#fff;border-color:var(--red)}
.quiz-fib-row{display:flex;gap:8px;align-items:center;margin-top:8px;flex-wrap:wrap}
.quiz-fib-input{padding:8px 14px;border-radius:10px;border:2px solid #ddd;font-size:14px;font-weight:700;width:140px;outline:none;transition:border-color .2s}
.quiz-fib-input:focus{border-color:var(--yellow)}
.quiz-fib-input.correct{border-color:#2ECC71;background:var(--green-l)}
.quiz-fib-input.wrong{border-color:var(--red);background:#fff0f0}
.quiz-fib-submit{padding:8px 18px;border-radius:30px;border:none;background:var(--yellow);color:#fff;font-weight:700;font-size:13px;cursor:pointer;transition:all .2s}
.quiz-fib-submit:hover{opacity:.85}
.quiz-fib-submit:disabled{opacity:.4;cursor:default}
.quiz-next-btn{margin-top:12px;padding:7px 20px;border-radius:30px;border:2px solid var(--yellow);background:#fff;color:var(--orange-d);font-weight:700;font-size:13px;cursor:pointer;display:none;transition:all .2s}
.quiz-next-btn:hover{background:var(--yellow-l)}

/* ═══ PRACTICE TEST OVERLAY ═════════════════════ */
.pt-overlay{position:fixed;inset:0;background:rgba(30,20,60,.85);z-index:2000;display:none;align-items:center;justify-content:center;backdrop-filter:blur(4px)}
.pt-overlay.active{display:flex}
.pt-card{background:#fff;border-radius:24px;width:min(94vw,560px);max-height:92vh;overflow-y:auto;padding:32px 28px;position:relative;box-shadow:0 20px 60px rgba(0,0,0,.25)}
.pt-close{position:absolute;top:14px;right:18px;font-size:22px;cursor:pointer;background:none;border:none;color:#aaa}
.pt-close:hover{color:#333}
/* Start screen */
.pt-start-icons{font-size:32px;text-align:center;margin:8px 0 18px;letter-spacing:8px}
.pt-start-title{font-family:'Fredoka One',cursive;font-size:28px;text-align:center;color:var(--purple);margin-bottom:6px}
.pt-start-sub{text-align:center;color:var(--muted);font-size:14px;margin-bottom:24px}
.pt-btn{width:100%;padding:14px;border-radius:50px;border:none;font-family:'Fredoka One',cursive;font-size:18px;cursor:pointer;transition:all .2s}
.pt-btn-go{background:var(--purple);color:#fff}
.pt-btn-go:hover{opacity:.88;transform:translateY(-2px)}
.pt-btn-next{background:var(--green);color:#fff;margin-top:14px;display:none}
.pt-btn-next:hover{opacity:.88}
.pt-btn-retry{background:var(--orange);color:#fff;margin-top:10px}
.pt-btn-retry:hover{opacity:.88}
/* Progress */
.pt-progress-wrap{margin-bottom:18px}
.pt-progress-label{font-size:12px;font-weight:700;color:var(--muted);margin-bottom:4px}
.pt-progress-bg{height:8px;background:#eee;border-radius:99px;overflow:hidden}
.pt-progress-fill{height:100%;background:var(--purple);border-radius:99px;transition:width .4s ease}
/* Subject badge */
.pt-badge{display:inline-block;padding:3px 12px;border-radius:20px;font-size:12px;font-weight:700;margin-bottom:12px}
/* Question */
.pt-question{font-family:'Fredoka One',cursive;font-size:20px;color:#2D2D44;margin-bottom:18px;line-height:1.35}
.pt-opts{display:flex;flex-direction:column;gap:10px}
.pt-opt{padding:12px 18px;border-radius:14px;border:2.5px solid #ddd;background:#fff;font-weight:700;font-size:14px;cursor:pointer;text-align:left;transition:all .2s}
.pt-opt:hover:not(:disabled){border-color:var(--purple);background:var(--purple-l)}
.pt-opt.correct{background:#2ECC71;color:#fff;border-color:#2ECC71}
.pt-opt.wrong{background:var(--red);color:#fff;border-color:var(--red)}
.pt-tf-row{display:flex;gap:12px}
.pt-tf-btn{flex:1;padding:12px;border-radius:14px;border:2.5px solid #ddd;background:#fff;font-weight:700;font-size:16px;cursor:pointer;transition:all .2s}
.pt-tf-btn:hover:not(:disabled){border-color:var(--purple);background:var(--purple-l)}
.pt-tf-btn.correct{background:#2ECC71;color:#fff;border-color:#2ECC71}
.pt-tf-btn.wrong{background:var(--red);color:#fff;border-color:var(--red)}
.pt-fib-row{display:flex;gap:8px;flex-wrap:wrap}
.pt-fib-input{flex:1;min-width:120px;padding:10px 14px;border-radius:10px;border:2px solid #ddd;font-size:15px;font-weight:700;outline:none}
.pt-fib-input:focus{border-color:var(--purple)}
.pt-fib-input.correct{border-color:#2ECC71;background:var(--green-l)}
.pt-fib-input.wrong{border-color:var(--red);background:#fff0f0}
.pt-fib-submit{padding:10px 20px;border-radius:30px;border:none;background:var(--purple);color:#fff;font-weight:700;font-size:14px;cursor:pointer}
.pt-fib-submit:disabled{opacity:.4;cursor:default}
.pt-feedback{margin-top:12px;font-weight:800;font-size:14px;min-height:20px}
/* Results */
.pt-results-score{font-family:'Fredoka One',cursive;font-size:42px;text-align:center;color:var(--purple);margin:8px 0 4px}
.pt-results-msg{font-family:'Fredoka One',cursive;font-size:20px;text-align:center;color:var(--orange);margin-bottom:16px}
.pt-review-toggle{width:100%;padding:10px;border-radius:12px;border:2px solid var(--purple);background:#fff;color:var(--purple);font-weight:700;font-size:14px;cursor:pointer;margin-top:6px}
.pt-review-list{margin-top:12px;display:none;border-radius:12px;overflow:hidden;border:1px solid #eee}
.pt-review-list.open{display:block}
.pt-review-item{padding:12px 14px;border-bottom:1px solid #eee;font-size:13px}
.pt-review-item:last-child{border-bottom:none}
.pt-review-correct{color:#2ECC71;font-weight:700}
/* Confetti */
.pt-confetti-wrap{position:absolute;inset:0;pointer-events:none;overflow:hidden;border-radius:24px}
.pt-confetto{position:absolute;width:10px;height:10px;border-radius:2px;opacity:0;animation:confettiFall 2.5s ease forwards}
@keyframes confettiFall{
  0%{transform:translateY(-20px) rotate(0deg);opacity:1}
  100%{transform:translateY(520px) rotate(720deg);opacity:0}
}
/* Floating button */
.pt-float-btn{position:fixed;bottom:24px;right:24px;z-index:1500;padding:14px 22px;border-radius:50px;background:var(--purple);color:#fff;font-family:'Fredoka One',cursive;font-size:16px;border:none;cursor:pointer;box-shadow:0 6px 24px rgba(155,89,232,.45);transition:all .25s}
.pt-float-btn:hover{transform:translateY(-3px);box-shadow:0 10px 30px rgba(155,89,232,.5)}
.pt-float-btn.hidden{display:none}
```

- [ ] **Step 2: Verify in browser**

Open `Aarika_Maths_Dashboard.html` locally. No visual change yet — but open DevTools > Console and confirm **zero CSS errors**.

- [ ] **Step 3: Commit**

```bash
cd "C:/Users/trade/OneDrive/Desktop/aarika_studies_repo"
git add Aarika_Maths_Dashboard.html
git commit -m "style: add CSS for enhanced quizzes and practice test overlay"
```

---

## Chunk 2: Question Banks + Dynamic Mini-Quizzes

### Task 2: Replace static quiz boxes with dynamic renderer

**Files:**
- Modify: `Aarika_Maths_Dashboard.html` — 4 quiz box replacements in HTML + new JS in `<script>` block

- [ ] **Step 1: Replace the Time section quiz box (line ~620)**

Find and replace this entire block:
```html
  <!-- Mini Quiz -->
  <div class="quiz-box">
    <h4>🧠 Quick Quiz – Time!</h4>
    <div class="quiz-q" id="tq-q">How many minutes are in 2 hours?</div>
    <div class="quiz-opts" id="tq-opts">
      <button class="quiz-opt" onclick="checkQ('tq',this,'wrong')">100</button>
      <button class="quiz-opt" onclick="checkQ('tq',this,'correct')">120</button>
      <button class="quiz-opt" onclick="checkQ('tq',this,'wrong')">60</button>
      <button class="quiz-opt" onclick="checkQ('tq',this,'wrong')">140</button>
    </div>
    <div class="quiz-feedback" id="tq-fb"></div>
  </div>
```

With:
```html
  <!-- Mini Quiz -->
  <div class="quiz-box" id="mini-quiz-time"></div>
```

- [ ] **Step 2: Replace the Calendar section quiz box (line ~733)**

Find and replace:
```html
  <div class="quiz-box">
    <h4>🧠 Quick Quiz – Calendar!</h4>
    <div class="quiz-q" id="cq-q">How many days are in 4 weeks?</div>
    <div class="quiz-opts" id="cq-opts">
      <button class="quiz-opt" onclick="checkQ('cq',this,'wrong')">24</button>
      <button class="quiz-opt" onclick="checkQ('cq',this,'correct')">28</button>
      <button class="quiz-opt" onclick="checkQ('cq',this,'wrong')">30</button>
      <button class="quiz-opt" onclick="checkQ('cq',this,'wrong')">32</button>
    </div>
    <div class="quiz-feedback" id="cq-fb"></div>
  </div>
```

With:
```html
  <div class="quiz-box" id="mini-quiz-calendar"></div>
```

- [ ] **Step 3: Replace the Fractions section quiz box (line ~910)**

Find and replace:
```html
  <div class="quiz-box">
    <h4>🧠 Quick Quiz – Fractions!</h4>
    <div class="quiz-q" id="fq-q">What is 3/8 + 4/8 = ?</div>
    <div class="quiz-opts" id="fq-opts">
      <button class="quiz-opt" onclick="checkQ('fq',this,'wrong')">7/16</button>
      <button class="quiz-opt" onclick="checkQ('fq',this,'correct')">7/8</button>
      <button class="quiz-opt" onclick="checkQ('fq',this,'wrong')">1/8</button>
      <button class="quiz-opt" onclick="checkQ('fq',this,'wrong')">3/4</button>
    </div>
    <div class="quiz-feedback" id="fq-fb"></div>
  </div>
```

With:
```html
  <div class="quiz-box" id="mini-quiz-fractions"></div>
```

- [ ] **Step 4: Replace the Data section quiz box (line ~1043)**

Find and replace:
```html
  <div class="quiz-box">
    <h4>🧠 Quick Quiz – Data!</h4>
    <div class="quiz-q" id="dq-q">In the Sunday activity table, which activity was done by the LEAST children?</div>
    <div class="quiz-opts" id="dq-opts">
      <button class="quiz-opt" onclick="checkQ('dq',this,'wrong')">Played games</button>
      <button class="quiz-opt" onclick="checkQ('dq',this,'wrong')">Watched TV</button>
      <button class="quiz-opt" onclick="checkQ('dq',this,'correct')">Read a story book</button>
      <button class="quiz-opt" onclick="checkQ('dq',this,'wrong')">Took a walk</button>
    </div>
    <div class="quiz-feedback" id="dq-fb"></div>
  </div>
```

With:
```html
  <div class="quiz-box" id="mini-quiz-data"></div>
```

- [ ] **Step 5: Replace old checkQ function + add QUIZ_BANKS + renderMiniQuiz**

Find the entire `// ── QUIZ ──` block (lines ~1332–1349):
```js
// ── QUIZ ──────────────────────────────────────────────────────────────
function checkQ(prefix, btn, result){
  const opts = document.querySelectorAll(`#${prefix}-opts .quiz-opt`);
  opts.forEach(o=>o.disabled=true);
  btn.classList.add(result);
  const fb = document.getElementById(`${prefix}-fb`);
  if(result==='correct'){
    fb.textContent = '🎉 Correct! Well done, Aarika!';
    fb.style.color='#2ECC71';
    // add a star
    addStar();
  } else {
    fb.textContent = '❌ Not quite! Try again after reviewing the notes.';
    fb.style.color='#E74C3C';
    // show correct
    opts.forEach(o=>{ if(o.onclick.toString().includes("'correct'")) o.classList.add('correct'); });
  }
}
```

Replace with:
```js
// ── QUIZ BANKS ────────────────────────────────────────────────────────
const QUIZ_BANKS = {
  time: [
    { type:'mcq', q:'How many minutes are in 1 hour?', opts:['30','45','60','90'], ans:'60' },
    { type:'tf',  q:'There are 24 hours in a day.', ans:'true' },
    { type:'fib', q:'60 seconds = ___ minute(s)', ans:'1', alt:['one'] },
    { type:'mcq', q:'How many hours are in 2 days?', opts:['24','36','48','72'], ans:'48' },
    { type:'tf',  q:'Half past 3 means 3:30.', ans:'true' },
    { type:'mcq', q:'What time is it: hour hand on 5, minute hand on 12?', opts:['12:05','5:00','5:12','12:00'], ans:'5:00' },
    { type:'fib', q:'Quarter past 7 = 7:___', ans:'15', alt:['fifteen'] },
    { type:'tf',  q:'30 minutes is the same as half an hour.', ans:'true' },
    { type:'mcq', q:'How many minutes in half an hour?', opts:['15','20','30','45'], ans:'30' },
    { type:'fib', q:'2 hours = ___ minutes', ans:'120', alt:['one hundred twenty','one hundred and twenty'] },
  ],
  calendar: [
    { type:'mcq', q:'How many days are in a week?', opts:['5','6','7','8'], ans:'7' },
    { type:'tf',  q:'February always has 28 days.', ans:'false' },
    { type:'fib', q:'There are ___ months in a year.', ans:'12', alt:['twelve'] },
    { type:'mcq', q:'Which month comes after July?', opts:['June','August','September','October'], ans:'August' },
    { type:'tf',  q:'A leap year has 366 days.', ans:'true' },
    { type:'mcq', q:'How many days are in 3 weeks?', opts:['14','18','21','24'], ans:'21' },
    { type:'fib', q:'The 3rd month of the year is ___', ans:'march', alt:['March'] },
    { type:'mcq', q:'How many weeks are in a year?', opts:['48','50','52','54'], ans:'52' },
    { type:'tf',  q:'Sunday comes after Saturday.', ans:'true' },
    { type:'mcq', q:'How many days are in 4 weeks?', opts:['24','28','30','32'], ans:'28' },
  ],
  fractions: [
    { type:'mcq', q:'What is 1/2 + 1/2?', opts:['1/4','1','2/4','3/4'], ans:'1' },
    { type:'tf',  q:'3/4 is greater than 1/2.', ans:'true' },
    { type:'fib', q:'1/4 + 2/4 = ___/4', ans:'3', alt:['three'] },
    { type:'mcq', q:'Which fraction is the smallest?', opts:['3/4','1/2','1/4','2/4'], ans:'1/4' },
    { type:'tf',  q:'2/4 is the same as 1/2.', ans:'true' },
    { type:'mcq', q:'What is 7/8 − 3/8?', opts:['4/8','3/8','1/2','5/8'], ans:'4/8' },
    { type:'fib', q:'Half of 10 is ___', ans:'5', alt:['five'] },
    { type:'mcq', q:'3/8 + 4/8 = ?', opts:['7/16','1/8','7/8','3/4'], ans:'7/8' },
    { type:'tf',  q:'1/3 is greater than 1/4.', ans:'true' },
    { type:'mcq', q:'How many quarters make a whole?', opts:['2','3','4','6'], ans:'4' },
  ],
  data: [
    { type:'mcq', q:'In a tally, IIII (5 marks) = ?', opts:['4','5','6','10'], ans:'5' },
    { type:'tf',  q:'A pictograph uses pictures to show data.', ans:'true' },
    { type:'fib', q:'A bar graph uses ___ to show data.', ans:'bars', alt:['bar','rectangles'] },
    { type:'mcq', q:'In the Sunday activity table, which had 12 children?', opts:['Read book','Watched TV','Played games','Took a walk'], ans:'Played games' },
    { type:'tf',  q:'The player who scored 80 runs was Sehwag.', ans:'true' },
    { type:'mcq', q:'Which player scored the LEAST runs in the pictograph?', opts:['Sachin','Sehwag','Yuvraj','Zaheer'], ans:'Zaheer' },
    { type:'fib', q:'Each bat symbol in the pictograph = ___ runs', ans:'10', alt:['ten'] },
    { type:'mcq', q:'How many children read a story book on Sunday?', opts:['5','7','8','12'], ans:'7' },
    { type:'tf',  q:'Sachin scored more runs than Yuvraj.', ans:'true' },
    { type:'mcq', q:'What is the total of Sachin + Zaheer runs?', opts:['100','110','120','130'], ans:'110' },
  ],
};

// Badge styles per section
const SECTION_STYLE = {
  time:      { bg:'var(--teal-l)',    color:'var(--teal-d)',    label:'⏰ Time' },
  calendar:  { bg:'var(--purple-l)',  color:'var(--purple-d)',  label:'📅 Calendar' },
  fractions: { bg:'var(--orange-l)',  color:'var(--orange-d)',  label:'🍕 Fractions' },
  data:      { bg:'var(--green-l)',   color:'var(--green)',     label:'📊 Data' },
};

// ── MINI QUIZ RENDERER ────────────────────────────────────────────────
function renderMiniQuiz(containerId, bankKey) {
  const container = document.getElementById(containerId);
  if (!container) return;
  const bank = QUIZ_BANKS[bankKey];
  const style = SECTION_STYLE[bankKey];
  let shuffled = shuffle([...bank]);
  let idx = 0, correct = 0, total = 0;

  function render() {
    if (idx >= shuffled.length) {
      // bank exhausted — reshuffle and reset
      shuffled = shuffle([...bank]);
      idx = 0; correct = 0; total = 0;
    }
    const q = shuffled[idx];
    container.innerHTML = buildQuizHTML(q, style, bankKey, correct, total);
    attachQuizEvents(container, q, style, bankKey);
  }

  function buildQuizHTML(q, style, bankKey, c, t) {
    const badge = `<span class="pt-badge" style="background:${style.bg};color:${style.color}">${style.label}</span>`;
    const tally = `<span class="quiz-tally">${c} / ${t} ✅</span>`;
    let answerHTML = '';
    if (q.type === 'mcq') {
      const opts = shuffle([...q.opts]);
      answerHTML = `<div class="quiz-opts">${opts.map(o=>`<button class="quiz-opt" data-val="${o}">${o}</button>`).join('')}</div>`;
    } else if (q.type === 'tf') {
      answerHTML = `<div class="quiz-tf-opts"><button class="quiz-tf-btn" data-val="true">✅ True</button><button class="quiz-tf-btn" data-val="false">❌ False</button></div>`;
    } else {
      answerHTML = `<div class="quiz-fib-row"><input class="quiz-fib-input" type="text" placeholder="Type your answer…" /><button class="quiz-fib-submit">Submit</button></div>`;
    }
    return `
      <div class="quiz-header">${badge}${tally}</div>
      <div class="quiz-q">${q.q}</div>
      ${answerHTML}
      <div class="quiz-feedback"></div>
      <button class="quiz-next-btn">Next ▶</button>`;
  }

  function attachQuizEvents(container, q, style, bankKey) {
    const fb = container.querySelector('.quiz-feedback');
    const nextBtn = container.querySelector('.quiz-next-btn');

    function onCorrect(el, cls) {
      el.classList.add('correct'); el.disabled = true;
      fb.textContent = '🎉 Correct! Well done, Aarika!';
      fb.style.color = '#2ECC71';
      correct++; total++;
      updateTally();
      addStar();
      nextBtn.style.display = 'inline-block';
    }

    function onWrong(el, cls, correctVal) {
      el.classList.add('wrong'); el.disabled = true;
      fb.textContent = `❌ Not quite! Answer: ${correctVal}`;
      fb.style.color = 'var(--red)';
      total++;
      updateTally();
      // highlight correct
      if (q.type === 'mcq') {
        container.querySelectorAll('.quiz-opt').forEach(b => { if (b.dataset.val === q.ans) b.classList.add('correct'); b.disabled = true; });
      } else if (q.type === 'tf') {
        container.querySelectorAll('.quiz-tf-btn').forEach(b => { if (b.dataset.val === q.ans) b.classList.add('correct'); b.disabled = true; });
      }
      nextBtn.style.display = 'inline-block';
    }

    function updateTally() {
      container.querySelector('.quiz-tally').textContent = `${correct} / ${total} ✅`;
    }

    // MCQ
    container.querySelectorAll('.quiz-opt').forEach(btn => {
      btn.addEventListener('click', () => {
        if (btn.dataset.val === q.ans) onCorrect(btn);
        else onWrong(btn, 'wrong', q.ans);
        container.querySelectorAll('.quiz-opt').forEach(b => b.disabled = true);
      });
    });

    // TF
    container.querySelectorAll('.quiz-tf-btn').forEach(btn => {
      btn.addEventListener('click', () => {
        if (btn.dataset.val === q.ans) onCorrect(btn);
        else onWrong(btn, 'wrong', q.ans === 'true' ? 'True' : 'False');
        container.querySelectorAll('.quiz-tf-btn').forEach(b => b.disabled = true);
      });
    });

    // FIB
    const fibInput = container.querySelector('.quiz-fib-input');
    const fibSubmit = container.querySelector('.quiz-fib-submit');
    if (fibInput && fibSubmit) {
      function checkFib() {
        if (!fibInput.value.trim()) return;
        const val = fibInput.value.trim().toLowerCase();
        const correct_ans = q.ans.toLowerCase();
        const alts = (q.alt || []).map(a => a.toLowerCase());
        if (val === correct_ans || alts.includes(val)) {
          fibInput.classList.add('correct');
          fibSubmit.disabled = true;
          fibInput.disabled = true;
          onCorrect(fibInput);
        } else {
          fibInput.classList.add('wrong');
          fibSubmit.disabled = true;
          fibInput.disabled = true;
          onWrong(fibInput, 'wrong', q.ans);
        }
      }
      fibSubmit.addEventListener('click', checkFib);
      fibInput.addEventListener('keydown', e => { if (e.key === 'Enter') checkFib(); });
      // disable submit when empty
      fibInput.addEventListener('input', () => { fibSubmit.disabled = !fibInput.value.trim(); });
      fibSubmit.disabled = true;
    }

    // Next
    nextBtn.addEventListener('click', () => { idx++; render(); });
  }

  render(); // kick off
}

// shuffle helper (Fisher-Yates)
function shuffle(arr) {
  for (let i = arr.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}
```

- [ ] **Step 6: Update DOMContentLoaded to init mini-quizzes**

Find the `window.addEventListener('DOMContentLoaded', ...)` block:
```js
window.addEventListener('DOMContentLoaded', ()=>{
  // init home
  setTimeout(animateProgressBars, 400);
  calcMins();
});
```

Replace with:
```js
window.addEventListener('DOMContentLoaded', ()=>{
  // init home
  setTimeout(animateProgressBars, 400);
  calcMins();
  // init mini quizzes
  renderMiniQuiz('mini-quiz-time',      'time');
  renderMiniQuiz('mini-quiz-calendar',  'calendar');
  renderMiniQuiz('mini-quiz-fractions', 'fractions');
  renderMiniQuiz('mini-quiz-data',      'data');
});
```

- [ ] **Step 7: Verify in browser**

Open the file locally. Navigate to each section (Time, Calendar, Fractions, Data). Check:
- ✅ Quiz box renders with a question and badge
- ✅ Score tally shows `0 / 0 ✅`
- ✅ MCQ, TF, and FIB types all appear across the 4 sections
- ✅ Clicking correct answer turns green, tally updates
- ✅ Clicking wrong turns red, correct is highlighted
- ✅ "Next ▶" button appears after answering, loads new question

- [ ] **Step 8: Commit**

```bash
cd "C:/Users/trade/OneDrive/Desktop/aarika_studies_repo"
git add Aarika_Maths_Dashboard.html
git commit -m "feat: replace static quizzes with dynamic question banks (MCQ/TF/FIB)"
```

---

## Chunk 3: Practice Test Overlay

### Task 3: Add Practice Test HTML, JS, and floating button

**Files:**
- Modify: `Aarika_Maths_Dashboard.html` — add overlay HTML before `</body>` and PT JS before `</script>`

- [ ] **Step 1: Add overlay HTML + floating button before `</body>`**

Find `</body>` and insert immediately before it:
```html
<!-- ═══ PRACTICE TEST OVERLAY ══════════════════════════════════════════ -->
<button class="pt-float-btn" id="ptFloatBtn" onclick="ptOpen()">📝 Practice Test</button>

<div class="pt-overlay" id="ptOverlay">
  <div class="pt-card" id="ptCard">
    <button class="pt-close" onclick="ptClose()">✕</button>
    <div class="pt-confetti-wrap" id="ptConfettiWrap"></div>
    <div id="ptContent"></div>
  </div>
</div>
```

- [ ] **Step 2: Add Practice Test JS before `</script>`**

Find `</script>` and insert immediately before it:
```js
// ── PRACTICE TEST ────────────────────────────────────────────────────
(function(){
  const MESSAGES = [
    { min:18, msg:'🌟 Superstar Aarika!' },
    { min:14, msg:'🎉 Brilliant effort!' },
    { min:10, msg:'😊 Good try, keep going!' },
    { min:0,  msg:'💪 Practice makes perfect!' },
  ];

  let ptQuestions = [], ptIdx = 0, ptScore = 0, ptWrong = [];

  function ptSample(arr, n) {
    return shuffle([...arr]).slice(0, n);
  }

  function ptBuildPool() {
    const pool = [];
    Object.keys(QUIZ_BANKS).forEach(k => {
      ptSample(QUIZ_BANKS[k], 5).forEach(q => pool.push({ ...q, _section: k }));
    });
    return shuffle(pool);
  }

  window.ptOpen = function() {
    document.getElementById('ptOverlay').classList.add('active');
    document.getElementById('ptFloatBtn').classList.add('hidden');
    ptShowStart();
  };

  window.ptClose = function() {
    document.getElementById('ptOverlay').classList.remove('active');
    document.getElementById('ptFloatBtn').classList.remove('hidden');
    clearConfetti();
  };

  function ptShowStart() {
    document.getElementById('ptContent').innerHTML = `
      <div class="pt-start-title">📚 Practice Test</div>
      <div class="pt-start-icons">⏰ 📅 🍕 📊</div>
      <div class="pt-start-sub">20 questions · All subjects · No timer</div>
      <button class="pt-btn pt-btn-go" onclick="ptBegin()">Ready? Let's go! 🚀</button>`;
  }

  window.ptBegin = function() {
    ptQuestions = ptBuildPool();
    ptIdx = 0; ptScore = 0; ptWrong = [];
    ptShowQuestion();
  };

  function ptShowQuestion() {
    const q = ptQuestions[ptIdx];
    const style = SECTION_STYLE[q._section];
    const num = ptIdx + 1;
    const pct = Math.round((num / ptQuestions.length) * 100);
    let ansHTML = '';
    if (q.type === 'mcq') {
      const opts = shuffle([...q.opts]);
      ansHTML = `<div class="pt-opts">${opts.map(o=>`<button class="pt-opt" data-val="${o}">${o}</button>`).join('')}</div>`;
    } else if (q.type === 'tf') {
      ansHTML = `<div class="pt-tf-row"><button class="pt-tf-btn" data-val="true">✅ True</button><button class="pt-tf-btn" data-val="false">❌ False</button></div>`;
    } else {
      ansHTML = `<div class="pt-fib-row"><input class="pt-fib-input" type="text" placeholder="Type your answer…"/><button class="pt-fib-submit" disabled>Submit</button></div>`;
    }
    document.getElementById('ptContent').innerHTML = `
      <div class="pt-progress-wrap">
        <div class="pt-progress-label">Question ${num} of ${ptQuestions.length}</div>
        <div class="pt-progress-bg"><div class="pt-progress-fill" style="width:${pct}%"></div></div>
      </div>
      <span class="pt-badge" style="background:${style.bg};color:${style.color}">${style.label}</span>
      <div class="pt-question">${q.q}</div>
      ${ansHTML}
      <div class="pt-feedback" id="ptFb"></div>
      <button class="pt-btn pt-btn-next" id="ptNextBtn" onclick="ptNext()">${ptIdx < ptQuestions.length - 1 ? 'Next →' : 'See Results 🎉'}</button>`;
    ptAttachEvents(q);
  }

  function ptAttachEvents(q) {
    const fb = document.getElementById('ptFb');
    const nextBtn = document.getElementById('ptNextBtn');

    function onRight(el) {
      el.classList.add('correct'); el.disabled = true;
      fb.textContent = '🎉 Correct!'; fb.style.color = '#2ECC71';
      ptScore++;
      disableAll();
      nextBtn.style.display = 'block';
      addStar();
    }
    function onWrong(el, correctVal) {
      el.classList.add('wrong'); el.disabled = true;
      fb.textContent = `❌ Answer: ${correctVal}`; fb.style.color = 'var(--red)';
      ptWrong.push({ q: q.q, correct: q.ans, section: q._section });
      highlightCorrect(q, correctVal);
      disableAll();
      nextBtn.style.display = 'block';
    }
    function disableAll() {
      document.querySelectorAll('.pt-opt,.pt-tf-btn').forEach(b => b.disabled = true);
    }
    function highlightCorrect(q, correctVal) {
      if (q.type === 'mcq') document.querySelectorAll('.pt-opt').forEach(b => { if (b.dataset.val === q.ans) b.classList.add('correct'); });
      if (q.type === 'tf')  document.querySelectorAll('.pt-tf-btn').forEach(b => { if (b.dataset.val === q.ans) b.classList.add('correct'); });
    }

    document.querySelectorAll('.pt-opt').forEach(b => {
      b.addEventListener('click', () => b.dataset.val === q.ans ? onRight(b) : onWrong(b, q.ans));
    });
    document.querySelectorAll('.pt-tf-btn').forEach(b => {
      b.addEventListener('click', () => b.dataset.val === q.ans ? onRight(b) : onWrong(b, q.ans === 'true' ? 'True' : 'False'));
    });
    const fibInput = document.querySelector('.pt-fib-input');
    const fibSubmit = document.querySelector('.pt-fib-submit');
    if (fibInput && fibSubmit) {
      function checkFib() {
        if (!fibInput.value.trim()) return;
        const val = fibInput.value.trim().toLowerCase();
        const alts = (q.alt || []).map(a => a.toLowerCase());
        fibInput.disabled = true; fibSubmit.disabled = true;
        if (val === q.ans.toLowerCase() || alts.includes(val)) onRight(fibInput);
        else onWrong(fibInput, q.ans);
      }
      fibSubmit.addEventListener('click', checkFib);
      fibInput.addEventListener('keydown', e => { if (e.key === 'Enter') checkFib(); });
      fibInput.addEventListener('input', () => { fibSubmit.disabled = !fibInput.value.trim(); });
    }
  }

  window.ptNext = function() {
    ptIdx++;
    if (ptIdx >= ptQuestions.length) ptShowResults();
    else ptShowQuestion();
  };

  function ptShowResults() {
    launchConfetti();
    const msg = MESSAGES.find(m => ptScore >= m.min).msg;
    const reviewHTML = ptWrong.length === 0 ? '<p style="color:var(--green);font-weight:700;text-align:center">🎯 Perfect score! No mistakes!</p>' :
      `<button class="pt-review-toggle" onclick="this.nextElementSibling.classList.toggle('open')">Review Wrong Answers ▼ (${ptWrong.length})</button>
       <div class="pt-review-list">
         ${ptWrong.map((w,i)=>`<div class="pt-review-item"><b>Q${i+1}:</b> ${w.q}<br>✅ <span class="pt-review-correct">${w.correct}</span></div>`).join('')}
       </div>`;
    document.getElementById('ptContent').innerHTML = `
      <div class="pt-results-score">${ptScore} / ${ptQuestions.length} 🌟</div>
      <div class="pt-results-msg">${msg}</div>
      ${reviewHTML}
      <button class="pt-btn pt-btn-retry" onclick="ptBegin()">Try Again 🔁</button>`;
  }

  function launchConfetti() {
    const wrap = document.getElementById('ptConfettiWrap');
    clearConfetti();
    const colours = ['#9B59E8','#FF7A2F','#2ECC71','#F1C40F','#E74C3C','#3498DB'];
    for (let i = 0; i < 60; i++) {
      const el = document.createElement('div');
      el.className = 'pt-confetto';
      el.style.cssText = `
        left:${Math.random()*100}%;
        background:${colours[Math.floor(Math.random()*colours.length)]};
        animation-delay:${Math.random()*1.2}s;
        animation-duration:${1.8 + Math.random()}s;
        width:${6+Math.random()*8}px;
        height:${6+Math.random()*8}px;
        border-radius:${Math.random()>0.5?'50%':'2px'};
      `;
      wrap.appendChild(el);
    }
    setTimeout(clearConfetti, 4000);
  }

  function clearConfetti() {
    const wrap = document.getElementById('ptConfettiWrap');
    if (wrap) wrap.innerHTML = '';
  }
})();
```

- [ ] **Step 3: Verify in browser**

Open the file locally. Check:
- ✅ Purple "📝 Practice Test" button visible bottom-right
- ✅ Clicking it opens full-screen overlay with Start screen
- ✅ "Let's go" begins test with progress bar
- ✅ All 3 question types render and respond correctly
- ✅ Score tracked correctly
- ✅ Results screen shows score + message + confetti
- ✅ "Review Wrong Answers" accordion opens/closes
- ✅ "Try Again" restarts the test
- ✅ Float button hidden during test, reappears after close/finish

- [ ] **Step 4: Commit**

```bash
cd "C:/Users/trade/OneDrive/Desktop/aarika_studies_repo"
git add Aarika_Maths_Dashboard.html
git commit -m "feat: add full-screen practice test with confetti celebration"
```

---

## Chunk 4: Final Push to GitHub Pages

- [ ] **Step 1: Push to GitHub**

```bash
cd "C:/Users/trade/OneDrive/Desktop/aarika_studies_repo"
git push
```

- [ ] **Step 2: Verify GitHub Pages**

Wait ~60 seconds, then visit:
`https://abhijeet2021.github.io/aarika_studies/Aarika_Maths_Dashboard.html`

Confirm the live site shows:
- ✅ Dynamic mini-quizzes in all 4 sections
- ✅ Practice Test float button visible
- ✅ Confetti works on results

- [ ] **Step 3: Commit plan + spec docs**

```bash
cd "C:/Users/trade/OneDrive/Desktop/aarika_studies_repo"
git add docs/
git commit -m "docs: add quiz enhancement spec and implementation plan"
git push
```
