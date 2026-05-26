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
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            animation: containerAppear 0.8s cubic-bezier(0.68, -0.55, 0.265, 1.55);
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
        }

        h2 {
            color: #555;
            margin-bottom: 20px;
            font-size: 1.5em;
        }

        .role-switcher {
            display: flex;
            gap: 15px;
            margin-bottom: 30px;
            justify-content: center;
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
            transition: all 0.3s;
            box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.1);
        }

        .role-btn:hover {
            transform: translateY(-5px) scale(1.05);
        }

        .role-btn.active {
            background: linear-gradient(145deg, #667eea, #764ba2);
            color: white;
        }

        .task-form {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .task-form input,
        .task-form textarea {
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 12px;
            font-size: 16px;
            transition: all 0.4s;
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
            transition: all 0.3s;
        }

        #addTaskBtn:hover {
            transform: translateY(-4px) scale(1.05);
            box-shadow: 0 15px 35px rgba(102, 126, 234, 0.4);
        }

        #filterEmployee {
            width: 100%;
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 12px;
            font-size: 16px;
            margin-bottom: 20px;
            transition: all 0.4s;
        }

        #filterEmployee:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 25px rgba(102, 126, 234, 0.3);
        }

        .tasks-container {
            margin-top: 40px;
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
            transition: all 0.4s;
            animation: taskAppear 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }

        @keyframes taskAppear {
            0% {
                opacity: 0;
                transform: translateX(-50px);
            }
            100% {
                opacity: 1;
                transform: translateX(0);
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
        }

        .task-status {
            padding: 8px 18px;
            border-radius: 25px;
            font-size: 0.85em;
            font-weight: 600;
        }

        .status-pending {
            background: linear-gradient(145deg, #fff3cd, #ffeaa7);
            color: #856404;
        }

        .status-completed {
            background: linear-gradient(145deg, #d4edda, #c3e6cb);
            color: #155724;
        }

        .task-description {
            color: #666;
            margin-bottom: 12px;
            line-height: 1.6;
        }

        .task-employee {
            color: #667eea;
            font-weight: 600;
            margin-bottom: 18px;
        }

        .task-date {
            font-size: 0.85em;
            color: #999;
            margin-bottom: 18px;
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
            transition: all 0.3s;
        }

        .complete-btn {
            background: linear-gradient(145deg, #28a745, #218838);
            color: white;
        }

        .complete-btn:hover {
            transform: translateY(-3px) scale(1.05);
            box-shadow: 0 10px 20px rgba(40, 167, 69, 0.4);
        }

        .delete-btn {
            background: linear-gradient(145deg, #dc3545, #c82333);
            color: white;
        }

        .delete-btn:hover {
            transform: translateY(-3px) scale(1.05);
            box-shadow: 0 10px 20px rgba(220, 53, 69, 0.4);
        }

        .panel {
            margin-bottom: 30px;
        }

        .empty-state {
            text-align: center;
            color: #999;
            padding: 50px;
            font-size: 1.2em;
        }

        .loading {
            text-align: center;
            padding: 40px;
            color: #667eea;
            font-size: 1.1em;
        }

        .sync-indicator {
            position: fixed;
            top: 20px;
            right: 20px;
            background: #28a745;
            color: white;
            padding: 10px 20px;
            border-radius: 25px;
            font-size: 14px;
            font-weight: 600;
            display: none;
            animation: fadeIn 0.3s;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
    </style>
</head>
<body>
    <div class="sync-indicator" id="syncIndicator">✓ Синхронизировано</div>

    <div class="container">
        <h1>📋 Система управления задачами</h1>
        
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
            <div id="tasksList">
                <div class="loading">⏳ Загрузка задач...</div>
            </div>
        </div>
    </div>

    <!-- Firebase SDK -->
    <script type="module">
        import { initializeApp } from 'https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js';
        import { getDatabase, ref, push, set, onValue, remove, update } from 'https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js';

        // 🔥 ЗАМЕНИТЕ ЭТИ ДАННЫЕ НА СВОИ (инструкция ниже)
        const firebaseConfig = {
            apiKey: "ВАШ_API_KEY",
            authDomain: "ВАШ_PROJECT.firebaseapp.com",
            databaseURL: "https://ВАШ_PROJECT.firebaseio.com",
            projectId: "ВАШ_PROJECT_ID",
            storageBucket: "ВАШ_PROJECT.appspot.com",
            messagingSenderId: "ВАШ_ID",
            appId: "ВАШ_APP_ID"
        };

        // Инициализация Firebase
        const app = initializeApp(firebaseConfig);
        const database = getDatabase(app);
        const tasksRef = ref(database, 'tasks');

        // Переменные
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
        const syncIndicator = document.getElementById('syncIndicator');

        // Показать индикатор синхронизации
        function showSync() {
            syncIndicator.style.display = 'block';
            setTimeout(() => {
                syncIndicator.style.display = 'none';
            }, 2000);
        }

        // Переключение роли
        managerBtn.addEventListener('click', () => {
            currentRole = 'manager';
            managerBtn.classList.add('active');
            employeeBtn.classList.remove('active');
            managerPanel.style.display = 'block';
            employeePanel.style.display = 'none';
            filterName = '';
        });

        employeeBtn.addEventListener('click', () => {
            currentRole = 'employee';
            employeeBtn.classList.add('active');
            managerBtn.classList.remove('active');
            managerPanel.style.display = 'none';
            employeePanel.style.display = 'block';
        });

        // Фильтр
        filterEmployee.addEventListener('input', (e) => {
            filterName = e.target.value.toLowerCase().trim();
        });

        // Добавление задачи
        addTaskBtn.addEventListener('click', () => {
            const title = document.getElementById('taskTitle').value.trim();
            const description = document.getElementById('taskDescription').value.trim();
            const employee = document.getElementById('employeeName').value.trim();

            if (!title || !employee) {
                alert('Пожалуйста, заполните название задачи и имя сотрудника!');
                return;
            }

            const newTaskRef = push(tasksRef);
            set(newTaskRef, {
                title,
                description,
                employee,
                completed: false,
                createdAt: new Date().toLocaleString('ru-RU')
            }).then(() => {
                document.getElementById('taskTitle').value = '';
                document.getElementById('taskDescription').value = '';
                document.getElementById('employeeName').value = '';
                showSync();
            });
        });

        // Отметка о выполнении
        window.toggleComplete = function(taskId, currentStatus) {
            const taskRef = ref(database, `tasks/${taskId}`);
            update(taskRef, {
                completed: !currentStatus
            }).then(() => {
                showSync();
            });
        };

        // Удаление задачи
        window.deleteTask = function(taskId) {
            if (confirm('Вы уверены, что хотите удалить эту задачу?')) {
                const taskRef = ref(database, `tasks/${taskId}`);
                remove(taskRef).then(() => {
                    showSync();
                });
            }
        };

        // Слушаем изменения в реальном времени
        onValue(tasksRef, (snapshot) => {
            const data = snapshot.val();
            
            if (!data) {
                tasksList.innerHTML = '<div class="empty-state">📭 Задач пока нет</div>';
                return;
            }

            // Преобразуем объект в массив
            let tasksArray = Object.keys(data).map(key => ({
                id: key,
                ...data[key]
            }));

            // Фильтрация
            if (currentRole === 'employee' && filterName) {
                tasksArray = tasksArray.filter(task => 
                    task.employee.toLowerCase().includes(filterName)
                );
            }

            // Сортировка
            tasksArray.sort((a, b) => {
                if (a.completed !== b.completed) {
                    return a.completed ? 1 : -1;
                }
                return new Date(b.createdAt) - new Date(a.createdAt);
            });

            // Отрисовка
            tasksList.innerHTML = tasksArray.map(task => `
                <div class="task-item ${task.completed ? 'completed' : ''}">
                    <div class="task-header">
                        <div class="task-title">${task.title}</div>
                        <span class="task-status ${task.completed ? 'status-completed' : 'status-pending'}">
                            ${task.completed ? '✅ Выполнено' : '⏳ В работе'}
                        </span>
                    </div>
                    ${task.description ? `<div class="task-description">${task.description}</div>` : ''}
                    <div class="task-employee">👤 Сотрудник: ${task.employee}</div>
                    <div class="task-date">📅 Создано: ${task.createdAt}</div>
                    <div class="task-actions">
                        <button class="complete-btn" onclick="toggleComplete('${task.id}', ${task.completed})">
                            ${task.completed ? '↶ Вернуть в работу' : '✓ Отметить выполненной'}
                        </button>
                        ${currentRole === 'manager' ? `
                            <button class="delete-btn" onclick="deleteTask('${task.id}')">
                                🗑 Удалить
                            </button>
                        ` : ''}
                    </div>
                </div>
            `).join('');
        });
    </script>
</body>
</html>
const firebaseConfig = {
    apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXX",  // ← Вставьте свои данные
    authDomain: "tasks-app.firebaseapp.com",
    databaseURL: "https://tasks-app.firebaseio.com",
    projectId: "tasks-app",
    storageBucket: "tasks-app.appspot.com",
    messagingSenderId: "123456789",
    appId: "1:123456789:web:xxxxx"
};
