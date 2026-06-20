<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Felege Gion Sunday School - Bulk Mark Entry</title>
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

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--bg);
            color: var(--text);
            padding: 1.5rem;
            display: flex;
            justify-content: center;
        }

        .container {
            width: 100%;
            max-width: 1200px;
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
        }

        header {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            padding: 1.5rem;
            border-radius: 12px;
            box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);
        }

        header h1 { font-size: 1.6rem; font-weight: 600; }
        header p { font-size: 0.9rem; opacity: 0.8; margin-top: 0.25rem; }

        .card {
            background: var(--card-bg);
            padding: 1.5rem;
            border-radius: 12px;
            border: 1px solid var(--border);
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
        }

        .card h2 {
            font-size: 1.2rem;
            margin-bottom: 1rem;
            color: var(--secondary);
            border-bottom: 2px solid var(--border);
            padding-bottom: 0.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        /* Bulk Entry Layout */
        .bulk-header-row {
            display: grid;
            grid-template-columns: 2.5fr 1.2fr 1.2fr 1fr;
            gap: 1rem;
            margin-bottom: 0.5rem;
            padding: 0 0.5rem;
            font-weight: bold;
            font-size: 0.85rem;
            color: var(--secondary);
        }

        .student-row {
            display: grid;
            grid-template-columns: 2.5fr 1.2fr 1.2fr 1fr;
            gap: 1rem;
            margin-bottom: 0.75rem;
            background: var(--bg);
            padding: 0.5rem;
            border-radius: 8px;
            align-items: center;
        }

        input {
            width: 100%;
            padding: 0.6rem;
            border: 1px solid var(--border);
            border-radius: 6px;
            font-size: 0.95rem;
            outline: none;
            background: white;
        }

        input:focus {
            border-color: var(--accent);
        }

        .calculated-total {
            font-weight: bold;
            text-align: center;
            font-size: 1rem;
        }

        .actions-bar {
            display: flex;
            gap: 1rem;
            margin-top: 1.5rem;
        }

        button {
            flex: 1;
            padding: 0.75rem;
            border-radius: 6px;
            font-size: 0.95rem;
            font-weight: 600;
            cursor: pointer;
            border: none;
            transition: background 0.2s;
        }

        .btn-primary { background-color: var(--accent); color: white; }
        .btn-primary:hover { background-color: #1d4ed8; }
        
        .btn-secondary { background-color: #64748b; color: white; }
        .btn-secondary:hover { background-color: #475569; }

        /* Output Table Styles */
        .table-container {
            overflow-x: auto;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            text-align: left;
            font-size: 0.9rem;
        }

        th, td {
            padding: 0.75rem 1rem;
            border-bottom: 1px solid var(--border);
        }

        th {
            background-color: var(--bg);
            color: var(--secondary);
            font-weight: 600;
        }

        .badge {
            font-weight: bold;
            padding: 0.2rem 0.5rem;
            border-radius: 4px;
            font-size: 0.8rem;
        }
        .badge.pass { color: var(--success); background: #f0fdf4; }
        .badge.fail { color: var(--danger); background: #fef2f2; }

        .empty-state {
            text-align: center;
            color: #94a3b8;
            padding: 2rem;
        }
    </style>
</head>
<body>

<div class="container">
    <header>
        <h1>Felege Gion Sunday School</h1>
        <p>Bulk Student Mark Entry Sheet (EXCEL Style Portal)</p>
    </header>

    <div class="card">
        <h2>Enter Student Marks (Batch Sheet)</h2>
        
        <div class="bulk-header-row">
            <div>Student Full Name</div>
            <div>Assessment (Max 40)</div>
            <div>Final Exam (Max 60)</div>
            <div style="text-align: center;">Live Total (100)</div>
        </div>

        <form id="bulkMarkForm">
            <div id="bulkRowsContainer">
                </div>

            <div class="actions-bar">
                <button type="button" class="btn-secondary" id="addMoreRowsBtn">+ Add 5 More Rows</button>
                <button type="submit" class="btn-primary">Process & Lock All Records</button>
            </div>
        </form>
    </div>

    <div class="card table-container">
        <h2>Saved Master Ledger Logs</h2>
        <table id="masterRecordsTable">
            <thead>
                <tr>
                    <th>Student Name</th>
                    <th>Assessment (40)</th>
                    <th>Final Exam (60)</th>
                    <th>Total (100)</th>
                    <th>Result Outcome</th>
                </tr>
            </thead>
            <tbody id="masterTableBody">
                <tr>
                    <td colspan="5" class="empty-state">No permanently saved records in ledger yet. Process form above.</td>
                </tr>
            </tbody>
        </table>
    </div>
</div>

<script>
    const container = document.getElementById('bulkRowsContainer');
    const addRowsBtn = document.getElementById('addMoreRowsBtn');
    const form = document.getElementById('bulkMarkForm');
    const masterTableBody = document.getElementById('masterTableBody');

    let totalRowCount = 0;
    let masterLedger = [];

    // Generates Entry Rows directly inside the UI
    function createEntryRows(count) {
        for (let i = 0; i < count; i++) {
            totalRowCount++;
            const row = document.createElement('div');
            row.className = 'student-row';
            row.innerHTML = `
                <div>
                    <input type="text" class="input-name" placeholder="Student Name ${totalRowCount}">
                </div>
                <div>
                    <input type="number" class="input-assess" min="0" max="40" step="0.5" placeholder="0 - 40">
                </div>
                <div>
                    <input type="number" class="input-final" min="0" max="60" step="0.5" placeholder="0 - 60">
                </div>
                <div class="calculated-total" id="total-row-${totalRowCount}">0.0</div>
            `;
            container.appendChild(row);

            // Real-Time Event Math Listeners per Row (Like Excel cells)
            const assessInput = row.querySelector('.input-assess');
            const finalInput = row.querySelector('.input-final');
            const totalDisplay = row.querySelector('.calculated-total');

            function calculateRowTotal() {
                const assessVal = parseFloat(assessInput.value) || 0;
                const finalVal = parseFloat(finalInput.value) || 0;
                const sum = assessVal + finalVal;
                totalDisplay.textContent = sum.toFixed(1);
                
                if (sum >= 50) {
                    totalDisplay.style.color = 'var(--success)';
                } else if (sum > 0) {
                    totalDisplay.style.color = 'var(--danger)';
                } else {
                    totalDisplay.style.color = 'var(--text)';
                }
            }

            assessInput.addEventListener('input', calculateRowTotal);
            finalInput.addEventListener('input', calculateRowTotal);
        }
    }

    // Initialize with 10 blank student rows immediately
    createEntryRows(10);

    // Expand form dynamically
    addRowsBtn.addEventListener('click', () => createEntryRows(5));

    // Handle full batch submission 
    form.addEventListener('submit', (e) => {
        e.preventDefault();

        const rows = container.querySelectorAll('.student-row');
        let newRecords = [];
        let validationFailed = false;

        rows.forEach(row => {
            const name = row.querySelector('.input-name').value.trim();
            const assessRaw = row.querySelector('.input-assess').value;
            const finalRaw = row.querySelector('.input-final').value;

            // Skip row if it is entirely empty
            if (!name && !assessRaw && !finalRaw) return;

            // If name or marks are partially filled, flag it 
            if (!name || assessRaw === '' || finalRaw === '') {
                alert("Error: Please make sure fields are completed fully for rows with data.");
                validationFailed = true;
                return;
            }

            const assessment = parseFloat(assessRaw);
            const final = parseFloat(finalRaw);
            
            if (assessment < 0 || assessment > 40 || final < 0 || final > 60) {
                alert(`Error: Invalid bounds found for ${name}. Marks must stay inside 40 and 60 parameters.`);
                validationFailed = true;
                return;
            }

            const total = assessment + final;
            const status = total >= 50 ? "Pass" : "Fail";

            newRecords.push({ name, assessment, final, total, status });
        });

        if (validationFailed) return;

        if (newRecords.length === 0) {
            alert("No student data found to process.");
            return;
        }

        // Add to global application state ledger
        masterLedger = [...masterLedger, ...newRecords];

        // Reset inputs on form
        form.reset();
        rows.forEach(row => row.querySelector('.calculated-total').textContent = '0.0');

        // Render Ledger View
        renderMasterTable();
    });

    function renderMasterTable() {
        if (masterLedger.length === 0) {
            masterTableBody.innerHTML = `<tr><td colspan="5" class="empty-state">No permanently saved records in ledger yet. Process form above.</td></tr>`;
            return;
        }

        masterTableBody.innerHTML = masterLedger.map(record => `
            <tr>
                <td><strong>${record.name}</strong></td>
                <td>${record.assessment.toFixed(1)}</td>
                <td>${record.final.toFixed(1)}</td>
                <td><strong>${record.total.toFixed(1)}</strong></td>
                <td><span class="badge ${record.status.toLowerCase()}">${record.status}</span></td>
            </tr>
        `).join('');
    }
</script>

</body>
</html>


