# Analisis Sentimen Komentar YouTube: Green SM vs KAI KRL Bekasi Timur
Proyek ini melakukan analisis sentimen terhadap komentar publik di YouTube terkait kecelakaan antara taksi listrik Green SM dan KRL di perlintasan sebidang Bekasi Timur pada 27 April 2026. Analisis ini digunakan sebagai data primer dalam makalah Studium Generale KU4078 Institut Teknologi Bandung.
 
> **Judul Makalah:** Analisis Kepemimpinan Publik dalam Manajemen Krisis: Studi Kasus Kecelakaan Green SM dan KRL di Bekasi Timur serta Dampaknya terhadap Kepercayaan Publik
--- 
## Struktur Repositori
 
```
.
├── sentiment_analysis.ipynb   # Notebook utama: scraping, preprocessing, klasifikasi
├── requirements.txt           # Daftar dependensi Python
├── output/
│   ├── comments_classified.csv        # Seluruh komentar beserta label sentimen
│   ├── comments_validation_sample.csv # Sampel 10% untuk validasi manual
│   └── sentiment_distribution.png    # Visualisasi distribusi sentimen
└── README.md
```
 
---
 
## Prasyarat
 
- Python 3.10 atau lebih baru
- YouTube Data API v3 key (gratis, daftar di [Google Cloud Console](https://console.cloud.google.com))
- GPU opsional — CPU sudah cukup untuk 300 komentar
Install seluruh dependensi dengan:
 
```bash
pip install -r requirements.txt
```
 
Dependensi utama yang digunakan:
 
| Library | Kegunaan |
|---|---|
| `google-api-python-client` | Scraping komentar via YouTube Data API v3 |
| `transformers` | Load model IndoBERT dari HuggingFace |
| `torch` | Backend inferensi model |
| `pandas` | Manipulasi dan ekspor data |
| `matplotlib` / `seaborn` | Visualisasi distribusi sentimen |
| `tqdm` | Progress bar saat inferensi |
 
---
 
## Setup API Key
 
1. Buka [Google Cloud Console](https://console.cloud.google.com) dan buat project baru
2. Aktifkan **YouTube Data API v3** pada project tersebut
3. Buat credentials bertipe **API Key**
4. Pada notebook, isi variabel berikut di sel konfigurasi:
```python
YOUTUBE_API_KEY = "ISI_API_KEY_KAMU_DI_SINI"
```
 
> Jangan commit API key ke repositori publik. Gunakan file `.env` atau Google Colab Secrets jika perlu.
 
---
 
## Cara Menjalankan
 
### Google Colab (direkomendasikan)
1. Buka file `sentiment_analysis.ipynb` di [Google Colab](https://colab.research.google.com)
2. Jalankan sel instalasi dependensi di bagian awal notebook
3. Isi `YOUTUBE_API_KEY` pada sel konfigurasi
4. Jalankan semua sel secara berurutan (`Runtime > Run all`)
### Lokal (VS Code / Jupyter)
```bash
git clone https://github.com/USERNAME/REPO_NAME.git
cd REPO_NAME
pip install -r requirements.txt
jupyter notebook sentiment_analysis.ipynb
```
 
---
 
## Metodologi
 
Pipeline analisis terdiri dari empat tahap utama:
 
**1. Pengumpulan Data**
Komentar diambil dari 10 video berita YouTube menggunakan YouTube Data API v3. Video dipilih secara *purposive sampling* berdasarkan relevansi topik, kanal terverifikasi, dan rentang tanggal 27 April–4 Mei 2026. 5 video dikategorikan sebagai kelompok `GREEN_SM` dan 5 video sebagai kelompok `KAI_KRL`.
 
**2. Preprocessing**
Komentar dibersihkan dari URL, mention, hashtag, dan karakter berlebihan. Komentar dengan panjang kurang dari 5 kata difilter sebagai noise.
 
**3. Klasifikasi Sentimen**
Menggunakan model [`mdhugol/indonesia-bert-sentiment-classifier`](https://huggingface.co/mdhugol/indonesia-bert-sentiment-classifier) dari HuggingFace — model IndoBERT yang telah di-*fine-tune* untuk analisis sentimen berbahasa Indonesia. Setiap komentar diklasifikasikan ke dalam tiga label: **POSITIF**, **NEGATIF**, atau **NETRAL**, disertai *confidence score*.
 
**4. Validasi Manual**
Sebanyak 10% komentar dipilih secara acak berlapis dan diekspor ke CSV terpisah untuk diverifikasi secara manual.
 
---
 
## Hasil
 
Total komentar yang dianalisis: **299 komentar** (setelah preprocessing dari 538 komentar mentah)
 
| Sentimen | Jumlah | Persentase |
|---|---|---|
| Negatif | 224 | 74,9% |
| Netral | 61 | 20,4% |
| Positif | 14 | 4,7% |
 
**Perbandingan per kelompok:**
 
| Kelompok | Negatif | Netral | Positif |
|---|---|---|---|
| GREEN_SM (120 komentar) | 76,7% | 20,0% | 3,3% |
| KAI_KRL (179 komentar) | 73,7% | 20,7% | 5,6% |
 
Visualisasi lengkap tersedia di `output/sentiment_distribution.png`.
 
---
 
## Referensi
 
- Koto, F., et al. (2020). [IndoNLU: Benchmark and Resources for Evaluating Indonesian Natural Language Understanding](https://arxiv.org/abs/2009.05387). *AACL-IJCNLP 2020*.
- Coombs, W. T. (2007). Protecting Organization Reputations During a Crisis: The Development and Application of Situational Crisis Communication Theory. *Corporate Reputation Review, 10*(3), 163–176.
- Model HuggingFace: [`mdhugol/indonesia-bert-sentiment-classifier`](https://huggingface.co/mdhugol/indonesia-bert-sentiment-classifier)
---
 
## Lisensi
 
Repositori ini dibuat untuk keperluan akademik. Data komentar bersumber dari platform publik YouTube dan dikumpulkan sesuai dengan [YouTube API Terms of Service](https://developers.google.com/youtube/terms/api-services-tos).
Author: Hakam Avicena Mustain - 13524075
