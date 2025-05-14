<meta name='viewport' content='width=device-width, initial-scale=1'/><!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Цооногийн мэдээлэл хайх</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    table, th, td { border: 1px solid black; border-collapse: collapse; padding: 5px; }
    table { margin-top: 20px; width: 100%; }
    textarea { width: 100%; height: 60px; }
  </style>
</head>
<body>

<h3>Цооногийн дугааруудаар шүүлт хийх</h3>
<p>Доорх талбарт цооногийн дугааруудаа оруулаад "Шүүх" товч дарна уу (нэг мөрөнд нэг дугаар, хоёрдох таталтыг бичихдээ: -/2, жишээ: T19-218-210-/2):</p>

<textarea id="searchInput"></textarea>
<br>
<button onclick="filterWells()">Шүүх</button>

<table id="dataTable">
  <thead>
    <tr>
      <th>Цооногийн дугаар</th>
      <th>Хүрсэн гүн</th>
      <th>Татсан гүн</th>
      <th>Нийт шингэн</th>
      <th>Газрын тос</th>
      <th>Ус</th>
    </tr>
  </thead>
  <tbody id="dataBody">
    <!-- Дата энд автоматаар нэмэгдэнэ -->
  </tbody>
</table>

<script>
const data = [
 { num: 'T19-130', depth: 1050, pumped: 1300, total: 2.5, oil: 1.5, water: 1 },
{ num: 'T19-204-205', depth: 1700, pumped: 1950, total: 2.5, oil: 1.5, water: 1 },
{ num: 'T19-218-210', depth: 250, pumped: 500, total: 2.5, oil: 1.5, water: 1 },
{ num: 'T19-218-210-/2', depth: 400, pumped: 600, total: 2, oil: 0, water: 2 },
{ num: 'T19-218-210-/3', depth: 500, pumped: 700, total: 2, oil: 0, water: 2 },
{ num: 'T19-46-1', depth: 550, pumped: 800, total: 2.5, oil: 1, water: 1.5 },
{ num: 'T19-46-3', depth: 1050, pumped: 1300, total: 2.5, oil: 2, water: 0.5 },
{ num: 'T19-51', depth: 2300, pumped: 2500, total: 2, oil: 2, water: 0 },
{ num: 'T19-57', depth: 1700, pumped: 1950, total: 2.5, oil: 2, water: 0.5 },
{ num: 'T19-57-1', depth: 500, pumped: 750, total: 2.5, oil: 1, water: 1.5 },
{ num: 'T19-8-1', depth: 1300, pumped: 1550, total: 2.5, oil: 1, water: 1.5 },
{ num: 'T19-8-2', depth: 620, pumped: 870, total: 2.5, oil: 0.8, water: 1.7 },
{ num: 'T19-8-3', depth: 1750, pumped: 2000, total: 2.5, oil: 1.5, water: 1 },
{ num: 'T19-8-4', depth: 1250, pumped: 1500, total: 2.5, oil: 1, water: 1.5 },
{ num: 'T19-98', depth: 1900, pumped: 2100, total: 2, oil: 1.5, water: 0.5 },
{ num: 'T19-98-1', depth: 710, pumped: 960, total: 2.5, oil: 1.2, water: 1.3 },
{ num: 'T19-217-211', depth: 450, pumped: 750, total: 3, oil: 1, water: 2 },
{ num: 'T19-108', depth: 1800, pumped: 2000, total: 2, oil: 1.5, water: 0.5 },
{ num: 'T19-118', depth: 2300, pumped: 2500, total: 2, oil: 2, water: 0 },
{ num: 'T19-99', depth: 2200, pumped: 2400, total: 2, oil: 2, water: 0 },
{ num: 'T19-225-238', depth: 1700, pumped: 1950, total: 2.5, oil: 2, water: 0.5 },
{ num: 'T19-62', depth: 100, pumped: 400, total: 3, oil: 0, water: 3 },
{ num: 'T19-62-1', depth: 50, pumped: 350, total: 3, oil: 0, water: 3 },
{ num: 'T19-65', depth: 300, pumped: 550, total: 2.5, oil: 1, water: 1.5 },
{ num: 'T19-65-1', depth: 1800, pumped: 2050, total: 2.5, oil: 2, water: 0.5 },
{ num: 'T19-170-234', depth: 650, pumped: 900, total: 2.5, oil: 2, water: 0.5 },
{ num: 'T19-213-208', depth: 0, pumped: 250, total: 2.5, oil: 0.8, water: 1.7 },
{ num: 'T19-199-204', depth: 0, pumped: 250, total: 2.5, oil: 1, water: 1.5 },
{ num: 'T19-166-234', depth: 1800, pumped: 2000, total: 2, oil: 2, water: 0 },
{ num: 'T19-197-202', depth: 1900, pumped: 2100, total: 2, oil: 2, water: 0 },
{ num: 'T19-174-166', depth: 500, pumped: 750, total: 2.5, oil: 1, water: 1.5 },
{ num: 'T19-200-205', depth: 850, pumped: 1100, total: 2.5, oil: 1.5, water: 1 },
{ num: 'T19-184-248', depth: 1950, pumped: 2150, total: 2, oil: 2, water: 0 },
{ num: 'T19-211-214', depth: 0, pumped: 250, total: 2.5, oil: 1.5, water: 1 },
{ num: 'T19-186-142', depth: 1720, pumped: 1970, total: 2.5, oil: 1.5, water: 1 },
{ num: 'T19-121', depth: 1000, pumped: 1200, total: 2, oil: 2, water: 0 },
{ num: 'T19-13-1', depth: 0, pumped: 250, total: 2.5, oil: 1, water: 1.5 },
{ num: 'T19-204-194', depth: 1850, pumped: 2050, total: 2, oil: 1.5, water: 0.5 },
{ num: 'T19-207-206', depth: 830, pumped: 1080, total: 2.5, oil: 1.5, water: 1 },
{ num: 'T19-199-201', depth: 0, pumped: 300, total: 3, oil: 0, water: 3 },
{ num: 'A90-50', depth: 50, pumped: 300, total: 2.5, oil: 1, water: 1.5 },
{ num: 'T19-223-238', depth: 1600, pumped: 1850, total: 2.5, oil: 1.5, water: 1 },
{ num: 'T19-86', depth: 1020, pumped: 1270, total: 2.5, oil: 2, water: 0.5 },
{ num: 'T19-226-212', depth: 1520, pumped: 1770, total: 2.5, oil: 1.3, water: 1.2 },
{ num: 'T19-219-208', depth: 600, pumped: 850, total: 2.5, oil: 1, water: 1.5 },
{ num: 'T19-211-211', depth: 0, pumped: 300, total: 3, oil: 0, water: 3 },
{ num: 'T19-15', depth: 1450, pumped: 1700, total: 2.5, oil: 1.5, water: 1 },
{ num: 'T19-230-166', depth: 400, pumped: 700, total: 3, oil: 0.5, water: 2.5 },
{ num: 'T19-232-214', depth: 1900, pumped: 2100, total: 2, oil: 2, water: 0 },
{ num: 'T19-232-216', depth: 1750, pumped: 2000, total: 2.5, oil: 2, water: 0.5 },
{ num: 'T19-254-163', depth: 0, pumped: 300, total: 3, oil: 1, water: 2 },
{ num: 'T19-258-168', depth: 0, pumped: 300, total: 3, oil: 0.5, water: 2.5 },
{ num: 'T19-262-170', depth: 350, pumped: 600, total: 2.5, oil: 1.5, water: 1 },
{ num: 'T19-266-168', depth: 1200, pumped: 1450, total: 2.5, oil: 1.5, water: 1 },
{ num: 'T19-272-173', depth: 1950, pumped: 2100, total: 2.5, oil: 1.5, water: 1 },
{ num: 'T19-274-161', depth: 1600, pumped: 1900, total: 3, oil: 1, water: 2 },
{ num: 'T19-34-3', depth: 0, pumped: 300, total: 3, oil: 0, water: 3 },
{ num: 'T19-117', depth: 1740, pumped: 1990, total: 2.5, oil: 2.5, water: 0 },
{ num: 'T19-119', depth: 1900, pumped: 2150, total: 2.5, oil: 1.8, water: 0.7 },
{ num: 'T19-124', depth: 1700, pumped: 1900, total: 2, oil: 2, water: 0 },
{ num: 'T19-125', depth: 1950, pumped: 2150, total: 2, oil: 1.5, water: 0.5 },
{ num: 'T19-126', depth: 1000, pumped: 1200, total: 2, oil: 1.5, water: 0.5 },
{ num: 'T19-132', depth: 1950, pumped: 2150, total: 2, oil: 1, water: 1 },
{ num: 'T19-250-323', depth: 1300, pumped: 1500, total: 2, oil: 2, water: 0 },
{ num: 'T19-252-321', depth: 760, pumped: 1010, total: 2.5, oil: 2, water: 0.5 },
{ num: 'T19-254-319', depth: 1990, pumped: 2190, total: 2, oil: 2, water: 0 },
{ num: 'T19-255-325', depth: 1000, pumped: 1200, total: 2, oil: 1, water: 1 },
{ num: 'T19-292-303', depth: 2090, pumped: 2300, total: 2, oil: 2, water: 0 },
{ num: 'T19-297-311', depth: 2050, pumped: 2250, total: 2, oil: 0.5, water: 1.5 },
{ num: 'T19-56', depth: 500, pumped: 700, total: 2, oil: 1.5, water: 0.5 },
{ num: 'T19-80', depth: 1700, pumped: 1950, total: 2.5, oil: 1.5, water: 1 },
{ num: 'T19-80-1', depth: 1750, pumped: 2000, total: 2.5, oil: 2, water: 0.5 },
{ num: 'T19-81', depth: 500, pumped: 700, total: 2, oil: 0.5, water: 1.5 },
{ num: 'T19-83', depth: 1950, pumped: 2150, total: 2, oil: 1, water: 1 },
{ num: 'T19-294-306', depth: 700, pumped: 950, total: 2.5, oil: 1, water: 1.5 },
{ num: 'T19-51-3', depth: 1400, pumped: 1650, total: 2.5, oil: 1.5, water: 1.0 },
{ num: 'T19-227-211', depth: 400, pumped: 600, total: 2, oil: 0.5, water: 1.5 },
{ num: 'T19-14', depth: 300, pumped: 550, total: 2.5, oil: 1.5, water: 1.0 },
{ num: 'T19-29', depth: 1430, pumped: 1680, total: 2.5, oil: 1.5, water: 1.0 },
{ num: 'T19-130-/2', depth: 1250, pumped: 1500, total: 2.5, oil: 0, water: 2.5 },
{ num: 'T19-62-/3', depth: 300, pumped: 600, total: 3, oil: 0, water: 3 },
{ num: 'T19-62-/2', depth: 150, pumped: 450, total: 3, oil: 0, water: 3 },
{ num: 'T19-62-1-/2', depth: 200, pumped: 500, total: 3, oil: 0, water: 3 },
{ num: 'T19-62-1-/3', depth: 350, pumped: 650, total: 3, oil: 0, water: 3 },
{ num: 'T19-170-234-/2', depth: 850, pumped: 1050, total: 2, oil: 0, water: 2 },
{ num: 'T19-211-211-/2', depth: 100, pumped: 400, total: 3, oil: 0, water: 3 },
{ num: 'T19-211-211-/3', depth: 250, pumped: 550, total: 3, oil: 0, water: 3 },
{ num: 'T19-254-163-/2', depth: 250, pumped: 550, total: 3, oil: 0, water: 3 },
{ num: 'T19-254-163-/3', depth: 500, pumped: 800, total: 3, oil: 0, water: 3 },
{ num: 'T19-34-3-/2', depth: 100, pumped: 400, total: 3, oil: 0, water: 3 },
{ num: 'T19-34-3-/3', depth: 250, pumped: 550, total: 3, oil: 0, water: 3 },
{ num: 'T19-', depth: 0, pumped: 0, total: 0, oil: 0, water: 0 },
  { num: 'T19-366-246', depth: 0, pumped: 300, total: 3, oil: 0, water: 3 },
{ num: 'T19-366-246-/2', depth: 80, pumped: 380, total: 3, oil: 0, water: 3 },
  { num: 'T19-199-201-/2', depth: 50, pumped: 350, total: 3, oil: 0, water: 3 },
{ num: 'T19-226-210', depth: 150, pumped: 450, total: 3, oil: 0, water: 3 },
{ num: 'T19-226-210-/2', depth: 400, pumped: 700, total: 3, oil: 0, water: 3 },
{ num: 'T19-226-210-/3', depth: 600, pumped: 900, total: 3, oil: 0, water: 3 },
{ num: 'T19-199-201-/3', depth: 120, pumped: 420, total: 3, oil: 0, water: 3 },

];

