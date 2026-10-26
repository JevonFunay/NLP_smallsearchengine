PENCARIAN ARTIKEL ASAG
=====================

STATUS PROYEK
Program dan data uji kecil tersedia. Korpus artikel tugas belum tersedia.
Jumlah artikel pada saat proyek dibuat: 0; jumlah dokumen valid: 0.
Index korpus pengumpulan belum dibuat karena tidak ada artikel yang valid.
ZIP adalah paket program yang masih perlu dilengkapi, belum siap dikumpulkan.

IDENTITAS KELOMPOK — WAJIB DIISI
Nama kelompok/kelas: [ISI]
Jumlah anggota: [ISI]
Nama anggota | NIM
[ISI NAMA ANGGOTA] | [ISI NIM]
Tambahkan satu baris untuk setiap anggota; tidak ada identitas yang dikarang.
Setiap anggota mengumpulkan 5 artikel. Target = jumlah anggota x 5.
Program membaca semua file yang tersedia tanpa menetapkan jumlah hardcode.
Jumlah anggota dan kecukupan target diperiksa kelompok sebelum pengumpulan.

TUJUAN DAN METODE
Mesin pencari dokumen artikel ilmiah berbahasa Inggris tentang Automated
Short Answer Grading (ASAG). Artikel adalah korpus, bukan data pelatihan
model penilai jawaban. Pencarian memakai TF-IDF manual dan cosine similarity.
Tidak menggunakan embedding, BERT, LLM, API AI, database, atau server.
Kesamaan cosine adalah skor numerik; bukan probabilitas atau akurasi.

DAFTAR DOKUMEN DAN METADATA SUMBER
Kode | Judul | Nama file | URL sumber
[BELUM ADA ARTIKEL — ISI SETELAH ARTIKEL ASLI DITAMBAHKAN]

Metadata masukan: corpus/metadata.csv, dengan header:
filename,title,source_url

Isi satu baris per file menggunakan nama file yang persis sama, judul asli,
dan URL sumber asli. Gunakan tanda kutip CSV jika judul mengandung koma.
Judul kosong memakai nama file sebagai judul sementara. URL yang belum
diketahui dibiarkan kosong dan wajib dilengkapi; jangan mengarang URL.
Tidak ada contoh artikel/penulis/sumber palsu di dalam corpus.

Kode D01, D02, ... diberikan otomatis berdasarkan urutan nama semua file
artikel (case-insensitive, nama asli sebagai pembeda), bukan dari metadata.
File gagal dibaca tetap menempati nomor kandidat sehingga kode dapat
berlubang. N hanya menghitung dokumen yang berhasil dibaca dan punya token.
Kode dapat bergeser jika file baru ditambahkan lebih awal secara alfabetis.
Setelah membangun index, output/documents.csv memuat daftar aktual lengkap:
kode, judul, nama file, URL, dan jumlah token. Salin daftar tersebut ke bagian
ini sebelum pengumpulan, dan perbarui jumlah artikel/dokumen di atas.

LINGKUNGAN DAN PEMASANGAN
Diuji dengan Python 3.11.18 dan Tk 8.6 pada macOS (Apple Silicon).
Versi dependensi pengujian akhir: NumPy 2.4.6, NLTK 3.10.4, pypdf 6.21.2.
Pengujian logika awal juga lulus pada Python 3.9.6 dengan NumPy 2.0.2 dan
NLTK 3.9.2. requirements.txt berisi rentang kompatibel untuk pemasangan.
Kode memakai Python 3.9 atau lebih baru; Python 3.11+ disarankan untuk
lingkungan desktop. Gunakan Python dengan Tkinter.
Tkinter adalah bawaan distribusi Python, jangan pip install tkinter.
Pandas tidak diperlukan; tabel UI memakai ttk dan ekspor memakai modul csv.

Di terminal, masuk ke folder proyek lalu jalankan:
  python3.11 -m venv .venv
  source .venv/bin/activate
  python -m pip install -r requirements.txt
  python -m nltk.downloader stopwords
  python main.py

Gunakan nama executable Python yang sesuai instalasi pada komputer lain.
Windows: gunakan "python" sebagai pengganti "python3.11"; aktivasi:
  .venv\Scripts\activate

