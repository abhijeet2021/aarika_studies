# Interactive Quizzes Design — Aarika's Maths Dashboard
**Date:** 2026-03-13
**File:** `Aarika_Maths_Dashboard.html` (enhance in-place)
**Target:** Maharashtra SSC Board, Std 3 Sem 2

---

## Overview
Enhance the existing 4 static mini-quizzes with randomised question banks, new question types, a running score tracker per section, and a full-screen Practice Test mode with confetti celebration on completion.

---

## Section 1 — Mini-Quiz Upgrades

### Question Banks
Each section gets 10 questions in a JS array. Questions are drawn without replacement (shuffled on load). When the bank is exhausted, it silently reshuffles and resets — the score tally also resets at that point. Practice Test always draws from a fresh shuffled copy independent of the mini-quiz state.

### Question Types
| Type | Description |
|---|---|
| MCQ | 4 options, one correct |
| True/False | 2 buttons: True / False |
| Fill-in-blank (FIB) | Text input + Submit; answer trimmed + lowercased; `alt` array of accepted alternates supported |

FIB matching rules: trim whitespace, lowercase both sides, check against `ans` and all entries in optional `alt:[]` array.

### Per-Section Quiz Box
- Dynamically rendered from bank (replaces all 4 hardcoded quiz boxes)
- **"Next ▶" button** appears after answering; loads next question from shuffled bank
- **Score tally** top-right: `3 / 5 ✅` — resets when bank reshuffles
- Correct/wrong colour feedback retained; feedback stays visible until Next is clicked (no auto-advance in mini-quiz)

### Subject Badge Colours (reuse existing CSS vars)
| Section | Background | Text |
|---|---|---|
| Time ⏰ | `var(--teal-l)` | `var(--teal-d)` |
| Calendar 📅 | `var(--purple-l)` | `var(--purple-d)` |
| Fractions 🍕 | `var(--orange-l)` | `var(--orange-d)` |
| Data 📊 | `var(--green-l)` | `var(--green-d)` |

---

## Section 2 — Practice Test Mode

### Trigger
- Sticky **"📝 Practice Test"** button, bottom-right corner, always visible
- Button is **hidden** while a test is in progress (prevents re-entry)

### Question Pool
- 5 questions drawn from a **fresh independent shuffle** of each bank = 20 total
- Shuffled before display; completely independent of mini-quiz state

### Screen Flow
```
[Start] → [Q1..Q20, one at a time] → [Results]
```

#### Start Screen
Subject icons ⏰📅🍕📊 + "Ready? Let's go! 🚀" button

#### Question Screen
- Progress bar: `Question X of 20` (animated fill)
- Subject badge (colour per table above)
- Question text (large, Fredoka One font)
- Answer UI rendered by type:
  - MCQ: 4 buttons
  - TF: True / False buttons
  - FIB: text input + Submit button
- On answer: instant ✅/❌ feedback shown
- **"Next →" button appears after feedback** (no auto-advance; child reads at own pace)

#### Results Screen
- 🎉 CSS confetti burst (pure CSS keyframe animation, no library)
- `You got X / 20 🌟`
- Encouraging message:
  - 18–20 → "🌟 Superstar Aarika!"
  - 14–17 → "🎉 Brilliant effort!"
  - 10–13 → "😊 Good try, keep going!"
  - <10 → "💪 Practice makes perfect!"
- **"Review Wrong Answers ▼"** toggle — inline accordion below score, shows each missed question with the correct answer highlighted green
- **"Try Again 🔁"** — fresh shuffle, restarts from Start screen; floating button reappears

---

## Full Question Banks

