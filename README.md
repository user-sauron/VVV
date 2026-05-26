<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Управление задачами</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
            overflow-x: hidden;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            animation: containerAppear 0.8s cubic-bezier(0.68, -0.55, 0.265, 1.55);
            position: relative;
            overflow: hidden;
        }

        @keyframes containerAppear {
            0% {
                opacity: 0;
                transform: translateY(50px) scale(0.9);
            }
            100% {
                opacity: 1;
                transform: translateY(0) scale(1);
            }
        }

        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 30px;
            font-size: 2.2em;
            animation: titleWave 2s ease-in-out infinite;
        }

        @keyframes titleWave {
            0%, 100% {
                transform: translateY(0);
            }
            50% {
                transform: translateY(-5px);
            }
        }

        h2 {
            color: #555;
            margin-bottom: 20px;
            font-size: 1.5em;
            opacity: 0;
            animation: fadeInUp 0.5s forwards;
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* Переключатель роли */
        .role-switcher {
            display: flex;
            gap: 15px;
            margin-bottom: 30px;
            justify-content: center;
            animation: roleSwitcherAppear 0.6s 0.2s backwards;
        }

        @keyframes roleSwitcherAppear {
            from {
                opacity: 0;
                transform: scale(0.8);
            }
        }

        .role-btn {
            padding: 14px 35px;
            border: none;
            background: linear-gradient(145deg, #e0e0e0, #ffffff);
            color: #667eea;
            border-radius: 30px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.1);
            position: relative;
            overflow: hidden;
        }

        .role-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
            transition: 0.5s;
        }

        .role-btn:hover::before {
            left: 100%;
        }

        .role-btn:hover {
            transform: translateY(-5px) scale(1.05);
            box-shadow: 10px 10px 20px rgba(102, 126, 234, 0.3);
        }

        .role-btn.active {
            background: linear-gradient(145deg, #667eea, #764ba2);
            color: white;
            transform: translateY(-2px);
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% {
                box-shadow: 0 0 0 0 rgba(102, 126, 234, 0.7);
            }
            50% {
                box-shadow: 0 0 0 10px rgba(102, 126, 234, 0);
            }
        }

        /* Форма создания задачи */
        .task-form {
            display: flex;
            flex-direction: column;
            gap: 15px;
            animation: formSlideIn 0.8s 0.3s backwards;
        }

        @keyframes formSlideIn {
            from {
                opacity: 0;
                transform: translateX(-50px);
            }
        }

        .task-form input,
        .task-form textarea {
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 12px;
            font-size: 16px;
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            background: linear-gradient(to right, #f8f9fa, #ffffff);
        }

        .task-form input:focus,
        .task-form textarea:focus {
            outline: none;
            border-color: #667eea;
            transform: scale(1.02);
            box-shadow: 0 0 20px rgba(102, 126, 234, 0.2);
        }

        .task-form textarea {
            min-height: 120px;
            resize: vertical;
            font-family: inherit;
        }

        #addTaskBtn {
            padding: 18px;
            background: linear-gradient(145deg, #667eea, #764ba2);
            color: white;
            border: none;
            border-radius: 12px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            position: relative;
            overflow: hidden;
            animation: buttonPulse 3s infinite;
        }

        @keyframes buttonPulse {
            0%, 100% {
                box-shadow: 0 5px 20px rgba(102, 126, 234, 0.4);
            }
            50% {
                box-shadow: 0 5px 30px rgba(102, 126, 234, 0.6);
            }
        }

        #addTaskBtn:hover {
            transform: translateY(-4px) scale(1.05);
            box-shadow: 0 15px 35px rgba(102, 126, 234, 0.4);
        }

        #addTaskBtn:active {
            transform: translateY(0) scale(0.98);
        }

        /* Фильтр */
        #filterEmployee {
            width: 100%;
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 12px;
            font-size: 16px;
            margin-bottom: 20px;
            transition: all 0.4s;
            animation: filterSlide 0.6s 0.4s backwards;
        }

        @keyframes filterSlide {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
        }

        #filterEmployee:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 25px rgba(102, 126, 234, 0.3);
        }

        /* Список задач */
        .tasks-container {
            margin-top: 40px;
            animation: listAppear 0.7s 0.5s backwards;
        }

        @keyframes listAppear {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
        }

        #tasksList {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .task-item {
            background: linear-gradient(145deg, #f8f9fa, #ffffff);
            padding: 25px;
            border-radius: 15px;
            border-left: 6px solid #667eea;
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            position: relative;
            overflow: hidden;
            animation: taskAppear 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }

        @keyframes taskAppear {
            0% {
                opacity: 0;
                transform: translateX(-50px) rotate(-5deg);
            }
            100% {
                opacity: 1;
                transform: translateX(0) rotate(0);
            }
        }

        .task-item::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 50%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(102, 126, 234, 0.1), transparent);
            transform: skewX(-25deg);
            animation: shine 3s infinite;
        }

        @keyframes shine {
            0% {
                left: -100%;
            }
            100% {
                left: 150%;
            }
        }

        .task-item:hover {
            transform: translateY(-8px) translateX(5px);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.15);
        }

        .task-item.completed {
            opacity: 0.8;
            border-left-color: #28a745;
            background: linear-gradient(145deg, #e8f5e9, #f1f8e9);
        }

        .task-item.completed::after {
            content: '';
            position: absolute;
            top: 0;
            right: 0;
            width: 0;
            height: 0;
            border-top: 50px solid #28a745;
            border-left: 50px solid transparent;
            animation: cornerFold 0.5s forwards;
        }

        @keyframes cornerFold {
            to {
                border-top-width: 60px;
                border-left-width: 60px;
            }
        }

        .task-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px;
        }

        .task-title {
            font-size: 1.3em;
            font-weight: 600;
            color: #333;
            position: relative;
        }

        .task-status {
            padding: 8px 18px;
            border-radius: 25px;
            font-size: 0.85em;
            font-weight: 600;
            animation: statusPulse 2s infinite;
        }

        @keyframes statusPulse {
            0%, 100% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.05);
            }
        }

        .status-pending {
            background: linear-gradient(145deg, #fff3cd, #ffeaa7);
            color: #856404;
        }

        .status-completed {
            background: linear-gradient(145deg, #d4edda, #c3e6cb);
            color: #155724;
            animation: completionBounce 0.6s;
        }

        @keyframes completionBounce {
            0% {
                transform: scale(0.3);
                opacity: 0;
            }
            50% {
                transform: scale(1.1);
            }
            100% {
                transform: scale(1);
                opacity: 1;
            }
        }

        .task-description {
            color: #666;
            margin-bottom: 12px;
            line-height: 1.6;
            animation: descriptionFade 0.5s 0.2s backwards;
        }

        @keyframes descriptionFade {
            from {
                opacity: 0;
                transform: translateX(-20px);
            }
        }

        .task-employee {
            color: #667eea;
            font-weight: 600;
            margin-bottom: 18px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .task-employee::before {
            content: '👤';
            animation: employeeIconSpin 10s linear infinite;
        }

        @keyframes employeeIconSpin {
            0% {
                transform: rotate(0deg);
            }
            100% {
                transform: rotate(360deg);
            }
        }

        .task-date {
            font-size: 0.85em;
            color: #999;
            margin-bottom: 18px;
            animation: dateFade 0.4s 0.3s backwards;
        }

        @keyframes dateFade {
            from {
                opacity: 0;
                transform: translateY(10px);
            }
        }

        .task-actions {
            display: flex;
            gap: 12px;
            flex-wrap: wrap;
        }

        .complete-btn,
        .delete-btn {
            padding: 12px 24px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-size: 15px;
            font-weight: 600;
            transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            position: relative;
            overflow: hidden;
        }

        .complete-btn {
            background: linear-gradient(145deg, #28a745, #218838);
            color: white;
        }

        .complete-btn:hover {
            transform: translateY(-3px) scale(1.05);
            box-shadow: 0 10px 20px rgba(40, 167, 69, 0.4);
        }

        .complete-btn::after {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 5px;
            height: 5px;
            background: rgba(255, 255, 255, 0.5);
            opacity: 0;
            border-radius: 100%;
            transform: scale(1, 1) translate(-50%);
            transform-origin: 50% 50%;
        }

        .complete-btn:focus:not(:active)::after {
            animation: ripple 1s ease-out;
        }

        @keyframes ripple {
            0% {
                transform: scale(0, 0);
                opacity: 0.5;
            }
            100% {
                transform: scale(40, 40);
                opacity: 0;
            }
        }

        .delete-btn {
            background: linear-gradient(145deg, #dc3545, #c82333);
            color: white;
        }

        .delete-btn:hover {
            transform: translateY(-3px) scale(1.05);
            box-shadow: 0 10px 20px rgba(220, 53, 69, 0.4);
        }

        .delete-btn:active {
            animation: shake 0.5s;
        }

        @keyframes shake {
            0%, 100% {
                transform: translateX(0) rotate(0);
            }
            25% {
                transform: translateX(-5px) rotate(-5deg);
            }
            75% {
                transform: translateX(5px) rotate(5deg);
            }
        }

        .panel {
            margin-bottom: 30px;
            animation: panelSlide 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }

        @keyframes panelSlide {
            from {
                opacity: 0;
                transform: translateY(-20px);
            }
        }

        .empty-state {
            text-align: center;
            color: #999;
            padding: 50px;
            font-size: 1.2em;
            animation: emptyStatePulse 2s infinite;
        }

        @keyframes emptyStatePulse {
            0%, 100% {
                opacity: 0.7;
                transform: scale(1);
            }
            50% {
                opacity: 1;
                transform: scale(1.02);
            }
        }

        /* Декоративные элементы */
        .floating {
            animation: floating 3s ease-in-out infinite;
        }

        @keyframes floating {
            0%, 100% {
                transform: translateY(0px);
            }
            50% {
                transform: translateY(-10px);
            }
        }

        .background-decoration {
            position: fixed;
            width: 200px;
            height: 200px;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.1);
            pointer-events: none;
            z-index: -1;
        }

        .decoration-1 {
            top: 10%;
            left: 10%;
            animation: float 6s ease-in-out infinite;
        }

        .decoration-2 {
            top: 60%;
            right: 10%;
            animation: float 8s ease-in-out infinite reverse;
            width: 150px;
            height: 150px;
        }

        @keyframes float {
            0%, 100% {
                transform: translate(0, 0) rotate(0deg);
            }
            33% {
                transform: translate(30px, -50px) rotate(120deg);
            }
            66% {
                transform: translate(-20px, 20px) rotate(240deg);
            }
        }
    </style>
</head>
<body>
    <!-- Декоративные элементы -->
    <div class="background-decoration decoration-1"></div>
    <div class="background-decoration decoration-2"></div>

    <div class="container">
        <h1 class="floating">📋 Система управления задачами</h1>
        
        <div class="role-switcher">
            <button id="managerBtn" class="role-btn active">Руководитель</button>
            <button id="employeeBtn" class="role-btn">Сотрудник</button>
        </div>

        <div id="managerPanel" class="panel">
            <h2>Создать новую задачу</h2>
            <div class="task-form">
                <input type="text" id="taskTitle" placeholder="Название задачи" />
                <textarea id="taskDescription" placeholder="Описание задачи"></textarea>
                <input type="text" id="employeeName" placeholder="Имя сотрудника" />
                <button id="addTaskBtn">➕ Добавить задачу</button>
            </div>
        </div>

        <div id="employeePanel" class="panel" style="display: none;">
            <h2>Мои задачи</h2>
            <input type="text" id="filterEmployee" placeholder="Введите ваше имя для фильтрации" />
        </div>

        <div class="tasks-container">
            <h2>Все задачи</h2>
            <div id="tasksList"></div>
        </div>
    </div>

    <script>
        // Инициализация
        let tasks = JSON.parse(localStorage.getItem('tasks')) || [];
        let currentRole = 'manager';
        let filterName = '';

        // Элементы DOM
        const managerBtn = document.getElementById('managerBtn');
        const employeeBtn = document.getElementById('employeeBtn');
        const managerPanel = document.getElementById('managerPanel');
        const employeePanel = document.getElementById('employeePanel');
        const tasksList = document.getElementById('tasksList');
        const addTaskBtn = document.getElementById('addTaskBtn');
        const filterEmployee = document.getElementById('filterEmployee');

        // Переключение роли с анимацией
        managerBtn.addEventListener('click', () => {
            currentRole = 'manager';
            managerBtn.classList.add('active');
            employeeBtn.classList.remove('active');
            
            // Анимация переключения
            managerPanel.style.animation = 'none';
            employeePanel.style.animation = 'none';
            
            setTimeout(() => {
                managerPanel.style.display = 'block';
                employeePanel.style.display = 'none';
                managerPanel.style.animation = 'panelSlide 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55)';
            }, 100);
            
            filterName = '';
            filterEmployee.value = '';
            renderTasks();
        });

        employeeBtn.addEventListener('click', () => {
            currentRole = 'employee';
            employeeBtn.classList.add('active');
            managerBtn.classList.remove('active');
            
            // Анимация переключения
            managerPanel.style.animation = 'none';
            employeePanel.style.animation = 'none';
            
            setTimeout(() => {
                managerPanel.style.display = 'none';
                employeePanel.style.display = 'block';
                employeePanel.style.animation = 'panelSlide 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55)';
            }, 100);
            
            renderTasks();
        });

        // Фильтр с анимацией
        filterEmployee.addEventListener('input', (e) => {
            filterName = e.target.value.toLowerCase().trim();
            renderTasks();
            
            // Анимация при вводе
            e.target.style.transform = 'scale(1.02)';
            setTimeout(() => {
                e.target.style.transform = 'scale(1)';
            }, 200);
        });

        // Добавление задачи с анимацией успеха
        addTaskBtn.addEventListener('click', () => {
            const title = document.getElementById('taskTitle').value.trim();
            const description = document.getElementById('taskDescription').value.trim();
            const employee = document.getElementById('employeeName').value.trim();

            if (!title || !employee) {
                // Анимация ошибки
                const btn = document.getElementById('addTaskBtn');
                btn.style.background = 'linear-gradient(145deg, #dc3545, #c82333)';
                btn.style.animation = 'shake 0.5s';
                
                setTimeout(() => {
                    btn.style.background = 'linear-gradient(145deg, #667eea, #764ba2)';
                    btn.style.animation = 'buttonPulse 3s infinite';
                }, 500);
                
                alert('Пожалуйста, заполните название задачи и имя сотрудника!');
                return;
            }

            const task = {
                id: Date.now(),
                title,
                description,
                employee,
                completed: false,
                createdAt: new Date().toLocaleString('ru-RU')
            };

            // Анимация успешного добавления
            addTaskBtn.style.transform = 'scale(0.95)';
            addTaskBtn.style.background = 'linear-gradient(145deg, #28a745, #218838)';
            
            setTimeout(() => {
                tasks.push(task);
                saveTasks();
                renderTasks();
                
                // Очистка формы с анимацией
                const inputs = document.querySelectorAll('.task-form input, .task-form textarea');
                inputs.forEach((input, index) => {
                    setTimeout(() => {
                        input.style.transform = 'scale(0.98)';
                        input.value = '';
                        setTimeout(() => {
                            input.style.transform = 'scale(1)';
                        }, 100);
                    }, index * 100);
                });
                
                // Возврат кнопки в исходное состояние
                addTaskBtn.style.transform = 'scale(1)';
                addTaskBtn.style.background = 'linear-gradient(145deg, #667eea, #764ba2)';
            }, 300);
        });

        // Отметка о выполнении с анимацией
        function toggleComplete(id) {
            const task = tasks.find(t => t.id === id);
            if (task) {
                const taskElement = document.querySelector(`[data-id="${id}"]`);
                
                // Анимация изменения статуса
                if (taskElement) {
                    taskElement.style.transform = 'scale(0.98)';
                    taskElement.style.opacity = '0.8';
                    
                    setTimeout(() => {
                        task.completed = !task.completed;
                        saveTasks();
                        
                        // Анимация успешного выполнения
                        if (task.completed) {
                            const statusElement = taskElement.querySelector('.task-status');
                            if (statusElement) {
                                statusElement.style.animation = 'none';
                                setTimeout(() => {
                                    statusElement.style.animation = 'completionBounce 0.6s';
                                }, 10);
                            }
                        }
                        
                        taskElement.style.transform = 'scale(1)';
                        taskElement.style.opacity = '1';
                        
                        setTimeout(() => {
                            renderTasks();
                        }, 300);
                    }, 300);
                } else {
                    task.completed = !task.completed;
                    saveTasks();
                    renderTasks();
                }
            }
        }

        // Удаление задачи с анимацией
        function deleteTask(id) {
            const taskElement = document.querySelector(`[data-id="${id}"]`);
            
            if (taskElement) {
                // Анимация удаления
                taskElement.style.transform = 'translateX(100px) rotate(10deg)';
                taskElement.style.opacity = '0';
                taskElement.style.transition = 'all 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55)';
                
                setTimeout(() => {
                    tasks = tasks.filter(t => t.id !== id);
                    saveTasks();
                    renderTasks();
                }, 500);
            } else {
                tasks = tasks.filter(t => t.id !== id);
                saveTasks();
                renderTasks();
            }
        }

        // Сохранение в localStorage
        function saveTasks() {
            localStorage.setItem('tasks', JSON.stringify(tasks));
        }

        // Отрисовка задач с анимацией
        function renderTasks() {
            let filteredTasks = tasks;

            // Фильтрация для сотрудника
            if (currentRole === 'employee' && filterName) {
                filteredTasks = tasks.filter(task => 
                    task.employee.toLowerCase().includes(filterName)
                );
            }

            if (filteredTasks.length === 0) {
                tasksList.innerHTML = '<div class="empty-state">📭 Задач пока нет</div>';
                return;
            }

            // Сортировка: сначала невыполненные, потом по дате
            filteredTasks.sort((a, b) => {
                if (a.completed !== b.completed) {
                    return a.completed ? 1 : -1;
                }
                return new Date(b.createdAt) - new Date(a.createdAt);
            });

            tasksList.innerHTML = filteredTasks.map((task, index) => `
                <div class="task-item ${task.completed ? 'completed' : ''}" 
                     data-id="${task.id}"
                     style="animation-delay: ${index * 0.1}s">
                    <div class="task-header">
                        <div class="task-title">${task.title}</div>
                        <span class="task-status ${task.completed ? 'status-completed' : 'status-pending'}">
                            ${task.completed ? '✅ Выполнено' : '⏳ В работе'}
                        </span>
                    </div>
                    ${task.description ? `<div class="task-description">${task.description}</div>` : ''}
                    <div class="task-employee">Сотрудник: ${task.employee}</div>
                    <div class="task-date">📅 Создано: ${task.createdAt}</div>
                    <div class="task-actions">
                        ${!task.completed ? `
                            <button class="complete-btn" onclick="toggleComplete(${task.id})">
                                ✓ Отметить выполненной
                            </button>
                        ` : `
                            <button class="complete-btn" onclick="toggleComplete(${task.id})">
                                ↶ Вернуть в работу
                            </button>
                        `}
                        ${currentRole === 'manager' ? `
                            <button class="delete-btn" onclick="deleteTask(${task.id})">
                                🗑 Удалить
                            </button>
                        ` : ''}
                    </div>
                </div>
            `).join('');
        }

        // Добавляем интерактивные эффекты при наведении
        document.addEventListener('mousemove', (e) => {
            const container = document.querySelector('.container');
            if (container) {
                const rect = container.getBoundingClientRect();
                const x = e.clientX - rect.left - rect.width / 2;
                const y = e.clientY - rect.top - rect.height / 2;
                
                container.style.transform = `
                    perspective(1000px)
                    rotateY(${x * 0.01}deg)
                    rotateX(${-y * 0.01}deg)
                    translateZ(10px)
                `;
            }
        });

        // Возвращаем контейнер в исходное положение при уходе мыши
        document.addEventListener('mouseleave', () => {
            const container = document.querySelector('.container');
            if (container) {
                container.style.transform = 'perspective(1000px) rotateY(0deg) rotateX(0deg) translateZ(0)';
            }
        });

        // Первоначальная отрисовка
        renderTasks();

        // Добавляем звуковые эффекты (опционально)
        function playSound(type) {
            const audioContext = new (window.AudioContext || window.webkitAudioContext)();
            const oscillator = audioContext.createOscillator();
            const gainNode = audioContext.createGain();
            
            oscillator.connect(gainNode);
            gainNode.connect(audioContext.destination);
            
            if (type === 'success') {
                oscillator.frequency.setValueAtTime(523.25, audioContext.currentTime); // C5
                oscillator.frequency.setValueAtTime(659.25, audioContext.currentTime + 0.1); // E5
                oscillator.frequency.setValueAtTime(783.99, audioContext.currentTime + 0.2); // G5
            } else if (type === 'error') {
                oscillator.frequency.setValueAtTime(196, audioContext.currentTime); // G3
                oscillator.frequency.setValueAtTime(174.61, audioContext.currentTime + 0.1); // F3
            } else if (type === 'delete') {
                oscillator.frequency.setValueAtTime(880, audioContext.currentTime); // A5
                oscillator.frequency.setValueAtTime(698.46, audioContext.currentTime + 0.1); // F5
                oscillator.frequency.setValueAtTime(523.25, audioContext.currentTime + 0.2); // C5
            }
            
            gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
            gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.3);
            
            oscillator.start(audioContext.currentTime);
            oscillator.stop(audioContext.currentTime + 0.3);
        }

        // Раскомментируйте для звуков:
        // addTaskBtn.addEventListener('click', () => setTimeout(() => playSound('success'), 300));
        // document.addEventListener('click', (e) => {
        //     if (e.target.classList.contains('delete-btn')) playSound('delete');
        // });
    </script>
</body>
</html>
