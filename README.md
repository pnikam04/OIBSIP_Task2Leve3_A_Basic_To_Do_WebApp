# OIBSIP_Task2Leve3_A_Basic_To_Do_WebApp
I Developed this A Basic To Do Web App using HTML &amp; CSS , JAVA SCRIPT
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>To-Do App</title>
    <style>
      @import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap");

      body {
        font-family: "Roboto", sans-serif;
        background-color: #f0f2f5;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        margin: 0;
      }

      .container {
        background-color: #fff;
        padding: 30px;
        border-radius: 10px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        width: 450px;
        text-align: center;
      }

      h1 {
        margin-bottom: 20px;
        color: #333;
        font-weight: 700;
      }

      .input-section {
        display: flex;
        justify-content: center;
        margin-bottom: 20px;
      }

      input[type="text"] {
        width: 70%;
        padding: 10px 15px;
        border: 1px solid #ddd;
        border-radius: 4px;
        font-size: 16px;
      }

      button {
        padding: 10px 20px;
        border: none;
        background-color: #28a745;
        color: white;
        border-radius: 4px;
        cursor: pointer;
        margin-left: 10px;
        font-size: 16px;
        font-weight: 500;
      }

      button:hover {
        background-color: #218838;
      }

      .tasks-section {
        margin-bottom: 20px;
      }

      ul {
        list-style-type: none;
        padding: 0;
        margin: 0;
      }

      li {
        background-color: #f9f9f9;
        padding: 10px 15px;
        border: 1px solid #ddd;
        border-radius: 4px;
        margin-bottom: 10px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        transition: background-color 0.3s;
      }

      li:hover {
        background-color: #f1f1f1;
      }

      li.completed {
        text-decoration: line-through;
        color: #888;
      }

      li button {
        margin-left: 10px;
        padding: 5px 10px;
        font-size: 14px;
      }

      .edit-btn,
      .delete-btn,
      .complete-btn {
        border: none;
        color: white;
        border-radius: 4px;
        cursor: pointer;
        font-weight: 500;
      }

      .edit-btn {
        background-color: #007bff;
      }

      .delete-btn {
        background-color: #dc3545;
      }

      .complete-btn {
        background-color: #28a745;
      }

      .edit-btn:hover {
        background-color: #0056b3;
      }

      .delete-btn:hover {
        background-color: #c82333;
      }

      .complete-btn:hover {
        background-color: #218838;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <h1>To-Do List</h1>
      <div class="input-section">
        <input type="text" id="task-input" placeholder="Add a new task" />
        <button onclick="addTask()">Add Task</button>
      </div>
      <div class="tasks-section">
        <h2>Pending Tasks</h2>
        <ul id="pending-tasks"></ul>
      </div>
      <div class="tasks-section">
        <h2>Completed Tasks</h2>
        <ul id="completed-tasks"></ul>
      </div>
    </div>
    <script>
      let pendingTasks = [];
      let completedTasks = [];

      function addTask() {
        const taskInput = document.getElementById("task-input");
        const taskText = taskInput.value.trim();

        if (taskText === "") {
          alert("Please enter a task.");
          return;
        }

        const task = {
          text: taskText,
          id: Date.now(),
          completed: false,
        };

        pendingTasks.push(task);
        taskInput.value = "";
        renderTasks();
      }

      function renderTasks() {
        const pendingTasksList = document.getElementById("pending-tasks");
        const completedTasksList = document.getElementById("completed-tasks");

        pendingTasksList.innerHTML = "";
        completedTasksList.innerHTML = "";

        pendingTasks.forEach((task) => {
          const li = createTaskElement(task);
          pendingTasksList.appendChild(li);
        });

        completedTasks.forEach((task) => {
          const li = createTaskElement(task);
          li.classList.add("completed");
          completedTasksList.appendChild(li);
        });
      }

      function createTaskElement(task) {
        const li = document.createElement("li");
        li.innerText = task.text;

        const completeBtn = document.createElement("button");
        completeBtn.innerText = "Complete";
        completeBtn.className = "complete-btn";
        completeBtn.onclick = () => completeTask(task.id);

        const editBtn = document.createElement("button");
        editBtn.innerText = "Edit";
        editBtn.className = "edit-btn";
        editBtn.onclick = () => editTask(task.id);

        const deleteBtn = document.createElement("button");
        deleteBtn.innerText = "Delete";
        deleteBtn.className = "delete-btn";
        deleteBtn.onclick = () => deleteTask(task.id);

        li.appendChild(completeBtn);
        li.appendChild(editBtn);
        li.appendChild(deleteBtn);

        return li;
      }

      function completeTask(taskId) {
        const taskIndex = pendingTasks.findIndex((task) => task.id === taskId);
        if (taskIndex !== -1) {
          const [task] = pendingTasks.splice(taskIndex, 1);
          task.completed = true;
          completedTasks.push(task);
          renderTasks();
        }
      }

      function editTask(taskId) {
        const taskIndex = pendingTasks.findIndex((task) => task.id === taskId);
        if (taskIndex !== -1) {
          const task = pendingTasks[taskIndex];
          const newText = prompt("Edit task:", task.text);
          if (newText) {
            task.text = newText;
            renderTasks();
          }
        } else {
          const taskIndex = completedTasks.findIndex(
            (task) => task.id === taskId
          );
          if (taskIndex !== -1) {
            const task = completedTasks[taskIndex];
            const newText = prompt("Edit task:", task.text);
            if (newText) {
              task.text = newText;
              renderTasks();
            }
          }
        }
      }

      function deleteTask(taskId) {
        const taskIndex = pendingTasks.findIndex((task) => task.id === taskId);
        if (taskIndex !== -1) {
          pendingTasks.splice(taskIndex, 1);
          renderTasks();
        } else {
          const taskIndex = completedTasks.findIndex(
            (task) => task.id === taskId
          );
          if (taskIndex !== -1) {
            completedTasks.splice(taskIndex, 1);
            renderTasks();
          }
        }
      }
    </script>
  </body>
</html>