Unduh stopwords satu kali dengan interpreter/venv yang sama. Program tidak
mengunduh resource otomatis pada startup. Jika resource belum tersedia,
pesan pemasangan muncul di Catatan Proses atau terminal. Tidak perlu punkt.
Dalam lingkungan pengujian ini, 198 stopword Inggris NLTK sudah tersedia.

Jika import tkinter gagal, pasang/gunakan distribusi Python yang menyertakan
Tkinter (misalnya installer Python desktop), atau paket Tk dari pengelola
paket sistem Linux. Uji dukungan Tk dengan: python -m tkinter
Pada komputer pengembangan ini, Python sistem 3.9 tersendat saat membuka Tk.
Komponen python-tk@3.11 dipasang melalui Homebrew dan .venv telah dibuat ulang
memakai Python 3.11.18. Lingkungan lokal sudah dapat menjalankan widget Tk.
Untuk memakai lingkungan yang sudah disiapkan: .venv/bin/python main.py

MENJALANKAN DAN MEMAKAI APLIKASI
1. Masukkan artikel ke corpus dan isi corpus/metadata.csv.
2. Jalankan "python main.py". Judul: Pencarian Artikel ASAG.
3. Pilih Folder bila artikel ada di lokasi lain. Folder tersebut harus
   memiliki metadata.csv sendiri dengan format yang sama.
4. Klik Bangun Index. Jumlah dokumen valid dan catatan file ditampilkan.
5. Tulis kueri Inggris, misalnya frasa dari artikel, lalu Cari atau Enter.
6. Hasil diurutkan dari cosine terbesar; skor sama memakai urutan leksikal
   kode dokumen. Skor ditampilkan sebagai desimal empat angka.
7. Pilih hasil untuk pratinjau teks asli; Buka Dokumen memakai aplikasi
   default komputer. URL sumber tampil di panel bila metadata diisi.
8. Tab Lihat Index menyediakan TF dan TF-IDF, DF, IDF, serta scrollbar
   horizontal dan vertikal. Catatan Proses memuat kegagalan per file.

Pembacaan, preprocessing, dan ekspor dijalankan pada worker thread.
Pembaruan widget dilakukan di thread Tk utama melalui queue dan after.
Tabel index diisi bertahap agar antarmuka tetap dapat merespons.

Opsi lokasi (UI juga menerima kedua opsi folder):
  python main.py --corpus /lokasi/artikel --output /lokasi/hasil
Mode terminal tanpa membuka UI:
  python main.py --build-index
  python main.py --query "automated short answer grading"
Mode terminal membangun ulang index dari korpus, lalu mengekspor hasil.
Jika korpus kosong/tidak valid, program menjelaskan penyebab dan exit code 1.

MENAMBAHKAN DOKUMEN
- Gunakan artikel asli berbahasa Inggris yang membahas ASAG.
- Format: .txt UTF-8 atau PDF berbasis teks, langsung di folder corpus.
- Satu file = satu artikel = satu dokumen. Subfolder tidak diindeks.
- Jangan memecah artikel atau memakai salinan duplikat untuk mengejar target.
- Masukkan metadata sumber, lalu Bangun Index kembali setelah ada perubahan.
- Bahasa/topik/keaslian dan duplikasi perlu diperiksa kelompok secara manual;
  program tidak mengklaim memvalidasi isi ilmiah secara otomatis.
- File kosong, rusak, gagal decode UTF-8, atau tanpa token dilewati dengan
  pesan yang menyebut nama file; dokumen tersebut tidak termasuk N.
- PDF scan/kosong yang tidak memberi teks dijelaskan perlu OCR atau versi
  teks. Lakukan OCR di luar program, lalu gunakan PDF teks atau TXT hasilnya.
  Program tidak membuat teks pengganti dan tidak mengisi dokumen kosong.

PREPROCESSING DOKUMEN DAN KUERI
Keduanya memakai fungsi TextPreprocessor.process yang sama:
1. Case folding menjadi huruf kecil.
2. Whitespace dinormalisasi.
3. Semua karakter selain huruf ASCII a-z diganti pemisah spasi.
4. Tokenisasi dengan split whitespace.
5. Hapus stopword bahasa Inggris dari NLTK sebelum stemming.
6. Stemming dengan NLTK PorterStemmer.

