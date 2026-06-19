<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Felege Gion Sunday School - Mark Portal</title>
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
            padding: 2rem;
            display: flex;
            justify-content: center;
        }

        .container {
            width: 100%;
            max-width: 1100px;
            display: grid;
            grid-template-columns: 1fr;
            gap: 2rem;
        }

        @media (min-width: 768px) {
            .container {
                grid-template-columns: 350px 1fr;
            }
        }

        header {
            grid-column: 1 / -1;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            padding: 1.5rem 2rem;
            border-radius: 12px;
            box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);
        }

        header h1 { font-size: 1.5rem; font-weight: 600; }
        header p { font-size: 0.875rem; opacity: 0.8; margin-top: 0.25rem; }

        .card {
            background: var(--card-bg);
            padding: 1.5rem;
            border-radius: 12px;
            border: 1px solid var(--border);
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
            height: fit-content;
        }

        .card h2 {
            font-size: 1.1rem;
            margin-bottom: 1.25rem;
            color: var(--secondary);
            border-bottom: 2px solid var(--border);
            padding-bottom: 0.5rem;
        }

        .form-group {
            margin-bottom: 1rem;
        }

        label {
            display: block;
            font-size: 0.85rem;
            font-weight: 600;
            margin-bottom: 0.4rem;
        }

        input {
            width: 100%;
            padding: 0.6rem 0.75rem;
            border: 1px solid var(--border);
            border-radius: 6px;
            font-size: 0.95rem;
            outline: none;
            transition: border-color 0.2s;
        }

        input:focus {
            border-color: var(--accent);
        }

        .preview-box {
            background: var(--bg);
            padding: 1rem;
            border-radius: 8px;
            margin-top: 1rem;
            border-left: 4px solid var(--accent);
        }

        .preview-row {
            display: flex;
            justify-content: space-between;
            font-size: 0.875rem;
            margin-bottom: 0.4rem;
        }
        .preview-row:last-child { margin-bottom: 0; }

        .badge {
            font-weight: bold;
            padding: 0.1rem 0.4rem;
            border-radius: 4px;
        }
        .badge.pass { color: var(--success); background: #f0fdf4; }
        .badge.fail { color: var(--danger); background: #fef2f2; }

        button {
            width: 100%;
            background-color: var(--accent);
            color: white;
            border: none;
            padding: 0.75rem;
            border-radius: 6px;
            font-size: 0.95rem;
            font-weight: 600;
            cursor: pointer;
            margin-top: 1rem;
            transition: background 0.2s;
        }

        button:hover { background-color: #1d4ed8; }

        .table-container {
            overflow-x: auto;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            text-align: left;
            font-size: 0.9rem;
        }

        th {
            background-color: var(--bg);
            color: var(--secondary);
            font-weight: 600;
            padding: 0.75rem 1rem;
            border-bottom: 2px solid var(--border);
        }

        td {
            padding: 0.75rem 1rem;
            border-bottom: 1px solid var(--border);
        }

        tr:hover { background-color: #f8fafc; }
        
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
        <p>Student Mark Management Portal & System Preview</p>
    </header>

    <div class="card">
        <h2>Record Student Marks</h2>
        <form id="markForm">
            <div class="form-group">
                <label for="studentName">Student Full Name</label>
                <input type="text" id="studentName" required placeholder="e.g. Yohannes Alemu">
            </div>
            <div class="form-group">
                <label for="assessment">Assessment Mark (Max 40)</label>
                <input type="number" id="assessment" min="0" max="40" step="0.5" required placeholder="0.0 - 40.0">
            </div>
            <div class="form-group">
                <label for="finalExam">Final Exam Mark (Max 60)</label>
                <input type="number" id="finalExam" min="0" max="60" step="0.5" required placeholder="0.0 - 60.0">
            </div>

            <div class="preview-box">
                <div class="preview-row">
                    <span>Calculated Total:</span>
                    <strong id="previewTotal">0.00 / 100</strong>
                </div>
                <div class="preview-row">
                    <span>Status Outcome:</span>
                    <span id="previewStatus" class="badge fail">Fail</span>
                </div>
            </div>

            <button type="submit">Submit and Save</button>
        </form>
    </div>

    <div class="card table-container">
        <h2>Saved Records Log</h2>
        <table id="recordsTable">
            <thead>
                <tr>
                    <th>Student Name</th>
                    <th>Assessment (40)</th>
                    <th>Final Exam (60)</th>
                    <th>Total (100)</th>
                    <th>Result Status</th>
                </tr>
            </thead>
            <tbody id="tableBody">
                <tr>
                    <td colspan="5" class="empty-state">No student records entered yet.</td>
                </tr>
            </tbody>
        </table>
    </div>
</div>

<script>
    const form = document.getElementById('markForm');
    const studentNameInput = document.getElementById('studentName');
    const assessmentInput = document.getElementById('assessment');
    const finalExamInput = document.getElementById('finalExam');
    
    const previewTotal = document.getElementById('previewTotal');
    const previewStatus = document.getElementById('previewStatus');
    const tableBody = document.getElementById('tableBody');

    let records = [];

    // Live Preview Calculation Event Listeners
    function updateLivePreview() {
        const assessment = parseFloat(assessmentInput.value) || 0;
        const finalExam = parseFloat(finalExamInput.value) || 0;
        const total = assessment + finalExam;

        previewTotal.textContent = `${total.toFixed(2)} / 100`;

        if (total >= 50) {
            previewStatus.textContent = "Pass";
            previewStatus.className = "badge pass";
        } else {
            previewStatus.textContent = "Fail";
            previewStatus.className = "badge fail";
        }
    }

    assessmentInput.addEventListener('input', updateLivePreview);
    finalExamInput.addEventListener('input', updateLivePreview);

    // Render Table Logic
    function renderTable() {
        if (records.length === 0) {
            tableBody.innerHTML = `<tr><td colspan="5" class="empty-state">No student records entered yet.</td></tr>`;
            return;
        }

        tableBody.innerHTML = records.map(record => `
            <tr>
                <strong><td>${record.name}</td></strong>
                <td>${record.assessment.toFixed(1)}</td>
                <td>${record.final.toFixed(1)}</td>
                <td><strong>${record.total.toFixed(1)}</strong></td>
                <td><span class="badge ${record.status.toLowerCase()}">${record.status}</span></td>
            </tr>
        `).join('');
    }

    // Form Submission Handling
    form.addEventListener('submit', (e) => {
        e.preventDefault();

        const name = studentNameInput.value.trim();
        const assessment = parseFloat(assessmentInput.value);
        const final = parseFloat(finalExamInput.value);
        const total = assessment + final;
        const status = total >= 50 ? "Pass" : "Fail";

        // Double check bounds safety
        if (assessment < 0 || assessment > 40 || final < 0 || final > 60) {
            alert("Entry rejected. Keep values inside 40 and 60 marks bounds.");
            return;
        }

        // Add to tracking array
        records.push({ name, assessment, final, total, status });

        // Reset system view
        form.reset();
        updateLivePreview();
        renderTable();
        studentNameInput.focus();
    });
</script>

</body>
</html>

