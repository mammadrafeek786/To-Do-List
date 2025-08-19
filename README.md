# To-Do-List
new repositary
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>To-Do List</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(to right, #a8edea, #fed6e3);
      display: flex;
      justify-content: center;
      align-items: flex-start;
      min-height: 100vh;
      margin: 0;
      padding: 20px;
    }
    .todo-container {
      background: white;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.2);
      width: 100%;
      max-width: 400px;
    }
    h1 {
      text-align: center;
      margin-bottom: 20px;
      color: #333;
    }
    .input-group {
      display: flex;
      gap: 8px;
    }
    input {
      flex: 1;
      padding: 10px;
      border: 2px solid #ddd;
      border-radius: 6px;
      font-size: 16px;
    }
    button {
      padding: 10px 16px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      background: #4CAF50;
      color: white;
      font-size: 16px;
    }
    button:hover {
      background: #45a049;
    }
    ul {
      list-style: none;
      padding: 0;
      margin-top: 20px;
    }
    li {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 10px;
      margin-bottom: 8px;
      border-radius: 6px;
      background: #f9f9f9;
      transition: background 0.3s;
    }
    li.completed span {
      text-decoration: line-through;
      color: gray;
    }
    .actions {
      display: flex;
      gap: 6px;
    }
    .delete-btn {
      background: #e53935;
    }
    .delete-btn:hover {
      background: #c62828;
    }
    .complete-btn {
      background: #2196f3;
    }
    .complete-btn:hover {
      background: #1976d2;
    }
  </style>
</head>
<body>
  <div class="todo-container">
    <h1>To-Do List ✅</h1>
    <div class="input-group">
      <input type="text" id="taskInput" placeholder="Enter new task..."/>
      <button onclick="addTask()">Add</button>
    </div>
    <ul id="taskList"></ul>
  </div>

  <script>
    const taskInput = document.getElementById("taskInput");
    const taskList = document.getElementById("taskList");

    // Load tasks from localStorage
    let tasks = JSON.parse(localStorage.getItem("tasks")) || [];
    renderTasks();

    function addTask() {
      const text = taskInput.value.trim();
      if (text === "") return;

      const task = { text, completed: false };
      tasks.push(task);
      taskInput.value = "";
      saveAndRender();
    }

    function toggleTask(index) {
      tasks[index].completed = !tasks[index].completed;
      saveAndRender();
    }

    function deleteTask(index) {
      tasks.splice(index, 1);
      saveAndRender();
    }

    function saveAndRender() {
      localStorage.setItem("tasks", JSON.stringify(tasks));
      renderTasks();
    }

    function renderTasks() {
      taskList.innerHTML = "";
      tasks.forEach((task, index) => {
        const li = document.createElement("li");
        li.className = task.completed ? "completed" : "";

        const span = document.createElement("span");
        span.textContent = task.text;

        const actions = document.createElement("div");
        actions.className = "actions";

        const completeBtn = document.createElement("button");
        completeBtn.textContent = task.completed ? "Undo" : "Done";
        completeBtn.className = "complete-btn";
        completeBtn.onclick = () => toggleTask(index);

        const deleteBtn = document.createElement("button");
        deleteBtn.textContent = "Delete";
        deleteBtn.className = "delete-btn";
        deleteBtn.onclick = () => deleteTask(index);

        actions.appendChild(completeBtn);
        actions.appendChild(deleteBtn);

        li.appendChild(span);
        li.appendChild(actions);
        taskList.appendChild(li);
      });
    }
  </script>
</body>
</html>