Angka, tanda hubung, apostrof, tanda baca, dan huruf non-ASCII menjadi
pemisah, bukan digabung atau ditransliterasi. Contoh "short-answer" menjadi
"short", "answer"; "score42model" menjadi "score", "model". Token kemudian
dihapus bila stopword dan di-stem ("grading" menjadi "grade"). Jumlah
kemunculan token tetap dipertahankan untuk TF mentah. Teks asli disimpan
untuk pratinjau serta index_metadata.json dan tidak dipakai langsung untuk
perhitungan bobot.

RUMUS PERHITUNGAN
TF(t,d) = banyak kemunculan term t pada dokumen d setelah preprocessing.
DF(t) = banyak dokumen yang mengandung t; satu dokumen dihitung sekali.
N = jumlah dokumen valid (bukan banyak file gagal dan bukan termasuk kueri).
IDF(t) = log10(N / DF(t)).
TFIDF(t,d) = TF(t,d) x IDF(t).
TFIDF(t,q) = TF(t,q) x IDF(t), memakai IDF dan vocabulary dari korpus.
cos(q,d) = sum_i(q_i x d_i) /
           (sqrt(sum_i(q_i^2)) x sqrt(sum_i(d_i^2))).

TF dihitung dengan Counter. Vocabulary diurutkan alfabetis. Tidak ada
log-TF, smoothing IDF, konstanta +1, atau default TfidfVectorizer. Kueri
tidak ditambahkan ke dokumen dan tidak mengubah N/DF/IDF. Term kueri di luar
vocabulary diabaikan dan diberi keterangan. Norma dokumen menggunakan
seluruh fitur dokumen, termasuk fitur yang tidak terdapat pada kueri.

VEKTOR NOL DAN KONDISI KHUSUS
Term di semua dokumen memiliki IDF 0. Jika semua term suatu vektor berbobot
0, norma vektor adalah 0: cosine secara matematis tidak terdefinisi dan
program menetapkan skor operasional 0 untuk mencegah pembagian nol/NaN.
Satu dokumen valid juga menghasilkan semua IDF 0 dengan rumus wajib ini.
Jika seluruh skor 0, pesan "Tidak ditemukan kecocokan berbobot" ditampilkan;
daftar dokumen tetap dapat dipratinjau dengan skor 0, bukan kecocokan positif.
Kueri kosong atau hanya stopword/nonhuruf memberi pesan dan hasil kosong.
Kueri sebelum index, folder hilang/kosong, dan file tidak terbaca ditangani.

INDEX DAN TRANSPOSE
Matriks internal TF/TF-IDF berukuran dokumen x term. Untuk tampilan dan CSV
dipakai .T (transpose), sehingga baris = term dan kolom = kode dokumen.
DF dan IDF menjadi kolom tambahan agar perhitungan mudah diperiksa.
Bangun Index dengan dokumen valid menyimpan ke output:
- tf.csv: term, DF, IDF, TF untuk setiap kode dokumen.
- tfidf.csv: term, DF, IDF, bobot TF-IDF untuk setiap kode dokumen.
- documents.csv: pemetaan kode, judul, nama file, sumber, jumlah token.
- index_metadata.json: waktu UTC, rumus/kebijakan, jumlah, teks asli, token.
CSV memakai UTF-8 BOM agar mudah dibuka aplikasi spreadsheet; nilai ekspor
menyimpan presisi numerik lebih tinggi dari pembulatan tampilan UI.

Index dibangun ulang di memori saat tombol ditekan; tidak ada pemuatan cache
index yang berpotensi usang. Output adalah snapshot build terakhir yang
berhasil; build kosong/gagal tidak menghapus output lama. Periksa metadata,
bangun ulang dengan korpus final, dan pastikan output sesuai sebelum ZIP.
Pada proyek awal ini output belum berisi index karena korpus masih kosong.

PENGUJIAN DAN HASIL
Jalankan:
  python -m unittest discover -s tests -v
  python -m compileall -q main.py ui.py corpus.py preprocessing.py search_engine.py tests
