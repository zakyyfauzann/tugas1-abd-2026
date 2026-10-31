# Data Tugas 1

## Dataset yang Dipilih

Dataset yang digunakan dalam proyek ini adalah SIAGA Pantura: Coupled Flood and Water Stress Dataset, dengan menggunakan subset data banjir.

| Item | Isi |
|---|---|
| Nama dataset | SIAGA Pantura: Coupled Flood and Water Stress Dataset (Flood Dataset) |
| Sumber | [Hugging Face](https://huggingface.co/datasets/ethanchrstian/siaga-pantura-hazard-data) |
| Lisensi/ketentuan pakai | MIT (sesuai metadata repositori Hugging Face) |
| Ukuran | 16,27 MB dengan 1.152.432 baris |
| Periode data | 30 Januari 2015 – 31 Desember 2024 |
| Unit analisis | Kecamatan per hari |

**Pemenuhan ketentuan ukuran:** Dataset memenuhi persyaratan melalui jumlah baris yang lebih dari 1.000.000 baris. Ukuran file belum mencapai 500 MB.

## Tempat Mencari Dataset

Pilih dataset Indonesia yang legal digunakan, dapat didokumentasikan sumbernya, dan memenuhi batas ukuran tugas.

| Situs | Kegunaan |
|---|---|
| [Satu Data Indonesia](https://data.go.id/) | Portal data terbuka lintas instansi pemerintah Indonesia. |
| [Badan Pusat Statistik](https://www.bps.go.id/) | Statistik sosial, ekonomi, kependudukan, dan data wilayah. |
| [BMKG Data Online](https://dataonline.bmkg.go.id/) | Data cuaca, iklim, gempa bumi, dan observasi meteorologi. |
| [Hugging Face Datasets](https://huggingface.co/datasets) | Dataset publik yang dapat dicari berdasarkan topik, bahasa, atau ukuran. |
| [Kaggle Datasets](https://www.kaggle.com/datasets) | Katalog dataset publik; periksa lisensi dan dokumentasi pembuatnya. |
| [Google Dataset Search](https://datasetsearch.research.google.com/) | Mesin pencari untuk menemukan dataset dari berbagai portal. |

## Cara Memperoleh Data

1. Buka URL sumber di atas.
2. Unduh file ke folder `data/raw/` tanpa mengubah data mentah.
3. Catat nama file dan checksum bila tersedia.
4. Ubah variabel `DATA_PATH` pada `notebooks/01_data_profiling.ipynb` agar menunjuk ke file tersebut.

## Aturan Penyimpanan

- Jangan commit dataset mentah atau hasil olahan berukuran besar ke Git.
- File pada `data/raw/` adalah data asli dan tidak boleh diubah.
- Simpan hasil transformasi yang dapat direproduksi pada `data/processed/`.

## Hasil Akhir Data Cleaning

Proses Data Cleaning dilakukan menggunakan Polars dan DuckDB.

### Ringkasan Hasil

| Item | Hasil |
|---|---|
| Jumlah baris | 1.152.432 |
| Jumlah kolom awal | 13 |
| Jumlah kolom akhir | 20 |
| Ukuran file hasil cleaning | 27 MB |
| Missing values | 0 |
| Duplikasi kunci bisnis | 0 |
| Baris yang dihapus | 0 |

### Penanganan Outlier

Deteksi outlier menggunakan metode Interquartile Range (IQR) dengan batas 1,5 × IQR.

Winsorization dilakukan dengan membuat tujuh kolom tambahan berakhiran `_winsorized`. Kolom asli tetap dipertahankan agar informasi pengukuran awal tidak hilang.

### Validasi Data

Validasi akhir menunjukkan:
- Tidak terdapat missing values.
- Tidak terdapat duplikasi berdasarkan `district_id` dan `date`.
- Tidak terdapat ketidaksesuaian tahun dan tanggal.
- Tidak terdapat ketidaksesuaian bulan dan tanggal.
- Seluruh label banjir dan kategori pesisir valid.
- Tidak terdapat nilai non-finite.

### File Hasil Pengolahan

| File | Keterangan |
|---|---|
| `flood_dataset_cleaned.parquet` | Dataset final hasil cleaning |
| `duckdb_summarize.csv` | Ringkasan statistik 20 kolom menggunakan DuckDB |

Dataset hasil cleaning disimpan menggunakan format Parquet dengan kompresi Zstandard.
