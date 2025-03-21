<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>โปรแกรมคำนวณหนี้</title>
    <style>
        body {
            font-family: 'Prompt', sans-serif;
            background-color: #f0f8ff;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
        }
        h1 {
            color: #1e90ff;
        }
        label {
            display: block;
            margin-top: 10px;
        }
        input, select, button {
            margin-top: 5px;
            padding: 8px;
            font-size: 14px;
        }
        button {
            background-color: #1e90ff;
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #4682b4;
        }
        #result {
            margin-top: 20px;
            padding: 10px;
            border: 1px solid #1e90ff;
            background-color: white;
        }
    </style>
</head>
<body>
    <h1>โปรแกรมคำนวณหนี้</h1>

    <label>จำนวนเงินต้น (บาท):</label>
    <input type="number" id="principal" required>

    <label>อัตราดอกเบี้ยต่อปี (%):</label>
    <input type="number" id="interestRate" required>

    <label>วันที่ศาลพิพากษา:</label>
    <input type="date" id="judgmentDate" required>

    <label>คำนวณยอดหนี้ถึงวันที่:</label>
    <input type="date" id="calculationDate" required>

    <button onclick="calculateDebt()">คำนวณ</button>

    <div id="result"></div>

    <script>
        function calculateDebt() {
            const principal = parseFloat(document.getElementById('principal').value);
            const interestRate = parseFloat(document.getElementById('interestRate').value);
            const judgmentDate = new Date(document.getElementById('judgmentDate').value);
            const calculationDate = new Date(document.getElementById('calculationDate').value);

            if (isNaN(principal) || isNaN(interestRate) || isNaN(judgmentDate) || isNaN(calculationDate)) {
                alert('กรุณากรอกข้อมูลให้ครบถ้วน');
                return;
            }

            const oneDay = 24 * 60 * 60 * 1000;
            const daysDifference = Math.round((calculationDate - judgmentDate) / oneDay);
            const interestPerDay = (interestRate / 100) / 365;
            const totalInterest = principal * interestPerDay * daysDifference;
            const totalDebt = principal + totalInterest;

            const options = { year: 'numeric', month: 'long', day: 'numeric' };
            const judgmentDateThai = judgmentDate.toLocaleDateString('th-TH', options);
            const calculationDateThai = calculationDate.toLocaleDateString('th-TH', options);

            const resultDiv = document.getElementById('result');
            resultDiv.innerHTML = `
                <h2>ผลการคำนวณ</h2>
                <p>จากวันที่ศาลพิพากษา: ${judgmentDateThai}</p>
                <p>ถึงวันที่คำนวณ: ${calculationDateThai}</p>
                <p>จำนวนวันที่คิดดอกเบี้ย: ${daysDifference} วัน</p>
                <p>ยอดดอกเบี้ยที่ต้องชำระ: ${totalInterest.toFixed(2)} บาท</p>
                <p>ยอดหนี้รวม: ${totalDebt.toFixed(2)} บาท</p>
            `;
        }
    </script>
</body>
</html>
