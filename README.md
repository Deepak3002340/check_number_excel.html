https://yourusername.github.io/check-number-app/
# check_number_excel.html 

<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <title>नंबर खोजें</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 30px;
      background: #f7f7f7;
    }
    .card {
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      max-width: 600px;
      margin: auto;
    }
    input, textarea, button {
      width: 100%;
      margin: 10px 0;
      padding: 10px;
      font-size: 16px;
    }
    button {
      background: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
      border-radius: 5px;
    }
    button:hover {
      background: #45a049;
    }
    .result {
      margin-top: 15px;
      padding: 10px;
      background: #e9ffe9;
      border: 1px solid #b2ffb2;
      border-radius: 5px;
    }
  </style>
</head>
<body>
  <div class="card">
    <h2>📞 नंबर किसके पास है?</h2>

    <label>Contacts CSV Upload करें (Format: Name,Number)</label>
    <input type="file" id="fileInput" accept=".csv">

    <label>या फिर JSON Contacts डालें:</label>
    <textarea id="contactsInput" rows="6">
{
  "Amit": ["9876543210", "9235189044"],
  "Rohit": ["9235189044"],
  "Sita": ["8888888888"],
  "Geeta": ["9999999999", "7777777777"]
}
    </textarea>

    <label>खोजने वाला नंबर:</label>
    <input type="text" id="numberInput" placeholder="उदाहरण: 9235189044">

    <button onclick="checkNumber()">Check करें</button>

    <div id="result" class="result" style="display:none;"></div>
  </div>

  <script>
    let contactsData = {};

    // CSV File पढ़ना
    document.getElementById("fileInput").addEventListener("change", function(event) {
      const file = event.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = function(e) {
        const text = e.target.result;
        contactsData = parseCSV(text);
        alert("✅ Contacts CSV से लोड हो गए!");
      };
      reader.readAsText(file);
    });

    // CSV से contacts object बनाना
    function parseCSV(csvText) {
      const lines = csvText.split("\n");
      const data = {};
      for (let line of lines) {
        const [name, number] = line.split(",").map(s => s.trim());
        if (name && number) {
          if (!data[name]) data[name] = [];
          data[name].push(number);
        }
      }
      return data;
    }

    function checkNumber() {
      let contacts;

      // अगर CSV upload हुआ है तो वही यूज़ करें
      if (Object.keys(contactsData).length > 0) {
        contacts = contactsData;
      } else {
        try {
          contacts = JSON.parse(document.getElementById("contactsInput").value);
        } catch (e) {
          alert("❌ Contacts JSON सही नहीं है!");
          return;
        }
      }

      const number = document.getElementById("numberInput").value.trim();
      let foundIn = [];

      for (let name in contacts) {
        if (contacts[name].includes(number)) {
          foundIn.push(name);
        }
      }

      const resultDiv = document.getElementById("result");
      if (foundIn.length > 0) {
        resultDiv.innerHTML = `✅ यह नंबर इन लोगों के पास है: <b>${foundIn.join(", ")}</b>`;
      } else {
        resultDiv.innerHTML = "❌ यह नंबर किसी के पास नहीं मिला।";
      }
      resultDiv.style.display = "block";
    }
  </script>
</body>
</html>
