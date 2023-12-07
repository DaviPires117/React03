<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" rel="stylesheet">
  <style>
    body {
      padding: 20px;
    }
    .item-container {
      margin-top: 20px;
    }
  </style>
  <title>Lista de Compras</title>
</head>
<body>

  <div class="container mt-5">
    <h1 class="mb-4">Lista de Compras</h1>

    <form id="shoppingForm">
      <div class="form-group">
        <label for="itemName">Item:</label>
        <input type="text" class="form-control" id="itemName" placeholder="Nome do item" required>
      </div>
      <div class="form-row">
        <div class="form-group col-md-4">
          <label for="quantity">Quantidade:</label>
          <input type="number" class="form-control" id="quantity" placeholder="Quantidade" required>
        </div>
        <div class="form-group col-md-4">
          <label for="price">Valor:</label>
          <input type="number" class="form-control" id="price" placeholder="Valor" required>
        </div>
        <div class="form-group col-md-4">
          <button type="button" class="btn btn-success mt-4" onclick="addItem()">Adicionar</button>
        </div>
      </div>
    </form>

    <div class="item-container">
      <h2>Itens Adicionados</h2>
      <ul class="list-group" id="itemList">
        <!-- Lista de itens -->
      </ul>
    </div>

    <div class="item-container">
      <h2>Total Geral</h2>
      <ul class="list-group" id="totalList">
        <!-- Lista de totais -->
      </ul>
    </div>
  </div>

  <script>
    var items = [];
    var totals = [];

    function addItem() {
      var itemName = document.getElementById("itemName").value;
      var quantity = parseInt(document.getElementById("quantity").value);
      var price = parseFloat(document.getElementById("price").value);

      if (!isNaN(quantity) && !isNaN(price)) {
        var totalItemPrice = quantity * price;
        items.push({ name: itemName, quantity: quantity, price: price });
        totals.push(totalItemPrice);

        updateLists();
        clearForm();
      } else {
        alert("Por favor, insira valores válidos para quantidade e preço.");
      }
    }

    function updateLists() {
      var itemList = document.getElementById("itemList");
      var totalList = document.getElementById("totalList");

    
      itemList.innerHTML = "";
      totalList.innerHTML = "";

      var totalGeneral = 0;

      for (var i = 0; i < items.length; i++) {
        var item = items[i];
        var totalItemPrice = item.quantity * item.price;

      
        itemList.innerHTML += `
          <li class="list-group-item">
            ${item.quantity} ${item.name} - ${item.price.toFixed(2)}
            <button type="button" class="btn btn-sm btn-success ml-2" onclick="increaseQuantity(${i})">+</button>
            <button type="button" class="btn btn-sm btn-warning ml-2" onclick="decreaseQuantity(${i})">-</button>
            <button type="button" class="btn btn-sm btn-danger ml-2" onclick="removeItem(${i})">x</button>
          </li>
        `;

        
        totalGeneral += totalItemPrice;
        totalList.innerHTML += `
          <li class="list-group-item">
            ${item.quantity} ${item.name} - ${totalItemPrice.toFixed(2)}
          </li>
        `;
      }

      
      totalList.innerHTML += `
        <li class="list-group-item">
          Total Geral: ${totalGeneral.toFixed(2)}
        </li>
      `;
    }

    function increaseQuantity(index) {
      items[index].quantity++;
      updateLists();
    }

    function decreaseQuantity(index) {
      if (items[index].quantity > 1) {
        items[index].quantity--;
        updateLists();
      }
    }

    function removeItem(index) {
      items.splice(index, 1);
      totals.splice(index, 1);
      updateLists();
    }

    function clearForm() {
      document.getElementById("itemName").value = "";
      document.getElementById("quantity").value = "";
      document.getElementById("price").value = "";
    }
  </script>

</body>
</html>
