# To-do-app
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="theme-color" content="#0f0f12">
<title>ZenFlow - Task Manager</title>
<style>
:root{--bg:#0f0f12;--surface:#1a1a1f;--surface-2:#23232a;--surface-3:#2d2d36;--text:#e8e8ec;--text-2:#a0a0b0;--text-3:#6b6b7b;--accent:#7c6bff;--accent-2:#5b4fd6;--success:#4ade80;--warning:#fbbf24;--danger:#f87171;--info:#60a5fa;--radius:12px;--radius-sm:8px;--shadow:0 4px 24px rgba(0,0,0,0.4);--transition:0.2s cubic-bezier(0.4,0,0.2,1)}
.light{--bg:#f5f5f7;--surface:#fff;--surface-2:#f0f0f5;--surface-3:#e5e5eb;--text:#1a1a2e;--text-2:#555570;--text-3:#8888a0}
*{margin:0;padding:0;box-sizing:border-box;font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,sans-serif}
body{background:var(--bg);color:var(--text);height:100vh;overflow:hidden}
.app{display:flex;height:100vh}
.sidebar{width:260px;background:var(--surface);border-right:1px solid var(--surface-3);display:flex;flex-direction:column;flex-shrink:0;overflow-y:auto}
.sidebar.collapsed{width:60px}
.sidebar.collapsed .nav-text,.sidebar.collapsed .section-title{display:none}
.main{flex:1;display:flex;flex-direction:column;overflow:hidden}
.header{height:56px;background:var(--surface);border-bottom:1px solid var(--surface-3);display:flex;align-items:center;justify-content:space-between;padding:0 20px;flex-shrink:0}
.content{flex:1;overflow-y:auto;padding:20px}
.logo{padding:16px 20px;font-size:18px;font-weight:700;color:var(--accent);display:flex;align-items:center;gap:10px;cursor:pointer}
.logo-icon{width:32px;height:32px;background:linear-gradient(135deg,var(--accent),var(--accent-2));border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:16px}
.nav-section{padding:8px 12px}
.section-title{font-size:10px;text-transform:uppercase;letter-spacing:1px;color:var(--text-3);margin-bottom:4px;padding:0 8px}
.nav-item{display:flex;align-items:center;gap:12px;padding:8px 12px;border-radius:var(--radius-sm);cursor:pointer;color:var(--text-2);font-size:14px;transition:all var(--transition);position:relative}
.nav-item:hover{background:var(--surface-2);color:var(--text)}
.nav-item.active{background:var(--surface-2);color:var(--accent)}
.nav-item .badge{position:absolute;right:8px;background:var(--accent);color:white;font-size:10px;padding:1px 6px;border-radius:10px}
.nav-icon{width:20px;text-align:center}
.header-left{display:flex;align-items:center;gap:16px}
.view-tabs{display:flex;gap:4px;background:var(--surface-2);padding:4px;border-radius:var(--radius-sm)}
.view-tab{padding:6px 14px;border-radius:6px;font-size:13px;cursor:pointer;color:var(--text-2);transition:all var(--transition)}
.view-tab.active{background:var(--surface);color:var(--text);box-shadow:0 1px 3px rgba(0,0,0,0.2)}
.header-right{display:flex;align-items:center;gap:12px}
.search-box{position:relative}
.search-box input{background:var(--surface-2);border:1px solid var(--surface-3);border-radius:var(--radius-sm);padding:8px 12px 8px 32px;color:var(--text);font-size:13px;width:200px;outline:none}
.btn-icon{width:36px;height:36px;border-radius:var(--radius-sm);background:var(--surface-2);border:none;color:var(--text-2);cursor:pointer;display:flex;align-items:center;justify-content:center;font-size:16px}
.btn-icon:hover{background:var(--surface-3);color:var(--text)}
.btn-primary{background:var(--accent);color:white;border:none;padding:8px 16px;border-radius:var(--radius-sm);font-size:13px;font-weight:600;cursor:pointer}
.task-input-area{background:var(--surface);border:1px solid var(--surface-3);border-radius:var(--radius);padding:16px;margin-bottom:20px}
.task-input{width:100%;background:transparent;border:none;color:var(--text);font-size:15px;outline:none;resize:none;min-height:24px}
.task-input::placeholder{color:var(--text-3)}
.input-actions{display:flex;justify-content:space-between;align-items:center;margin-top:12px;padding-top:12px;border-top:1px solid var(--surface-3)}
.input-left{display:flex;gap:8px}
.input-btn{background:var(--surface-2);border:none;color:var(--text-2);padding:6px 10px;border-radius:6px;font-size:12px;cursor:pointer}
.task-list{display:flex;flex-direction:column;gap:2px}
.task-item{background:var(--surface);border:1px solid var(--surface-3);border-radius:var(--radius-sm);padding:12px 16px;display:flex;align-items:flex-start;gap:12px;cursor:pointer;transition:all var(--transition);position:relative}
.task-item:hover{border-color:var(--surface-2);background:var(--surface-2)}
.task-item.completed .task-title{text-decoration:line-through;color:var(--text-3)}
.task-item.p1{border-left:3px solid var(--danger)}
.task-item.p2{border-left:3px solid var(--warning)}
.task-item.p3{border-left:3px solid var(--info)}
.task-item.p4{border-left:3px solid var(--success)}
.task-item.starred::after{content:'★';position:absolute;right:12px;top:12px;color:var(--warning);font-size:14px}
.checkbox{width:18px;height:18px;border:2px solid var(--surface-3);border-radius:4px;flex-shrink:0;margin-top:2px;cursor:pointer;display:flex;align-items:center;justify-content:center}
.checkbox.checked{background:var(--accent);border-color:var(--accent)}
.checkbox.checked::after{content:'✓';color:white;font-size:11px;font-weight:bold}
.task-content{flex:1;min-width:0}
.task-title{font-size:14px;color:var(--text);line-height:1.4}
.task-meta{display:flex;gap:8px;margin-top:6px;flex-wrap:wrap}
.task-tag{font-size:11px;padding:2px 8px;border-radius:4px;background:var(--surface-2);color:var(--text-2)}
.task-due{font-size:11px;color:var(--danger)}
.task-project{font-size:11px;color:var(--accent)}
.task-priority{font-size:10px;padding:1px 6px;border-radius:4px;font-weight:600}
.task-priority.p1{background:rgba(248,113,113,0.15);color:var(--danger)}
.task-priority.p2{background:rgba(251,191,36,0.15);color:var(--warning)}
.task-priority.p3{background:rgba(96,165,250,0.15);color:var(--info)}
.task-priority.p4{background:rgba(74,222,128,0.15);color:var(--success)}
.subtask-count{font-size:11px;color:var(--text-3);margin-left:auto}
.expand-btn{width:20px;height:20px;display:flex;align-items:center;justify-content:center;cursor:pointer;color:var(--text-3);font-size:10px;transition:transform var(--transition);flex-shrink:0;margin-top:2px}
.expand-btn.expanded{transform:rotate(90deg)}
.subtasks{margin-left:30px;padding-left:16px;border-left:2px solid var(--surface-3);display:none}
.subtasks.visible{display:flex;flex-direction:column;gap:2px}
.subtasks .task-item{background:var(--surface-2);border-color:var(--surface-3)}
.kanban-board{display:flex;gap:16px;overflow-x:auto;padding-bottom:8px}
.kanban-column{min-width:280px;background:var(--surface-2);border-radius:var(--radius);padding:12px}
.kanban-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px}
.kanban-title{font-size:14px;font-weight:600}
.kanban-count{font-size:12px;color:var(--text-3);background:var(--surface-3);padding:2px 8px;border-radius:10px}
.kanban-tasks{display:flex;flex-direction:column;gap:8px}
.kanban-task{background:var(--surface);border-radius:var(--radius-sm);padding:12px;cursor:pointer;border:1px solid transparent;transition:all var(--transition)}
.kanban-task:hover{border-color:var(--accent);transform:translateY(-2px);box-shadow:var(--shadow)}
.matrix-grid{display:grid;grid-template-columns:1fr 1fr;grid-template-rows:1fr 1fr;gap:12px;height:calc(100vh - 180px)}
.matrix-quadrant{background:var(--surface);border-radius:var(--radius);padding:16px;border:2px solid var(--surface-3);overflow-y:auto}
.matrix-quadrant.q1{border-color:var(--danger)}
.matrix-quadrant.q2{border-color:var(--warning)}
.matrix-quadrant.q3{border-color:var(--info)}
.matrix-quadrant.q4{border-color:var(--success)}
.matrix-label{font-size:13px;font-weight:600;margin-bottom:12px;display:flex;align-items:center;gap:8px}
.matrix-label .dot{width:8px;height:8px;border-radius:50%}
.matrix-tasks{display:flex;flex-direction:column;gap:6px}
.matrix-task{background:var(--surface-2);padding:10px 12px;border-radius:var(--radius-sm);font-size:13px;cursor:pointer}
.matrix-task:hover{background:var(--surface-3)}
.calendar-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:1px;background:var(--surface-3);border-radius:var(--radius);overflow:hidden}
.cal-header{background:var(--surface);padding:12px;text-align:center;font-size:12px;font-weight:600;color:var(--text-2);text-transform:uppercase}
.cal-day{background:var(--surface);min-height:100px;padding:8px;cursor:pointer}
.cal-day:hover{background:var(--surface-2)}
.cal-day.today{background:var(--surface-2)}
.cal-day.today .day-num{color:var(--accent);font-weight:700}
.cal-day.other-month{opacity:0.4}
.day-num{font-size:13px;margin-bottom:4px}
.cal-task{font-size:10px;padding:2px 6px;border-radius:3px;background:var(--accent);color:white;margin-bottom:2px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;cursor:pointer}
.cal-task.completed{opacity:0.5;text-decoration:line-through}
.focus-overlay{position:fixed;inset:0;background:var(--bg);z-index:100;display:none;flex-direction:column;align-items:center;justify-content:center;padding:40px}
.focus-overlay.active{display:flex}
.focus-task{font-size:28px;font-weight:300;text-align:center;max-width:600px;line-height:1.4;margin-bottom:40px}
.pomodoro-timer{font-size:72px;font-weight:200;font-variant-numeric:tabular-nums;margin-bottom:30px;color:var(--accent)}
.pomodoro-controls{display:flex;gap:16px}
.pomo-btn{width:60px;height:60px;border-radius:50%;border:2px solid var(--surface-3);background:transparent;color:var(--text);font-size:20px;cursor:pointer;display:flex;align-items:center;justify-content:center}
.pomo-btn.primary{background:var(--accent);border-color:var(--accent);color:white}
.focus-close{position:absolute;top:20px;right:20px;background:none;border:none;color:var(--text-3);font-size:24px;cursor:pointer}
.dashboard-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(280px,1fr));gap:16px}
.dash-card{background:var(--surface);border-radius:var(--radius);padding:20px;border:1px solid var(--surface-3)}
.dash-card h3{font-size:14px;color:var(--text-2);margin-bottom:16px;font-weight:500}
.stat-row{display:flex;justify-content:space-between;align-items:center;padding:8px 0;border-bottom:1px solid var(--surface-2);font-size:14px}
.stat-row:last-child{border-bottom:none}
.stat-num{font-size:24px;font-weight:700;color:var(--accent)}
.progress-bar{height:6px;background:var(--surface-2);border-radius:3px;overflow:hidden;margin-top:8px}
.progress-fill{height:100%;background:linear-gradient(90deg,var(--accent),var(--accent-2));border-radius:3px;transition:width 0.5s ease}
.modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,0.6);z-index:50;display:none;align-items:center;justify-content:center;backdrop-filter:blur(4px)}
.modal-overlay.active{display:flex}
.modal{background:var(--surface);border-radius:var(--radius);width:90%;max-width:560px;max-height:85vh;overflow-y:auto;box-shadow:var(--shadow)}
.modal-header{padding:20px;border-bottom:1px solid var(--surface-3);display:flex;justify-content:space-between;align-items:center}
.modal-header h2{font-size:18px;font-weight:600}
.modal-close{background:none;border:none;color:var(--text-3);font-size:20px;cursor:pointer}
.modal-body{padding:20px}
.form-group{margin-bottom:16px}
.form-group label{display:block;font-size:12px;color:var(--text-2);margin-bottom:6px;text-transform:uppercase;letter-spacing:0.5px}
.form-group input,.form-group textarea,.form-group select{width:100%;background:var(--surface-2);border:1px solid var(--surface-3);border-radius:var(--radius-sm);padding:10px 12px;color:var(--text);font-size:14px;outline:none}
.form-group input:focus,.form-group textarea:focus,.form-group select:focus{border-color:var(--accent)}
.form-group textarea{resize:vertical;min-height:80px}
.form-row{display:grid;grid-template-columns:1fr 1fr;gap:12px}
.tag-picker{display:flex;flex-wrap:wrap;gap:6px}
.tag-option{padding:4px 10px;border-radius:12px;font-size:12px;cursor:pointer;border:1px solid var(--surface-3)}
.tag-option.selected{background:var(--accent);color:white;border-color:var(--accent)}
.priority-picker{display:flex;gap:8px}
.priority-option{padding:6px 14px;border-radius:var(--radius-sm);font-size:12px;font-weight:600;cursor:pointer;border:2px solid transparent}
.priority-option.selected{border-color:var(--text)}
.modal-footer{padding:16px 20px;border-top:1px solid var(--surface-3);display:flex;justify-content:flex-end;gap:8px}
.btn-secondary{background:var(--surface-2);border:1px solid var(--surface-3);color:var(--text);padding:8px 16px;border-radius:var(--radius-sm);font-size:13px;cursor:pointer}
.btn-danger{background:rgba(248,113,113,0.1);border:1px solid var(--danger);color:var(--danger);padding:8px 16px;border-radius:var(--radius-sm);font-size:13px;cursor:pointer}
.toast-container{position:fixed;bottom:20px;right:20px;z-index:200;display:flex;flex-direction:column;gap:8px}
.toast{background:var(--surface);border:1px solid var(--surface-3);border-radius:var(--radius-sm);padding:12px 16px;font-size:13px;box-shadow:var(--shadow);display:flex;align-items:center;gap:8px;animation:slideIn 0.3s ease}
.toast.success{border-left:3px solid var(--success)}
.toast.error{border-left:3px solid var(--danger)}
@keyframes slideIn{from{transform:translateX(100%);opacity:0}to{transform:translateX(0);opacity:1}}
.context-menu{position:fixed;background:var(--surface);border:1px solid var(--surface-3);border-radius:var(--radius-sm);padding:6px 0;min-width:180px;box-shadow:var(--shadow);z-index:150;display:none}
.context-menu.active{display:block}
.context-item{padding:8px 16px;font-size:13px;cursor:pointer;display:flex;align-items:center;gap:10px;color:var(--text-2)}
.context-item:hover{background:var(--surface-2);color:var(--text)}
.context-item.danger{color:var(--danger)}
.context-divider{height:1px;background:var(--surface-3);margin:4px 0}
.bulk-bar{position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:var(--surface);border:1px solid var(--surface-3);border-radius:var(--radius);padding:10px 20px;display:none;align-items:center;gap:16px;box-shadow:var(--shadow);z-index:40}
.bulk-bar.active{display:flex}
.bulk-count{font-size:13px;color:var(--text-2)}
.empty-state{text-align:center;padding:60px 20px;color:var(--text-3)}
.empty-state-icon{font-size:48px;margin-bottom:16px;opacity:0.5}
.empty-state h3{font-size:18px;color:var(--text-2);margin-bottom:8px;font-weight:500}
.timeblock-grid{display:grid;grid-template-columns:60px 1fr;gap:1px;background:var(--surface-3);border-radius:var(--radius);overflow:hidden}
.time-label{background:var(--surface);padding:8px 12px;font-size:12px;color:var(--text-3);text-align:right;border-right:1px solid var(--surface-3)}
.time-slot{background:var(--surface);min-height:50px;padding:4px 8px;cursor:pointer}
.time-slot:hover{background:var(--surface-2)}
.time-slot .tb-task{font-size:11px;padding:2px 6px;border-radius:3px;background:var(--accent);color:white;margin-bottom:2px}
.trash-item{opacity:0.6}
.trash-item .task-title{text-decoration:line-through}
.restore-btn{font-size:11px;color:var(--accent);cursor:pointer;margin-left:8px}
.hidden{display:none!important}
@media(max-width:768px){
.sidebar{position:fixed;z-index:30;height:100vh;transform:translateX(-100%)}
.sidebar.open{transform:translateX(0)}
.search-box input{width:140px}
.matrix-grid{grid-template-columns:1fr;grid-template-rows:auto;height:auto}
.kanban-board{flex-direction:column}
.kanban-column{min-width:auto}
}
</style>
</head>
<body>
<div class="app" id="app">
<aside class="sidebar" id="sidebar">
<div class="logo" onclick="toggleSidebar()"><div class="logo-icon">⚡</div><span class="nav-text">ZenFlow</span></div>
<div class="nav-section">
<div class="section-title nav-text">Views</div>
<div class="nav-item active" data-view="inbox" onclick="switchView('inbox')"><span class="nav-icon">📥</span><span class="nav-text">Inbox</span><span class="badge" id="inbox-badge">0</span></div>
<div class="nav-item" data-view="today" onclick="switchView('today')"><span class="nav-icon">📅</span><span class="nav-text">Today</span><span class="badge" id="today-badge">0</span></div>
<div class="nav-item" data-view="upcoming" onclick="switchView('upcoming')"><span class="nav-icon">📆</span><span class="nav-text">Upcoming</span></div>
<div class="nav-item" data-view="someday" onclick="switchView('someday')"><span class="nav-icon">🌙</span><span class="nav-text">Someday</span></div>
<div class="nav-item" data-view="starred" onclick="switchView('starred')"><span class="nav-icon">⭐</span><span class="nav-text">Starred</span></div>
<div class="nav-item" data-view="completed" onclick="switchView('completed')"><span class="nav-icon">✅</span><span class="nav-text">Completed</span></div>
<div class="nav-item" data-view="trash" onclick="switchView('trash')"><span class="nav-icon">🗑️</span><span class="nav-text">Trash</span></div>
</div>
<div class="nav-section"><div class="section-title nav-text">Areas</div><div id="areas-list"></div></div>
<div class="nav-section"><div class="section-title nav-text">Projects</div><div id="projects-list"></div><div class="nav-item" onclick="showNewProjectModal()"><span class="nav-icon">+</span><span class="nav-text">New Project</span></div></div>
<div class="nav-section"><div class="section-title nav-text">Tags</div><div id="tags-list"></div></div>
</aside>
<main class="main">
<header class="header">
<div class="header-left">
<button class="btn-icon" onclick="toggleSidebar()" id="menu-btn">☰</button>
<div class="view-tabs" id="view-tabs">
<div class="view-tab active" data-tab="list" onclick="switchTab('list')">List</div>
<div class="view-tab" data-tab="kanban" onclick="switchTab('kanban')">Board</div>
<div class="view-tab" data-tab="matrix" onclick="switchTab('matrix')">Matrix</div>
<div class="view-tab" data-tab="calendar" onclick="switchTab('calendar')">Calendar</div>
<div class="view-tab" data-tab="timeblock" onclick="switchTab('timeblock')">Schedule</div>
<div class="view-tab" data-tab="dashboard" onclick="switchTab('dashboard')">Dashboard</div>
</div>
</div>
<div class="header-right">
<div class="search-box"><input type="text" placeholder="Search tasks..." id="search-input" oninput="handleSearch(this.value)"></div>
<button class="btn-icon" onclick="toggleTheme()">🌓</button>
<button class="btn-icon" onclick="enterFocusMode()">🎯</button>
<button class="btn-icon" onclick="showExportModal()">💾</button>
<button class="btn-primary" onclick="showNewTaskModal()">+ New Task</button>
</div>
</header>
<div class="content" id="content"></div>
</main>
</div>

