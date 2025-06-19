# 🧠 AI for Hiring: Automated Cover Letter Classification and Summarization
**Oleh: Orvalamarva**

---

## 📌 Project Overview

### Latar Belakang
Di tengah pasar kerja yang kompetitif, tim Human Resources (HR) dihadapkan pada tantangan untuk menyaring ratusan hingga ribuan surat lamaran (cover letter) untuk setiap posisi yang dibuka. Proses manual ini tidak hanya memakan waktu dan tenaga, tetapi juga rentan terhadap bias dan inkonsistensi. Kecepatan dan ketepatan dalam mengidentifikasi kandidat yang paling relevan menjadi kunci untuk memenangkan talenta terbaik. Melalui model bahasa besar (LLM) IBM Granite, proyek ini memproses data riil dari cover letter dataset berisi 813 surat lamaran dari berbagai posisi pekerjaan di sektor teknologi dan data. 

### Permasalahan
Proses penyaringan surat lamaran secara manual seringkali menjadi hambatan (*bottleneck*) dalam alur rekrutmen, menyebabkan:
1.  **Waktu Respons yang Lambat:** Kandidat potensial bisa kehilangan minat atau menerima tawaran lain.
2.  **Kelelahan Penganalisis:** Menurunnya kualitas penyaringan setelah membaca puluhan dokumen.
3.  **Kesulitan Identifikasi Cepat:** Sulit untuk mendapatkan "gambaran besar" profil kandidat tanpa membaca keseluruhan dokumen secara mendalam.

### Tujuan Proyek
Untuk menjawab tantangan-tantangan tersebut, proyek ini menghadirkan pendekatan berbasis AI (Large Language Model) untuk mengotomatisasi dan memperkaya proses analisis surat lamaran. Tujuan utamanya adalah:
1.  **Mengklasifikasikan secara otomatis** setiap surat lamaran ke posisi pekerjaan yang paling sesuai.
2.  **Menghasilkan ringkasan terstruktur** dari setiap surat lamaran untuk menyajikan informasi utama secara cepat dan efisien.

Melalui pendekatan tersebut, sistem menghasilkan output berupa klasifikasi posisi pekerjaan dan ringkasan kandidat yang siap digunakan oleh tim HR untuk mempercepat proses pre-screening secara objektif dan efisien.

---

## 🔗 Dataset

* **Sumber Data:** Dataset publik dari Hugging Face – ShashiVish/cover-letter-dataset
* **Format File:** `cover-letter-dataset.parquet`  
* **Jumlah Sampel:** 813 surat lamaran
* **Jumlah Kategori Pekerjaan:** 56 posisi unik

### 🗂️ Deskripsi Singkat
Dataset ini berisi surat lamaran dari pelamar berbagai posisi di sektor teknologi dan data. Setiap entri mencakup:
* **Job Title** – posisi pekerjaan yang dilamar
* **Cover Letter** – isi surat lamaran dalam bentuk teks bebas
* **Informasi Pendukung** - pengalaman kerja, kualifikasi, dan skillset

### 📝 Catatan Penggunaan
File yang digunakan berasal dari bagian train dalam dataset asli dan telah dinamai ulang menjadi cover-letter-data.parquet untuk memudahkan dokumentasi. Dataset ini dipilih karena:
* Bersifat terbuka dan dapat diakses publik
* Representatif terhadap kebutuhan otomasi analisis dalam proses pre-screening rekrutmen

---

## 🚀 Analysis Process
Analisis dilakukan melalui beberapa langkah sistematis untuk memastikan kualitas data dan hasil yang akurat:

#### a. Pembersihan dan Pra-pemrosesan Data
Langkah awal adalah membersihkan data untuk memastikan konsistensi dan kualitas. Proses ini mencakup:
* **Normalisasi Teks:** Mengubah semua teks menjadi huruf kecil dan menghapus spasi berlebih.
* **Penanganan Data Kosong:** Memastikan tidak ada nilai kosong pada kolom krusial (`Job Title` dan `Cover Letter`).
* **Deduplikasi Cerdas:** Menghapus baris yang merupakan duplikat berdasarkan **kombinasi `Job Title` dan `Cover Letter`**. Alasan penggunaan metode ini adalah untuk memastikan setiap data merupakan contoh unik dari sebuah surat lamaran untuk sebuah posisi spesifik, yang sangat penting untuk akurasi model klasifikasi.
Setelah pembersihan, data yang tersisa sebanyak 759 baris unik yang siap digunakan untuk klasifikasi dan ringkasan.

#### b. Klasifikasi Posisi Pekerjaan dengan AI
* **Metode:** Menggunakan model `ibm-granite/granite-3.3-8b-instruct` melalui platform Replicate.
* **Teknik:** *Prompt Engineering* canggih digunakan untuk memaksimalkan akurasi. *Prompt* dirancang untuk memberikan peran, tugas spesifik, daftar pilihan jawaban yang valid (diambil dari data unik `Job Title`), dan sebuah contoh (*few-shot learning*). Teknik ini mengarahkan model untuk memilih job title yang paling relevan, menghindari bias jawaban bebas, dan meningkatkan konsistensi klasifikasi.

#### c. Ringkasan Terstruktur dengan AI
* **Metode:** Memanfaatkan model yang sama (`ibm-granite/granite-3.3-8b-instruct`).
* **Teknik:** *Prompt* dirancang untuk tidak hanya meringkas, tetapi juga **mengekstrak informasi kunci** dan menyajikannya dalam format terstruktur dengan judul: `Ringkasan Pengalaman`, `Skillset Utama`, dan `Posisi Relevan Sebelumnya`. Pendekatan ini mengubah data tidak terstruktur (teks bebas) menjadi data terstruktur yang mudah dicerna.

---

## 🔍 Insight & Findings
Adapun beberapa hal yang berhasil ditemukan dalam project ini:

### 🔢 Efektivitas Ekstraksi Informasi Utama oleh AI
Ringkasan terstruktur yang dihasilkan oleh model AI terbukti sangat efektif dalam menangkap esensi dari profil kandidat. Dalam pengujian sampel, model berhasil secara konsisten mengidentifikasi jumlah tahun pengalaman, 3-5 skill utama, dan posisi relevan yang disebutkan, bahkan ketika informasi tersebut tersebar di berbagai bagian surat lamaran. Insight ini menunjukkan potensi besar untuk mengurangi waktu baca per-kandidat dari beberapa menit menjadi beberapa detik.

### 🧾 Pola Bahasa sebagai Indikator Posisi
Model klasifikasi menunjukkan kemampuan tinggi dalam membedakan posisi pekerjaan berdasarkan nuansa bahasa dalam surat lamaran. Ditemukan bahwa:
* Lamaran untuk posisi teknis seperti **'software engineer'** atau **'data scientist'** kaya akan terminologi spesifik (misalnya, *'REST API', 'microservices', 'machine learning', 'scikit-learn'*).
* Lamaran untuk posisi manajerial seperti **'project manager'** lebih menonjolkan kata kerja yang berorientasi pada proses dan hasil (misalnya, *'mengelola', 'mengkoordinasikan', 'memimpin', 'mencapai target'*).

### 📊 Variasi Surat Lamaran Sangat Luas
Terdapat 50+ job title berbeda dalam dataset, mulai dari Data Scientist, Senior Java Developer, hingga Quantitative Analyst. Adanya variasi tersebut menjadi tantangan tersendiri dalam membangun model klasifikasi multi-kelas.

### 🔎 Informasi Tidak Selalu Eksplisit
Beberapa surat lamaran tidak menyebutkan posisi sebelumnya atau tidak menyebutkan tools secara spesifik. Namun, model tetap mampu menyusun ringkasan yang masuk akal berdasarkan konteks umum, memperlihatkan bahwa model dapat mengisi celah informasi implisit meskipun tetap perlu kehati-hatian dalam interpretasinya.

