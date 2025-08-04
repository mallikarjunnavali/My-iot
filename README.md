<!DOCTYPE html>
<html>
<head>
  <title>Dust Sensor Dashboard</title>
  <meta charset="UTF-8">
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f2f2;
      text-align: center;
      padding: 50px;
    }
    .card {
      background: white;
      padding: 30px;
      margin: auto;
      width: 300px;
      border-radius: 10px;
      box-shadow: 0 0 10px #ccc;
    }
    .value {
      font-size: 48px;
      font-weight: bold;
      color: #333;
    }
    .status {
      font-size: 18px;
      margin-top: 10px;
    }
  </style>
</head>
<body>

  <h1>🌫 Dust Sensor Monitor</h1>
  <div class="card">
    <div>Dust Level:</div>
    <div id="dustValue" class="value">--</div>
    <div id="dustStatus" class="status">Loading...</div>
  </div>

  <script>
    const firebaseUrl = "https://iot-project-57452-default-rtdb.firebaseio.com/dustSensor/dust.json";

    async function fetchDustData() {
      try {
        const response = await fetch(firebaseUrl);
        const dust = await response.json();

        document.getElementById("dustValue").textContent = dust;
        let statusText = "";

        if (dust > 400) {
          statusText = "⚠ Poor Air Quality";
          document.body.style.backgroundColor = "#ffcccc";
        } else {
          statusText = "✅ Air Quality Normal";
          document.body.style.backgroundColor = "#ccffcc";
        }

        document.getElementById("dustStatus").textContent = statusText;

      } catch (error) {
        console.error("Error fetching data:", error);
        document.getElementById("dustStatus").textContent = "❌ Failed to load data.";
      }
    }

    fetchDustData();
    setInterval(fetchDustData, 7000); // Refresh every 7 seconds
  </script>

</body>
</html>