<div class="focus-overlay" id="focus-overlay">
<button class="focus-close" onclick="exitFocusMode()">✕</button>
<div class="focus-task" id="focus-task-title">Select a task to focus on</div>
<div class="pomodoro-timer" id="pomodoro-timer">25:00</div>
<div class="pomodoro-controls">
<button class="pomo-btn" onclick="resetPomodoro()">↺</button>
<button class="pomo-btn primary" id="pomo-play" onclick="togglePomodoro()">▶</button>
<button class="pomo-btn" onclick="skipPomodoro()">⏭</button>
</div>
<div style="margin-top:30px;color:var(--text-3);font-size:14px"><span id="pomo-session">Work session 1/4</span> · <span id="pomo-mode">Focus</span></div>
</div>

<div class="modal-overlay" id="task-modal">
<div class="modal">
<div class="modal-header"><h2 id="modal-title">New Task</h2><button class="modal-close" onclick="closeModal()">✕</button></div>
<div class="modal-body">
<div class="form-group"><label>Task Title</label><input type="text" id="task-title-input" placeholder="What needs to be done?"></div>
<div class="form-group"><label>Description</label><textarea id="task-desc-input" placeholder="Add details..."></textarea></div>
<div class="form-row">
<div class="form-group"><label>Project</label><select id="task-project-input"></select></div>
<div class="form-group"><label>Area</label><select id="task-area-input"></select></div>
</div>
<div class="form-row">
<div class="form-group"><label>Due Date</label><input type="date" id="task-due-input"></div>
<div class="form-group"><label>Start Date</label><input type="date" id="task-start-input"></div>
</div>
<div class="form-row">
<div class="form-group">
<label>Priority</label>
<div class="priority-picker" id="priority-picker">
<div class="priority-option p1" data-p="1" onclick="selectPriority(1)" style="background:rgba(248,113,113,0.15);color:var(--danger)">P1</div>
<div class="priority-option p2" data-p="2" onclick="selectPriority(2)" style="background:rgba(251,191,36,0.15);color:var(--warning)">P2</div>
<div class="priority-option p3" data-p="3" onclick="selectPriority(3)" style="background:rgba(96,165,250,0.15);color:var(--info)">P3</div>
<div class="priority-option p4" data-p="4" onclick="selectPriority(4)" style="background:rgba(74,222,128,0.15);color:var(--success)">P4</div>
</div>
</div>
<div class="form-group"><label>Time Estimate (min)</label><input type="number" id="task-estimate-input" placeholder="30" min="0"></div>
</div>
<div class="form-group"><label>Tags</label><div class="tag-picker" id="tag-picker"></div></div>
<div class="form-group"><label>Recurrence</label><select id="task-recur-input"><option value="">None</option><option value="daily">Daily</option><option value="weekly">Weekly</option><option value="monthly">Monthly</option><option value="weekdays">Weekdays</option></select></div>
<div class="form-group"><label><input type="checkbox" id="task-star-input"> Star this task</label></div>
</div>
<div class="modal-footer">
<button class="btn-danger" id="modal-delete-btn" onclick="deleteTaskFromModal()" style="margin-right:auto;display:none">Delete</button>
<button class="btn-secondary" onclick="closeModal()">Cancel</button>
<button class="btn-primary" onclick="saveTask()">Save Task</button>
</div>
</div>
</div>

