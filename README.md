- 👋 Hi, I’m @hopdot
- 👀 I’m interested in Machine Learning and AI.
- 🌱 I’m currently learning the use of multiple language integration.
- 💞️ I’m looking to collaborate on any project.
- 📫 incfreshstart@gmail.com!
[Screen_recording_20240312_164123.webm](https://github.com/hopdot/hopdot/assets/107817209/b8d87416-6447-42c7-b70e-ae2c95204357)


<!---
hopdot/hopdot is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Economic Forecasting Dashboard</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            text-align: center;
        }
        h1 {
            color: #333;
        }
        #chart-container {
            width: 80%;
            margin: auto;
        }
        table {
            width: 80%;
            margin: auto;
            border-collapse: collapse;
            background-color: white;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
        }
        th, td {
            padding: 12px;
            border: 1px solid #ddd;
            text-align: center;
        }
        th {
            background-color: #007bff;
            color: white;
        }
        .download-btn {
            display: inline-block;
            margin-top: 20px;
            padding: 10px 20px;
            background-color: #28a745;
            color: white;
            text-decoration: none;
            border-radius: 5px;
            cursor: pointer;
        }
    </style>
</head>
<body>

    <h1>Economic Forecasting Dashboard</h1>

    <select id="sectorSelect">
        <option value="Technology">Technology</option>
        <option value="Healthcare">Healthcare</option>
        <option value="Finance">Finance</option>
        <option value="Real Estate">Real Estate</option>
        <option value="Retail">Retail</option>
        <option value="Manufacturing">Manufacturing</option>
    </select>

    <button onclick="fetchEconomicData()">Predict</button>

    <div id="chart-container">
        <canvas id="economicChart"></canvas>
    </div>

    <table>
        <tr>
            <th>Year</th>
            <th>Projected Growth (%)</th>
        </tr>
        <tbody id="economic-data-table"></tbody>
    </table>

    <a id="downloadBtn" class="download-btn">Download Report</a>

    <script>
        async function fetchEconomicData() {
            const sector = document.getElementById("sectorSelect").value;
            const response = await fetch(`https://api.worldbank.org/v2/country/global/indicator/NY.GDP.MKTP.KD.ZG?format=json`);
            const data = await response.json();
            
            let years = [];
            let growthRates = [];
            let tableBody = "";

            data[1].slice(0, 10).forEach(entry => {
                years.push(entry.date);
                growthRates.push(entry.value);
                tableBody += `<tr><td>${entry.date}</td><td>${entry.value.toFixed(2)}%</td></tr>`;
            });

            document.getElementById("economic-data-table").innerHTML = tableBody;

            const ctx = document.getElementById('economicChart').getContext('2d');
            new Chart(ctx, {
                type: 'line',
                data: {
                    labels: years.reverse(),
                    datasets: [{
                        label: `Projected Growth - ${sector}`,
                        data: growthRates.reverse(),
                        borderColor: 'blue',
                        fill: false
                    }]
                },
                options: {
                    responsive: true,
                    scales: {
                        y: { beginAtZero: true }
                    }
                }
            });
        }

        document.getElementById("downloadBtn").addEventListener("click", function() {
            const content = document.documentElement.outerHTML;
            const blob = new Blob([content], { type: "text/html" });
            const url = URL.createObjectURL(blob);
            const a = document.createElement("a");
            a.href = url;
            a.download = "Economic_Predictions.html";
            a.click();
            URL.revokeObjectURL(url);
        });
    </script>

</body>
</html>
