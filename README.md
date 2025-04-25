# start uchun
# Create a complete HTML file combining all 6 economic modules

html_content = """
<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Iqtisodiy Ilova</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f9f9f9;
      padding: 20px;
    }
    section {
      background: white;
      padding: 20px;
      margin: 20px auto;
      max-width: 800px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    h2 {
      color: #333;
    }
    input, select, button {
      padding: 8px;
      margin: 5px;
    }
    canvas {
      max-width: 100%;
    }
  </style>
</head>
<body>

<section>
  <h2>1. Vazifa Ro'yxati</h2>
  <input id="task" type="text" placeholder="Vazifa yozing">
  <button onclick="addTask()">Qo'shish</button>
  <ul id="list"></ul>
</section>

<section>
  <h2>2. Valyuta Konvertori</h2>
  <input type="number" id="currency-amount" placeholder="Miqdorni kiriting">
  <select id="from-currency"><option value="USD">USD</option><option value="UZS">UZS</option></select>
  <select id="to-currency"><option value="UZS">UZS</option><option value="USD">USD</option></select>
  <button onclick="convertCurrency()">Aylantirish</button>
  <p id="conversion-result"></p>
</section>

<section>
  <h2>3. Kredit Kalkulyatori</h2>
  <input type="number" id="loan-amount" placeholder="Kredit miqdori (so'm)">
  <input type="number" id="interest-rate" placeholder="Foiz stavkasi (% yillik)">
  <input type="number" id="loan-years" placeholder="Muddat (yil)">
  <button onclick="calculateLoan()">Hisoblash</button>
  <p id="loan-result"></p>
</section>

<section>
  <h2>4. Makroiqtisodiy Ko‘rsatkichlar</h2>
  <canvas id="macroChart" height="100"></canvas>
</section>

<section>
  <h2>5. Soliq Hisobi</h2>
  <input type="number" id="income" placeholder="Yillik daromad (so'm)">
  <button onclick="calculateTax()">Hisoblash</button>
  <p id="tax-result"></p>
</section>

<section>
  <h2>6. Narx – Taklif Grafikasi</h2>
  <canvas id="supplyDemandChart" height="120"></canvas>
</section>

<script>
function addTask() {
  const taskInput = document.getElementById("task");
  const taskText = taskInput.value.trim();
  if (taskText === "") return;
  const li = document.createElement("li");
  li.innerHTML = taskText + ' <button onclick="removeTask(this)">O‘chirish</button>';
  document.getElementById("list").appendChild(li);
  taskInput.value = "";
}
function removeTask(button) {
  button.parentElement.remove();
}

function convertCurrency() {
  const amount = parseFloat(document.getElementById("currency-amount").value);
  const from = document.getElementById("from-currency").value;
  const to = document.getElementById("to-currency").value;
  let rate = (from === "USD" && to === "UZS") ? 12500 : (from === "UZS" && to === "USD") ? 1 / 12500 : 1;
  const result = amount * rate;
  document.getElementById("conversion-result").innerText = amount + " " + from + " = " + result.toFixed(2) + " " + to;
}

function calculateLoan() {
  const amount = parseFloat(document.getElementById("loan-amount").value);
  const annualRate = parseFloat(document.getElementById("interest-rate").value) / 100;
  const years = parseFloat(document.getElementById("loan-years").value);
  const monthlyRate = annualRate / 12;
  const n = years * 12;
  const monthlyPayment = (amount * monthlyRate) / (1 - Math.pow(1 + monthlyRate, -n));
  const totalPayment = monthlyPayment * n;
  document.getElementById("loan-result").innerText = !isNaN(monthlyPayment)
    ? `Oylik to'lov: ${monthlyPayment.toFixed(2)} so'm\nJami to'lov: ${totalPayment.toFixed(2)} so'm`
    : "Iltimos, barcha maydonlarni to‘ldiring.";
}

function calculateTax() {
  const income = parseFloat(document.getElementById("income").value);
  let tax = 0;
  if (income <= 12000000) tax = income * 0.12;
  else if (income <= 24000000) tax = income * 0.15;
  else tax = income * 0.20;
  document.getElementById("tax-result").innerText = `Hisoblangan yillik soliq: ${tax.toLocaleString()} so'm`;
}

new Chart(document.getElementById("macroChart"), {
  type: "line",
  data: {
    labels: ["2021", "2022", "2023", "2024"],
    datasets: [
      { label: "YAIM (mlrd $)", data: [70, 78, 85, 90], borderColor: "blue", fill: false },
      { label: "Inflyatsiya (%)", data: [11.5, 10.3, 9.8, 8.6], borderColor: "red", fill: false },
      { label: "Ishsizlik (%)", data: [9.5, 8.7, 8.2, 7.5], borderColor: "green", fill: false }
    ]
  }
});

new Chart(document.getElementById("supplyDemandChart"), {
  type: "line",
  data: {
    labels: [10, 20, 30, 40, 50],
    datasets: [
      { label: "Talab", data: [90, 70, 50, 30, 10], borderColor: "blue", fill: false },
      { label: "Taklif", data: [10, 30, 50, 70, 90], borderColor: "orange", fill: false }
    ]
  }
});
</script>

</body>
</html>
"""

# Save the content to an HTML file
file_path = "/mnt/data/iqtisodiy_ilova.html"
with open(file_path, "w", encoding="utf-8") as file:
    file.write(html_content)

file_path