<div class="modal-overlay" id="project-modal">
<div class="modal" style="max-width:400px">
<div class="modal-header"><h2>New Project</h2><button class="modal-close" onclick="closeProjectModal()">✕</button></div>
<div class="modal-body">
<div class="form-group"><label>Project Name</label><input type="text" id="project-name-input" placeholder="My Project"></div>
<div class="form-group"><label>Area</label><select id="project-area-input"></select></div>
<div class="form-group"><label>Color</label><input type="color" id="project-color-input" value="#7c6bff"></div>
</div>
<div class="modal-footer">
<button class="btn-secondary" onclick="closeProjectModal()">Cancel</button>
<button class="btn-primary" onclick="saveProject()">Create Project</button>
</div>
</div>
</div>

<div class="modal-overlay" id="export-modal">
<div class="modal" style="max-width:450px">
<div class="modal-header"><h2>Data Management</h2><button class="modal-close" onclick="closeExportModal()">✕</button></div>
<div class="modal-body">
<div class="form-group"><label>Export Data</label><div style="display:flex;gap:8px;flex-wrap:wrap"><button class="btn-secondary" onclick="exportData('json')">Export JSON</button><button class="btn-secondary" onclick="exportData('csv')">Export CSV</button><button class="btn-secondary" onclick="exportData('markdown')">Export Markdown</button></div></div>
<div class="form-group"><label>Import Data (JSON)</label><input type="file" id="import-file" accept=".json" onchange="importData(this)"></div>
<div class="form-group"><label>Danger Zone</label><button class="btn-danger" onclick="clearAllData()">Clear All Data</button></div>
</div>
<div class="modal-footer"><button class="btn-secondary" onclick="closeExportModal()">Close</button></div>
</div>
</div>

<div class="context-menu" id="context-menu">
<div class="context-item" onclick="ctxEdit()">✏️ Edit</div>
<div class="context-item" onclick="ctxDuplicate()">📋 Duplicate</div>
<div class="context-item" onclick="ctxStar()">⭐ Star/Unstar</div>
<div class="context-item" onclick="ctxFocus()">🎯 Focus Mode</div>
<div class="context-divider"></div>
<div class="context-item" onclick="ctxAddSubtask()">➕ Add Subtask</div>
<div class="context-item" onclick="ctxMove()">📁 Move to Project</div>
<div class="context-divider"></div>
<div class="context-item danger" onclick="ctxDelete()">🗑️ Move to Trash</div>
</div>

