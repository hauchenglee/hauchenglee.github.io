<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dynamic Table</title>
  <style>
    table {
      border-collapse: collapse;
      width: 80%;
      margin: 20px;
    }

    table, th, td {
      border: 1px solid black;
    }

    th, td {
      padding: 10px;
      text-align: center;
    }

    button {
      margin: 10px;
    }
  </style>
</head>
<body>

<table id="dynamicTable">
  <thead>
    <tr>
      <th>序號</th>
      <th>操作</th>
      <th>欄位1</th>
      <th>欄位2</th>
      <th>欄位3</th>
      <th>欄位4</th>
      <th>欄位5</th>
      <th>欄位6</th>
    </tr>
  </thead>
  <tbody>
    <!-- 初始表格只有一行 -->
    <tr>
      <td>1</td>
      <td><button onclick="addRow(this)">新增行</button><button onclick="removeRow(this)">刪除行</button></td>
      <td contenteditable="true"></td>
      <td contenteditable="true"></td>
      <td contenteditable="true"></td>
      <td contenteditable="true"></td>
      <td contenteditable="true"></td>
      <td contenteditable="true"></td>
    </tr>
  </tbody>
</table>

<script>
  // 新增行的事件處理函數
  function addRow(button) {
    var row = button.parentNode.parentNode;
    var newRow = row.cloneNode(true);

    // 清空新行的內容
    var cells = newRow.getElementsByTagName('td');
    for (var i = 2; i < cells.length; i++) {
      cells[i].textContent = '';
    }

    // 插入新行
    row.parentNode.insertBefore(newRow, row.nextSibling);

    // 重新排序序號
    updateSerialNumbers();
  }

  // 刪除行的事件處理函數
  function removeRow(button) {
    var row = button.parentNode.parentNode;

    // 至少保留一行
    if (document.getElementById('dynamicTable').getElementsByTagName('tbody')[0].getElementsByTagName('tr').length > 1) {
      row.parentNode.removeChild(row);

      // 重新排序序號
      updateSerialNumbers();
    }
  }

  // 更新表格序號
  function updateSerialNumbers() {
    var rows = document.getElementById('dynamicTable').getElementsByTagName('tbody')[0].getElementsByTagName('tr');
    for (var i = 0; i < rows.length; i++) {
      rows[i].getElementsByTagName('td')[0].textContent = i + 1;
    }
  }
</script>

</body>
</html>
