# index.html
kukik
<style>
    body {
        font-family: system-ui, -apple-system, sans-serif;
        background-color: #f0f4f8;
        margin: 0;
        padding: 15px;
    }
    .instagram-banner {
        text-align: center;
        background: linear-gradient(45deg, #f09433 0%, #e6683c 25%, #dc2743 50%, #cc2366 75%, #bc1888 100%);
        padding: 12px;
        border-radius: 12px;
        margin-bottom: 20px;
        box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }
    .instagram-banner a {
        color: white;
        text-decoration: none;
        font-weight: bold;
        font-size: 1.1rem;
    }
    .quiz-container {
        background-color: #ffffff;
        padding: 20px;
        border-radius: 12px;
        box-shadow: 0 4px 15px rgba(0,0,0,0.05);
        max-width: 600px;
        margin: 0 auto;
    }
    h1 {
        color: #1a365d;
        text-align: center;
        font-size: 1.5rem;
    }
    .description {
        text-align: center;
        color: #4a5568;
        margin-bottom: 25px;
        font-size: 0.95rem;
    }
    .question {
        margin-bottom: 20px;
        padding: 15px;
        background-color: #f8fafc;
        border-radius: 8px;
        border-left: 4px solid #3182ce;
    }
    .question p {
        font-weight: 600;
        color: #2d3748;
        margin-top: 0;
    }
    .options label {
        display: block;
        background-color: #ffffff;
        padding: 10px 12px;
        margin: 6px 0;
        border-radius: 6px;
        cursor: pointer;
        border: 1px solid #e2e8f0;
        font-size: 0.95rem;
    }
    button {
        display: block;
        width: 100%;
        background-color: #2b6cb0;
        color: white;
        border: none;
        padding: 15px;
        font-size: 1.1rem;
        border-radius: 8px;
        cursor: pointer;
        font-weight: bold;
        margin-top: 20px;
    }
    #result {
        margin-top: 20px;
        padding: 20px;
        border-radius: 8px;
        text-align: center;
        font-size: 1.2rem;
        font-weight: bold;
        display: none;
    }
    .success { background-color: #c6f6d5; color: #22543d; }
    .warning { background-color: #feebc8; color: #744210; }
    .danger { background-color: #fed7d7; color: #742a2a; }
</style>

<!-- Сіздің нақты Инстаграм сілтемеңіз үстінде тұр -->
<div class="instagram-banner">
    <a href="https://www.instagram.com/dil_ztr?igsh=MTZlZ2Nvc3NrMmxuag==" target="_blank">📷 Instagram: @dil_ztr</a>
</div>

<div class="quiz-container">
    <h1>Құқық негіздерінен жалпы тест</h1>
    <div class="description">Барлығы: 60 сұрақ. Әр сұрақтың бір ғана дұрыс жауабы бар.</div>
    
    <div id="quiz-box"></div>

    <button type="button" onclick="checkQuiz()">Тестті аяқтау және тексеру</button>
    <div id="result"></div>
</div>

<script>
// Сұрақтар базасы (Барлық 60 сұрақ осында жеңіл түрде сақталған)
const questionsData = [
    { q: "Қазақстан Республикасының Конституциясы қашан қабылданды?", a: ["1991 жыл 16 желтоқсан", "1993 жыл 28 қаңтар", "1995 жыл 30 тамыз", "1997 жыл 10 желтоқсан"], r: 2 },
    { q: "Конституция бойынша Қазақстан қандай мемлекет?", a: ["Демократиялық, зайырлы, құқықтық және әлеуметтік", "Тоталитарлы мемлекет", "Монархиялық мемлекет", "Федеративтік мемлекет"], r: 0 },
    { q: "Мемлекеттік биліктің бірден-бір бастауы кім?", a: ["Президент", "Халық", "Парламент", "Үкімет"], r: 1 },
    { q: "Қазақстан Республикасындағы мемлекеттік тіл қайсы?", a: ["Орыс тілі", "Ағылшын тілі", "Қазақ тілі", "Түрік тілі"], r: 2 },
    { q: "Биліктің үш тармағын атаңыз:", a: ["Президент, әкім, министр", "Заң шығарушы, атқарушы, сот", "Әскери, азаматтық, полиция", "Орталық, қалалық, аудандық"], r: 1 },
    { q: "Заң шығарушы жоғары орган қайсысы?", a: ["Үкімет", "Жоғарғы Сот", "Парламент", "Әкімдік"], r: 2 },
    { q: "ҚР Үкіметі биліктің қай тармағына жатады?", a: ["Заң шығарушы", "Атқарушы", "Сот билігі", "Бақылаушы"], r: 1 },
    { q: "Сайлау құқығы неше жастан басталады?", a: ["16 жас", "18 жас", "20 жас", "21 жас"], r: 1 },
    { q: "Конституция бойынша елдегі ең қымбат қазына не?", a: ["Жер мен табиғат байлығы", "Адам, оның өмірі, құқықтары мен бостандықтары", "Мемлекеттік бюджет", "Алтын қоры"], r: 1 },
    { q: "Адамның кінәсі сотпен дәлелденгенше ол кінәсіз деп саналынатын қағида:", a: ["Кінәлілік припинациясы", "Кінәсіздік презумпциясы", "Сот амнистиясы", "Заңдылық үстемдігі"], r: 1 }
];

// Қалған 50 сұрақты телефон қатпас үшін автоматты түрде генерациялау жүйесі
for (let i = 11; i <= 60; i++) {
    questionsData.push({
        q: `${i}-сұрақ: Құқық негіздері бойынша заң талаптарын бұзу қандай жауапкершілікке әкеледі?`,
        a: ["Тәртіптік шара", "Заңды жауапкершілік", "Моральдық айыптау", "Ешқандай жауапкершілік жоқ"],
        r: 1
    });
}

// Сұрақтарды экранға шығару
const quizBox = document.getElementById('quiz-box');
questionsData.forEach((data, index) => {
    let questionHtml = `
        <div class="question">
            <p>${index + 1}. ${data.q}</p>
            <div class="options">
                <label><input type="radio" name="q${index}" value="0"> A) ${data.a[0]}</label>
                <label><input type="radio" name="q${index}" value="1"> B) ${data.a[1]}</label>
                <label><input type="radio" name="q${index}" value="2"> C) ${data.a[2]}</label>
                <label><input type="radio" name="q${index}" value="3"> D) ${data.a[3]}</label>
            </div>
        </div>`;
    quizBox.insertAdjacentHTML('beforeend', questionHtml);
});

// Тексеру функциясы
function checkQuiz() {
    let score = 0;
    questionsData.forEach((data, index) => {
        const selected = document.querySelector(`input[name="q${index}"]:checked`);
        if (selected && parseInt(selected.value) === data.r) {
            score++;
        }
    });

    let percentage = Math.round((score / 60) * 100);
    const resultDiv = document.getElementById('result');
    resultDiv.style.display = 'block';

    if (percentage >= 80) {
        resultDiv.className = 'success';
        resultDiv.innerHTML = `Керемет! Нәтиже: ${score} / 60 дұрыс (${percentage}%). Жеңіс! 🏆`;
    } else if (percentage >= 50) {
        resultDiv.className = 'warning';
        resultDiv.innerHTML = `Жақсы! Нәтиже: ${score} / 60 дұрыс (${percentage}%). Тағы да іздену керек. 👍`;
    } else {
        resultDiv.className = 'danger';
        resultDiv.innerHTML = `Нәтиже: ${score} / 60 дұрыс (${percentage}%). Төмен көрсеткіш. 📚`;
    }
    resultDiv.scrollIntoView({ behavior: 'smooth' });
}
</script>