<div class="bulk-bar" id="bulk-bar">
<span class="bulk-count" id="bulk-count">0 selected</span>
<div class="bulk-actions">
<button class="btn-secondary" onclick="bulkComplete()">✓ Complete</button>
<button class="btn-secondary" onclick="bulkStar()">⭐ Star</button>
<button class="btn-secondary" onclick="bulkMove()">📁 Move</button>
<button class="btn-secondary" onclick="bulkTag()">🏷️ Tag</button>
<button class="btn-danger" onclick="bulkDelete()">🗑️ Delete</button>
<button class="btn-icon" onclick="clearSelection()" style="margin-left:8px">✕</button>
</div>
</div>

<div class="toast-container" id="toast-container"></div>

<script>
var state={tasks:[],projects:[],areas:[],tags:[],trash:[],currentView:'inbox',currentTab:'list',selectedTasks:new Set,editingTaskId:null,selectedPriority:4,selectedTags:new Set,pomodoro:{running:false,time:1500,mode:'work',session:1,interval:null},contextTaskId:null,theme:'dark'};
function gid(){return'id_'+Date.now()+'_'+Math.random().toString(36).substr(2,9)}
function gt(id){return state.tasks.find(function(t){return t.id===id})}
function eh(s){var d=document.createElement('div');d.textContent=s;return d.innerHTML}
function fd(d){if(!d)return'';var p=d.split('-');return p[1]+'/'+p[2]}
function toast(msg,type){type=type||'success';var c=document.getElementById('toast-container');var t=document.createElement('div');t.className='toast '+type;t.innerHTML=(type==='success'?'&#10003;':'&#10005;')+' '+msg;c.appendChild(t);setTimeout(function(){t.remove()},3000)}

function initDemo(){
var now=new Date(),tomo=new Date(now);tomo.setDate(tomo.getDate()+1);
var week=new Date(now);week.setDate(week.getDate()+7);
var ts=now.toISOString().split('T')[0],tms=tomo.toISOString().split('T')[0],ws=week.toISOString().split('T')[0];
state.areas=[{id:'a1',name:'Personal',color:'#7c6bff'},{id:'a2',name:'Work',color:'#60a5fa'},{id:'a3',name:'Health',color:'#4ade80'},{id:'a4',name:'Learning',color:'#fbbf24'}];
state.projects=[{id:'p1',name:'Personal',areaId:'a1',color:'#7c6bff',created:Date.now()},{id:'p2',name:'Work',areaId:'a2',color:'#60a5fa',created:Date.now()},{id:'p3',name:'Health',areaId:'a3',color:'#4ade80',created:Date.now()}];
state.tags=[{id:'t1',name:'urgent',color:'#f87171'},{id:'t2',name:'quick',color:'#4ade80'},{id:'t3',name:'deep-work',color:'#7c6bff'},{id:'t4',name:'meeting',color:'#60a5fa'}];
state.tasks=[
{id:'task1',title:'Review quarterly goals',desc:'Go through Q3 objectives',projectId:'p2',areaId:'a2',priority:1,dueDate:tms,tags:['t1','t3'],starred:true,completed:false,subtasks:[],created:Date.now(),timeEstimate:60,timeLogged:0,parentId:null,deleted:false,recurrence:'',startDate:''},
{id:'task2',title:'Morning workout routine',desc:'30 min cardio + strength',projectId:'p3',areaId:'a3',priority:2,dueDate:ts,tags:['t2'],starred:false,completed:false,subtasks:[{id:'sub1',title:'Warm up 5 min',completed:false},{id:'sub2',title:'Cardio 20 min',completed:false},{id:'sub3',title:'Cool down 5 min',completed:false}],created:Date.now()-864e5,timeEstimate:30,timeLogged:0,parentId:null,deleted:false,recurrence:'daily',startDate:''},
{id:'task3',title:'Read Atomic Habits ch5',desc:'',projectId:'p1',areaId:'a1',priority:3,dueDate:ws,tags:[],starred:false,completed:false,subtasks:[],created:Date.now()-1728e5,timeEstimate:45,timeLogged:0,parentId:null,deleted:false,recurrence:'',startDate:''},
{id:'task4',title:'Team standup meeting',desc:'Prepare updates',projectId:'p2',areaId:'a2',priority:2,dueDate:ts,tags:['t4'],starred:true,completed:false,subtasks:[],created:Date.now()-36e5,timeEstimate:15,timeLogged:0,parentId:null,deleted:false,recurrence:'daily',startDate:''},
{id:'task5',title:'Grocery shopping',desc:'Buy veggies and protein',projectId:'p1',areaId:'a1',priority:4,dueDate:tms,tags:['t2'],starred:false,completed:true,subtasks:[],created:Date.now()-2e5,timeEstimate:40,timeLogged:35,parentId:null,deleted:false,recurrence:'weekly',startDate:''},
{id:'task6',title:'Plan weekend trip',desc:'Research destinations',projectId:'p1',areaId:'a1',priority:3,dueDate:'',tags:[],starred:false,completed:false,subtasks:[{id:'sub4',title:'Research 3 destinations',completed:true},{id:'sub5',title:'Compare prices',completed:false},{id:'sub6',title:'Book hotel',completed:false}],created:Date.now()-5e5,timeEstimate:90,timeLogged:20,parentId:null,deleted:false,recurrence:'',startDate:''},
{id:'task7',title:'Learn TypeScript patterns',desc:'Generics and mapped types',projectId:null,areaId:'a4',priority:3,dueDate:'',tags:['t3'],starred:false,completed:false,subtasks:[],created:Date.now()-1e6,timeEstimate:120,timeLogged:0,parentId:null,deleted:false,recurrence:'',startDate:''},
{id:'task8',title:'Call dentist',desc:'',projectId:null,areaId:'a3',priority:2,dueDate:ts,tags:['t1'],starred:false,completed:false,subtasks:[],created:Date.now()-2e6,timeEstimate:10,timeLogged:0,parentId:null,deleted:false,recurrence:'',startDate:''}
];
}

function save(){try{var d={tasks:state.tasks,projects:state.projects,areas:state.areas,tags:state.tags,trash:state.trash,theme:state.theme};localStorage.setItem('zenflow_data',JSON.stringify(d))}catch(e){}}
function load(){try{var r=localStorage.getItem('zenflow_data');if(r){var d=JSON.parse(r);if(d.tasks)state.tasks=d.tasks;if(d.projects)state.projects=d.projects;if(d.areas)state.areas=d.areas;if(d.tags)state.tags=d.tags;if(d.trash)state.trash=d.trash;if(d.theme){state.theme=d.theme;document.body.classList.toggle('light',state.theme==='light')}}}catch(e){}}

function switchView(view){state.currentView=view;state.selectedTasks.clear();updateBulk();render();if(window.innerWidth<768)document.getElementById('sidebar').classList.remove('open')}
function switchTab(tab){state.currentTab=tab;document.querySelectorAll('.view-tab').forEach(function(el){el.classList.toggle('active',el.dataset.tab===tab)});renderContent()}
function toggleSidebar(){var sb=document.getElementById('sidebar');if(window.innerWidth<768)sb.classList.toggle('open');else sb.classList.toggle('collapsed')}
function toggleTheme(){state.theme=state.theme==='dark'?'light':'dark';document.body.classList.toggle('light',state.theme==='light');save()}

function gvt(){var tasks=state.tasks.filter(function(t){return!t.deleted});var view=state.currentView;var today=new Date().toISOString().split('T')[0];switch(view){case'inbox':tasks=tasks.filter(function(t){return!t.projectId&&!t.parentId});break;case'today':tasks=tasks.filter(function(t){return t.dueDate===today&&!t.completed});break;case'upcoming':tasks=tasks.filter(function(t){return t.dueDate>today&&!t.completed});break;case'someday':tasks=tasks.filter(function(t){return!t.dueDate&&!t.completed});break;case'starred':tasks=tasks.filter(function(t){return t.starred&&!t.completed});break;case'completed':tasks=state.tasks.filter(function(t){return t.completed});break;case'trash':return state.trash;default:if(view.indexOf('project_')===0)tasks=tasks.filter(function(t){return t.projectId===view.replace('project_','')&&!t.parentId});else if(view.indexOf('area_')===0)tasks=tasks.filter(function(t){return t.areaId===view.replace('area_','')&&!t.parentId});else if(view.indexOf('tag_')===0)tasks=tasks.filter(function(t){return t.tags.indexOf(view.replace('tag_',''))!==-1&&!t.parentId})}}var search=(document.getElementById('search-input')&&document.getElementById('search-input').value||'').toLowerCase();if(search)tasks=tasks.filter(function(t){return t.title.toLowerCase().indexOf(search)!==-1||(t.desc&&t.desc.toLowerCase().indexOf(search)!==-1)});return tasks}

