# sortsmart-
codes for hackathon 

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SortSmart | Waste, Sorted</title>

<style>
:root {
  --green: #26734f;
  --dark: #123d2d;
  --light: #eaf4ed;
  --bg: #f5f7f3;
  --text: #17241d;
  --muted: #68766d;
}

* { box-sizing: border-box; }

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: var(--bg);
  color: var(--text);
}

header {
  background: var(--dark);
  color: white;
  padding: 22px;
  text-align: center;
}

header h1 { margin: 0; font-size: 30px; }
header p { margin-bottom: 0; color: #d5e5da; }

nav {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 10px;
  padding: 15px;
  background: white;
  position: sticky;
  top: 0;
}

nav button, .btn {
  border: 0;
  border-radius: 9px;
  padding: 11px 16px;
  background: var(--green);
  color: white;
  cursor: pointer;
  font-weight: bold;
}

nav button:hover, .btn:hover { opacity: .85; }

main {
  max-width: 1100px;
  margin: auto;
  padding: 25px;
}

.page { display: none; }
.page.active { display: block; }

h2 { color: var(--dark); }

.card {
  background: white;
  border: 1px solid #e1e9e2;
  border-radius: 15px;
  padding: 20px;
  margin: 15px 0;
}

.stats {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 15px;
}

.stat {
  background: var(--light);
  padding: 20px;
  border-radius: 12px;
  text-align: center;
}

.stat strong {
  display: block;
  font-size: 30px;
  color: var(--green);
}

input, select {
  width: 100%;
  padding: 12px;
  margin: 9px 0;
  border: 1px solid #ccd8ce;
  border-radius: 8px;
}

.result {
  background: var(--light);
  padding: 18px;
  border-radius: 12px;
  margin-top: 15px;
  display: none;
}

.item-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 12px;
}

.item-card {
  background: white;
  padding: 15px;
  border: 1px solid #dce6de;
  border-radius: 12px;
  cursor: pointer;
}

.item-card:hover { border-color: var(--green); }

.notice {
  background: #fff3d9;
  padding: 12px;
  border-radius: 8px;
  color: #72501b;
  margin-top: 12px;
}

.activity {
  border-bottom: 1px solid #e1e9e2;
  padding: 12px 0;
}

footer {
  text-align: center;
  padding: 25px;
  color: var(--muted);
  font-size: 13px;
}

@media(max-width:700px) {
  .stats { grid-template-columns: repeat(2, 1fr); }
  .item-grid { grid-template-columns: repeat(2, 1fr); }
  main { padding: 15px; }
}

@media(max-width:420px) {
  .item-grid { grid-template-columns: 1fr; }
}
</style>
</head>

<body>

<header>
  <h1>♻ SortSmart</h1>
  <p>Waste less. Sort better.</p>
</header>

<nav>
  <button onclick="showPage('dashboard')">Dashboard</button>
  <button onclick="showPage('sort')">Sort an Item</button>
  <button onclick="showPage('guide')">Sorting Guide</button>
  <button onclick="showPage('report')">Impact Report</button>
</nav>

<main>

<!-- DASHBOARD -->
<section id="dashboard" class="page active">
  <h2>Welcome to SortSmart</h2>
  <p>Make smarter decisions about everyday waste.</p>

  <div class="stats">
    <div class="stat">
      <strong id="total">0</strong>
      Items Sorted
    </div>
    <div class="stat">
      <strong id="types">0</strong>
      Item Types
    </div>
    <div class="stat">
      <strong id="week">0</strong>
      This Week
    </div>
    <div class="stat">
      <strong id="special">0</strong>
      Special Waste
    </div>
  </div>

  <div class="card">
    <h2>Recent Activity</h2>
    <div id="recent">No activity yet.</div>
  </div>

  <div class="card">
    <h2>Daily Challenge</h2>
    <p>Try to record five items that you have actually sorted today.</p>
    <h2 id="daily">0 / 5</h2>
  </div>
</section>

<!-- SORT AN ITEM -->
<section id="sort" class="page">
  <h2>Where does your waste belong?</h2>
  <p>Search for an item to get sorting instructions.</p>

  <div class="card">
    <label>Optional item photo</label>
    <input type="file" id="photo" accept="image/*">
    <img id="preview" style="display:none;max-width:100%;max-height:220px;">

    <label>Enter item name</label>
    <input id="search" placeholder="Example: plastic bottle, battery">

    <button class="btn" onclick="findItem()">Get Sorting Guidance</button>

    <div id="result" class="result"></div>
  </div>

  <div class="notice">
    Never open, crush, burn, or dismantle batteries or electronics.
    Use authorised collection points for special waste.
  </div>
</section>

<!-- SORTING GUIDE -->
<section id="guide" class="page">
  <h2>Waste Sorting Guide</h2>
  <input id="guideSearch" placeholder="Search waste categories..." oninput="renderGuide()">
  <div id="items" class="item-grid"></div>
</section>

