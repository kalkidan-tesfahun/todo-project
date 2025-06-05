<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple To-Do List</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>My To-Do List</h1>

        <div class="task-input">
            <input type="text" id="newTaskInput" placeholder="Add a new task...">
            <button id="addTaskBtn">Add Task</button>
        </div>

        <ul id="taskList">
            </ul>
    </div>

    <script src="script.js"></script>
</body>
</html>

body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    display: flex;
    justify-content: center;
    align-items: flex-start; /* Align to top, not center */
    min-height: 100vh;
    margin: 20px; /* Add some margin around the container */
    box-sizing: border-box; /* Include padding and border in element's total width and height */
}

.container {
    background-color: #ffffff;
    padding: 30px;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    width: 100%;
    max-width: 500px; /* Max width for better readability on large screens */
}

h1 {
    text-align: center;
    color: #333;
    margin-bottom: 25px;
}

.task-input {
    display: flex;
    margin-bottom: 25px;
}

.task-input input[type="text"] {
    flex-grow: 1;
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 16px;
    margin-right: 10px;
}

.task-input button {
    padding: 12px 20px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 16px;
    transition: background-color 0.2s ease;
}

.task-input button:hover {
    background-color: #0056b3;
}

#taskList {
    list-style: none;
    padding: 0;
}

#taskList li {
    display: flex;
    align-items: center;
    background-color: #f9f9f9;
    border: 1px solid #eee;
    padding: 12px 15px;
    margin-bottom: 10px;
    border-radius: 4px;
    justify-content: space-between; /* Space out task text and buttons */
    word-break: break-word; /* Allow long words to break */
    white-space: normal; /* Allow text to wrap */
}

#taskList li span.task-text {
    flex-grow: 1;
    cursor: pointer; /* Indicate it's clickable to mark as complete */
    padding-right: 10px; /* Space between text and buttons */
}

#taskList li.completed span.task-text {
    text-decoration: line-through; /* Strikethrough for completed tasks */
    color: #888; /* Dim the color for completed tasks */
}

#taskList li .actions {
    display: flex;
    gap: 8px; /* Space between action buttons */
}

#taskList li .actions button {
    padding: 8px 12px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 14px;
    transition: background-color 0.2s ease;
}

#taskList li .actions button.delete-btn {
    background-color: #dc3545;
    color: white;
}

#taskList li .actions button.delete-btn:hover {
    background-color: #c82333;
}

/* Optional: Add a subtle animation for task addition/deletion */
#taskList li {
    opacity: 0;
    transform: translateY(10px);
    animation: fadeIn 0.3s forwards ease-out;
}

@keyframes fadeIn {
    to {
        opacity: 1;
        transform: translateY(0);
    }
}
document.addEventListener('DOMContentLoaded', () => {
    // 1. Hardcoded array of tasks
    let tasks = [
        { id: 1, title: 'Buy groceries', completed: false },
        { id: 2, title: 'Read a book', completed: true },
        { id: 3, title: 'Walk the dog', completed: false }
    ];

    const taskList = document.getElementById('taskList');
    const newTaskInput = document.getElementById('newTaskInput');
    const addTaskBtn = document.getElementById('addTaskBtn');

    // Function to render (display) all tasks
    function renderTasks() {
        taskList.innerHTML = ''; // Clear existing list items

        tasks.forEach(task => {
            const listItem = document.createElement('li');
            listItem.setAttribute('data-id', task.id); // Store ID for easy reference

            if (task.completed) {
                listItem.classList.add('completed');
            }

            const taskText = document.createElement('span');
            taskText.classList.add('task-text');
            taskText.textContent = task.title;
            taskText.addEventListener('click', () => toggleComplete(task.id)); // Mark as complete/incomplete

            const actionsDiv = document.createElement('div');
            actionsDiv.classList.add('actions');

            const deleteBtn = document.createElement('button');
            deleteBtn.classList.add('delete-btn');
            deleteBtn.textContent = 'Delete';
            deleteBtn.addEventListener('click', () => deleteTask(task.id));

            actionsDiv.appendChild(deleteBtn);
            listItem.appendChild(taskText);
            listItem.appendChild(actionsDiv);
            taskList.appendChild(listItem);
        });
    }

    // Function to add a new task
    function addTask() {
        const title = newTaskInput.value.trim(); // .trim() removes leading/trailing spaces
        if (title === '') {
            alert('Task title cannot be empty!');
            return;
        }

        const newId = tasks.length > 0 ? Math.max(...tasks.map(t => t.id)) + 1 : 1; // Generate unique ID
        const newTask = {
            id: newId,
            title: title,
            completed: false
        };

        tasks.push(newTask); // Add new task to our array
        newTaskInput.value = ''; // Clear input field
        renderTasks(); // Re-render the list to show the new task
    }

    // Function to toggle task completion status
    function toggleComplete(id) {
        tasks = tasks.map(task =>
            task.id === id ? { ...task, completed: !task.completed } : task
        );
        renderTasks(); // Re-render the list to update styling
    }

    // Function to delete a task
    function deleteTask(id) {
        tasks = tasks.filter(task => task.id !== id); // Filter out the task to be deleted
        renderTasks(); // Re-render the list
    }

    // Event Listeners
    addTaskBtn.addEventListener('click', addTask);
    // Allow adding tasks by pressing Enter key in the input field
    newTaskInput.addEventListener('keypress', (event) => {
        if (event.key === 'Enter') {
            addTask();
        }
    });

    // Initial render when the page loads
    renderTasks();
});

