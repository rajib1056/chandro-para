<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>অনুদান সংস্থার হিসাব ব্যবস্থাপনা</title>
    <style>
        body {
            font-family: 'Segoe UI', sans-serif;
            background-color: #edf2f7;
            text-align: center;
            padding: 20px;
        }
        h1 {
            font-size: 2em;
            margin-bottom: 10px;
        }
        select, input, button {
            padding: 10px;
            margin: 5px;
            width: 200px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        table {
            width: 90%;
            margin: 20px auto;
            border-collapse: collapse;
            background-color: white;
        }
        th, td {
            padding: 10px;
            border: 1px solid #ddd;
        }
        th {
            background-color: #f7fafc;
        }
    </style>
</head>
<body>

    <h1>অনুদান সংস্থার হিসাব ব্যবস্থাপনা</h1>

    <input type="date" id="dateInput">
    <select id="typeInput">
        <option value="donation">অনুদান</option>
        <option value="expense">খরচ</option>
    </select><br>

    <input type="text" id="descriptionInput" placeholder="বিবরণ"><br>
    <input type="number" id="amountInput" placeholder="পরিমাণ"><br>
    <button onclick="addRecord()">যোগ করুন</button><br><br>

    <input type="text" id="searchInput" placeholder="অনুসন্ধান করুন...">
    <select id="filterType" onchange="filterTable()">
        <option value="all">সব দেখান</option>
        <option value="donation">অনুদান</option>
        <option value="expense">খরচ</option>
    </select>

    <table id="recordTable">
        <thead>
            <tr>
                <th>তারিখ</th>
                <th>ধরন</th>
                <th>বিবরণ</th>
                <th>পরিমাণ (৳)</th>
            </tr>
        </thead>
        <tbody></tbody>
    </table>

    <script>
        let records = [];

        function addRecord() {
            const date = document.getElementById('dateInput').value;
            const type = document.getElementById('typeInput').value;
            const description = document.getElementById('descriptionInput').value;
            const amount = parseFloat(document.getElementById('amountInput').value);

            if (!date || !description || isNaN(amount)) {
                alert('সব তথ্য সঠিকভাবে পূরণ করুন।');
                return;
            }

            records.push({ date, type, description, amount });
            displayRecords();
            clearInputs();
        }

        function clearInputs() {
            document.getElementById('descriptionInput').value = '';
            document.getElementById('amountInput').value = '';
        }

        function displayRecords() {
            const tbody = document.getElementById('recordTable').getElementsByTagName('tbody')[0];
            tbody.innerHTML = '';
            records.forEach(record => {
                const row = tbody.insertRow();
                row.insertCell(0).innerText = record.date;
                row.insertCell(1).innerText = record.type === 'donation' ? 'অনুদান' : 'খরচ';
                row.insertCell(2).innerText = record.description;
                row.insertCell(3).innerText = record.amount.toFixed(2);
            });
        }

        function filterTable() {
            const searchText = document.getElementById('searchInput').value.toLowerCase();
            const filterType = document.getElementById('filterType').value;
            const tbody = document.getElementById('recordTable').getElementsByTagName('tbody')[0];
            tbody.innerHTML = '';

            records.forEach(record => {
                if ((filterType === 'all' || record.type === filterType) &&
                    (record.description.toLowerCase().includes(searchText) || record.date.includes(searchText))) {
                    const row = tbody.insertRow();
                    row.insertCell(0).innerText = record.date;
                    row.insertCell(1).innerText = record.type === 'donation' ? 'অনুদান' : 'খরচ';
                    row.insertCell(2).innerText = record.description;
                    row.insertCell(3).innerText = record.amount.toFixed(2);
                }
            });
        }
    </script>

</body>
</html>