function render(){renderSidebar();renderContent();updateBadges()}
function renderSidebar(){document.getElementById('areas-list').innerHTML=state.areas.map(function(a){var c=state.tasks.filter(function(t){return t.areaId===a.id&&!t.deleted&&!t.completed}).length;return'<div class="nav-item" data-view="area_'+a.id+'" onclick="switchView(&apos;area_'+a.id+'&apos;)"><span class="nav-icon" style="color:'+a.color+'">&#9679;</span><span class="nav-text">'+a.name+'</span>'+(c>0?'<span class="badge">'+c+'</span>':'')+'</div>'}).join('');document.getElementById('projects-list').innerHTML=state.projects.map(function(p){var c=state.tasks.filter(function(t){return t.projectId===p.id&&!t.deleted&&!t.completed}).length;return'<div class="nav-item" data-view="project_'+p.id+'" onclick="switchView(&apos;project_'+p.id+'&apos;)"><span class="nav-icon" style="color:'+p.color+'">&#9632;</span><span class="nav-text">'+p.name+'</span>'+(c>0?'<span class="badge">'+c+'</span>':'')+'</div>'}).join('');document.getElementById('tags-list').innerHTML=state.tags.map(function(t){var c=state.tasks.filter(function(x){return x.tags.indexOf(t.id)!==-1&&!x.deleted&&!x.completed}).length;return'<div class="nav-item" data-view="tag_'+t.id+'" onclick="switchView(&apos;tag_'+t.id+'&apos;)"><span class="nav-icon" style="color:'+t.color+'">#</span><span class="nav-text">'+t.name+'</span>'+(c>0?'<span class="badge">'+c+'</span>':'')+'</div>'}).join('');document.querySelectorAll('.nav-item').forEach(function(el){el.classList.toggle('active',el.dataset.view===state.currentView)})}
function renderContent(){var c=document.getElementById('content');var tab=state.currentTab;if(tab==='list')renderList(c);else if(tab==='kanban')renderKanban(c);else if(tab==='matrix')renderMatrix(c);else if(tab==='calendar')renderCalendar(c);else if(tab==='timeblock')renderTimeblock(c);else if(tab==='dashboard')renderDashboard(c)}

function renderList(container){var tasks=gvt();var isTrash=state.currentView==='trash';var html='';if(!isTrash){html+='<div class="task-input-area"><textarea class="task-input" id="quick-input" placeholder="Add a task... (try: &quot;Meeting tomorrow 3pm #work p1&quot;)" rows="1" onkeydown="hqk(event)" oninput="ar(this)"></textarea><div class="input-actions"><div class="input-left"><button class="input-btn" onclick="showNewTaskModal()">&#128197; Due</button><button class="input-btn" onclick="showNewTaskModal()">&#127991; Tag</button><button class="input-btn" onclick="showNewTaskModal()">&#128193; Project</button><button class="input-btn" onclick="showNewTaskModal()">&#9889; Priority</button></div><button class="btn-primary" onclick="pqa()">Add Task</button></div></div>'}if(tasks.length===0){html+='<div class="empty-state"><div class="empty-state-icon">'+(isTrash?'&#128465;':'&#128203;')+'</div><h3>'+(isTrash?'Trash is empty':'No tasks here')+'</h3><p>'+(isTrash?'Deleted tasks appear here for 30 days':'Add a task to get started')+'</p></div>'}else{html+='<div class="task-list">';tasks.forEach(function(t){html+=rti(t)});html+='</div>'}container.innerHTML=html}

function rti(task){var hasSub=task.subtasks&&task.subtasks.length>0;var compSub=hasSub?task.subtasks.filter(function(s){return s.completed}).length:0;var project=state.projects.find(function(p){return p.id===task.projectId});var isTrash=state.currentView==='trash';var html='<div class="task-item '+(task.completed?'completed ':'')+'p'+task.priority+' '+(task.starred?'starred ':'')+(isTrash?'trash-item':'')+'" data-id="'+task.id+'" onclick="htc(event,\''+task.id+'\')" oncontextmenu="scm(event,\''+task.id+'\')">';if(hasSub)html+='<div class="expand-btn" onclick="tsub(event,\''+task.id+'\')">&#9654;</div>';else html+='<div style="width:20px;flex-shrink:0"></div>';html+='<div class="checkbox '+(task.completed?'checked':'')+'" onclick="tc(event,\''+task.id+'\')"></div>';html+='<div class="task-content"><div class="task-title">'+eh(task.title)+'</div>';if(task.desc)html+='<div style="font-size:12px;color:var(--text-3);margin-top:2px">'+eh(task.desc.substring(0,100))+(task.desc.length>100?'...':'')+'</div>';html+='<div class="task-meta">';if(task.priority)html+='<span class="task-priority p'+task.priority+'">P'+task.priority+'</span>';if(task.dueDate){var overdue=task.dueDate<new Date().toISOString().split('T')[0]&&!task.completed;html+='<span class="task-due" style="color:'+(overdue?'var(--danger)':'var(--text-3)')+'">&#128197; '+fd(task.dueDate)+'</span>'}if(project)html+='<span class="task-project" style="color:'+project.color+'">&#9632; '+project.name+'</span>';task.tags.forEach(function(tid){var tag=state.tags.find(function(t){return t.id===tid});if(tag)html+='<span class="task-tag" style="background:'+tag.color+'22;color:'+tag.color+'">#'+tag.name+'</span>'});if(task.timeEstimate)html+='<span style="font-size:11px;color:var(--text-3)">&#9201; '+task.timeEstimate+'m</span>';if(task.recurrence)html+='<span style="font-size:11px;color:var(--text-3)">&#128260; '+task.recurrence+'</span>';html+='</div></div>';if(hasSub)html+='<span class="subtask-count">'+compSub+'/'+task.subtasks.length+'</span>';if(isTrash)html+='<span class="restore-btn" onclick="rest(event,\''+task.id+'\')">Restore</span>';html+='</div>';if(hasSub){html+='<div class="subtasks" id="subtasks-'+task.id+'">';task.subtasks.forEach(function(sub){html+='<div class="task-item '+(sub.completed?'completed':'')+'" style="padding:8px 12px"><div style="width:20px;flex-shrink:0"></div><div class="checkbox '+(sub.completed?'checked':'')+'" onclick="tsubc(event,\''+task.id+'\',\''+sub.id+'\')"></div><div class="task-content"><div class="task-title" style="font-size:13px">'+eh(sub.title)+'</div></div></div>'});html+='</div>'}return html}

function renderKanban(container){var cols=[{id:'todo',name:'To Do',filter:function(t){return!t.completed&&!t.deleted}},{id:'inprogress',name:'In Progress',filter:function(t){return!t.completed&&!t.deleted&&t.timeLogged>0}},{id:'review',name:'Review',filter:function(t){return!t.completed&&!t.deleted&&t.subtasks.some(function(s){return s.completed})}},{id:'done',name:'Done',filter:function(t){return t.completed&&!t.deleted}}];var html='<div class="kanban-board">';cols.forEach(function(col){var tasks=state.tasks.filter(col.filter);html+='<div class="kanban-column"><div class="kanban-header"><span class="kanban-title">'+col.name+'</span><span class="kanban-count">'+tasks.length+'</span></div><div class="kanban-tasks">';tasks.forEach(function(t){var p=state.projects.find(function(pr){return pr.id===t.projectId});html+='<div class="kanban-task" onclick="sem(\''+t.id+'\')"><div style="font-size:13px;margin-bottom:6px">'+eh(t.title)+'</div><div style="display:flex;gap:6px;flex-wrap:wrap">';if(t.priority)html+='<span class="task-priority p'+t.priority+'" style="font-size:10px">P'+t.priority+'</span>';if(p)html+='<span style="font-size:10px;color:'+p.color+'">&#9632; '+p.name+'</span>';if(t.dueDate)html+='<span style="font-size:10px;color:var(--text-3)">&#128197; '+fd(t.dueDate)+'</span>';html+='</div></div>'});html+='</div></div>'});html+='</div>';container.innerHTML=html}

