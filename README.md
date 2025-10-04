Badges and live demo

Overview
Goal: Simple, fast and operator-controlled To-Do List app — add, execute, delete, save.

Approach: Minimal, clear UI; local storage; clean file structure; fast deployment via GitHub Pages.

Result: Live demo ready, documented debugging procedure, professional closure provided.

Features
➕ Add: Add a new task and add it to the list.

✅ Complete: Set the task to “completed” status (mark it with a line).

🗑️ Remove: Remove the task from the list.

💾 Save: Automatically save via browser localStorage.

🧱 Minimal UI: Clean, dark mode-compatible, responsive design.

🔐 No-backend: Full front-end, static hosting on GitHub Pages.

Technologies
Stack: HTML5, CSS3, Vanilla JavaScript

Hosting: GitHub Pages (main branch, root)

Versioning: Git + GitHub (manual, bug-free commits)

Architecture and file structure
Structural philosophy: Operator-first; code is clean, modules are minimal; paths are simplified.

Code
todo-app/
├── index.html
├── styles/
│ └── style.css
├── scripts/
│ └── script.js
└── indexdb/ # (for future local DB/backups, if used)
HTML: Semantic structure; proper meta and doctype in <head>.

CSS: External style.css; visual simplicity, flex-based layout.

JS: DOM events, localStorage CRUD, state re-rendering.

Run (local)
Clone the repo:

Code
git clone https://github.com/Jorabek-Hasanov/todo-app.git
cd todo-app
Open files:

Open index.html in a browser.

Dev server (optional):

Code
npx serve .
or VS Code Live Server extension.

Deploy — GitHub Pages
Settings → Pages:

Source: Deploy from a branch

Branch: main

Folder: /(root)

Live link:

https://Jorabek-Hasanov.github.io/todo-app/

Troubleshooting
Quirks Mode:

Reason: No <!DOCTYPE html>.

Solution: Add the following to the top of index.html:

Code
<!DOCTYPE html>
Commit:

Code
git add index.html
git commit -m "Fix: add HTML5 doctype"
git push
404 / Failed to load resource:

Reason: Invalid path or filename (styles/style.css, scripts/script.js).

Solution: In index.html:

Code
<link rel="stylesheet" href="styles/style.css" />
<script src="scripts/script.js"></script>
Check: DevTools → Network (should not contain red text).

Blank page (white background):

Reason: JS errors or empty <body>.

Solution: DevTools → Console (fix red text).

Hard refresh: Ctrl + Shift + R (Windows/Linux), Cmd + Shift + R (Mac).

CSS not working:

Cause: <style> in the wrong place (inside the element).

Solution: Move all style rules to styles/style.css; link them in <head>.

Roadmap
Filter: All / Active / Completed views.

Drag & drop: Sort tasks.

Clear completed: Clear completed with one click.

Accessibility: Keyboard control, ARIA attributes.

Unit tests (optional): Minimal DOM event tests.

Contributing
Branching: in the format feature/<name>.

Commit messages: feat: ..., fix: ..., docs: ....

PR: Short description, screenshot, “Before/After” comments.

License
MIT License: Free to use, modify, and redistribute the code.

Snippets for quick viewing
Minimal HTML skeleton:

html
<!DOCTYPE html>
<html lang="en">
<head> 
<meta charset="UTF-8" /> 
<meta name="viewport" content="width=device-width, initial-scale=1.0" /> 
<title>To-Do List</title> 
<link rel="stylesheet" href="styles/style.css" />
</head>
<body> 
<div class="container"> 
<header><h1>To-Do List</h1></header> 
<main> 
<form id="taskForm"> 
<input id="taskInput" type="text" placeholder="Enter a new task" required /> 
<button type="submit">Add Task</button> 
</form> 
<ul id="taskList"></ul> 
</main> 
</div> 
<script src="scripts/script.js"></script>
</body>
</html>
Minimal JS (localStorage with):

javascript
const form = document.getElementById('taskForm');
const input = document.getElementById('taskInput');
const list = document.getElementById('taskList');

const load = () => JSON.parse(localStorage.getItem('tasks') || '[]');
const save = (tasks) => localStorage.setItem('tasks', JSON.stringify(tasks));

let tasks = load();
const render = () => { 
list.innerHTML = ''; 
tasks.forEach((t, i) => { 
const li = document.createElement('li'); 
li.className = t.completed ? 'completed' : ''; 
li.textContent = t.text; 

const toggleBtn = document.createElement('button'); 
toggleBtn.textContent = t.completed ? 'Undo' : 'Done'; 
toggleBtn.onclick = () => { 
tasks[i].completed = !tasks[i].completed; 
save(tasks); render(); 
}; 

const delBtn = document.createElement('button'); 
delBtn.textContent = 'Delete'; 
delBtn.onclick = () => { 
tasks.splice(i, 1); 
save(tasks); render(); 
}; 

li.append(toggleBtn, delBtn); 
list.appendChild(li); 
});
};

form.addEventListener('submit', (e) => { 
e.preventDefault(); 
const text = input.value.trim(); 
if (!text) return; 
tasks.push({ text, completed: false }); 
input.value = ''; 
save(tasks); render();
});

render();
