<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบแจ้งเตือนการบ้าน - วิทยาลัยการอาชีพอ่าวลึก</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Kanit:wght@400;500;600&display=swap');
        body { font-family: 'Kanit', sans-serif; }
        .fade-in { animation: fadeIn 0.3s ease-in; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    </style>
</head>
<body class="bg-gradient-to-br from-blue-50 to-purple-50 min-h-screen">
    <!-- Login Screen -->
    <div id="loginScreen" class="min-h-screen flex items-center justify-center px-4">
        <div class="bg-white rounded-xl shadow-lg p-6 w-full max-w-md">
            <div class="text-center mb-6">
                <h1 class="text-2xl font-bold text-gray-800">ระบบแจ้งเตือนการบ้าน</h1>
                <p class="text-gray-600">วิทยาลัยการอาชีพอ่าวลึก</p>
            </div>
            
            <form id="loginForm" class="space-y-4">
                <select id="userType" class="w-full px-4 py-2 border rounded-lg">
                    <option value="student">นักเรียน</option>
                    <option value="teacher">ครู</option>
                </select>
                
                <input type="text" id="userName" required placeholder="ชื่อ-นามสกุล"
                       class="w-full px-4 py-2 border rounded-lg">
                
                <input type="password" id="userPassword" required placeholder="รหัสผ่าน"
                       class="w-full px-4 py-2 border rounded-lg">
                
                <button type="submit" class="w-full bg-blue-500 text-white py-2 rounded-lg hover:bg-blue-600">
                    เข้าสู่ระบบ
                </button>
            </form>
            
            <div class="mt-4 p-3 bg-green-50 rounded-lg text-center">
                <p class="text-xs text-green-700">
                    🔔 <strong>แจ้งเตือนแบบเรียลไทม์</strong><br>
                    ระบบจะแจ้งเตือนทุกเครื่องที่เปิดแอปนี้<br>
                    เมื่อมีการบ้านใหม่หรือมีการส่งงาน
                </p>
            </div>

        </div>
    </div>

    <!-- Main App -->
    <div id="mainApp" class="hidden container mx-auto px-4 py-6 max-w-4xl">
        <!-- Header -->
        <div class="bg-white rounded-xl shadow-lg p-4 mb-6 flex justify-between items-center">
            <div>
                <h1 class="text-xl font-bold text-gray-800">ระบบแจ้งเตือนการบ้าน - วิทยาลัยการอาชีพอ่าวลึก</h1>
                <p class="text-sm text-gray-600" id="welcomeText">ยินดีต้อนรับ</p>
            </div>
            <button onclick="logout()" class="bg-red-500 text-white px-4 py-2 rounded-lg hover:bg-red-600">
                ออกจากระบบ
            </button>
        </div>

        <!-- Teacher Panel -->
        <div id="teacherPanel" class="hidden bg-white rounded-xl shadow-lg p-6 mb-6">
            <h2 class="text-lg font-semibold mb-4">มอบหมายการบ้าน</h2>
            <form id="homeworkForm" class="space-y-4">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <input type="text" id="subject" required placeholder="ชื่อวิชา"
                           class="px-4 py-2 border rounded-lg">
                    <input type="datetime-local" id="dueDate" required 
                           class="px-4 py-2 border rounded-lg">
                </div>
                <textarea id="description" required placeholder="รายละเอียดการบ้าน" rows="2"
                          class="w-full px-4 py-2 border rounded-lg"></textarea>
                <button type="submit" class="w-full bg-green-500 text-white py-2 rounded-lg hover:bg-green-600">
                    มอบหมายการบ้าน
                </button>
            </form>
        </div>

        <!-- Teacher Stats -->
        <div id="teacherStats" class="hidden bg-white rounded-xl shadow-lg p-6 mb-6">
            <h2 class="text-lg font-semibold mb-4">สถิติการส่งงาน</h2>
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 text-center">
                <div class="bg-blue-50 p-3 rounded-lg">
                    <div class="text-xl font-bold text-blue-600" id="teacherTotalCount">0</div>
                    <div class="text-xs text-gray-600">การบ้านทั้งหมด</div>
                </div>
                <div class="bg-green-50 p-3 rounded-lg">
                    <div class="text-xl font-bold text-green-600" id="teacherSubmittedCount">0</div>
                    <div class="text-xs text-gray-600">ส่งแล้ว</div>
                </div>
                <div class="bg-yellow-50 p-3 rounded-lg">
                    <div class="text-xl font-bold text-yellow-600" id="teacherPendingCount">0</div>
                    <div class="text-xs text-gray-600">รอส่ง</div>
                </div>
                <div class="bg-red-50 p-3 rounded-lg">
                    <div class="text-xl font-bold text-red-600" id="teacherOverdueCount">0</div>
                    <div class="text-xs text-gray-600">เลยกำหนด</div>
                </div>
            </div>
        </div>

        <!-- Student Stats -->
        <div id="studentStats" class="hidden bg-white rounded-xl shadow-lg p-6 mb-6">
            <h2 class="text-lg font-semibold mb-4">สถานะการบ้าน</h2>
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 text-center">
                <div class="bg-blue-50 p-3 rounded-lg">
                    <div class="text-xl font-bold text-blue-600" id="totalCount">0</div>
                    <div class="text-xs text-gray-600">ทั้งหมด</div>
                </div>
                <div class="bg-green-50 p-3 rounded-lg">
                    <div class="text-xl font-bold text-green-600" id="submittedCount">0</div>
                    <div class="text-xs text-gray-600">ส่งแล้ว</div>
                </div>
                <div class="bg-yellow-50 p-3 rounded-lg">
                    <div class="text-xl font-bold text-yellow-600" id="pendingCount">0</div>
                    <div class="text-xs text-gray-600">รอส่ง</div>
                </div>
                <div class="bg-red-50 p-3 rounded-lg">
                    <div class="text-xl font-bold text-red-600" id="overdueCount">0</div>
                    <div class="text-xs text-gray-600">เลยกำหนด</div>
                </div>
            </div>
        </div>

        <!-- Homework List -->
        <div class="bg-white rounded-xl shadow-lg p-6">
            <h2 class="text-lg font-semibold mb-4" id="listTitle">รายการการบ้าน</h2>
            <div id="homeworkList" class="space-y-4">
                <!-- Homework items will be added here -->
            </div>
            <div id="emptyState" class="text-center py-8 hidden">
                <p class="text-gray-500">ยังไม่มีการบ้าน</p>
            </div>
        </div>
    </div>

    <!-- Notification -->
    <div id="notification" class="fixed top-4 right-4 bg-green-500 text-white px-4 py-2 rounded-lg shadow-lg transform translate-x-full transition-transform">
        <span id="notificationText">แจ้งเตือน</span>
    </div>

    <script>
        let currentUser = null;
        let homeworks = JSON.parse(localStorage.getItem('homeworks')) || [];
        let submissions = JSON.parse(localStorage.getItem('submissions')) || [];
        let lastNotificationCheck = Date.now();

        // Start with completely empty system - no sample data

        // Enhanced real-time sync system for all devices
        function startRealTimeSync() {
            // Check for updates every 500ms for faster real-time experience
            setInterval(() => {
                syncAllData();
                checkForBroadcastMessages();
            }, 500);
            
            // Also listen for storage events (when other tabs make changes)
            window.addEventListener('storage', function(e) {
                if (e.key === 'homeworks' || e.key === 'submissions' || e.key === 'broadcastMessage') {
                    syncAllData();
                    if (e.key === 'broadcastMessage') {
                        checkForBroadcastMessages();
                    }
                }
            });
        }

        function syncAllData() {
            if (!currentUser) return;
            
            // Get current data from localStorage
            const currentHomeworks = JSON.parse(localStorage.getItem('homeworks')) || [];
            const currentSubmissions = JSON.parse(localStorage.getItem('submissions')) || [];
            
            // Check if homework data has changed
            if (JSON.stringify(currentHomeworks) !== JSON.stringify(homeworks)) {
                const newHomeworks = currentHomeworks.filter(hw => {
                    return !homeworks.some(existingHw => existingHw.id === hw.id);
                });
                
                // Show notification for new homework (for students)
                if (currentUser.type === 'student' && newHomeworks.length > 0) {
                    newHomeworks.forEach(hw => {
                        showNotification(`📚 การบ้านใหม่: ${hw.subject} จากครู${hw.teacherName}`);
                        showBrowserNotification(`การบ้านใหม่!`, `${hw.subject} - กำหนดส่ง: ${formatDate(new Date(hw.dueDate))}`, '📚');
                        
                        // Play notification sound
                        playNotificationSound();
                    });
                }
                
                homeworks = currentHomeworks;
                renderHomeworks();
                updateStats();
            }
            
            // Check if submission data has changed
            if (JSON.stringify(currentSubmissions) !== JSON.stringify(submissions)) {
                const newSubmissions = currentSubmissions.filter(sub => {
                    return !submissions.some(existingSub => existingSub.id === sub.id);
                });
                
                // Show notification for new submissions (for teachers)
                if (currentUser.type === 'teacher' && newSubmissions.length > 0) {
                    newSubmissions.forEach(sub => {
                        const homework = homeworks.find(hw => hw.id === sub.homeworkId);
                        if (homework && homework.teacherName === currentUser.name) {
                            showNotification(`✅ ${sub.studentName} ส่งงาน ${homework.subject} แล้ว`);
                            showBrowserNotification(`มีงานส่งใหม่!`, `${sub.studentName} ส่งงาน ${homework.subject}`, '✅');
                            
                            // Play notification sound
                            playNotificationSound();
                        }
                    });
                }
                
                submissions = currentSubmissions;
                renderHomeworks();
                updateStats();
            }
        }

        // Broadcast system for cross-device notifications
        function broadcastToAllDevices(message, type, data) {
            const broadcastData = {
                id: Date.now() + Math.random(),
                message: message,
                type: type,
                data: data,
                timestamp: Date.now(),
                sender: currentUser ? currentUser.name : 'System'
            };
            
            localStorage.setItem('broadcastMessage', JSON.stringify(broadcastData));
            
            // Clear after 2 seconds to prevent repeated notifications
            setTimeout(() => {
                const current = JSON.parse(localStorage.getItem('broadcastMessage') || '{}');
                if (current.id === broadcastData.id) {
                    localStorage.removeItem('broadcastMessage');
                }
            }, 2000);
        }

        function checkForBroadcastMessages() {
            const broadcastData = JSON.parse(localStorage.getItem('broadcastMessage') || '{}');
            
            if (broadcastData.id && broadcastData.timestamp) {
                const messageAge = Date.now() - broadcastData.timestamp;
                
                // Only show messages that are less than 3 seconds old and not from current user
                if (messageAge < 3000 && broadcastData.sender !== (currentUser ? currentUser.name : '')) {
                    if (broadcastData.type === 'homework' && currentUser && currentUser.type === 'student') {
                        showNotification(`📚 ${broadcastData.message}`);
                        showBrowserNotification('การบ้านใหม่!', broadcastData.message, '📚');
                        playNotificationSound();
                    } else if (broadcastData.type === 'submission' && currentUser && currentUser.type === 'teacher') {
                        showNotification(`✅ ${broadcastData.message}`);
                        showBrowserNotification('มีงานส่งใหม่!', broadcastData.message, '✅');
                        playNotificationSound();
                    }
                }
            }
        }

        // Enhanced browser notification system
        function showBrowserNotification(title, body, icon) {
            if (Notification.permission === 'granted') {
                const notification = new Notification(title, {
                    body: body,
                    icon: `data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><text y=".9em" font-size="90">${icon}</text></svg>`,
                    badge: `data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><text y=".9em" font-size="90">🔔</text></svg>`,
                    tag: 'homework-notification',
                    requireInteraction: false,
                    silent: false
                });
                
                // Auto close after 5 seconds
                setTimeout(() => notification.close(), 5000);
                
                // Focus window when notification is clicked
                notification.onclick = function() {
                    window.focus();
                    notification.close();
                };
            }
        }

        // Notification sound (using Web Audio API)
        function playNotificationSound() {
            try {
                const audioContext = new (window.AudioContext || window.webkitAudioContext)();
                const oscillator = audioContext.createOscillator();
                const gainNode = audioContext.createGain();
                
                oscillator.connect(gainNode);
                gainNode.connect(audioContext.destination);
                
                oscillator.frequency.setValueAtTime(800, audioContext.currentTime);
                oscillator.frequency.setValueAtTime(600, audioContext.currentTime + 0.1);
                
                gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
                gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.3);
                
                oscillator.start(audioContext.currentTime);
                oscillator.stop(audioContext.currentTime + 0.3);
            } catch (e) {
                // Fallback: no sound if Web Audio API is not supported
                console.log('Audio notification not supported');
            }
        }

        // Enhanced notification permission system
        function requestNotificationPermission() {
            if ('Notification' in window) {
                if (Notification.permission === 'default') {
                    Notification.requestPermission().then(function(permission) {
                        if (permission === 'granted') {
                            showNotification('🔔 เปิดการแจ้งเตือนแล้ว! คุณจะได้รับแจ้งเตือนทุกครั้งที่มีการบ้านใหม่');
                            
                            // Show a test notification
                            setTimeout(() => {
                                showBrowserNotification('ระบบพร้อมใช้งาน!', 'คุณจะได้รับการแจ้งเตือนแบบเรียลไทม์', '✅');
                            }, 1000);
                        } else {
                            showNotification('⚠️ การแจ้งเตือนถูกปิด - คุณอาจพลาดการบ้านใหม่');
                        }
                    });
                } else if (Notification.permission === 'granted') {
                    showNotification('🔔 การแจ้งเตือนพร้อมใช้งาน!');
                }
            }
        }

        // Auto-request permission for all users
        function autoRequestPermission() {
            setTimeout(() => {
                if ('Notification' in window && Notification.permission === 'default') {
                    const shouldRequest = confirm('ต้องการเปิดการแจ้งเตือนเพื่อรับข่าวสารการบ้านแบบเรียลไทม์หรือไม่?');
                    if (shouldRequest) {
                        requestNotificationPermission();
                    }
                }
            }, 2000);
        }

        // Login
        document.getElementById('loginForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const userType = document.getElementById('userType').value;
            const userName = document.getElementById('userName').value;
            const userPassword = document.getElementById('userPassword').value;
            
            // Simple password validation (in real app, use proper authentication)
            if (userPassword.length < 4) {
                showNotification('รหัสผ่านต้องมีอย่างน้อย 4 ตัวอักษร');
                return;
            }
            
            currentUser = {
                type: userType,
                name: userName,
                password: userPassword
            };
            
            // Register student names for teacher tracking
            if (currentUser.type === 'student') {
                let studentRegistry = JSON.parse(localStorage.getItem('studentRegistry')) || [];
                if (!studentRegistry.includes(currentUser.name)) {
                    studentRegistry.push(currentUser.name);
                    localStorage.setItem('studentRegistry', JSON.stringify(studentRegistry));
                }
            }
            
            showMainApp();
            showNotification(`ยินดีต้อนรับ ${currentUser.name} (${currentUser.type === 'teacher' ? 'ครู' : 'นักเรียน'})!`);
        });

        // Add homework (teacher)
        document.getElementById('homeworkForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const homework = {
                id: Date.now(),
                subject: document.getElementById('subject').value,
                description: document.getElementById('description').value,
                dueDate: document.getElementById('dueDate').value,
                teacherName: currentUser.name
            };
            homeworks.push(homework);
            localStorage.setItem('homeworks', JSON.stringify(homeworks));
            
            // Broadcast to all devices
            broadcastToAllDevices(
                `การบ้านใหม่: ${homework.subject} จากครู${homework.teacherName}`,
                'homework',
                homework
            );
            
            renderHomeworks();
            updateStats();
            this.reset();
            showNotification(`✅ มอบหมายการบ้าน "${homework.subject}" แล้ว! ส่งแจ้งเตือนไปยังนักเรียนทุกเครื่องแล้ว 📢`);
        });

        function showMainApp() {
            document.getElementById('loginScreen').classList.add('hidden');
            document.getElementById('mainApp').classList.remove('hidden');
            document.getElementById('welcomeText').textContent = 
                `ยินดีต้อนรับ ${currentUser.name} (${currentUser.type === 'teacher' ? 'ครู' : 'นักเรียน'})`;
            
            if (currentUser.type === 'teacher') {
                document.getElementById('teacherPanel').classList.remove('hidden');
                document.getElementById('teacherStats').classList.remove('hidden');
                document.getElementById('listTitle').textContent = 'การบ้านที่มอบหมาย';
                
                // Request notification permission for teachers too
                requestNotificationPermission();
            } else {
                document.getElementById('studentStats').classList.remove('hidden');
                document.getElementById('listTitle').textContent = 'การบ้านของฉัน';
                
                // Request notification permission for students
                requestNotificationPermission();
            }
            
            // Auto-request permission after a delay
            autoRequestPermission();
            
            renderHomeworks();
            updateStats();
            
            // Start real-time sync for all users
            startRealTimeSync();
        }

        function logout() {
            currentUser = null;
            document.getElementById('mainApp').classList.add('hidden');
            document.getElementById('loginScreen').classList.remove('hidden');
            document.getElementById('teacherPanel').classList.add('hidden');
            document.getElementById('teacherStats').classList.add('hidden');
            document.getElementById('studentStats').classList.add('hidden');
            document.getElementById('loginForm').reset();
        }

        function renderHomeworks() {
            const container = document.getElementById('homeworkList');
            const emptyState = document.getElementById('emptyState');
            
            if (homeworks.length === 0) {
                container.innerHTML = '';
                emptyState.classList.remove('hidden');
                return;
            }
            
            emptyState.classList.add('hidden');
            
            container.innerHTML = homeworks.map(homework => {
                const dueDate = new Date(homework.dueDate);
                const isOverdue = dueDate < new Date();
                const submitted = isSubmitted(homework.id);
                
                let statusClass = 'border-l-yellow-400';
                let statusText = 'รอส่ง';
                
                if (submitted) {
                    statusClass = 'border-l-green-400';
                    statusText = 'ส่งแล้ว';
                } else if (isOverdue) {
                    statusClass = 'border-l-red-400';
                    statusText = 'เลยกำหนด';
                }
                
                return `
                    <div class="border-l-4 ${statusClass} bg-gray-50 p-4 rounded-r-lg fade-in">
                        <div class="flex justify-between items-start">
                            <div class="flex-1">
                                <div class="flex items-center justify-between mb-2">
                                    <h3 class="font-semibold text-gray-800">${homework.subject}</h3>
                                    <span class="text-sm ${submitted ? 'text-green-600' : isOverdue ? 'text-red-600' : 'text-yellow-600'}">
                                        ${statusText}
                                    </span>
                                </div>
                                <p class="text-gray-600 text-sm mb-2">${homework.description}</p>
                                <div class="flex items-center justify-between text-xs text-gray-500">
                                    <span>กำหนดส่ง: ${formatDate(dueDate)}</span>
                                    <span>ครู: ${homework.teacherName}</span>
                                </div>
                            </div>
                            <div class="ml-4 flex space-x-2">
                                ${currentUser.type === 'student' && !submitted ? `
                                    <button onclick="submitHomework(${homework.id})" 
                                            class="bg-green-500 text-white px-3 py-1 rounded text-sm hover:bg-green-600">
                                        ส่งงาน
                                    </button>
                                ` : ''}
                                ${currentUser.type === 'teacher' ? `
                                    <button onclick="viewSubmissions(${homework.id})" 
                                            class="bg-blue-500 text-white px-3 py-1 rounded text-sm hover:bg-blue-600 mr-2">
                                        ดูสถานะ
                                    </button>
                                    <button onclick="deleteHomework(${homework.id})" 
                                            class="bg-red-500 text-white px-3 py-1 rounded text-sm hover:bg-red-600">
                                        ลบ
                                    </button>
                                ` : ''}
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
        }

        function submitHomework(homeworkId) {
            const homework = homeworks.find(hw => hw.id === homeworkId);
            if (!homework) return;
            
            const submission = {
                id: Date.now(),
                homeworkId: homeworkId,
                studentName: currentUser.name,
                submittedAt: new Date().toISOString()
            };
            submissions.push(submission);
            localStorage.setItem('submissions', JSON.stringify(submissions));
            
            // Broadcast to all devices (especially teachers)
            broadcastToAllDevices(
                `${currentUser.name} ส่งงาน ${homework.subject} แล้ว`,
                'submission',
                submission
            );
            
            renderHomeworks();
            updateStats();
            showNotification(`✅ ส่งงาน "${homework.subject}" เรียบร้อยแล้ว! แจ้งครูแล้ว 📤`);
        }

        function deleteHomework(id) {
            if (confirm('คุณแน่ใจหรือไม่ที่จะลบการบ้านนี้?')) {
                homeworks = homeworks.filter(hw => hw.id !== id);
                localStorage.setItem('homeworks', JSON.stringify(homeworks));
                
                // Trigger update for all connected devices
                localStorage.setItem('lastDataUpdate', Date.now().toString());
                
                renderHomeworks();
                updateStats();
                showNotification(`ครู ${currentUser.name}: ลบการบ้านแล้ว`);
            }
        }

        function updateStats() {
            if (currentUser && currentUser.type === 'student') {
                const total = homeworks.length;
                const submitted = homeworks.filter(hw => isSubmitted(hw.id)).length;
                const pending = homeworks.filter(hw => !isSubmitted(hw.id) && new Date(hw.dueDate) >= new Date()).length;
                const overdue = homeworks.filter(hw => !isSubmitted(hw.id) && new Date(hw.dueDate) < new Date()).length;
                
                document.getElementById('totalCount').textContent = total;
                document.getElementById('submittedCount').textContent = submitted;
                document.getElementById('pendingCount').textContent = pending;
                document.getElementById('overdueCount').textContent = overdue;
            } else if (currentUser && currentUser.type === 'teacher') {
                const teacherHomeworks = homeworks.filter(hw => hw.teacherName === currentUser.name);
                const total = teacherHomeworks.length;
                const submitted = submissions.length;
                const pending = teacherHomeworks.filter(hw => new Date(hw.dueDate) >= new Date()).length - submitted;
                const overdue = teacherHomeworks.filter(hw => new Date(hw.dueDate) < new Date()).length;
                
                document.getElementById('teacherTotalCount').textContent = total;
                document.getElementById('teacherSubmittedCount').textContent = submitted;
                document.getElementById('teacherPendingCount').textContent = Math.max(0, pending);
                document.getElementById('teacherOverdueCount').textContent = overdue;
            }
        }

        function viewSubmissions(homeworkId) {
            const homework = homeworks.find(hw => hw.id === homeworkId);
            if (!homework) return;
            
            const homeworkSubmissions = submissions.filter(sub => sub.homeworkId === homeworkId);
            const submittedStudents = homeworkSubmissions.map(sub => sub.studentName);
            
            // Get all students who have ever logged in (from localStorage or create a student registry)
            let allStudents = JSON.parse(localStorage.getItem('studentRegistry')) || [];
            const notSubmittedStudents = allStudents.filter(student => !submittedStudents.includes(student));
            
            let message = `การบ้าน: ${homework.subject}\n`;
            message += `กำหนดส่ง: ${formatDate(new Date(homework.dueDate))}\n\n`;
            
            if (submittedStudents.length > 0) {
                message += `✅ ส่งแล้ว (${submittedStudents.length} คน):\n`;
                submittedStudents.forEach(student => {
                    const submission = homeworkSubmissions.find(sub => sub.studentName === student);
                    const submitDate = new Date(submission.submittedAt);
                    message += `• ${student} - ${formatDate(submitDate)}\n`;
                });
                message += '\n';
            }
            
            if (notSubmittedStudents.length > 0) {
                message += `❌ ยังไม่ส่ง (${notSubmittedStudents.length} คน):\n`;
                notSubmittedStudents.forEach(student => {
                    message += `• ${student}\n`;
                });
            } else if (allStudents.length > 0 && submittedStudents.length > 0) {
                message += '🎉 นักเรียนทุกคนส่งงานแล้ว!';
            } else if (allStudents.length === 0) {
                message += 'ยังไม่มีนักเรียนเข้าใช้ระบบ';
            }
            
            alert(message);
        }

        function isSubmitted(homeworkId) {
            return submissions.some(sub => sub.homeworkId === homeworkId && sub.studentName === currentUser.name);
        }

        function showNotification(message) {
            const notification = document.getElementById('notification');
            const notificationText = document.getElementById('notificationText');
            
            notificationText.textContent = message;
            notification.classList.remove('translate-x-full');
            
            setTimeout(() => {
                notification.classList.add('translate-x-full');
            }, 3000);
        }

        function formatDate(date) {
            return date.toLocaleDateString('th-TH', {
                month: 'short',
                day: 'numeric',
                hour: '2-digit',
                minute: '2-digit'
            });
        }

        // Set minimum date to today
        document.addEventListener('DOMContentLoaded', function() {
            const now = new Date();
            now.setMinutes(now.getMinutes() - now.getTimezoneOffset());
            const dateInput = document.getElementById('dueDate');
            if (dateInput) {
                dateInput.min = now.toISOString().slice(0, 16);
            }
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'97a3edfc0471893b',t:'MTc1NzA1NjQ2NS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