function renderMatrix(container){var today=new Date().toISOString().split('T')[0];var q1=state.tasks.filter(function(t){return!t.deleted&&!t.completed&&t.priority<=2&&t.dueDate&&t.dueDate<=today});var q2=state.tasks.filter(function(t){return!t.deleted&&!t.completed&&t.priority<=2&&(!t.dueDate||t.dueDate>today)});var q3=state.tasks.filter(function(t){return!t.deleted&&!t.completed&&t.priority>2&&t.dueDate&&t.dueDate<=today});var q4=state.tasks.filter(function(t){return!t.deleted&&!t.completed&&t.priority>2&&(!t.dueDate||t.dueDate>today)});var quads=[{id:'q1',name:'Do First',color:'var(--danger)',tasks:q1},{id:'q2',name:'Schedule',color:'var(--warning)',tasks:q2},{id:'q3',name:'Delegate',color:'var(--info)',tasks:q3},{id:'q4',name:'Eliminate',color:'var(--success)',tasks:q4}];var html='<div class="matrix-grid">';quads.forEach(function(q){html+='<div class="matrix-quadrant '+q.id+'"><div class="matrix-label"><span class="dot" style="background:'+q.color+'"></span>'+q.name+' <span style="color:var(--text-3);font-weight:400">('+q.tasks.length+')</span></div><div class="matrix-tasks">';q.tasks.forEach(function(t){html+='<div class="matrix-task" onclick="sem(\''+t.id+'\')">'+eh(t.title)+'</div>'});html+='</div></div>'});html+='</div>';container.innerHTML=html}

function renderCalendar(container){var now=new Date();var year=now.getFullYear(),month=now.getMonth();var firstDay=new Date(year,month,1);var lastDay=new Date(year,month+1,0);var startOffset=firstDay.getDay();var daysInMonth=lastDay.getDate();var days=[];for(var i=0;i<startOffset;i++)days.push({day:'',other:true});for(var i=1;i<=daysInMonth;i++){var ds=year+'-'+String(month+1).padStart(2,'0')+'-'+String(i).padStart(2,'0');days.push({day:i,dateStr:ds,tasks:state.tasks.filter(function(t){return t.dueDate===ds&&!t.deleted}),today:ds===new Date().toISOString().split('T')[0]})}var dayNames=['Sun','Mon','Tue','Wed','Thu','Fri','Sat'];var html='<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:16px"><h2 style="font-size:18px">'+now.toLocaleDateString('en-US',{month:'long',year:'numeric'})+'</h2></div>';html+='<div class="calendar-grid">';dayNames.forEach(function(d){html+='<div class="cal-header">'+d+'</div>'});days.forEach(function(d){html+='<div class="cal-day '+(d.other?'other-month ':'')+(d.today?'today':'')+'"><div class="day-num">'+d.day+'</div>';if(d.tasks)d.tasks.forEach(function(t){html+='<div class="cal-task '+(t.completed?'completed':'')+'" onclick="sem(\''+t.id+'\')">'+eh(t.title)+'</div>'});html+='</div>'});html+='</div>';container.innerHTML=html}

function renderTimeblock(container){var html='<div class="timeblock-grid">';for(var h=6;h<23;h++){var timeStr=h+':00';var hourTasks=state.tasks.filter(function(t){return!t.deleted&&!t.completed&&t.dueDate===new Date().toISOString().split('T')[0]});html+='<div class="time-label">'+timeStr+'</div><div class="time-slot" onclick="sntm(\''+timeStr+'\')">';hourTasks.forEach(function(t){html+='<div class="tb-task" onclick="event.stopPropagation();sem(\''+t.id+'\')">'+eh(t.title)+'</div>'});html+='</div>'}html+='</div>';container.innerHTML=html}

function renderDashboard(container){var today=new Date().toISOString().split('T')[0];var active=state.tasks.filter(function(t){return!t.deleted&&!t.completed});var overdue=active.filter(function(t){return t.dueDate&&t.dueDate<today});var dueToday=active.filter(function(t){return t.dueDate===today});var completed=state.tasks.filter(function(t){return t.completed}).length;var total=state.tasks.filter(function(t){return!t.deleted}).length;var pct=total>0?Math.round((completed/total)*100):0;var html='<div class="dashboard-grid">';html+='<div class="dash-card"><h3>&#128202; Overview</h3><div class="stat-row"><span>Active Tasks</span><span class="stat-num">'+active.length+'</span></div><div class="stat-row"><span>Completed</span><span class="stat-num">'+completed+'</span></div><div class="stat-row"><span>Overdue</span><span class="stat-num" style="color:var(--danger)">'+overdue.length+'</span></div><div class="stat-row"><span>Due Today</span><span class="stat-num" style="color:var(--warning)">'+dueToday.length+'</span></div><div class="progress-bar"><div class="progress-fill" style="width:'+pct+'%"></div></div><div style="text-align:center;margin-top:8px;font-size:12px;color:var(--text-3)">'+pct+'% complete</div></div>';html+='<div class="dash-card"><h3>&#128293; Priority Breakdown</h3>';var colors=['var(--danger)','var(--warning)','var(--info)','var(--success)'];var names=['Critical','High','Medium','Low'];[1,2,3,4].forEach(function(p){var count=active.filter(function(t){return t.priority===p}).length;html+='<div class="stat-row"><span style="color:'+colors[p-1]+'">&#9679; '+names[p-1]+'</span><span class="stat-num" style="color:'+colors[p-1]+'">'+count+'</span></div>'});html+='</div>';html+='<div class="dash-card"><h3>&#128193; Projects</h3>';state.projects.forEach(function(p){var pt=state.tasks.filter(function(t){return t.projectId===p.id&&!t.deleted});var pc=pt.filter(function(t){return t.completed}).length;var pp=pt.length>0?Math.round((pc/pt.length)*100):0;html+='<div style="margin-bottom:12px"><div style="display:flex;justify-content:space-between;margin-bottom:4px"><span style="font-size:13px">'+p.name+'</span><span style="font-size:12px;color:var(--text-3)">'+pc+'/'+pt.length+'</span></div><div class="progress-bar"><div class="progress-fill" style="width:'+pp+'%;background:'+p.color+'"></div></div></div>'});html+='</div>';html+='<div class="dash-card"><h3>&#127991; Tags</h3>';state.tags.forEach(function(t){var count=state.tasks.filter(function(x){return x.tags.indexOf(t.id)!==-1&&!x.deleted&&!x.completed}).length;html+='<div class="stat-row"><span><span style="color:'+t.color+'">#</span> '+t.name+'</span><span style="font-size:14px;font-weight:600">'+count+'</span></div>'});html+='</div>';html+='</div>';container.innerHTML=html}

function updateBadges(){var today=new Date().toISOString().split('T')[0];var inboxCount=state.tasks.filter(function(t){return!t.projectId&&!t.parentId&&!t.deleted&&!t.completed}).length;var todayCount=state.tasks.filter(function(t){return t.dueDate===today&&!t.deleted&&!t.completed}).length;var ib=document.getElementById('inbox-badge');var tb=document.getElementById('today-badge');if(ib){ib.textContent=inboxCount;ib.style.display=inboxCount>0?'block':'none'}if(tb){tb.textContent=todayCount;tb.style.display=todayCount>0?'block':'none'}}

function htc(e,id){if(e.shiftKey){e.preventDefault();if(state.selectedTasks.has(id))state.selectedTasks.delete(id);else state.selectedTasks.add(id);updateBulk();renderContent();return}if(state.selectedTasks.size>0&&!e.target.closest('.checkbox')){state.selectedTasks.clear();updateBulk();renderContent();return}if(!e.target.closest('.checkbox')&&!e.target.closest('.expand-btn')&&!e.target.closest('.restore-btn')){sem(id)}}
function tc(e,id){e.stopPropagation();var t=gt(id);if(t){t.completed=!t.completed;if(t.completed&&t.recurrence){var next=new Date(t.dueDate);if(t.recurrence==='daily')next.setDate(next.getDate()+1);else if(t.recurrence==='weekly')next.setDate(next.getDate()+7);else if(t.recurrence==='monthly')next.setMonth(next.getMonth()+1);else if(t.recurrence==='weekdays'){next.setDate(next.getDate()+1);while(next.getDay()===0||next.getDay()===6)next.setDate(next.getDate()+1)}var newTask=JSON.parse(JSON.stringify(t));newTask.id=gid();newTask.completed=false;newTask.dueDate=next.toISOString().split('T')[0];newTask.created=Date.now();state.tasks.push(newTask);toast('Recurring task created for '+fd(newTask.dueDate))}save();render()}}
function tsub(e,id){e.stopPropagation();var el=document.getElementById('subtasks-'+id);var btn=e.target;if(el){el.classList.toggle('visible');btn.classList.toggle('expanded')}}
function tsubc(e,taskId,subId){e.stopPropagation();var t=gt(taskId);if(t&&t.subtasks){var s=t.subtasks.find(function(x){return x.id===subId});if(s){s.completed=!s.completed;save();render()}}}
function handleSearch(val){renderContent()}
function ar(el){el.style.height='auto';el.style.height=el.scrollHeight+'px'}
function hqk(e){if(e.key==='Enter'&&!e.shiftKey){e.preventDefault();pqa()}}