### Time (10 questions)
```js
{ type:'mcq', q:'How many minutes are in 1 hour?', opts:['30','45','60','90'], ans:'60' },
{ type:'tf',  q:'There are 24 hours in a day.', ans:'true' },
{ type:'fib', q:'60 seconds = ___ minute(s)', ans:'1', alt:['one'] },
{ type:'mcq', q:'How many hours are in 2 days?', opts:['24','36','48','72'], ans:'48' },
{ type:'tf',  q:'Half past 3 means 3:30.', ans:'true' },
{ type:'mcq', q:'What time is shown: hour hand on 5, minute hand on 12?', opts:['12:05','5:00','5:12','12:00'], ans:'5:00' },
{ type:'fib', q:'Quarter past 7 = 7:___', ans:'15', alt:['fifteen'] },
{ type:'tf',  q:'30 minutes is the same as half an hour.', ans:'true' },
{ type:'mcq', q:'How many minutes in half an hour?', opts:['15','20','30','45'], ans:'30' },
{ type:'fib', q:'2 hours = ___ minutes', ans:'120', alt:['one hundred twenty','one hundred and twenty'] },
```

### Calendar (10 questions)
```js
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
```

### Fractions (10 questions)
```js
{ type:'mcq', q:'What is 1/2 + 1/2?', opts:['1/4','1','2/4','3/4'], ans:'1' },
{ type:'tf',  q:'3/4 is greater than 1/2.', ans:'true' },
{ type:'fib', q:'1/4 + 2/4 = ___/4', ans:'3', alt:['three'] },
{ type:'mcq', q:'Which fraction is smallest?', opts:['3/4','1/2','1/4','2/4'], ans:'1/4' },
{ type:'tf',  q:'2/4 is the same as 1/2.', ans:'true' },
{ type:'mcq', q:'What is 7/8 - 3/8?', opts:['4/8','3/8','1/2','5/8'], ans:'4/8' },
{ type:'fib', q:'Half of 10 is ___', ans:'5', alt:['five'] },
{ type:'mcq', q:'3/8 + 4/8 = ?', opts:['7/16','1/8','7/8','3/4'], ans:'7/8' },
{ type:'tf',  q:'1/3 is greater than 1/4.', ans:'true' },
{ type:'mcq', q:'How many quarters make a whole?', opts:['2','3','4','6'], ans:'4' },
```

### Data (10 questions)
```js
{ type:'mcq', q:'In a tally, IIII (4 marks with a cross) = ?', opts:['4','5','6','10'], ans:'5' },
{ type:'tf',  q:'A pictograph uses pictures to show data.', ans:'true' },
{ type:'fib', q:'A bar graph uses ___ to show data.', ans:'bars', alt:['bar','rectangles'] },
{ type:'mcq', q:'In the Sunday activity table, which had 12 children?', opts:['Read book','Watched TV','Played games','Took a walk'], ans:'Played games' },
{ type:'tf',  q:'The child who scored 80 runs was Sehwag.', ans:'true' },
{ type:'mcq', q:'Which player scored the LEAST runs in the pictograph?', opts:['Sachin','Sehwag','Yuvraj','Zaheer'], ans:'Zaheer' },
{ type:'fib', q:'Each 🏏 symbol in the pictograph = ___ runs', ans:'10', alt:['ten'] },
{ type:'mcq', q:'How many children read a story book on Sunday?', opts:['5','7','8','12'], ans:'7' },
{ type:'tf',  q:'Sachin scored more runs than Yuvraj.', ans:'true' },
{ type:'mcq', q:'What is the total of Sachin + Zaheer runs?', opts:['100','110','120','130'], ans:'110' },
```

---

## Implementation Scope
- All changes inside `Aarika_Maths_Dashboard.html` — no new files
- Add `QUIZ_BANKS` JS object with all 40 questions
- Add `renderMiniQuiz(sectionId, bankKey)` helper to replace 4 static quiz boxes
- Add Practice Test overlay HTML + CSS + JS at bottom of file
- Floating button hidden during active test, restored on finish/close
- Push updated file to GitHub (existing repo/Pages setup)

---

## Out of Scope
- Timer / exam mode
- localStorage persistence
- Audio/sound effects
- Deployment configuration (already set up)