<!-- IMPACT REPORT -->
<section id="report" class="page">
  <h2>Your Impact Report</h2>

  <div class="stats">
    <div class="stat">
      <strong id="reportTotal">0</strong>
      Total Logged
    </div>
    <div class="stat">
      <strong id="reportTypes">0</strong>
      Unique Items
    </div>
    <div class="stat">
      <strong id="reportSpecial">0</strong>
      Special Waste
    </div>
    <div class="stat">
      <strong id="reportToday">0</strong>
      Today
    </div>
  </div>

  <div class="card">
    <h2>Activity History</h2>
    <div id="history"></div>
    <button class="btn" onclick="exportCSV()">Export CSV</button>
    <button class="btn" onclick="clearLog()">Clear History</button>
  </div>

  <div class="notice">
    These figures represent user-reported sorting actions.
    They are not verified recycling or environmental savings.
  </div>
</section>

</main>

<footer>
  SortSmart — Waste, sorted. ♻
  <br>Making better waste decisions, one item at a time.
</footer>

<script>
const wasteItems = [
  {
    name: "Food scraps",
    keys: ["food", "banana peel", "vegetable peel"],
    category: "Wet / Organic Waste",
    bin: "Separate wet waste",
    special: false,
    steps: [
      "Put food scraps in the wet waste container.",
      "Keep them separate from dry recyclables.",
      "Use a suitable compost system if available."
    ]
  },
  {
    name: "Plastic bottle",
    keys: ["plastic", "water bottle", "pet bottle"],
    category: "Dry Recyclable",
    bin: "Clean, dry plastic collection",
    special: false,
    steps: [
      "Empty the bottle.",
      "Rinse it if needed.",
      "Check local plastic recycling rules."
    ]
  },
  {
    name: "Paper and cardboard",
    keys: ["paper", "newspaper", "cardboard", "box"],
    category: "Dry Recyclable",
    bin: "Paper collection",
    special: false,
    steps: [
      "Keep paper clean and dry.",
      "Flatten cardboard boxes.",
      "Avoid mixing oily or heavily soiled paper."
    ]
  },
  {
    name: "Glass bottle",
    keys: ["glass", "jar"],
    category: "Recyclable where collection exists",
    bin: "Designated glass collection",
    special: false,
    steps: [
      "Empty the bottle.",
      "Keep it separate.",
      "Avoid breaking the glass."
    ]
  },
  {
    name: "Metal can",
    keys: ["metal", "aluminium", "tin can"],
    category: "Dry Recyclable",
    bin: "Metal collection",
    special: false,
    steps: [
      "Empty the can.",
      "Rinse if necessary.",
      "Check local metal recycling rules."
    ]
  },
  {
    name: "Battery",
    keys: ["battery", "cell", "phone battery"],
    category: "Special Waste",
    bin: "Authorised battery collection",
    special: true,
    steps: [
      "Keep batteries separate from household waste.",
      "Never open, burn, crush or puncture them.",
      "Take them to an authorised collection point."
    ]
  },
  {
    name: "Electronic waste",
    keys: ["phone", "charger", "laptop", "earphones", "electronics"],
    category: "Special E-Waste",
    bin: "Authorised e-waste collection",
    special: true,
    steps: [
      "Keep electronics separate from ordinary waste.",
      "Use an authorised e-waste collection service.",
      "Remove personal data from working devices."
    ]
  },
  {
    name: "Light bulb",
    keys: ["bulb", "lamp", "tube light"],
    category: "Special Collection",
    bin: "Designated collection",
    special: true,
    steps: [
      "Keep bulbs separate.",
      "Avoid breaking them.",
      "Ask an authorised collector about disposal."
    ]
  },
  {
    name: "Clothing",
    keys: ["clothes", "shirt", "fabric", "shoes"],
    category: "Reuse First",
    bin: "Donation or textile collection",
    special: false,
    steps: [
      "Repair or reuse wearable clothing.",
      "Donate clean items where accepted.",
      "Look for textile collection programmes."
    ]
  },
  {
    name: "Plastic wrapper",
    keys: ["wrapper", "snack packet", "milk packet"],
    category: "Check Local Acceptance",
    bin: "Local dry-waste guidance",
    special: false,
    steps: [
      "Empty the packaging.",
      "Check whether local collectors accept it.",
      "Follow local residual-waste guidance if not accepted."
    ]
  },
  {
    name: "Sanitary waste",
    keys: ["sanitary pad", "diaper", "mask"],
    category: "Special Handling",
    bin: "Follow local sanitary-waste rules",
    special: true,
    steps: [
      "Wrap it as instructed by your local waste service.",
      "Keep it separate from recyclable materials.",
      "Use designated collection where provided."
    ]
  },
  {
    name: "Wood",
    keys: ["wood", "timber", "wooden box"],
    category: "Reuse / Bulky Waste",
    bin: "Reuse or designated collection",
    special: false,
    steps: [
      "Reuse or repair usable wooden items.",
      "Separate treated wood from clean wood.",
      "Ask about bulky waste collection."
    ]
  }
];

