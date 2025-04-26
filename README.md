<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>অনুদান সংস্থার হিসাব ব্যবস্থাপনা</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f2f5;
            padding: 20px;
            text-align: center;
        }
        h1 {
            font-size: 28px;
            margin-bottom: 20px;
        }
        input, select, button {
            margin: 5px;
            padding: 8px;
            font-size: 16px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
            background-color: white;
        }
        th, td {
            border: 1px solid #ccc;
            padding: 10px;
            text-align: center;
        }
        th {
            background-color: #e0e0e0;
        }
    </style>
</head>
<body>

    <h1>অনুদান সংস্থার হিসাব ব্যবস্থাপনা</h1>

    <input type="date" id="dateInput">
    <select id="typeInput">
        <option value="donation">অনুদান</option>
        <option value="expense">খরচ</option>
    </select>
    <input type="text" id="descriptionInput" placeholder="বিবরণ">
    <input type="number" id="amountInput" placeholder="পরিমাণ">
    <button onclick="addRecord()">যোগ করুন</button>
    <br><br>

    <input type="text" id="searchInput" placeholder="অনুসন্ধান করুন...">
    <select id="filterType" onchange="filterTable()">
        <option value="all">সব দেখান</option>
        <option value="donation">অনুদান</option>
        <option value="expense">খরচ</option>
    </select>

    <table>
        <thead>
            <tr>
                <th>তারিখ</th>
                <th>ধরণ</th>
                <th>বিবরণ</th>
                <th>পরিমাণ (৳)</th>
            </tr>
        </thead>
        <tbody id="recordTable">
            <!-- রেকর্ডগুলো এখানে যোগ হবে -->
        </tbody>
    </table>

    <script>
        let records = [];

        function addRecord() {
            const date = document.getElementById('dateInput').value;
            const type = document.getElementById('typeInput').value;
            const description = document.getElementById('descriptionInput').value;
            const amount = document.getElementById('amountInput').value;

            if (!date || !description || !amount) {
                alert('সব ঘর পূরণ করুন।');
                return;
            }

            records.push({ date, type, description, amount: parseFloat(amount) });
            filterTable();
            
            document.getElementById('dateInput').value = '';
            document.getElementById('descriptionInput').value = '';
            document.getElementById('amountInput').value = '';
        }

        function filterTable() {
            const searchText = document.getElementById('searchInput').value.toLowerCase();
            const filterType = document.getElementById('filterType').value;
            const tbody = document.getElementById('recordTable');
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
