<!DOCTYPE html>
<html dir="rtl" lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام الاختبار الرقمي</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f5f5f5;
            margin: 0;
            padding: 20px;
            direction: rtl;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        h1, h2 {
            color: #2c3e50;
            text-align: center;
        }
        .login-section, .quiz-section, .results-section, .admin-section {
            margin-bottom: 20px;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        .hidden {
            display: none;
        }
        input, button, select {
            padding: 10px;
            margin: 10px 0;
            width: 100%;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
        }
        button {
            background-color: #3498db;
            color: white;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #2980b9;
        }
        .question {
            margin-bottom: 20px;
            padding: 15px;
            border: 1px solid #eee;
            border-radius: 5px;
        }
        .options {
            margin-top: 10px;
        }
        .option {
            margin: 5px 0;
        }
        .result-item {
            margin: 10px 0;
            padding: 10px;
            background-color: #f9f9f9;
            border-radius: 5px;
        }
        .correct {
            color: green;
        }
        .incorrect {
            color: red;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 10px;
            text-align: right;
        }
        th {
            background-color: #f2f2f2;
        }
        .admin-login {
            max-width: 400px;
            margin: 20px auto;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 5px;
            background-color: #f9f9f9;
        }
        .tab-buttons {
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
        }
        .tab-button {
            padding: 10px 20px;
            margin: 0 5px;
            background-color: #ddd;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            width: auto;
        }
        .tab-button.active {
            background-color: #3498db;
            color: white;
        }
        .tab {
            display: none;
        }
        .tab.active {
            display: block;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>نظام الاختبار الرقمي</h1>
        
        <!-- صفحة تسجيل الدخول -->
        <div id="loginSection" class="login-section">
            <h2>تسجيل الدخول</h2>
            <input type="text" id="studentId" placeholder="رقم الطالب">
            <input type="password" id="password" placeholder="كلمة المرور">
            <button id="loginBtn">تسجيل الدخول</button>
            <p id="loginMessage"></p>
            <div class="admin-login">
                <h3>تسجيل دخول المشرف</h3>
                <input type="password" id="adminPassword" placeholder="كلمة مرور المشرف">
                <button id="adminLoginBtn">دخول كمشرف</button>
            </div>
        </div>

        <!-- قسم الاختبار -->
        <div id="quizSection" class="quiz-section hidden">
            <h2 id="welcomeMessage"></h2>
            <div id="timer">الوقت المتبقي: <span id="timeLeft">30:00</span></div>
            <div id="questions"></div>
            <button id="submitQuizBtn">إرسال الإجابات</button>
        </div>

        <!-- قسم النتائج -->
        <div id="resultsSection" class="results-section hidden">
            <h2>نتائج الاختبار</h2>
            <div id="quizResults"></div>
            <div id="totalScore"></div>
            <button id="backToLoginBtn">العودة إلى صفحة تسجيل الدخول</button>
        </div>

        <!-- قسم المشرف -->
        <div id="adminSection" class="admin-section hidden">
            <h2>لوحة تحكم المشرف</h2>
            
            <div class="tab-buttons">
                <button class="tab-button active" data-tab="studentsData">بيانات الطلاب</button>
                <button class="tab-button" data-tab="manageStudents">إدارة الطلاب</button>
                <button class="tab-button" data-tab="manageQuestions">إدارة الأسئلة</button>
                <button class="tab-button" data-tab="exportData">تصدير البيانات</button>
            </div>
            
            <div id="studentsData" class="tab active">
                <h3>بيانات الطلاب ونتائجهم</h3>
                <table id="studentsTable">
                    <thead>
                        <tr>
                            <th>رقم الطالب</th>
                            <th>الاسم</th>
                            <th>الدرجة</th>
                            <th>وقت الإكمال</th>
                        </tr>
                    </thead>
                    <tbody id="studentsTableBody">
                        <!-- سيتم ملء هذا الجزء بالبيانات عن طريق JavaScript -->
                    </tbody>
                </table>
            </div>
            
            <div id="manageStudents" class="tab">
                <h3>إضافة طالب جديد</h3>
                <input type="text" id="newStudentId" placeholder="رقم الطالب">
                <input type="text" id="newStudentName" placeholder="اسم الطالب">
                <input type="password" id="newStudentPassword" placeholder="كلمة المرور">
                <button id="addStudentBtn">إضافة طالب</button>
                
                <h3>حذف طالب</h3>
                <select id="studentToDelete">
                    <option value="">اختر طالباً للحذف</option>
                    <!-- سيتم ملء هذا الجزء بالبيانات عن طريق JavaScript -->
                </select>
                <button id="deleteStudentBtn">حذف الطالب</button>
            </div>
            
            <div id="manageQuestions" class="tab">
                <h3>إضافة سؤال جديد</h3>
                <input type="text" id="newQuestionText" placeholder="نص السؤال">
                <div id="newOptions">
                    <input type="text" class="new-option" placeholder="الخيار 1">
                    <input type="text" class="new-option" placeholder="الخيار 2">
                    <input type="text" class="new-option" placeholder="الخيار 3">
                    <input type="text" class="new-option" placeholder="الخيار 4">
                </div>
                <select id="correctOption">
                    <option value="0">الإجابة الصحيحة: الخيار 1</option>
                    <option value="1">الإجابة الصحيحة: الخيار 2</option>
                    <option value="2">الإجابة الصحيحة: الخيار 3</option>
                    <option value="3">الإجابة الصحيحة: الخيار 4</option>
                </select>
                <button id="addQuestionBtn">إضافة السؤال</button>
                
                <h3>حذف سؤال</h3>
                <select id="questionToDelete">
                    <option value="">اختر سؤالاً للحذف</option>
                    <!-- سيتم ملء هذا الجزء بالبيانات عن طريق JavaScript -->
                </select>
                <button id="deleteQuestionBtn">حذف السؤال</button>
            </div>
            
            <div id="exportData" class="tab">
                <h3>تصدير بيانات الطلاب</h3>
                <button id="exportCsvBtn">تصدير كملف CSV</button>
                <button id="printDataBtn">طباعة البيانات</button>
            </div>
            
            <button id="adminLogoutBtn">تسجيل الخروج</button>
        </div>
    </div>

    <script>
        // البيانات الافتراضية للنظام
        const adminPass = "admin123"; // كلمة مرور المشرف - يمكنك تغييرها
        
        // بيانات الطلاب (رقم الطالب، الاسم، كلمة المرور)
        let students = [
            { id: "1001", name: "أحمد محمد", password: "pass1001", score: null, completionTime: null, answers: [] },
            { id: "1002", name: "سارة أحمد", password: "pass1002", score: null, completionTime: null, answers: [] },
            { id: "1003", name: "محمد علي", password: "pass1003", score: null, completionTime: null, answers: [] }
        ];
        
        // أسئلة الاختبار
        let questions = [
            {
                text: "ما هي عاصمة المملكة العربية السعودية؟",
                options: ["جدة", "الرياض", "مكة", "المدينة"],
                correctAnswer: 1
            },
            {
                text: "كم عدد كواكب المجموعة الشمسية؟",
                options: ["7", "8", "9", "10"],
                correctAnswer: 1
            },
            {
                text: "من هو مؤلف كتاب 'في ظلال القرآن'؟",
                options: ["محمد الغزالي", "سيد قطب", "يوسف القرضاوي", "محمد متولي الشعراوي"],
                correctAnswer: 1
            },
            {
                text: "ما هو أطول نهر في العالم؟",
                options: ["النيل", "الأمازون", "المسيسيبي", "اليانغتسي"],
                correctAnswer: 0
            },
            {
                text: "في أي عام تأسست المملكة العربية السعودية الحديثة؟",
                options: ["1902", "1925", "1932", "1945"],
                correctAnswer: 2
            }
        ];
        
        // المتغيرات العامة للنظام
        let currentUser = null;
        let quizTime = 30 * 60; // 30 دقيقة
        let timer;
        
        // عناصر واجهة المستخدم
        const loginSection = document.getElementById('loginSection');
        const quizSection = document.getElementById('quizSection');
        const resultsSection = document.getElementById('resultsSection');
        const adminSection = document.getElementById('adminSection');
        
        // تحميل البيانات من التخزين المحلي إذا كانت متوفرة
        function loadData() {
            const savedStudents = localStorage.getItem('quizStudents');
            const savedQuestions = localStorage.getItem('quizQuestions');
            
            if (savedStudents) {
                students = JSON.parse(savedStudents);
            }
            
            if (savedQuestions) {
                questions = JSON.parse(savedQuestions);
            }
            
            updateStudentDropdowns();
            updateQuestionDropdowns();
            updateStudentsTable();
        }
        
        // حفظ البيانات في التخزين المحلي
        function saveData() {
            localStorage.setItem('quizStudents', JSON.stringify(students));
            localStorage.setItem('quizQuestions', JSON.stringify(questions));
        }
        
        // تسجيل دخول الطالب
        document.getElementById('loginBtn').addEventListener('click', function() {
            const studentId = document.getElementById('studentId').value;
            const password = document.getElementById('password').value;
            const loginMessage = document.getElementById('loginMessage');
            
            const student = students.find(s => s.id === studentId && s.password === password);
            
            if (student) {
                if (student.score !== null) {
                    loginMessage.textContent = "لقد أكملت الاختبار بالفعل. لا يمكنك إعادة الامتحان.";
                    loginMessage.style.color = "red";
                    return;
                }
                
                currentUser = student;
                document.getElementById('welcomeMessage').textContent = `مرحباً، ${student.name}!`;
                loginSection.classList.add('hidden');
                quizSection.classList.remove('hidden');
                
                // عرض الأسئلة
                displayQuestions();
                
                // بدء المؤقت
                startTimer();
            } else {
                loginMessage.textContent = "رقم الطالب أو كلمة المرور غير صحيحة";
                loginMessage.style.color = "red";
            }
        });
        
        // تسجيل دخول المشرف
        document.getElementById('adminLoginBtn').addEventListener('click', function() {
            const password = document.getElementById('adminPassword').value;
            
            if (password === adminPass) {
                loginSection.classList.add('hidden');
                adminSection.classList.remove('hidden');
                updateStudentsTable();
            } else {
                alert("كلمة مرور المشرف غير صحيحة");
            }
        });
        
        // عرض أسئلة الاختبار
        function displayQuestions() {
            const questionsContainer = document.getElementById('questions');
            questionsContainer.innerHTML = '';
            
            questions.forEach((q, qIndex) => {
                const questionDiv = document.createElement('div');
                questionDiv.className = 'question';
                questionDiv.innerHTML = `
                    <p><strong>السؤال ${qIndex + 1}:</strong> ${q.text}</p>
                    <div class="options">
                        ${q.options.map((opt, optIndex) => `
                            <div class="option">
                                <input type="radio" name="q${qIndex}" id="q${qIndex}opt${optIndex}" value="${optIndex}">
                                <label for="q${qIndex}opt${optIndex}">${opt}</label>
                            </div>
                        `).join('')}
                    </div>
                `;
                questionsContainer.appendChild(questionDiv);
            });
        }
        
        // بدء مؤقت الاختبار
        function startTimer() {
            let timeRemaining = quizTime;
            updateTimerDisplay(timeRemaining);
            
            timer = setInterval(() => {
                timeRemaining--;
                updateTimerDisplay(timeRemaining);
                
                if (timeRemaining <= 0) {
                    clearInterval(timer);
                    submitQuiz();
                }
            }, 1000);
        }
        
        // تحديث عرض المؤقت
        function updateTimerDisplay(seconds) {
            const minutes = Math.floor(seconds / 60);
            const remainingSeconds = seconds % 60;
            document.getElementById('timeLeft').textContent = `${minutes.toString().padStart(2, '0')}:${remainingSeconds.toString().padStart(2, '0')}`;
        }
        
        // إرسال الاختبار
        document.getElementById('submitQuizBtn').addEventListener('click', submitQuiz);
        
        function submitQuiz() {
            clearInterval(timer);
            
            // جمع الإجابات
            const userAnswers = [];
            questions.forEach((_, qIndex) => {
                const selectedOption = document.querySelector(`input[name="q${qIndex}"]:checked`);
                userAnswers.push(selectedOption ? parseInt(selectedOption.value) : null);
            });
            
            // حساب الدرجة
            let score = 0;
            const results = userAnswers.map((answer, index) => {
                const isCorrect = answer === questions[index].correctAnswer;
                if (isCorrect) score++;
                return {
                    question: questions[index].text,
                    userAnswer: answer !== null ? questions[index].options[answer] : "لم تتم الإجابة",
                    correctAnswer: questions[index].options[questions[index].correctAnswer],
                    isCorrect
                };
            });
            
            // تخزين النتائج
            currentUser.score = score;
            currentUser.answers = userAnswers;
            currentUser.completionTime = new Date().toLocaleString('ar-SA');
            saveData();
            
            // عرض النتائج
            displayResults(results, score);
            
            // الانتقال إلى صفحة النتائج
            quizSection.classList.add('hidden');
            resultsSection.classList.remove('hidden');
        }
        
        // عرض نتائج الاختبار
        function displayResults(results, score) {
            const resultsContainer = document.getElementById('quizResults');
            resultsContainer.innerHTML = '';
            
            results.forEach((result, index) => {
                const resultDiv = document.createElement('div');
                resultDiv.className = 'result-item';
                resultDiv.innerHTML = `
                    <p><strong>السؤال ${index + 1}:</strong> ${result.question}</p>
                    <p>إجابتك: ${result.userAnswer}</p>
                    <p>الإجابة الصحيحة: ${result.correctAnswer}</p>
                    <p class="${result.isCorrect ? 'correct' : 'incorrect'}">
                        ${result.isCorrect ? '✓ صحيح' : '✗ خطأ'}
                    </p>
                `;
                resultsContainer.appendChild(resultDiv);
            });
            
            document.getElementById('totalScore').textContent = `الدرجة النهائية: ${score} من ${questions.length}`;
        }
        
        // العودة إلى صفحة تسجيل الدخول
        document.getElementById('backToLoginBtn').addEventListener('click', function() {
            currentUser = null;
            resultsSection.classList.add('hidden');
            loginSection.classList.remove('hidden');
            document.getElementById('studentId').value = '';
            document.getElementById('password').value = '';
            document.getElementById('loginMessage').textContent = '';
        });
        
        // تسجيل خروج المشرف
        document.getElementById('adminLogoutBtn').addEventListener('click', function() {
            adminSection.classList.add('hidden');
            loginSection.classList.remove('hidden');
            document.getElementById('adminPassword').value = '';
        });
        
        // تحديث جدول بيانات الطلاب
        function updateStudentsTable() {
            const tableBody = document.getElementById('studentsTableBody');
            tableBody.innerHTML = '';
            
            students.forEach(student => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${student.id}</td>
                    <td>${student.name}</td>
                    <td>${student.score !== null ? `${student.score}/${questions.length}` : 'لم يكمل الاختبار'}</td>
                    <td>${student.completionTime || '-'}</td>
                `;
                tableBody.appendChild(row);
            });
        }
        
        // تحديث القوائم المنسدلة للطلاب
        function updateStudentDropdowns() {
            const studentToDelete = document.getElementById('studentToDelete');
            studentToDelete.innerHTML = '<option value="">اختر طالباً للحذف</option>';
            
            students.forEach(student => {
                const option = document.createElement('option');
                option.value = student.id;
                option.textContent = `${student.id} - ${student.name}`;
                studentToDelete.appendChild(option);
            });
        }
        
        // تحديث القوائم المنسدلة للأسئلة
        function updateQuestionDropdowns() {
            const questionToDelete = document.getElementById('questionToDelete');
            questionToDelete.innerHTML = '<option value="">اختر سؤالاً للحذف</option>';
            
            questions.forEach((question, index) => {
                const option = document.createElement('option');
                option.value = index;
                option.textContent = `السؤال ${index + 1}: ${question.text.substring(0, 30)}${question.text.length > 30 ? '...' : ''}`;
                questionToDelete.appendChild(option);
            });
        }
        
        // إضافة طالب جديد
        document.getElementById('addStudentBtn').addEventListener('click', function() {
            const id = document.getElementById('newStudentId').value;
            const name = document.getElementById('newStudentName').value;
            const password = document.getElementById('newStudentPassword').value;
            
            if (!id || !name || !password) {
                alert("يرجى ملء جميع الحقول");
                return;
            }
            
            if (students.some(s => s.id === id)) {
                alert("رقم الطالب موجود بالفعل");
                return;
            }
            
            students.push({
                id,
                name,
                password,
                score: null,
                completionTime: null,
                answers: []
            });
            
            saveData();
            updateStudentsTable();
            updateStudentDropdowns();
            
            // إعادة تعيين الحقول
            document.getElementById('newStudentId').value = '';
            document.getElementById('newStudentName').value = '';
            document.getElementById('newStudentPassword').value = '';
            
            alert("تمت إضافة الطالب بنجاح");
        });
        
        // حذف طالب
        document.getElementById('deleteStudentBtn').addEventListener('click', function() {
            const studentId = document.getElementById('studentToDelete').value;
            
            if (!studentId) {
                alert("يرجى اختيار طالب للحذف");
                return;
            }
            
            if (confirm("هل أنت متأكد من حذف هذا الطالب؟")) {
                const index = students.findIndex(s => s.id === studentId);
                if (index !== -1) {
                    students.splice(index, 1);
                    saveData();
                    updateStudentsTable();
                    updateStudentDropdowns();
                    alert("تم حذف الطالب بنجاح");
                }
            }
        });
        
        // إضافة سؤال جديد
        document.getElementById('addQuestionBtn').addEventListener('click', function() {
            const text = document.getElementById('newQuestionText').value;
            const optionInputs = document.querySelectorAll('.new-option');
            const options = Array.from(optionInputs).map(input => input.value);
            const correctAnswer = parseInt(document.getElementById('correctOption').value);
            
            if (!text || options.some(opt => !opt)) {
                alert("يرجى ملء جميع الحقول");
                return;
            }
            
            questions.push({
                text,
                options,
                correctAnswer
            });
            
            saveData();
            updateQuestionDropdowns();
            
            // إعادة تعيين الحقول
            document.getElementById('newQuestionText').value = '';
            optionInputs.forEach(input => input.value = '');
            document.getElementById('correctOption').value = '0';
            
            alert("تمت إضافة السؤال بنجاح");
        });
        
        // حذف سؤال
        document.getElementById('deleteQuestionBtn').addEventListener('click', function() {
            const questionIndex = document.getElementById('questionToDelete').value;
            
            if (!questionIndex) {
                alert("يرجى اختيار سؤال للحذف");
                return;
            }
            
            if (confirm("هل أنت متأكد من حذف هذا السؤال؟")) {
                questions.splice(parseInt(questionIndex), 1);
                saveData();
                updateQuestionDropdowns();
                alert("تم حذف السؤال بنجاح");
            }
        });
        
        // تصدير البيانات كملف CSV
        document.getElementById('exportCsvBtn').addEventListener('click', function() {
            let csvContent = "رقم الطالب,الاسم,الدرجة,وقت الإكمال\n";
            
            students.forEach(student => {
                csvContent += `${student.id},"${student.name}",${student.score !== null ? student.score : ''},${student.completionTime || ''}\n`;
            });
            
            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const url = URL.createObjectURL(blob);
            const link = document.createElement('a');
            link.setAttribute('href', url);
            link.setAttribute('download', 'بيانات_الطلاب.csv');
            link.style.visibility = 'hidden';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        });
        
        // طباعة البيانات
        document.getElementById('printDataBtn').addEventListener('click', function() {
            window.print();
        });
        
        // وظيفة التنقل بين علامات التبويب
        const tabButtons = document.querySelectorAll('.tab-button');
        tabButtons.forEach(button => {
            button.addEventListener('click', function() {
                const tabId = this.getAttribute('data-tab');
                
                // تعطيل جميع علامات التبويب
                document.querySelectorAll('.tab').forEach(tab => {
                    tab.classList.remove('active');
                });
                
                // تعطيل جميع الأزرار
                tabButtons.forEach(btn => {
                    btn.classList.remove('active');
                });
                
                // تفعيل علامة التبويب والزر المحدد
                document.getElementById(tabId).classList.add('active');
                this.classList.add('active');
            });
        });
        
        // تحميل البيانات عند بدء التطبيق
        loadData();
    </script>
</body>
</html>