const STORAGE_KEY = "sortsmart_activity";
let activity = loadActivity();

function loadActivity() {
  try {
    return JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
  } catch {
    return [];
  }
}

function saveActivity() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(activity));
  updateDashboard();
  updateReport();
}

function showPage(id) {
  document.querySelectorAll(".page").forEach(page => {
    page.classList.remove("active");
  });
  document.getElementById(id).classList.add("active");

  if (id === "report") updateReport();
}

function findWasteItem(query) {
  query = query.trim().toLowerCase();

  return wasteItems.find(item =>
    item.name.toLowerCase().includes(query) ||
    item.keys.some(key => key.includes(query) || query.includes(key))
  );
}

function findItem() {
  const query = document.getElementById("search").value;
  const item = findWasteItem(query);
  const result = document.getElementById("result");

  result.style.display = "block";

  if (!item) {
    result.innerHTML =
      "<h3>No confident match found.</h3><p>Try another item name or browse the sorting guide.</p>";
    return;
  }

  result.innerHTML = `
    <h3>${item.name}</h3>
    <p><b>Category:</b> ${item.category}</p>
    <p><b>Where it belongs:</b> ${item.bin}</p>
    <ul>${item.steps.map(step => `<li>${step}</li>`).join("")}</ul>
    <button class="btn" id="logButton">I sorted this item</button>
  `;

  document.getElementById("logButton").onclick = () => {
    activity.unshift({
      name: item.name,
      category: item.category,
      special: item.special,
      date: new Date().toISOString()
    });

    saveActivity();
    alert("Item added to your activity log!");
  };
}

function renderGuide() {
  const query = document.getElementById("guideSearch").value.toLowerCase();
  const container = document.getElementById("items");

  const filtered = wasteItems.filter(item =>
    (item.name + item.category + item.keys.join(" "))
      .toLowerCase().includes(query)
  );

  container.innerHTML = filtered.map(item => `
    <div class="item-card" onclick="selectItem('${item.name}')">
      <h3>${item.name}</h3>
      <p>${item.category}</p>
      <small>${item.bin}</small>
    </div>
  `).join("") || "<p>No matching items found.</p>";
}

function selectItem(name) {
  showPage("sort");
  document.getElementById("search").value = name;
  findItem();
}

function updateDashboard() {
  const today = new Date().toDateString();
  const weekAgo = Date.now() - 7 * 86400000;

  document.getElementById("total").textContent = activity.length;

  document.getElementById("types").textContent =
    new Set(activity.map(x => x.name)).size;

  document.getElementById("week").textContent =
    activity.filter(x => new Date(x.date).getTime() >= weekAgo).length;

  document.getElementById("special").textContent =
    activity.filter(x => x.special).length;

  const todayCount = activity.filter(
    x => new Date(x.date).toDateString() === today
  ).length;

  document.getElementById("daily").textContent =
    Math.min(todayCount, 5) + " / 5";

  document.getElementById("recent").innerHTML =
    activity.slice(0, 5).map(x => `
      <div class="activity">
        <b>${x.name}</b>
        <br><small>${new Date(x.date).toLocaleString()}</small>
      </div>
    `).join("") || "No activity yet.";
}

function updateReport() {
  const today = new Date().toDateString();

  document.getElementById("reportTotal").textContent = activity.length;

  document.getElementById("reportTypes").textContent =
    new Set(activity.map(x => x.name)).size;

  document.getElementById("reportSpecial").textContent =
    activity.filter(x => x.special).length;

  document.getElementById("reportToday").textContent =
    activity.filter(x => new Date(x.date).toDateString() === today).length;

  document.getElementById("history").innerHTML =
    activity.map(x => `
      <div class="activity">
        <b>${x.name}</b>
        <br>${x.category}
        <br><small>${new Date(x.date).toLocaleString()}</small>
      </div>
    `).join("") || "No activity recorded yet.";
}

function exportCSV() {
  const rows = [
    ["Item", "Category", "Special Waste", "Date"],
    ...activity.map(x => [
      x.name,
      x.category,
      x.special ? "Yes" : "No",
      x.date
    ])
  ];

  const csv = rows.map(row =>
    row.map(value =>
      '"' + String(value).replace(/"/g, '""') + '"'
    ).join(",")
  ).join("\n");

  const blob = new Blob([csv], {type: "text/csv"});
  const link = document.createElement("a");

  link.href = URL.createObjectURL(blob);
  link.download = "sortsmart-activity.csv";
  link.click();

  URL.revokeObjectURL(link.href);
}

function clearLog() {
  if (confirm("Are you sure you want to clear your activity history?")) {
    activity = [];
    saveActivity();
  }
}

document.getElementById("photo").addEventListener("change", event => {
  const file = event.target.files[0];
  if (!file) return;

  const preview = document.getElementById("preview");
  preview.src = URL.createObjectURL(file);
  preview.style.display = "block";
});

renderGuide();
updateDashboard();
updateReport();
</script>

</body>
</html>