Beberapa insight ini membuktikan bahwa meskipun model AI bekerja dengan baik dalam kondisi ideal, kualitas input tetap menjadi penentu akurasi hasil. Oleh karena itu, penguatan pada tahap pre-processing dan desain prompt sangat krusial dalam sistem berbasis LLM seperti ini.

---

## 🤝 Conclusion & Recommendations

Proyek ini berhasil menunjukkan bahwa AI, khususnya model bahasa seperti IBM Granite, dapat diimplementasikan secara efektif untuk mentransformasi proses penyaringan surat lamaran. Dengan mengotomatiskan klasifikasi dan ringkasan, kita dapat menciptakan alur kerja rekrutmen yang lebih cepat, lebih konsisten, dan memberikan dasar pengambilan keputusan yang lebih obyektif serta terstandarisasi di tahap awal rekrutmen. Berdasarkan hasil di atas, berikut adalah rekomendasi yang konkret dan dapat ditindaklanjuti:

###  **💡 Implementasi Sistem Pra-Filter Otomatis** 
Rekomendasi utama adalah mengintegrasikan **model klasifikasi AI** ke dalam sistem pelacakan pelamar (Applicant Tracking System - ATS). Setiap surat lamaran yang masuk dapat secara otomatis diberi label `Job Title`, memungkinkan tim HR untuk langsung memfilter dan meninjau kandidat sesuai dengan lowongan yang relevan.

###  **⏱️ Integrasi "AI Summary" pada Dasbor HR** 
Tampilkan **ringkasan terstruktur** yang dihasilkan AI di samping setiap nama pelamar pada dasbor utama sistem HR. Hal ini akan memberikan *first-look* yang sangat cepat dan informatif kepada perekrut tanpa harus membuka dokumen asli terlebih dahulu, mempercepat keputusan untuk "melihat lebih lanjut" atau "melewatkan".

###  **📈 Pengembangan Analisis Lanjutan** 
Data terstruktur dari hasil ringkasan (khususnya `Skillset Utama`) dapat diagregasi untuk analisis tren. Perusahaan dapat memetakan skill apa yang paling sering muncul untuk posisi tertentu, membantu dalam perencanaan strategis tenaga kerja dan penyesuaian deskripsi pekerjaan di masa depan.

---

## 🤖 AI Support Explanation

#### Model IBM Granite 3.3 8B Instruct digunakan dalam dua tugas utama:

**1 Multiclass Text Classification**
Model **IBM Granite** dimanfaatkan sebagai *classifier zero-shot* yang canggih. Dengan merancang *prompt* yang memberikan konteks, pilihan jawaban, dan contoh, model mampu mengkategorikan surat lamaran ke dalam kelas `Job Title` yang spesifik dengan akurasi tinggi. Ini adalah penggunaan AI yang relevan untuk mengubah tugas manual yang repetitif menjadi proses otomatis.

**2 Structured Summarization**
Ringkasan yang dihasilkan AI disusun dalam format tiga bagian utama: Ringkasan Pengalaman, Skillset Utama, dan Posisi Relevan Sebelumnya. Lebih dari sekadar meringkas, model **IBM Granite** digunakan untuk **ekstraksi informasi terstruktur**. *Prompt* yang dirancang secara spesifik menginstruksikan AI untuk bertindak seperti asisten HR, mengidentifikasi dan menyusun data-data penting (pengalaman, skill, jabatan) ke dalam format yang telah ditentukan. Ini adalah contoh penggunaan AI untuk mengubah data tidak terstruktur (teks bebas) menjadi wawasan terstruktur yang bernilai dan dapat ditindaklanjuti.

Eksekusi via Langchain + Replicate API, serta dukungan pembersihan data menggunakan Python dan Pandas, sistem ini mencerminkan penerapan LLM yang efisien dan terukur untuk masalah dunia nyata dalam bidang HR-tech melalui kombinasi antara prompt engineering.

---
