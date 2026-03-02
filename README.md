# Double-Link-cheekar-premium-System-github.io
double name cheek
<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8">
  <title>Double Name Cheekar Premium tools</title>
  <style>
    body { 
      font-family: sans-serif; 
      padding: 20px; 
      background: linear-gradient(135deg, #0d1117, #1a1f2e); 
      color: #e6edf3; 
    }
    h2 { 
      color: #ff9f1c; 
      text-align: center; 
      text-shadow: 1px 1px 3px #000; 
    }
    textarea { 
      width: 100%; 
      height: 180px; 
      background: #0d1117; 
      color: #f1f1f1; 
      border: 1px solid #444c56; 
      border-radius: 8px; 
      padding: 12px; 
      font-family: monospace; 
    }
    button { 
      background: linear-gradient(90deg, #ff006e, #fb5607); 
      color: white; 
      border: none; 
      padding: 10px 16px; 
      margin-top: 10px; 
      border-radius: 8px; 
      cursor: pointer; 
      font-size: 14px; 
      font-weight: bold; 
      transition: 0.3s; 
    }
    button:hover { 
      background: linear-gradient(90deg, #8338ec, #3a86ff); 
      transform: scale(1.05); 
    }
    #result, #activity { 
      margin-top: 20px; 
      background: #0d1117; 
      padding: 15px; 
      border-radius: 10px; 
      border: 1px solid #444c56; 
    }
    .dup-item { 
      margin-bottom: 15px; 
      padding: 12px; 
      background: linear-gradient(135deg, #1f2937, #2a2f45); 
      border-radius: 8px; 
      border-left: 5px solid #ff006e; 
      box-shadow: 0 0 10px rgba(255,0,110,0.4); 
    }
    .name { font-weight: bold; color: #3a86ff; font-size: 18px; }
    .count { color: #ffbe0b; font-size: 14px; }
    .numbers { color: #fb5607; font-size: 14px; display: block; margin-top: 5px; }
    .activity-item { 
      margin-bottom: 20px; 
      border-bottom: 1px dashed #444c56; 
      padding-bottom: 10px; 
      background: #1a1f2e; 
      border-radius: 8px; 
      padding: 10px; 
    }
    .time { font-size: 13px; color: #ffbe0b; margin-bottom: 5px; font-weight:bold; }
    .list-text { white-space: pre-wrap; font-family: monospace; color: #e6edf3; }
    .copy-btn, .delete-btn {
      background: linear-gradient(90deg, #3a86ff, #8338ec);
      color: white;
      border: none;
      padding: 6px 12px;
      border-radius: 6px;
      cursor: pointer;
      font-size: 14px;
      margin-top: 8px;
      margin-right: 8px;
    }
    .copy-btn:hover {
      background: linear-gradient(90deg, #ff006e, #fb5607);
    }
    .delete-btn {
      background: linear-gradient(90deg, #ff0000, #ff7f50);
    }
    .delete-btn:hover {
      background: linear-gradient(90deg, #fb5607, #8338ec);
    }
  </style>
</head>
<body>
  <h2>🌈 Double Name Cheekar </h2>
  <textarea id="input" placeholder="এখানে নামের লিস্ট পেস্ট করুন..."></textarea>
  <div style="display:flex; justify-content:space-between; gap:2%;">
    <button onclick="check()">✨ Check List</button>
    <button onclick="showActivity()">📱 Activity</button>
  </div>
  <div id="result"></div>
  <div id="activity" style="display:none;"></div>

<script>
let activityData = JSON.parse(localStorage.getItem("activityData")) || [];

function check() {
    const text = document.getElementById('input').value.trim();
    if (!text) { alert("দয়া করে লিস্ট দিন!"); return; }

    const lines = text.split('\n');
    const nameToPos = new Map();

    lines.forEach(line => {
        const lineTrim = line.trim();
        if (!lineTrim) return;

        if (lineTrim.includes('➤')) {
            const parts = lineTrim.split('➤');
            const numberPart = parts[0].trim();   // সিরিয়াল নম্বর
            const namePart = parts[1].trim();     // নাম

            if (!nameToPos.has(namePart)) {
                nameToPos.set(namePart, []);
            }
            nameToPos.get(namePart).push(numberPart);
        }
    });

    let output = "<h3 style='color:#ff9f1c'>ডুপ্লিকেট ফলাফল:</h3>";
    let found = false;

    nameToPos.forEach((positions, name) => {
        if (positions.length > 1) {
            found = true;
            output += `
                <div class="dup-item">
                    <span class="name">${name}</span>
                    <span class="count"> — ${positions.length} বার </span>
                    <span class="numbers">পজিশন: ${positions.join(', ')}</span>
                </div>`;
        }
    });

    const finalResult = found ? output : "✅ কোনো ডুপ্লিকেট নাম পাওয়া যায়নি!";
    document.getElementById('result').innerHTML = finalResult;

    // Activity তে স্থায়ীভাবে সংরক্ষণ (Local Storage সহ)
    const now = new Date();
    const options = { weekday: 'long', year: 'numeric', month: '2-digit', day: '2-digit', hour:'2-digit', minute:'2-digit', second:'2-digit' };
    const formatted = now.toLocaleDateString('bn-BD', options);

    activityData.push({time: formatted, list: text, result: finalResult});
    localStorage.setItem("activityData", JSON.stringify(activityData));
}

function showActivity() {
    if (activityData.length === 0) {
        document.getElementById('activity').innerHTML = "ℹ️ এখনো কোনো Activity নেই!";
    } else {
        let activityOutput = "<h3 style='color:#ff9f1c'>📱 আপনার Activity:</h3>";
        activityData.forEach((item, index) => {
            activityOutput += `
                <div class="activity-item">
                    <div class="time">সময়/তারিখ/বার: ${item.time}</div>
                    <div class="list-text">${item.list}</div>
                    <button class="copy-btn" onclick="copyList(${index})">📋 Copy List</button>
                    <button class="delete-btn" onclick="deleteActivity(${index})">🗑️ Delete</button>
                    <div>${item.result}</div>
                </div>`;
        });
        document.getElementById('activity').innerHTML = activityOutput;
    }
    document.getElementById('activity').style.display = "block";
}

function copyList(index) {
    const textToCopy = activityData[index].list;
    navigator.clipboard.writeText(textToCopy).then(() => {
        alert("✅ লিস্ট কপি হয়ে গেছে!");
    });
}

function deleteActivity(index) {
    if (confirm("⚠️ আপনি কি এই Activity মুছে ফেলতে চান?")) {
        activityData.splice(index, 1);
        localStorage.setItem("activityData", JSON.stringify(activityData));
        showActivity();
    }
}
</script>
</body>
</html>
