<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>EDUPULSE | Smart Attendance</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    *{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',sans-serif}
    body{background:linear-gradient(135deg,#1a2a6c,#b21f1f,#fdbb2d);min-height:100vh;display:flex;justify-content:center;align-items:center;padding:20px}
    .container{width:100%;max-width:1200px;display:flex;flex-direction:column;gap:30px}
    header{text-align:center;color:#fff;padding:20px;background:rgba(0,0,0,.3);border-radius:20px;backdrop-filter:blur(10px)}
    h1{font-size:2.5rem;margin-bottom:8px}
    .subtitle{font-size:1.1rem}
    .role-selection{display:flex;justify-content:center;gap:20px;flex-wrap:wrap}
    .role-card{width:280px;background:rgba(255,255,255,.9);border-radius:20px;padding:25px;text-align:center;box-shadow:0 10px 25px rgba(0,0,0,.2);cursor:pointer;transition:.3s}
    .role-card:hover{transform:translateY(-8px)}
    .role-icon{font-size:3rem;margin-bottom:15px}
    .teacher-card .role-icon{color:#1a2a6c}.student-card .role-icon{color:#b21f1f}.parent-card .role-icon{color:#fdbb2d}
    .role-card h2{color:#1a2a6c;margin-bottom:10px}
    .login-btn{padding:12px;border:none;border-radius:50px;font-size:1rem;font-weight:600;width:100%;cursor:pointer}
    .teacher-card .login-btn{background:linear-gradient(to right,#1a2a6c,#243b8a);color:#fff}
    .student-card .login-btn{background:linear-gradient(to right,#b21f1f,#c42c2c);color:#fff}
    .parent-card .login-btn{background:linear-gradient(to right,#fdbb2d,#f9a825);color:#fff}
    .login-page,.dashboard{display:none;text-align:center}
    .login-card{background:#fff;border-radius:20px;padding:25px;max-width:450px;margin:auto;box-shadow:0 10px 25px rgba(0,0,0,.2)}
    .login-header h2{margin-bottom:10px;color:#1a2a6c}
    .form-group{margin-bottom:18px;text-align:left}
    .form-group label{font-weight:600;color:#1a2a6c}
    .form-group input{width:100%;padding:12px;border:2px solid #ddd;border-radius:10px;font-size:1rem}
    .error-message{color:#b21f1f;font-size:.85rem;display:none;margin-top:5px}
    .success-message{color:#4caf50;font-size:.85rem;display:none;margin-top:5px}
    .submit-btn{background:linear-gradient(to right,#1a2a6c,#b21f1f);color:#fff;padding:12px;border:none;border-radius:50px;font-weight:600;width:100%;cursor:pointer;transition:.3s}
    .submit-btn:hover{transform:translateY(-3px)}
    .back-to-dashboard{display:inline-block;margin-top:15px;color:#1a2a6c;font-weight:600;cursor:pointer}
    .dashboard-header{display:flex;justify-content:space-between;align-items:center;border-bottom:2px solid #eee;padding-bottom:10px;margin-bottom:20px}
    .user-info{display:flex;align-items:center;gap:10px}
    .user-avatar{width:40px;height:40px;border-radius:50%;background:linear-gradient(to right,#1a2a6c,#b21f1f);color:#fff;display:flex;align-items:center;justify-content:center;font-weight:bold}
    .logout-btn{background:#b21f1f;color:#fff;border:none;padding:6px 12px;border-radius:20px;cursor:pointer}
    .dashboard-content{display:grid;grid-template-columns:repeat(auto-fit,minmax(250px,1fr));gap:20px}
    .feature-card{background:#fff;border-radius:15px;padding:20px;box-shadow:0 5px 15px rgba(0,0,0,.1)}
    .attendance-system,.curriculum-system,.performance-system{display:none;margin-top:20px;background:#fff;border-radius:15px;padding:20px}
    table{width:100%;border-collapse:collapse;margin-top:15px}
    th,td{border-bottom:1px solid #ddd;padding:10px;text-align:left}
    th{background:#1a2a6c;color:#fff}
    .status-badge{padding:5px 10px;border-radius:12px;font-size:.85rem;font-weight:600}
    .present{background:#4caf50;color:#fff}.absent{background:#f44336;color:#fff}
    .password-section{margin-top:20px;text-align:left}
    .action-btn{padding:4px 8px;border:none;border-radius:6px;cursor:pointer;font-size:.85rem;margin-right:5px}
    .reset-btn{background:#2196f3;color:#fff}
    .delete-btn{background:#f44336;color:#fff}
    footer{text-align:center;color:#fff;margin-top:20px;font-size:.9rem}
    
    /* New styles for curriculum and performance */
    .form-row {display: flex; gap: 10px; margin-bottom: 15px; align-items: end;}
    .form-row .form-group {flex: 1; margin-bottom: 0;}
    .chart-container {position: relative; height: 300px; margin-top: 20px;}
    .stats-grid {display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; margin: 20px 0;}
    .stat-card {background: #f5f7ff; padding: 15px; border-radius: 10px; text-align: center;}
    .stat-value {font-size: 24px; font-weight: bold; color: #1a2a6c;}
    .stat-label {font-size: 14px; color: #666;}
    .tabs {display: flex; border-bottom: 2px solid #eee; margin-bottom: 20px;}
    .tab {padding: 10px 20px; cursor: pointer; border-bottom: 3px solid transparent;}
    .tab.active {border-color: #1a2a6c; font-weight: bold;}
    .module-item {background: #f9f9f9; padding: 12px; border-radius: 8px; margin-bottom: 10px; display: flex; justify-content: space-between;}
    .module-actions {display: flex; gap: 10px;}
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1>EDUPULSE</h1>
      <p class="subtitle">Smart attendance and curriculum tracker</p>
    </header>

    <!-- Role Selection -->
    <div class="role-selection" id="roleSelection">
      <div class="role-card teacher-card"><div class="role-icon"><i class="fas fa-chalkboard-teacher"></i></div><h2>Teacher Portal</h2><p>Track attendance, manage curriculum.</p><button class="login-btn" data-role="teacher">Login as Teacher</button></div>
      <div class="role-card student-card"><div class="role-icon"><i class="fas fa-user-graduate"></i></div><h2>Student Portal</h2><p>View your attendance & progress.</p><button class="login-btn" data-role="student">Login as Student</button></div>
      <div class="role-card parent-card"><div class="role-icon"><i class="fas fa-users"></i></div><h2>Parent Portal</h2><p>Monitor your child's records.</p><button class="login-btn" data-role="parent">Login as Parent</button></div>
    </div>

    <!-- Teacher Login -->
    <div class="login-page" id="teacherLogin">
      <div class="login-card">
        <div class="login-header"><h2>Teacher Login</h2></div>
        <div class="form-group"><label>Email</label><input type="email" id="teacherEmail"></div>
        <div class="form-group"><label>Password</label><input type="password" id="teacherPassword"></div>
        <div class="error-message" id="teacherError">Invalid credentials</div>
        <button class="submit-btn" id="teacherLoginBtn">Login</button>
        <a class="back-to-dashboard" data-back>← Back</a>
      </div>
    </div>

    <!-- Student Login -->
    <div class="login-page" id="studentLogin">
      <div class="login-card">
        <div class="login-header"><h2>Student Login</h2></div>
        <div class="form-group"><label>Student ID</label><input type="text" id="studentIdLogin"></div>
        <div class="form-group"><label>Password</label><input type="password" id="studentPassword"></div>
        <div class="error-message" id="studentIdError">Invalid ID</div>
        <div class="error-message" id="studentPasswordError">Invalid Password</div>
        <button class="submit-btn" id="studentLoginBtn">Login</button>
        <a class="back-to-dashboard" data-back>← Back</a>
      </div>
    </div>

    <!-- Parent Login -->
    <div class="login-page" id="parentLogin">
      <div class="login-card">
        <div class="login-header"><h2>Parent Login</h2></div>
        <div class="form-group"><label>Child Student ID</label><input type="text" id="parentChildIdLogin"></div>
        <div class="form-group"><label>Password</label><input type="password" id="parentPassword"></div>
        <div class="error-message" id="parentChildIdError">Invalid ID</div>
        <div class="error-message" id="parentPasswordError">Invalid Password</div>
        <button class="submit-btn" id="parentLoginBtn">Login</button>
        <a class="back-to-dashboard" data-back>← Back</a>
      </div>
    </div>

    <!-- Teacher Dashboard -->
    <div class="dashboard" id="teacherDashboard">
      <div class="dashboard-header">
        <h2>Teacher Dashboard</h2>
        <div class="user-info"><div class="user-avatar">T</div><span>Teacher</span><button class="logout-btn" data-logout>Logout</button></div>
      </div>
      
      <div class="dashboard-content">
        <div class="feature-card"><h3>Attendance</h3><button id="openAttendance">Manage Attendance</button></div>
        <div class="feature-card"><h3>Curriculum</h3><button id="openCurriculum">Manage Curriculum</button></div>
        <div class="feature-card"><h3>Performance</h3><button id="openPerformance">View Analysis</button></div>
      </div>
      
      <div class="attendance-system" id="attendanceSystem">
        <h3>Attendance Management</h3>
        <div class="form-row">
          <div class="form-group"><input type="text" id="studentName" placeholder="Name"></div>
          <div class="form-group"><input type="text" id="studentId" placeholder="ID"></div>
          <div class="form-group"><select id="attendanceStatus"><option value="present">Present</option><option value="absent">Absent</option></select></div>
          <button class="submit-btn" id="saveStudentBtn" style="padding: 10px; width: auto;">Save</button>
        </div>
        <table><thead><tr><th>ID</th><th>Name</th><th>Status</th><th>Date</th><th>Actions</th></tr></thead><tbody id="attendanceList"></tbody></table>
      </div>
      
      <div class="curriculum-system" id="curriculumSystem">
        <h3>Curriculum Management</h3>
        <div class="tabs">
          <div class="tab active" data-tab="modules">Modules</div>
          <div class="tab" data-tab="assignments">Assignments</div>
          <div class="tab" data-tab="resources">Resources</div>
        </div>
        
        <div id="modulesTab">
          <div class="form-row">
            <div class="form-group"><label>Module Title</label><input type="text" id="moduleTitle" placeholder="Enter module title"></div>
            <div class="form-group"><label>Due Date</label><input type="date" id="moduleDueDate"></div>
            <button class="submit-btn" id="addModuleBtn" style="padding: 10px; width: auto;">Add Module</button>
          </div>
          <div id="modulesList"></div>
        </div>
        
        <div id="assignmentsTab" style="display: none;">
          <div class="form-row">
            <div class="form-group"><label>Assignment Title</label><input type="text" id="assignmentTitle" placeholder="Enter assignment title"></div>
            <div class="form-group"><label>Max Score</label><input type="number" id="assignmentMaxScore" placeholder="100"></div>
            <button class="submit-btn" id="addAssignmentBtn" style="padding: 10px; width: auto;">Add Assignment</button>
          </div>
          <div id="assignmentsList"></div>
        </div>
        
        <div id="resourcesTab" style="display: none;">
          <div class="form-row">
            <div class="form-group"><label>Resource Title</label><input type="text" id="resourceTitle" placeholder="Enter resource title"></div>
            <div class="form-group"><label>Resource Link</label><input type="url" id="resourceLink" placeholder="https://..."></div>
            <button class="submit-btn" id="addResourceBtn" style="padding: 10px; width: auto;">Add Resource</button>
          </div>
          <div id="resourcesList"></div>
        </div>
      </div>
      
      <div class="performance-system" id="performanceSystem">
        <h3>Performance Analysis</h3>
        
        <div class="stats-grid">
          <div class="stat-card">
            <div class="stat-value" id="totalStudents">0</div>
            <div class="stat-label">Total Students</div>
          </div>
          <div class="stat-card">
            <div class="stat-value" id="attendanceRate">0%</div>
            <div class="stat-label">Overall Attendance</div>
          </div>
          <div class="stat-card">
            <div class="stat-value" id="completionRate">0%</div>
            <div class="stat-label">Curriculum Completion</div>
          </div>
        </div>
        
        <div class="tabs">
          <div class="tab active" data-chart="attendance">Attendance</div>
          <div class="tab" data-chart="performance">Performance</div>
          <div class="tab" data-chart="progress">Progress</div>
        </div>
        
        <div class="chart-container">
          <canvas id="performanceChart"></canvas>
        </div>
      </div>
    </div>

    <!-- Student Dashboard -->
    <div class="dashboard" id="studentDashboard">
      <div class="dashboard-header"><h2>Student Dashboard</h2><div class="user-info"><div class="user-avatar">S</div><span></span><button class="logout-btn" data-logout>Logout</button></div></div>
      <table><thead><tr><th>ID</th><th>Name</th><th>Status</th><th>Date</th></tr></thead><tbody id="studentAttendanceList"></tbody></table>
      <div class="password-section">
        <h3>Change Password</h3>
        <input type="password" id="studentNewPassword" placeholder="New Password">
        <button class="submit-btn" id="studentChangePasswordBtn">Update Password</button>
        <div class="success-message" id="studentPasswordSuccess">Password updated successfully!</div>
      </div>
    </div>

    <!-- Parent Dashboard -->
    <div class="dashboard" id="parentDashboard">
      <div class="dashboard-header"><h2>Parent Dashboard</h2><div class="user-info"><div class="user-avatar">P</div><span></span><button class="logout-btn" data-logout>Logout</button></div></div>
      <table><thead><tr><th>ID</th><th>Name</th><th>Status</th><th>Date</th></tr></thead><tbody id="parentAttendanceList"></tbody></table>
      <div class="password-section">
        <h3>Change Password</h3>
        <input type="password" id="parentNewPassword" placeholder="New Password">
        <button class="submit-btn" id="parentChangePasswordBtn">Update Password</button>
        <div class="success-message" id="parentPasswordSuccess">Password updated successfully!</div>
      </div>
    </div>

    <footer><p>© 2023 EDUPULSE</p></footer>
  </div>

  <script>
    let students = JSON.parse(localStorage.getItem("students")) || [];
    let curriculum = JSON.parse(localStorage.getItem("curriculum")) || {
      modules: [],
      assignments: [],
      resources: []
    };
    let performanceChart = null;

    const roleSelection=document.getElementById("roleSelection"),
          teacherLogin=document.getElementById("teacherLogin"),
          studentLogin=document.getElementById("studentLogin"),
          parentLogin=document.getElementById("parentLogin"),
          teacherDashboard=document.getElementById("teacherDashboard"),
          studentDashboard=document.getElementById("studentDashboard"),
          parentDashboard=document.getElementById("parentDashboard"),
          attendanceSystem=document.getElementById("attendanceSystem"),
          curriculumSystem=document.getElementById("curriculumSystem"),
          performanceSystem=document.getElementById("performanceSystem"),
          attendanceList=document.getElementById("attendanceList");

    function saveStudents(){localStorage.setItem("students",JSON.stringify(students));}
    function saveCurriculum(){localStorage.setItem("curriculum",JSON.stringify(curriculum));}

    function renderAttendanceList(){
      attendanceList.innerHTML="";
      students.forEach((s,i)=>{
        attendanceList.innerHTML+=`<tr>
          <td>${s.id}</td>
          <td>${s.name}</td>
          <td><span class="status-badge ${s.status}">${s.status}</span></td>
          <td>${s.date}</td>
          <td>
            <button class="action-btn reset-btn" onclick="resetPassword(${i})">Reset</button>
            <button class="action-btn delete-btn" onclick="deleteStudent(${i})">Delete</button>
          </td>
        </tr>`;
      });
    }

    function renderCurriculum() {
      // Render modules
      const modulesList = document.getElementById("modulesList");
      modulesList.innerHTML = "";
      curriculum.modules.forEach((module, index) => {
        modulesList.innerHTML += `
          <div class="module-item">
            <div>
              <strong>${module.title}</strong>
              <div>Due: ${module.dueDate}</div>
            </div>
            <div class="module-actions">
              <button class="action-btn reset-btn" onclick="editModule(${index})">Edit</button>
              <button class="action-btn delete-btn" onclick="deleteModule(${index})">Delete</button>
            </div>
          </div>
        `;
      });
      
      // Render assignments
      const assignmentsList = document.getElementById("assignmentsList");
      assignmentsList.innerHTML = "";
      curriculum.assignments.forEach((assignment, index) => {
        assignmentsList.innerHTML += `
          <div class="module-item">
            <div>
              <strong>${assignment.title}</strong>
              <div>Max Score: ${assignment.maxScore}</div>
            </div>
            <div class="module-actions">
              <button class="action-btn reset-btn" onclick="editAssignment(${index})">Edit</button>
              <button class="action-btn delete-btn" onclick="deleteAssignment(${index})">Delete</button>
            </div>
          </div>
        `;
      });
      
      // Render resources
      const resourcesList = document.getElementById("resourcesList");
      resourcesList.innerHTML = "";
      curriculum.resources.forEach((resource, index) => {
        resourcesList.innerHTML += `
          <div class="module-item">
            <div>
              <strong>${resource.title}</strong>
              <div><a href="${resource.link}" target="_blank">View Resource</a></div>
            </div>
            <div class="module-actions">
              <button class="action-btn reset-btn" onclick="editResource(${index})">Edit</button>
              <button class="action-btn delete-btn" onclick="deleteResource(${index})">Delete</button>
            </div>
          </div>
        `;
      });
    }

    function renderPerformanceAnalysis() {
      // Update stats
      document.getElementById("totalStudents").textContent = students.length;
      
      // Calculate attendance rate
      const presentCount = students.filter(s => s.status === "present").length;
      const attendanceRate = students.length > 0 ? Math.round((presentCount / students.length) * 100) : 0;
      document.getElementById("attendanceRate").textContent = `${attendanceRate}%`;
      
      // Calculate completion rate (mock data for demo)
      const completionRate = Math.min(100, Math.round((curriculum.modules.length / 10) * 100));
      document.getElementById("completionRate").textContent = `${completionRate}%`;
      
      // Render attendance chart
      renderAttendanceChart();
    }

    function renderAttendanceChart() {
      const ctx = document.getElementById('performanceChart').getContext('2d');
      
      // Destroy previous chart if it exists
      if (performanceChart) {
        performanceChart.destroy();
      }
      
      // Get active tab
      const activeTab = document.querySelector('.tab.active[data-chart]');
      const chartType = activeTab ? activeTab.getAttribute('data-chart') : 'attendance';
      
      if (chartType === 'attendance') {
        // Attendance chart
        const presentData = students.filter(s => s.status === "present").length;
        const absentData = students.filter(s => s.status === "absent").length;
        const performancechart=students.filter(s => s.status === "bad").length;
        
        performanceChart = new Chart(ctx, {
          type: 'doughnut',
          data: {
            labels: ['Present', 'Absent'],
            datasets: [{
              data: [presentData, absentData],
              backgroundColor: ['#4caf50', '#f44336']
            }]
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
              title: {
                display: true,
                text: 'Attendance Distribution'
              }
            }
          }
        });
      } else if (chartType === 'performance') {
        // Performance chart (mock data for demo)
        performanceChart = new Chart(ctx, {
          type: 'bar',
          data: {
            labels: students.map(s => s.name),
            datasets: [{
              label: 'Test Scores',
              data: students.map(() => Math.floor(Math.random() * 40) + 60), // Random scores between 60-100
              backgroundColor: '#1a2a6c'
            }]
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
              title: {
                display: true,
                text: 'Student Performance'
              }
            },
            scales: {
              y: {
                beginAtZero: true,
                max: 100
              }
            }
          }
        });
      } else if (chartType === 'progress') {
        // Progress chart (mock data for demo)
        performanceChart = new Chart(ctx, {
          type: 'line',
          data: {
            labels: ['Week 1', 'Week 2', 'Week 3', 'Week 4', 'Week 5'],
            datasets: [{
              label: 'Class Progress',
              data: [30, 45, 60, 75, 90],
              borderColor: '#1a2a6c',
              tension: 0.1,
              fill: false
            }]
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
              title: {
                display: true,
                text: 'Curriculum Progress'
              }
            },
            scales: {
              y: {
                beginAtZero: true,
                max: 100
              }
            }
          }
        });
      }
    }

    function renderStudentAttendance(id){
      const tbody=document.getElementById("studentAttendanceList");tbody.innerHTML="";
      students.filter(s=>s.id==id).forEach(s=>{
        tbody.innerHTML+=`<tr><td>${s.id}</td><td>${s.name}</td><td><span class="status-badge ${s.status}">${s.status}</span></td><td>${s.date}</td></tr>`;
      });
    }
    function renderParentAttendance(id){
      const tbody=document.getElementById("parentAttendanceList");tbody.innerHTML="";
      students.filter(s=>s.id==id).forEach(s=>{
        tbody.innerHTML+=`<tr><td>${s.id}</td><td>${s.name}</td><td><span class="status-badge ${s.status}">${s.status}</span></td><td>${s.date}</td></tr>`;
      });
    }

    // Curriculum management functions
    function addModule() {
      const title = document.getElementById("moduleTitle").value.trim();
      const dueDate = document.getElementById("moduleDueDate").value;
      
      if (title && dueDate) {
        curriculum.modules.push({
          title,
          dueDate,
          completed: false
        });
        saveCurriculum();
        renderCurriculum();
        document.getElementById("moduleTitle").value = "";
        document.getElementById("moduleDueDate").value = "";
      } else {
        alert("Please fill all fields");
      }
    }
    
    function deleteModule(index) {
      if (confirm("Are you sure you want to delete this module?")) {
        curriculum.modules.splice(index, 1);
        saveCurriculum();
        renderCurriculum();
      }
    }
    
    function addAssignment() {
      const title = document.getElementById("assignmentTitle").value.trim();
      const maxScore = document.getElementById("assignmentMaxScore").value;
      
      if (title && maxScore) {
        curriculum.assignments.push({
          title,
          maxScore,
          submissions: []
        });
        saveCurriculum();
        renderCurriculum();
        document.getElementById("assignmentTitle").value = "";
        document.getElementById("assignmentMaxScore").value = "";
      } else {
        alert("Please fill all fields");
      }
    }
    
    function deleteAssignment(index) {
      if (confirm("Are you sure you want to delete this assignment?")) {
        curriculum.assignments.splice(index, 1);
        saveCurriculum();
        renderCurriculum();
      }
    }
    
    function addResource() {
      const title = document.getElementById("resourceTitle").value.trim();
      const link = document.getElementById("resourceLink").value;
      
      if (title && link) {
        curriculum.resources.push({
          title,
          link
        });
        saveCurriculum();
        renderCurriculum();
        document.getElementById("resourceTitle").value = "";
        document.getElementById("resourceLink").value = "";
      } else {
        alert("Please fill all fields");
      }
    }
    
    function deleteResource(index) {
      if (confirm("Are you sure you want to delete this resource?")) {
        curriculum.resources.splice(index, 1);
        saveCurriculum();
        renderCurriculum();
      }
    }

    document.getElementById("saveStudentBtn").addEventListener("click",()=>{
      const name=document.getElementById("studentName").value.trim(),
            id=document.getElementById("studentId").value.trim(),
            status=document.getElementById("attendanceStatus").value,
            date=new Date().toISOString().split("T")[0];
      if(name&&id){
        students.push({id,name,status,date,password:"123456"});
        saveStudents();renderAttendanceList();
      }else alert("Fill all fields");
    });

    document.getElementById("openAttendance").addEventListener("click",()=>{
      attendanceSystem.style.display="block";
      curriculumSystem.style.display="none";
      performanceSystem.style.display="none";
      renderAttendanceList();
    });

    document.getElementById("openCurriculum").addEventListener("click",()=>{
      attendanceSystem.style.display="none";
      curriculumSystem.style.display="block";
      performanceSystem.style.display="none";
      renderCurriculum();
    });

    document.getElementById("openPerformance").addEventListener("click",()=>{
      attendanceSystem.style.display="none";
      curriculumSystem.style.display="none";
      performanceSystem.style.display="block";
      renderPerformanceAnalysis();
    });

    document.getElementById("teacherLoginBtn").addEventListener("click",()=>{
      teacherLogin.style.display="none";teacherDashboard.style.display="block";
    });

    document.getElementById("studentLoginBtn").addEventListener("click",()=>{
      const id=document.getElementById("studentIdLogin").value.trim(),
            pass=document.getElementById("studentPassword").value.trim(),
            idError=document.getElementById("studentIdError"),
            pwError=document.getElementById("studentPasswordError");
      idError.style.display=pwError.style.display="none";
      const student=students.find(s=>s.id==id);
      if(!student){idError.style.display="block";return;}
      if(pass!==student.password){pwError.style.display="block";return;}
      studentLogin.style.display="none";studentDashboard.style.display="block";
      renderStudentAttendance(id);
      studentDashboard.querySelector(".user-info span").textContent=student.name;
      studentDashboard.querySelector(".user-avatar").textContent=student.name.charAt(0);
      document.getElementById("studentChangePasswordBtn").onclick=()=>{
        const newPass=document.getElementById("studentNewPassword").value.trim();
        if(newPass.length<4)return alert("Password too short");
        student.password=newPass;saveStudents();
        document.getElementById("studentPasswordSuccess").style.display="block";
        setTimeout(()=>document.getElementById("studentPasswordSuccess").style.display="none",2000);
      }
    });

    document.getElementById("parentLoginBtn").addEventListener("click",()=>{
      const id=document.getElementById("parentChildIdLogin").value.trim(),
            pass=document.getElementById("parentPassword").value.trim(),
            idError=document.getElementById("parentChildIdError"),
            pwError=document.getElementById("parentPasswordError");
      idError.style.display=pwError.style.display="none";
      const child=students.find(s=>s.id==id);
      if(!child){idError.style.display="block";return;}
      if(pass!==child.password){pwError.style.display="block";return;}
      parentLogin.style.display="none";parentDashboard.style.display="block";
      renderParentAttendance(id);
      parentDashboard.querySelector(".user-info span").textContent="Parent of "+child.name;
      parentDashboard.querySelector(".user-avatar").textContent=child.name.charAt(0);
      document.getElementById("parentChangePasswordBtn").onclick=()=>{
        const newPass=document.getElementById("parentNewPassword").value.trim();
        if(newPass.length<4)return alert("Password too short");
        child.password=newPass;saveStudents();
        document.getElementById("parentPasswordSuccess").style.display="block";
        setTimeout(()=>document.getElementById("parentPasswordSuccess").style.display="none",2000);
      }
    });

    // Curriculum event listeners
    document.getElementById("addModuleBtn").addEventListener("click", addModule);
    document.getElementById("addAssignmentBtn").addEventListener("click", addAssignment);
    document.getElementById("addResourceBtn").addEventListener("click", addResource);

    // Tab switching for curriculum
    document.querySelectorAll(".tab[data-tab]").forEach(tab => {
      tab.addEventListener("click", () => {
        // Update active tab
        document.querySelectorAll(".tab[data-tab]").forEach(t => t.classList.remove("active"));
        tab.classList.add("active");
        
        // Show appropriate tab content
        const tabName = tab.getAttribute("data-tab");
        document.getElementById("modulesTab").style.display = tabName === "modules" ? "block" : "none";
        document.getElementById("assignmentsTab").style.display = tabName === "assignments" ? "block" : "none";
        document.getElementById("resourcesTab").style.display = tabName === "resources" ? "block" : "none";
      });
    });

    // Tab switching for performance charts
    document.querySelectorAll(".tab[data-chart]").forEach(tab => {
      tab.addEventListener("click", () => {
        document.querySelectorAll(".tab[data-chart]").forEach(t => t.classList.remove("active"));
        tab.classList.add("active");
        renderPerformanceAnalysis();
      });
    });

    document.querySelector('.login-btn[data-role="teacher"]').addEventListener("click",()=>{roleSelection.style.display="none";teacherLogin.style.display="block";});
    document.querySelector('.login-btn[data-role="student"]').addEventListener("click",()=>{roleSelection.style.display="none";studentLogin.style.display="block";});
    document.querySelector('.login-btn[data-role="parent"]').addEventListener("click",()=>{roleSelection.style.display="none";parentLogin.style.display="block";});
    document.querySelectorAll("[data-back]").forEach(b=>b.addEventListener("click",()=>{teacherLogin.style.display=studentLogin.style.display=parentLogin.style.display="none";roleSelection.style.display="flex";}));
    document.querySelectorAll("[data-logout]").forEach(b=>b.addEventListener("click",()=>{teacherDashboard.style.display=studentDashboard.style.display=parentDashboard.style.display="none";roleSelection.style.display="flex";}));

    // 🔄 Reset Password
    function resetPassword(index){
      students[index].password="123456";
      saveStudents();
      alert("Password reset to default (123456)");
    }

    // ❌ Delete Student
    function deleteStudent(index){
      if(confirm("Are you sure you want to delete this student?")){
        students.splice(index,1);
        saveStudents();renderAttendanceList();
      }
    }

    // Initialize with some sample data if empty
    if (students.length === 0) {
      students = [
        {id: "S001", name: "John Doe", status: "present", date: "2023-10-15", password: "123456"},
        {id: "S002", name: "Jane Smith", status: "absent", date: "2023-10-15", password: "123456"},
        {id: "S003", name: "Robert Johnson", status: "present", date: "2023-10-15", password: "123456"}
      ];
      saveStudents();
    }
    
    if (curriculum.modules.length === 0) {
      curriculum.modules = [
        {title: "Introduction to Mathematics", dueDate: "2023-10-20", completed: false},
        {title: "Algebra Basics", dueDate: "2023-10-27", completed: false}
      ];
      saveCurriculum();
    }
  </script>
</body>
</html>
