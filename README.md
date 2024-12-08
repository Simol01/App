function doGet() {
  const html = HtmlService.createHtmlOutputFromFile('index')
    .setTitle('Catalogo Prodotti');
  return html;
}

// Funzione per ottenere i dati dal foglio Google
function getProducts() {
  const sheet = SpreadsheetApp.openById('ID_DEL_TUO_SHEET').getSheetByName('Catalogo');
  const data = sheet.getDataRange().getValues();
  const headers = data.shift();
  return data.map(row => {
    let product = {};
    headers.forEach((header, index) => product[header] = row[index]);
    return product;
  });
}

// Salvataggio di un ordine
function saveOrder(order) {
  const sheet = SpreadsheetApp.openById('ID_DEL_TUO_SHEET').getSheetByName('Ordini');
  sheet.appendRow([order.productId, order.quantity, order.customerName, order.customerEmail, new Date()]);
  return 'Ordine ricevuto con successo!';
}
