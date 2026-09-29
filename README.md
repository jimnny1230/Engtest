<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>영어 본문 암기 프로그램</title>
<style>
:root{
  --bg:#faf7f2; --card:#ffffff; --ink:#2b2620; --sub:#7a7266;
  --accent:#c0562c; --accent2:#3d6b52; --line:#e6ddd0; --ok:#3d6b52; --bad:#c0562c; --chip:#f1e9db;
}
@media (prefers-color-scheme: dark){
  :root:not([data-theme="light"]){
    --bg:#1c1a17; --card:#26221d; --ink:#f1ece2; --sub:#a89e8d;
    --accent:#e2895a; --accent2:#7fbf9d; --line:#3a352c; --ok:#7fbf9d; --bad:#e2895a; --chip:#332d24;
  }
}
:root[data-theme="dark"]{
  --bg:#1c1a17; --card:#26221d; --ink:#f1ece2; --sub:#a89e8d;
  --accent:#e2895a; --accent2:#7fbf9d; --line:#3a352c; --ok:#7fbf9d; --bad:#e2895a; --chip:#332d24;
}
*{box-sizing:border-box;}
html{scroll-padding-top:env(safe-area-inset-top,0px);}
body{
  margin:0; background:var(--bg); color:var(--ink);
  font-family:'Georgia','Noto Serif KR',serif;
  padding-top:env(safe-area-inset-top,0px); padding-bottom:env(safe-area-inset-bottom,0px);
  min-height:100%;
}
.wrap{max-width:720px;margin:0 auto;padding:24px 18px 60px;}
h1{font-size:1.35rem;margin:4px 0 2px;letter-spacing:-0.3px;}
.tagline{color:var(--sub);font-size:0.85rem;margin-bottom:18px;}
.tabs{display:flex;gap:8px;margin-bottom:14px;flex-wrap:wrap;}
.tab-btn{
  border:1px solid var(--line);background:var(--card);color:var(--ink);
  padding:8px 14px;border-radius:20px;font-size:0.85rem;cursor:pointer;font-family:inherit;
}
.tab-btn.active{background:var(--accent);color:#fff;border-color:var(--accent);}
.panel{
  background:var(--card);border:1px solid var(--line);border-radius:14px;
  padding:20px;margin-bottom:16px;
}
.row{display:flex;gap:10px;flex-wrap:wrap;align-items:center;margin-bottom:12px;}
select,button{
  font-family:inherit;font-size:0.85rem;border-radius:10px;border:1px solid var(--line);
  background:var(--bg);color:var(--ink);padding:8px 12px;cursor:pointer;
}
button.primary{background:var(--accent);color:#fff;border-color:var(--accent);}
button.ghost{background:transparent;}
.progress{color:var(--sub);font-size:0.8rem;}
.card{
  min-height:100px;display:flex;flex-direction:column;justify-content:center;
  gap:10px;padding:14px 4px;
}
.sent{font-size:1.15rem;line-height:1.6;}
.blank{color:var(--sub);letter-spacing:1px;}
.ko{color:var(--accent2);font-size:0.92rem;margin-top:4px;line-height:1.5;}
textarea, input.typebox{
  width:100%;font-family:inherit;font-size:1rem;border:1px solid var(--line);
  border-radius:10px;padding:10px;background:var(--bg);color:var(--ink);resize:vertical;
}
.result{margin-top:10px;font-size:0.95rem;line-height:1.7;word-break:break-word;}
.ok{color:var(--ok);font-weight:bold;}
.bad{color:var(--bad);text-decoration:line-through;}
.miss{color:var(--bad);text-decoration:line-through;opacity:0.75;}
.extra{color:var(--bad);background:rgba(192,86,44,0.15);border-radius:4px;padding:0 3px;}
.legend{font-size:0.75rem;color:var(--sub);margin-top:6px;}
.miss{color:var(--sub);font-style:italic;border-bottom:1px dashed var(--bad);}
.nav{display:flex;justify-content:space-between;margin-top:14px;}
.small{font-size:0.78rem;color:var(--sub);}
.listitem{padding:10px 0;border-bottom:1px solid var(--line);font-size:0.95rem;line-height:1.55;}
.listitem:last-child{border-bottom:none;}
.listitem .ko{margin-top:4px;}
.num{color:var(--accent);font-weight:bold;margin-right:6px;}
.wordbank{display:flex;flex-wrap:wrap;gap:8px;min-height:44px;padding:8px 0;border-bottom:1px dashed var(--line);margin-bottom:10px;}
.built{display:flex;flex-wrap:wrap;gap:8px;min-height:52px;padding:10px;background:var(--chip);border-radius:10px;margin-bottom:10px;}
.wchip{
  background:var(--chip);border:1px solid var(--line);border-radius:8px;
  padding:6px 10px;font-size:0.95rem;cursor:pointer;user-select:none;
}
.wchip.used{opacity:0.35;pointer-events:none;}
.built .wchip{background:var(--card);border-color:var(--accent);cursor:pointer;}
.hidden{display:none;}
.scorecards{display:flex;gap:10px;flex-wrap:wrap;margin-bottom:6px;}
.scorecard{flex:1;min-width:130px;background:var(--chip);border-radius:10px;padding:14px;text-align:center;}
.scorecard .big{font-size:1.6rem;font-weight:bold;color:var(--accent);}
.scorecard .lbl{font-size:0.78rem;color:var(--sub);margin-top:2px;}
.weakitem{padding:8px 0;border-bottom:1px solid var(--line);font-size:0.9rem;line-height:1.5;}
.weakitem:last-child{border-bottom:none;}
.weakitem .pct{color:var(--bad);font-weight:bold;margin-left:6px;}
.built.col,.wordbank.col{flex-direction:column;align-items:stretch;}
.schip{display:flex;justify-content:space-between;gap:8px;background:var(--chip);border:1px solid var(--line);border-radius:8px;padding:8px 10px;font-size:0.9rem;line-height:1.5;cursor:pointer;user-select:none;}
.built .schip{background:var(--card);border-color:var(--accent);}
.schip.used{opacity:.35;pointer-events:none;}
.wchip.sel,.schip.sel{outline:2px solid var(--accent2);}
.wchip.cok,.schip.cok{border-color:var(--ok);border-width:2px;}
.wchip.cbad,.schip.cbad{border-color:var(--bad);border-width:2px;}
.xbtn{color:var(--sub);margin-left:6px;cursor:pointer;font-weight:normal;}
input.bl{font-family:inherit;font-size:1rem;border:none;border-bottom:2px solid var(--accent);background:transparent;color:var(--ink);text-align:center;padding:0 2px;min-width:3ch;outline:none;}
input.bl.ok{color:var(--ok);border-color:var(--ok);}
input.bl.bad{color:var(--bad);border-color:var(--bad);}
.trow{padding:10px;border:1px solid var(--line);border-radius:8px;margin-bottom:8px;cursor:pointer;line-height:1.55;font-size:0.95rem;}
.trow.right{border-color:var(--ok);background:rgba(61,107,82,0.14);}
.trow.wrong{border-color:var(--bad);background:rgba(192,86,44,0.14);}
.ans{padding-left:20px;font-size:0.9rem;line-height:1.6;}
.kchip{background:var(--chip);border:1px solid var(--line);border-radius:8px;padding:8px 12px;font-size:1rem;cursor:pointer;user-select:none;}
.built .kchip{background:var(--card);border-color:var(--accent);}
.kchip.used{opacity:.35;pointer-events:none;}
.kchip.sel{outline:2px solid var(--accent2);}
.kchip.cok{border-color:var(--ok);border-width:2px;}
.kchip.cbad{border-color:var(--bad);border-width:2px;}
</style>
</head>
<body>
<div class="wrap">
  <h1>📘 영어 본문 암기 프로그램</h1>
  <div class="tagline">교과서 Lesson 1·2 · 모의고사 · 올림포스</div>

  <div class="tabs" id="lessonTabs"></div>

  <div class="panel">
    <div class="row">
      <span class="small">모드</span>
      <button class="tab-btn mode-btn active" data-mode="card">🗂 카드 암기</button>
      <button class="tab-btn mode-btn" data-mode="order">🧩 단어 순서</button>
      <button class="tab-btn mode-btn" data-mode="chunk">🧱 덩어리 순서</button>
      <button class="tab-btn mode-btn" data-mode="blank">✏️ 빈칸 채우기</button>
      <button class="tab-btn mode-btn" data-mode="sorder">🔀 문장 순서</button>
      <button class="tab-btn mode-btn" data-mode="topic">🎯 주제문 찾기</button>
      <button class="tab-btn mode-btn" data-mode="type">⌨️ 타이핑 연습</button>
      <button class="tab-btn mode-btn" data-mode="list">📋 전체 보기</button>
      <button class="tab-btn mode-btn" data-mode="score">📊 채점 결과</button>
    </div>
  </div>

  <div class="panel" id="cardPanel">
    <div class="row" style="justify-content:space-between;">
      <span class="progress" id="cardProgress"></span>
      <button class="ghost" id="shuffleBtn">🔀 순서 섞기</button>
    </div>
    <div class="card">
      <div class="sent blank" id="cardSent">가려짐 — 버튼을 눌러 확인하세요</div>
      <div class="ko" id="cardKo"></div>
    </div>
    <div class="row">
      <button class="primary" id="revealBtn">정답 보기</button>
      <button id="hintBtn">힌트 (첫 글자)</button>
      <button id="koBtn">해석 보기/숨기기</button>
    </div>
    <div class="nav">
      <button id="prevBtn">◀ 이전</button>
      <button id="nextBtn">다음 ▶</button>
    </div>
    <div class="small" style="margin-top:6px;">⌨️ Enter: 정답 보기 → 다음 · ←/→: 이전/다음</div>
  </div>

  <div class="panel" id="orderPanel" style="display:none;">
    <div class="row" style="justify-content:space-between;">
      <span class="progress" id="orderProgress"></span>
      <span class="small" id="orderScore"></span>
    </div>
    <div class="ko" id="orderKo" style="margin-bottom:10px;"></div>
    <div class="small">아래 단어를 순서대로 눌러 문장을 완성하세요.</div>
    <div class="built" id="builtBox"></div>
    <div class="wordbank" id="wordBank"></div>
    <div class="row">
      <button class="primary" id="checkOrderBtn">채점하기</button>
      <button id="resetOrderBtn">다시 시작</button>
      <button id="revealOrderBtn">정답 보기</button>
    </div>
    <div class="result" id="orderResult"></div>
    <div class="nav">
      <button id="orderPrevBtn">◀ 이전</button>
      <button id="orderNextBtn">다음 ▶</button>
    </div>
    <div class="small" style="margin-top:6px;">⌨️ Enter: 채점 → 다음 · ←/→: 이전/다음</div>
  </div>

  <div class="panel" id="chunkPanel" style="display:none;">
    <div class="row" style="justify-content:space-between;"><span class="progress" id="chunkProgress"></span><span class="small" id="chunkScore"></span></div>
    <div class="row"><span class="small">덩어리 크기</span>
      <select id="kSize"><option value="small">잘게 (2~3단어)</option><option value="normal" selected>보통 (짧은 구)</option><option value="large">크게 (절 단위)</option></select></div>
    <div class="ko" id="chunkKo" style="margin-bottom:10px;"></div>
    <div class="small">덩어리를 순서대로 눌러 문장을 완성하세요. 쌓은 뒤에도 드래그하거나, 하나 눌러 선택 → 다른 덩어리를 눌러 자리를 바꿀 수 있어요. (×는 빼기)</div>
    <div class="built" id="kBuiltBox"></div>
    <div class="wordbank" id="kBank"></div>
    <div class="row"><button class="primary" id="kCheck">채점하기</button><button id="kReset">다시 시작</button><button id="kReveal">정답 보기</button></div>
    <div class="result" id="chunkResult"></div>
    <div class="nav"><button id="kPrev">◀ 이전</button><button id="kNext">다음 ▶</button></div>
    <div class="small" style="margin-top:6px;">⌨️ Enter: 채점 → 다음 · ←/→: 이전/다음 (덩어리를 선택한 상태에선 ←/→로 위치 이동, Esc로 선택 해제)</div>
  </div>

  <div class="panel" id="blankPanel" style="display:none;">
    <div class="row" style="justify-content:space-between;"><span class="progress" id="blankProgress"></span><span class="small" id="blankScore"></span></div>
    <div class="row"><span class="small">유형</span>
      <select id="blankKind"><option value="vocab">중요 단어 채우기</option><option value="gram">문법(전치사·접속사·관계사 등) 채우기</option></select>
      <button class="ghost" id="blankRedo">🔄 다른 빈칸</button></div>
    <div class="ko" id="blankKo" style="margin-bottom:8px;"></div>
    <div class="sent" id="blankSent" style="line-height:2.3;"></div>
    <div class="small" style="margin-top:10px;">보기 (눌러서 빈칸에 넣거나, 직접 입력하세요)</div>
    <div class="wordbank" id="blankBank"></div>
    <div class="row"><button class="primary" id="blankCheck">채점하기</button><button id="blankReveal">정답 보기</button></div>
    <div class="result" id="blankResult"></div>
    <div class="nav"><button id="blankPrev">◀ 이전</button><button id="blankNext">다음 ▶</button></div>
    <div class="small" style="margin-top:6px;">⌨️ Tab: 다음 빈칸 · Enter: 채점 → 다음 문장 · ←/→: 이전/다음</div>
  </div>

  <div class="panel" id="sorderPanel" style="display:none;">
    <div class="row" style="justify-content:space-between;"><span class="progress" id="sorderProgress"></span><span class="small" id="sorderScore"></span></div>
    <div class="row"><span class="small">지문</span><select id="secSelect"></select><button class="ghost" id="sorderRedo">🔀 다시 섞기</button></div>
    <div class="small">문장을 눌러 순서대로 쌓으세요. 쌓은 뒤에도 드래그하거나, 하나 눌러 선택 → 다른 문장을 눌러 자리를 바꿀 수 있어요. (×는 빼기)</div>
    <div class="built col" id="sBuiltBox"></div>
    <div class="wordbank col" id="sBank"></div>
    <div class="row"><button class="primary" id="sCheck">채점하기</button><button id="sReveal">정답 보기</button></div>
    <div class="result" id="sResult"></div>
    <div class="nav"><button id="sPrev">◀ 이전 지문</button><button id="sNext">다음 지문 ▶</button></div>
    <div class="small" style="margin-top:6px;">⌨️ Enter: 채점 → 다음 · ←/→: 이전/다음 지문 (문장을 선택한 상태에선 ←/→로 위치 이동, Esc로 선택 해제)</div>
  </div>

  <div class="panel" id="topicPanel" style="display:none;">
    <div class="row" style="justify-content:space-between;"><span class="progress" id="topicProgress"></span><span class="small" id="topicScore"></span></div>
    <div class="row"><span class="small">지문</span><select id="topicSelect"></select></div>
    <div class="small" style="margin-bottom:8px;">이 글의 주제문(핵심 문장)이라고 생각하는 문장을 고르세요. (숫자키 1~9)</div>
    <div id="topicBox"></div>
    <div class="result" id="topicResult"></div>
    <div class="nav"><button id="topicPrev">◀ 이전 지문</button><button id="topicNext">다음 지문 ▶</button></div>
    <div class="small" style="margin-top:6px;">※ 정답은 지문 내용을 보고 고른 것이라 학교 선생님 답과 다를 수 있어요. ⌨️ Enter: 다음 지문</div>
  </div>

  <div class="panel" id="typePanel" style="display:none;">
    <div class="row" style="justify-content:space-between;">
      <span class="progress" id="typeProgress"></span>
      <span class="small" id="typeScore"></span>
    </div>
    <div class="ko" id="typeKo" style="margin-bottom:10px;"></div>
    <div class="row" style="margin-bottom:6px;">
      <span class="small">난이도</span>
      <select id="typeDifficulty">
        <option value="easy">쉬움 (다음 단어 자동완성)</option>
        <option value="normal">보통 (다음 단어 앞글자만 힌트)</option>
        <option value="hard">어려움 (힌트 없음)</option>
      </select>
    </div>
    <textarea id="typeInput" rows="3" placeholder="문장을 기억나는 대로 영어로 입력하고 Enter를 누르세요 (마침표·쉼표 같은 기호는 안 써도 돼요)"></textarea>
    <div class="row" style="margin-top:8px;">
      <span class="small" id="hintLabel"></span>
      <button class="ghost" id="hintChip" style="display:none;"></button>
      <span class="small">Tab: 단어 자동완성 · Enter: 채점하기</span>
    </div>
    <div class="row">
      <button class="primary" id="checkBtn">채점하기</button>
      <button id="revealTypeBtn">정답 보기</button>
    </div>
    <div class="result" id="typeResult"></div>
    <div class="nav">
      <button id="typePrevBtn">◀ 이전</button>
      <button id="typeNextBtn">다음 ▶</button>
    </div>
  </div>

  <div class="panel" id="listPanel" style="display:none;">
    <div id="listBox"></div>
  </div>

  <div class="panel" id="scorePanel" style="display:none;">
    <div class="row" style="justify-content:space-between;">
      <span class="progress">전체 학습 기록</span>
      <button class="ghost" id="resetScoreBtn">기록 초기화</button>
    </div>
    <div id="scoreSummary"></div>
    <div class="small" style="margin-top:14px;margin-bottom:6px;">🔻 정확도가 낮은 문장 (더 연습이 필요해요)</div>
    <div id="weakList"></div>
  </div>
</div>

<script>
const DATA = {
"Lesson 1": ["Imagine an urban area that is fast becoming a slum after an economic recession.","Businesses close their doors and building owners have left.","The area is full of abandoned buildings.","There is no heating even in winter.","Only those who can't afford to move out remain.","Unemployment is high, many gangs emerge, and gang violence between young people becomes an everyday thing.","Despair is on every street corner.","One day, however, a gang called the Ghetto Brothers, which is respected because of its free meal and medical care programs, decides to stop fighting and instead persuade young people to unite and create a better community.","They suggest a peace treaty.","This is for peace, men. The violence took away our brothers.","We're not gangsters anymore.","We've got to make this town a better place to live.","Thanks to these new voices, gang leaders from all over the area sign the treaty.","Never again will they use violence to solve their disputes.","This is the Bronx in the New York of the early 1970s, where hip-hop music was born as a major part of a new culture of peace created by African Americans and immigrants.","The story of hip-hop, now one of the world's most dominant music genres, is a fascinating one.","Let's explore it.","After the peace treaty, the gangs that had fought one another started partying together.","Abandoned buildings and parking lots were used for events called block parties.","What eventually emerged from these parties?","A vibrant youth movement called hip-hop culture, based on DJing, breakdancing, MCing, and graffiti.","It replaced gang violence with music, dance, art, and style.","The most popular block parties were those hosted by DJ Kool Herc, who is regarded as the founding father of hip-hop.","Herc's party for his sister's birthday in August 1973 is now known as the birthplace of hip-hop music because of his new DJ technique.","At this party, using two turntables at the same time, Herc played two copies of the same record and then switched between them to extend the drum section known as the break.","No longer did people have to stop dancing at the end of the record while the DJ changed the LP disc.","Kool Herc's style of DJing quickly became influential in the rise of hip-hop music and breakdancing.","The break section became the most anticipated part of a song.","People would form dancer circles and use the break to show off their individual dancing skills.","Kool Herc named the people dancing to his music B-Boys and B-Girls, short for Break-Boys and Break-Girls.","These people began to dance in groups in the streets and build a street dance battle culture.","DJs like Kool Herc would speak in rhythm and rhyme over instrumental parts of songs to excite the crowd.","They would shout phrases like \"To the beat\" and \"You don't stop!\"","This was the beginning of rapping or MCing.","When DJs couldn't do both DJing and rhythmic shouting at the same time, they asked a friend to take the role of addressing the audience, that is, of the Master of Ceremonies or MC, at their parties.","This separate role became MCing.","Then where did the word hip-hop originate from?","Nobody knows the exact origin, but it is said that the word hip-hop was first used at a block party.","A young man at the party was about to join the army.","His good friend, the MC of the party, reminded him in a jokey way that his days of freedom were over by moving across the stage like a military instructor to the beat, chanting, \"Hip-hop-hip-hop-hip-hop.\"","The crowd loved it.","After that, MCs tried out new lines at parties like \"Hip, hop, hippy to the hippy hop-bop,\" \"I said a hip-hop, a hibbit, hibby-dibby, hip-hip-hop and you don't stop.\"","Such lines became a standard part of rap and were included in the lyrics of the first recorded hip-hop album, Rapper's Delight (1979), which was an immediate success.","The new music and dancing spread very fast.","There was a sharp unexplained decline in violence during the 1980s in New York.","One police officer asked a boy on the street if he knew why.","The boy said, \"It's because they're dancing.\"","Rap lyrics, which started as the party chants of early hip-hop, typically focused on having a good time together and boasting about the ability of the DJ or the rapper.","However, a significant shift occurred during the rise of hip-hop music in the 1980s.","Most importantly, the appearance of The Message by the group Grandmaster Flash and Furious Five in 1982 was groundbreaking.","This song's powerful lyrics detailed the harsh realities of life in the ghetto, including the sad story of a boy who was born there and had to live and die as a second-class citizen.","With this song, hip-hop, whose original aim was creating peace through partying, started a new tradition of social criticism against injustice.","The Message became one of the most influential rap singles of all time, and was followed by many hip-hop songs that confronted racial discrimination in the 1980s and 1990s.","At the same time, their music with its rhythmic beat and rapping made people move or dance.","People heard the social message while moving to the beat.","Also, a new attitude gradually emerged in hip-hop lyrics, which can be summarized as \"respect.\"","Hip-hop music began to be a way to express respect for one another, and particularly for hip-hop pioneers.","For rappers, the pioneers are heroes who overcame hardship and injustice and went on to create new music and culture.","Therefore, many hip-hoppers agree that they cannot have too much respect for these heroes.","The phrase \"something from nothing,\" often used to summarize the genre's history, is also an expression of admiration for early hip-hoppers, who developed a new culture in the ghetto despite very limited resources.","Rappers also proudly expressed their feeling that they, too, and perhaps the audience, had suffered and survived in a similar way to the pioneers.","This is why the message of respect is about both self-worth and respect for others.","The importance of the idea of respect in hip-hop cannot be emphasized strongly enough.","\"The HipHop Declaration of Peace,\" presented to the UN on May 16th, 2001, officially recognized hip-hop as an international culture of peace.","This document reminds us of hip-hop's original mission to bring peace to the Bronx, implying global respect for hip-hop history.","When hip-hop artist Kendrick Lamar was awarded a Pulitzer Prize for Music in 2018, it was another important event.","It meant that hip-hop, which started as underground music, had become a major art form like classical music.","If hip-hop music was born as part of a cultural movement for peace in the 1970s, today more people regard it as a powerful means to express oneself, fight against injustice, bring about change through social messages, and still be engaged with music and connected to others.","With its unique and growing appeal, hip-hop continues to be a global phenomenon."],
"Lesson 2": ["Jiyun, a high school student, starts her day by logging in to a music streaming service on her smartphone.","She enjoys listening to her favorite music, discovering new songs, and exploring new artists every day.","For a healthy breakfast, Jiyun receives a delivery from a subscription service that provides fresh vegetables and fruits.","After school, Jiyun utilizes a video lecture service to expand her knowledge in whatever she finds interesting.","For example, she watches various academic lectures to review her schoolwork and stay updated about the latest knowledge in her chosen field of study.","During weekends, Jiyun and her family spend quality time together watching movies or dramas using a streaming service.","To be sure, the subscription economy is a popular economic model nowadays, and Jiyun is actively taking part in it.","The concept of business models based on subscriptions is not new.","Initially it was limited to products such as milk and newspapers.","However, these business models have expanded to all industries, including entertainment, technology, fashion, education, and much more.","Instead of creating a hit product that will be sold once, companies now prioritize providing continuing value, such as new content, more personalization, or access to updates.","Customers pay for these benefits via a regular subscription.","The subscription economy brings advantages for both companies and consumers.","Companies can have a stable revenue and build customer loyalty by using the subscription model.","From the consumers' perspective, they can enjoy a wider range of choices and personalized experiences.","They can also save money by having flexible subscription contracts.","The rise of the subscription economy is closely connected to two major drivers: changes in consumption trends and the rapid growth of online platforms.","The subscription economy is highly relevant to how people consume goods and services nowadays.","More and more people prioritize experiences over owning things.","This makes the subscription economy attractive to them because it offers access to services or content without the requirement of ownership.","For example, by subscribing to a music streaming service, consumers can enjoy limitless music without the need for having disc albums.","Another example is subscribing to clothing services, where consumers can explore a variety of clothing styles without filling up their drawers.","Moreover, consumers value diversity and customization.","The subscription economy offers a diverse range of subscription options, enabling individuals to personalize their experiences based on their own preferences and interests.","This aspect of the subscription economy is more popular among younger generations, as they enjoy expressing their uniqueness and discovering valuable content and services that fit with their individual tastes.","A popular example that has gained attention is the cosmetics subscription service.","It stands out by thoroughly analyzing customers' current skin conditions and providing specialized recommendations, including manufacturing cosmetics created to address each individual's unique skin concerns.","To come to the point, consumers receive a personalized experience that prioritizes their individual skin conditions, rather than a uniform purchasing process.","Furthermore, consumers appreciate flexibility and convenience.","In some subscription models, consumers can experience a variety of products or services for a fixed cost.","In other models, they can flexibly adjust subscription fees by choosing only the necessary services or products when needed.","This means they can either enjoy a range of offerings for a set price or save money by selecting only what they really need.","Moreover, subscription services are made easily accessible by online platforms, enhancing consumers' convenience.","With just a few clicks, consumers can receive services or products.","If they wish to change their device model, they can do so.","Whoever desires an upgrade can get it and experience the latest models.","With the development of various digital devices centered on smartphones, consumers have been able to do numerous things with great ease.","In the past, people had no choice but to go to the theater or purchase the videos they wanted to watch.","Today, whoever wants to watch a movie can access online media subscription platforms and enjoy a vast selection of movies on their smartphones or other digital devices.","At the same time, these platforms make it easy for companies to offer customized products and services to customers.","By applying AI and big data algorithm technology, companies identify consumers' needs, tastes, and consumption patterns.","People appreciate having diverse choices and customization, which in turn enhances their satisfaction.","Services, such as suggesting personalized clothing styles based on customers' purchase history or recommending videos that match their movie and video viewing history, bring them great satisfaction.","Although the subscription economy offers many advantages, there are also some disadvantages to consider.","One concern is the potential for overconsumption.","The convenience and accessibility of the subscription model make it easy for consumers to sign up for multiple services and use a lot of content or products than actually needed.","This can result in excessive consumption and using more subscription services than actually needed.","It is crucial to subscribe only to services that are truly necessary and avoid subscribing to similar services.","Related to the concern of overconsumption is the financial burden that subscriptions can create.","While the cost of individual subscriptions may seem affordable, subscribing to multiple services can add up quickly.","It is important to carefully consider the costs of these subscriptions to avoid financial strain.","Regularly reviewing and canceling unnecessary subscriptions can be beneficial.","Additionally, the subscription economy can contribute to environmental pollution.","The regular delivery of products in packaging materials can increase the use of disposable packaging, which harms the environment.","Using environmentally friendly packaging materials and opting for reusable packaging can help reduce this problem.","Despite the potential limitations of subscription services, they have become deeply embedded in our lives and more and more businesses are jumping onto the subscription economy model.","It is expected that new subscription services will be continuously provided to consumers in new areas in the future.","We need to have a deeper understanding of the subscription economy and become wise consumers who receive the services that are really needed."]
};

const KO = {
"Lesson 1": ["경기 침체 이후 빠르게 슬럼화되어 가는 도시 지역을 상상해보라.","사업체들은 문을 닫고 건물주들은 떠나버린다.","그 지역은 버려진 건물들로 가득하다.","겨울에도 난방이 되지 않는다.","이사 갈 여유가 없는 사람들만 남는다.","실업률이 높고, 많은 갱단이 생겨나며, 젊은이들 사이의 갱 폭력이 일상적인 일이 된다.","절망이 거리 곳곳에 있다.","그러나 어느 날, 무료 급식과 의료 지원 프로그램으로 존경받는 '게토 브라더스'라는 갱단이 싸움을 멈추고 대신 젊은이들을 설득해 단결하여 더 나은 공동체를 만들기로 결심한다.","그들은 평화 조약을 제안한다.","\"이건 평화를 위한 거야, 형제들. 폭력이 우리 형제들을 앗아갔어.\"","\"우린 더 이상 갱스터가 아니야.\"","\"우린 이 동네를 더 살기 좋은 곳으로 만들어야 해.\"","이 새로운 목소리들 덕분에, 그 지역 전역의 갱 리더들이 조약에 서명한다.","그들은 다시는 분쟁을 해결하기 위해 폭력을 쓰지 않는다.","이곳이 바로 1970년대 초 뉴욕의 브롱크스이며, 아프리카계 미국인과 이민자들이 만들어낸 새로운 평화의 문화의 주요한 부분으로서 힙합 음악이 탄생한 곳이다.","이제 세계에서 가장 지배적인 음악 장르 중 하나가 된 힙합의 이야기는 매력적이다.","함께 탐구해보자.","평화 조약 이후, 서로 싸웠던 갱들이 함께 파티를 시작했다.","버려진 건물과 주차장이 '블록 파티'라는 행사에 사용되었다.","이런 파티에서 결국 무엇이 생겨났을까?","DJing, 브레이크댄싱, MCing, 그래피티를 기반으로 한 활기찬 청년 운동, 즉 힙합 문화였다.","그것은 갱 폭력을 음악, 춤, 예술, 스타일로 대체했다.","가장 인기 있던 블록 파티는 '힙합의 창시자'로 여겨지는 DJ 쿨 허크가 주최한 파티였다.","1973년 8월 그의 여동생 생일을 위해 열린 허크의 파티는 그의 새로운 DJ 기법 덕분에 오늘날 힙합 음악의 탄생지로 알려져 있다.","이 파티에서, 두 대의 턴테이블을 동시에 사용하여 허크는 같은 레코드 두 장을 틀고 브레이크라 불리는 드럼 파트를 늘리기 위해 그 둘을 전환했다.","더 이상 사람들은 DJ가 LP판을 바꾸는 동안 레코드가 끝날 때 춤을 멈출 필요가 없었다.","쿨 허크의 DJing 스타일은 빠르게 힙합 음악과 브레이크댄싱의 부상에 영향력을 갖게 되었다.","브레이크 구간은 노래에서 가장 기대되는 부분이 되었다.","사람들은 댄서 서클을 만들어 브레이크 구간을 이용해 각자의 춤 실력을 뽐냈다.","쿨 허크는 그의 음악에 맞춰 춤추는 사람들을 'B-보이', 'B-걸'이라 이름 붙였는데, 이는 'Break-Boys'와 'Break-Girls'의 줄임말이다.","이 사람들은 거리에서 그룹을 지어 춤추기 시작했고 거리 댄스 배틀 문화를 만들었다.","쿨 허크 같은 DJ들은 관중을 흥분시키기 위해 노래의 연주 부분 위에서 리듬과 라임을 넣어 말하곤 했다.","그들은 \"To the beat\", \"You don't stop!\" 같은 문구를 외치곤 했다.","이것이 랩, 즉 MCing의 시작이었다.","DJ들이 DJing과 리듬감 있는 외침을 동시에 할 수 없을 때, 그들은 친구에게 파티에서 청중을 상대하는 역할, 즉 MC(사회자)의 역할을 맡아달라고 부탁했다.","이 별개의 역할이 MCing이 되었다.","그렇다면 '힙합'이라는 단어는 어디서 유래했을까?","정확한 기원은 아무도 모르지만, '힙합'이라는 단어가 어느 블록 파티에서 처음 쓰였다고 전해진다.","파티에 있던 한 젊은 남자가 군대에 입대하려던 참이었다.","그의 좋은 친구이자 그 파티의 MC는 무대를 가로질러 움직이며 마치 군대 교관처럼 박자에 맞춰 \"Hip-hop-hip-hop-hip-hop\"이라고 외치면서, 그의 자유로운 날들이 끝났다는 것을 농담조로 상기시켜 주었다.","관중들은 그것을 매우 좋아했다.","그 후, MC들은 \"Hip, hop, hippy to the hippy hop-bop,\" \"I said a hip-hop, a hibbit, hibby-dibby, hip-hip-hop and you don't stop\" 같은 새로운 가사들을 파티에서 시도했다.","이러한 구절들은 랩의 표준적인 부분이 되었고, 즉각적인 성공을 거둔 최초의 녹음된 힙합 앨범 Rapper's Delight(1979)의 가사에 포함되었다.","새로운 음악과 춤은 매우 빠르게 퍼져나갔다.","1980년대 뉴욕에서는 설명되지 않는 급격한 폭력 감소가 있었다.","한 경찰관이 거리의 한 소년에게 그 이유를 아는지 물었다.","그 소년은 \"다들 춤을 추고 있어서 그래요.\"라고 말했다.","초기 힙합의 파티 구호로 시작된 랩 가사는 대개 함께 즐거운 시간을 보내는 것과 DJ나 래퍼의 실력을 자랑하는 내용에 초점을 맞췄다.","그러나 1980년대 힙합 음악의 부상기에 중요한 변화가 일어났다.","가장 중요하게는, 1982년 그룹 Grandmaster Flash and Furious Five의 \"The Message\"의 등장이 획기적이었다.","이 곡의 강렬한 가사는 게토에서의 가혹한 삶의 현실을, 그곳에서 태어나 이류 시민으로 살고 죽어야 했던 한 소년의 이야기를 자세히 담았다.","원래 파티를 통한 평화 창출을 목표로 했던 힙합은, 이 곡을 통해 부정의에 맞선 사회 비판이라는 새로운 전통을 시작했다.","\"The Message\"는 역대 가장 영향력 있는 랩 싱글 중 하나가 되었고, 이후 1980~90년대 인종차별에 맞선 많은 힙합 노래들이 뒤따랐다.","동시에, 리드미컬한 비트와 랩이 담긴 그들의 음악은 사람들을 움직이거나 춤추게 만들었다.","사람들은 비트에 맞춰 몸을 움직이며 그 사회적 메시지를 들었다.","또한, 힙합 가사에서 새로운 태도가 점차 생겨났는데, 이는 '존중'으로 요약될 수 있다.","힙합 음악은 서로에 대한, 특히 힙합 선구자들에 대한 존중을 표현하는 방식이 되기 시작했다.","래퍼들에게 그 선구자들은 역경과 부정의를 극복하고 새로운 음악과 문화를 만들어낸 영웅들이다.","그래서 많은 힙합 팬들은 이 영웅들에 대해 아무리 존경해도 지나치지 않다는 데 동의한다.","\"something from nothing\"이라는 표현은 매우 제한된 자원 속에서도 새로운 문화를 만들어낸 초기 힙합퍼들에 대한 존경의 표현이다.","래퍼들은 또한 자신들이, 그리고 어쩌면 청중들도, 선구자들과 비슷하게 고통받고 살아남았다는 느낌을 자랑스럽게 표현했다.","이것이 바로 존중이라는 메시지가 자기 존중과 타인에 대한 존중 둘 다를 의미하는 이유이다.","힙합에서 존중이라는 개념의 중요성은 아무리 강조해도 지나치지 않다.","2001년 5월 16일 UN에 제출된 \"The HipHop Declaration of Peace\"는 힙합을 국제적인 평화의 문화로 공식 인정했다.","이 문서는 힙합의 원래 사명, 즉 브롱크스에 평화를 가져오는 것을 상기시키며, 전 세계적인 힙합 역사에 대한 존중을 함축하고 있다.","힙합 아티스트 켄드릭 라마가 2018년 퓰리처 음악상을 수상했을 때, 그것은 또 다른 중요한 사건이었다.","그것은 언더그라운드 음악으로 시작한 힙합이 클래식 음악처럼 주요한 예술 형식이 되었다는 것을 의미했다.","힙합 음악이 1970년대 평화를 위한 문화 운동의 일부로 태어났다면, 오늘날 더 많은 사람들이 그것을 자신을 표현하고, 부정의에 맞서 싸우고, 사회적 메시지를 통해 변화를 가져오며, 여전히 음악에 참여하고 다른 사람들과 연결되는 강력한 수단으로 여긴다.","독특하고 계속 커지는 매력과 함께, 힙합은 계속해서 세계적인 현상으로 남아 있다."],
"Lesson 2": ["고등학생인 지윤이는 스마트폰으로 음악 스트리밍 서비스에 로그인하며 하루를 시작한다.","그녀는 매일 좋아하는 음악을 듣고, 새로운 곡을 발견하고, 새로운 아티스트를 탐색하는 것을 즐긴다.","건강한 아침 식사를 위해, 지윤이는 신선한 채소와 과일을 제공하는 구독 서비스에서 배달을 받는다.","방과 후, 지윤이는 흥미로운 어떤 것이든 지식을 넓히기 위해 영상 강의 서비스를 이용한다.","예를 들어, 그녀는 학업을 복습하고 자신이 선택한 분야의 최신 지식을 파악하기 위해 다양한 학술 강의를 시청한다.","주말 동안, 지윤이와 가족은 스트리밍 서비스를 이용해 영화나 드라마를 보며 함께 시간을 보낸다.","확실히, 구독 경제는 오늘날 인기 있는 경제 모델이며 지윤이도 이에 적극적으로 참여하고 있다.","구독을 기반으로 한 비즈니스 모델의 개념은 새로운 것이 아니다.","처음에는 우유나 신문 같은 제품에 한정되어 있었다.","그러나 이러한 비즈니스 모델은 엔터테인먼트, 기술, 패션, 교육 등 모든 산업으로 확장되었다.","한 번 팔리고 끝나는 히트 상품을 만드는 대신, 기업들은 이제 새로운 콘텐츠, 더 많은 개인화, 또는 업데이트 접근권 같은 지속적인 가치를 제공하는 데 우선순위를 둔다.","고객들은 정기 구독을 통해 이러한 혜택에 대한 비용을 지불한다.","구독 경제는 기업과 소비자 모두에게 이점을 가져다준다.","기업은 구독 모델을 이용함으로써 안정적인 수익을 얻고 고객 충성도를 쌓을 수 있다.","소비자의 관점에서는, 더 폭넓은 선택지와 개인 맞춤형 경험을 누릴 수 있다.","그들은 또한 유연한 구독 계약을 통해 돈을 절약할 수 있다.","구독 경제의 부상은 두 가지 주요 요인, 즉 소비 트렌드의 변화와 온라인 플랫폼의 빠른 성장과 밀접하게 연관되어 있다.","구독 경제는 오늘날 사람들이 상품과 서비스를 소비하는 방식과 매우 밀접한 관련이 있다.","점점 더 많은 사람들이 소유하는 것보다 경험을 우선시한다.","이것이 구독 경제를 매력적으로 만드는 이유인데, 소유의 부담 없이 서비스나 콘텐츠에 접근할 수 있게 해주기 때문이다.","예를 들어, 음악 스트리밍 서비스를 구독함으로써 소비자는 음반을 소유할 필요 없이 무제한으로 음악을 즐길 수 있다.","또 다른 예로는 옷장을 채울 필요 없이 다양한 의류 스타일을 탐색할 수 있는 의류 구독 서비스가 있다.","게다가, 소비자들은 다양성과 맞춤화를 중요하게 여긴다.","구독 경제는 다양한 구독 옵션을 제공하여 개인이 자신의 선호와 관심사에 맞게 경험을 맞춤화할 수 있게 해준다.","구독 경제의 이러한 측면은 젊은 세대들 사이에서 더 인기가 있는데, 그들은 자신만의 개성을 표현하고 개인의 취향에 맞는 콘텐츠와 서비스를 발견하는 것을 즐기기 때문이다.","관심을 끄는 대표적인 예가 화장품 구독 서비스이다.","그것은 고객의 현재 피부 상태를 철저히 분석하고 각자의 고유한 피부 고민을 해결하기 위해 제작된 화장품을 포함한 맞춤형 추천을 제공함으로써 두각을 나타낸다.","요컨대, 소비자들은 획일적인 구매 과정이 아니라 자신의 피부 상태를 우선시하는 맞춤형 경험을 받는다.","게다가, 소비자들은 유연성과 편리함을 중요하게 여긴다.","일부 구독 모델에서는 소비자가 고정된 비용으로 다양한 제품이나 서비스를 경험할 수 있다.","다른 모델에서는 필요할 때 구독료를 유연하게 조정할 수 있다.","이는 그들이 정해진 가격에 다양한 서비스를 즐기거나, 정말 필요한 것만 선택해서 돈을 절약할 수 있다는 것을 의미한다.","게다가, 구독 서비스는 온라인 플랫폼을 통해 쉽게 접근 가능해져서 소비자의 편의성을 높인다.","클릭 몇 번만으로, 소비자는 서비스나 제품을 받을 수 있다.","기기 모델을 바꾸고 싶다면, 그렇게 할 수 있다.","업그레이드를 원하는 사람은 누구든 최신 모델을 얻고 경험할 수 있다.","스마트폰을 중심으로 한 다양한 디지털 기기의 발전으로, 소비자들은 수많은 일을 매우 쉽게 할 수 있게 되었다.","과거에는 사람들이 보고 싶은 영화를 보려면 극장에 가거나 비디오를 구매하는 것 외에는 선택지가 없었다.","오늘날, 영화를 보고 싶은 사람은 누구든 온라인 미디어 구독 플랫폼에 접속하여 스마트폰이나 다른 디지털 기기에서 방대한 영화 선택지를 즐길 수 있다.","동시에, 이러한 플랫폼들은 기업이 고객에게 맞춤형 상품과 서비스를 제공하는 것을 쉽게 만든다.","AI와 빅데이터 알고리즘 기술을 적용함으로써, 기업들은 소비자의 필요, 취향, 소비 패턴을 파악한다.","사람들은 다양한 선택지와 맞춤화를 갖는 것을 좋아하며, 이는 결국 그들의 만족도를 높인다.","고객의 구매 이력에 기반한 맞춤형 의류 스타일 제안이나, 영화·비디오 시청 이력에 맞는 영상 추천 같은 서비스는 그들에게 큰 만족을 가져다준다.","구독 경제가 많은 장점을 제공하지만, 고려해야 할 몇 가지 단점도 있다.","한 가지 우려는 과소비의 가능성이다.","구독 모델의 편리함과 접근성은 소비자가 여러 서비스에 가입하고 많은 콘텐츠나 제품을 사용하는 것을 쉽게 만든다.","이는 과도한 소비와 실제로 필요한 것보다 더 많은 구독 서비스를 사용하는 결과를 초래할 수 있다.","정말 필요한 서비스만 구독하고, 비슷한 서비스에 중복 가입하는 것을 피하는 것이 중요하다.","과소비 문제와 관련하여, 구독이 야기할 수 있는 재정적 부담이 있다.","개별 구독의 비용은 저렴해 보일 수 있지만, 여러 서비스를 구독하면 비용이 빠르게 늘어날 수 있다.","재정적 부담을 피하기 위해 이러한 구독의 비용을 신중하게 고려하는 것이 중요하다.","불필요한 구독을 정기적으로 검토하고 해지하는 것이 도움이 될 수 있다.","게다가, 구독 경제는 환경 오염에 기여할 수 있다.","포장재를 이용한 정기적인 제품 배송은 일회용 포장재 사용을 증가시켜 환경에 해를 끼칠 수 있다.","환경친화적인 포장재를 사용하고 재사용 가능한 포장을 선택하는 것이 이 문제를 줄이는 데 도움이 될 수 있다.","구독 서비스의 잠재적 한계에도 불구하고, 그것들은 우리 삶에 깊이 자리 잡았고 점점 더 많은 기업들이 구독 경제 모델에 뛰어들고 있다.","앞으로 새로운 분야에서 새로운 구독 서비스가 소비자들에게 계속 제공될 것으로 예상된다.","우리는 구독 경제에 대해 더 깊이 이해하고, 정말 필요한 서비스만 받는 현명한 소비자가 되어야 한다."]
};

let curLesson = "Lesson 1";
let order = [];
let idx = 0;
let stats = {ok:0, total:0};
let orderStats = {ok:0, total:0};
let builtWords = [];
let bankState = [];
// attemptLog[lesson][sentenceIndex] = {ok, total}  (typing + order 채점 통합 기록)
let attemptLog = {"Lesson 1":{}, "Lesson 2":{}};

function saveProgress(){
  try{ localStorage.setItem('hs_progress', JSON.stringify({lesson:curLesson, idx, stats, orderStats, attemptLog})); }catch(e){}
}
function loadProgress(){
  try{
    const raw = localStorage.getItem('hs_progress');
    if(raw){
      const p = JSON.parse(raw);
      if(p.lesson && DATA[p.lesson]) curLesson = p.lesson;
      if(typeof p.idx === 'number') idx = p.idx;
      if(p.stats) stats = p.stats;
      if(p.orderStats) orderStats = p.orderStats;
      if(p.attemptLog) attemptLog = p.attemptLog;
    }
  }catch(e){}
}
function logAttempt(lesson, sentIdx, isCorrect){
  if(!attemptLog[lesson]) attemptLog[lesson] = {};
  if(!attemptLog[lesson][sentIdx]) attemptLog[lesson][sentIdx] = {ok:0, total:0};
  attemptLog[lesson][sentIdx].total++;
  if(isCorrect) attemptLog[lesson][sentIdx].ok++;
}
function renderScore(){
  const summary = document.getElementById('scoreSummary');
  const weakBox = document.getElementById('weakList');
  let totalOk=0, totalAll=0;
  let rows = [];
  Object.keys(DATA).forEach(lesson=>{
    let lOk=0, lAll=0;
    const log = attemptLog[lesson] || {};
    Object.keys(log).forEach(i=>{
      lOk += log[i].ok; lAll += log[i].total;
      const pct = Math.round((log[i].ok/log[i].total)*100);
      rows.push({lesson, idx:Number(i), pct, total:log[i].total, sent:DATA[lesson][i]});
    });
    totalOk += lOk; totalAll += lAll;
  });
  const overallPct = totalAll ? Math.round((totalOk/totalAll)*100) : 0;
  let grade = '-';
  if(totalAll>0){
    if(overallPct>=90) grade='A';
    else if(overallPct>=75) grade='B';
    else if(overallPct>=60) grade='C';
    else grade='D';
  }
  summary.innerHTML = `
    <div class="scorecards">
      <div class="scorecard"><div class="big">${overallPct}%</div><div class="lbl">전체 정확도</div></div>
      <div class="scorecard"><div class="big">${grade}</div><div class="lbl">학습 등급</div></div>
      <div class="scorecard"><div class="big">${totalOk} / ${totalAll}</div><div class="lbl">누적 정답 / 시도</div></div>
      <div class="scorecard"><div class="big">${rows.length}</div><div class="lbl">시도한 문장 수</div></div>
    </div>`;
  rows.sort((a,b)=> a.pct - b.pct || b.total - a.total);
  const weakest = rows.filter(r=>r.pct<80).slice(0,8);
  weakBox.innerHTML = weakest.length ? weakest.map(r=>
    `<div class="weakitem"><span class="num">${r.lesson} #${r.idx+1}</span>${r.sent}<span class="pct">${r.pct}%</span></div>`
  ).join('') : `<div class="small">아직 충분한 채점 기록이 없어요. 타이핑/순서 배열 연습을 해보세요!</div>`;
}
document.getElementById('resetScoreBtn').onclick = ()=>{
  attemptLog = {"Lesson 1":{}, "Lesson 2":{}};
  stats = {ok:0, total:0};
  orderStats = {ok:0, total:0};
  saveProgress();
  renderScore();
  renderType();
};

function buildTabs(){
  const el = document.getElementById('lessonTabs');
  el.innerHTML = '';
  Object.keys(DATA).forEach(name=>{
    const b = document.createElement('button');
    b.className = 'tab-btn' + (name===curLesson ? ' active' : '');
    b.textContent = name;
    b.onclick = ()=>{ curLesson = name; idx = 0; sSec = 0; tSec = 0; resetOrderList(); renderAll(); };
    el.appendChild(b);
  });
}
function resetOrderList(){ order = DATA[curLesson].map((_,i)=>i); }
function shuffleOrder(){
  for(let i=order.length-1;i>0;i--){ const j=Math.floor(Math.random()*(i+1)); [order[i],order[j]]=[order[j],order[i]]; }
  idx = 0; renderAll();
}
function currentSentence(){ return DATA[curLesson][order[idx]]; }
function currentKo(){ return (KO[curLesson]||[])[order[idx]]||''; }
function blankVersion(s){ return s.replace(/[A-Za-z']+/g, w => '▢'.repeat(Math.min(w.length,6))); }

function renderCard(){
  document.getElementById('cardProgress').textContent = `${curLesson} · ${idx+1} / ${order.length}`;
  const sentEl = document.getElementById('cardSent');
  sentEl.className = 'sent blank';
  sentEl.textContent = blankVersion(currentSentence());
  document.getElementById('cardKo').textContent = '';
  cardRevealed = false;
}
function renderType(){
  document.getElementById('typeProgress').textContent = `${curLesson} · ${idx+1} / ${order.length}`;
  document.getElementById('typeScore').textContent = stats.total ? `정답 ${stats.ok} / ${stats.total}` : '';
  document.getElementById('typeInput').value = '';
  document.getElementById('typeResult').innerHTML = '';
  document.getElementById('typeKo').textContent = koLine();
  typeChecked = false;
  updateHint();
}
function currentWordIndexInfo(){
  const target = currentSentence().split(' ').filter(w=>w.replace(/[—–]/g,'').trim());
  const val = document.getElementById('typeInput').value;
  const endsWithSpace = /\s$/.test(val) || val.length===0;
  const typedWords = val.trim().length ? val.trim().split(/\s+/) : [];
  let idxWord = endsWithSpace ? typedWords.length : typedWords.length - 1;
  if(idxWord < 0) idxWord = 0;
  return {target, idxWord};
}
function updateHint(){
  const diff = document.getElementById('typeDifficulty').value;
  const hintChip = document.getElementById('hintChip');
  const hintLabel = document.getElementById('hintLabel');
  const {target, idxWord} = currentWordIndexInfo();
  if(diff==='hard' || idxWord>=target.length){
    hintChip.style.display='none'; hintLabel.textContent=''; return;
  }
  const full = target[idxWord];
  const shown = diff==='easy' ? full : (full.slice(0,2) + '···');
  hintLabel.textContent = '다음 단어 힌트:';
  hintChip.style.display='inline-block';
  hintChip.textContent = shown;
  hintChip.dataset.full = full;
}
function acceptHint(){
  const diff = document.getElementById('typeDifficulty').value;
  if(diff==='hard') return;
  const {target, idxWord} = currentWordIndexInfo();
  if(idxWord>=target.length) return;
  const newWords = target.slice(0, idxWord+1);
  let newVal = newWords.join(' ');
  if(idxWord+1 < target.length) newVal += ' ';
  const box = document.getElementById('typeInput');
  box.value = newVal;
  box.focus();
  updateHint();
}
document.getElementById('typeInput').addEventListener('input', updateHint);
let typeChecked = false;
let cardRevealed = false;
let orderChecked = false;
document.getElementById('typeInput').addEventListener('keydown', (e)=>{
  if(e.key === 'Tab'){ e.preventDefault(); acceptHint(); }
  else if(e.key === 'Enter' && !e.shiftKey){
    e.preventDefault();
    if(typeChecked){ document.getElementById('typeNextBtn').click(); }
    else { document.getElementById('checkBtn').click(); typeChecked = true; }
  }
});
document.getElementById('hintChip').addEventListener('click', acceptHint);
document.getElementById('typeDifficulty').addEventListener('change', updateHint);

// 화살표 키로 이전/다음 문장 이동 (입력창/드롭다운에 포커스가 없을 때)
function isFocusedOnField(){
  const ae = document.activeElement;
  return ae && (ae.tagName==='TEXTAREA' || ae.tagName==='SELECT' || ae.tagName==='INPUT');
}
function activeModeName(){
  const b = document.querySelector('.mode-btn.active');
  return b ? b.dataset.mode : 'card';
}
const KEYMODES=['card','order','chunk','blank','sorder','topic','type'];
function navigate(d){
  const m=activeModeName();
  if(m==='card') document.getElementById(d>0?'nextBtn':'prevBtn').click();
  else if(m==='order') document.getElementById(d>0?'orderNextBtn':'orderPrevBtn').click();
  else if(m==='type') document.getElementById(d>0?'typeNextBtn':'typePrevBtn').click();
  else if(m==='chunk') chunkMove(d);
  else if(m==='blank') blankMove(d);
  else if(m==='sorder'){ const n=secList().length; if(n){ sSec=(sSec+d+n)%n; document.getElementById('secSelect').value=String(sSec); renderSorder(); } }
  else if(m==='topic'){ const n=topicSecs().length; if(n){ tSec=(tSec+d+n)%n; document.getElementById('topicSelect').value=String(tSec); renderTopic(); } }
}
document.addEventListener('keydown', (e)=>{
  if(isFocusedOnField()) return;
  const m=activeModeName();
  if(!KEYMODES.includes(m)) return;
  if(e.key==='ArrowRight'||e.key==='ArrowLeft'){
    const d=e.key==='ArrowRight'?1:-1; e.preventDefault();
    if(m==='order'&&orderSel.pos!==null) moveSel(builtWords,orderSel,d,drawOrderUI);
    else if(m==='sorder'&&sSel.pos!==null) moveSel(sBuilt,sSel,d,drawSorder);
    else if(m==='chunk'&&kSel.pos!==null) moveSel(kBuilt,kSel,d,drawChunk);
    else navigate(d);
  } else if(e.key==='Enter'){
    e.preventDefault();
    if(m==='card'){ if(!cardRevealed) document.getElementById('revealBtn').click(); else navigate(1); }
    else if(m==='order'){ if(!orderChecked) document.getElementById('checkOrderBtn').click(); else navigate(1); }
    else if(m==='type'){ document.getElementById('typeInput').focus(); }
    else if(m==='chunk'){ if(!kChecked) checkChunk(); else navigate(1); }
    else if(m==='blank'){ if(!blankChecked) checkBlank(); else navigate(1); }
    else if(m==='sorder'){ if(!sChecked) checkSorder(); else navigate(1); }
    else if(m==='topic'){ if(tAnswered) navigate(1); }
  } else if(e.key==='Escape'){
    orderSel.pos=null; sSel.pos=null; kSel.pos=null; if(m==='chunk') drawChunk(); if(m==='order') drawOrderUI(); if(m==='sorder') drawSorder();
  } else if(m==='topic' && /^[1-9]$/.test(e.key)){ answerTopic(Number(e.key)-1); }
});
function renderList(){
  const box = document.getElementById('listBox');
  box.innerHTML = '';
  DATA[curLesson].forEach((s,i)=>{
    const d = document.createElement('div');
    d.className = 'listitem';
    d.innerHTML = `<span class="num">${i+1}.</span>${s}<div class="ko">${(KO[curLesson]||[])[i]||''}</div>`;
    box.appendChild(d);
  });
}
function tokenize(s){
  return s.match(/[A-Za-z0-9']+|[.,!?"]/g) || [];
}
function renderOrder(){
  document.getElementById('orderProgress').textContent = `${curLesson} · ${idx+1} / ${order.length}`;
  document.getElementById('orderScore').textContent = orderStats.total ? `정답 ${orderStats.ok} / ${orderStats.total}` : '';
  document.getElementById('orderKo').textContent = koLine();
  document.getElementById('orderResult').innerHTML = '';
  const words = tokenize(currentSentence()).filter(w=>/[A-Za-z0-9]/.test(w));
  bankState = words.map((w,i)=>({w, id:i, used:false}));
  for(let i=bankState.length-1;i>0;i--){ const j=Math.floor(Math.random()*(i+1)); [bankState[i],bankState[j]]=[bankState[j],bankState[i]]; }
  builtWords = [];
  orderChecked = false;
  orderSel.pos = null;
  drawOrderUI();
}
function drawOrderUI(){
  const bank=document.getElementById('wordBank'); bank.innerHTML='';
  bankState.forEach(item=>{
    const chip=document.createElement('span');
    chip.className='wchip'+(item.used?' used':'');
    chip.textContent=item.w;
    chip.onclick=()=>{ if(item.used) return; item.used=true; builtWords.push(item); drawOrderUI(); };
    bank.appendChild(chip);
  });
  drawBuilt(document.getElementById('builtBox'), builtWords, orderSel, 'wchip', pos=>{ builtWords[pos].used=false; builtWords.splice(pos,1); drawOrderUI(); });
}
function renderAll(){
  buildTabs();
  if(!order.length || order.length !== DATA[curLesson].length) resetOrderList();
  if(idx >= order.length) idx = 0;
  renderCard();
  renderType();
  renderList();
  renderOrder();
  renderScore();
  renderBlank();
  renderChunk();
  initSorder();
  initTopic();
  saveProgress();
}

document.getElementById('revealBtn').onclick = ()=>{
  const sentEl = document.getElementById('cardSent');
  sentEl.className = 'sent';
  sentEl.textContent = currentSentence();
  cardRevealed = true;
};
document.getElementById('koBtn').onclick = ()=>{
  const koEl = document.getElementById('cardKo');
  koEl.textContent = koEl.textContent ? '' : (currentKo()||'(이 본문은 해석이 아직 없어요)');
};
document.getElementById('hintBtn').onclick = ()=>{
  const sentEl = document.getElementById('cardSent');
  const s = currentSentence();
  sentEl.className = 'sent blank';
  sentEl.textContent = s.split(' ').map(w=>{
    const m = w.match(/^[A-Za-z']+/);
    if(!m) return w;
    return w[0] + '▢'.repeat(Math.max(m[0].length-1,1)) + w.slice(m[0].length);
  }).join(' ');
};
document.getElementById('shuffleBtn').onclick = shuffleOrder;
document.getElementById('nextBtn').onclick = ()=>{ idx=(idx+1)%order.length; renderCard(); saveProgress(); };
document.getElementById('prevBtn').onclick = ()=>{ idx=(idx-1+order.length)%order.length; renderCard(); saveProgress(); };
document.getElementById('typeNextBtn').onclick = ()=>{ idx=(idx+1)%order.length; renderType(); saveProgress(); };
document.getElementById('typePrevBtn').onclick = ()=>{ idx=(idx-1+order.length)%order.length; renderType(); saveProgress(); };
document.getElementById('orderNextBtn').onclick = ()=>{ idx=(idx+1)%order.length; renderOrder(); saveProgress(); };
document.getElementById('orderPrevBtn').onclick = ()=>{ idx=(idx-1+order.length)%order.length; renderOrder(); saveProgress(); };
document.getElementById('resetOrderBtn').onclick = renderOrder;

function normalize(s){ return s.toLowerCase().replace(/[—–\/]/g,' ').replace(/[.,!?"'’“”‘:;()$%\-]/g,'').replace(/\s+/g,' ').trim(); }

// LCS 기반 단어 정렬 diff: 단어 하나를 빠뜨려도 뒤 단어들이 전부 밀려서
// 오답 처리되지 않도록, 실제로 빠진 단어/틀린 단어만 정확히 찾아준다.
function diffWords(target, user){
  const n = target.length, m = user.length;
  const dp = Array.from({length:n+1}, ()=>new Array(m+1).fill(0));
  for(let i=n-1;i>=0;i--){
    for(let j=m-1;j>=0;j--){
      dp[i][j] = target[i]===user[j] ? dp[i+1][j+1]+1 : Math.max(dp[i+1][j], dp[i][j+1]);
    }
  }
  let i=0, j=0; const ops=[];
  while(i<n && j<m){
    if(target[i]===user[j]){ ops.push({type:'equal', word:target[i]}); i++; j++; }
    else if(dp[i+1][j] >= dp[i][j+1]){ ops.push({type:'missing', word:target[i]}); i++; }
    else { ops.push({type:'extra', word:user[j]}); j++; }
  }
  while(i<n){ ops.push({type:'missing', word:target[i]}); i++; }
  while(j<m){ ops.push({type:'extra', word:user[j]}); j++; }
  return ops;
}

document.getElementById('checkBtn').onclick = ()=>{
  const answer = currentSentence();
  const input = document.getElementById('typeInput').value;
  const a = normalize(answer).split(' ').filter(Boolean);
  const b = normalize(input).split(' ').filter(Boolean);
  const ops = diffWords(a, b);
  let correctWords = 0, missingCount = 0, extraCount = 0;
  let html = '';
  ops.forEach(op=>{
    if(op.type==='equal'){ correctWords++; html += `<span class="ok">${op.word}</span> `; }
    else if(op.type==='missing'){ missingCount++; html += `<span class="miss">[빠짐: ${op.word}]</span> `; }
    else { extraCount++; html += `<span class="bad">${op.word}</span> `; }
  });
  const acc = a.length ? Math.round((correctWords/a.length)*100) : 0;
  stats.total++; if(acc>=90) stats.ok++;
  logAttempt(curLesson, order[idx], acc>=90);
  document.getElementById('typeResult').innerHTML =
    `<div>정확도: <b>${acc}%</b> <span class="small">(맞은 단어 ${correctWords}/${a.length}, 빠진 단어 ${missingCount}개, 잘못 쓴/여분 단어 ${extraCount}개)</span></div>
     <div style="margin-top:6px;">${html}</div>
     <div style="margin-top:8px;" class="small">정답: ${answer}</div>`;
  document.getElementById('typeScore').textContent = `정답 ${stats.ok} / ${stats.total}`;
  saveProgress();
};
document.getElementById('revealTypeBtn').onclick = ()=>{
  document.getElementById('typeResult').innerHTML = `<div class="small">정답: ${currentSentence()}</div>`;
};

document.getElementById('checkOrderBtn').onclick = ()=>{
  orderChecked = true;
  const answerTokens = tokenize(currentSentence()).filter(w=>/[A-Za-z0-9]/.test(w));
  const userTokens = builtWords.map(i=>i.w);
  let correctCount = 0;
  const maxLen = Math.max(answerTokens.length, userTokens.length);
  let html = '';
  for(let i=0;i<maxLen;i++){
    const wa = answerTokens[i]; const wu = userTokens[i];
    if(wa && wa===wu){ correctCount++; html += `<span class="ok">${wu}</span> `; }
    else if(wu){ html += `<span class="extra" title="이 위치에 와야 할 단어가 아니에요">${wu}</span> `; }
  }
  const isPerfect = correctCount===answerTokens.length && userTokens.length===answerTokens.length;
  orderStats.total++; if(isPerfect) orderStats.ok++;
  logAttempt(curLesson, order[idx], isPerfect);
  document.getElementById('orderResult').innerHTML = isPerfect
    ? `<div class="ok">✅ 정확해요! (${correctCount}/${answerTokens.length})</div>`
    : `<div class="bad">✕ ${correctCount}/${answerTokens.length} 위치가 정확해요. 음영 표시된 단어의 위치를 다시 확인해보세요.</div>
       <div style="margin-top:6px;">${html}</div>
       <div class="small" style="margin-top:6px;">정답: ${currentSentence()}</div>`;
  document.getElementById('orderScore').textContent = `정답 ${orderStats.ok} / ${orderStats.total}`;
  saveProgress();
};
document.getElementById('revealOrderBtn').onclick = ()=>{
  document.getElementById('orderResult').innerHTML = `<div class="small">정답: ${currentSentence()}</div>`;
};

const PANELS={card:'cardPanel',order:'orderPanel',chunk:'chunkPanel',blank:'blankPanel',sorder:'sorderPanel',topic:'topicPanel',type:'typePanel',list:'listPanel',score:'scorePanel'};
document.querySelectorAll('.mode-btn').forEach(btn=>{
  btn.onclick = ()=>{
    document.querySelectorAll('.mode-btn').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    const mode = btn.dataset.mode;
    Object.keys(PANELS).forEach(k=>{ document.getElementById(PANELS[k]).style.display = k===mode ? '' : 'none'; });
    if(mode==='order') renderOrder();
    if(mode==='score') renderScore();
    if(mode==='blank') renderBlank();
    if(mode==='chunk') renderChunk();
    if(mode==='sorder') initSorder();
    if(mode==='topic') initTopic();
  };
});


// ===================== 신규 본문 (모의고사 / 올림포스) =====================
const PASSAGES_NEW = {
"모의고사": [
{t:"22번 · 낮은 확률의 과대평가", topic:3, s:[
"For each objective probability — from one percent to 100 percent — we want to know the subjective probability weight.",
"When we make decisions, do we treat a one-percent chance just like one percent, or like something else?",
"Events that could happen but aren't likely — low-probability events — can be defined as probabilities up to about 25 percent.",
"Low-probability events tend to factor more into decisions than they should. That is, an event that has an objective one-percent chance of occurring could subjectively seem like it has a five-percent chance of occurring.",
"We overestimate the likelihood of these low-probability events.",
"In general, the smaller the probability, the more we overestimate its likelihood.",
"For example, people who play the lottery are often optimistic about winning.",
"However, the odds of a single ticket winning the largest, most popular U.S. lotteries are on the order of one in 175 million."]},
{t:"30번 · 첨단 기술 제품의 가격 탄력성", topic:0, s:[
"Usually, innovative high-tech products have low price elasticity.",
"This means that high variation in price, an increase as well as a decrease, does not significantly modify demand.",
"These high-tech products have few substitutes, meaning that the costs to switch to another product are high.",
"Additionally, at the first stage of the technology, buyers — either innovators or forerunners — are less sensitive to price than to additional performance; they often have deep pockets or are ready to spend a lot for a new innovative and outstanding product.",
"For instance, with its new product range, a high-end mobile phone manufacturer is aiming at the 3% of millionaire households with assets of more than $2.5 million; those are customers who are ready to accept the price range the manufacturer has set.",
"At that stage, the competitors are not interested in lowering their price significantly whatsoever, because they are chasing the same categories of customers, not sensitive to price.",
"Furthermore, the high price of these products is often perceived as a sign of quality and reinforces customer's confidence in the company."]},
{t:"32번 · 식물과 접촉 (애기장대)", topic:0, s:[
"The genomics revolution made it possible to see just how impactful touch is to plants on a deeper level.",
"Peering at the genes of Arabidopsis thaliana, a weedy plant in the mustard family and the lab rat of the plant biology world, researchers saw that touch quietly triggered such a dramatic response in their hormones and gene expression that it could substantially inhibit their growth.",
"They stroked the arabidopsis with soft paintbrushes, and then analyzed the plants' genetic responses.",
"Within thirty minutes of being touched, 10 percent of the plant's genome was altered.",
"Clearly, the plant was reorganizing its priorities to deal with the disturbance, and rerouting the energy away from the hard work of getting taller.",
"Touched multiple times, arabidopsis cut its upward growth rate by as much as 30 percent."]},
{t:"33번 · 진화와 변화하는 환경", topic:2, s:[
"Evolution will come to a standstill until something in the conditions changes: the onset of an ice age, a change in the average rainfall of the area, a shift in the prevailing wind.",
"Such changes do happen when we are dealing with a timescale as long as the evolutionary one.",
"As a consequence, evolution normally does not come to a halt, but constantly tracks the changing environment.",
"If there is a steady downward drift in the average temperature in the area, a drift that persists over centuries, successive generations of animals will be propelled by a steady selection 'pressure' in the direction, say, of growing longer coats of hair.",
"If after a few thousand years of reduced temperature the trend reverses and average temperatures creep up again, the animals will come under the influence of a new selection pressure, and will be pushed towards growing shorter coats again."]},
{t:"34번 · 뉴욕의 핫도그 카트", topic:0, s:[
"The high price of land in metropolitan areas has implications for the efficient employment of resources.",
"For example, in New York City, as in many large cities, sidewalk vending carts sell everything from hot dogs to ice cream.",
"Why are these carts so popular, with over 3,000 in New York City alone? Consider the resources used to supply hot dogs: land, labor, capital, entrepreneurial ability, plus intermediate goods such as hot dogs, buns, and other ingredients.",
"Which of these do you suppose is most expensive in New York City?",
"Retail space along Madison Avenue rents for an average of $550 a year per square foot.",
"Because operating a hot dog cart requires about 4 square yards, it could cost as much as $20,000 a year to rent that much commercial space.",
"Aside from the necessary public permits, however, space on the public sidewalk is free to vendors. Profit-maximizing street vendors substitute public sidewalks for costly commercial space."]},
{t:"37번 · 얼어붙기 반응과 언어", topic:3, s:[
"Think about a time when you were startled. Did you come to a sudden stop, freeze all your movements, and hold your breath as you scanned the environment for a threat?",
"All these responses maximize our ability to avoid detection, locate the source of possible danger, and prepare to fight or flee.",
"As sophisticated as language has become, it is still the creation of sound that might give away our location to a potential predator.",
"Because of this, the freeze response results in the inhibition of language in highly stressful and traumatic situations.",
"Natural selection promoted the evolution of language but conserved the freeze rule.",
"When the threat passes, we begin to relax and find our voices again, perhaps even to laugh at our own reactions.",
"But what if we can never relax? What if our experiences shape our brain to be in a constant state of fear?",
"This interference with the proper development and integration of neural networks can result in chronic stress and even mental illness."]},
{t:"38번 · 폭력적 게임과 공감", topic:6, s:[
"Video gaming is one of the most studied technologies with regard to empathy.",
"Because there have been longstanding concerns about the impact of violent video games on youth, much research has been conducted over the last couple of decades on the topic.",
"While measuring and documenting the impact of video game violence, scientists have formulated credible explanations of how video gaming could affect empathy.",
"Part of a normal, healthy reaction to witnessing violent events is to feel negative emotions, and to experience physiological arousal that is connected to fear or disgust.",
"With repeated exposure to violent events—such as what might happen when a person plays violent video games—this normal reaction might become blunted, a phenomenon known as desensitization.",
"The next step in this problematic process would be having diminished emotional responses to violence, which could also interfere with the ability to recognize and/or to sympathize with others, leading to reduced empathy.",
"A significant body of work shows an association between extensive violent video gaming and reduced empathy in people."]},
{t:"39번 · 목적 세탁 (purpose-washing)", topic:0, s:[
"Realising the benefits of differentiation, some brands broadcast their social purpose before they do much actual good.",
"Their marketers are using social purpose to boost the brand's reputation, rather than embracing the purpose itself. Making a real difference with social problems is hard enough; to succeed, it has to follow from an authentic, company-wide commitment to the purpose.",
"Otherwise the mission is just window-dressing, or what we call “purpose-washing” — similar to green-washing, where purpose-driven activities serve mainly for publicity purposes.",
"We end up with what Anand Giridhardas describes in Winners Take All: “social purpose that just furthers the charade of elites disrupting the old order without giving much back”.",
"Purpose-washing brands gain one of the benefits of social purpose, differentiation, but only for a short while.",
"They lose out on positioning for future markets, and they might even make employees feel worse about working for them."]},
{t:"40번 · 음악과 기대의 위반", topic:3, s:[
"The brain needs to create a model of a constant pulse—a schema—so that we know when the musicians are not conforming to it.",
"This is similar to variations of a melody: We need to have a mental representation of what the melody is in order to know—and appreciate when the musician is taking liberties with it.",
"Metrical extraction, knowing what the pulse is and when we expect it to occur, is a crucial part of musical emotion.",
"Music communicates to us emotionally through systematic violations of expectations.",
"These violations can occur in any domain—the domain of pitch, rhythm, tempo, and so on—but occur they must.",
"Music is organized sound, but the organization has to involve some element of the unexpected or it is emotionally flat and robotic.",
"Too much organization may technically still be music, but it would be music that no one wants to listen to.",
"The brain models anticipated musical patterns, which creates the possibility for emotional impact to arise when musicians break these patterns, making music engaging."]}
],
"올림포스": [
{t:"1. 주식 시장의 무작위성 (p26)", topic:2, s:[
"“Human nature likes order,” wrote the economist Burton Malkiel in his seminal book A Random Walk Down Wall Street.",
"“People find it hard to accept the notion of randomness.”",
"Malkiel popularized the idea that the movement of any individual stock in the market is essentially random — it's impossible to know why a stock is doing what it's doing.",
"People who reliably make money from the market are those who own a diverse portfolio of different kinds of investments, which spreads out the risk, with the broader principle that the market, over the long haul, will eventually increase in value.",
"Picking individual stocks, or betting on certain trends, is much closer to gambling than science.",
"Which is why we shouldn't be too surprised that a cat is just as likely to make a killing on Wall Street as a day trader."]},
{t:"2. 과학적 방법과 의료직의 지위 (p32)", topic:0, s:[
"The introduction of scientific methods into medical practice transformed the profession as well as its object.",
"Until the late nineteenth century, doctors were not required to have studied medicine and were relied on mainly to provide comfort and guidance to their patients.",
"As the practice of medicine shifted from cure to prevention, doctors were now expected to provide results based on scientific evidence.",
"As a result of this access to forms of knowledge beyond the understanding of the general public, more authority and power was granted to the medical profession, and the nature of the doctor/patient relationship changed.",
"Once the source of a disease was identified, patients expected that doctors should be able to cure them.",
"Additionally, those doctors with scientific training were now distinguished from a range of alternative healers, from homeopaths to midwives, resulting in an elevation in the eyes of the public of the status of the profession as compared with other healing practices, which persists today."]},
{t:"3. 비례 대표제에 대한 우려 (p54)", topic:3, s:[
"Critics sometimes worry that by making it easier for small parties to win seats, proportional representation will encourage the growth of extremist groups standing on hateful or anti-democratic platforms.",
"Of course, no one committed to liberal and democratic values wants to see these kinds of parties taking seats in the legislature.",
"But it would be wrong to rig our political system to exclude them just because we disagree with their views.",
"Proportional voting systems provide a democratic vent for populist anger and discontent, creating clear incentives for mainstream parties to address underlying social problems and to win back votes.",
"We also have to remember that small parties can play a valuable role in highlighting specific issues that have been overlooked, as has often been the case with 'Green' parties.",
"In any case, the European experience suggests that there is no overall tendency for extremist parties to increase their numbers over time under proportional systems."]},
{t:"4. 자연법의 정의와 비판 (p62)", topic:0, s:[
"According to natural law theory, moral principles are not simply the result of human convention or social agreement, but are based on fundamental principles of nature, including human nature.",
"The term “natural law” refers to a set of ethical and moral principles that are thought to be inherent in the natural world and applicable to all human beings.",
"These principles are considered to be objective, universal, and immutable, and are often seen as a source of guidance for human behaviour.",
"Natural law theorists believe that the natural world operates according to a set of rational principles, and that these principles can be discovered through human reason and observation.",
"They argue that these principles provide a foundation for moral and legal systems, and that they are binding on all individuals, regardless of their cultural or social background.",
"Critics of natural law theory argue that it relies too heavily on unprovable assumptions about the existence of a divine purpose, and that it fails to account for the diversity of moral beliefs and practices across cultures and historical periods."]},
{t:"5. 협력과 비협조의 기준 (p70)", topic:1, s:[
"We not only absorb our moral codes and definitions of right and wrong from the group; the group also transmits cues about cooperation and defection and what it means to act in a trustworthy manner.",
"People are more likely to suppress their self-interest in favor of the group interest if they feel that others are doing so as well, and they're less likely to do so if they feel that others are taking advantage of them.",
"The psychological mechanism for this is unclear, but certainly it is related to our innate sense of fairness.",
"We generally don't mind sacrificing for the group, as long as we're all sacrificing equally.",
"But if we feel like we're being taken advantage of by others who are defecting, we're more likely to defect as well."]},
{t:"6. 인지 지도의 특징 (p76)", topic:4, s:[
"I suppose everyone has at one time or another drawn a mental map, and it offers little conceptual difficulty.",
"In my classes, I ask students to draw a map by hand, in just five minutes, showing their route to and from class.",
"No two maps are ever entirely the same, of course, and none are to scale.",
"Nevertheless, most of the maps are easily understood.",
"This shows that while we all produce our own versions of spatial reality, we can see particular landmarks that communicate to all of us in a social community.",
"Mental maps tend to highlight important parts of a route, with streets labeled to indicate where to turn.",
"Such maps tend to include informal but understood cultural references.",
"Where a professionally made street map might give you numbered addresses, a mental map is more likely to describe a route by referencing visible features like “a giant blue gorilla” outside a car dealership or “that old pink Victorian house.”"]},
{t:"7. 중학년 문해력 교육 (p82)", topic:0, s:[
"Literacy is crucial to the teaching-learning process that occurs in the middle grades because this is when young adolescents begin to move from narrative to expository text, a process that places increasing demands on the students' literacy skills.",
"Unfortunately, despite these increasing demands on their literacy skills, formal reading instruction ends for many young adolescents once they enter middle school.",
"One reason for this is that only about 50 percent of middle-grades teachers receive training in the teaching of literacy, broadly conceived as integrated reading, writing, speaking, and listening.",
"Fewer still receive specific training in programs such as writing across the curriculum.",
"Consequently, many teachers are less than ideally prepared to teach content-area literacy strategies to their students.",
"Given the increasing emphasis on integrated curricula in the middle grades, all teachers, regardless of the subjects they teach, are being called on to integrate the language arts into their subjects."]},
{t:"8. 영국의 주택 가격 (p90)", topic:2, s:[
"Houses in Britain are too expensive in relation to income for households to buy a house for ready money at the beginning of their housing career or accumulate the purchase money from prior savings.",
"Most householders must therefore either hire a house, or buy one with borrowed money.",
"Housing must therefore be financed, and the finance has to be for a long term.",
"For buyers using borrowed funds, long-term loans are necessary to ensure that the principal repayments can be spread out thinly enough to be covered by annual income.",
"The investor in rental properties often finds that the yearly rent only covers a small portion of the debt used to purchase the property."]},
{t:"9. 자녀를 위한 규칙 이해 교육 (p116)", topic:0, s:[
"In her book written for parents, Emmi Pikler illustrates with an instructive example how to draw children's attention to rules in a peaceful manner.",
"First, she suggests creating a safe play area for the children, suitable for their abilities and where nothing needs to be prohibited.",
"Then when children are moving around on all fours, they may leave this safe area in their mother's company to discover their environment.",
"Then the mother can draw their children's attention to the rules and prohibitions.",
"The nearby books look intriguing, and they are happy to hang on to the tablecloth too.",
"But then the mother repeats, “Don't do this!” If the children find this difficult to accept, put them back into their secure area where everything is allowed.",
"However, the children should not feel this as a kind of punishment but should feel that their mother trusts them.",
"“You are too young for these rules, but with time, this will change.”",
"These “walks” can be repeated from time to time.",
"This way, Pikler says, the world gradually unfolds before the children instead of shrinking (which is what happens when we ban something that they used to be allowed to do beforehand and that we may even have found amusing).",
"The children's world unfolds; and at the same time, they can understand more and more of these limitations and accept what the adult — gently but expressly — expects from them."]}
]};

const SECTIONS = {};
[["Lesson 1",[["The Story of Hip-Hop Music (p17)",0,17],["DJing·Breakdancing·MCing (p18)",17,27],["B-Boy와 MC의 탄생 (p19)",27,36],["The Origin of the Word Hip-Hop (p20)",36,47],["The Messages of Hip-Hop (p21)",47,55],["Respect (p22)",55,63,0],["Hip-Hop in the 21st Century (p23)",63,69]]],
 ["Lesson 2",[["Jiyun의 하루 (p45)",0,6],["Subscriptions are everywhere (p46)",6,17,6],["People love the subscriptions (p47)",17,28],["유연성과 편리함 (p48)",28,36,0],["Online platforms (p49)",36,43],["Limitations (p50)",43,52,0],["환경 문제와 결론 (p51)",52,58]]]
].forEach(([k,arr])=>{ SECTIONS[k]=arr.map(([t,a,b,topic])=>({t,a,b,topic})); });
Object.keys(PASSAGES_NEW).forEach(k=>{
  DATA[k]=[]; SECTIONS[k]=[];
  PASSAGES_NEW[k].forEach(p=>{ const a=DATA[k].length; p.s.forEach(x=>DATA[k].push(x)); SECTIONS[k].push({t:p.t,a,b:DATA[k].length,topic:p.topic}); });
});

// ===================== 공통 유틸 =====================
function shuffle(a){ for(let i=a.length-1;i>0;i--){ const j=Math.floor(Math.random()*(i+1)); [a[i],a[j]]=[a[j],a[i]]; } return a; }
function koLine(){ const k=currentKo(); return k ? '해석: '+k : ''; }
function secList(){ return SECTIONS[curLesson]||[]; }
// 쌓아 둔 칩을 드래그/선택-교체/×로 정렬·제거할 수 있게 그림
function drawBuilt(box, list, sel, cls, onRemove, marks){
  box.innerHTML='';
  const redraw=()=>drawBuilt(box,list,sel,cls,onRemove,null);
  list.forEach((item,pos)=>{
    const chip=document.createElement('span');
    chip.className=cls+(sel.pos===pos?' sel':'')+(marks&&marks[pos]?' '+marks[pos]:'');
    chip.draggable=true;
    const lab=document.createElement('span'); lab.textContent=item.w; chip.appendChild(lab);
    const x=document.createElement('b'); x.className='xbtn'; x.textContent='×'; x.title='빼기';
    x.onclick=(e)=>{ e.stopPropagation(); sel.pos=null; onRemove(pos); };
    chip.appendChild(x);
    chip.onclick=()=>{
      if(sel.pos===null) sel.pos=pos;
      else if(sel.pos===pos) sel.pos=null;
      else { const t=list[sel.pos]; list[sel.pos]=list[pos]; list[pos]=t; sel.pos=null; }
      redraw();
    };
    chip.ondragstart=(e)=>{ e.dataTransfer.setData('text/plain', String(pos)); };
    chip.ondragover=(e)=>e.preventDefault();
    chip.ondrop=(e)=>{ e.preventDefault(); const d=e.dataTransfer.getData('text/plain'); if(d==='') return; const from=Number(d); if(from===pos) return; const m=list.splice(from,1)[0]; list.splice(pos,0,m); sel.pos=null; redraw(); };
    box.appendChild(chip);
  });
}
function moveSel(list, sel, d, redrawFn){
  const p=sel.pos, q=p+d; if(q<0||q>=list.length) return;
  const t=list[p]; list[p]=list[q]; list[q]=t; sel.pos=q; redrawFn();
}
let orderSel={pos:null};

// ===================== 빈칸 채우기 (단어 / 문법) =====================
const GRAM=new Set("that which who whom whose where when while if as than because although though whether to of in on at for by with from into about over under between during through is are was were be been being have has had do does did can could will would should may might must not and but or so no both either such what whoever whatever there it they their this these those".split(' '));
let blankKind='vocab', blankChecked=false, blankStats={ok:0,total:0};
function blankInputs(){ return Array.prototype.slice.call(document.querySelectorAll('#blankSent input.bl')); }
function renderBlank(){
  document.getElementById('blankProgress').textContent=`${curLesson} · ${idx+1} / ${order.length}`;
  document.getElementById('blankScore').textContent=blankStats.total?`정답 ${blankStats.ok} / ${blankStats.total}`:'';
  document.getElementById('blankKo').textContent=koLine();
  document.getElementById('blankResult').innerHTML='';
  blankChecked=false;
  const parts=currentSentence().match(/[A-Za-z]+(?:'[A-Za-z]+)?|[^A-Za-z]+/g)||[];
  const wi=[]; parts.forEach((p,i)=>{ if(/^[A-Za-z]/.test(p)) wi.push(i); });
  let cands = blankKind==='gram' ? wi.filter(i=>GRAM.has(parts[i].toLowerCase())) : wi.filter(i=>parts[i].length>=7 && !GRAM.has(parts[i].toLowerCase()));
  if(!cands.length) cands = wi.filter(i=>parts[i].length>=(blankKind==='gram'?2:4));
  shuffle(cands);
  const n=Math.max(1,Math.min(6,Math.round(cands.length*0.5)));
  const pick=new Set(cands.slice(0,n));
  const box=document.getElementById('blankSent'); box.innerHTML='';
  const answers=[];
  parts.forEach((p,i)=>{
    if(pick.has(i)){
      const inp=document.createElement('input'); inp.className='bl'; inp.dataset.ans=p; inp.autocomplete='off'; inp.spellcheck=false;
      inp.style.width=Math.max(4,p.length+1)+'ch';
      inp.addEventListener('keydown',e=>{ if(e.key==='Enter'){ e.preventDefault(); if(blankChecked) blankMove(1); else checkBlank(); } });
      box.appendChild(inp); answers.push(p);
    } else box.appendChild(document.createTextNode(p));
  });
  const bank=document.getElementById('blankBank'); bank.innerHTML='';
  shuffle(answers.slice()).forEach(w=>{
    const c=document.createElement('span'); c.className='wchip'; c.textContent=w;
    c.onclick=()=>{ if(c.classList.contains('used')||blankChecked) return; const t=blankInputs().filter(i=>!i.value.trim())[0]; if(!t) return; t.value=w; c.classList.add('used'); const nx=blankInputs().filter(i=>!i.value.trim())[0]; (nx||t).focus(); };
    bank.appendChild(c);
  });
  if(document.getElementById('blankPanel').style.display!=='none'){ const f=blankInputs()[0]; if(f) f.focus(); }
}
function checkBlank(){
  if(blankChecked) return; blankChecked=true;
  const ins=blankInputs(); let ok=0; const wrong=[];
  ins.forEach(inp=>{ const a=inp.dataset.ans, v=inp.value.trim();
    if(v.toLowerCase()===a.toLowerCase()){ inp.classList.add('ok'); ok++; }
    else { inp.classList.add('bad'); wrong.push((v||'(빈칸)')+' → '+a); inp.value=a; } });
  const all=ok===ins.length; blankStats.total++; if(all) blankStats.ok++;
  logAttempt(curLesson, order[idx], all); saveProgress();
  document.getElementById('blankScore').textContent=`정답 ${blankStats.ok} / ${blankStats.total}`;
  document.getElementById('blankResult').innerHTML= all
    ? `<div class="ok">✅ 전부 맞았어요! (${ok}/${ins.length})</div>`
    : `<div style="color:var(--bad);font-weight:bold;">✕ ${ok}/${ins.length} 정답</div><div class="small" style="margin-top:6px;">틀린 곳 (내 답 → 정답): ${wrong.join(' · ')}</div>`;
}
function blankMove(d){ idx=(idx+d+order.length)%order.length; renderBlank(); saveProgress(); }
document.getElementById('blankKind').onchange=function(){ blankKind=this.value; renderBlank(); };
document.getElementById('blankRedo').onclick=renderBlank;
document.getElementById('blankCheck').onclick=checkBlank;
document.getElementById('blankReveal').onclick=()=>{ blankInputs().forEach(i=>{ i.value=i.dataset.ans; }); };
document.getElementById('blankPrev').onclick=()=>blankMove(-1);
document.getElementById('blankNext').onclick=()=>blankMove(1);

// ===================== 문장 순서 배열 =====================
let sSec=0, sBank=[], sBuilt=[], sSel={pos:null}, sChecked=false, sStats={ok:0,total:0};
function initSorder(){
  const sel=document.getElementById('secSelect'); sel.innerHTML='';
  secList().forEach((s,i)=>{ const o=document.createElement('option'); o.value=String(i); o.textContent=s.t; sel.appendChild(o); });
  if(sSec>=secList().length) sSec=0;
  sel.value=String(sSec); renderSorder();
}
function renderSorder(){
  const sec=secList()[sSec]; if(!sec) return;
  sBank=DATA[curLesson].slice(sec.a,sec.b).map((w,i)=>({w,i,used:false}));
  if(sBank.length>1){ let tries=0; do{ shuffle(sBank); tries++; } while(sBank.every((x,k)=>x.i===k)&&tries<10); }
  sBuilt=[]; sSel.pos=null; sChecked=false;
  document.getElementById('sorderProgress').textContent=`${curLesson} · ${sSec+1} / ${secList().length} · ${sBank.length}문장`;
  document.getElementById('sorderScore').textContent=sStats.total?`정답 ${sStats.ok} / ${sStats.total}`:'';
  document.getElementById('sResult').innerHTML='';
  drawSorder();
}
function sRemove(pos){ sBuilt[pos].used=false; sBuilt.splice(pos,1); drawSorder(); }
function drawSorder(marks){
  const bank=document.getElementById('sBank'); bank.innerHTML='';
  sBank.forEach(it=>{ const c=document.createElement('div'); c.className='schip'+(it.used?' used':''); c.textContent=it.w;
    c.onclick=()=>{ if(it.used) return; it.used=true; sBuilt.push(it); drawSorder(); }; bank.appendChild(c); });
  drawBuilt(document.getElementById('sBuiltBox'), sBuilt, sSel, 'schip', sRemove, marks);
}
function checkSorder(){
  const marks=sBuilt.map((it,pos)=>it.i===pos?'cok':'cbad');
  const right=marks.filter(m=>m==='cok').length;
  const good=sBuilt.length===sBank.length && right===sBank.length;
  if(!sChecked){ sStats.total++; if(good) sStats.ok++; }
  sChecked=true;
  document.getElementById('sorderScore').textContent=`정답 ${sStats.ok} / ${sStats.total}`;
  drawSorder(marks);
  document.getElementById('sResult').innerHTML= good ? '<div class="ok">✅ 순서가 정확해요!</div>'
    : `<div style="color:var(--bad);font-weight:bold;">✕ ${right}/${sBank.length}개가 제자리예요</div><div class="small" style="margin-top:4px;">초록 테두리 = 맞는 자리 · 주황 테두리 = 틀린 자리${sBuilt.length<sBank.length?' · 아직 안 쌓은 문장이 있어요':''}</div>`;
}
document.getElementById('secSelect').onchange=function(){ sSec=Number(this.value); renderSorder(); };
document.getElementById('sorderRedo').onclick=renderSorder;
document.getElementById('sCheck').onclick=checkSorder;
document.getElementById('sReveal').onclick=()=>{ sChecked=true; document.getElementById('sResult').innerHTML='<ol class="ans">'+sBank.slice().sort((a,b)=>a.i-b.i).map(x=>'<li>'+x.w+'</li>').join('')+'</ol>'; };
document.getElementById('sPrev').onclick=()=>navigate(-1);
document.getElementById('sNext').onclick=()=>navigate(1);

// ===================== 주제문 찾기 =====================
let tSec=0, tAnswered=false, tStats={ok:0,total:0};
function topicSecs(){ return secList().map((s,i)=>Object.assign({i:i},s)).filter(s=>typeof s.topic==='number'&&s.topic>=0); }
function initTopic(){
  const sel=document.getElementById('topicSelect'); sel.innerHTML='';
  const secs=topicSecs();
  secs.forEach((s,k)=>{ const o=document.createElement('option'); o.value=String(k); o.textContent=s.t; sel.appendChild(o); });
  if(tSec>=secs.length) tSec=0;
  sel.value=String(tSec); renderTopic();
}
function renderTopic(){
  const secs=topicSecs(), box=document.getElementById('topicBox'); box.innerHTML='';
  document.getElementById('topicResult').innerHTML=''; tAnswered=false;
  document.getElementById('topicScore').textContent=tStats.total?`정답 ${tStats.ok} / ${tStats.total}`:'';
  if(!secs.length){ document.getElementById('topicProgress').textContent=''; box.innerHTML='<div class="small">이 교재에는 주제문 문제가 거의 없어요. 모의고사·올림포스 탭에서 풀어보세요.</div>'; return; }
  const sec=secs[tSec];
  document.getElementById('topicProgress').textContent=`${curLesson} · ${tSec+1} / ${secs.length}`;
  DATA[curLesson].slice(sec.a,sec.b).forEach((sent,k)=>{ const d=document.createElement('div'); d.className='trow'; d.textContent=(k+1)+'. '+sent; d.onclick=()=>answerTopic(k); box.appendChild(d); });
}
function answerTopic(k){
  const secs=topicSecs(); if(!secs.length||tAnswered) return;
  const rows=Array.prototype.slice.call(document.querySelectorAll('#topicBox .trow')); if(k>=rows.length) return;
  const sec=secs[tSec]; tAnswered=true;
  const ok=k===sec.topic; tStats.total++; if(ok) tStats.ok++;
  rows[sec.topic].classList.add('right'); if(!ok) rows[k].classList.add('wrong');
  document.getElementById('topicScore').textContent=`정답 ${tStats.ok} / ${tStats.total}`;
  document.getElementById('topicResult').innerHTML= ok ? '<div class="ok">✅ 정답!</div>' : `<div style="color:var(--bad);font-weight:bold;">✕ 정답은 ${sec.topic+1}번 문장이에요 (초록색)</div>`;
}
document.getElementById('topicSelect').onchange=function(){ tSec=Number(this.value); renderTopic(); };
document.getElementById('topicPrev').onclick=()=>navigate(-1);
document.getElementById('topicNext').onclick=()=>navigate(1);


// ===================== 덩어리(구·절) 순서 배열 =====================
const K_CONJ=new Set("that which who whom whose where when while because although though if whether and but or so as than until since once unless".split(' '));
const K_PREP=new Set("to of in on at for by with from into about over under between during through after before without among across despite like".split(' '));
function chunkify(sent, level){
  const toks=[];
  sent.split(/\s+/).filter(Boolean).forEach(w=>{
    if(/^[—–-]+$/.test(w)){ if(toks.length){ toks[toks.length-1].t+=' '+w; toks[toks.length-1].brk=true; } return; }
    toks.push({t:w, brk:/[,;:]["”’']?$/.test(w)||/[—–]$/.test(w)});
  });
  const cfg={small:[2,2,3,3],normal:[2,3,5,3],large:[3,999,9,4]}[level]||[2,3,5,3];
  const conjMin=cfg[0], prepMin=cfg[1], cap=cfg[2], hard=cap+cfg[3];
  let chunks=[], cur=[];
  toks.forEach(tk=>{
    const w=tk.t.toLowerCase().replace(/[^a-z']/g,'');
    if(cur.length>0 && ((K_CONJ.has(w)&&cur.length>=conjMin)||(K_PREP.has(w)&&cur.length>=prepMin)||(cur.length>=cap&&(K_PREP.has(w)||K_CONJ.has(w)))||cur.length>=hard)){ chunks.push(cur.join(' ')); cur=[]; }
    cur.push(tk.t);
    if(tk.brk){ chunks.push(cur.join(' ')); cur=[]; }
  });
  if(cur.length) chunks.push(cur.join(' '));
  const wc=c=>c.split(' ').length;
  for(let i=0;i<chunks.length;){ // 한 단어짜리는 다음(마지막이면 앞) 덩어리에 합침
    if(chunks.length>1 && wc(chunks[i])<=1){ if(i<chunks.length-1){ chunks[i+1]=chunks[i]+' '+chunks[i+1]; chunks.splice(i,1); } else { chunks[i-1]+=' '+chunks[i]; chunks.pop(); } } else i++;
  }
  if(chunks.length===1){ const ws=chunks[0].split(' '); if(ws.length>=3){ const h=Math.ceil(ws.length/2); chunks=[ws.slice(0,h).join(' '), ws.slice(h).join(' ')]; } }
  return chunks;
}
let kBank=[], kBuilt=[], kSel={pos:null}, kChunks=[], kChecked=false, kStats={ok:0,total:0}, kLevel='normal';
function renderChunk(){
  kChunks=chunkify(currentSentence(), kLevel);
  kBank=kChunks.map((w,i)=>({w,i,used:false}));
  if(kBank.length>1){ let t=0; do{ shuffle(kBank); t++; } while(kBank.every((x,k)=>x.w===kChunks[k])&&t<10); }
  kBuilt=[]; kSel.pos=null; kChecked=false;
  document.getElementById('chunkProgress').textContent=`${curLesson} · ${idx+1} / ${order.length} · ${kChunks.length}덩어리`;
  document.getElementById('chunkScore').textContent=kStats.total?`정답 ${kStats.ok} / ${kStats.total}`:'';
  document.getElementById('chunkKo').textContent=koLine();
  document.getElementById('chunkResult').innerHTML='';
  drawChunk();
}
function kRemove(pos){ kBuilt[pos].used=false; kBuilt.splice(pos,1); drawChunk(); }
function drawChunk(marks){
  const bank=document.getElementById('kBank'); bank.innerHTML='';
  kBank.forEach(it=>{ const c=document.createElement('span'); c.className='kchip'+(it.used?' used':''); c.textContent=it.w;
    c.onclick=()=>{ if(it.used) return; it.used=true; kBuilt.push(it); drawChunk(); }; bank.appendChild(c); });
  drawBuilt(document.getElementById('kBuiltBox'), kBuilt, kSel, 'kchip', kRemove, marks);
}
function checkChunk(){
  const marks=kBuilt.map((it,pos)=>kChunks[pos]===it.w?'cok':'cbad');
  const right=marks.filter(m=>m==='cok').length;
  const good=kBuilt.length===kChunks.length && right===kChunks.length;
  if(!kChecked){ kStats.total++; if(good) kStats.ok++; logAttempt(curLesson, order[idx], good); saveProgress(); }
  kChecked=true;
  document.getElementById('chunkScore').textContent=`정답 ${kStats.ok} / ${kStats.total}`;
  drawChunk(marks);
  document.getElementById('chunkResult').innerHTML= good ? '<div class="ok">✅ 정확해요!</div>'
    : `<div style="color:var(--bad);font-weight:bold;">✕ ${right}/${kChunks.length}개가 제자리예요</div><div class="small" style="margin-top:4px;">초록 테두리 = 맞는 자리 · 주황 테두리 = 틀린 자리${kBuilt.length<kChunks.length?' · 아직 안 쌓은 덩어리가 있어요':''}</div>`;
}
function chunkMove(d){ idx=(idx+d+order.length)%order.length; renderChunk(); saveProgress(); }
document.getElementById('kSize').onchange=function(){ kLevel=this.value; renderChunk(); };
document.getElementById('kCheck').onclick=checkChunk;
document.getElementById('kReset').onclick=renderChunk;
document.getElementById('kReveal').onclick=()=>{ kChecked=true; document.getElementById('chunkResult').innerHTML='<div class="small">정답: '+kChunks.join(' / ')+'</div><div style="margin-top:4px;">'+currentSentence()+'</div>'; };
document.getElementById('kPrev').onclick=()=>chunkMove(-1);
document.getElementById('kNext').onclick=()=>chunkMove(1);

loadProgress();
resetOrderList();
renderAll();
</script>
</body>
</html>
