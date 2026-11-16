# Dataset FMCG Sintetis (2024-2025)

DATA INI SINTETIS (dibuat dengan simulasi), bukan data bisnis nyata. Tulis hal ini dengan jelas di README proyekmu.

## Tabel
- fact_sales.csv: 1 baris = 1 transaksi (tanggal, toko, produk, kuantitas, harga, diskon)
  - transaction_id, date, store_id, product_id, quantity, unit_price, discount_pct
- dim_product.csv: product_id, product_name, category, brand, list_price, unit_cost
- dim_store.csv: store_id, store_name, channel, city, region

## Relasi di Power BI
- fact_sales[product_id] -> dim_product[product_id] (many-to-one)
- fact_sales[store_id]   -> dim_store[store_id]     (many-to-one)
- Tabel tanggal: buat sendiri dengan DAX (CALENDAR) lalu hubungkan ke fact_sales[date]

## Rumus dasar
- Sales  = quantity * unit_price * (1 - discount_pct)
- Cost   = quantity * unit_cost
- Profit = Sales - Cost

## Latihan data cleaning (sengaja ada masalah kecil)
- Ada nilai kosong di quantity dan discount_pct
- Ada transaksi duplikat (transaction_id sama)
Putuskan cara menanganinya, lalu tulis keputusanmu dan alasannya di README.

## Pertanyaan yang disarankan
1. Bagaimana tren penjualan per bulan dan adakah pola musiman?
2. Kategori apa yang paling menyumbang sales, dan mana yang margin profitnya rendah?
3. Wilayah atau channel mana yang performanya paling rendah, dan kenapa?
Cari penyebabnya dengan drill-down, lalu tulis rekomendasi bisnis dengan angka.
