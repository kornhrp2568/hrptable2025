<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <title>ระบบจองโต๊ะจีน</title>
  <style>
    body { font-family: Arial, sans-serif; text-align: center; margin: 20px; }
    h1 { color: #b22222; }
    .form-group { margin: 10px 0; }
    input { padding: 8px; width: 200px; }
    button { padding: 10px 20px; font-size: 16px; cursor: pointer; }
    .tables { margin-top: 30px; display: grid; grid-template-columns: repeat(5,1fr); gap: 10px; max-width: 800px; margin: auto; }
    .table { padding: 15px; background: #eee; border: 1px solid #ccc; cursor: pointer; }
    .table.booked { background: #f08080; color: #fff; }
    .result { margin-top: 20px; }
  </style>
</head>
<body>

  <h1>ระบบจองโต๊ะจีน</h1>

  <div class="form-group">
    <input id="nameInput" type="text" placeholder="ชื่อผู้จอง">
  </div>
  <div class="form-group">
    <input id="phoneInput" type="text" placeholder="เบอร์โทรศัพท์">
  </div>

  <div class="tables" id="tableContainer"></div>

  <div class="result" id="resultBox"></div>

<script>
  // **เปลี่ยนตรงนี้ให้เป็น Web App URL ของคุณ**
  const webAppUrl = "[AKfycbwrmfUBGqJKfaAI1rQCYyf8Ea9s2kMh3pDaG9NnQQ0](https://script.google.com/macros/s/AKfycbwFEBiqPqL-1nHNdZTZpU3-auxGmeM3SrVWwHQ38ME449Y22CU7JLXbqqJ-2yjy3Tv7/exec)";

  const totalTables = 50;
  const vipTables = [3,8,13];
  const bookings = {};

  const container = document.getElementById("tableContainer");
  const resultBox = document.getElementById("resultBox");

  // สร้างกริดโต๊ะ
  for (let i = 1; i <= totalTables; i++) {
    const div = document.createElement("div");
    div.className = "table" + (vipTables.includes(i) ? " vip" : "");
    div.textContent = `โต๊ะ ${i}`;
    div.dataset.no = i;
    div.onclick = () => reserve(i, div);
    container.appendChild(div);
  }

  function reserve(tableNo, el) {
    const name = document.getElementById("nameInput").value.trim();
    const phone = document.getElementById("phoneInput").value.trim();
    if (!name || !phone) return alert("กรุณากรอกชื่อและเบอร์โทรให้ครบ");
    fetch(webAppUrl, {
      method: "POST",
      body: JSON.stringify({ name, phone, table: tableNo }),
      headers: { "Content-Type": "application/json" }
    })
    .then(r=>r.text())
    .then(_=>{
      bookings[tableNo] = name;
      el.classList.add("booked");
      showResult();
    })
    .catch(e=>alert("ไม่สามารถจองได้: "+e));
  }

  function showResult() {
    const list = Object.keys(bookings)
      .sort((a,b)=>a-b)
      .map(n=>`โต๊ะ ${n}: ${bookings[n]}`)
      .join("<br>");
    resultBox.innerHTML = list || "ยังไม่มีการจอง";
  }
</script>

</body>
</html>
