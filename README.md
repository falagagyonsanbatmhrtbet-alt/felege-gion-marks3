<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Felege Gion Sunday School Portal</title>
    <style>
        :root { --primary: #1a365d; --secondary: #2b6cb0; --bg: #f7fafc; --text: #2d3748; }
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: var(--bg); color: var(--text); margin: 0; padding: 20px; }
        .container { max-width: 1100px; margin: 0 auto; }
        .card { background: white; padding: 25px; border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.05); margin-bottom: 25px; }
        h2 { margin-top: 0; color: var(--primary); border-bottom: 2px solid #e2e8f0; padding-bottom: 10px; }
        .form-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 5px; font-weight: 600; }
        input[type="text"], input[type="password"], input[type="number"], select { width: 100%; padding: 10px; border: 1px solid #cbd5e0; border-radius: 4px; box-sizing: border-box; }
        button { background: var(--secondary); color: white; border: none; padding: 10px 20px; border-radius: 4px; cursor: pointer; font-weight: bold; }
        button:hover { background: var(--primary); }
        .hidden { display: none !important; }
        .dashboard-grid { display: grid; grid-template-columns: 1fr 2fr; gap: 20px; }
        table { width: 100%; border-collapse: collapse; margin-top: 15px; }
        th, td { text-align: left; padding: 12px; border-bottom: 1px solid #e2e8f0; }
        th { background: #edf2f7; color: var(--primary); }
        .avatar { width: 40px; height: 40px; border-radius: 50%; object-fit: cover; }
        .logout-btn { background: #e53e3e; float: right; }
        .logout-btn:hover { background: #9b2c2c; }
    </style>
</head>
<body>

<div class="container">
    <div id="authScreen" class="card">
        <h2>Felege Gion Admin Login</h2>
        <form id="loginForm">
            <div class="form-group">
                <label>Admin Email</label>
                <input type="text" id="loginEmail" required placeholder="admin@felegegion.com">
            </div>
            <div class="form-group">
                <label>Password</label>
                <input type="password" id="loginPassword" required placeholder="••••••••">
            </div>
            <button type="submit">Access Dashboard</button>
        </form>
    </div>

    <div id="appScreen" class="hidden">
        <div style="overflow: hidden; margin-bottom: 20px;">
            <button class="logout-btn" id="logoutBtn">Log Out</button>
            <h1 style="margin: 0; color: var(--primary);">Felege Gion Sunday School Portal</h1>
        </div>

        <div class="dashboard-grid">
            <div class="card">
                <h2>Register New Student</h2>
                <form id="registrationForm">
                    <div class="form-group">
                        <label>Full Name</label>
                        <input type="text" id="studentName" required>
                    </div>
                    <div class="form-group">
                        <label>Grade (1-10)</label>
                        <select id="studentGrade" required>
                            <option value="">Select Grade</option>
                            </select>
                    </div>
                    <div class="form-group">
                        <label>Student Photo</label>
                        <input type="file" id="studentPhoto" accept="image/*" required>
                    </div>
                    <button type="submit">Generate Record & ID</button>
                </form>
            </div>

            <div class="card">
                <h2>Student Records & Continuous Assessment</h2>
                <div class="form-group">
                    <label>Filter View By Grade</label>
                    <select id="gradeFilter">
                        <option value="All">All Grades</option>
                    </select>
                </div>
                <table>
                    <thead>
                        <tr>
                            <th>Photo</th>
                            <th>System ID</th>
                            <th>Name</th>
                            <th>Grade</th>
                            <th>Academic Mark</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody id="studentTableBody">
                        </tbody>
                </table>
            </div>
        </div>
    </div>
</div>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
    import { getAnalytics } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-analytics.js";
    import { getAuth, signInWithEmailAndPassword, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
    import { getFirestore, collection, addDoc, doc, updateDoc, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";
    import { getStorage, ref, uploadBytes, getDownloadURL } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-storage.js";

    // Your Explicit Live Target Credentials
    const firebaseConfig = {
        apiKey: "AIzaSyB4FdrgFQop1WvubMDhANtUcrHWnQ-kWf4",
        authDomain: "sundayschoolportal-a0931.firebaseapp.com",
        projectId: "sundayschoolportal-a0931",
        storageBucket: "sundayschoolportal-a0931.firebasestorage.app",
        messagingSenderId: "964240028485",
        appId: "1:964240028485:web:59fe14c660638d84dbc42c",
        measurementId: "G-XK9Y8BMNBV"
    };

    // Core Instantiations
    const app = initializeApp(firebaseConfig);
    const analytics = getAnalytics(app);
    const auth = getAuth(app);
    const db = getFirestore(app);
    const storage = getStorage(app);

    // Layout Hook References
    const authScreen = document.getElementById('authScreen');
    const appScreen = document.getElementById('appScreen');
    const loginForm = document.getElementById('loginForm');
    const registrationForm = document.getElementById('registrationForm');
    const studentTableBody = document.getElementById('studentTableBody');
    const gradeFilter = document.getElementById('gradeFilter');
    const logoutBtn = document.getElementById('logoutBtn');

    // Render loop processing for numeric option blocks
    const gradeDropdowns = [document.getElementById('studentGrade'), gradeFilter];
    for(let i=1; i<=10; i++) {
        gradeDropdowns[0].options.add(new Option(`Grade ${i}`, i));
        gradeDropdowns[1].options.add(new Option(`Grade ${i}`, i));
    }

    // ==========================================
    // SECURITY AUTHENTICATION LAYER
    // ==========================================
    onAuthStateChanged(auth, (user) => {
        if (user) {
            authScreen.classList.add('hidden');
            appScreen.classList.remove('hidden');
            syncStudentDataPipeline();
        } else {
            authScreen.classList.remove('hidden');
            appScreen.classList.add('hidden');
            studentTableBody.innerHTML = '';
        }
    });

    loginForm.addEventListener('submit', async (e) => {
        e.preventDefault();
        try {
            await signInWithEmailAndPassword(auth, document.getElementById('loginEmail').value, document.getElementById('loginPassword').value);
        } catch (error) {
            alert("Login Failed: " + error.message);
        }
    });

    logoutBtn.addEventListener('click', () => signOut(auth));

    // ==========================================
    // DATA WRITER PIPELINE (STORAGE + FIRESTORE)
    // ==========================================
    registrationForm.addEventListener('submit', async (e) => {
        e.preventDefault();
        const file = document.getElementById('studentPhoto').files[0];
        const name = document.getElementById('studentName').value;
        const grade = document.getElementById('studentGrade').value;

        if(!file) return alert("Photo file payload missing.");

        try {
            // Write image object to Firebase Storage Bucket
            const storageRef = ref(storage, `profiles/${Date.now()}_${file.name}`);
            const snapshot = await uploadBytes(storageRef, file);
            const downloadURL = await getDownloadURL(snapshot.ref);

            // Link structured student metadata record to Firestore
            // Mark begins as explicit null per database integrity rules
            await addDoc(collection(db, "students"), {
                name: name,
                grade: parseInt(grade),
                photoURL: downloadURL,
                mark: null, 
                timestamp: new Date()
            });

            registrationForm.reset();
            alert("Student registration completed successfully.");
        } catch (error) {
            alert("Database Error: " + error.message);
        }
    });

    // ==========================================
    // LIVE BROADCAST STREAM & ENGINE
    // ==========================================
    let activeStreamKiller = () => {};

    function syncStudentDataPipeline() {
        activeStreamKiller(); 
        const selectedGrade = gradeFilter.value;
        let queryTarget = collection(db, "students");

        if (selectedGrade !== 'All') {
            queryTarget = query(collection(db, "students"), where("grade", "==", parseInt(selectedGrade)));
        }

        // Live Firestore document stream mapping loop
        activeStreamKiller = onSnapshot(queryTarget, (snapshot) => {
            studentTableBody.innerHTML = '';
            snapshot.forEach((docSnap) => {
                const data = docSnap.data();
                const id = docSnap.id;
                const visualId = id.substring(0,6).toUpperCase();

                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td><img src="${data.photoURL || 'https://via.placeholder.com/40'}" class="avatar"></td>
                    <td><strong>FG-${visualId}</strong></td>
                    <td>${data.name}</td>
                    <td>Grade ${data.grade}</td>
                    <td>
                        <span id="txt-${id}">${data.mark !== null ? data.mark + '%' : '<em>Unassigned</em>'}</span>
                        <input type="number" id="input-${id}" class="hidden" min="0" max="100" value="${data.mark || ''}" style="width:60px;">
                    </td>
                    <td>
                        <button id="edit-${id}" onclick="toggleEditState('${id}')">Set Mark</button>
                        <button id="save-${id}" class="hidden" onclick="commitMarkRecord('${id}')" style="background:#2f855a;">Save</button>
                    </td>
                `;
                studentTableBody.appendChild(tr);
            });
        });
    }

    gradeFilter.addEventListener('change', syncStudentDataPipeline);

    // Global DOM manipulation hooks for dynamically generated rows
    window.toggleEditState = function(id) {
        document.getElementById(`txt-${id}`).classList.toggle('hidden');
        document.getElementById(`input-${id}`).classList.toggle('hidden');
        document.getElementById(`edit-${id}`).classList.toggle('hidden');
        document.getElementById(`save-${id}`).classList.toggle('hidden');
    }

    window.commitMarkRecord = async function(id) {
        const inputField = document.getElementById(`input-${id}`);
        if(inputField.value === '') return alert("Field value cannot remain null.");
        
        const validatedMark = parseInt(inputField.value);
        if(validatedMark < 0 || validatedMark > 100) return alert("System requires percentage validation scores (0-100).");

        try {
            const documentReference = doc(db, "students", id);
            await updateDoc(documentReference, {
                mark: validatedMark
            });
        } catch(error) {
            alert("Write sync verification failed: " + error.message);
        }
    }
</script>
</body>
</html>