function filterWells() {
  const input = document.getElementById("searchInput").value
    .split('\n')
    .map(s => s.trim())
    .filter(s => s !== '');

  const tableBody = document.getElementById("dataBody");
  tableBody.innerHTML = '';

  const filtered = data.filter(row => input.includes(row.num));

  for (const row of filtered) {
    const tr = document.createElement("tr");
    tr.innerHTML = `<td>${row.num}</td><td>${row.depth}</td><td>${row.pumped}</td><td>${row.total}</td><td>${row.oil}</td><td>${row.water}</td>`;
    tableBody.appendChild(tr);
  }

  if (filtered.length === 0) {
    tableBody.innerHTML = `<tr><td colspan="6">Хайлтад тохирох үр дүн олдсонгүй.</td></tr>`;
  }
}
</script>

</body>
</html><style>body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: #f5f7fa;
  padding: 30px;
  color: #333;
}

.container {
  max-width: 800px;
  margin: auto;
  background: white;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 8px 16px rgba(0,0,0,0.1);
}

h1 {
  text-align: center;
  color: #2c3e50;
}

input[type="text"] {
  width: 100%;
  padding: 12px;
  margin-top: 10px;
  margin-bottom: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  font-size: 16px;
  transition: border-color 0.3s;
}

input[type="text"]:focus {
  border-color: #3498db;
  outline: none;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  padding: 12px;
  border: 1px solid #ddd;
  text-align: left;
}

th {
  background-color: #3498db;
  color: white;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}

tr:hover {
  background-color: #eaf2f8;
}</style><script><script>
  document.getElementById('search').addEventListener('keyup', function () {
    let searchValue = this.value.toLowerCase();
    let rows = document.querySelectorAll('#data tr');

    rows.forEach(function (row) {
      let rowText = row.textContent.toLowerCase();
      if (rowText.includes(searchValue)) {
        row.style.display = '';
      } else {
        row.style.display = 'none';
      }
    });
  });
</script></script>
