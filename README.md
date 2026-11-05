<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Behind the Veil of Ignorance - Identity Lottery</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .card-flip {
      perspective: 1000px;
    }
    .card-inner {
      position: relative;
      width: 100%;
      height: 100%;
      text-align: center;
      transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
      transform-style: preserve-3d;
    }
    .card-flipped {
      transform: rotateY(180deg);
    }
    .card-front, .card-back {
      position: absolute;
      width: 100%;
      height: 100%;
      -webkit-backface-visibility: hidden;
      backface-visibility: hidden;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      border-radius: 0.75rem;
    }
    .card-back {
      transform: rotateY(180deg);
    }
    @keyframes pulse-ring {
      0% { transform: scale(0.95); opacity: 0.8; }
      50% { transform: scale(1.05); opacity: 1; }
      100% { transform: scale(0.95); opacity: 0.8; }
    }
    .pulse-glow {
      animation: pulse-ring 2s infinite ease-in-out;
    }
  </style>
</head>
<body class="bg-slate-900 min-h-screen flex items-center justify-center p-3 font-sans text-slate-800">

  <div class="max-w-md w-full bg-slate-50 rounded-3xl shadow-2xl overflow-hidden border border-slate-700">
    
    <!-- 최상단 헤더 -->
    <div class="bg-gradient-to-r from-indigo-700 via-indigo-600 to-purple-700 text-white p-5 text-center relative shadow-md">
      <div class="inline-block px-2.5 py-0.5 bg-indigo-900/60 rounded-full text-[10px] tracking-widest uppercase font-semibold text-indigo-200 border border-indigo-400/30 mb-1">
        SDG 10 &amp; 11 Inquiry Activity
      </div>
      <h1 class="text-xl font-black tracking-tight">Behind the Veil of Ignorance</h1>
      <p class="text-xs text-indigo-200 mt-0.5">무지의 베일: 당신은 어떤 조건으로 태어나시겠습니까?</p>
      
      <!-- 점수 디스플레이 (게임 시작 시 활성화) -->
      <div id="scoreContainer" class="hidden mt-3 bg-slate-900/70 p-2.5 rounded-xl border border-white/10 flex items-center justify-between px-4">
        <span class="text-xs font-semibold text-slate-300">Life Security Score (삶의 안정도)</span>
        <div id="scoreDisplay" class="text-2xl font-black text-amber-300">50 pts</div>
      </div>
    </div>

    <!-- 메인 컨테이너 -->
    <div class="p-5">
      
      <!-- ================= 1단계: 신분 추첨 룰렛/카드 선택 화면 ================= -->
      <div id="lotteryScreen" class="space-y-4">
        <div class="text-center">
          <h2 class="text-sm font-bold text-slate-800">Draw Your 6 Social Identities</h2>
          <p class="text-[11px] text-slate-500">6개의 물음표 카드를 각각 터치하거나, 아래 버튼을 눌러 무작위 추첨하세요.</p>
        </div>

        <!-- 6개 신분 카드 그리드 (2열 3행) -->
        <div class="grid grid-cols-2 gap-2.5" id="cardGrid">
          
          <!-- 카드 1: 나이 -->
          <div class="card-flip h-28 cursor-pointer" onclick="flipSingleCard('age')">
            <div id="cardInner-age" class="card-inner">
              <div class="card-front bg-indigo-50 border-2 border-dashed border-indigo-300 hover:border-indigo-500 hover:bg-indigo-100/60 transition p-2">
                <span class="text-xl mb-0.5">🎂</span>
                <span class="text-[11px] font-bold text-indigo-900">Age (나이)</span>
                <span class="text-[9px] text-indigo-400 font-medium mt-1">Tap to reveal</span>
              </div>
              <div id="cardBack-age" class="card-back bg-white border border-indigo-200 shadow-sm p-2 text-center">
                <span class="text-[10px] font-extrabold text-indigo-600 uppercase">Age</span>
                <p id="textEn-age" class="text-xs font-bold text-slate-800 mt-1 leading-tight"></p>
                <p id="textKo-age" class="text-[10px] text-slate-500 mt-0.5"></p>
              </div>
            </div>
          </div>

          <!-- 카드 2: 성별 -->
          <div class="card-flip h-28 cursor-pointer" onclick="flipSingleCard('gender')">
            <div id="cardInner-gender" class="card-inner">
              <div class="card-front bg-indigo-50 border-2 border-dashed border-indigo-300 hover:border-indigo-500 hover:bg-indigo-100/60 transition p-2">
                <span class="text-xl mb-0.5">⚧️</span>
                <span class="text-[11px] font-bold text-indigo-900">Gender (성별)</span>
                <span class="text-[9px] text-indigo-400 font-medium mt-1">Tap to reveal</span>
              </div>
              <div id="cardBack-gender" class="card-back bg-white border border-indigo-200 shadow-sm p-2 text-center">
                <span class="text-[10px] font-extrabold text-indigo-600 uppercase">Gender</span>
                <p id="textEn-gender" class="text-xs font-bold text-slate-800 mt-1 leading-tight"></p>
                <p id="textKo-gender" class="text-[10px] text-slate-500 mt-0.5"></p>
              </div>
            </div>
          </div>

          <!-- 카드 3: 출신/이주배경 -->
          <div class="card-flip h-28 cursor-pointer" onclick="flipSingleCard('background')">
            <div id="cardInner-background" class="card-inner">
              <div class="card-front bg-indigo-50 border-2 border-dashed border-indigo-300 hover:border-indigo-500 hover:bg-indigo-100/60 transition p-2">
                <span class="text-xl mb-0.5">🌐</span>
                <span class="text-[11px] font-bold text-indigo-900">Background (배경)</span>
                <span class="text-[9px] text-indigo-400 font-medium mt-1">Tap to reveal</span>
              </div>
              <div id="cardBack-background" class="card-back bg-white border border-indigo-200 shadow-sm p-2 text-center">
                <span class="text-[10px] font-extrabold text-indigo-600 uppercase">Background</span>
                <p id="textEn-background" class="text-[11px] font-bold text-slate-800 mt-1 leading-tight"></p>
                <p id="textKo-background" class="text-[9px] text-slate-500 mt-0.5"></p>
              </div>
            </div>
          </div>

          <!-- 카드 4: 경제력 -->
          <div class="card-flip h-28 cursor-pointer" onclick="flipSingleCard('wealth')">
            <div id="cardInner-wealth" class="card-inner">
              <div class="card-front bg-indigo-50 border-2 border-dashed border-indigo-300 hover:border-indigo-500 hover:bg-indigo-100/60 transition p-2">
                <span class="text-xl mb-0.5">💰</span>
                <span class="text-[11px] font-bold text-indigo-900">Wealth (경제력)</span>
                <span class="text-[9px] text-indigo-400 font-medium mt-1">Tap to reveal</span>
              </div>
              <div id="cardBack-wealth" class="card-back bg-white border border-indigo-200 shadow-sm p-2 text-center">
                <span class="text-[10px] font-extrabold text-indigo-600 uppercase">Wealth</span>
                <p id="textEn-wealth" class="text-[11px] font-bold text-slate-800 mt-1 leading-tight"></p>
                <p id="textKo-wealth" class="text-[9px] text-slate-500 mt-0.5"></p>
              </div>
            </div>
          </div>

          <!-- 카드 5: 건강 및 장애 -->
          <div class="card-flip h-28 cursor-pointer" onclick="flipSingleCard('disability')">
            <div id="cardInner-disability" class="card-inner">
              <div class="card-front bg-indigo-50 border-2 border-dashed border-indigo-300 hover:border-indigo-500 hover:bg-indigo-100/60 transition p-2">
                <span class="text-xl mb-0.5">♿</span>
                <span class="text-[11px] font-bold text-indigo-900">Health (건강/장애)</span>
                <span class="text-[9px] text-indigo-400 font-medium mt-1">Tap to reveal</span>
              </div>
              <div id="cardBack-disability" class="card-back bg-white border border-indigo-200 shadow-sm p-2 text-center">
                <span class="text-[10px] font-extrabold text-indigo-600 uppercase">Health</span>
                <p id="textEn-disability" class="text-[11px] font-bold text-slate-800 mt-1 leading-tight"></p>
                <p id="textKo-disability" class="text-[9px] text-slate-500 mt-0.5"></p>
              </div>
            </div>
          </div>

          <!-- 카드 6: 주거 환경 -->
          <div class="card-flip h-28 cursor-pointer" onclick="flipSingleCard('housing')">
            <div id="cardInner-housing" class="card-inner">
              <div class="card-front bg-indigo-50 border-2 border-dashed border-indigo-300 hover:border-indigo-500 hover:bg-indigo-100/60 transition p-2">
                <span class="text-xl mb-0.5">🏡</span>
                <span class="text-[11px] font-bold text-indigo-900">Housing (주거환경)</span>
                <span class="text-[9px] text-indigo-400 font-medium mt-1">Tap to reveal</span>
              </div>
              <div id="cardBack-housing" class="card-back bg-white border border-indigo-200 shadow-sm p-2 text-center">
                <span class="text-[10px] font-extrabold text-indigo-600 uppercase">Housing</span>
                <p id="textEn-housing" class="text-[11px] font-bold text-slate-800 mt-1 leading-tight"></p>
                <p id="textKo-housing" class="text-[9px] text-slate-500 mt-0.5"></p>
              </div>
            </div>
          </div>

        </div>

        <!-- 버튼 영역 -->
        <div class="space-y-2 pt-2">
          <button onclick="revealAllCardsRandomly()" class="w-full bg-slate-800 hover:bg-slate-900 text-white font-semibold py-2.5 rounded-xl text-xs transition duration-150 flex items-center justify-center gap-1.5 shadow">
            <span>🎲</span> Spin All Randomly (한 번에 모두 뽑기)
          </button>
          <button id="startSimBtn" disabled onclick="enterSimulationWorld()" class="w-full bg-indigo-400 text-white font-bold py-3 rounded-xl text-xs transition duration-150 shadow-md cursor-not-allowed">
            Reveal All 6 Cards to Start (6개를 모두 뽑아야 시작됩니다)
          </button>
        </div>
      </div>

      <!-- ================= 2단계: 상황 시뮬레이션 진행 화면 ================= -->
      <div id="simScreen" class="hidden space-y-4">
        
        <!-- 내 페르소나 아코디언 바 (간략 요약) -->
        <div class="bg-indigo-50/80 border border-indigo-100 rounded-xl p-2.5 text-[11px] text-slate-700">
          <div class="font-bold text-indigo-900 mb-1 flex items-center justify-between">
            <span>👤 My Persona (나의 확정된 조건)</span>
            <span class="text-[9px] bg-indigo-200 text-indigo-800 px-1.5 py-0.2 rounded font-semibold">Fixed</span>
          </div>
          <div id="personaSummaryBar" class="grid grid-cols-2 gap-x-2 gap-y-0.5 text-[10px] text-slate-600"></div>
        </div>

        <!-- 상황 진행 카드 -->
        <div class="bg-white rounded-2xl p-4 border border-slate-200 shadow-sm">
          <div class="flex items-center gap-2 mb-1.5">
            <span id="stepBadge" class="text-[10px] font-bold bg-indigo-100 px-2 py-0.5 rounded text-indigo-700"></span>
            <h3 id="eventTitle" class="text-xs font-bold text-slate-900"></h3>
          </div>
          <p id="eventTitleKo" class="text-[11px] text-slate-500 mb-2"></p>
          
          <div class="bg-slate-50 p-3 rounded-xl border border-slate-200/80 mb-3 text-xs leading-relaxed">
            <p id="eventDesc" class="font-medium text-slate-800"></p>
            <p id="eventDescKo" class="text-slate-500 mt-1 text-[11px]"></p>
          </div>

          <!-- 판정 결과 박스 -->
          <div id="resultBox" class="hidden p-3 rounded-xl text-xs leading-relaxed transition-all mb-3">
            <p id="resultEn" class="font-bold"></p>
            <p id="resultKo" class="text-[11px] mt-0.5 opacity-90"></p>
          </div>

          <!-- 조작 버튼 -->
          <button id="resolveBtn" onclick="resolveEvent()" class="w-full bg-slate-900 hover:bg-black text-white font-semibold py-2.5 rounded-xl text-xs transition shadow">
            Check the Outcome (결과 확인하기)
          </button>
          <button id="nextBtn" onclick="nextEvent()" class="hidden w-full bg-indigo-600 hover:bg-indigo-700 text-white font-semibold py-2.5 rounded-xl text-xs transition shadow">
            Next Situation (다음 상황으로) ➔
          </button>
        </div>
      </div>

      <!-- ================= 3단계: 최종 결과 & 성찰 (Debriefing) ================= -->
      <div id="endScreen" class="hidden space-y-4">
        <div class="text-center py-2">
          <div class="text-4xl mb-1">🏁</div>
          <h2 class="text-base font-extrabold text-slate-900">Simulation Complete</h2>
          <p class="text-xs text-slate-500">최종 결과 및 성찰</p>
          <div id="finalScore" class="text-3xl font-black text-indigo-600 mt-1.5"></div>
          <p id="finalStatus" class="text-xs font-semibold mt-1"></p>
        </div>

        <div class="bg-amber-50 border border-amber-200 rounded-2xl p-4 text-xs space-y-3">
          <h4 class="font-bold text-amber-900 flex items-center gap-1.5">
            <span>💡</span> Reflective Questions (생각해볼 점):
          </h4>
          <div class="space-y-2.5">
            <div>
              <p class="font-bold text-slate-800">1. Personal Effort vs. Luck?</p>
              <p class="text-[11px] text-slate-600">당신이 점수를 얻거나 잃은 것은 개인의 노력 때문이었나요, 아니면 처음 뽑은 운 때문이었나요?</p>
            </div>
            <div>
              <p class="font-bold text-slate-800">2. The Hardest Barrier?</p>
              <p class="text-[11px] text-slate-600">당신의 페르소나에게 가장 극복하기 힘들었던 사회적 장벽(교통, 주거, 언어, 의료비 등)은 무엇이었나요?</p>
            </div>
            <div>
              <p class="font-bold text-slate-800">3. Policy Behind the Veil?</p>
              <p class="text-[11px] text-slate-600">만약 태어나기 전 사회 제도를 정할 수 있다면, 어떤 최소 안전망을 가장 먼저 만들겠습니까?</p>
            </div>
          </div>
        </div>

        <button onclick="resetAll()" class="w-full bg-slate-800 hover:bg-slate-900 text-white font-semibold py-2.5 rounded-xl text-xs transition">
          🔄 Draw New Cards (새로운 조건으로 다시 뽑기)
        </button>
      </div>

    </div>
  </div>

  <script>
    const attributes = {
      age: [
        { id: "teen", en: "16 yrs (Student)", ko: "16세 (청소년/학생)" },
        { id: "job_seeker", en: "26 yrs (Job Seeker)", ko: "26세 (청년 구직자)" },
        { id: "middle_aged", en: "48 yrs (Breadwinner)", ko: "48세 (중장년 가장)" },
        { id: "elderly", en: "74 yrs (Living Alone)", ko: "74세 (독거노인)" }
      ],
      gender: [
        { id: "female", en: "Female", ko: "여성" },
        { id: "male", en: "Male", ko: "남성" }
      ],
      background: [
        { id: "native", en: "Native Citizen", ko: "내국인" },
        { id: "multicultural", en: "Multicultural Youth", ko: "다문화가정 자녀" },
        { id: "migrant_worker", en: "Foreign Worker", ko: "외국인 노동자 (단기비자)" },
        { id: "refugee", en: "Refugee Applicant", ko: "난민 신청자" }
      ],
      wealth: [
        { id: "low", en: "Low Income Recipient", ko: "기초생활수급자 (저소득)" },
        { id: "middle", en: "Middle Income", ko: "중위소득층 (평범한 가구)" },
        { id: "high", en: "Top 10% High Income", ko: "상위 10% 고소득층" }
      ],
      disability: [
        { id: "none", en: "Non-disabled", ko: "비장애인" },
        { id: "wheelchair", en: "Wheelchair User", ko: "휠체어 이용자 (지체장애)" },
        { id: "visual", en: "Visually Impaired", ko: "시각장애인" },
        { id: "neurodivergent", en: "Neurodivergent", ko: "발달장애/신경다양인" }
      ],
      housing: [
        { id: "modern_apt", en: "Modern Apartment", ko: "역세권 신축 아파트" },
        { id: "semi_basement", en: "Semi-basement Room", ko: "침수 위험 반지하방" },
        { id: "rural", en: "Isolated Rural House", ko: "대중교통 없는 노후주택" }
      ]
    };

    const events = [
      {
        titleEn: "Situation 1: Extreme Heavy Rainfall",
        titleKo: "상황 1: 기록적인 폭우와 침수",
        descEn: "A record storm hits your town. Flood water starts pouring into homes.",
        descKo: "기록적인 폭우가 마을을 덮쳐 주택가로 빗물이 들이치기 시작합니다.",
        resolve: (p) => {
          if (p.housing.id === "semi_basement" || p.wealth.id === "low") {
            return { score: -20, en: "Your room was flooded. Emergency repair cost is devastating. (-20 pts)", ko: "방이 침수되어 막대한 복구 비용과 함께 일상이 마비되었습니다. (-20점)", neg: true };
          } else if (p.wealth.id === "high") {
            return { score: 0, en: "Modern drainage and flood walls kept you safe. (0 pts)", ko: "최신 차수벽과 배수 시스템 덕분에 아무 피해가 없었습니다. (0점)", neg: false };
          }
          return { score: -5, en: "Temporary blackout caused minor inconveniences. (-5 pts)", ko: "일시적인 정전으로 약간의 불편을 겪었습니다. (-5점)", neg: true };
        }
      },
      {
        titleEn: "Situation 2: Public Transportation",
        titleKo: "상황 2: 대중교통을 이용한 이동",
        descEn: "You have an urgent appointment across the city during rush hour.",
        descKo: "출근 시간대에 도시 반대편으로 긴급하게 이동해야 합니다.",
        resolve: (p) => {
          if (p.disability.id === "wheelchair") {
            return { score: -15, en: "The station lift was broken. You were late for 1 hour. (-15 pts)", ko: "지하철 리프트가 고장 나 약속에 1시간 지각했습니다. (-15점)", neg: true };
          } else if (p.housing.id === "rural") {
            return { score: -10, en: "Buses only run twice a day. You waited 2 hours. (-10 pts)", ko: "버스가 하루 두 번만 다녀 정류장에서 2시간을 기다렸습니다. (-10점)", neg: true };
          }
          return { score: +5, en: "Arrived on time using the seamless transit transfer! (+5 pts)", ko: "편리한 대중교통 환승으로 제시간에 도착했습니다! (+5점)", neg: false };
        }
      },
      {
        titleEn: "Situation 3: Emergency Relief Application",
        titleKo: "상황 3: 긴급 재난지원금 신청",
        descEn: "Government relief is provided, but requires online Korean identity auth.",
        descKo: "정부 지원금이 지급되지만, 한국어 온라인 본인 인증으로만 진행됩니다.",
        resolve: (p) => {
          if (p.background.id === "refugee" || p.background.id === "migrant_worker") {
            return { score: -15, en: "Language barriers and strict visa status blocked you. (-15 pts)", ko: "언어 장벽과 비자 제한으로 지원 대상에서 배제되었습니다. (-15점)", neg: true };
          } else if (p.age.id === "elderly" || p.disability.id === "visual") {
            return { score: -10, en: "Complex digital screens made it impossible to apply alone. (-10 pts)", ko: "복잡한 모바일 인증 절차 때문에 신청을 포기했습니다. (-10점)", neg: true };
          }
          return { score: +10, en: "You received the funds safely via simple mobile auth. (+10 pts)", ko: "모바일 간편인증으로 지원금을 무사히 수령했습니다. (+10점)", neg: false };
        }
      },
      {
        titleEn: "Situation 4: Sudden Emergency Illness",
        titleKo: "상황 4: 야간 응급 진료",
        descEn: "You suffer from severe high fever at midnight and need urgent care.",
        descKo: "한밤중에 심한 고열이 발생해 긴급하게 병원 진료가 필요합니다.",
        resolve: (p) => {
          if (p.wealth.id === "low") {
            return { score: -15, en: "You hesitated to call 119 fearing medical bills. (-15 pts)", ko: "병원비 부담 때문에 구급차를 부르지 못하고 참았습니다. (-15점)", neg: true };
          } else if (p.housing.id === "rural") {
            return { score: -15, en: "No emergency room nearby. It took 90 minutes to find a doctor. (-15 pts)", ko: "인근에 응급실이 없어 병원 이송에 90분이 걸렸습니다. (-15점)", neg: true };
          }
          return { score: +5, en: "Treated promptly at a general hospital 10 mins away. (+5 pts)", ko: "10분 거리의 종합병원에서 신속하게 치료받았습니다. (+5점)", neg: false };
        }
      }
    ];

    let myPersona = {
      age: null,
      gender: null,
      background: null,
      wealth: null,
      disability: null,
      housing: null
    };

    let score = 50;
    let currentStep = 0;

    function getRandomItem(arr) {
      return arr[Math.floor(Math.random() * arr.length)];
    }

    // 개별 카드 뒤집기 (터치 시 랜덤 선택 & 회전)
    function flipSingleCard(type) {
      const inner = document.getElementById(`cardInner-${type}`);
      if (inner.classList.contains("card-flipped")) return; // 이미 뒤집힌 경우 무시

      const chosen = getRandomItem(attributes[type]);
      myPersona[type] = chosen;

      document.getElementById(`textEn-${type}`).innerText = chosen.en;
      document.getElementById(`textKo-${type}`).innerText = chosen.ko;
      inner.classList.add("card-flipped");

      checkAllFlipped();
    }

    // 전체 카드 한 번에 랜덤 뽑기
    function revealAllCardsRandomly() {
      Object.keys(attributes).forEach((type, idx) => {
        setTimeout(() => {
          const inner = document.getElementById(`cardInner-${type}`);
          const chosen = getRandomItem(attributes[type]);
          myPersona[type] = chosen;
          document.getElementById(`textEn-${type}`).innerText = chosen.en;
          document.getElementById(`textKo-${type}`).innerText = chosen.ko;
          inner.classList.add("card-flipped");
          checkAllFlipped();
        }, idx * 120);
      });
    }

    // 6개 카드 모두 뒤집혔는지 체크하여 시작 버튼 활성화
    function checkAllFlipped() {
      const allReady = Object.values(myPersona).every(val => val !== null);
      const btn = document.getElementById("startSimBtn");
      if (allReady) {
        btn.disabled = false;
        btn.className = "w-full bg-gradient-to-r from-indigo-600 to-purple-600 hover:from-indigo-700 hover:to-purple-700 text-white font-black py-3 rounded-xl text-xs transition duration-200 shadow-lg pulse-glow cursor-pointer";
        btn.innerHTML = "Enter the World with this Identity ➔ (이 신분으로 세상에 들어가기)";
      }
    }

    // 시뮬레이션 화면으로 전환
    function enterSimulationWorld() {
      score = 50;
      currentStep = 0;

      // 상단 요약 바 구성
      const summaryBar = document.getElementById("personaSummaryBar");
      summaryBar.innerHTML = `
        <div>🎂 <b>${myPersona.age.en}</b> (${myPersona.age.ko})</div>
        <div>⚧️ <b>${myPersona.gender.en}</b> (${myPersona.gender.ko})</div>
        <div>🌐 <b>${myPersona.background.en}</b> (${myPersona.background.ko})</div>
        <div>💰 <b>${myPersona.wealth.en}</b> (${myPersona.wealth.ko})</div>
        <div>♿ <b>${myPersona.disability.en}</b> (${myPersona.disability.ko})</div>
        <div>🏡 <b>${myPersona.housing.en}</b> (${myPersona.housing.ko})</div>
      `;

      document.getElementById("scoreDisplay").innerText = score + " pts";
      document.getElementById("lotteryScreen").classList.add("hidden");
      document.getElementById("simScreen").classList.remove("hidden");
      document.getElementById("scoreContainer").classList.remove("hidden");

      loadEvent(currentStep);
    }

    function loadEvent(step) {
      const e = events[step];
      document.getElementById("stepBadge").innerText = `Situation ${step + 1} of ${events.length}`;
      document.getElementById("eventTitle").innerText = e.titleEn;
      document.getElementById("eventTitleKo").innerText = e.titleKo;
      document.getElementById("eventDesc").innerText = e.descEn;
      document.getElementById("eventDescKo").innerText = e.descKo;

      document.getElementById("resultBox").classList.add("hidden");
      document.getElementById("resolveBtn").classList.remove("hidden");
      document.getElementById("nextBtn").classList.add("hidden");
    }

    function resolveEvent() {
      const e = events[currentStep];
      const outcome = e.resolve(myPersona);
      score += outcome.score;
      document.getElementById("scoreDisplay").innerText = score + " pts";

      const box = document.getElementById("resultBox");
      box.className = outcome.neg 
        ? "p-3 rounded-xl text-xs leading-relaxed mb-3 bg-rose-50 border border-rose-200 text-rose-800" 
        : "p-3 rounded-xl text-xs leading-relaxed mb-3 bg-emerald-50 border border-emerald-200 text-emerald-800";
      
      document.getElementById("resultEn").innerText = outcome.en;
      document.getElementById("resultKo").innerText = outcome.ko;
      box.classList.remove("hidden");

      document.getElementById("resolveBtn").classList.add("hidden");
      document.getElementById("nextBtn").classList.remove("hidden");
    }

    function nextEvent() {
      currentStep++;
      if (currentStep < events.length) {
        loadEvent(currentStep);
      } else {
        showEnd();
      }
    }

    function showEnd() {
      document.getElementById("simScreen").classList.add("hidden");
      document.getElementById("endScreen").classList.remove("hidden");
      document.getElementById("finalScore").innerText = score + " pts";
      
      let status = "";
      if (score >= 65) status = "🌟 Stable & Protected Life (안정적인 삶)";
      else if (score >= 40) status = "⚠️ Vulnerable to Crises (위기에 취약한 상태)";
      else status = "🚨 Severe Human Rights Crisis (기본권 침해 위기)";
      
      document.getElementById("finalStatus").innerText = status;
    }

    function resetAll() {
      myPersona = { age: null, gender: null, background: null, wealth: null, disability: null, housing: null };
      Object.keys(attributes).forEach(type => {
        document.getElementById(`cardInner-${type}`).classList.remove("card-flipped");
      });
      const btn = document.getElementById("startSimBtn");
      btn.disabled = true;
      btn.className = "w-full bg-indigo-400 text-white font-bold py-3 rounded-xl text-xs transition duration-150 shadow-md cursor-not-allowed";
      btn.innerText = "Reveal All 6 Cards to Start (6개를 모두 뽑아야 시작됩니다)";

      document.getElementById("endScreen").classList.add("hidden");
      document.getElementById("scoreContainer").classList.add("hidden");
      document.getElementById("lotteryScreen").classList.remove("hidden");
    }
  </script>
</body>
</html># equality