Uji alur widget opsional, memerlukan Tk desktop dan resource stopwords:
  python -m tests.ui_smoke

30 pengujian lulus: preprocessing; instruksi resource hilang; sorting dan
metadata; folder kosong/hilang; UTF-8/file rusak; PDF teks dua halaman sebagai
satu dokumen; PDF kosong/scan; TF berulang; DF per dokumen; IDF log10 tanpa
smoothing; IDF semua-dokumen=0; bobot TF-IDF; vocabulary kueri konsisten;
kueri tidak mengubah index; cosine identik=1; norma dokumen lengkap;
vektor nol/satu dokumen; ranking/tie; kasus kueri; ekspor transpose/metadata.
PDF integrasi dibuat di folder sementara memakai pypdf, bukan artikel tugas.

Data uji kecil ada di tests/data, TERPISAH dari corpus:
D01: The common grading grading rubric.
D02: The common grading answer.
D03: The common answer.
Setelah preprocessing, vocabulary: answer, common, grade, rubric.
TF grade = [2,1,0]; DF grade = 2; IDF grade = log10(3/2)=0.1760912591.
DF common=3; IDF common=0; IDF rubric=log10(3)=0.4771212547.
Kueri grading: D02=0.7071067812; D01=0.5938758662; D03=0.
Contoh ini data buatan untuk pengujian hitung, bukan artikel ilmiah/korpus.

Mode terminal diuji dengan stopword Inggris NLTK asli, ekspor CSV/JSON dan
kueri fixture ke direktori sementara; hasil cocok dengan hitung manual.
Korpus tugas kosong diuji menghasilkan penjelasan dan tidak membuat index.
Pemeriksaan sintaks semua modul lulus. Uji alur widget Tk juga lulus pada
Python 3.11.18: instansiasi aplikasi, kueri sebelum index, build worker,
pencarian/ranking, pratinjau, tabel TF, skor nol, dan rebuild korpus kosong.
tests/ui_smoke.py memakai jendela tersembunyi dan output sementara, tanpa
membuka dokumen pada aplikasi lain. Ini menguji fungsi widget secara otomatis.
UI belum diperiksa secara visual karena akses computer-use tidak tersedia.

STRUKTUR FOLDER
  main.py                   entry point UI dan CLI
  preprocessing.py          normalisasi, stopword NLTK, PorterStemmer
  corpus.py                 pembacaan TXT/PDF, metadata, validasi file
  search_engine.py          TF/DF/IDF/TF-IDF, cosine, ranking, ekspor
  ui.py                     Tkinter/ttk, tabel, pratinjau, worker queue
  build_zip.py              pembuat ZIP dengan daftar file terbatas
  requirements.txt          dependensi pip (tanpa tkinter)
  README.txt                petunjuk dan kelengkapan tugas
  corpus/metadata.csv       metadata sumber; isi artikel di corpus/
  output/                   hasil index korpus setelah build berhasil
  tests/test_*.py           pengujian unittest
  tests/ui_smoke.py          uji alur widget opsional dengan Tk desktop
  tests/data/doc_*.txt       fixture hitung; bukan artikel pengumpulan
  .venv/                    lingkungan lokal, tidak masuk ZIP

ZIP DAN KELENGKAPAN PENGUMPULAN
Buat ulang setelah artikel, metadata, dan identitas dilengkapi:
  python build_zip.py
Nama: tugas_nlp_pencarian_asag.zip.
Arsip berisi source, README, requirements, tests terpisah, korpus, metadata,
dan hasil index jika tersedia. Venv, cache, .git, rahasia, file sementara,
symlink, dan ZIP lain tidak disertakan. Jika memakai Pilih Folder di luar
proyek, salin artikel dan metadata aslinya ke corpus sebelum membuat paket.

Yang masih harus dilengkapi:
- Nama/NIM seluruh anggota dan jumlah anggota.
- Artikel asli sebanyak jumlah anggota x 5 (saat ini 0).
- Judul dan URL sumber asli tiap artikel pada metadata.csv.
- Bangun index dari korpus final; perbarui daftar/jumlah pada README.
- Pemeriksaan tampilan UI secara visual (fungsi widget otomatis sudah lulus).
- Buat ulang ZIP setelah data final dimasukkan.
