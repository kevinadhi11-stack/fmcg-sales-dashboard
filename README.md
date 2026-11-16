# Dashboard Analisis Penjualan FMCG (2024–2025)

Dashboard Power BI untuk menganalisis penjualan, profit, dan margin sebuah perusahaan FMCG fiktif di enam provinsi di Indonesia, dengan fokus menemukan pola musiman, anomali margin, dan wilayah yang kinerjanya tertinggal.

> **Catatan:** data yang dipakai adalah data **sintetis** (hasil simulasi), bukan data bisnis nyata. Proyek ini bertujuan menunjukkan alur analisis: data cleaning, pemodelan data, DAX, visualisasi, dan rekomendasi bisnis.

## Ringkasan Hasil

| Metrik | Nilai |
|---|---|
| Total penjualan (2024–2025) | Rp 1,66 miliar |
| Total profit | Rp 505,89 juta |
| Margin profit | 30,46% |

![Halaman 1](images/page1.png)
![Halaman 2](images/page2.png)

## Pertanyaan Bisnis
1. Bagaimana tren penjualan bulanan dan adakah pola musiman?
2. Kategori produk mana yang paling menguntungkan, dan mana yang marginnya rendah?
3. Provinsi atau channel mana yang kinerjanya tertinggal, dan apa penyebabnya?

## Data
Tiga tabel (star schema): `fact_sales` (transaksi), `dim_product` (produk dan kategori), `dim_store` (toko, kota, provinsi, channel). Tabel tanggal `dim_date` dibuat dengan DAX (`CALENDAR`). Periode data: Januari 2024 – Desember 2025.

## Proses
**Data cleaning (Power Query)**
- Menghapus transaksi duplikat berdasarkan `transaction_id`.
- Nilai `discount_pct` yang kosong diisi 0 (dianggap tanpa diskon).
- Baris dengan `quantity` kosong dihapus karena nilai transaksinya tidak bisa dihitung.
- Memperbaiki tipe data `discount_pct` menjadi *Decimal* karena sempat terbaca sebagai bilangan bulat sehingga diskon terbulatkan menjadi 0, dan mengatur locale impor ke English (US).

**Measure DAX utama**
```
Total Sales = SUMX(fact_sales, fact_sales[quantity] * fact_sales[unit_price] * (1 - fact_sales[discount_pct]))
Total Cost = SUMX(fact_sales, fact_sales[quantity] * RELATED(dim_product[unit_cost]))
Profit = [Total Sales] - [Total Cost]
Profit Margin % = DIVIDE([Profit], [Total Sales])
Avg Discount % = AVERAGE(fact_sales[discount_pct])
Sales per Store = DIVIDE([Total Sales], DISTINCTCOUNT(fact_sales[store_id]))
```

## Temuan

1. **Penjualan punya pola musiman yang jelas.** Penjualan bulanan biasanya berkisar Rp 55–70 juta, tetapi melonjak mendekati Rp 95–100 juta pada Maret–April 2024 dan Maret 2025, serta naik lagi pada Desember (sekitar Rp 77 juta pada 2024 dan Rp 84 juta pada 2025). Pola ini konsisten dengan periode Ramadan/Lebaran dan akhir tahun. *(Sumbu Y pada grafik tren dipotong agar pola musiman lebih jelas.)*

2. **Margin kategori sangat berbeda.** Perawatan Pribadi paling menguntungkan (margin 43,5%), sedangkan Susu & Olahan paling rendah (18,0%). Makanan Ringan dan Minuman berada di sekitar 29%, Kebutuhan Rumah Tangga 24,6%.

3. **Anomali margin: Susu & Olahan di Sumatera Utara hanya 7,38%**, jauh di bawah sekitar 19% di provinsi lain. Penyebabnya terlihat dari diskon: rata-rata diskon kategori ini di Sumatera Utara mencapai **15,18%**, sedangkan di provinsi lain sekitar 2–3,5%. Kategori lain di Sumatera Utara marginnya normal, sehingga masalahnya spesifik pada kebijakan diskon susu di provinsi tersebut.

4. **Sulawesi Selatan tertinggal secara kinerja, bukan karena jumlah toko.** Penjualan per toko di Sulawesi Selatan sekitar Rp 6 juta, jauh di bawah provinsi lain (sekitar Rp 17 juta di Jawa Tengah, Jawa Timur, dan Sumatera Utara; sekitar Rp 25–26 juta di Jawa Barat dan DKI Jakarta).

5. **Komposisi channel:** Modern Trade menyumbang 50,8% penjualan, Toko Tradisional 39,1%, dan E-commerce baru 10,1%.

## Rekomendasi
1. **Tinjau kebijakan diskon Susu & Olahan di Sumatera Utara.** Diskon rata-rata 15% menekan margin hingga 7%. Tetapkan batas diskon, atau evaluasi apakah kenaikan volume sebanding dengan margin yang hilang.
2. **Selidiki penyebab rendahnya penjualan per toko di Sulawesi Selatan** (distribusi, ketersediaan produk, harga, atau aktivitas promosi) sebelum menambah toko baru.
3. **Siapkan stok dan promosi sebelum puncak musiman** (Februari–Maret menjelang Ramadan/Lebaran dan Desember) agar tidak kehabisan stok di bulan dengan permintaan tertinggi.
4. **Prioritaskan produk Perawatan Pribadi** dalam promosi dan penempatan, karena marginnya tertinggi (43,5%).
5. **Pertimbangkan pengembangan e-commerce.** Kontribusinya baru sekitar 10%. Data ini tidak cukup untuk menilai tren pertumbuhannya, sehingga perlu dianalisis lebih lanjut.

## Keterbatasan
- Data sintetis, jadi temuan menggambarkan alur analisis dan bukan kondisi bisnis nyata.
- Hubungan antara diskon dan margin di Sumatera Utara bersifat korelasi. Untuk memastikan sebab-akibat diperlukan data tambahan (misalnya perubahan volume setelah diskon).
- Tidak ada data biaya operasional, sehingga profit yang dihitung adalah laba kotor (penjualan dikurangi harga pokok).

## Tools
Power BI Desktop (Power Query, DAX), GitHub.

## Isi Repositori
```
├── fmcg_dashboard.pbix
├── data/ (fact_sales.csv, dim_product.csv, dim_store.csv)
├── images/ (page1.png, page2.png)
└── README.md
```
