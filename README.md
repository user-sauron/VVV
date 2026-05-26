[tasks.html](https://github.com/user-attachments/files/28250754/tasks.html)
# VVV<!DOCTYPE html>
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
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
        }

        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 30px;
            font-size: 2em;
        }

        h2 {
            color: #555;
            margin-bottom: 20px;
            font-size: 1.5em;
        }

        .role-switcher {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
            justify-content: center;
        }

        .role-btn {
            padding: 12px 30px;
            border: 2px solid #667eea;
            background: white;
            color: #667eea;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            transition: all 0.3s;
        }

        .role-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3);
        }

        .role-btn.active {
            background: #667eea;
            color: white;
        }

        .task-form {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .task-form input,
        .task-form textarea {
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 16px;
            transition: border 0.3s;
        }

        .task-form input:focus,
        .task-form textarea:focus {
            outline: none;
            border-color: #667eea;
        }

        .task-form textarea {
            min-height: 100px;
            resize: vertical;
            font-family: inherit;
        }

        #addTaskBtn {
            padding: 15px;
            background: #667eea;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
        }

        #addTaskBtn:hover {
            background: #5568d3;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }

        #filterEmployee {
            width: 100%;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 16px;
            margin-bottom: 20px;
        }

        #filterEmployee:focus {
            outline: none;
            border-color: #667eea;
        }

        .tasks-container {
            margin-top: 40px;
        }

        #tasksList {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .task-item {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 10px;
            border-left: 5px solid #667eea;
            transition: all 0.3s;
        }

        .task-item:hover {
            transform: translateX(5px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }

        .task-item.completed {
            opacity: 0.7;
            border-left-color: #28a745;
            background: #e8f5e9;
        }

        .task-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }

        .task-title {
            font-size: 1.2em;
            font-weight: 600;
            color: #333;
        }

        .task-status {
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.85em;
            font-weight: 600;
        }

        .status-pending {
            background: #fff3cd;
            color: #856404;
        }

        .status-completed {
            background: #d4edda;
            color: #155724;
        }

        .task-description {
            color: #666;
            margin-bottom: 10px;
            line-height: 1.5;
        }

        .task-employee {
            color: #667eea;
            font-weight: 600;
            margin-bottom: 15px;
        }

        .task-actions {
            display: flex;
            gap: 10px;
        }

        .complete-btn,
        .delete-btn {
            padding: 10px 20px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 14px;
            font-weight: 600;
            transition: all 0.3s;
        }

        .complete-btn {
            background: #28a745;
            color: white;
        }

        .complete-btn:hover {
            background: #218838;
            transform: translateY(-2px);
        }

        .delete-btn {
            background: #dc3545;
            color: white;
        }

        .delete-btn:hover {
            background: #c82333;
            transform: translateY(-2px);
        }

        .panel {
            margin-bottom: 30px;
        }

        .empty-state {
            text-align: center;
            color: #999;
            padding: 40px;
            font-size: 1.1em;
        }
    </style>
</head>
<body>
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
            <div id="tasksList"></div>
        </div>
    </div>

    <script>
        let tasks = JSON.parse(localStorage.getItem('tasks')) || [];
        let currentRole = 'manager';
        let filterName = '';

        const managerBtn = document.getElementById('managerBtn');
        const employeeBtn = document.getElementById('employeeBtn');
        const managerPanel = document.getElementById('managerPanel');
        const employeePanel = document.getElementById('employeePanel');
        const tasksList = document.getElementById('tasksList');
        const addTaskBtn = document.getElementById('addTaskBtn');
        const filterEmployee = document.getElementById('filterEmployee');

        managerBtn.addEventListener('click', () => {
            currentRole = 'manager';
            managerBtn.classList.add('active');
            employeeBtn.classList.remove('active');
            managerPanel.style.display = 'block';
            employeePanel.style.display = 'none';
            filterName = '';
            renderTasks();
        });

        employeeBtn.addEventListener('click', () => {
            currentRole = 'employee';
            employeeBtn.classList.add('active');
            managerBtn.classList.remove('active');
            managerPanel.style.display = 'none';
            employeePanel.style.display = 'block';
            renderTasks();
        });

        filterEmployee.addEventListener('input', (e) => {
            filterName = e.target.value.toLowerCase().trim();
            renderTasks();
        });

        addTaskBtn.addEventListener('click', () => {
            const title = document.getElementById('taskTitle').value.trim();
            const description = document.getElementById('taskDescription').value.trim();
            const employee = document.getElementById('employeeName').value.trim();

            if (!title || !employee) {
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

            tasks.push(task);
            saveTasks();
            renderTasks();

            document.getElementById('taskTitle').value = '';
            document.getElementById('taskDescription').value = '';
            document.getElementById('employeeName').value = '';
        });

        function toggleComplete(id) {
            const task = tasks.find(t => t.id === id);
            if (task) {
                task.completed = !task.completed;
                saveTasks();
                renderTasks();
            }
        }

        function deleteTask(id) {
            if (confirm('Вы уверены, что хотите удалить эту задачу?')) {
                tasks = tasks.filter(t => t.id !== id);
                saveTasks();
                renderTasks();
            }
        }

        function saveTasks() {
            localStorage.setItem('tasks', JSON.stringify(tasks));
        }

        function renderTasks() {
            let filteredTasks = tasks;

            if (currentRole === 'employee' && filterName) {
                filteredTasks = tasks.filter(task => 
                    task.employee.toLowerCase().includes(filterName)
                );
            }

            if (filteredTasks.length === 0) {
                tasksList.innerHTML = '<div class="empty-state">📭 Задач пока нет</div>';
                return;
            }

            tasksList.innerHTML = filteredTasks.map(task => `
                <div class="task-item ${task.completed ? 'completed' : ''}">
                    <div class="task-header">
                        <div class="task-title">${task.title}</div>
                        <span class="task-status ${task.completed ? 'status-completed' : 'status-pending'}">
                            ${task.completed ? '✅ Выполнено' : '⏳ В работе'}
                        </span>
                    </div>
                    ${task.description ? `<div class="task-description">${task.description}</div>` : ''}
                    <div class="task-employee">👤 Сотрудник: ${task.employee}</div>
                    <div style="font-size: 0.85em; color: #999; margin-bottom: 15px;">
                        📅 Создано: ${task.createdAt}
                    </div>
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

        renderTasks();
    </script>
</body>
</html>