function pqa(){var input=document.getElementById('quick-input');if(!input||!input.value.trim())return;var text=input.value.trim();var task={id:gid(),title:text,desc:'',projectId:null,areaId:state.areas[0].id,priority:4,dueDate:'',tags:[],starred:false,completed:false,subtasks:[],created:Date.now(),timeEstimate:0,timeLogged:0,parentId:null,deleted:false,recurrence:'',startDate:''};var lower=text.toLowerCase();if(lower.indexOf('tomorrow')!==-1){var t=new Date();t.setDate(t.getDate()+1);task.dueDate=t.toISOString().split('T')[0];task.title=text.replace(/tomorrow/gi,'').trim()}else if(lower.indexOf('today')!==-1){task.dueDate=new Date().toISOString().split('T')[0];task.title=text.replace(/today/gi,'').trim()}var pMatch=text.match(/\bp([1-4])\b/i);if(pMatch){task.priority=parseInt(pMatch[1]);task.title=task.title.replace(/\bp[1-4]\b/gi,'').trim()}state.tags.forEach(function(tag){if(lower.indexOf('#'+tag.name)!==-1){task.tags.push(tag.id);task.title=task.title.replace('#'+tag.name,'').trim()}});state.tasks.push(task);input.value='';input.style.height='auto';save();render();toast('Task added')}

function showNewTaskModal(){state.editingTaskId=null;state.selectedPriority=4;state.selectedTags.clear();document.getElementById('modal-title').textContent='New Task';document.getElementById('task-title-input').value='';document.getElementById('task-desc-input').value='';document.getElementById('task-due-input').value='';document.getElementById('task-start-input').value='';document.getElementById('task-estimate-input').value='';document.getElementById('task-star-input').checked=false;document.getElementById('task-recur-input').value='';document.getElementById('modal-delete-btn').style.display='none';popSel();renderTP();renderPP();document.getElementById('task-modal').classList.add('active')}
function sem(id){var t=gt(id);if(!t)return;state.editingTaskId=id;state.selectedPriority=t.priority;state.selectedTags=new Set(t.tags);document.getElementById('modal-title').textContent='Edit Task';document.getElementById('task-title-input').value=t.title;document.getElementById('task-desc-input').value=t.desc||'';document.getElementById('task-due-input').value=t.dueDate||'';document.getElementById('task-start-input').value=t.startDate||'';document.getElementById('task-estimate-input').value=t.timeEstimate||'';document.getElementById('task-star-input').checked=t.starred;document.getElementById('task-recur-input').value=t.recurrence||'';document.getElementById('modal-delete-btn').style.display='inline-block';popSel();document.getElementById('task-project-input').value=t.projectId||'';document.getElementById('task-area-input').value=t.areaId||'';renderTP();renderPP();document.getElementById('task-modal').classList.add('active')}
function popSel(){var ps=document.getElementById('task-project-input');var as=document.getElementById('task-area-input');var pas=document.getElementById('project-area-input');if(ps)ps.innerHTML='<option value="">None</option>'+state.projects.map(function(p){return'<option value="'+p.id+'">'+p.name+'</option>'}).join('');if(as)as.innerHTML=state.areas.map(function(a){return'<option value="'+a.id+'">'+a.name+'</option>'}).join('');if(pas)pas.innerHTML=state.areas.map(function(a){return'<option value="'+a.id+'">'+a.name+'</option>'}).join('')}
function renderTP(){var el=document.getElementById('tag-picker');if(!el)return;el.innerHTML=state.tags.map(function(t){var sel=state.selectedTags.has(t.id)?'selected':'';return'<div class="tag-option '+sel+'" onclick="tt(\''+t.id+'\')" style="background:'+(sel?t.color:'')+';color:'+(sel?'#fff':'')+'">#'+t.name+'</div>'}).join('')}
function tt(id){if(state.selectedTags.has(id))state.selectedTags.delete(id);else state.selectedTags.add(id);renderTP()}
function selectPriority(p){state.selectedPriority=p;renderPP()}
function renderPP(){document.querySelectorAll('.priority-option').forEach(function(el){el.classList.toggle('selected',parseInt(el.dataset.p)===state.selectedPriority)})}
function saveTask(){var title=document.getElementById('task-title-input').value.trim();if(!title){toast('Title is required','error');return}var desc=document.getElementById('task-desc-input').value.trim();var projectId=document.getElementById('task-project-input').value||null;var areaId=document.getElementById('task-area-input').value;var dueDate=document.getElementById('task-due-input').value;var startDate=document.getElementById('task-start-input').value;var estimate=parseInt(document.getElementById('task-estimate-input').value)||0;var starred=document.getElementById('task-star-input').checked;var recurrence=document.getElementById('task-recur-input').value;var tags=Array.from(state.selectedTags);if(state.editingTaskId){var t=gt(state.editingTaskId);if(t){t.title=title;t.desc=desc;t.projectId=projectId;t.areaId=areaId;t.dueDate=dueDate;t.startDate=startDate;t.priority=state.selectedPriority;t.timeEstimate=estimate;t.starred=starred;t.recurrence=recurrence;t.tags=tags}}else{state.tasks.push({id:gid(),title:title,desc:desc,projectId:projectId,areaId:areaId,priority:state.selectedPriority,dueDate:dueDate,tags:tags,starred:starred,completed:false,subtasks:[],created:Date.now(),timeEstimate:estimate,timeLogged:0,parentId:null,deleted:false,recurrence:recurrence,startDate:startDate})}save();closeModal();render();toast(state.editingTaskId?'Task updated':'Task created')}
function deleteTaskFromModal(){if(!state.editingTaskId)return;mt(state.editingTaskId);closeModal();render();toast('Task moved to trash')}
function closeModal(){document.getElementById('task-modal').classList.remove('active');state.editingTaskId=null}

function showNewProjectModal(){popSel();document.getElementById('project-modal').classList.add('active')}
function closeProjectModal(){document.getElementById('project-modal').classList.remove('active')}
function saveProject(){var name=document.getElementById('project-name-input').value.trim();if(!name){toast('Project name required','error');return}var areaId=document.getElementById('project-area-input').value;var color=document.getElementById('project-color-input').value;state.projects.push({id:gid(),name:name,areaId:areaId,color:color,created:Date.now()});save();closeProjectModal();render();toast('Project created')}

function showExportModal(){document.getElementById('export-modal').classList.add('active')}
function closeExportModal(){document.getElementById('export-modal').classList.remove('active')}
function exportData(format){var data={tasks:state.tasks,projects:state.projects,areas:state.areas,tags:state.tags,trash:state.trash};var blob,filename;if(format==='json'){blob=new Blob([JSON.stringify(data,null,2)],{type:'application/json'});filename='zenflow-backup.json'}else if(format==='csv'){var csv='Title,Description,Project,Area,Priority,Due Date,Completed,Tags\n';state.tasks.forEach(function(t){var proj=state.projects.find(function(p){return p.id===t.projectId});var area=state.areas.find(function(a){return a.id===t.areaId});var tagNames=t.tags.map(function(tid){var tag=state.tags.find(function(x){return x.id===tid});return tag?tag.name:''}).join(', ');csv+='"'+t.title+'","'+(t.desc||'')+'","'+(proj?proj.name:'')+'","'+(area?area.name:'')+'",'+t.priority+',"'+(t.dueDate||'')+'",'+t.completed+',"'+tagNames+'"\n'});blob=new Blob([csv],{type:'text/csv'});filename='zenflow-tasks.csv'}else if(format==='markdown'){var md='# ZenFlow Tasks\n\n';state.tasks.forEach(function(t){md+='- ['+(t.completed?'x':' ')+'] '+t.title;if(t.dueDate)md+=' (due: '+t.dueDate+')';if(t.priority)md+=' [P'+t.priority+']';md+='\n';if(t.desc)md+='  - '+t.desc+'\n'});blob=new Blob([md],{type:'text/markdown'});filename='zenflow-tasks.md'}var a=document.createElement('a');a.href=URL.createObjectURL(blob);a.download=filename;a.click();toast('Exported as '+format.toUpperCase())}
function importData(input){var file=input.files[0];if(!file)return;var reader=new FileReader();reader.onload=function(e){try{var data=JSON.parse(e.target.result);if(data.tasks)state.tasks=data.tasks;if(data.projects)state.projects=data.projects;if(data.areas)state.areas=data.areas;if(data.tags)state.tags=data.tags;if(data.trash)state.trash=data.trash;save();render();toast('Data imported successfully')}catch(err){toast('Invalid JSON file','error')}};reader.readAsText(file);input.value=''}
function clearAllData(){if(!confirm('Are you sure? This will delete ALL data permanently.'))return;state.tasks=[];state.projects=[];state.areas=[];state.tags=[];state.trash=[];localStorage.removeItem('zenflow_data');initDemo();save();render();toast('All data cleared')}

