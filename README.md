# 📊 Memahami SQL HAVING melalui Studi Kasus Nyata (Praktikal & Siap Publish)

Query `HAVING` sering kali membuat bingung pemula karena terlihat mirip dengan `WHERE`. Padahal, di dunia nyata database, `HAVING` **sangat sering digunakan** untuk analisis data berbasis agregasi.

Artikel ini disusun sebagai **blog praktikal berbasis modul Basis Data Part 10**, lengkap dengan contoh kasus dan query SQL yang bisa langsung dicoba.

---

## 🔍 Apa Itu HAVING?

`HAVING` digunakan untuk **memfilter data hasil agregasi** (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`).

Perbedaan utama:

* `WHERE` → filter data **sebelum** `GROUP BY`
* `HAVING` → filter data **setelah** `GROUP BY`

### Sintaks Umum

```sql
SELECT kolom
FROM tabel
GROUP BY kolom
HAVING kondisi;
```

---

## 📌 Studi Kasus 1: Customer dengan Lebih dari 1 Transaksi

### Masalah

Menampilkan customer yang melakukan lebih dari satu transaksi.

### Query

```sql
SELECT customer_id
FROM subscription
GROUP BY customer_id
HAVING COUNT(customer_id) > 1;
```

### Penjelasan

* `GROUP BY customer_id` → mengelompokkan transaksi per customer
* `COUNT(customer_id)` → menghitung jumlah transaksi
* `HAVING COUNT(customer_id) > 1` → filter customer aktif

---

## 📌 Studi Kasus 2: JOIN + HAVING (Kasus Nyata)

### Masalah

Menampilkan data customer yang memiliki lebih dari satu subscription.

### Query

```sql
SELECT b.name, b.address, b.phone, a.product_id, a.subscription_date
FROM subscription a
JOIN customer b ON a.customer_id = b.id
WHERE b.id IN (
  SELECT customer_id
  FROM subscription
  GROUP BY customer_id
  HAVING COUNT(customer_id) > 1
)
ORDER BY b.id ASC;
```

### Kenapa Ini Penting?

Query seperti ini **sangat sering muncul di sistem bisnis**, misalnya:

* Customer loyal
* Analisis langganan
* Segmentasi pengguna

---

## 📈 Studi Kasus 3: HAVING dengan Fungsi MAX

### Menampilkan Total Transaksi Terbesar per Produk

```sql
SELECT product_id, MAX(total_price) AS total
FROM invoice
GROUP BY product_id;
```

### Filter Produk dengan Transaksi di Atas 1.000.000

```sql
SELECT product_id, MAX(total_price) AS total
FROM invoice
GROUP BY product_id
HAVING MAX(total_price) > 1000000;
```

---

## 📉 Studi Kasus 4: HAVING dengan MIN

### Query Dasar

```sql
SELECT product_id, MIN(total_price) AS total
FROM invoice
GROUP BY product_id;
```

### Filter Transaksi di Bawah 500.000

```sql
SELECT product_id, MIN(total_price) AS total
FROM invoice
GROUP BY product_id
HAVING MIN(total_price) < 500000;
```

---

## 📊 Studi Kasus 5: HAVING dengan AVG

### Rata-Rata Transaksi per Produk

```sql
SELECT product_id, AVG(total_price) AS total
FROM invoice
GROUP BY product_id;
```

### Filter Rata-Rata di Atas 100.000

```sql
SELECT product_id, AVG(total_price) AS total
FROM invoice
GROUP BY product_id
HAVING AVG(total_price) > 100000;
```

---

## 🧠 Kenapa HAVING Penting di Dunia Kerja?

* Digunakan dalam **data analysis & reporting**
* Sering muncul di **query dashboard**
* Dipakai untuk **decision making bisnis**
* Hampir selalu dipakai bersama `GROUP BY`

Jika Anda memahami `HAVING`, Anda **selangkah lebih maju** dari banyak pemula SQL.

---

## 📝 Latihan (Quiz)

### Quiz 1

Menampilkan:

* `ordernumber`
* `itemscount` (total quantityOrdered)
* `total` (total sales)

```sql
SELECT ordernumber,
       SUM(quantityordered) AS itemscount,
       SUM(priceeach * quantityordered) AS total
FROM orderdetails
GROUP BY ordernumber;
```

### Filter Total Sales > 1000

```sql
SELECT ordernumber,
       SUM(quantityordered) AS itemscount,
       SUM(priceeach * quantityordered) AS total
FROM orderdetails
GROUP BY ordernumber
HAVING total > 1000;
```

### Filter Total > 1000 dan Itemscount > 600

```sql
SELECT ordernumber,
       SUM(quantityordered) AS itemscount,
       SUM(priceeach * quantityordered) AS total
FROM orderdetails
GROUP BY ordernumber
HAVING total > 1000 AND itemscount > 600;
```

---

### Quiz 2: JOIN + HAVING

```sql
SELECT o.status,
       SUM(od.priceeach * od.quantityordered) AS total
FROM orders o
JOIN orderdetails od ON o.ordernumber = od.ordernumber
WHERE o.status = 'Shipped'
GROUP BY o.status
HAVING total > 1500;
```

---

## ✅ Penutup

`HAVING` bukan sekadar materi kuliah, tapi **alat penting dalam analisis data nyata**. Dengan memahami studi kasus di atas, Anda sudah siap mengerjakan query SQL level industri.

Artikel ini cocok dipublikasikan sebagai **blog GitHub (README.md)** atau materi terlihat profesional di portfolio.

---

📌 *Sumber: Modul Basis Data Part 10*
