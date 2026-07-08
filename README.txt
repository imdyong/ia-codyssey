<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LLM 프롬프트 엔지니어링 미션</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,500;9..144,600;9..144,700&family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --paper:#EFF1F6;
    --paper-raised:#FBFBFD;
    --ink:#1B2136;
    --ink-soft:#4A5170;
    --line:#D7DAE6;
    --indigo:#3B4EA6;
    --indigo-deep:#232C5C;
    --amber:#DE9A32;
    --mint:#2F8F76;
    --mint-bg:#E4F3EE;
    --rose:#C24A56;
    --rose-bg:#FBEAEC;
    --mono:'JetBrains Mono', monospace;
    --serif:'Fraunces', serif;
    --sans:'Inter', sans-serif;
  }
  *{box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  body{
    margin:0;
    background:var(--paper);
    color:var(--ink);
    font-family:var(--sans);
    line-height:1.7;
    -webkit-font-smoothing:antialiased;
  }

  /* progress bar */
  #progress{
    position:fixed; top:0; left:0; height:3px; width:0%;
    background:linear-gradient(90deg,var(--indigo),var(--amber));
    z-index:200; transition:width .1s linear;
  }

  /* nav */
  nav{
    position:sticky; top:0; z-index:100;
    background:rgba(239,241,246,.9);
    backdrop-filter:blur(8px);
    border-bottom:1px solid var(--line);
    padding:14px 0;
  }
  .nav-inner{
    max-width:840px; margin:0 auto; padding:0 28px;
    display:flex; align-items:center; gap:22px;
    overflow-x:auto; scrollbar-width:none;
  }
  .nav-inner::-webkit-scrollbar{display:none;}
  .nav-brand{
    font-family:var(--mono); font-weight:700; font-size:13px;
    color:var(--indigo-deep); white-space:nowrap;
    letter-spacing:.02em;
  }
  .nav-links{display:flex; gap:16px;}
  .nav-links a{
    font-family:var(--mono); font-size:12px; color:var(--ink-soft);
    text-decoration:none; white-space:nowrap; padding:4px 2px;
    border-bottom:2px solid transparent; transition:.15s;
  }
  .nav-links a:hover, .nav-links a.active{
    color:var(--indigo-deep); border-bottom-color:var(--amber);
  }

  /* hero */
  header.hero{
    max-width:840px; margin:0 auto; padding:88px 28px 56px;
  }
  .eyebrow{
    font-family:var(--mono); font-size:12px; letter-spacing:.14em;
    text-transform:uppercase; color:var(--indigo);
    display:flex; align-items:center; gap:10px; margin-bottom:22px;
  }
  .eyebrow::before{
    content:''; width:22px; height:1px; background:var(--indigo);
  }
  h1.title{
    font-family:var(--serif); font-weight:600; font-size:clamp(34px,5vw,54px);
    line-height:1.12; margin:0 0 20px; color:var(--indigo-deep);
    letter-spacing:-.01em;
  }
  h1.title em{
    font-style:italic; color:var(--amber); font-weight:500;
  }
  .hero-sub{
    font-size:17px; color:var(--ink-soft); max-width:560px; margin:0 0 28px;
  }
  .tags{display:flex; flex-wrap:wrap; gap:8px;}
  .tag{
    font-family:var(--mono); font-size:11px; padding:5px 10px;
    background:var(--paper-raised); border:1px solid var(--line);
    border-radius:20px; color:var(--ink-soft);
  }
  .quote-console{
    margin:44px 0 0; background:var(--indigo-deep); color:#DCE1F4;
    border-radius:10px; padding:22px 24px; font-family:var(--mono);
    font-size:13.5px; line-height:1.85; position:relative;
    box-shadow:0 12px 30px -14px rgba(35,44,92,.5);
  }
  .quote-console::before{
    content:'● ● ●'; display:block; color:#5A6499; letter-spacing:4px;
    font-size:10px; margin-bottom:14px;
  }
  .quote-console .prompt-line::before{content:'❯ '; color:var(--amber);}
  .quote-console .answer{color:#9AA6D6; display:block; margin-top:10px;}
  .quote-console .bold{color:#F4E3C1; font-weight:600;}

  /* main container */
  main{max-width:840px; margin:0 auto; padding:0 28px 40px;}

  section.chapter{
    padding:64px 0; border-top:1px solid var(--line);
    display:grid; grid-template-columns:64px 1fr; gap:28px;
  }
  section.chapter:first-of-type{border-top:none;}
  .ch-num{
    font-family:var(--serif); font-size:52px; font-weight:600;
    color:var(--line); line-height:1; user-select:none;
  }
  .ch-body h2{
    font-family:var(--serif); font-size:28px; font-weight:600;
    margin:0 0 6px; color:var(--indigo-deep);
  }
  .ch-body .ch-kicker{
    font-family:var(--mono); font-size:12px; color:var(--amber);
    text-transform:uppercase; letter-spacing:.08em; margin-bottom:22px; display:block;
  }
  .ch-body h3{
    font-family:var(--sans); font-weight:700; font-size:16px;
    color:var(--indigo-deep); margin:38px 0 12px;
    display:flex; align-items:center; gap:8px;
  }
  .ch-body h3 .num{
    font-family:var(--mono); font-size:12px; color:#fff; background:var(--indigo);
    padding:2px 7px; border-radius:5px;
  }
  .ch-body p{margin:0 0 14px; color:var(--ink); font-size:15px;}
  .ch-body ul, .ch-body ol{margin:0 0 14px; padding-left:20px; font-size:15px;}
  .ch-body li{margin-bottom:6px;}
  .callout{
    background:var(--paper-raised); border-left:3px solid var(--indigo);
    padding:14px 18px; border-radius:0 8px 8px 0; font-size:14px;
    color:var(--ink-soft); margin:16px 0;
  }
  .callout.warn{border-left-color:var(--amber); }
  .callout b{color:var(--ink);}

  /* deliverable cards */
  .deliver-grid{display:grid; gap:14px; margin-top:8px;}
  .deliver-card{
    background:var(--paper-raised); border:1px solid var(--line);
    border-radius:12px; padding:20px 22px; position:relative;
  }
  .deliver-card .d-icon{
    font-family:var(--mono); font-size:11px; color:var(--indigo);
    background:#E7EAF7; display:inline-block; padding:3px 9px;
    border-radius:6px; margin-bottom:10px; letter-spacing:.04em;
  }
  .deliver-card h4{margin:0 0 6px; font-family:var(--serif); font-size:19px; color:var(--indigo-deep);}
  .deliver-card p{margin:0; font-size:14px; color:var(--ink-soft);}

  /* checklist */
  .checklist{list-style:none; padding:0; margin:16px 0;}
  .checklist li{
    display:flex; gap:12px; align-items:flex-start;
    padding:11px 0; border-bottom:1px dashed var(--line); font-size:14.5px;
  }
  .checklist li::before{
    content:'✓'; font-family:var(--mono); font-weight:700; color:var(--mint);
    background:var(--mint-bg); width:20px; height:20px; border-radius:5px;
    display:flex; align-items:center; justify-content:center; font-size:12px;
    flex-shrink:0; margin-top:1px;
  }

  /* table */
  table{width:100%; border-collapse:collapse; margin:14px 0 20px; font-size:14px;}
  th{
    text-align:left; font-family:var(--mono); font-size:11px; text-transform:uppercase;
    letter-spacing:.05em; color:var(--ink-soft); border-bottom:1px solid var(--ink);
    padding:8px 10px;
  }
  td{padding:10px 10px; border-bottom:1px solid var(--line); vertical-align:top;}
  tr:last-child td{border-bottom:none;}

  /* pass/fail badges */
  .badge{
    font-family:var(--mono); font-size:11px; font-weight:700; padding:3px 9px;
    border-radius:5px; display:inline-block;
  }
  .badge.pass{background:var(--mint-bg); color:var(--mint);}
  .badge.fail{background:var(--rose-bg); color:var(--rose);}

  /* code / template block */
  .code-block{
    background:var(--indigo-deep); color:#D4D9EF; font-family:var(--mono);
    font-size:13px; line-height:1.8; padding:18px 20px; border-radius:8px;
    margin:14px 0; overflow-x:auto; white-space:pre-wrap;
  }
  .code-block .field{color:var(--amber);}

  /* diff block - signature element for v1/v2 comparisons */
  .diff{
    border:1px solid var(--line); border-radius:10px; overflow:hidden;
    margin:16px 0; font-family:var(--mono); font-size:13px;
  }
  .diff-head{
    background:var(--ink); color:#DDE1F0; padding:9px 16px; font-size:11.5px;
    display:flex; justify-content:space-between; letter-spacing:.03em;
  }
  .diff-row{display:flex; border-top:1px solid var(--line);}
  .diff-row:first-child{border-top:none;}
  .diff-mark{
    width:34px; flex-shrink:0; text-align:center; padding:10px 0;
    font-weight:700;
  }
  .diff-row.minus{background:var(--rose-bg);}
  .diff-row.minus .diff-mark{color:var(--rose);}
  .diff-row.plus{background:var(--mint-bg);}
  .diff-row.plus .diff-mark{color:var(--mint);}
  .diff-text{padding:10px 14px 10px 0; flex:1; color:var(--ink);}

  /* chat transcript - signature element for dialogue */
  .chat{margin:16px 0; display:flex; flex-direction:column; gap:10px;}
  .bubble{
    max-width:78%; padding:11px 15px; border-radius:14px; font-size:14px;
  }
  .bubble .who{
    font-family:var(--mono); font-size:10px; text-transform:uppercase;
    letter-spacing:.06em; opacity:.65; display:block; margin-bottom:3px;
  }
  .bubble.user{
    align-self:flex-end; background:var(--indigo); color:#fff;
    border-bottom-right-radius:3px;
  }
  .bubble.ai{
    align-self:flex-start; background:var(--paper-raised); color:var(--ink);
    border:1px solid var(--line); border-bottom-left-radius:3px;
  }
  .chat .ellipsis{
    align-self:center; font-family:var(--mono); color:var(--ink-soft); font-size:12px; padding:2px 0;
  }

  /* details / accordion */
  details{
    background:var(--paper-raised); border:1px solid var(--line);
    border-radius:10px; margin:12px 0; padding:0; overflow:hidden;
  }
  details summary{
    padding:14px 18px; cursor:pointer; font-weight:600; font-size:14.5px;
    color:var(--indigo-deep); list-style:none; display:flex;
    align-items:center; justify-content:space-between;
  }
  details summary::-webkit-details-marker{display:none;}
  details summary::after{
    content:'+'; font-family:var(--mono); font-size:16px; color:var(--amber);
    transition:transform .2s;
  }
  details[open] summary::after{transform:rotate(45deg);}
  details .d-inner{padding:0 18px 18px;}
  details[open] summary{border-bottom:1px solid var(--line);}

  /* bonus cards */
  .bonus-grid{display:grid; grid-template-columns:1fr 1fr; gap:14px; margin-top:8px;}
  .bonus-card{
    background:linear-gradient(155deg,var(--indigo-deep),#2E3A78);
    color:#EDEFFA; border-radius:12px; padding:22px;
  }
  .bonus-card .b-tag{
    font-family:var(--mono); font-size:10px; color:var(--amber);
    letter-spacing:.08em; text-transform:uppercase; margin-bottom:8px; display:block;
  }
  .bonus-card h4{margin:0 0 8px; font-family:var(--serif); font-size:18px;}
  .bonus-card p{margin:0; font-size:13.5px; color:#C3C9EA;}

  footer{
    max-width:840px; margin:0 auto; padding:50px 28px 90px;
    border-top:1px solid var(--line); text-align:center;
  }
  footer .fnote{
    font-family:var(--mono); font-size:12px; color:var(--ink-soft);
  }
  footer .fmain{
    font-family:var(--serif); font-size:22px; color:var(--indigo-deep); margin:6px 0 14px;
  }

  @media(max-width:640px){
    section.chapter{grid-template-columns:1fr; gap:8px;}
    .ch-num{font-size:32px;}
    .bonus-grid{grid-template-columns:1fr;}
    .bubble{max-width:90%;}
  }
</style>
</head>
<body>

<div id="progress"></div>

<nav>
  <div class="nav-inner">
    <span class="nav-brand">MISSION.md</span>
    <div class="nav-links">
      <a href="#ch1">01 소개</a>
      <a href="#ch2">02 결과물</a>
      <a href="#ch3">03 목표</a>
      <a href="#ch4">04 요구사항</a>
      <a href="#ch5">05 보너스</a>
      <a href="#ch6">06 제약</a>
      <a href="#ch7">07 예시</a>
    </div>
  </div>
</nav>

<header class="hero">
  <span class="eyebrow">Prompt Engineering Mission</span>
  <h1 class="title">"그냥 물어보는 것"과<br><em>"제대로 시키는 것"</em>의 차이</h1>
  <p class="hero-sub">LLM의 작동 원리를 이해하고, 프롬프트 설계·검증·개선 역량을 실전 과업으로 증명하는 미션입니다.</p>
  <div class="tags">
    <span class="tag">3+ LLM 비교</span>
    <span class="tag">시스템 프롬프트 설계</span>
    <span class="tag">Few-shot ×3</span>
    <span class="tag">v1 → v2 개선</span>
    <span class="tag">환각 검증</span>
    <span class="tag">10턴+ 대화 로그</span>
  </div>

  <div class="quote-console">
    <span class="prompt-line">"ChatGPT한테 물어봤는데, 생각보다 별로던데요?"</span>
    <span class="answer">분명 세상을 바꿀 혁명이라는데, 막상 내 업무를 맡기면 겉도는 말만 반복하는 AI.<br>옆자리 동료는 같은 AI로 보고서 초안까지 뚝딱 해치운다. 뭐가 다를까?</span>
    <span class="answer bold">→ 답은 간단하다. 질문을 어떻게 설계했느냐, 그 차이다.</span>
  </div>
</header>

<main>

  <!-- ================= CH 1 ================= -->
  <section class="chapter" id="ch1">
    <div class="ch-num">01</div>
    <div class="ch-body">
      <span class="ch-kicker">Overview</span>
      <h2>미션 소개</h2>
      <p>같은 모델이라도 프롬프트 한 줄이 달라지면 결과물의 수준은 완전히 바뀝니다. <b>'그냥 물어보는 것'과 '제대로 시키는 것' 사이의 간극을 직접 체감하는 것</b>이 이번 미션의 출발점입니다.</p>
      <p>이번 미션에서는 대규모 언어 모델(LLM)의 작동 원리를 이해하고, 프롬프트 엔지니어링을 통해 원하는 텍스트 결과물을 정교하게 제어하는 기술을 익힙니다.</p>

      <table>
        <tr><th>단계</th><th>활동</th></tr>
        <tr><td>1</td><td>3종 이상의 LLM을 동일한 업무 과업에 투입해 성능 비교 → 평가 축과 근거로 최적 모델 선정</td></tr>
        <tr><td>2</td><td>특정 페르소나를 가진 AI 챗봇의 시스템 프롬프트 직접 설계</td></tr>
        <tr><td>3</td><td>Few-shot 예시와 단계적 추론 유도 기법을 적용해 프롬프트를 버전별로 개선</td></tr>
        <tr><td>4</td><td>10턴 이상 대화에서 문맥을 유지하는 텍스트 생성 파이프라인 구축</td></tr>
        <tr><td>5</td><td>환각(Hallucination)을 체계적으로 검증</td></tr>
      </table>

      <p>이 과정을 마치면, 여러분은 AI를 단순한 검색 대용이 아니라 <b>업무 생산성을 끌어올리는 핵심 파트너</b>로 다룰 수 있게 됩니다. 이 미션에서 쌓은 프롬프트 설계·검증·개선 역량은 향후 멀티모달 콘텐츠 제작과 AI 서비스 개발의 튼튼한 밑거름이 될 것입니다.</p>
    </div>
  </section>

  <!-- ================= CH 2 ================= -->
  <section class="chapter" id="ch2">
    <div class="ch-num">02</div>
    <div class="ch-body">
      <span class="ch-kicker">Deliverables</span>
      <h2>최종 결과물</h2>
      <p>다음 3가지 산출물을 포함한 업무 자동화 프롬프트 패키지 1개를 완성합니다.</p>

      <div class="deliver-grid">
        <div class="deliver-card">
          <span class="d-icon">DOC · 01</span>
          <h4>LLM 모델 비교·선정 보고서</h4>
          <p>동일한 업무 과업 프롬프트(테스트 질문/케이스 포함)를 3종 이상의 모델에 입력하고, 모델별 결과 비교표와 최종 선정 결론(근거 포함)을 정리한 문서.</p>
        </div>
        <div class="deliver-card">
          <span class="d-icon">DOC · 02</span>
          <h4>시스템 설계 문서</h4>
          <p>타겟 사용자, 업무 문제, 페르소나, 시스템/유저 프롬프트, Few-shot 예시, 검증 전략을 포함하며, 프롬프트 버전(v1→v2) 개선 이력과 최종 프롬프트 전문을 담은 문서.</p>
        </div>
        <div class="deliver-card">
          <span class="d-icon">LOG · 03</span>
          <h4>실행 로그 (대화 로그)</h4>
          <p>선택한 업무 과업 시나리오로 10턴 이상 실제 대화를 수행한 전문과, 문제 발생 지점(환각/문맥 끊김 등) 및 수정 결과 요약을 담은 로그.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- ================= CH 3 ================= -->
  <section class="chapter" id="ch3">
    <div class="ch-num">03</div>
    <div class="ch-body">
      <span class="ch-kicker">Learning Outcomes</span>
      <h2>과제 목표</h2>
      <p>이 과제를 마친 후, 학습자는 아래를 스스로 설명할 수 있어야 합니다.</p>
      <ul class="checklist">
        <li>LLM이 "확률적 생성" 기반으로 동작하며, 왜 환각(Hallucination)이 발생하는지 말로 설명할 수 있다.</li>
        <li>Zero-shot / Few-shot / 단계적 추론 유도(Reasoning Prompt) 기법의 차이를 구분하고, 업무 과업에 맞게 적용할 수 있다.</li>
        <li>3종 이상의 LLM을 비교할 때, "느낌"이 아니라 평가 축과 근거를 통해 선택 이유를 설명할 수 있다.</li>
        <li>페르소나와 시스템 프롬프트가 결과 품질(일관성/안전성/형식 준수)에 미치는 영향을 설명할 수 있다.</li>
        <li>생성 결과를 테스트로 검증하고(특히 사실 오류), 프롬프트를 반복 개선하는 과정을 재현 가능하게 설명할 수 있다.</li>
      </ul>
    </div>
  </section>

  <!-- ================= CH 4 ================= -->
  <section class="chapter" id="ch4">
    <div class="ch-num">04</div>
    <div class="ch-body">
      <span class="ch-kicker">Requirements — all must be satisfied</span>
      <h2>기능 요구 사항</h2>

      <h3><span class="num">4-1</span> 모델 비교 및 선정 (최소 3종)</h3>
      <p>서로 다른 3개 이상의 LLM을 선정해 동일한 업무 과업을 수행시킵니다.<br>
      <span style="color:var(--ink-soft); font-size:13.5px;">예: GPT-4o, Claude 3.5 Sonnet, Gemini 1.5 Pro (또는 동급/유사 목적의 최신 모델)</span></p>
      <table>
        <tr><th>필수 항목</th><th>설명</th></tr>
        <tr><td>비교 대상 모델명</td><td>실제 테스트한 모델</td></tr>
        <tr><td>사용 환경</td><td>웹 / 앱 / API 중 무엇인지</td></tr>
        <tr><td>평가 축</td><td>최소 4개 + 모델별 점수(1~5) + 근거</td></tr>
        <tr><td>최종 선정 결론</td><td>"왜 이 모델이 업무 과업에 최적"인지 3줄 이상 근거로 설명</td></tr>
      </table>

      <h3><span class="num">4-2</span> 업무 자동화 과업 1개 선택 및 문제 정의</h3>
      <p>아래 예시 중 1개 이상을 선택하거나, 본인 업무에 준하는 과업을 정의합니다.</p>
      <ul>
        <li>메일 초안 작성 (요청/상황/톤 지정)</li>
        <li>회의록 요약 (결정사항/Action Items 추출)</li>
        <li>보고서 개요 생성 (목차/핵심 메시지/리스크)</li>
        <li>요구사항 정리 (사용자 스토리/수용 기준)</li>
      </ul>
      <p>선택한 과업에 대해 다음을 문서에 명시합니다: <b>타겟 사용자</b> / <b>입력 데이터 형태</b> / <b>출력 규격</b>. 재사용 가능한 "입력 템플릿"을 1개 이상 제시합니다.</p>
      <div class="code-block">[<span class="field">목적</span>] [<span class="field">대상</span>] [<span class="field">핵심 포인트</span>] [<span class="field">톤</span>] [<span class="field">금지어</span>] [<span class="field">필수 포함 항목</span>]</div>

      <h3><span class="num">4-3</span> 페르소나 정의 및 시스템 프롬프트 설계</h3>
      <p><b>페르소나</b>를 구체적으로 정의합니다 — 이름 / 역할(직무) / 전문 분야 / 말투 / 금지 사항 / 우선순위(정확성 vs 친절함 등).</p>
      <p><b>시스템 프롬프트</b>에는 최소 아래 요소가 포함되어야 합니다.</p>
      <ol>
        <li>역할(페르소나)과 목표(업무 과업)</li>
        <li>출력 형식 규칙 (예: 제목/불릿/표현 톤)</li>
        <li>안전장치 (모르면 모른다고 말하기, 확인 질문하기)</li>
        <li>사실/정책/수치가 포함될 때의 처리 규칙 (근거 제시 또는 확인 필요 표기)</li>
      </ol>

      <h3><span class="num">4-4</span> Few-shot 예시 (최소 3개)</h3>
      <p>동일 과업에 대해 "좋은 입력 → 좋은 출력" 예시를 최소 3개 제공합니다.</p>
      <div class="callout warn"><b>필수 조건</b> — 예시 3개 중 1개는 "모호한 입력" 케이스로 구성하고, AI가 되물어야 하는 지점을 보여줘야 합니다.</div>

      <h3><span class="num">4-5</span> 단계적 추론 유도 적용 및 전/후 비교</h3>
      <p>같은 과업 입력에 대해 v1(일반 지시)과 v2(단계적 접근 유도)를 비교합니다. 단, 최종 출력에는 장문의 추론 과정이 그대로 노출되지 않도록 <b>"최종 답변은 요약/근거 중심"</b> 규칙을 함께 둡니다.</p>

      <div class="diff">
        <div class="diff-head"><span>prompt.diff</span><span>v1 → v2</span></div>
        <div class="diff-row minus"><div class="diff-mark">−</div><div class="diff-text">아래 회의 내용을 요약해줘.</div></div>
        <div class="diff-row plus"><div class="diff-mark">+</div><div class="diff-text">① 먼저 목표 출력 항목(결정/할일/리스크)을 선언한다</div></div>
        <div class="diff-row plus"><div class="diff-mark">+</div><div class="diff-text">② 정보가 부족하면 최대 3개까지 확인 질문을 한다</div></div>
        <div class="diff-row plus"><div class="diff-mark">+</div><div class="diff-text">③ 마지막에 템플릿 형식으로 출력한다</div></div>
        <div class="diff-row plus"><div class="diff-mark">+</div><div class="diff-text">④ 최종 답변에는 핵심 근거 3개만 짧게 bullet로 덧붙인다</div></div>
      </div>

      <h3><span class="num">4-6</span> 환각(Hallucination) 검증</h3>
      <div class="callout"><b>정의</b> — "사실/수치/정책/계산처럼 정답 또는 검증 가능한 근거가 있어야 하는 질문에서, 근거 없이 틀린 정보를 확신하는 답변." 창의적 글쓰기(소설/카피)는 환각으로 보지 않되, "허구"임을 명시하거나 세계관 규칙을 지켜야 합니다.</div>
      <p>사실 기반 검증 질문 5개 이상을 준비하고, <b>기대 정답</b> / <b>허용 오차</b> / <b>참고 근거</b>를 함께 기록합니다.</p>
      <table>
        <tr><th>판정</th><th>기준</th></tr>
        <tr><td><span class="badge pass">PASS</span></td><td>정답을 맞히고 근거를 간단히 제시한다 <b>또는</b> 모르면 "모른다/확인 필요"를 명시하고 확인 절차를 제안한다</td></tr>
        <tr><td><span class="badge fail">FAIL</span></td><td>틀린 사실을 근거 없이 단정한다 <b>또는</b> 전제가 불명확한데도 확인 질문 없이 임의로 사실을 만들어 답한다</td></tr>
      </table>

      <h3><span class="num">4-7</span> 10턴 이상 대화 로그 + 문맥 유지 검증</h3>
      <p>선택한 업무 과업 시나리오로 10턴 이상 대화를 수행하고 전문을 남깁니다. 아래 중 1개 이상을 반드시 포함합니다.</p>
      <ul>
        <li>중간에 조건 변경 (톤/대상/정책 변경)</li>
        <li>추가 정보가 뒤늦게 제공됨 (첨부 내용/제약 조건)</li>
      </ul>
      <div class="callout">"문맥 유지"는 다음을 모두 만족하면 Pass — 이전에 확정된 요구사항(톤, 대상, 금지어 등)을 10턴 내내 일관되게 유지하고, 조건 변경이 들어오면 변경된 부분만 반영하고 나머지는 유지.</div>

      <h3><span class="num">4-8</span> 제출물 최소 목차 (형식 자유, 항목은 필수)</h3>
      <div class="code-block">📁 <span class="field">모델 비교·선정 보고서</span>
 ├─ 비교 목적 / 동일 입력 / 평가 축(점수표)
 ├─ 모델별 결과 요약
 └─ 최종 선정 근거

📁 <span class="field">시스템 설계 문서</span>
 ├─ 문제 정의 / 타겟 사용자 / 입력 템플릿
 ├─ 페르소나 / 시스템 프롬프트 / Few-shot
 ├─ 환각 검증 설계
 └─ 개선 이력 (v1 → v2)

📁 <span class="field">실행 로그(대화 로그)</span>
 ├─ 시나리오 설명
 ├─ 10턴 이상 대화 전문
 └─ 문제 지점과 개선 요약</div>
    </div>
  </section>

  <!-- ================= CH 5 ================= -->
  <section class="chapter" id="ch5">
    <div class="ch-num">05</div>
    <div class="ch-body">
      <span class="ch-kicker">Optional</span>
      <h2>보너스 과제</h2>
      <div class="bonus-grid">
        <div class="bonus-card">
          <span class="b-tag">Bonus 01 · 재사용성</span>
          <h4>나만의 봇 배포</h4>
          <p>커스텀 인스트럭션 또는 GPTs/에이전트 기능 등으로 최종 프롬프트를 "재사용 가능한 형태"로 배포해봅니다.</p>
        </div>
        <div class="bonus-card">
          <span class="b-tag">Bonus 02 · 시각화</span>
          <h4>멀티모달 확장</h4>
          <p>생성된 결과(보고서/기획/요약)를 바탕으로 이미지 생성 AI를 활용해 1장 이상의 시각 자료를 만들어봅니다.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- ================= CH 6 ================= -->
  <section class="chapter" id="ch6">
    <div class="ch-num">06</div>
    <div class="ch-body">
      <span class="ch-kicker">Constraints</span>
      <h2>제약 사항</h2>

      <h3><span class="num">6-1</span> 모델/환경 기록 (재현성)</h3>
      <table>
        <tr><th>기록 항목</th><th>예시</th></tr>
        <tr><td>모델명</td><td>GPT-4o, Claude 3.5 Sonnet 등</td></tr>
        <tr><td>요금제</td><td>무료 / 유료</td></tr>
        <tr><td>사용 채널</td><td>웹 / 앱 / API</td></tr>
        <tr><td>사용 날짜</td><td>YYYY-MM-DD</td></tr>
        <tr><td>주요 설정</td><td>temperature / top_p / max_tokens 등 (가능한 범위)</td></tr>
      </table>
      <div class="callout warn">무료 버전 사용 시 <b>"무료라서 제한된 점"</b>을 1줄로 명시합니다.</div>

      <h3><span class="num">6-2</span> 안전 / 윤리</h3>
      <ul>
        <li>폭력적/선정적 콘텐츠 생성을 시도하지 않는다.</li>
        <li>개인정보(실명, 전화번호, 주소, 계정정보 등)를 프롬프트/로그에 직접 포함하지 않는다.</li>
        <li>사실/정책/수치가 포함되는 답변은 "근거/확인 필요" 규칙을 반드시 따른다.</li>
      </ul>

      <h3><span class="num">6-3</span> 제출물 편집</h3>
      <p>대화 로그를 그대로 덤프하지 말고 가독성을 위해 섹션/번호/요약을 추가해 정리합니다. 단, <b>"원본 로그(전문)"</b>는 별도 파일로라도 남겨 재현 가능하게 합니다.</p>
    </div>
  </section>

  <!-- ================= CH 7 ================= -->
  <section class="chapter" id="ch7">
    <div class="ch-num">07</div>
    <div class="ch-body">
      <span class="ch-kicker">Reference only — not the answer key</span>
      <h2>결과 예시</h2>
      <p>아래는 정답이 아니라 참고 예시입니다. 실제 문구와 구성은 달라도 됩니다.</p>

      <details open>
        <summary>모델 비교 보고서 일부</summary>
        <div class="d-inner">
          <table>
            <tr><th>#</th><th>평가 축</th></tr>
            <tr><td>1</td><td>정확성 (사실/계산)</td></tr>
            <tr><td>2</td><td>한국어 자연스러움</td></tr>
            <tr><td>3</td><td>긴 문맥 (10턴) 유지</td></tr>
            <tr><td>4</td><td>형식 준수 (템플릿 출력)</td></tr>
            <tr><td>5</td><td>속도/안정성</td></tr>
            <tr><td>6</td><td>비용 (요금제/토큰)</td></tr>
            <tr><td>7</td><td>도구 연동 적합성</td></tr>
          </table>
          <div class="callout">결론(예시) — "회의록 요약" 과업은 긴 문맥 유지와 형식 준수가 중요하므로, 해당 축 점수가 높은 모델을 채택.</div>
        </div>
      </details>

      <details>
        <summary>업무 과업 입력 템플릿</summary>
        <div class="d-inner">
          <div class="code-block">[업무 과업] 회의록 요약
[원문] (회의 메모/녹취 요약 텍스트)
[원하는 결과] 결정사항 / Action Items / 리스크 / 후속 일정
[톤] 간결, 사내 공유용
[금지] 추측성 표현 금지, 인물 실명 대신 역할명 사용
[확인 질문 규칙] 정보가 부족하면 최대 3개까지 질문 후 요약 시작</div>
        </div>
      </details>

      <details>
        <summary>페르소나 및 시스템 프롬프트 요약</summary>
        <div class="d-inner">
          <p><b>페르소나:</b> "프로젝트 매니저 출신 업무 자동화 코치"</p>
          <ol>
            <li>사실/수치/정책은 확인 가능해야 함</li>
            <li>부족하면 질문</li>
            <li>최종 출력은 템플릿 준수</li>
          </ol>
        </div>
      </details>

      <details>
        <summary>Few-shot 예시 1개</summary>
        <div class="d-inner">
          <div class="chat">
            <div class="bubble user"><span class="who">User</span>회의 메모가 너무 길어. 결정사항과 할 일만 뽑아줘. 대신 확실한 내용만.</div>
            <div class="bubble ai"><span class="who">AI</span>- 결정사항: …<br>- Action Items: 담당/기한/작업 …<br>- 확인 필요: … (근거 부족 항목)</div>
          </div>
        </div>
      </details>

      <details>
        <summary>단계적 추론 유도 프롬프트 전/후</summary>
        <div class="d-inner">
          <div class="diff">
            <div class="diff-head"><span>example.diff</span><span>v1 → v2</span></div>
            <div class="diff-row minus"><div class="diff-mark">−</div><div class="diff-text">아래 회의 내용을 요약해줘.</div></div>
            <div class="diff-row plus"><div class="diff-mark">+</div><div class="diff-text">먼저 목표 출력 항목(결정/할일/리스크)을 선언</div></div>
            <div class="diff-row plus"><div class="diff-mark">+</div><div class="diff-text">정보 부족하면 최대 3개 확인 질문</div></div>
            <div class="diff-row plus"><div class="diff-mark">+</div><div class="diff-text">마지막에 템플릿으로 출력</div></div>
            <div class="diff-row plus"><div class="diff-mark">+</div><div class="diff-text">최종 답변에는 핵심 근거 3개만 짧게 덧붙이기</div></div>
          </div>
        </div>
      </details>

      <details>
        <summary>환각 검증 기록</summary>
        <div class="d-inner">
          <p><b>Q.</b> "(업무 규정) 출장비 정산 규정의 예외 조건이 뭐야?"</p>
          <table>
            <tr><th>판정</th><th>답변 예시</th></tr>
            <tr><td><span class="badge pass">PASS</span></td><td>"규정 문서가 없으면 확답 불가. 사내 위키/규정 PDF의 '예외' 섹션을 확인해야 함. 확인 후 요약 제공 가능."</td></tr>
            <tr><td><span class="badge fail">FAIL</span></td><td>"예외는 무조건 … 입니다." (근거 없음)</td></tr>
          </table>
        </div>
      </details>

      <details>
        <summary>10턴 대화 로그</summary>
        <div class="d-inner">
          <div class="chat">
            <div class="bubble user"><span class="who">Turn 1 · User</span>(회의록 원문 제공)</div>
            <div class="bubble ai"><span class="who">Turn 2 · AI</span>확인 질문 2개 (누락 정보)</div>
            <div class="bubble user"><span class="who">Turn 3 · User</span>추가 정보 제공</div>
            <div class="bubble ai"><span class="who">Turn 4 · AI</span>템플릿 요약 출력</div>
            <div class="ellipsis">⋯ (5~8턴 생략)</div>
            <div class="bubble user"><span class="who">Turn 9 · User</span>"톤을 더 단호하게, 책임자를 역할명으로 바꿔줘"</div>
            <div class="bubble ai"><span class="who">Turn 10 · AI</span>변경 반영 + 기존 결정사항 유지</div>
          </div>
        </div>
      </details>
    </div>
  </section>

</main>

<footer>
  <span class="fnote">// end of mission brief</span>
  <div class="fmain">Good luck 🚀</div>
  <span class="fnote">prompt-engineering-mission · v1.0</span>
</footer>

<script>
  // reading progress bar
  const progress = document.getElementById('progress');
  window.addEventListener('scroll', () => {
    const h = document.documentElement;
    const scrolled = (h.scrollTop) / (h.scrollHeight - h.clientHeight) * 100;
    progress.style.width = scrolled + '%';
  });

  // active nav link on scroll
  const sections = document.querySelectorAll('section.chapter');
  const links = document.querySelectorAll('.nav-links a');
  const io = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if(entry.isIntersecting){
        links.forEach(l => l.classList.remove('active'));
        const active = document.querySelector('.nav-links a[href="#' + entry.target.id + '"]');
        if(active) active.classList.add('active');
      }
    });
  }, { rootMargin: '-40% 0px -55% 0px' });
  sections.forEach(s => io.observe(s));
</script>

</body>
</html>