function scm(e,id){e.preventDefault();state.contextTaskId=id;var menu=document.getElementById('context-menu');menu.style.left=e.clientX+'px';menu.style.top=e.clientY+'px';menu.classList.add('active')}
function hideCM(){document.getElementById('context-menu').classList.remove('active')}
document.addEventListener('click',function(e){if(!e.target.closest('.context-menu'))hideCM()});
function ctxEdit(){if(state.contextTaskId)sem(state.contextTaskId);hideCM()}
function ctxDuplicate(){if(!state.contextTaskId)return;var t=gt(state.contextTaskId);if(t){var dup=JSON.parse(JSON.stringify(t));dup.id=gid();dup.title=t.title+' (copy)';dup.completed=false;dup.created=Date.now();state.tasks.push(dup);save();render();toast('Task duplicated')}hideCM()}
function ctxStar(){if(!state.contextTaskId)return;var t=gt(state.contextTaskId);if(t){t.starred=!t.starred;save();render()}hideCM()}
function ctxFocus(){if(state.contextTaskId)enterFocusMode(state.contextTaskId);hideCM()}
function ctxAddSubtask(){if(!state.contextTaskId)return;var t=gt(state.contextTaskId);if(t){var title=prompt('Subtask title:');if(title){t.subtasks.push({id:gid(),title:title,completed:false});save();render()}}hideCM()}
function ctxMove(){if(!state.contextTaskId)return;var t=gt(state.contextTaskId);if(t){var names=state.projects.map(function(p,i){return(i+1)+'. '+p.name}).join('\n');var choice=prompt('Move to project (enter number):\n'+names);var idx=parseInt(choice)-1;if(idx>=0&&idx<state.projects.length){t.projectId=state.projects[idx].id;t.areaId=state.projects[idx].areaId;save();render();toast('Task moved')}}hideCM()}
function ctxDelete(){if(state.contextTaskId){mt(state.contextTaskId);render();toast('Moved to trash')}hideCM()}

function mt(id){var t=gt(id);if(t){t.deleted=true;t.deletedAt=Date.now();state.trash.push(t)}}
function rest(e,id){e.stopPropagation();var idx=state.trash.findIndex(function(t){return t.id===id});if(idx!==-1){var t=state.trash[idx];t.deleted=false;t.deletedAt=null;state.trash.splice(idx,1);save();render();toast('Task restored')}}

function updateBulk(){var bar=document.getElementById('bulk-bar');var count=document.getElementById('bulk-count');if(state.selectedTasks.size>0){bar.classList.add('active');count.textContent=state.selectedTasks.size+' selected'}else{bar.classList.remove('active')}}
function clearSelection(){state.selectedTasks.clear();updateBulk();renderContent()}
function bulkComplete(){state.selectedTasks.forEach(function(id){var t=gt(id);if(t)t.completed=true});save();clearSelection();render();toast('Tasks completed')}
function bulkStar(){state.selectedTasks.forEach(function(id){var t=gt(id);if(t)t.starred=!t.starred});save();clearSelection();render();toast('Tasks starred')}
function bulkMove(){var names=state.projects.map(function(p,i){return(i+1)+'. '+p.name}).join('\n');var choice=prompt('Move to project (enter number):\n'+names);var idx=parseInt(choice)-1;if(idx>=0&&idx<state.projects.length){var pid=state.projects[idx].id;var aid=state.projects[idx].areaId;state.selectedTasks.forEach(function(id){var t=gt(id);if(t){t.projectId=pid;t.areaId=aid}});save();clearSelection();render();toast('Tasks moved')}}
function bulkTag(){var names=state.tags.map(function(t,i){return(i+1)+'. '+t.name}).join('\n');var choice=prompt('Add tag (enter number):\n'+names);var idx=parseInt(choice)-1;if(idx>=0&&idx<state.tags.length){var tid=state.tags[idx].id;state.selectedTasks.forEach(function(id){var t=gt(id);if(t&&t.tags.indexOf(tid)===-1)t.tags.push(tid)});save();clearSelection();render();toast('Tags added')}}
function bulkDelete(){if(!confirm('Move '+state.selectedTasks.size+' tasks to trash?'))return;state.selectedTasks.forEach(function(id){mt(id)});save();clearSelection();render();toast('Tasks moved to trash')}

function enterFocusMode(taskId){var id=taskId||state.contextTaskId;if(!id){var active=state.tasks.filter(function(t){return!t.deleted&&!t.completed});if(active.length>0)id=active[0].id}if(id){var t=gt(id);if(t)document.getElementById('focus-task-title').textContent=t.title}document.getElementById('focus-overlay').classList.add('active')}
function exitFocusMode(){document.getElementById('focus-overlay').classList.remove('active');if(state.pomodoro.interval){clearInterval(state.pomodoro.interval);state.pomodoro.interval=null}state.pomodoro.running=false;updatePomo()}
function togglePomodoro(){if(state.pomodoro.running){clearInterval(state.pomodoro.interval);state.pomodoro.interval=null;state.pomodoro.running=false}else{state.pomodoro.running=true;state.pomodoro.interval=setInterval(function(){state.pomodoro.time--;if(state.pomodoro.time<=0){clearInterval(state.pomodoro.interval);state.pomodoro.interval=null;state.pomodoro.running=false;toast('Pomodoro complete!');if(state.pomodoro.mode==='work'){state.pomodoro.mode='break';state.pomodoro.time=state.pomodoro.session%4===0?900:300;document.getElementById('pomo-mode').textContent='Break'}else{state.pomodoro.mode='work';state.pomodoro.session++;state.pomodoro.time=1500;document.getElementById('pomo-mode').textContent='Focus';document.getElementById('pomo-session').textContent='Work session '+state.pomodoro.session+'/4'}}updatePomo()},1000)}updatePomo()}
function resetPomodoro(){if(state.pomodoro.interval){clearInterval(state.pomodoro.interval);state.pomodoro.interval=null}state.pomodoro.running=false;state.pomodoro.time=1500;state.pomodoro.mode='work';state.pomodoro.session=1;document.getElementById('pomo-mode').textContent='Focus';document.getElementById('pomo-session').textContent='Work session 1/4';updatePomo()}
function skipPomodoro(){if(state.pomodoro.interval){clearInterval(state.pomodoro.interval);state.pomodoro.interval=null}state.pomodoro.running=false;if(state.pomodoro.mode==='work'){state.pomodoro.mode='break';state.pomodoro.time=state.pomodoro.session%4===0?900:300;document.getElementById('pomo-mode').textContent='Break'}else{state.pomodoro.mode='work';state.pomodoro.session++;state.pomodoro.time=1500;document.getElementById('pomo-mode').textContent='Focus';document.getElementById('pomo-session').textContent='Work session '+state.pomodoro.session+'/4'}updatePomo()}
function updatePomo(){var m=Math.floor(state.pomodoro.time/60);var s=state.pomodoro.time%60;document.getElementById('pomodoro-timer').textContent=String(m).padStart(2,'0')+':'+String(s).padStart(2,'0');document.getElementById('pomo-play').textContent=state.pomodoro.running?'&#9208;':'&#9654;'}

function sntm(time){showNewTaskModal();document.getElementById('task-due-input').value=new Date().toISOString().split('T')[0]}

function init(){load();if(state.tasks.length===0)initDemo();render();document.body.classList.toggle('light',state.theme==='light')}
init();
</script>
</body>
</html>
