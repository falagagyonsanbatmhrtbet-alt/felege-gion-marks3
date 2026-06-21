<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Felege Gion Sunday School Portal</title>
    <style>
        :root {
            --primary: #1e3a8a;
            --secondary: #0f172a;
            --accent: #2563eb;
            --success: #16a34a;
            --danger: #dc2626;
            --bg: #f8fafc;
            --card-bg: #ffffff;
            --text: #334155;
            --border: #e2e8f0;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', sans-serif; }
        body { background-color: var(--bg); color: var(--text); padding: 0.5rem; display: flex; justify-content: center; }
        .container { width: 100%; max-width: 1400px; display: flex; flex-direction: column; gap: 1rem; }
        
        header {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white; padding: 1rem; border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            display: flex; align-items: center; gap: 1rem;
        }

        .logo-img {
            width: 60px; height: 60px; border-radius: 50%;
            background-color: white; object-fit: cover; border: 2px solid white;
        }

        .header-text h1 { font-size: 1.3rem; font-weight: 600; }
        .header-text p { font-size: 0.8rem; opacity: 0.8; margin-top: 0.15rem; }

        .card {
            background: var(--card-bg); padding: 1rem; border-radius: 8px;
            border: 1px solid var(--border); box-shadow: 0 1px 3px rgba(0,0,0,0.05);
        }

        h2 { font-size: 1.05rem; margin-bottom: 0.75rem; color: var(--secondary); border-bottom: 2px solid var(--border); padding-bottom: 0.25rem; }
        
        .config-panel { margin-bottom: 1rem; background: #f1f5f9; padding: 0.75rem; border-radius: 6px; max-width: 260px; }
        label { display: block; font-size: 0.8rem; font-weight: 600; margin-bottom: 0.25rem; color: var(--secondary); }
        select { width: 100%; padding: 0.5rem; border: 1px solid var(--border); border-radius: 6px; font-size: 0.9rem; background: white; }

        /* Optimized Spreadsheet Wrapper */
        .spreadsheet-wrapper {
            overflow-x: auto;
            margin-bottom: 1rem;
        }

        /* Matrix Grid Configuration with Fixed Input Box Widths */
        .bulk-header-row {
            display: grid; 
            grid-template-columns: minmax(160px, 2fr) repeat(6, 50px) 55px 40px; 
            gap: 0.35rem; margin-bottom: 0.4rem; padding: 0 0.25rem; font-weight: bold; font-size: 0.75rem; text-align: center;
            align-items: end; min-width: 560px;
        }

        .student-row {
            display: grid; 
            grid-template-columns: minmax(160px, 2fr) repeat(6, 50px) 55px 40px; 
            gap: 0.35rem; margin-bottom: 0.5rem; background: var(--bg); padding: 0.35rem; border-radius: 6px; align-items: center;
            min-width: 560px;
        }

        input { width: 100%; padding: 0.4rem 0.2rem; border: 1px solid var(--border); border-radius: 4px; font-size: 0.85rem; text-align: center; }
        input.input-name { text-align: left; padding-left: 0.5rem; }
        input:focus { border-color: var(--accent); outline: none; background-color: #fff; }

        .calculated-total { font-weight: bold; text-align: center; font-size: 0.85rem; }
        .actions-bar { display: flex; gap: 0.75rem; margin-top: 1rem; }
        
        button { padding: 0.6rem; border-radius: 6px; font-size: 0.9rem; font-weight: 600; cursor: pointer; border: none; }
        .btn-primary { flex: 2; background-color: var(--accent); color: white; }
        .btn-secondary { flex: 1; background-color: #64748b; color: white; }
        
        .btn-remove { background-color: transparent; color: var(--danger); font-size: 1rem; cursor: pointer; display: inline-block; width: 100%; }

        .table-container { overflow-x: auto; margin-top: 1rem; }
        table { width: 100%; border-collapse: collapse; text-align: left; font-size: 0.8rem; min-width: 650px; }
        th, td { padding: 0.5rem 0.35rem; border-bottom: 1px solid var(--border); text-align: center; }
        th.text-left, td.text-left { text-align: left; }
        th { background-color: var(--bg); color: var(--secondary); }
        
        .badge { font-weight: bold; padding: 0.15rem 0.35rem; border-radius: 4px; font-size: 0.7rem; }
        .badge.pass { color: var(--success); background: #f0fdf4; }
        .badge.fail { color: var(--danger); background: #fef2f2; }
        .empty-state { text-align: center; color: #94a3b8; padding: 1.5rem; }
    </style>
</head>
<body>

<div class="container">
    <header>
        <img src="logo.jpg" alt="Sunday School Logo" class="logo-img" onerror="this.style.display='none'">
        <div class="header-text">
            <h1>Felege Gion Sunday School</h1>
            <p>Official Digital Mark Registration Ledger</p>
        </div>
    </header>

    <div class="card">
        <h2>1. Class Selection</h2>
        <div class="config-panel">
            <label for="gradeSelect">Grade Level / ክፍል</label>
            <select id="gradeSelect">
                <option value="Grade 1">Grade 1</option>
                <option value="Grade 2">Grade 2</option>
                <option value="Grade 3">Grade 3</option>
                <option value="Grade 4">Grade 4</option>
                <option value="Grade 5">Grade 5</option>
                <option value="Grade 6">Grade 6</option>
                <option value="Grade 7">Grade 7</option>
                <option value="Grade 8">Grade 8</option>
                <option value="Grade 9">Grade 9</option>
                <option value="Grade 10">Grade 10</option>
                <option value="Grade 11">Grade 11</option>
                <option value="Grade 12">Grade 12</option>
            </select>
        </div>

        <h2>2. Roster Mark Sheet Entry</h2>
        <form id="bulkMarkForm">
            <div class="spreadsheet-wrapper">
                <div class="bulk-header-row">
                    <div style="text-align: left;">Student Name</div>
                    <div>Taste<br>10%</div>
                    <div>Asgn<br>10%</div>
                    <div>Book<br>5%</div>
                    <div>Att<br>10%</div>
                    <div>Mid<br>25%</div>
                    <div>Fin<br>40%</div>
                    <div>Total<br>100%</div>
                    <div>Rem</div>
                </div>
                <div id="bulkRowsContainer"></div>
            </div>
            <div class="actions-bar">
                <button type="button" class="btn-secondary" id="addMoreRowsBtn">+ Add 5 Rows</button>
                <button type="submit" class="btn-primary" id="submitBtn">Save Marks to Firebase Cloud</button>
            </div>
        </form>
    </div>

    <div class="card table-container">
        <h2>Cloud Storage Master Ledger Log</h2>
        <table id="masterRecordsTable">
            <thead>
                <tr>
                    <th>Grade</th>
                    <th class="text-left">Student Name</th>
                    <th>Taste (10)</th>
                    <th>Assign (10)</th>
                    <th>Book (5)</th>
                    <th>Atten (10)</th>
                    <th>Mid (25)</th>
                    <th>Final (40)</th>
                    <th>Total (100)</th>
                    <th>Status</th>
                </tr>
            </thead>
            <tbody id="masterTableBody">
                <tr><td colspan="10" class="empty-state">Loading database records...</td></tr>
            </tbody>
        </table>
    </div>
</div>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
    import { getFirestore, collection, addDoc, getDocs, query, orderBy } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";
    import { getAnalytics } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-analytics.js";

    // Your integrated Firebase web credentials
    const firebaseConfig = {
        apiKey: "AIzaSyB4FdrgFQop1WvubMDhANtUcrHWnQ-kWf4",
        authDomain: "sundayschoolportal-a0931.firebaseapp.com",
        projectId: "sundayschoolportal-a0931",
        storageBucket: "sundayschoolportal-a0931.firebasestorage.app",
        messagingSenderId: "964240028485",
        appId: "1:964240028485:web:655a48afc5bdad0fdbc42c",
        measurementId: "G-8TNMKQ8LJ9"
    };

    const app = initializeApp(firebaseConfig);
    const db = getFirestore(app);
    const analytics = getAnalytics(app);

    const container = document.getElementById('bulkRowsContainer');
    const addRowsBtn = document.getElementById('addMoreRowsBtn');
    const form = document.getElementById('bulkMarkForm');
    const masterTableBody = document.getElementById('masterTableBody');
    const gradeSelect = document.getElementById('gradeSelect');

    function createEntryRows(count) {
        for (let i = 0; i < count; i++) {
            const row = document.createElement('div');
            row.className = 'student-row';
            row.innerHTML = `
                <div><input type="text" class="input-name" placeholder="Full Name"></div>
                <div><input type="number" class="input-taste" min="0" max="10" step="0.5" placeholder="10"></div>
                <div><input type="number" class="input-assign" min="0" max="10" step="0.5" placeholder="10"></div>
                <div><input type="number" class="input-book" min="0" max="5" step="0.5" placeholder="5"></div>
                <div><input type="number" class="input-atten" min="0" max="10" step="0.5" placeholder="10"></div>
                <div><input type="number" class="input-mid" min="0" max="25" step="0.5" placeholder="25"></div>
                <div><input type="number" class="input-final" min="0" max="40" step="0.5" placeholder="40"></div>
                <div class="calculated-total">0.0</div>
                <div style="text-align: center;"><button type="button" class="btn-remove" title="Subtract row">✖</button></div>
            `;
            container.appendChild(row);

            const inputs = row.querySelectorAll('input[type="number"]');
            const totalDisplay = row.querySelector('.calculated-total');
            const removeBtn = row.querySelector('.btn-remove');

            function calculateRow() {
                let total = 0;
                inputs.forEach(input => {
                    total += parseFloat(input.value) || 0;
                });
                totalDisplay.textContent = total.toFixed(1);
                totalDisplay.style.color = total >= 50 ? 'var(--success)' : (total > 0 ? 'var(--danger)' : 'var(--text)');
            }

            inputs.forEach(input => input.addEventListener('input', calculateRow));
            removeBtn.addEventListener('click', () => { row.remove(); });
        }
    }

    createEntryRows(10);
    addRowsBtn.addEventListener('click', () => createEntryRows(5));

    async function fetchFromCloud() {
        try {
            const q = query(collection(db, "sunday_school_marks"), orderBy("timestamp", "desc"));
            const querySnapshot = await getDocs(q);
            if(querySnapshot.empty) {
                masterTableBody.innerHTML = `<tr><td colspan="10" class="empty-state">No records saved in database yet.</td></tr>`;
                return;
            }
            
            let output = "";
            querySnapshot.forEach(doc => {
                const data = doc.data();
                output += `
                    <tr>
                        <td><strong>${data.grade}</strong></td>
                        <td class="text-left">${data.name}</td>
                        <td>${data.taste}</td>
                        <td>${data.assignment}</td>
                        <td>${data.bookMark}</td>
                        <td>${data.attendance}</td>
                        <td>${data.midExam}</td>
                        <td>${data.finalExam}</td>
                        <td><strong>${data.totalScore}</strong></td>
                        <td><span class="badge ${data.status.toLowerCase()}">${data.status}</span></td>
                    </tr>
                `;
            });
            masterTableBody.innerHTML = output;
        } catch (e) {
            console.error(e);
            masterTableBody.innerHTML = `<tr><td colspan="10" class="empty-state" style="color:var(--danger)">Failed to synchronize cloud ledger data. Make sure rules are set to test mode.</td></tr>`;
        }
    }

    fetchFromCloud();

    form.addEventListener('submit', async (e) => {
        e.preventDefault();
        const rows = container.querySelectorAll('.student-row');
        const selectedGrade = gradeSelect.value;
        let uploadQueue = [];
        let validationError = false;

        rows.forEach(row => {
            const name = row.querySelector('.input-name').value.trim();
            const taste = row.querySelector('.input-taste').value;
            const assign = row.querySelector('.input-assign').value;
            const book = row.querySelector('.input-book').value;
            const atten = row.querySelector('.input-atten').value;
            const mid = row.querySelector('.input-mid').value;
            const final = row.querySelector('.input-final').value;

            if (!name && taste==='' && assign==='' && book==='' && atten==='' && mid==='' && final==='') return;

            if (!name || taste==='' || assign==='' || book==='' || atten==='' || mid==='' || final==='') {
                alert("Error: Please fully complete or subtract incomplete records using the ✖ option.");
                validationError = true;
                return;
            }

            const tVal = parseFloat(taste);
            const asVal = parseFloat(assign);
            const bVal = parseFloat(book);
            const atVal = parseFloat(atten);
            const mVal = parseFloat(mid);
            const fVal = parseFloat(final);

            if (tVal<0 || tVal>10 || asVal<0 || asVal>10 || bVal<0 || bVal>5 || atVal<0 || atVal>10 || mVal<0 || mVal>25 || fVal<0 || fVal>40) {
                alert(`Error: Out of bounds scores identified on entry for ${name}.`);
                validationError = true;
                return;
            }

            const totalScore = tVal + asVal + bVal + atVal + mVal + fVal;
            const status = totalScore >= 50 ? "Pass" : "Fail";

            uploadQueue.push(addDoc(collection(db, "sunday_school_marks"), {
                grade: selectedGrade,
                name: name,
                taste: tVal,
                assignment: asVal,
                bookMark: bVal,
                attendance: atVal,
                midExam: mVal,
                finalExam: fVal,
                totalScore: totalScore,
                status: status,
                timestamp: new Date()
            }));
        });

        if (validationError) return;
        if (uploadQueue.length === 0) return alert("No valid inputs detected.");

        try {
            await Promise.all(uploadQueue);
            alert(`Roster entries for ${selectedGrade} uploaded directly into cloud backend!`);
            container.innerHTML = "";
            createEntryRows(10);
            fetchFromCloud();
        } catch (err) {
            console.error(err);
            alert("Database entry writing aborted.");
        }
    });
</script>
</body>
</html>



