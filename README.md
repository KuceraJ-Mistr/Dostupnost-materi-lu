<!DOCTYPE html>
<html lang="cs">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Skladový systém</title>
  <style>
  body { font-family: Arial, sans-serif; text-align: center; }
  .box-container { display: grid; grid-template-columns: repeat(10, 1fr); gap: 10px; margin: 20px; }
  .box { padding: 10px; border: 2px solid #000; cursor: pointer; }
  .free { background-color: lightgray; }
  .occupied { background-color: lightgreen; }
  .waiting { background-color: lightcoral; }
  .modal { display: none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); background-color: white; padding: 20px; border: 2px solid #000; }
  .modal.active { display: block; }
  .modal input { display: block; margin: 10px 0; }
  </style>
</head>
<body>
  <h1>Skladový systém</h1>
  <div id="boxContainer" class="box-container"></div>
 
  <div id="modal" class="modal">
  <h2>Vyplňte údaje</h2>
  <input type="text" id="orderNumber" placeholder="Číslo zakázky">
  <input type="text" id="textileType" placeholder="Druh textilu">
  <input type="date" id="exportDate" placeholder="Datum vývozu">
  <input type="text" id="exporter" placeholder="Jméno vývozce">
  <button onclick="saveData()">Uložit</button>
  </div>
 
  <script>
  const rows = ['A', 'B', 'C'];
  const cols = 10;
  const boxContainer = document.getElementById("boxContainer");
  let storage = JSON.parse(localStorage.getItem("warehouse")) || {};
 
  function updateStorage() {
  localStorage.setItem("warehouse", JSON.stringify(storage));
  }
 
  function createBox(row, col) {
  const box = document.createElement("div");
  const id = row + col;
  box.className = "box " + (storage[id]?.status || "free");
  box.textContent = id;
  box.onclick = () => openModal(id, box);
  boxContainer.appendChild(box);
  }
 
  function openModal(id, box) {
  const modal = document.getElementById("modal");
  modal.classList.add("active");
  modal.dataset.id = id;
  document.getElementById("orderNumber").value = storage[id]?.orderNumber || '';
  document.getElementById("textileType").value = storage[id]?.textileType || '';
  document.getElementById("exportDate").value = storage[id]?.exportDate || '';
  document.getElementById("exporter").value = storage[id]?.exporter || '';
  }
 
  function saveData() {
  const modal = document.getElementById("modal");
  const id = modal.dataset.id;
  storage[id] = {
  status: "occupied",
  orderNumber: document.getElementById("orderNumber").value,
  textileType: document.getElementById("textileType").value,
  exportDate: document.getElementById("exportDate").value,
  exporter: document.getElementById("exporter").value
  };
  updateStorage();
  modal.classList.remove("active");
  document.location.reload();
  }
 
  rows.forEach(row => {
  for (let i = 1; i <= cols; i++) {
  createBox(row, i);
  }
  });
  </script>
</body>
</html>
 
