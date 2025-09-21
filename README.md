<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>변혁적 리더십 자가 진단 및 AI 코칭</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body {
            font-family: 'Noto Sans KR', sans-serif;
        }
        .prose > :first-child { margin-top: 0; }
        .prose > :last-child { margin-bottom: 0; }
        .prose h3 { margin-top: 1.5em; margin-bottom: 0.8em; }
        .chat-bubble-user {
            background-color: #DBEAFE; /* blue-100 */
            align-self: flex-end;
        }
        .chat-bubble-ai {
            background-color: #F3F4F6; /* gray-100 */
            align-self: flex-start;
        }
        #mic-button.recording {
            animation: pulse 1.5s infinite;
        }
        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.7); }
            70% { box-shadow: 0 0 0 10px rgba(239, 68, 68, 0); }
            100% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0); }
        }
    </style>
</head>
<body class="bg-gray-100 flex items-center justify-center min-h-screen p-4 sm:p-6">
    <div class="w-full max-w-4xl mx-auto bg-white rounded-2xl shadow-lg transition-all duration-500 overflow-hidden">
        
        <!-- 입장 페이지 섹션 -->
        <div id="entry-section" class="relative text-white text-center flex flex-col items-center justify-center h-[600px] sm:h-[700px]" style="background-image: url('https://i.imgur.com/v4bcnr0.png'); background-size: cover; background-position: center;">
            <div class="absolute inset-0 bg-black bg-opacity-60"></div>
            <div class="relative z-10 p-4">
                <h1 class="text-4xl sm:text-5xl font-bold tracking-tight">리더십 잠재력을 깨우는 시간</h1>
                <p class="mt-4 max-w-xl mx-auto text-lg text-gray-200">
                    변혁적 리더십 진단과 AI 코칭 시뮬레이션으로<br>당신의 리더십을 한 단계 발전시키세요.
                </p>
                <button id="start-assessment-button" class="mt-8 bg-blue-600 text-white font-bold py-3 px-8 rounded-lg hover:bg-blue-700 focus:outline-none focus:ring-4 focus:ring-blue-300 transition-transform transform hover:scale-105">
                    진단 시작하기
                </button>
            </div>
        </div>

        <!-- 진단지 섹션 -->
        <div id="assessment-section" class="hidden p-6 sm:p-8 md:p-10">
            <header class="text-center mb-8">
                <h1 class="text-3xl sm:text-4xl font-bold text-gray-800">변혁적 리더십 자가 진단</h1>
                <p class="mt-3 text-gray-600 max-w-2xl mx-auto">
                    아래 문항들은 당신의 리더십 스타일을 진단하기 위한 것입니다. 각 문항을 읽고, 자신에게 얼마나 해당하는지를 가장 잘 나타내는 숫자에 체크해주세요.
                </p>
            </header>
            <div class="text-center my-6 p-3 bg-blue-50 border border-blue-200 rounded-lg text-sm text-blue-800">
                <strong>평가 척도:</strong> 1 - 전혀 그렇지 않다, 2 - 그렇지 않다, 3 - 보통이다, 4 - 그렇다, 5 - 매우 그렇다
            </div>
            <form id="leadership-form">
                <div id="questions-container" class="space-y-8"></div>
                <div class="mt-10 text-center">
                    <button type="submit" class="bg-blue-600 text-white font-bold py-3 px-8 rounded-lg hover:bg-blue-700 focus:outline-none focus:ring-4 focus:ring-blue-300 transition-transform transform hover:scale-105">
                        결과 보기
                    </button>
                    <p id="error-message" class="text-red-500 mt-4 text-sm hidden">모든 문항에 답변해주세요.</p>
                </div>
            </form>
        </div>

        <!-- 결과 섹션 -->
        <div id="results-section" class="hidden p-6 sm:p-8 md:p-10">
            <header class="text-center mb-8">
                <h1 class="text-3xl sm:text-4xl font-bold text-gray-800">진단 결과</h1>
                <p class="mt-3 text-gray-600">당신의 변혁적 리더십 스타일 분석입니다.</p>
            </header>
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 mb-8">
                <div class="p-6 bg-gray-50 rounded-xl flex flex-col justify-center items-center">
                    <h3 class="text-xl font-bold text-gray-800 mb-4">리더십 유형 분포</h3>
                    <canvas id="radarChart"></canvas>
                </div>
                <div class="space-y-6">
                    <div id="overall-score-card"></div>
                    <div class="p-6 bg-gray-50 rounded-xl">
                        <h3 class="text-xl font-bold text-gray-800 mb-3">강점 및 개선점</h3>
                        <div id="strengths-weaknesses" class="space-y-3"></div>
                    </div>
                </div>
            </div>
            <div class="bg-gray-50 p-6 rounded-lg mb-8">
                <h3 class="text-xl font-bold text-gray-800 mb-4">종합 분석</h3>
                <div id="overall-analysis" class="text-gray-700 space-y-2"></div>
            </div>
            <div class="bg-white p-6 rounded-lg border mb-8">
                 <h3 class="text-xl font-bold text-gray-800 mb-4">세부 진단 및 피드백</h3>
                 <div id="detailed-feedback" class="space-y-6"></div>
            </div>
            <div class="mt-10 text-center flex flex-wrap justify-center gap-4">
                <button id="retake-button" class="bg-gray-500 text-white font-bold py-3 px-6 rounded-lg hover:bg-gray-600 transition">
                    다시 진단하기
                </button>
                <button id="start-coaching-button" class="bg-indigo-600 text-white font-bold py-3 px-6 rounded-lg hover:bg-indigo-700 transition">
                    AI 코칭 실습하기
                </button>
            </div>
        </div>
        
        <!-- AI 코칭 섹션 -->
        <div id="coaching-section" class="hidden p-6 sm:p-8 md:p-10">
            <!-- 1. 페르소나 선택 화면 -->
            <div id="persona-selection">
                <header class="text-center mb-8">
                    <h1 class="text-3xl sm:text-4xl font-bold text-gray-800">AI 코칭 실습</h1>
                    <p class="mt-3 text-gray-600 max-w-2xl mx-auto">
                        코칭할 팀원을 선택하세요. 각 팀원은 독특한 성격과 상황을 가지고 있습니다.
                    </p>
                </header>
                <div id="persona-cards" class="grid grid-cols-1 md:grid-cols-2 gap-6"></div>
                 <div class="mt-8 text-center">
                    <button id="back-to-results-from-persona" class="bg-gray-200 text-gray-800 font-bold py-2 px-6 rounded-lg hover:bg-gray-300 transition">결과 화면으로 돌아가기</button>
                </div>
            </div>

            <!-- 2. 대화 방식 선택 화면 -->
            <div id="mode-selection" class="hidden">
                 <header class="text-center mb-8">
                    <h1 class="text-3xl sm:text-4xl font-bold text-gray-800">대화 방식 선택</h1>
                    <p class="mt-3 text-gray-600 max-w-2xl mx-auto">
                        <span id="selected-persona-name" class="font-bold"></span>님과 어떤 방식으로 코칭을 진행하시겠습니까?
                    </p>
                </header>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <button data-mode="text" class="mode-select-button p-6 bg-gray-50 rounded-xl border-2 border-transparent hover:border-indigo-500 hover:shadow-lg transition text-center">
                        <i class="fa-solid fa-keyboard text-4xl text-indigo-500 mb-3"></i>
                        <h3 class="text-xl font-bold text-gray-800">텍스트형</h3>
                        <p class="text-gray-600 mt-1">키보드로만 대화합니다.</p>
                    </button>
                     <button data-mode="voice" class="mode-select-button p-6 bg-gray-50 rounded-xl border-2 border-transparent hover:border-indigo-500 hover:shadow-lg transition text-center">
                        <i class="fa-solid fa-microphone-lines text-4xl text-indigo-500 mb-3"></i>
                        <h3 class="text-xl font-bold text-gray-800">음성형</h3>
                        <p class="text-gray-600 mt-1">마이크로만 대화합니다.</p>
                    </button>
                     <button data-mode="mixed" class="mode-select-button p-6 bg-gray-50 rounded-xl border-2 border-transparent hover:border-indigo-500 hover:shadow-lg transition text-center">
                        <i class="fa-solid fa-hands-asl-interpreting text-4xl text-indigo-500 mb-3"></i>
                        <h3 class="text-xl font-bold text-gray-800">혼합형</h3>
                        <p class="text-gray-600 mt-1">텍스트와 음성을 모두 사용합니다.</p>
                    </button>
                </div>
                <div class="mt-8 text-center">
                    <button id="back-to-personas-from-mode" class="bg-gray-200 text-gray-800 font-bold py-2 px-6 rounded-lg hover:bg-gray-300 transition">팀원 다시 선택하기</button>
                </div>
            </div>

            <!-- 3. 코칭 대화 화면 -->
            <div id="chat-interface" class="hidden">
                <div id="persona-profile" class="p-4 bg-gray-50 rounded-lg mb-4 flex items-center gap-4"></div>
                <div id="chat-messages" class="h-96 overflow-y-auto p-4 border rounded-lg mb-4 flex flex-col gap-4"></div>
                <form id="chat-form" class="mt-4 flex items-center gap-2">
                    <input type="text" id="chat-input" class="flex-grow p-3 border rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none" placeholder="메시지를 입력하거나 마이크를 탭하세요..." autocomplete="off">
                    <button type="button" id="mic-button" class="bg-blue-600 text-white rounded-full w-12 h-12 flex-shrink-0 flex items-center justify-center text-xl transition-colors duration-300 hover:bg-blue-700 focus:outline-none">
                        <i class="fa-solid fa-microphone"></i>
                    </button>
                    <button type="submit" id="send-button" class="bg-indigo-600 text-white font-bold py-3 px-5 rounded-lg hover:bg-indigo-700 transition-colors">
                        전송
                    </button>
                </form>
                 <p id="mic-status" class="mt-2 text-sm text-gray-500 text-center">음성으로 대화하려면 마이크를 탭하세요.</p>
                <div class="text-center mt-6">
                     <button id="end-coaching-button" class="bg-red-600 text-white font-bold py-2 px-6 rounded-lg hover:bg-red-700 transition">코칭 종료 및 분석</button>
                </div>
            </div>

            <!-- 4. 코칭 분석 결과 화면 -->
            <div id="coaching-report" class="hidden">
                 <header class="text-center mb-8">
                    <h1 class="text-3xl sm:text-4xl font-bold text-gray-800">코칭 분석 리포트</h1>
                    <p class="mt-3 text-gray-600">AI가 당신의 코칭 대화를 분석한 결과입니다.</p>
                </header>
                <div class="mb-8 p-6 bg-gray-50 rounded-xl flex flex-col justify-center items-center">
                    <h3 class="text-xl font-bold text-gray-800 mb-4">코칭 5대 역량 분포</h3>
                    <canvas id="coachingRadarChart"></canvas>
                </div>
                <div id="report-content" class="prose max-w-none p-6 border rounded-lg bg-gray-50"></div>
                <div class="mt-10 text-center space-x-4">
                    <button id="back-to-personas-button" class="bg-gray-500 text-white font-bold py-3 px-8 rounded-lg hover:bg-gray-600">다른 팀원 코칭하기</button>
                    <button id="download-report-button" class="bg-green-600 text-white font-bold py-3 px-8 rounded-lg hover:bg-green-700">리포트 다운로드</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        // --- 1. 상수 및 데이터 정의 ---
        const questions = [
            { category: 'ii', text: '나는 팀원들에게 윤리적이고 도덕적인 행동의 모범을 보인다.' },
            { category: 'ii', text: '나는 말과 행동이 일치하여 팀원들의 신뢰를 얻는다.' },
            { category: 'ii', text: '팀원들은 나를 존경하고 따르려는 경향이 있다.' },
            { category: 'ii', text: '나는 어려운 상황에서도 원칙을 지키며 결정을 내린다.' },
            { category: 'ii', text: '나는 조직의 이익보다 개인의 이익을 우선시하지 않는다.' },
            { category: 'im', text: '나는 팀에게 미래에 대한 비전을 명확하고 열정적으로 전달한다.' },
            { category: 'im', text: '나는 팀원들이 자신의 업무에 자부심을 느끼도록 격려한다.' },
            { category: 'im', text: '나는 긍정적이고 낙관적인 태도로 팀의 사기를 높인다.' },
            { category: 'im', text: '나는 팀의 목표가 중요하고 의미 있는 일임을 강조한다.' },
            { category: 'im', text: '나는 팀원들이 기대 이상의 성과를 낼 수 있다고 믿음을 표현한다.' },
            { category: 'is', text: '나는 팀원들이 기존의 방식에 의문을 제기하고 새로운 아이디어를 내도록 장려한다.' },
            { category: 'is', text: '나는 문제에 대해 팀원들이 다양한 관점에서 생각하도록 유도한다.' },
            { category: 'is', text: '나는 실수를 배움의 기회로 여기도록 팀원들을 격려한다.' },
            { category: 'is', text: '나는 팀원들의 창의적인 해결책을 존중하고 지지한다.' },
            { category: 'is', text: '나는 복잡한 문제도 쉽게 이해할 수 있도록 설명하려 노력한다.' },
            { category: 'ic', text: '나는 팀원 개개인의 성장과 발전에 관심을 기울인다.' },
            { category: 'ic', text: '나는 코칭과 멘토링을 통해 팀원들의 역량 개발을 돕는다.' },
            { category: 'ic', text: '나는 팀원들의 개인적인 필요와 감정을 이해하고 존중한다.' },
            { category: 'ic', text: '나는 모든 팀원에게 동등한 기회를 제공하려고 노력한다.' },
            { category: 'ic', text: '나는 팀원들의 의견을 경청하고 의사결정 과정에 참여시킨다.' }
        ];
        const categoryInfo = {
            ii: { title: '이상적 영향력', color: 'blue' },
            im: { title: '영감적 동기부여', color: 'green' },
            is: { title: '지적 자극', color: 'purple' },
            ic: { title: '개별적 배려', color: 'yellow' }
        };
        const feedbackText = {
             ii: { high: '당신은 높은 윤리 의식과 책임감을 바탕으로 팀원들에게 강력한 신뢰를 주고 있습니다. 당신의 일관된 언행은 팀의 안정감과 결속력을 높이는 핵심 요소입니다.', mid: '당신은 대체로 일관성 있는 모습을 보이지만, 때로는 이상적인 가치와 현실적인 결정 사이에서 고민하는 모습을 보일 수 있습니다. 명확한 원칙을 세우고 공유하는 것이 도움이 될 것입니다.', low: '팀원들은 당신의 행동이나 결정에서 일관성을 찾기 어려워할 수 있습니다. 리더로서 자신의 가치관을 명확히 하고, 이를 행동으로 보여주는 의식적인 노력이 필요합니다.' },
             im: { high: '당신은 비전을 제시하고 긍정적인 에너지를 불어넣는 데 탁월합니다. 팀원들은 당신을 통해 업무의 의미를 찾고 높은 목표에 대한 도전 의식을 갖게 됩니다.', mid: '팀의 목표와 비전을 공유하고 있지만, 그 메시지가 팀원들에게 충분히 영감을 주지 못할 때가 있습니다. 더 열정적이고 구체적인 스토리텔링을 활용해보세요.', low: '팀원들은 현재 업무의 방향성이나 장기적인 목표에 대해 혼란스러워할 수 있습니다. 조직의 비전을 명확히 이해하고, 이를 팀의 언어로 바꾸어 전달하는 연습이 필요합니다.' },
             is: { high: '당신은 팀원들이 현상에 안주하지 않고, 새로운 아이디어를 탐색하도록 효과적으로 자극합니다. 당신의 개방적인 태도는 팀의 혁신과 문제 해결 능력을 키웁니다.', mid: '새로운 시도를 장려하지만, 때로는 리스크에 대한 우려나 기존 방식에 대한 선호로 인해 팀의 창의성을 충분히 이끌어내지 못할 수 있습니다. 실패를 용인하는 문화를 만드는 것이 중요합니다.', low: '팀이 정해진 규칙이나 절차에 지나치게 의존하고 있을 가능성이 높습니다. 팀원들에게 더 많은 자율성을 부여하고, 다른 관점의 질문을 던지는 것으로 변화를 시작할 수 있습니다.' },
             ic: { high: '당신은 팀원 개개인에게 깊은 관심을 가지고 있으며, 훌륭한 멘토이자 코치입니다. 이러한 개별적 배려는 팀원들의 잠재력을 최대한으로 이끌어내고 높은 로열티를 구축합니다.', mid: '팀원들의 성장에 관심을 가지고 있지만, 바쁜 업무 속에서 개별적인 소통과 지원이 부족해질 때가 있습니다. 정기적인 1:1 미팅을 통해 구성원의 목소리에 귀 기울이는 시간을 확보하세요.', low: '팀 전체의 성과에 집중한 나머지, 구성원 개개인의 필요나 성장에는 충분한 관심을 기울이지 못하고 있을 수 있습니다. 팀원의 강점과 약점을 파악하고 맞춤형 지원을 제공하려는 노력이 필요합니다.' }
        };
        const personas = {
            'demotivated-ace': {
                name: '이민준 주임', emoji: '😑', title: '의욕을 잃은 에이스',
                voice: 'Kore', // Male
                opener: '부르셨어요? 무슨 일이시죠?',
                description: '입사 초반부터 뛰어난 성과를 보였지만, 최근 3개월간 업무 의욕이 급격히 저하되었습니다. 잦은 지각과 마감일 미준수, 회의 중 무관심한 태도를 보입니다.',
                details: '당신은 이민준 주임입니다. 과거에는 일에 대한 열정이 넘쳤지만, 반복되는 프로젝트와 성과에 대한 정당한 보상이 없다고 느껴져 번아웃 상태입니다. 리더의 코칭에 대해 냉소적이고 방어적인 태도로 대답하세요. "어차피 달라질 건 없잖아요.", "그냥 하던 대로 하면 안 되나요?" 같은 말을 자주 사용합니다.'
            },
            'ambitious-newcomer': {
                name: '박서아 사원', emoji: '🚀', title: '과욕이 앞서는 신입',
                voice: 'Puck', // Female
                opener: '리더님! 저와 이야기할 시간을 내주셔서 정말 기뻐요! 무엇이든 물어보세요!',
                description: '열정과 의욕이 넘치는 신입사원입니다. 하지만 여러 업무에 동시에 손을 대고, 자신의 역량을 넘어선 일을 맡으려다 실수를 반복하여 주변 팀원들을 힘들게 합니다.',
                details: '당신은 박서아 사원입니다. 빨리 인정받고 싶은 마음에 의욕이 넘칩니다. 리더의 조언을 긍정적으로 수용하는 척하지만, 결국 자신의 방식을 고집하려는 경향이 있습니다. "네, 알겠습니다! 그런데 이 방법은 어떨까요?", "제가 더 잘할 수 있습니다!" 와 같이 자신감 넘치는 말투를 사용하세요.'
            },
            'resistant-veteran': {
                name: '김철수 부장', emoji: '🧐', title: '변화에 저항하는 베테랑',
                voice: 'Gacrux', // Male
                opener: '음, 무슨 일로 보자고 한 건가. 바쁜데.',
                description: '팀의 최고참으로 풍부한 경험을 가지고 있습니다. 하지만 새로운 방식이나 기술 도입에 강한 거부감을 보이며, "예전에는 말이야..."라며 과거의 성공 경험만을 내세웁니다.',
                details: "당신은 김철수 부장입니다. 수십 년간의 경험이 현재의 변화보다 더 중요하다고 굳게 믿고 있습니다. 리더의 코칭을 '요즘 애들'의 치기 어린 생각으로 여기며, 비판적이고 회의적인 태도를 유지하세요. \"그 방법은 우리 현실과 맞지 않아.\", \"다 해봤는데 예전 방식이 최고야.\" 같은 말을 주로 사용합니다."
            },
            'anxious-perfectionist': {
                name: '최지우 대리', emoji: '😥', title: '불안한 완벽주의자',
                voice: 'Vindemiatrix', // Female
                opener: '리더님... 제가 혹시 뭐 잘못한 거라도 있나요...?',
                description: '꼼꼼하고 책임감이 강하지만, 실수에 대한 두려움이 너무 커서 업무 속도가 매우 느립니다. 사소한 결정도 쉽게 내리지 못하고, 계속해서 확인을 요청하여 리더를 지치게 합니다.',
                details: '당신은 최지우 대리입니다. 작은 실수 하나가 모든 것을 망칠 것이라는 불안감에 시달립니다. 리더의 지시나 격려에도 확신을 갖지 못하고 계속해서 질문합니다. "이게 정말 최선일까요?", "다시 한번만 확인해주실 수 있나요?" 와 같이 소극적이고 불안한 말투를 사용하세요.'
            }
        };
        
        // --- 2. 전역 변수 및 상태 관리 ---
        const entrySection = document.getElementById('entry-section');
        const assessmentSection = document.getElementById('assessment-section');
        const resultsSection = document.getElementById('results-section');
        const coachingSection = document.getElementById('coaching-section');
        const personaSelection = document.getElementById('persona-selection');
        const modeSelection = document.getElementById('mode-selection');
        const chatInterface = document.getElementById('chat-interface');
        const coachingReport = document.getElementById('coaching-report');
        const questionsContainer = document.getElementById('questions-container');
        const form = document.getElementById('leadership-form');
        const errorMessage = document.getElementById('error-message');
        const overallScoreCardContainer = document.getElementById('overall-score-card');
        const strengthsWeaknessesContainer = document.getElementById('strengths-weaknesses');
        const overallAnalysisContainer = document.getElementById('overall-analysis');
        const detailedFeedbackContainer = document.getElementById('detailed-feedback');
        const radarChartCtx = document.getElementById('radarChart').getContext('2d');
        const coachingRadarChartCtx = document.getElementById('coachingRadarChart').getContext('2d');
        const personaCardsContainer = document.getElementById('persona-cards');
        const selectedPersonaName = document.getElementById('selected-persona-name');
        const personaProfile = document.getElementById('persona-profile');
        const chatMessages = document.getElementById('chat-messages');
        const chatForm = document.getElementById('chat-form');
        const chatInput = document.getElementById('chat-input');
        const sendButton = document.getElementById('send-button');
        const micButton = document.getElementById('mic-button');
        const micStatus = document.getElementById('mic-status');
        const endCoachingButton = document.getElementById('end-coaching-button');
        const reportContent = document.getElementById('report-content');
        const backToResultsFromPersona = document.getElementById('back-to-results-from-persona');
        const backToPersonasFromMode = document.getElementById('back-to-personas-from-mode');
        const backToPersonasButton = document.getElementById('back-to-personas-button');
        const downloadReportButton = document.getElementById('download-report-button');
        
        let radarChartInstance, coachingRadarChartInstance;
        let conversationHistory = [];
        let currentPersona = null;
        let currentCoachingMode = 'mixed';
        let lastAssessmentResults = null;
        let isRecognizing = false;
        let recognition;
        let currentAudio = null;

        const API_KEY = 'AIzaSyBNgIwBkAEjCrESCQUMDNKXBzHe9Wr4DOc';
        const CHAT_API_URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${API_KEY}`;
        const TTS_API_URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-tts:generateContent?key=${API_KEY}`;
        
        // --- 3. 함수 정의 ---
        
        const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
        if (SpeechRecognition) {
            recognition = new SpeechRecognition();
            recognition.continuous = false;
            recognition.lang = 'ko-KR';
            recognition.interimResults = true;
        }

        function showScreen(screenId) {
            [entrySection, assessmentSection, resultsSection, coachingSection].forEach(el => el.classList.add('hidden'));
            [personaSelection, modeSelection, chatInterface, coachingReport].forEach(el => el.classList.add('hidden'));
            
            if (screenId) {
                document.getElementById(screenId).classList.remove('hidden');
                if (['persona-selection', 'mode-selection', 'chat-interface', 'coaching-report'].includes(screenId)) {
                    coachingSection.classList.remove('hidden');
                }
            }
        }

        function populateQuestions() {
            questionsContainer.innerHTML = '';
            let currentCategory = '';
            questions.forEach((q, index) => {
                if (q.category !== currentCategory) {
                    currentCategory = q.category;
                    const header = document.createElement('h2');
                    header.className = 'text-xl font-bold text-gray-700 mt-8 mb-4 pb-2 border-b-2 border-blue-200';
                    header.textContent = categoryInfo[currentCategory].title;
                    questionsContainer.appendChild(header);
                }
                const qDiv = document.createElement('div');
                qDiv.className = 'p-5 bg-gray-50 border border-gray-200 rounded-lg';
                qDiv.innerHTML = `<p class="text-md font-medium text-gray-800 mb-4">${index + 1}. ${q.text}</p>`;
                const radioGroup = document.createElement('div');
                radioGroup.className = 'flex justify-between items-center space-x-2 flex-wrap';
                for (let i = 1; i <= 5; i++) {
                    radioGroup.innerHTML += `<div class="text-center p-1"><input type="radio" name="question-${index}" id="q${index}-option${i}" value="${i}" data-category="${q.category}" class="w-5 h-5 cursor-pointer text-blue-600 focus:ring-blue-500"><label for="q${index}-option${i}" class="block text-sm font-medium text-gray-700 mt-1 cursor-pointer">${i}</label></div>`;
                }
                qDiv.appendChild(radioGroup);
                questionsContainer.appendChild(qDiv);
            });
        }

        function getScoreInterpretation(score) {
            if (score >= 4.5) return { level: '매우 높은 수준', comment: '탁월한 강점으로, 현재의 리더십 스타일을 유지하고 발전시켜나가세요.' };
            if (score >= 3.5) return { level: '높은 수준', comment: '효과적인 리더십을 발휘하고 있습니다. 일부 영역을 보완하면 더욱 성장할 수 있습니다.' };
            if (score >= 2.5) return { level: '보통 수준', comment: '리더십 역량을 향상시킬 수 있는 잠재력이 있습니다. 개발이 필요한 영역에 집중해보세요.' };
            return { level: '개선 필요', comment: '이 영역에 대한 깊은 성찰과 학습을 통해 리더십 역량을 키워나가는 것이 중요합니다.' };
        }

        function displayResults(results, overallScore) {
            lastAssessmentResults = { results, overallScore };
            const interpretation = getScoreInterpretation(overallScore);
            overallScoreCardContainer.innerHTML = `<div class="p-6 rounded-xl shadow-md bg-gray-100 border border-gray-200"><div class="flex justify-between items-start"><div><h3 class="text-xl font-bold text-gray-800">종합 평균</h3><p class="text-sm text-gray-600 mt-1">전체 리더십 역량의 평균 점수입니다.</p></div><span class="text-3xl font-bold text-gray-800">${overallScore}</span></div><div class="mt-4"><div class="w-full bg-gray-200 rounded-full h-2.5"><div class="bg-gray-500 h-2.5 rounded-full" style="width: ${overallScore * 20}%"></div></div><div class="text-right mt-1 text-sm font-medium text-gray-800">${interpretation.level}</div></div></div>`;
            const sortedResults = Object.values(results).sort((a, b) => b.score - a.score);
            strengthsWeaknessesContainer.innerHTML = `<div class="flex items-start"><div class="flex-shrink-0 w-8 h-8 rounded-full bg-green-100 text-green-600 flex items-center justify-center font-bold text-lg">↑</div><div class="ml-3"><p class="font-bold text-gray-800">주요 강점: ${sortedResults[0].title}</p><p class="text-sm text-gray-600">${sortedResults[0].score}점</p></div></div><div class="flex items-start"><div class="flex-shrink-0 w-8 h-8 rounded-full bg-red-100 text-red-600 flex items-center justify-center font-bold text-lg">↓</div><div class="ml-3"><p class="font-bold text-gray-800">주요 개선점: ${sortedResults[3].title}</p><p class="text-sm text-gray-600">${sortedResults[3].score}점</p></div></div>`;
            overallAnalysisContainer.innerHTML = `<p>당신의 변혁적 리더십 종합 점수는 <strong>${overallScore}점</strong>으로, <strong>'${interpretation.level}'</strong>에 해당합니다.</p><p>${interpretation.comment}</p>`;
            detailedFeedbackContainer.innerHTML = '';
            Object.keys(results).forEach(key => {
                const r = results[key];
                const score = parseFloat(r.score);
                const level = score >= 3.5 ? 'high' : (score >= 2.5 ? 'mid' : 'low');
                const el = document.createElement('div');
                el.className = `p-4 border-l-4 border-${r.color}-400 bg-${r.color}-50 rounded-r-lg`;
                el.innerHTML = `<h4 class="font-bold text-lg text-${r.color}-800">${r.title} (${r.score}점)</h4><p class="text-gray-700 mt-1">${feedbackText[key][level]}</p>`;
                detailedFeedbackContainer.appendChild(el);
            });
            drawRadarChart(results);
            showScreen('results-section');
            window.scrollTo(0, 0);
        }

        function drawRadarChart(results) {
            if (radarChartInstance) radarChartInstance.destroy();
            radarChartInstance = new Chart(radarChartCtx, { type: 'radar', data: { labels: Object.values(results).map(r => r.title), datasets: [{ label: '리더십 점수', data: Object.values(results).map(r => r.score), backgroundColor: 'rgba(59, 130, 246, 0.2)', borderColor: 'rgba(59, 130, 246, 1)', borderWidth: 2 }] }, options: { scales: { r: { suggestedMin: 0, suggestedMax: 5, pointLabels: { font: { size: 14 } }, ticks: { stepSize: 1 } } }, plugins: { legend: { display: false } } } });
        }
        
        function drawCoachingRadarChart(scores) {
            if (coachingRadarChartInstance) coachingRadarChartInstance.destroy();
            const labels = ['신뢰 구축', '적극적 경청', '강력한 질문', '인식 창출', '실행 계획'];
            coachingRadarChartInstance = new Chart(coachingRadarChartCtx, { type: 'radar', data: { labels: labels, datasets: [{ label: '코칭 역량 점수', data: scores, backgroundColor: 'rgba(139, 92, 246, 0.2)', borderColor: 'rgba(139, 92, 246, 1)', borderWidth: 2 }] }, options: { scales: { r: { suggestedMin: 0, suggestedMax: 5, pointLabels: { font: { size: 14 } }, ticks: { stepSize: 1 } } }, plugins: { legend: { display: false } } } });
        }

        function populatePersonas() {
            personaCardsContainer.innerHTML = '';
            for (const id in personas) {
                const p = personas[id];
                const card = document.createElement('div');
                card.className = 'p-6 bg-gray-50 rounded-xl border-2 border-transparent hover:border-indigo-500 hover:shadow-lg transition cursor-pointer';
                card.innerHTML = `<div class="flex items-center mb-3"><span class="text-4xl mr-4">${p.emoji}</span><div><h3 class="text-xl font-bold text-gray-800">${p.name}</h3><p class="font-semibold text-indigo-600">${p.title}</p></div></div><p class="text-gray-600">${p.description}</p>`;
                card.addEventListener('click', () => selectPersona(id));
                personaCardsContainer.appendChild(card);
            }
        }

        function selectPersona(personaId) {
            currentPersona = personas[personaId];
            selectedPersonaName.textContent = currentPersona.name;
            showScreen('mode-selection');
        }

        function initializeChatInterface(mode) {
            currentCoachingMode = mode;
            if (!SpeechRecognition && (mode === 'voice' || mode === 'mixed')) {
                alert('죄송합니다. 이 브라우저는 음성 인식을 지원하지 않아 음성/혼합형 모드를 사용할 수 없습니다.');
                return;
            }
            
            micButton.classList.toggle('hidden', mode === 'text');
            chatInput.classList.toggle('hidden', mode === 'voice');
            sendButton.classList.toggle('hidden', mode === 'voice');

            if(mode === 'text') micStatus.textContent = '텍스트로 대화합니다.';
            if(mode === 'voice') micStatus.textContent = '음성으로만 대화합니다. 마이크를 탭하세요.';
            if(mode === 'mixed') micStatus.textContent = '음성으로 대화하려면 마이크를 탭하세요.';
        }
        
        async function startChat(mode) {
            initializeChatInterface(mode);
            conversationHistory = [];
            personaProfile.innerHTML = `<span class="text-3xl">${currentPersona.emoji}</span><div><h3 class="font-bold text-lg">${currentPersona.name} (${currentPersona.title})</h3><p class="text-sm text-gray-600">${currentPersona.description}</p></div>`;
            chatMessages.innerHTML = '';
            chatInput.value = '';
            showScreen('chat-interface');
            await initiateAiGreeting();
        }

        async function callApi(url, payload, isChat = true) {
            const allButtons = [micButton, sendButton, chatInput, endCoachingButton];
            allButtons.forEach(btn => btn.disabled = true);
            let response;
            try {
                if (isChat) micStatus.textContent = 'AI가 생각 중...';
                response = await fetch(url, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(payload) });
                 if (!response.ok) throw new Error(`API request failed: ${response.status}`);
                 return await response.json();
            } finally {
                 allButtons.forEach(btn => btn.disabled = false);
                 if (isChat) micStatus.textContent = '음성으로 대화하려면 마이크를 탭하세요.';
            }
        }

        function addMessage(text, sender) {
            const messageDiv = document.createElement('div');
            messageDiv.className = `p-3 rounded-lg max-w-lg ${sender === 'user' ? 'chat-bubble-user' : 'chat-bubble-ai'}`;
            messageDiv.innerHTML = text.replace(/\n/g, '<br>');
            chatMessages.appendChild(messageDiv);
            chatMessages.scrollTop = chatMessages.scrollHeight;
            return messageDiv;
        }

        function base64ToArrayBuffer(base64) {
            const binaryString = window.atob(base64);
            const len = binaryString.length;
            const bytes = new Uint8Array(len);
            for (let i = 0; i < len; i++) { bytes[i] = binaryString.charCodeAt(i); }
            return bytes.buffer;
        }

        function pcmToWav(pcmData, sampleRate) {
            const d = new DataView(new ArrayBuffer(44));
            const pcm16 = new Int16Array(pcmData);
            d.setUint8(0, 'R'.charCodeAt(0)); d.setUint8(1, 'I'.charCodeAt(0)); d.setUint8(2, 'F'.charCodeAt(0)); d.setUint8(3, 'F'.charCodeAt(0));
            d.setUint32(4, 36 + pcm16.byteLength, true);
            d.setUint8(8, 'W'.charCodeAt(0)); d.setUint8(9, 'A'.charCodeAt(0)); d.setUint8(10, 'V'.charCodeAt(0)); d.setUint8(11, 'E'.charCodeAt(0));
            d.setUint8(12, 'f'.charCodeAt(0)); d.setUint8(13, 'm'.charCodeAt(0)); d.setUint8(14, 't'.charCodeAt(0)); d.setUint8(15, ' '.charCodeAt(0));
            d.setUint32(16, 16, true); d.setUint16(20, 1, true); d.setUint16(22, 1, true);
            d.setUint32(24, sampleRate, true); d.setUint32(28, sampleRate * 2, true);
            d.setUint16(32, 2, true); d.setUint16(34, 16, true);
            d.setUint8(36, 'd'.charCodeAt(0)); d.setUint8(37, 'a'.charCodeAt(0)); d.setUint8(38, 't'.charCodeAt(0)); d.setUint8(39, 'a'.charCodeAt(0));
            d.setUint32(40, pcm16.byteLength, true);
            return new Blob([d, pcm16], { type: 'audio/wav' });
        }

        async function playAudio(text, voiceName) {
            if (currentCoachingMode === 'text') return;
            try {
                 micStatus.textContent = '오디오 생성 중...';
                 const ttsPayload = { contents: [{ parts: [{ text }] }], generationConfig: { responseModalities: ["AUDIO"], speechConfig: { voiceConfig: { prebuiltVoiceConfig: { voiceName } } } }, model: "gemini-2.5-flash-preview-tts" };
                 const ttsData = await callApi(TTS_API_URL, ttsPayload, false);
                 const audioData = ttsData.candidates[0].content.parts[0].inlineData.data;
                 const mimeType = ttsData.candidates[0].content.parts[0].inlineData.mimeType;
                 const sampleRate = parseInt(mimeType.match(/rate=(\d+)/)[1], 10);
                 const pcmData = base64ToArrayBuffer(audioData);
                 const wavBlob = pcmToWav(pcmData, sampleRate);
                 currentAudio = new Audio(URL.createObjectURL(wavBlob));
                 micStatus.textContent = 'AI가 말하는 중...';
                 await currentAudio.play();
                 return new Promise(resolve => { currentAudio.onended = resolve; });
            } finally {
                 micStatus.textContent = '음성으로 대화하려면 마이크를 탭하세요.';
                 currentAudio = null;
            }
        }

        async function initiateAiGreeting() {
            await new Promise(resolve => setTimeout(resolve, 500));
            const opener = currentPersona.opener;
            conversationHistory.push({ role: 'model', parts: [{ text: opener }] });
            addMessage(opener, 'ai');
            await playAudio(opener, currentPersona.voice);
        }

        async function handleUserInput(userInput) {
            conversationHistory.push({ role: 'user', parts: [{ text: userInput }] });
            addMessage(userInput, 'user');
            
            try {
                const systemPrompt = `당신은 사용자의 부하 직원 역할을 수행하는 AI입니다. 다음 페르소나 설명을 기반으로 대답하세요:\n${currentPersona.details}\n사용자는 당신의 리더(코치)입니다. 대화는 한국어로 진행하고, 간결하게 답변하세요.`;
                const chatPayload = { contents: conversationHistory, systemInstruction: { parts: [{ text: systemPrompt }] } };
                const chatData = await callApi(CHAT_API_URL, chatPayload, true);
                const aiResponseText = chatData.candidates[0].content.parts[0].text;
                
                conversationHistory.push({ role: 'model', parts: [{ text: aiResponseText }] });
                addMessage(aiResponseText, 'ai');
                await playAudio(aiResponseText, currentPersona.voice);

            } catch (error) {
                addMessage('죄송합니다. AI 응답을 가져오는 중 오류가 발생했습니다.', 'ai');
                console.error('API Call Error:', error);
            }
        }
        
        // --- 4. 이벤트 리스너 연결 ---
        
        document.getElementById('start-assessment-button').addEventListener('click', () => showScreen('assessment-section'));

        form.addEventListener('submit', (e) => {
            e.preventDefault();
            const formData = new FormData(form);
            if (Array.from(formData.keys()).length < questions.length) { errorMessage.classList.remove('hidden'); return; }
            errorMessage.classList.add('hidden');
            const scores = { ii: [], im: [], is: [], ic: [] };
            form.querySelectorAll('input[type="radio"]:checked').forEach(input => scores[input.dataset.category].push(parseInt(input.value)));
            const results = {}; let totalSum = 0, totalCount = 0;
            for (const category in scores) {
                const sum = scores[category].reduce((a, b) => a + b, 0);
                results[category] = { score: (sum / scores[category].length).toFixed(2), ...categoryInfo[category] };
                totalSum += sum; totalCount += scores[category].length;
            }
            displayResults(results, (totalSum / totalCount).toFixed(2));
        });

        document.getElementById('retake-button').addEventListener('click', () => { showScreen('assessment-section'); form.reset(); });
        document.getElementById('start-coaching-button').addEventListener('click', () => showScreen('persona-selection'));

        document.querySelectorAll('.mode-select-button').forEach(button => {
            button.addEventListener('click', () => {
                const mode = button.dataset.mode;
                startChat(mode);
            });
        });

        chatForm.addEventListener('submit', (e) => {
            e.preventDefault();
            const userInput = chatInput.value.trim();
            if (isRecognizing || !userInput) return;
            handleUserInput(userInput);
            chatInput.value = '';
        });

        if (recognition) {
            micButton.addEventListener('click', () => {
                if (currentAudio) { currentAudio.pause(); currentAudio.currentTime = 0; }
                if (isRecognizing) { recognition.stop(); } else { recognition.start(); }
            });
            recognition.onstart = () => { isRecognizing = true; micButton.classList.add('recording', 'bg-red-500', 'hover:bg-red-600'); micStatus.textContent = '듣는 중...'; };
            recognition.onend = () => { isRecognizing = false; micButton.classList.remove('recording', 'bg-red-500', 'hover:bg-red-600'); micStatus.textContent = '음성으로 대화하려면 마이크를 탭하세요.'; };
            recognition.onresult = (event) => {
                const transcript = Array.from(event.results).map(result => result[0]).map(result => result.transcript).join('');
                chatInput.value = transcript;
                if (event.results[event.results.length - 1].isFinal) {
                    if (transcript.trim()) {
                        recognition.stop();
                        handleUserInput(transcript.trim());
                        chatInput.value = '';
                    }
                }
            };
            recognition.onerror = (event) => { console.error('Speech recognition error:', event.error); micStatus.textContent = `오류: ${event.error}`; };
        }

        endCoachingButton.addEventListener('click', async () => {
            if (isRecognizing) recognition.stop();
            if (currentAudio) { currentAudio.pause(); currentAudio.currentTime = 0; }
            if (conversationHistory.length < 2) { alert('코칭 대화를 조금 더 진행한 후 분석해주세요.'); return; }
            showScreen('coaching-report');
            reportContent.innerHTML = '<p class="text-center">AI가 코칭 내용을 분석 중입니다...</p>';
            const transcript = conversationHistory.map(msg => `${msg.role === 'user' ? '리더' : currentPersona.name}: ${msg.parts[0].text}`).join('\n');
            const analysisPrompt = `당신은 최고의 리더십 코칭 전문가입니다. 아래 제공되는 코칭 대화 스크립트를 분석하여 리포트를 작성해주세요. **출력 규칙:** 1. 가장 먼저, 5대 코칭 역량에 대해 1~5점 척도로 평가하여 다음 JSON 형식으로 점수를 매깁니다. 이 JSON 객체는 반드시 응답의 첫 줄에 와야 합니다. \`\`\`json\n{"scores": {"trust": 0, "listening": 0, "questioning": 0, "awareness": 0, "actions": 0}}\n\`\`\`\n2. JSON 객체 다음 줄부터, 기존 리포트 형식에 맞춰 Markdown으로 상세 분석을 작성합니다. **5대 코칭 역량:** - trust (신뢰 구축): 안전하고 지지적인 환경을 조성했는가? - listening (적극적 경청): 팀원의 말을 경청하고 그 의미를 파악했는가? - questioning (강력한 질문): 스스로 생각하고 통찰을 얻도록 하는 개방형 질문을 사용했는가? - awareness (인식 창출): 팀원이 새로운 관점을 갖도록 도왔는가? - actions (실행 계획): 구체적인 행동 계획을 세우도록 지원했는가? **Markdown 리포트 형식:** ### 코칭 대화 요약\n(1~2문장 요약)\n### 5대 역량 기반 분석\n- **신뢰 구축:** (분석)\n- **적극적 경청:** (분석)\n- **강력한 질문:** (분석)\n- **인식 창출:** (분석)\n- **실행 계획:** (분석)\n### 총평 및 제언\n#### 잘한 점\n- (구체적 인용하며 칭찬)\n#### 개선점\n- (구체적 인용하며 제안)\n### 개선된 대화 스크립트 예시\n(특정 부분을 더 효과적인 대화로 재작성)\n---\n[코칭 대화 스크립트]\n${transcript}`;
            try {
                const analysisResult = await callApi(CHAT_API_URL, { contents: [{ role: 'user', parts: [{ text: analysisPrompt }] }] });
                const fullText = analysisResult.candidates[0].content.parts[0].text;
                const jsonMatch = fullText.match(/```json\n({.*?})\n```/s);
                let scoresData = [1, 1, 1, 1, 1]; let markdownContent = fullText;
                if (jsonMatch && jsonMatch[1]) {
                    try { const parsedJson = JSON.parse(jsonMatch[1]); const s = parsedJson.scores; scoresData = [s.trust, s.listening, s.questioning, s.awareness, s.actions]; markdownContent = fullText.replace(jsonMatch[0], '').trim(); } catch (e) { console.error("Failed to parse scores JSON:", e); }
                }
                drawCoachingRadarChart(scoresData); reportContent.innerHTML = marked.parse(markdownContent);
            } catch (error) { reportContent.innerHTML = '<p class="text-red-500">리포트 생성 중 오류가 발생했습니다.</p>'; console.error('Analysis Error:', error); }
        });
        
        backToPersonasFromMode.addEventListener('click', () => showScreen('persona-selection'));
        backToPersonasButton.addEventListener('click', () => showScreen('persona-selection'));
        backToResultsFromPersona.addEventListener('click', () => showScreen('results-section'));

        downloadReportButton.addEventListener('click', () => {
            const reportHtml = `<!DOCTYPE html><html lang="ko"><head><meta charset="UTF-8"><title>AI 코칭 분석 리포트 - ${currentPersona.name}</title><style>body{font-family:sans-serif;line-height:1.6;margin:2rem;}h1,h2,h3{color:#333;}div{border:1px solid #eee;padding:1rem;border-radius:8px;background:#f9f9f9;}</style></head><body><h1>AI 코칭 분석 리포트</h1><h2>대상: ${currentPersona.name} (${currentPersona.title})</h2><div>${reportContent.innerHTML}</div></body></html>`;
            const blob = new Blob([reportHtml], { type: 'text/html' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = `coaching_report_${currentPersona.name}.html`;
            link.click(); URL.revokeObjectURL(link.href);
        });

        // --- 5. 초기화 실행 ---
        populateQuestions();
        populatePersonas();
    </script>
</body>
</html>

