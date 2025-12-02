<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>My Eisenhower</title>

<style>
    body {
        margin: 0;
        font-family: 'Prompt', sans-serif;
        background: #1f1f1f;
        color: #fff;
    }

    /* Header */
    header {
        background: linear-gradient(135deg, #2b3a55, #1f2a40);
        padding: 24px 20px;
        text-align: center;
        color: #fff;
        border-bottom: 1px solid rgba(255,255,255,0.08);
    }

    header h1 {
        font-size: 1.8rem;
        margin: 0;
        font-weight: 600;
    }

    header p {
        margin-top: 6px;
        font-size: 1rem;
        opacity: 0.8;
    }

    /* Container */
    .container {
        padding: 16px;
        max-width: 600px;
        margin: auto;
    }

    /* Input Section */
    .box {
        background: #2d2d2d;
        padding: 16px;
        border-radius: 14px;
        margin-bottom: 20px;
        border: 1px solid rgba(255,255,255,0.05);
    }

    .box label {
        font-size: 0.9rem;
        opacity: 0.8;
    }

    .box input, .box select {
        width: 100%;
        padding: 12px;
        margin-top: 8px;
        border-radius: 10px;
        border: none;
        font-size: 1rem;
        background: #3a3a3a;
        color: #fff;
    }

    .btn-add {
        margin-top: 12px;
        width: 100%;
        padding: 12px;
        background: #4a90e2;
        border: none;
        border-radius: 12px;
        color: #fff;
        font-size: 1rem;
        font-weight: 600;
    }

    /* Grid Quadrants */
    .grid {
        display: grid;
        grid-template-columns: 1fr;
        gap: 14px;
    }

    @media (min-width: 720px) {
        .grid {
            grid-template-columns: 1fr 1fr;
        }
    }

    .quadrant {
        background: #2d2d2d;
        padding: 14px;
        border-radius: 14px;
        min-height: 160px;
        border: 1px solid rgba(255,255,255,0.05);
    }

    .quadrant h3 {
        margin-top: 0;
        font-size: 1.1rem;
        margin-bottom: 10px;
    }

    .task {
        background: #3b3b3b;
        padding: 10px;
        border-radius: 10px;
        margin-bottom: 10px;
        font-size: 0.9rem;
    }
</style>
</head>

<body>

<header>
    <h1>My Eisenhower</h1>
    <p>จัดการงานด้วย Eisenhower Matrix</p>
</header>

<div class="container">

    <!-- Add Task -->
    <div class="box">
        <label>เพิ่มงาน</label>
        <input type="text" id="taskInput" placeholder="เช่น ส่งรายงาน, ทำสไลด์...">

        <label style="margin-top:12px; display:block;">ความสำคัญ</label>
        <select id="importance">
            <option value="high">สำคัญ</option>
            <option value="low">ไม่สำคัญ</option>
        </select>

        <label style="margin-top:12px; display:block;">ความเร่งด่วน</label>
        <select id="urgency">
            <option value="high">เร่งด่วน</option>
            <option value="low">ไม่เร่งด่วน</option>
        </select>

        <button class="btn-add" onclick="addTask()">เพิ่มงาน</button>
    </div>

    <!-- Matrix -->
    <div class="grid">

        <div class="quadrant" id="do">
            <h3>📌 ทำทันที (สำคัญ + เร่งด่วน)</h3>
        </div>

        <div class="quadrant" id="plan">
            <h3>📅 วางแผน (สำคัญ + ไม่เร่งด่วน)</h3>
        </div>

        <div class="quadrant" id="delegate">
            <h3>🤝 มอบหมาย (ไม่สำคัญ + เร่งด่วน)</h3>
        </div>

        <div class="quadrant" id="eliminate">
            <h3>🚫 ตัดออก (ไม่สำคัญ + ไม่เร่งด่วน)</h3>
        </div>
    </div>
</div>

<script>
function addTask() {
    const text = document.getElementById("taskInput").value;
    const importance = document.getElementById("importance").value;
    const urgency = document.getElementById("urgency").value;

    if (!text) return;

    let target = "";

    if (importance === "high" && urgency === "high") target = "do";
    if (importance === "high" && urgency === "low") target = "plan";
    if (importance === "low" && urgency === "high") target = "delegate";
    if (importance === "low" && urgency === "low") target = "eliminate";

    const taskBox = document.createElement("div");
    taskBox.className = "task";
    taskBox.innerText = text;

    document.getElementById(target).appendChild(taskBox);

    document.getElementById("taskInput").value = "";
}
</script>

</body>
</html>
