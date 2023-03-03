# E-Commerce Design Assignment

## Given Product ID, return Product Details

![Product Details Using Sequence Diagram](./assets/product-details-sequence-diagram.png)

Menggunakan *Sequence Diagram* dikarenakan untuk merepresentasikan urutan pesan atau pemanggilan antara objek dalam sistem atau proses bisnis. Diagram ini membantu memvisualisasikan alur interaksi antara objek-objek tersebut dan dapat membantu dalam pemodelan dan analisis sistem yang kompleks.

## Create Order

![Create Order](./assets/create-order-microservice-diagram.png)

*Microservice Diagram*  digunakan untuk mewakili arsitektur perangkat lunak yang kompleks dengan banyak layanan independen yang saling terhubung. Setiap layanan direpresentasikan sebagai komponen terpisah yang memungkinkan pemisahan tugas-tugas tertentu dalam proses. Hal ini memungkinkan pengembangan dan pengelolaan layanan secara terpisah, meningkatkan fleksibilitas dan skalabilitas sistem secara keseluruhan. Dalam konteks ini, menggunakan diagram mikroserivis dapat membantu untuk memahami dan merancang arsitektur perangkat lunak yang lebih terukur, dapat diandalkan, dan mudah dikembangkan.

Pseudocode:
(asumsi database sudah terurutkan berdasarkan id)
```
input: [{productID, quantity}] (object)
output: totalPrice

function getTotalPrice(products):
    let totalPrice = 0
    for each product in products:
        totalPrice = totalPrice + product.price
        product.stock -= 1

    return totalPrice
```
```
input: customerID (string), [{productID, quantity}] (object)
output: orderID (integer)

function generateOrderID(customerID, products):
    customerDatabase = query customer database's table
    orderIDDatabase = query order ID database's table
    if customerID in customerDatabase:
        orderID = max(database.id) + 1
        if checkStock(products) == true:
            if processPayment(getTotalPrice(products)) == true:
                callSeller(orderID)
                removeStock(products)
                return orderID 
    else:
        throw errorMassage
```
```
input: [{productID, quantity}] (object)
output: boolean

function checkStock(products):
    productDatabase= query product database's table
    
    for each productID, quantity in products.productID products.quantity:
        if productID valid in productDatabase:
            if quantity < quantity in productDatabaase:
                return true
```
```
input: orderID (integer)
output: none

    function notifySeller(orderID):
        send orderID to seller's email
```
```
input: [{productID, quantity}] (object)
output: none

function removeStock(products):
    productDatabase = query product database's table
    for each productID, quantity in products.productsID, products.quantity:
        productDatabase[productID] -= quantity
```
```
input: totalPrice (integer)
output: boolean

function processPayment(totalPrice):
    paymentDatabase = query payment database's table

    if totalPrice is valid in paymentDatabase:
        return true
    else:
        return false
```

|Function's name|Complexity|Analysis|
|---------------|----------|--------|
|getTotalPrice|O(n)|dikarenakan menggunakan n kali *looping* dengan input array of object dengan panjang n. Semakin besar ukuran masukan, semakin lama waktu yang dibutuhkan algoritma untuk menghitung total harga|
|generateOrderID|O(1)|Tidak ada pengulangan code yang dilakukan. Code hanya berjalan secara konstan yaitu 1 kali saja|
|checkStock|O(n)|dikarenakan menggunakan n kali *looping* dengan input array of object dengan panjang n. Semakin besar ukuran masukan, semakin lama waktu yang dibutuhkan algoritma untuk mengecek apakah stok terpenuhi|
|notifySeller|O(1)|Tidak ada pengulangan code yang dilakukan. Code hanya berjalan secara konstan yaitu 1 kali saja. Hanya untuk mengingatkan *seller*|
|removeStock|O(n)|dikarenakan menggunakan n kali *looping* dengan input array of object dengan panjang n. Semakin besar ukuran masukan, semakin lama waktu yang dibutuhkan algoritma untuk mengurangi stock pada database|
|prcessPayment|O(1)|Tidak ada pengulangan code yang dilakukan. Code hanya berjalan secara konstan yaitu 1 kali saja. Hanya untuk me-validasi pembayaran|