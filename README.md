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
    <style>
        body {
            font-family: 'Noto Sans KR', sans-serif;
        }
        .prose > :first-child { margin-top: 0; }
        .prose > :last-child { margin-bottom: 0; }
        .chat-bubble-user {
            background-color: #DBEAFE; /* blue-100 */
            align-self: flex-end;
        }
        .chat-bubble-ai {
            background-color: #F3F4F6; /* gray-100 */
            align-self: flex-start;
        }
    </style>
</head>
<body class="bg-gray-100 flex items-center justify-center min-h-screen p-4 sm:p-6">
    <div class="w-full max-w-4xl mx-auto bg-white rounded-2xl shadow-lg p-6 sm:p-8 md:p-10 transition-all duration-500">
        
        <!-- 진단지 섹션 -->
        <div id="assessment-section">
            <!-- ... 기존 진단지 HTML ... -->
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
        <div id="results-section" class="hidden">
            <!-- ... 기존 결과 섹션 HTML ... -->
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
            <div class="mt-10 text-center space-x-4">
                <button id="retake-button" class="bg-gray-500 text-white font-bold py-3 px-8 rounded-lg hover:bg-gray-600 focus:outline-none focus:ring-4 focus:ring-gray-300 transition-transform transform hover:scale-105">
                    다시 진단하기
                </button>
                <button id="start-coaching-button" class="bg-indigo-600 text-white font-bold py-3 px-8 rounded-lg hover:bg-indigo-700 focus:outline-none focus:ring-4 focus:ring-indigo-300 transition-transform transform hover:scale-105">
                    AI 코칭 실습하기
                </button>
            </div>
        </div>
        
        <!-- AI 코칭 섹션 -->
        <div id="coaching-section" class="hidden">
            <!-- 1. 페르소나 선택 화면 -->
            <div id="persona-selection">
                <header class="text-center mb-8">
                    <h1 class="text-3xl sm:text-4xl font-bold text-gray-800">AI 코칭 실습</h1>
                    <p class="mt-3 text-gray-600 max-w-2xl mx-auto">
                        코칭할 팀원을 선택하세요. 각 팀원은 독특한 성격과 상황을 가지고 있습니다.
                    </p>
                </header>
                <div id="persona-cards" class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <!-- 페르소나 카드가 여기에 동적으로 생성됩니다. -->
                </div>
            </div>

            <!-- 2. 코칭 대화 화면 -->
            <div id="chat-interface" class="hidden">
                <div id="persona-profile" class="p-4 bg-gray-50 rounded-lg mb-4 flex items-center gap-4"></div>
                <div id="chat-messages" class="h-96 overflow-y-auto p-4 border rounded-lg mb-4 flex flex-col gap-4"></div>
                <form id="chat-form" class="flex gap-2">
                    <input type="text" id="chat-input" class="flex-grow p-3 border rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none" placeholder="코칭 메시지를 입력하세요..." autocomplete="off">
                    <button type="submit" id="send-button" class="bg-indigo-600 text-white font-bold py-3 px-5 rounded-lg hover:bg-indigo-700 transition">전송</button>
                </form>
                <div class="text-center mt-6">
                     <button id="end-coaching-button" class="bg-red-600 text-white font-bold py-2 px-6 rounded-lg hover:bg-red-700 transition">코칭 종료 및 분석</button>
                </div>
            </div>

            <!-- 3. 코칭 분석 결과 화면 -->
            <div id="coaching-report" class="hidden">
                 <header class="text-center mb-8">
                    <h1 class="text-3xl sm:text-4xl font-bold text-gray-800">코칭 분석 리포트</h1>
                    <p class="mt-3 text-gray-600">AI가 당신의 코칭 대화를 분석한 결과입니다.</p>
                </header>
                <div id="report-content" class="prose max-w-none p-6 border rounded-lg bg-gray-50">
                    <!-- 분석 결과가 여기에 표시됩니다. -->
                </div>
                <div class="mt-10 text-center space-x-4">
                    <button id="back-to-personas-button" class="bg-gray-500 text-white font-bold py-3 px-8 rounded-lg hover:bg-gray-600">다른 팀원 코칭하기</button>
                    <button id="download-report-button" class="bg-green-600 text-white font-bold py-3 px-8 rounded-lg hover:bg-green-700">리포트 다운로드</button>
                </div>
            </div>
        </div>

    </div>

    <script>
        // --- 기존 진단 스크립트 (축약) ---
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
            ii: { title: '이상적 영향력', color: 'blue', description: '리더가 높은 기준의 윤리적, 도덕적 행동을 통해 구성원들에게 역할 모델이 되고, 존경과 신뢰를 얻는 능력입니다.' },
            im: { title: '영감적 동기부여', color: 'green', description: '리더가 구성원들에게 비전을 제시하고, 일의 의미와 중요성을 강조하여 열정과 도전 정신을 불러일으키는 능력입니다.' },
            is: { title: '지적 자극', color: 'purple', description: '리더가 구성원들이 낡은 가정을 버리고, 문제에 대해 새로운 관점으로 접근하며 창의적으로 해결하도록 격려하는 능력입니다.' },
            ic: { title: '개별적 배려', color: 'yellow', description: '리더가 구성원 개개인을 코치나 멘토로서 대하며, 각자의 필요와 잠재력에 관심을 기울여 성장과 발전을 돕는 능력입니다.' }
        };
        const feedbackText = {
             ii: { high: '당신은 높은 윤리 의식과 책임감을 바탕으로 팀원들에게 강력한 신뢰를 주고 있습니다. 당신의 일관된 언행은 팀의 안정감과 결속력을 높이는 핵심 요소입니다.', mid: '당신은 대체로 일관성 있는 모습을 보이지만, 때로는 이상적인 가치와 현실적인 결정 사이에서 고민하는 모습을 보일 수 있습니다. 명확한 원칙을 세우고 공유하는 것이 도움이 될 것입니다.', low: '팀원들은 당신의 행동이나 결정에서 일관성을 찾기 어려워할 수 있습니다. 리더로서 자신의 가치관을 명확히 하고, 이를 행동으로 보여주는 의식적인 노력이 필요합니다.' },
             im: { high: '당신은 비전을 제시하고 긍정적인 에너지를 불어넣는 데 탁월합니다. 팀원들은 당신을 통해 업무의 의미를 찾고 높은 목표에 대한 도전 의식을 갖게 됩니다.', mid: '팀의 목표와 비전을 공유하고 있지만, 그 메시지가 팀원들에게 충분히 영감을 주지 못할 때가 있습니다. 더 열정적이고 구체적인 스토리텔링을 활용해보세요.', low: '팀원들은 현재 업무의 방향성이나 장기적인 목표에 대해 혼란스러워할 수 있습니다. 조직의 비전을 명확히 이해하고, 이를 팀의 언어로 바꾸어 전달하는 연습이 필요합니다.' },
             is: { high: '당신은 팀원들이 현상에 안주하지 않고, 새로운 아이디어를 탐색하도록 효과적으로 자극합니다. 당신의 개방적인 태도는 팀의 혁신과 문제 해결 능력을 키웁니다.', mid: '새로운 시도를 장려하지만, 때로는 리스크에 대한 우려나 기존 방식에 대한 선호로 인해 팀의 창의성을 충분히 이끌어내지 못할 수 있습니다. 실패를 용인하는 문화를 만드는 것이 중요합니다.', low: '팀이 정해진 규칙이나 절차에 지나치게 의존하고 있을 가능성이 높습니다. 팀원들에게 더 많은 자율성을 부여하고, 다른 관점의 질문을 던지는 것으로 변화를 시작할 수 있습니다.' },
             ic: { high: '당신은 팀원 개개인에게 깊은 관심을 가지고 있으며, 훌륭한 멘토이자 코치입니다. 이러한 개별적 배려는 팀원들의 잠재력을 최대한으로 이끌어내고 높은 로열티를 구축합니다.', mid: '팀원들의 성장에 관심을 가지고 있지만, 바쁜 업무 속에서 개별적인 소통과 지원이 부족해질 때가 있습니다. 정기적인 1:1 미팅을 통해 구성원의 목소리에 귀 기울이는 시간을 확보하세요.', low: '팀 전체의 성과에 집중한 나머지, 구성원 개개인의 필요나 성장에는 충분한 관심을 기울이지 못하고 있을 수 있습니다. 팀원의 강점과 약점을 파악하고 맞춤형 지원을 제공하려는 노력이 필요합니다.' }
        };
        const questionsContainer = document.getElementById('questions-container');
        const form = document.getElementById('leadership-form');
        const errorMessage = document.getElementById('error-message');
        const assessmentSection = document.getElementById('assessment-section');
        const resultsSection = document.getElementById('results-section');
        const overallScoreCardContainer = document.getElementById('overall-score-card');
        const strengthsWeaknessesContainer = document.getElementById('strengths-weaknesses');
        const overallAnalysisContainer = document.getElementById('overall-analysis');
        const detailedFeedbackContainer = document.getElementById('detailed-feedback');
        const retakeButton = document.getElementById('retake-button');
        const radarChartCtx = document.getElementById('radarChart').getContext('2d');
        let radarChartInstance;
        function createQuestionElement(question, index) { /* ... */ }
        function populateQuestions() { /* ... */ }
        function getScoreInterpretation(score) { /* ... */ }
        form.addEventListener('submit', function(e) { /* ... */ });
        function displayResults(results, overallScore) { /* ... */ }
        function createResultCard(title, score, color, description) { /* ... */ }
        function drawRadarChart(results) { /* ... */ }
        retakeButton.addEventListener('click', () => { /* ... */ });
        // --- (기존 스크립트 상세 구현 생략) ---
        
        // (기존 스크립트 함수들 여기에 위치)
        populateQuestions();
        form.addEventListener('submit', function(e) {
            e.preventDefault();
            const formData = new FormData(form);
            if (Array.from(formData.keys()).length < questions.length) {
                errorMessage.classList.remove('hidden');
                return;
            }
            errorMessage.classList.add('hidden');
            const scores = { ii: [], im: [], is: [], ic: [] };
            form.querySelectorAll('input[type="radio"]:checked').forEach(input => {
                scores[input.dataset.category].push(parseInt(input.value));
            });
            const results = {};
            let totalSum = 0, totalCount = 0;
            for (const category in scores) {
                const sum = scores[category].reduce((a, b) => a + b, 0);
                results[category] = { score: (sum / scores[category].length).toFixed(2), ...categoryInfo[category] };
                totalSum += sum;
                totalCount += scores[category].length;
            }
            displayResults(results, (totalSum / totalCount).toFixed(2));
        });
        function displayResults(results, overallScore) {
            overallScoreCardContainer.innerHTML = '';
            overallScoreCardContainer.appendChild(createResultCard('종합 평균', overallScore, 'gray', '전체 리더십 역량의 평균 점수입니다.'));
            const sortedResults = Object.entries(results).sort((a, b) => b[1].score - a[1].score);
            strengthsWeaknessesContainer.innerHTML = `<div class="flex items-start"><div class="flex-shrink-0 w-8 h-8 rounded-full bg-green-100 text-green-600 flex items-center justify-center font-bold text-lg">↑</div><div class="ml-3"><p class="font-bold text-gray-800">주요 강점: ${sortedResults[0][1].title}</p><p class="text-sm text-gray-600">${sortedResults[0][1].score}점 - ${getScoreInterpretation(sortedResults[0][1].score).level}</p></div></div><div class="flex items-start"><div class="flex-shrink-0 w-8 h-8 rounded-full bg-red-100 text-red-600 flex items-center justify-center font-bold text-lg">↓</div><div class="ml-3"><p class="font-bold text-gray-800">주요 개선점: ${sortedResults[3][1].title}</p><p class="text-sm text-gray-600">${sortedResults[3][1].score}점 - ${getScoreInterpretation(sortedResults[3][1].score).level}</p></div></div>`;
            const overallInterpretation = getScoreInterpretation(overallScore);
            overallAnalysisContainer.innerHTML = `<p>당신의 변혁적 리더십 종합 점수는 <strong>${overallScore}점</strong>으로, <strong>'${overallInterpretation.level}'</strong>에 해당합니다.</p><p>${overallInterpretation.comment}</p>`;
            detailedFeedbackContainer.innerHTML = '';
            for (const key in results) {
                const r = results[key];
                const score = parseFloat(r.score);
                let level = score >= 3.5 ? 'high' : (score >= 2.5 ? 'mid' : 'low');
                const el = document.createElement('div');
                el.className = `p-4 border-l-4 border-${r.color}-400 bg-${r.color}-50 rounded-r-lg`;
                el.innerHTML = `<h4 class="font-bold text-lg text-${r.color}-800">${r.title} (${r.score}점)</h4><p class="text-gray-700 mt-1">${feedbackText[key][level]}</p>`;
                detailedFeedbackContainer.appendChild(el);
            }
            drawRadarChart(results);
            assessmentSection.classList.add('hidden');
            resultsSection.classList.remove('hidden');
            window.scrollTo(0, 0);
        }
        function createResultCard(title, score, color, description) {
            const interpretation = getScoreInterpretation(score);
            const card = document.createElement('div');
            card.className = `p-6 rounded-xl shadow-md bg-${color}-100 border border-${color}-200`;
            card.innerHTML = `<div class="flex justify-between items-start"><div><h3 class="text-xl font-bold text-${color}-800">${title}</h3><p class="text-sm text-gray-600 mt-1">${description}</p></div><span class="text-3xl font-bold text-${color}-800">${score}</span></div><div class="mt-4"><div class="w-full bg-gray-200 rounded-full h-2.5"><div class="bg-${color}-500 h-2.5 rounded-full" style="width: ${score * 20}%"></div></div><div class="text-right mt-1 text-sm font-medium text-${color}-800">${interpretation.level}</div></div>`;
            return card;
        }
        function getScoreInterpretation(score) {
            if (score >= 4.5) return { level: '매우 높은 수준', comment: '탁월한 강점으로, 현재의 리더십 스타일을 유지하고 발전시켜나가세요.' };
            if (score >= 3.5) return { level: '높은 수준', comment: '효과적인 리더십을 발휘하고 있습니다. 일부 영역을 보완하면 더욱 성장할 수 있습니다.' };
            if (score >= 2.5) return { level: '보통 수준', comment: '리더십 역량을 향상시킬 수 있는 잠재력이 있습니다. 개발이 필요한 영역에 집중해보세요.' };
            return { level: '개선 필요', comment: '이 영역에 대한 깊은 성찰과 학습을 통해 리더십 역량을 키워나가는 것이 중요합니다.' };
        }
        function drawRadarChart(results) {
            if (radarChartInstance) radarChartInstance.destroy();
            radarChartInstance = new Chart(radarChartCtx, { type: 'radar', data: { labels: Object.values(results).map(r => r.title), datasets: [{ label: '리더십 점수', data: Object.values(results).map(r => r.score), backgroundColor: 'rgba(59, 130, 246, 0.2)', borderColor: 'rgba(59, 130, 246, 1)', borderWidth: 2 }] }, options: { scales: { r: { suggestedMin: 0, suggestedMax: 5, pointLabels: { font: { size: 14 } }, ticks: { stepSize: 1 } } }, plugins: { legend: { display: false } } } });
        }
        retakeButton.addEventListener('click', () => {
            resultsSection.classList.add('hidden');
            assessmentSection.classList.remove('hidden');
            form.reset();
        });


        // --- AI 코칭 스크립트 ---
        const startCoachingButton = document.getElementById('start-coaching-button');
        const coachingSection = document.getElementById('coaching-section');
        const personaSelection = document.getElementById('persona-selection');
        const chatInterface = document.getElementById('chat-interface');
        const coachingReport = document.getElementById('coaching-report');

        const personaCardsContainer = document.getElementById('persona-cards');
        const personaProfile = document.getElementById('persona-profile');
        const chatMessages = document.getElementById('chat-messages');
        const chatForm = document.getElementById('chat-form');
        const chatInput = document.getElementById('chat-input');
        const sendButton = document.getElementById('send-button');
        const endCoachingButton = document.getElementById('end-coaching-button');
        const reportContent = document.getElementById('report-content');
        const backToPersonasButton = document.getElementById('back-to-personas-button');
        const downloadReportButton = document.getElementById('download-report-button');
        
        // **경고**: API 키를 클라이언트 사이드 코드에 직접 노출하는 것은 심각한 보안 위험을 초래할 수 있습니다.
        // 실제 애플리케이션에서는 서버를 통해 API를 호출하고 키를 안전하게 관리해야 합니다.
        // 이 코드는 교육 및 시뮬레이션 목적으로만 사용하세요.
        const API_KEY = 'AIzaSyBNgIwBkAEjCrESCQUMDNKXBzHe9Wr4DOc';
        const API_URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${API_KEY}`;
        
        let conversationHistory = [];
        let currentPersona = null;

        const personas = {
            'demotivated-ace': {
                name: '이민준 주임',
                emoji: '😑',
                title: '의욕을 잃은 에이스',
                description: '입사 초반부터 뛰어난 성과를 보였지만, 최근 3개월간 업무 의욕이 급격히 저하되었습니다. 잦은 지각과 마감일 미준수, 회의 중 무관심한 태도를 보입니다.',
                details: '당신은 이민준 주임입니다. 과거에는 일에 대한 열정이 넘쳤지만, 반복되는 프로젝트와 성과에 대한 정당한 보상이 없다고 느껴져 번아웃 상태입니다. 리더의 코칭에 대해 냉소적이고 방어적인 태도로 대답하세요. "어차피 달라질 건 없잖아요.", "그냥 하던 대로 하면 안 되나요?" 같은 말을 자주 사용합니다.'
            },
            'ambitious-newcomer': {
                name: '박서아 사원',
                emoji: '🚀',
                title: '과욕이 앞서는 신입',
                description: '열정과 의욕이 넘치는 신입사원입니다. 하지만 여러 업무에 동시에 손을 대고, 자신의 역량을 넘어선 일을 맡으려다 실수를 반복하여 주변 팀원들을 힘들게 합니다.',
                details: '당신은 박서아 사원입니다. 빨리 인정받고 싶은 마음에 의욕이 넘칩니다. 리더의 조언을 긍정적으로 수용하는 척하지만, 결국 자신의 방식을 고집하려는 경향이 있습니다. "네, 알겠습니다! 그런데 이 방법은 어떨까요?", "제가 더 잘할 수 있습니다!" 와 같이 자신감 넘치는 말투를 사용하세요.'
            },
            'resistant-veteran': {
                name: '김철수 부장',
                emoji: '🧐',
                title: '변화에 저항하는 베테랑',
                description: '팀의 최고참으로 풍부한 경험을 가지고 있습니다. 하지만 새로운 방식이나 기술 도입에 강한 거부감을 보이며, "예전에는 말이야..."라며 과거의 성공 경험만을 내세웁니다.',
                details: "당신은 김철수 부장입니다. 수십 년간의 경험이 현재의 변화보다 더 중요하다고 굳게 믿고 있습니다. 리더의 코칭을 '요즘 애들'의 치기 어린 생각으로 여기며, 비판적이고 회의적인 태도를 유지하세요. \"그 방법은 우리 현실과 맞지 않아.\", \"다 해봤는데 예전 방식이 최고야.\" 같은 말을 주로 사용합니다."
            },
            'anxious-perfectionist': {
                name: '최지우 대리',
                emoji: '😥',
                title: '불안한 완벽주의자',
                description: '꼼꼼하고 책임감이 강하지만, 실수에 대한 두려움이 너무 커서 업무 속도가 매우 느립니다. 사소한 결정도 쉽게 내리지 못하고, 계속해서 확인을 요청하여 리더를 지치게 합니다.',
                details: '당신은 최지우 대리입니다. 작은 실수 하나가 모든 것을 망칠 것이라는 불안감에 시달립니다. 리더의 지시나 격려에도 확신을 갖지 못하고 계속해서 질문합니다. "이게 정말 최선일까요?", "다시 한번만 확인해주실 수 있나요?" 와 같이 소극적이고 불안한 말투를 사용하세요.'
            }
        };

        function showScreen(screenId) {
            ['assessment-section', 'results-section', 'coaching-section', 'persona-selection', 'chat-interface', 'coaching-report'].forEach(id => {
                document.getElementById(id).classList.add('hidden');
            });
            if (screenId) {
                document.getElementById(screenId).classList.remove('hidden');
                if (['persona-selection', 'chat-interface', 'coaching-report'].includes(screenId)) {
                    document.getElementById('coaching-section').classList.remove('hidden');
                }
            }
        }

        startCoachingButton.addEventListener('click', () => {
            showScreen('persona-selection');
        });

        function populatePersonas() {
            personaCardsContainer.innerHTML = '';
            for (const id in personas) {
                const p = personas[id];
                const card = document.createElement('div');
                card.className = 'p-6 bg-gray-50 rounded-xl border-2 border-transparent hover:border-indigo-500 hover:shadow-lg transition cursor-pointer';
                card.dataset.id = id;
                card.innerHTML = `
                    <div class="flex items-center mb-3">
                        <span class="text-4xl mr-4">${p.emoji}</span>
                        <div>
                            <h3 class="text-xl font-bold text-gray-800">${p.name}</h3>
                            <p class="font-semibold text-indigo-600">${p.title}</p>
                        </div>
                    </div>
                    <p class="text-gray-600">${p.description}</p>
                `;
                card.addEventListener('click', () => startChat(id));
                personaCardsContainer.appendChild(card);
            }
        }
        
        function startChat(personaId) {
            currentPersona = personas[personaId];
            conversationHistory = [];
            
            personaProfile.innerHTML = `
                <span class="text-3xl">${currentPersona.emoji}</span>
                <div>
                    <h3 class="font-bold text-lg">${currentPersona.name} (${currentPersona.title})</h3>
                    <p class="text-sm text-gray-600">${currentPersona.description}</p>
                </div>
            `;
            chatMessages.innerHTML = '';
            showScreen('chat-interface');
        }

        chatForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            const userInput = chatInput.value.trim();
            if (!userInput) return;

            addMessage(userInput, 'user');
            conversationHistory.push({ role: 'user', parts: [{ text: userInput }] });
            chatInput.value = '';

            addTypingIndicator();

            try {
                const systemPrompt = `당신은 사용자의 부하 직원 역할을 수행하는 AI입니다. 다음 페르소나 설명을 기반으로 대답하세요:\n${currentPersona.details}\n사용자는 당신의 리더(코치)입니다. 대화는 한국어로 진행하고, 간결하게 답변하세요.`;
                const aiResponse = await callGeminiAPI(systemPrompt, conversationHistory);
                
                removeTypingIndicator();
                addMessage(aiResponse, 'ai');
                conversationHistory.push({ role: 'model', parts: [{ text: aiResponse }] });
            } catch (error) {
                removeTypingIndicator();
                addMessage('죄송합니다. AI 응답을 가져오는 중 오류가 발생했습니다.', 'ai');
                console.error('API Call Error:', error);
            }
        });
        
        async function callGeminiAPI(systemPrompt, history) {
            sendButton.disabled = true;
            const payload = {
                contents: history,
                systemInstruction: { parts: [{ text: systemPrompt }] },
                generationConfig: {
                    temperature: 0.7,
                    topP: 1.0,
                }
            };
            const response = await fetch(API_URL, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(payload)
            });
            if (!response.ok) {
                throw new Error(`API request failed with status ${response.status}`);
            }
            const data = await response.json();
            sendButton.disabled = false;
            return data.candidates[0].content.parts[0].text;
        }

        function addMessage(text, sender) {
            const messageDiv = document.createElement('div');
            messageDiv.className = `p-3 rounded-lg max-w-lg ${sender === 'user' ? 'chat-bubble-user' : 'chat-bubble-ai'}`;
            messageDiv.textContent = text;
            chatMessages.appendChild(messageDiv);
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }

        function addTypingIndicator() {
            const indicator = document.createElement('div');
            indicator.id = 'typing-indicator';
            indicator.className = 'p-3 rounded-lg max-w-lg chat-bubble-ai';
            indicator.innerHTML = '<div class="flex items-center gap-2"><div class="w-2 h-2 bg-gray-400 rounded-full animate-pulse"></div><div class="w-2 h-2 bg-gray-400 rounded-full animate-pulse [animation-delay:0.2s]"></div><div class="w-2 h-2 bg-gray-400 rounded-full animate-pulse [animation-delay:0.4s]"></div></div>';
            chatMessages.appendChild(indicator);
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }

        function removeTypingIndicator() {
            const indicator = document.getElementById('typing-indicator');
            if (indicator) indicator.remove();
        }

        endCoachingButton.addEventListener('click', async () => {
            if (conversationHistory.length < 2) {
                alert('코칭 대화를 조금 더 진행한 후 분석해주세요.');
                return;
            }
            showScreen('coaching-report');
            reportContent.innerHTML = '<p class="text-center">AI가 코칭 내용을 분석 중입니다. 잠시만 기다려주세요...</p>';
            
            const transcript = conversationHistory
                .map(msg => `${msg.role === 'user' ? '리더' : currentPersona.name}: ${msg.parts[0].text}`)
                .join('\n');
            
            const analysisPrompt = `
                당신은 최고의 리더십 코칭 전문가입니다. 아래 제공되는 코칭 대화 스크립트를 분석하여 리포트를 작성해주세요.
                분석은 다음 5가지 핵심 코칭 역량을 기준으로 진행합니다:
                1. 신뢰 구축 (Rapport & Trust): 안전하고 지지적인 환경을 조성했는가?
                2. 적극적 경청 (Active Listening): 팀원의 말을 경청하고 그 의미를 파악했는가?
                3. 강력한 질문 (Powerful Questioning): 스스로 생각하고 통찰을 얻도록 하는 개방형 질문을 사용했는가?
                4. 인식 창출 (Creating Awareness): 팀원이 새로운 관점을 갖도록 도왔는가?
                5. 실행 계획 (Designing Actions): 구체적인 행동 계획을 세우도록 지원했는가?

                리포트는 다음 형식에 맞춰 Markdown으로 작성해주세요:
                ### 코칭 대화 요약
                (대화의 전체적인 흐름을 1~2문장으로 요약)

                ### 5대 역량 기반 분석
                - **신뢰 구축:** (분석 내용)
                - **적극적 경청:** (분석 내용)
                - **강력한 질문:** (분석 내용)
                - **인식 창출:** (분석 내용)
                - **실행 계획:** (분석 내용)

                ### 총평 및 제언
                #### 잘한 점 (Strengths)
                - (구체적인 대화 내용을 인용하며 칭찬)
                
                #### 개선점 (Areas for Improvement)
                - (구체적인 대화 내용을 인용하며 개선할 점 제안)

                ### 개선된 대화 스크립트 예시
                (개선이 필요한 특정 대화 부분을 더 효과적인 코칭 대화로 다시 작성하여 제시)

                ---
                [코칭 대화 스크립트]
                ${transcript}
            `;
            
            try {
                const analysisResult = await callGeminiAPI(analysisPrompt, [{ role: 'user', parts: [{ text: '분석해주세요.' }] }]);
                reportContent.innerHTML = marked.parse(analysisResult);
            } catch (error) {
                reportContent.innerHTML = '<p class="text-red-500">리포트 생성 중 오류가 발생했습니다. 다시 시도해주세요.</p>';
                console.error('Analysis Error:', error);
            }
        });
        
        backToPersonasButton.addEventListener('click', () => {
            showScreen('persona-selection');
        });

        downloadReportButton.addEventListener('click', () => {
            const reportHtml = `
                <!DOCTYPE html>
                <html lang="ko">
                <head>
                    <meta charset="UTF-8">
                    <title>AI 코칭 분석 리포트 - ${currentPersona.name}</title>
                    <style>
                        body { font-family: sans-serif; line-height: 1.6; margin: 2rem; }
                        h1, h2, h3 { color: #333; }
                        div { border: 1px solid #eee; padding: 1rem; border-radius: 8px; background: #f9f9f9; }
                    </style>
                </head>
                <body>
                    <h1>AI 코칭 분석 리포트</h1>
                    <h2>대상: ${currentPersona.name} (${currentPersona.title})</h2>
                    <div>${reportContent.innerHTML}</div>
                </body>
                </html>
            `;
            const blob = new Blob([reportHtml], { type: 'text/html' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = `coaching_report_${currentPersona.name}.html`;
            link.click();
            URL.revokeObjectURL(link.href);
        });

        // 초기화
        populateQuestions();
        populatePersonas();
    </script>
</body>
</html>
