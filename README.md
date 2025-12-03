<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Eisenhower - Task Manager</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #F4F4F4 0%, #FFFFFF 100%);
            min-height: 100vh;
            padding: 20px;
            position: relative;
            overflow-x: hidden;
        }

        body::before {
            content: '';
            position: fixed;
            width: 600px;
            height: 600px;
            background: radial-gradient(circle, rgba(26, 75, 132, 0.08) 0%, transparent 70%);
            border-radius: 50% 40% 60% 50%;
            top: -200px;
            right: -200px;
            z-index: 0;
            animation: float 20s ease-in-out infinite;
        }

        body::after {
            content: '';
            position: fixed;
            width: 400px;
            height: 400px;
            background: radial-gradient(circle, rgba(251, 140, 0, 0.06) 0%, transparent 70%);
            border-radius: 60% 50% 40% 60%;
            bottom: -100px;
            left: -100px;
            z-index: 0;
            animation: float 15s ease-in-out infinite reverse;
        }

        @keyframes float {
            0%, 100% {
                transform: translate(0, 0) rotate(0deg);
            }
            33% {
                transform: translate(30px, -30px) rotate(5deg);
            }
            66% {
                transform: translate(-20px, 20px) rotate(-5deg);
            }
        }

        .container {
            max-width: 500px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.08), 0 8px 16px rgba(0,0,0,0.04);
            overflow: hidden;
            position: relative;
            z-index: 1;
        }

        .header {
            background: #1A4B84;
            color: white;
            padding: 25px;
            text-align: center;
        }

        .header h1 {
            font-size: 28px;
            margin-bottom: 5px;
        }

        .header p {
            font-size: 14px;
            opacity: 0.9;
        }

        .content {
            padding: 25px;
        }

        .form-section {
            margin-bottom: 25px;
        }

        .input-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
            color: #333;
            font-size: 14px;
        }

        input[type="text"],
        input[type="date"],
        select {
            width: 100%;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: 14px;
            transition: border-color 0.3s, box-shadow 0.3s;
        }

        input[type="text"]:focus,
        input[type="date"]:focus,
        select:focus {
            outline: none;
            border-color: #1A4B84;
            box-shadow: 0 0 0 3px rgba(26, 75, 132, 0.1);
        }

        .priority-group {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-bottom: 15px;
        }

        .priority-btn {
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            background: white;
            cursor: pointer;
            font-size: 14px;
            font-weight: 600;
            transition: all 0.3s;
        }

        .priority-btn.active {
            transform: scale(1.05);
        }

        .priority-btn.important {
            border-color: #e74c3c;
            background: #fee;
            color: #e74c3c;
        }

        .priority-btn.not-important {
            border-color: #95a5a6;
            background: #f5f5f5;
            color: #666;
        }

        .priority-btn.urgent {
            border-color: #f39c12;
            background: #fff5e6;
            color: #f39c12;
        }

        .priority-btn.not-urgent {
            border-color: #3498db;
            background: #e8f4f8;
            color: #3498db;
        }

        .add-task-btn {
            width: 100%;
            padding: 15px;
            background: #FB8C00;
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: transform 0.2s, background 0.2s;
        }

        .add-task-btn:hover {
            transform: translateY(-2px);
            background: #F57C00;
        }

        .matrix-container {
            display: none;
            margin-top: 30px;
            position: relative;
        }

        .matrix-container::before {
            content: '';
            position: absolute;
            width: 120%;
            height: 120%;
            background: radial-gradient(ellipse at center, rgba(26, 75, 132, 0.03) 0%, transparent 70%);
            border-radius: 60% 40% 50% 60%;
            top: -10%;
            left: -10%;
            z-index: -1;
            animation: pulse 8s ease-in-out infinite;
        }

        @keyframes pulse {
            0%, 100% {
                transform: scale(1) rotate(0deg);
                opacity: 0.5;
            }
            50% {
                transform: scale(1.05) rotate(2deg);
                opacity: 0.8;
            }
        }

        .matrix-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-bottom: 20px;
        }

        .quadrant {
            border: 2px solid #ddd;
            border-radius: 15px;
            padding: 15px;
            min-height: 150px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
            transition: transform 0.3s, box-shadow 0.3s;
        }

        .quadrant:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(0,0,0,0.08);
        }

        .quadrant h3 {
            font-size: 14px;
            margin-bottom: 10px;
            padding-bottom: 8px;
            border-bottom: 2px solid;
        }

        .q1 {
            background: #fee;
            border-color: #e74c3c;
        }

        .q1 h3 {
            color: #e74c3c;
            border-color: #e74c3c;
        }

        .q2 {
            background: #e8f8f5;
            border-color: #27ae60;
        }

        .q2 h3 {
            color: #27ae60;
            border-color: #27ae60;
        }

        .q3 {
            background: #fff5e6;
            border-color: #f39c12;
        }

        .q3 h3 {
            color: #f39c12;
            border-color: #f39c12;
        }

        .q4 {
            background: #f5f5f5;
            border-color: #95a5a6;
        }

        .q4 h3 {
            color: #95a5a6;
            border-color: #95a5a6;
        }

        .task-item {
            background: white;
            padding: 10px;
            border-radius: 8px;
            margin-bottom: 8px;
            font-size: 13px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.06);
            transition: transform 0.2s, box-shadow 0.2s;
        }

        .task-item:hover {
            transform: translateX(3px);
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }

        .task-item strong {
            display: block;
            margin-bottom: 3px;
        }

        .task-item small {
            color: #666;
            display: block;
            margin-top: 5px;
        }

        .ai-section {
            background: linear-gradient(135deg, #1A4B84 0%, #2563A8 100%);
            color: white;
            padding: 20px;
            border-radius: 15px;
            margin-top: 20px;
            box-shadow: 0 8px 24px rgba(26, 75, 132, 0.2);
            position: relative;
            overflow: hidden;
        }

        .ai-section::before {
            content: '';
            position: absolute;
            width: 200px;
            height: 200px;
            background: radial-gradient(circle, rgba(255, 255, 255, 0.1) 0%, transparent 70%);
            border-radius: 50%;
            top: -100px;
            right: -50px;
            pointer-events: none;
        }

        .ai-section h3 {
            font-size: 16px;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 8px;
            position: relative;
            z-index: 1;
        }

        .ai-section ol {
            padding-left: 20px;
            line-height: 1.6;
            position: relative;
            z-index: 1;
        }

        .ai-section li {
            margin-bottom: 8px;
            font-size: 14px;
        }

        .action-buttons {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-top: 20px;
        }

        .btn {
            padding: 12px;
            border: none;
            border-radius: 10px;
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
            transition: transform 0.2s;
        }

        .btn:hover {
            transform: translateY(-2px);
        }

        .btn-save {
            background: #27ae60;
            color: white;
        }

        .btn-reset {
            background: #e74c3c;
            color: white;
        }

        .empty-state {
            text-align: center;
            color: #999;
            font-style: italic;
            padding: 20px;
            font-size: 13px;
        }

        #captureArea {
            background: white;
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.06);
        }

        .user-info {
            text-align: center;
            padding: 15px;
            background: linear-gradient(135deg, #f8f9fa 0%, #ffffff 100%);
            border-radius: 10px;
            margin-bottom: 20px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.04);
        }

        .user-info h2 {
            font-size: 18px;
            color: #1A4B84;
            margin-bottom: 5px;
        }

        .subtask {
            font-size: 12px;
            color: #666;
            padding-left: 10px;
            margin-top: 3px;
        }

        .subtask-item {
            background: #f8f9fa;
            padding: 8px 10px;
            border-radius: 8px;
            margin-bottom: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 13px;
        }

        .subtask-item .subtask-info {
            flex: 1;
        }

        .subtask-item .remove-btn {
            background: #e74c3c;
            color: white;
            border: none;
            border-radius: 5px;
            padding: 5px 10px;
            cursor: pointer;
            font-size: 12px;
            font-weight: 600;
        }

        .subtask-date {
            color: #1A4B84;
            font-weight: 600;
            font-size: 11px;
            margin-top: 2px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎯 My Eisenhower</h1>
            <p>จัดการงานด้วย Eisenhower Matrix</p>
        </div>

        <div class="content">
            <div class="form-section">
                <div class="input-group">
                    <label>👤 ชื่อผู้ใช้</label>
                    <input type="text" id="username" placeholder="กรอกชื่อของคุณ">
                </div>

                <div class="input-group">
                    <label>📅 เดือน</label>
                    <select id="month">
                        <option value="">เลือกเดือน</option>
                        <option value="มกราคม">มกราคม</option>
                        <option value="กุมภาพันธ์">กุมภาพันธ์</option>
                        <option value="มีนาคม">มีนาคม</option>
                        <option value="เมษายน">เมษายน</option>
                        <option value="พฤษภาคม">พฤษภาคม</option>
                        <option value="มิถุนายน">มิถุนายน</option>
                        <option value="กรกฎาคม">กรกฎาคม</option>
                        <option value="สิงหาคม">สิงหาคม</option>
                        <option value="กันยายน">กันยายน</option>
                        <option value="ตุลาคม">ตุลาคม</option>
                        <option value="พฤศจิกายน">พฤศจิกายน</option>
                        <option value="ธันวาคม">ธันวาคม</option>
                    </select>
                </div>

                <div class="input-group">
                    <label>📝 ชื่องาน</label>
                    <input type="text" id="taskName" placeholder="เช่น ทำรายงาน, ประชุมทีม">
                </div>

                <div class="input-group">
                    <label>📋 กิจกรรมย่อย</label>
                    <div id="subtasksList"></div>
                    <div style="display: flex; gap: 10px; margin-top: 10px;">
                        <input type="text" id="subtaskName" placeholder="ชื่อกิจกรรมย่อย" style="flex: 1;">
                        <input type="date" id="subtaskDate" style="flex: 1;">
                        <button onclick="addSubtask()" style="padding: 12px 20px; background: #FB8C00; color: white; border: none; border-radius: 8px; cursor: pointer; font-weight: 600;">+</button>
                    </div>
                </div>

                <label style="margin-top: 20px;">🔴 ระดับความสำคัญ</label>
                <div class="priority-group">
                    <button class="priority-btn" data-type="importance" data-value="important">สำคัญ</button>
                    <button class="priority-btn" data-type="importance" data-value="not-important">ไม่สำคัญ</button>
                </div>

                <label>⚡ ระดับความเร่งด่วน</label>
                <div class="priority-group">
                    <button class="priority-btn" data-type="urgency" data-value="urgent">เร่งด่วน</button>
                    <button class="priority-btn" data-type="urgency" data-value="not-urgent">ไม่เร่งด่วน</button>
                </div>

                <button class="add-task-btn" onclick="addTask()">+ เพิ่มงาน</button>
            </div>

            <div class="matrix-container" id="matrixContainer">
                <div id="captureArea">
                    <div class="user-info">
                        <h2 id="displayUsername"></h2>
                        <p id="displayMonth"></p>
                    </div>

                    <div class="matrix-grid">
                        <div class="quadrant q1">
                            <h3>สำคัญ & เร่งด่วน</h3>
                            <div id="q1"></div>
                        </div>
                        <div class="quadrant q2">
                            <h3>สำคัญ & ไม่เร่งด่วน</h3>
                            <div id="q2"></div>
                        </div>
                        <div class="quadrant q3">
                            <h3>ไม่สำคัญ & เร่งด่วน</h3>
                            <div id="q3"></div>
                        </div>
                        <div class="quadrant q4">
                            <h3>ไม่สำคัญ & ไม่เร่งด่วน</h3>
                            <div id="q4"></div>
                        </div>
                    </div>

                    <div class="ai-section">
                        <h3>🤖 คำแนะนำจาก AI Assistant</h3>
                        <div id="aiRecommendation"></div>
                    </div>
                </div>

                <div class="action-buttons">
                    <button class="btn btn-save" id="saveBtn">💾 บันทึกเป็นรูป</button>
                    <button class="btn btn-reset" onclick="resetAll()">🔄 เริ่มใหม่</button>
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script>
        let tasks = [];
        let selectedImportance = null;
        let selectedUrgency = null;
        let currentSubtasks = [];

        // Handle priority button clicks
        document.querySelectorAll('.priority-btn').forEach(btn => {
            btn.addEventListener('click', function() {
                const type = this.dataset.type;
                const value = this.dataset.value;
                
                document.querySelectorAll(`[data-type="${type}"]`).forEach(b => {
                    b.classList.remove('active', 'important', 'not-important', 'urgent', 'not-urgent');
                });
                
                this.classList.add('active', value);
                
                if (type === 'importance') {
                    selectedImportance = value;
                } else {
                    selectedUrgency = value;
                }
            });
        });

        function addSubtask() {
            const subtaskName = document.getElementById('subtaskName').value;
            const subtaskDate = document.getElementById('subtaskDate').value;

            if (!subtaskName || !subtaskDate) {
                alert('กรุณากรอกชื่อกิจกรรมย่อยและวันกำหนดส่ง!');
                return;
            }

            currentSubtasks.push({
                name: subtaskName,
                date: subtaskDate
            });

            displaySubtasks();
            document.getElementById('subtaskName').value = '';
            document.getElementById('subtaskDate').value = '';
        }

        function displaySubtasks() {
            const container = document.getElementById('subtasksList');
            container.innerHTML = '';

            currentSubtasks.forEach((subtask, index) => {
                const div = document.createElement('div');
                div.className = 'subtask-item';
                div.innerHTML = `
                    <div class="subtask-info">
                        <div>${subtask.name}</div>
                        <div class="subtask-date">📅 ${new Date(subtask.date).toLocaleDateString('th-TH')}</div>
                    </div>
                    <button class="remove-btn" onclick="removeSubtask(${index})">ลบ</button>
                `;
                container.appendChild(div);
            });
        }

        function removeSubtask(index) {
            currentSubtasks.splice(index, 1);
            displaySubtasks();
        }

        function addTask() {
            const username = document.getElementById('username').value;
            const month = document.getElementById('month').value;
            const taskName = document.getElementById('taskName').value;

            if (!username || !month || !taskName || !selectedImportance || !selectedUrgency) {
                alert('กรุณากรอกข้อมูลให้ครบถ้วน!');
                return;
            }

            if (currentSubtasks.length === 0) {
                alert('กรุณาเพิ่มกิจกรรมย่อยอย่างน้อย 1 รายการ!');
                return;
            }

            const task = {
                name: taskName,
                subtasks: [...currentSubtasks],
                importance: selectedImportance,
                urgency: selectedUrgency,
                username: username,
                month: month
            };

            tasks.push(task);
            displayMatrix();
            clearForm();
        }

        function displayMatrix() {
            const username = document.getElementById('username').value;
            const month = document.getElementById('month').value;

            document.getElementById('displayUsername').textContent = username;
            document.getElementById('displayMonth').textContent = `เดือน ${month}`;
            document.getElementById('matrixContainer').style.display = 'block';

            // Clear quadrants
            ['q1', 'q2', 'q3', 'q4'].forEach(q => {
                document.getElementById(q).innerHTML = '';
            });

            // Categorize tasks
            tasks.forEach(task => {
                let quadrant;
                if (task.importance === 'important' && task.urgency === 'urgent') {
                    quadrant = 'q1';
                } else if (task.importance === 'important' && task.urgency === 'not-urgent') {
                    quadrant = 'q2';
                } else if (task.importance === 'not-important' && task.urgency === 'urgent') {
                    quadrant = 'q3';
                } else {
                    quadrant = 'q4';
                }

                const taskElement = document.createElement('div');
                taskElement.className = 'task-item';
                
                let subtasksHtml = '';
                task.subtasks.forEach(st => {
                    subtasksHtml += `<div class="subtask">• ${st.name} <span style="color: #1A4B84;">(${new Date(st.date).toLocaleDateString('th-TH')})</span></div>`;
                });
                
                taskElement.innerHTML = `
                    <strong>${task.name}</strong>
                    ${subtasksHtml}
                `;
                document.getElementById(quadrant).appendChild(taskElement);
            });

            // Check for empty quadrants
            ['q1', 'q2', 'q3', 'q4'].forEach(q => {
                if (document.getElementById(q).children.length === 0) {
                    document.getElementById(q).innerHTML = '<div class="empty-state">ไม่มีงาน</div>';
                }
            });

            generateAIRecommendation();
        }

        function generateAIRecommendation() {
            const q1Tasks = tasks.filter(t => t.importance === 'important' && t.urgency === 'urgent');
            const q2Tasks = tasks.filter(t => t.importance === 'important' && t.urgency === 'not-urgent');
            const q3Tasks = tasks.filter(t => t.importance === 'not-important' && t.urgency === 'urgent');
            const q4Tasks = tasks.filter(t => t.importance === 'not-important' && t.urgency === 'not-urgent');

            let recommendations = '<ol>';

            if (q1Tasks.length > 0) {
                recommendations += '<li><strong>ทำทันที:</strong> ';
                recommendations += q1Tasks.map(t => t.name).join(', ');
                recommendations += ' (สำคัญและเร่งด่วน)</li>';
            }

            if (q2Tasks.length > 0) {
                recommendations += '<li><strong>วางแผนทำ:</strong> ';
                recommendations += q2Tasks.map(t => t.name).join(', ');
                recommendations += ' (สำคัญแต่ไม่เร่งด่วน - จัดตารางเวลาที่ดี)</li>';
            }

            if (q3Tasks.length > 0) {
                recommendations += '<li><strong>พิจารณามอบหมาย:</strong> ';
                recommendations += q3Tasks.map(t => t.name).join(', ');
                recommendations += ' (เร่งด่วนแต่ไม่สำคัญ - ถ้าทำได้ให้คนอื่นช่วย)</li>';
            }

            if (q4Tasks.length > 0) {
                recommendations += '<li><strong>ลดหรือยกเลิก:</strong> ';
                recommendations += q4Tasks.map(t => t.name).join(', ');
                recommendations += ' (ไม่สำคัญและไม่เร่งด่วน - พิจารณาว่าจำเป็นหรือไม่)</li>';
            }

            recommendations += '</ol>';

            if (tasks.length === 0) {
                recommendations = '<p>ยังไม่มีงาน เพิ่มงานเพื่อรับคำแนะนำจาก AI</p>';
            }

            document.getElementById('aiRecommendation').innerHTML = recommendations;
        }

        function clearForm() {
            document.getElementById('taskName').value = '';
            currentSubtasks = [];
            displaySubtasks();
            
            document.querySelectorAll('.priority-btn').forEach(btn => {
                btn.classList.remove('active', 'important', 'not-important', 'urgent', 'not-urgent');
            });
            
            selectedImportance = null;
            selectedUrgency = null;
        }

        function resetAll() {
            if (confirm('คุณต้องการล้างข้อมูลทั้งหมดใช่หรือไม่?')) {
                tasks = [];
                currentSubtasks = [];
                document.getElementById('username').value = '';
                document.getElementById('month').value = '';
                clearForm();
                document.getElementById('matrixContainer').style.display = 'none';
            }
        }
    </script>
</body>
</html>
