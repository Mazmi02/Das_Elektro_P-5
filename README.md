<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu Basis Data Relasional (Relational Database)?", "id": "Basis Data Tabel Berelasi." },
  { "en": "Apa Itu Model Data Relasional (Relational Model)?", "id": "Model Data Berbasis Relasi (Tabel)." },
  { "en": "Apa Itu Tabel (Table) Basis Data?", "id": "Kumpulan Data Terstruktur Baris Kolom." },
  { "en": "Apa Nama Lain Tabel (Table)?", "id": "Relasi (Relation)." },
  { "en": "Apa Itu Baris (Row) Tabel?", "id": "Satu Entri Data (Record/Tuple)." },
  { "en": "Apa Nama Lain Baris (Row)?", "id": "Record (Rekaman) Atau Tuple." },
  { "en": "Apa Itu Kolom (Column) Tabel?", "id": "Atribut Data Spesifik (Field)." },
  { "en": "Apa Nama Lain Kolom (Column)?", "id": "Atribut (Attribute) Atau Field (Bidang)." },
  { "en": "Apa Itu Kunci Primer (Primary Key)?", "id": "Kolom Unik Identifikasi Baris Tabel." },
  { "en": "Apa Sifat Kunci Primer (Primary Key)?", "id": "Unik Dan Tidak Boleh Null." },
  { "en": "Apa Itu Kunci Asing (Foreign Key)?", "id": "Kolom Referensi Kunci Primer Tabel Lain." },
  { "en": "Apa Fungsi Kunci Asing (Foreign Key)?", "id": "Membangun Hubungan (Relasi) Antar Tabel." },
  { "en": "Apa Itu Integritas Referensial (Referential Integrity)?", "id": "Aturan Jaga Konsistensi Kunci Asing." },
  { "en": "Apa Itu Skema (Schema) Basis Data?", "id": "Struktur Logis Keseluruhan Basis Data." },
  { "en": "Apa Itu SQL (Structured Query Language)?", "id": "Bahasa Standar Basis Data Relasional." },
  { "en": "Apa Kepanjangan SQL (Structured Query Language)?", "id": "Bahasa Kueri Terstruktur." },
  { "en": "Apa Perintah SQL (Structured Query Language) Untuk Memilih Data?", "id": "Perintah SELECT." },
  { "en": "Apa Perintah SQL (Structured Query Language) Memasukkan Data?", "id": "Perintah INSERT INTO." },
  { "en": "Apa Perintah SQL (Structured Query Language) Memperbarui Data?", "id": "Perintah UPDATE." },
  { "en": "Apa Perintah SQL (Structured Query Language) Menghapus Data?", "id": "Perintah DELETE FROM." },
  { "en": "Apa Perintah SQL (Structured Query Language) Membuat Tabel?", "id": "Perintah CREATE TABLE." },
  { "en": "Apa Perintah SQL (Structured Query Language) Mengubah Tabel?", "id": "Perintah ALTER TABLE." },
  { "en": "Apa Perintah SQL (Structured Query Language) Menghapus Tabel?", "id": "Perintah DROP TABLE." },
  { "en": "Apa Itu Klausa WHERE (WHERE Clause) SQL?", "id": "Filter Baris Data Berdasarkan Kondisi." },
  { "en": "Apa Itu Klausa ORDER BY (ORDER BY Clause) SQL?", "id": "Mengurutkan Hasil Kueri Data." },
  { "en": "Apa Itu Klausa GROUP BY (GROUP BY Clause) SQL?", "id": "Mengelompokkan Baris Data Nilai Sama." },
  { "en": "Apa Fungsi Agregat (Aggregate Function) SQL?", "id": "Hitungan Kumpulan Baris (COUNT, SUM)." },
  { "en": "Apa Itu JOIN (JOIN Operation) SQL?", "id": "Menggabungkan Baris Dari Dua Tabel." },
  { "en": "Jenis JOIN (JOIN Operation) SQL Umum?", "id": "INNER JOIN, LEFT JOIN, RIGHT JOIN." },
  { "en": "Apa Itu INNER JOIN (INNER JOIN)?", "id": "Mengembalikan Baris Cocok Kedua Tabel." },
  { "en": "Apa Itu LEFT JOIN (LEFT JOIN)?", "id": "Semua Baris Kiri, Cocok Kanan." },
  { "en": "Apa Itu RIGHT JOIN (RIGHT JOIN)?", "id": "Semua Baris Kanan, Cocok Kiri." },
  { "en": "Apa Itu FULL OUTER JOIN (FULL OUTER JOIN)?", "id": "Semua Baris Kiri, Kanan Digabung." },
  { "en": "Apa Itu Subquery (Subquery) SQL?", "id": "Kueri Di Dalam Kueri SQL Lain." },
  { "en": "Apa Itu Indeks (Index) Basis Data?", "id": "Struktur Data Percepat Pencarian Data." },
  { "en": "Apa Trade-off (Trade-off) Penggunaan Indeks?", "id": "Percepat SELECT, Perlambat INSERT/UPDATE." },
  { "en": "Apa Itu Normalisasi (Normalization) Basis Data?", "id": "Proses Mengurangi Redundansi Data." },
  { "en": "Apa Tujuan Normalisasi (Normalization)?", "id": "Hindari Anomali Data, Jaga Konsistensi." },
  { "en": "Apa Itu Bentuk Normal Pertama (1NF)?", "id": "First Normal Form (1NF)." },
  { "en": "Syarat Bentuk Normal Pertama (1NF)?", "id": "Nilai Kolom Atomik (Tidak Multi-Value)." },
  { "en": "Apa Itu Bentuk Normal Kedua (2NF)?", "id": "Second Normal Form (2NF)." },
  { "en": "Syarat Bentuk Normal Kedua (2NF)?", "id": "1NF, Dependensi Fungsional Penuh." },
  { "en": "Apa Itu Bentuk Normal Ketiga (3NF)?", "id": "Third Normal Form (3NF)." },
  { "en": "Syarat Bentuk Normal Ketiga (3NF)?", "id": "2NF, Tidak Ada Dependensi Transitif." },
  { "en": "Apa Itu Bentuk Normal Boyce-Codd (BCNF)?", "id": "Boyce-Codd Normal Form (BCNF)." },
  { "en": "Apa Kepanjangan BCNF (Boyce-Codd Normal Form)?", "id": "Bentuk Normal Boyce-Codd." },
  { "en": "Apa Itu Denormalisasi (Denormalization)?", "id": "Menambah Redundansi Tingkatkan Kinerja Baca." },
  { "en": "Apa Itu Transaksi (Transaction) Basis Data?", "id": "Satu Unit Kerja Logis (Atomik)." },
  { "en": "Apa Sifat Transaksi (Transaction) ACID?", "id": "Atomicity, Consistency, Isolation, Durability." },
  { "en": "Apa Kepanjangan ACID (Atomicity Consistency Isolation Durability)?", "id": "Atomisitas, Konsistensi, Isolasi, Durabilitas." },
  { "en": "Apa Itu Atomisitas (Atomicity)?", "id": "Transaksi Berhasil Semua Atau Gagal Semua." },
  { "en": "Apa Itu Konsistensi (Consistency)?", "id": "Transaksi Jaga Basis Data Tetap Valid." },
  { "en": "Apa Itu Isolasi (Isolation)?", "id": "Transaksi Terisolasi Satu Sama Lain." },
  { "en": "Apa Itu Durabilitas (Durability)?", "id": "Hasil Transaksi Sukses Permanen." },
  { "en": "Apa Itu Commit (Commit) Transaksi?", "id": "Menyimpan Perubahan Transaksi Permanen." },
  { "en": "Apa Itu Rollback (Rollback) Transaksi?", "id": "Membatalkan Perubahan Transaksi Gagal." },
  { "en": "Apa Itu Concurrency Control (Kontrol Konkurensi)?", "id": "Mengelola Akses Simultan Basis Data." },
  { "en": "Masalah Apa Akibat Konkurensi Buruk?", "id": "Lost Update, Dirty Read, Phantom Read." },
  { "en": "Apa Itu Locking (Penguncian) Basis Data?", "id": "Mekanisme Kontrol Konkurensi (Kunci Baris/Tabel)." },
  { "en": "Apa Itu Deadlock (Deadlock) Basis Data?", "id": "Transaksi Saling Tunggu Lepas Kunci." },
  { "en": "Apa Itu Sistem Manajemen Basis Data (DBMS)?", "id": "Database Management System (DBMS)." },
  { "en": "Apa Kepanjangan DBMS (Database Management System)?", "id": "Sistem Manajemen Basis Data." },
  { "en": "Apa Fungsi DBMS (Database Management System)?", "id": "Membuat, Mengelola, Mengakses Basis Data." },
  { "en": "Contoh DBMS (Database Management System) Relasional?", "id": "MySQL, PostgreSQL, Oracle, SQL Server." },
  { "en": "Contoh DBMS (Database Management System) NoSQL?", "id": "MongoDB, Cassandra, Redis, Neo4j." },
  { "en": "Apa Itu Object-Relational Mapping (ORM)?", "id": "Teknik Pemetaan Objek Ke Tabel Relasional." },
  { "en": "Apa Kepanjangan ORM (Object-Relational Mapping)?", "id": "Pemetaan Objek Relasional." },
  { "en": "Contoh Library ORM (Object-Relational Mapping)?", "id": "Hibernate (Java), SQLAlchemy (Python)." },
  { "en": "Apa Itu Gudang Data (Data Warehouse)?", "id": "Basis Data Besar Analisis Historis." },
  { "en": "Apa Itu Data Mart (Data Mart)?", "id": "Subset Gudang Data Departemen Tertentu." },
  { "en": "Apa Itu ETL (Extract Transform Load)?", "id": "Proses Memuat Data Ke Gudang Data." },
  { "en": "Apa Itu Online Analytical Processing (OLAP)?", "id": "Analisis Data Multi Dimensi Cepat." },
  { "en": "Apa Kepanjangan OLAP (Online Analytical Processing)?", "id": "Pemrosesan Analitik Online." },
  { "en": "Apa Itu Kubus OLAP (OLAP Cube)?", "id": "Struktur Data Multi Dimensi OLAP." },
  { "en": "Apa Itu Online Transaction Processing (OLTP)?", "id": "Pemrosesan Transaksi Basis Data Operasional." },
  { "en": "Apa Kepanjangan OLTP (Online Transaction Processing)?", "id": "Pemrosesan Transaksi Online." },
  { "en": "Apa Beda OLTP (Online Transaction Processing), OLAP (Online Analytical Processing)?", "id": "OLTP (Operasional Cepat), OLAP (Analitik Kompleks)." },
  { "en": "Apa Itu Data Mining (Penambangan Data)?", "id": "Menemukan Pola Tersembunyi Data Besar." },
  { "en": "Teknik Data Mining (Penambangan Data) Umum?", "id": "Klasifikasi, Clustering, Asosiasi." },
  { "en": "Apa Itu Aturan Asosiasi (Association Rule)?", "id": "Menemukan Hubungan Antar Item Data." },
  { "en": "Contoh Aturan Asosiasi (Association Rule)?", "id": "Analisis Keranjang Pasar (Market Basket)." },
  { "en": "Apa Itu Big Data (Big Data)?", "id": "Volume Data Sangat Besar, Kompleks." },
  { "en": "Apa Karakteristik Big Data (Big Data) (Vs)?", "id": "Volume, Velocity, Variety, Veracity, Value." },
  { "en": "Apa Itu Hadoop (Hadoop)?", "id": "Kerangka Kerja Pemrosesan Big Data." },
  { "en": "Apa Itu HDFS (Hadoop Distributed File System)?", "id": "Sistem Berkas Terdistribusi Hadoop." },
  { "en": "Apa Kepanjangan HDFS (Hadoop Distributed File System)?", "id": "Sistem Berkas Terdistribusi Hadoop." },
  { "en": "Apa Itu MapReduce (MapReduce)?", "id": "Model Pemrograman Pemrosesan Data Paralel." },
  { "en": "Apa Itu Spark (Apache Spark)?", "id": "Mesin Pemrosesan Big Data Cepat In-Memory." },
  { "en": "Apa Itu Basis Data NoSQL (Not Only SQL)?", "id": "Basis Data Non-Relasional Skalabilitas Tinggi." },
  { "en": "Tipe Basis Data NoSQL (Not Only SQL)?", "id": "Dokumen, Key-Value, Kolom Lebar, Graf." },
  { "en": "Apa Itu MongoDB (MongoDB)?", "id": "Basis Data NoSQL (Not Only SQL) Berorientasi Dokumen." },
  { "en": "Apa Itu Redis (Redis)?", "id": "Basis Data NoSQL (Not Only SQL) Key-Value In-Memory." },
  { "en": "Apa Itu Cassandra (Cassandra)?", "id": "Basis Data NoSQL Kolom Lebar Terdistribusi." },
  { "en": "Apa Itu Neo4j (Neo4j)?", "id": "Basis Data NoSQL (Not Only SQL) Berorientasi Graf." },
  { "en": "Apa Itu Teorema CAP (CAP Theorem)?", "id": "Konsistensi, Ketersediaan, Toleransi Partisi." },
  { "en": "Apa Kepanjangan CAP (Consistency Availability Partition Tolerance)?", "id": "Konsistensi, Ketersediaan, Toleransi Partisi." },
  { "en": "Apa Maksud Teorema CAP (CAP Theorem)?", "id": "Sistem Terdistribusi Pilih Maksimal Dua." },
  { "en": "Apa Itu Konsistensi Akhirnya (Eventual Consistency)?", "id": "Data Akan Konsisten Seiring Waktu." },
  { "en": "Apa Itu Sharding (Sharding) Basis Data?", "id": "Membagi Data Horizontal Antar Server." },
  { "en": "Apa Itu Replikasi (Replication) Basis Data?", "id": "Menyalin Data Ke Server Lain." },
  { "en": "Tujuan Replikasi (Replication) Basis Data?", "id": "Ketersediaan Tinggi, Skalabilitas Baca." },
  { "en": "Apa Itu Master-Slave Replication (Replikasi Master-Slave)?", "id": "Satu Master (Tulis), Banyak Slave (Baca)." },
  { "en": "Apa Itu Master-Master Replication (Replikasi Master-Master)?", "id": "Beberapa Master (Tulis) Sinkronisasi." },
  { "en": "Apa Itu Caching (Caching) Basis Data?", "id": "Menyimpan Data Sering Diakses Memori Cepat." },
  { "en": "Contoh Sistem Caching (Caching)?", "id": "Redis (Redis), Memcached (Memcached)." },
  { "en": "Apa Itu Pemrosesan Sinyal (Signal Processing)?", "id": "Analisis, Modifikasi Sinyal Informasi." },
  { "en": "Apa Itu Domain Waktu (Time Domain)?", "id": "Representasi Sinyal Terhadap Waktu." },
  { "en": "Apa Itu Domain Frekuensi (Frequency Domain)?", "id": "Representasi Sinyal Terhadap Frekuensi." },
  { "en": "Transformasi Apa Ubah Waktu Ke Frekuensi?", "id": "Transformasi Fourier (Fourier Transform)." },
  { "en": "Apa Itu Sinyal Kontinu (Continuous Signal)?", "id": "Sinyal Terdefinisi Setiap Saat." },
  { "en": "Apa Itu Sinyal Diskrit (Discrete Signal)?", "id": "Sinyal Terdefinisi Waktu Tertentu (Sampel)." },
  { "en": "Apa Itu Pencuplikan (Sampling)?", "id": "Mengambil Sampel Sinyal Kontinu." },
  { "en": "Apa Itu Teorema Pencuplikan Nyquist (Nyquist Sampling)?", "id": "Fs Minimal Dua Kali Fmax." },
  { "en": "Apa Itu Aliasing (Aliasing)?", "id": "Distorsi Akibat Sampling Terlalu Rendah." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Pembulatan Nilai Sampel Ke Level Diskrit." },
  { "en": "Apa Itu Kesalahan Kuantisasi (Quantization Error)?", "id": "Selisih Nilai Asli, Hasil Kuantisasi." },
  { "en": "Apa Itu Filter Digital (Digital Filter)?", "id": "Memproses Sinyal Digital Ubah Spektrum." },
  { "en": "Apa Itu Filter FIR (Finite Impulse Response)?", "id": "Respon Impuls Berhingga (Tanpa Feedback)." },
  { "en": "Apa Itu Filter IIR (Infinite Impulse Response)?", "id": "Respon Impuls Tak Berhingga (Feedback)." },
  { "en": "Apa Itu Transformasi Fourier Cepat (FFT)?", "id": "Algoritma Efisien Hitung DFT." },
  { "en": "Apa Aplikasi FFT (Fast Fourier Transform)?", "id": "Analisis Spektrum, Kompresi Sinyal." },
  { "en": "Apa Itu Konvolusi (Convolution)?", "id": "Operasi Matematis (Output Sistem LTI)." },
  { "en": "Apa Itu Korelasi (Correlation)?", "id": "Ukuran Kemiripan Dua Sinyal." },
  { "en": "Apa Itu Autokorelasi (Autocorrelation)?", "id": "Korelasi Sinyal Dengan Dirinya Sendiri." },
  { "en": "Apa Itu Korelasi Silang (Cross-Correlation)?", "id": "Korelasi Antara Dua Sinyal Berbeda." },
  { "en": "Apa Itu Sistem Linear (Linear System)?", "id": "Memenuhi Prinsip Superposisi." },
  { "en": "Apa Itu Sistem Invarian Waktu (Time-Invariant)?", "id": "Respon Tidak Berubah Geseran Waktu." },
  { "en": "Apa Itu Sistem LTI (Linear Time-Invariant)?", "id": "Sistem Linear Dan Invarian Waktu." },
  { "en": "Apa Itu Respon Impuls (Impulse Response) h(t)?", "id": "Output Sistem LTI Jika Input Impuls." },
  { "en": "Apa Itu Elektromagnetisme (Electromagnetism)?", "id": "Studi Medan Listrik Dan Magnet." },
  { "en": "Apa Itu Persamaan Maxwell (Maxwell's Equations)?", "id": "Hukum Dasar Elektromagnetisme." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Getaran Medan E, B Merambat." },
  { "en": "Apa Kecepatan Gelombang EM (Electromagnetic) Di Vakum?", "id": "Kecepatan Cahaya (c)." },
  { "en": "Apa Itu Spektrum Elektromagnetik (Electromagnetic Spectrum)?", "id": "Rentang Frekuensi Gelombang EM." },
  { "en": "Apa Itu Propagasi (Propagation) Gelombang?", "id": "Perambatan Gelombang Lewat Medium." },
  { "en": "Apa Itu Atenuasi (Attenuation)?", "id": "Pelemahan Amplitudo Gelombang." },
  { "en": "Apa Itu Dispersi (Dispersion)?", "id": "Penyebaran Gelombang Berdasarkan Frekuensi." },
  { "en": "Apa Itu Refleksi (Reflection) Gelombang?", "id": "Pemantulan Gelombang Batas Medium." },
  { "en": "Apa Itu Refraksi (Refraction) Gelombang?", "id": "Pembelokan Gelombang Antar Medium." },
  { "en": "Apa Itu Difraksi (Diffraction) Gelombang?", "id": "Pelenturan Gelombang Sekitar Hambatan." },
  { "en": "Apa Itu Interferensi (Interference) Gelombang?", "id": "Superposisi Dua Gelombang Atau Lebih." },
  { "en": "Apa Itu Polarisasi (Polarization) Gelombang?", "id": "Orientasi Getaran Gelombang Transversal." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Transduser Gelombang EM, Arus Listrik." },
  { "en": "Apa Itu Pola Radiasi (Radiation Pattern)?", "id": "Distribusi Daya Dipancarkan Antena." },
  { "en": "Apa Itu Penguatan (Gain) Antena?", "id": "Ukuran Keterarahan Antena." },
  { "en": "Apa Itu Direktifitas (Directivity) Antena?", "id": "Rasio Intensitas Maksimum, Rata-Rata." },
  { "en": "Apa Itu Efisiensi (Efficiency) Antena?", "id": "Rasio Daya Radiasi, Daya Input." },
  { "en": "Apa Itu Impedansi Input (Input Impedance) Antena?", "id": "Impedansi Terminal Input Antena." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Antena?", "id": "Rentang Frekuensi Operasi Antena." },
  { "en": "Apa Itu Polarisasi (Polarization) Antena?", "id": "Polarisasi Gelombang Dipancarkan/Diterima." },
  { "en": "Apa Itu Antena Dipol (Dipole Antenna)?", "id": "Antena Dasar Dua Elemen Konduktif." },
  { "en": "Apa Itu Antena Monopol (Monopole Antenna)?", "id": "Setengah Dipol Di Atas Ground Plane." },
  { "en": "Apa Itu Antena Loop (Loop Antenna)?", "id": "Antena Berbentuk Loop Konduktor." },
  { "en": "Apa Itu Antena Yagi-Uda (Yagi-Uda Antenna)?", "id": "Antena Direktif (Driven, Reflector, Director)." },
  { "en": "Apa Itu Antena Parabola (Parabolic Antenna)?", "id": "Antena Reflektor Penguatan Sangat Tinggi." },
  { "en": "Apa Itu Antena Array (Antenna Array)?", "id": "Kumpulan Elemen Antena Bekerja Sama." },
  { "en": "Apa Itu Beamforming (Beamforming) Antena Array?", "id": "Mengatur Fasa Arahkan Pancaran Utama." },
  { "en": "Apa Itu MIMO (Multiple Input Multiple Output)?", "id": "Banyak Antena Pemancar, Penerima." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Struktur Pemandu Gelombang EM." },
  { "en": "Contoh Saluran Transmisi (Transmission Line)?", "id": "Kabel Koaksial, Twisted Pair, Microstrip." },
  { "en": "Apa Itu Impedansi Karakteristik (Characteristic Impedance) (Z₀)?", "id": "Impedansi Khas Saluran Transmisi." },
  { "en": "Apa Itu Koefisien Refleksi (Reflection Coefficient) (Γ)?", "id": "Rasio Gelombang Pantul Terhadap Datang." },
  { "en": "Kapan Refleksi (Reflection) Terjadi?", "id": "Saat Impedansi Beban Tidak Sama Z₀." },
  { "en": "Apa Itu VSWR (Voltage Standing Wave Ratio)?", "id": "Ukuran Ketidakcocokan Impedansi." },
  { "en": "VSWR (Voltage Standing Wave Ratio) Ideal Sama Dengan Berapa?", "id": "Satu (Pencocokan Sempurna)." },
  { "en": "Apa Itu Pencocokan Impedansi (Impedance Matching)?", "id": "Membuat Z Beban = Z₀* (Konjugat)." },
  { "en": "Metode Pencocokan Impedansi (Impedance Matching)?", "id": "Stub Tuning, Trafo Lambda/4." },
  { "en": "Apa Itu Transformator Lambda Perempat (Quarter-Wave Transformer)?", "id": "Saluran Lambda/4 Untuk Matching." },
  { "en": "Apa Itu Smith Chart (Bagan Smith)?", "id": "Alat Grafis Analisis Saluran Transmisi." },
  { "en": "Apa Itu Parameter S (Scattering Parameters)?", "id": "Parameter Karakterisasi Jaringan RF." },
  { "en": "Apa Itu Microwave (Gelombang Mikro)?", "id": "Gelombang EM Frekuensi Sangat Tinggi." },
  { "en": "Rentang Frekuensi Microwave (Gelombang Mikro)?", "id": "300 MHz Hingga 300 GHz." },
  { "en": "Komponen Apa Digunakan Di Microwave?", "id": "Waveguide, Resonator Rongga, Dioda Gunn." },
  { "en": "Apa Itu Waveguide (Pemandu Gelombang)?", "id": "Pipa Logam Pemandu Gelombang Mikro." },
  { "en": "Apa Itu Resonator Rongga (Cavity Resonator)?", "id": "Rongga Logam Resonansi Frekuensi Mikro." },
  { "en": "Apa Itu Radar (Radio Detection and Ranging)?", "id": "Sistem Deteksi Objek Gelombang Radio." },
  { "en": "Prinsip Dasar Radar (Radio Detection and Ranging)?", "id": "Memancarkan Pulsa, Deteksi Pantulan." },
  { "en": "Apa Itu Komunikasi Satelit (Satellite Communication)?", "id": "Komunikasi Menggunakan Satelit Buatan." },
  { "en": "Apa Itu Orbit Geostasioner (Geostationary Orbit)?", "id": "Satelit Tampak Diam Dari Bumi." },
  { "en": "Apa Itu Uplink (Uplink) Satelit?", "id": "Transmisi Dari Bumi Ke Satelit." },
  { "en": "Apa Itu Downlink (Downlink) Satelit?", "id": "Transmisi Dari Satelit Ke Bumi." },
  { "en": "Apa Itu Transponder (Transponder) Satelit?", "id": "Penerima, Penguat, Pemancar Di Satelit." },
  { "en": "Apa Itu Komunikasi Serat Optik (Fiber Optic)?", "id": "Transmisi Data Menggunakan Cahaya Serat." },
  { "en": "Apa Keunggulan Serat Optik (Fiber Optic)?", "id": "Bandwidth Besar, Atenuasi Rendah, Kebal EMI." },
  { "en": "Apa Prinsip Dasar Serat Optik?", "id": "Pemantulan Internal Total." },
  { "en": "Apa Itu Serat Optik Single Mode (Mode Tunggal)?", "id": "Inti Kecil, Jarak Jauh." },
  { "en": "Apa Itu Serat Optik Multi Mode (Mode Jamak)?", "id": "Inti Besar, Jarak Pendek." },
  { "en": "Sumber Cahaya (Light Source) Serat Optik?", "id": "Laser Diode (LD), LED." },
  { "en": "Detektor Cahaya (Light Detector) Serat Optik?", "id": "Fotodioda (PIN Atau APD)." },
  { "en": "Apa Itu WDM (Wavelength Division Multiplexing)?", "id": "Mengirim Banyak Lambda Satu Serat." },
  { "en": "Apa Itu Penguat Optik (Optical Amplifier)?", "id": "Menguatkan Sinyal Cahaya Langsung (EDFA)." },
  { "en": "Apa Itu Komunikasi Data (Data Communication)?", "id": "Transfer Data Digital Antar Perangkat." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Kumpulan Komputer Terhubung Berbagi Sumber." },
  { "en": "Apa Itu Protokol (Protocol) Jaringan?", "id": "Aturan Komunikasi Antar Perangkat." },
  { "en": "Apa Itu Model OSI (Open Systems Interconnection)?", "id": "Model Referensi Tujuh Lapisan Jaringan." },
  { "en": "Apa Itu Model TCP/IP (TCP/IP Model)?", "id": "Model Empat/Lima Lapisan Internet." },
  { "en": "Lapisan Apa Bertanggung Jawab Pengalamatan Logis?", "id": "Lapisan Jaringan (Network Layer) (IP)." },
  { "en": "Lapisan Apa Bertanggung Jawab Pengiriman Andal?", "id": "Lapisan Transport (Transport Layer) (TCP)." },
  { "en": "Apa Itu Alamat IP (IP Address)?", "id": "Alamat Logis Unik Perangkat Jaringan." },
  { "en": "Apa Itu Alamat MAC (MAC Address)?", "id": "Alamat Fisik Unik Kartu Jaringan." },
  { "en": "Apa Itu Ethernet (Ethernet)?", "id": "Teknologi Jaringan LAN Kabel Umum." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Teknologi Jaringan LAN Nirkabel Umum." },
  { "en": "Apa Itu Router (Router)?", "id": "Perangkat Menghubungkan Jaringan Berbeda." },
  { "en": "Apa Itu Switch (Saklar)?", "id": "Perangkat Menghubungkan Perangkat Satu LAN." },
  { "en": "Apa Itu Hub (Hub)?", "id": "Perangkat Penghubung LAN Sederhana (Lapisan Fisik)." },
  { "en": "Apa Itu Firewall (Firewall)?", "id": "Sistem Keamanan Penyaring Lalu Lintas Jaringan." },
  { "en": "Apa Itu Kabel UTP (Unshielded Twisted Pair)?", "id": "Kabel Ethernet Umum Tanpa Shielding." },
  { "en": "Apa Itu Konektor RJ-45 (Registered Jack 45)?", "id": "Konektor Standar Kabel Ethernet." },
  { "en": "Apa Itu SSID (Service Set Identifier)?", "id": "Nama Jaringan Wi-Fi." },
  { "en": "Apa Itu WPA2/WPA3 (Wi-Fi Protected Access)?", "id": "Protokol Keamanan Jaringan Wi-Fi." },
  { "en": "Apa Itu Osilasi (Oscillation) Relaksasi?", "id": "Osilasi Non-Sinusoidal (Pengisian Kapasitor)." },
  { "en": "Apa Itu Siklus Kerja (Duty Cycle) 50 Persen?", "id": "Waktu Tinggi Sama Waktu Rendah." },
  { "en": "Apa Itu Frekuensi Sudut (Angular Frequency) (ω)?", "id": "Laju Perubahan Fasa (Radian/Detik)." },
  { "en": "Apa Hubungan ω (Omega) Dan f (Frekuensi)?", "id": "ω = 2πf." },
  { "en": "Apa Itu Representasi Fasor (Phasor Representation)?", "id": "Amplitudo Dan Sudut Fasa." },
  { "en": "Bagaimana Menjumlahkan Fasor (Phasor)?", "id": "Jumlahkan Komponen Riil Dan Imajiner." },
  { "en": "Apa Itu Daya Rata-Rata (Average Power) Resistor?", "id": "P = Irms² * R." },
  { "en": "Apa Daya Rata-Rata (Average Power) Induktor Ideal?", "id": "Nol Watt." },
  { "en": "Apa Daya Rata-Rata (Average Power) Kapasitor Ideal?", "id": "Nol Watt." },
  { "en": "Komponen Apa Menyerap Daya Nyata (Real Power)?", "id": "Resistor (Hambatan Murni)." },
  { "en": "Komponen Apa Menyimpan Daya Reaktif (Reactive Power)?", "id": "Induktor Dan Kapasitor." },
  { "en": "Apa Satuan Energi (Energy)?", "id": "Joule (J)." },
  { "en": "Apa Hubungan Energi (Energy), Daya (Power), Waktu (Time)?", "id": "Energi = Daya x Waktu." },
  { "en": "Apa Itu Nilai Puncak Ke Puncak (Peak-To-Peak)?", "id": "Selisih Antara Puncak Positif, Negatif." },
  { "en": "Apa Rumus Nilai Puncak Ke Puncak Sinus?", "id": "Vpp = 2 * Vpuncak." },
  { "en": "Apa Itu Respons Impuls (Impulse Response)?", "id": "Output Sistem Terhadap Input Impuls." },
  { "en": "Apa Itu Konvolusi (Convolution)?", "id": "Operasi Matematis Output Sistem LTI." },
  { "en": "Apa Itu Transformasi Laplace (Laplace Transform)?", "id": "Alat Analisis Rangkaian Domain-s." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function)?", "id": "Rasio Output Terhadap Input Domain-s." },
  { "en": "Apa Itu Pole (Kutub) Fungsi Transfer?", "id": "Frekuensi Kompleks Menyebabkan Respon Tak Terhingga." },
  { "en": "Apa Itu Zero (Nol) Fungsi Transfer?", "id": "Frekuensi Kompleks Menyebabkan Respon Nol." },
  { "en": "Apa Kondisi Kestabilan (Stability) Sistem Domain-s?", "id": "Semua Pole Di Sisi Kiri Bidang-s." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response)?", "id": "Respon Sistem Terhadap Frekuensi Berbeda." },
  { "en": "Apa Itu Plot Bode (Bode Plot)?", "id": "Grafik Magnitudo, Fasa Vs Frekuensi." },
  { "en": "Apa Satuan Magnitudo (Magnitude) Plot Bode?", "id": "Desibel (dB)." },
  { "en": "Apa Itu Desibel (Decibel) (dB)?", "id": "Satuan Logaritmik Rasio Daya/Amplitudo." },
  { "en": "Rumus Desibel (Decibel) Untuk Rasio Tegangan?", "id": "20 * log10 (Vout / Vin)." },
  { "en": "Rumus Desibel (Decibel) Untuk Rasio Daya?", "id": "10 * log10 (Pout / Pin)." },
  { "en": "Apa Itu Frekuensi Sudut (Corner Frequency) Bode?", "id": "Frekuensi Batas Perubahan Kemiringan (-3dB)." },
  { "en": "Apa Itu Kemiringan (Slope) Asimtot Bode?", "id": "Kelipatan ±20 dB Per Dekade." },
  { "en": "Apa Itu Dekade (Decade) Frekuensi?", "id": "Perubahan Frekuensi Sepuluh Kali Lipat." },
  { "en": "Apa Itu Oktaf (Octave) Frekuensi?", "id": "Perubahan Frekuensi Dua Kali Lipat." },
  { "en": "Berapa Kemiringan (Slope) Per Oktaf?", "id": "Kelipatan ±6 dB Per Oktaf." },
  { "en": "Apa Itu Jaringan Dua Port (Two-Port Network)?", "id": "Kotak Hitam Dengan Dua Pasang Terminal." },
  { "en": "Parameter Apa Mengkarakterisasi Jaringan Dua Port?", "id": "Parameter Z, Y, h, ABCD." },
  { "en": "Apa Itu Parameter Impedansi (Z-parameters)?", "id": "Menggambarkan Tegangan Sebagai Fungsi Arus." },
  { "en": "Apa Itu Parameter Admitansi (Y-parameters)?", "id": "Menggambarkan Arus Sebagai Fungsi Tegangan." },
  { "en": "Apa Itu Parameter Hibrid (h-parameters)?", "id": "Campuran (Berguna Analisis Transistor)." },
  { "en": "Apa Itu Parameter Transmisi (ABCD-parameters)?", "id": "Menghubungkan Input Ke Output (Kaskade)." },
  { "en": "Apa Itu Dioda Terobosan Balik (Breakdown Diode)?", "id": "Dioda Beroperasi Daerah Breakdown Mundur." },
  { "en": "Apa Itu Efek Zener (Zener Effect)?", "id": "Breakdown Tegangan Rendah (Medan Listrik Kuat)." },
  { "en": "Apa Itu Efek Avalanche (Avalanche Effect)?", "id": "Breakdown Tegangan Tinggi (Tabrakan Ionisasi)." },
  { "en": "Apa Itu Kapasitansi Difusi (Diffusion Capacitance)?", "id": "Kapasitansi Dioda Saat Bias Maju." },
  { "en": "Apa Itu Kapasitansi Transisi (Transition Capacitance)?", "id": "Kapasitansi Dioda Saat Bias Mundur." },
  { "en": "Nama Lain Kapasitansi Transisi (Transition Capacitance)?", "id": "Kapasitansi Daerah Deplesi." },
  { "en": "Apa Itu Waktu Pemulihan Mundur (Reverse Recovery Time)?", "id": "Waktu Dioda Kembali Blokir Arus." },
  { "en": "Apa Itu Arus Puncak Maju (Peak Forward Current)?", "id": "Arus Maju Maksimum Sesaat Dioda." },
  { "en": "Apa Itu Arus Rata-Rata Maju (Average Forward Current)?", "id": "Arus Maju Rata-Rata Maksimum Dioda." },
  { "en": "Apa Itu Disipasi Daya (Power Dissipation) Dioda?", "id": "Daya Maksimum Dapat Dihilangkan Dioda." },
  { "en": "Bagaimana Bias Sambungan Basis-Emitor (BE) Aktif?", "id": "Diberi Bias Maju (Forward Bias)." },
  { "en": "Bagaimana Bias Sambungan Basis-Kolektor (BC) Aktif?", "id": "Diberi Bias Mundur (Reverse Bias)." },
  { "en": "Apa Itu Penguatan Alfa (Alpha) (α) BJT?", "id": "Rasio Arus Kolektor Terhadap Emitor (Ic/Ie)." },
  { "en": "Apa Nilai Tipikal Alfa (Alpha) (α)?", "id": "Sedikit Kurang Dari Satu." },
  { "en": "Apa Itu Penguatan Beta (Beta) (β) BJT?", "id": "Rasio Arus Kolektor Terhadap Basis (Ic/Ib)." },
  { "en": "Apa Nilai Tipikal Beta (Beta) (β)?", "id": "Puluhan Hingga Ratusan." },
  { "en": "Apa Hubungan Antara Alfa (α) Dan Beta (β)?", "id": "β = α / (1 - α)." },
  { "en": "Apa Itu Konfigurasi Common Emitter (Common Emitter) (CE)?", "id": "Emitor Bersama (Input Basis, Output Kolektor)." },
  { "en": "Karakteristik Penguat Common Emitter (Common Emitter) (CE)?", "id": "Penguatan Tegangan, Arus Tinggi, Fasa Terbalik." },
  { "en": "Apa Itu Konfigurasi Common Collector (Common Collector) (CC)?", "id": "Kolektor Bersama (Input Basis, Output Emitor)." },
  { "en": "Karakteristik Penguat Common Collector (Common Collector) (CC)?", "id": "Penguatan Tegangan ≈1, Arus Tinggi (Buffer)." },
  { "en": "Apa Itu Konfigurasi Common Base (Common Base) (CB)?", "id": "Basis Bersama (Input Emitor, Output Kolektor)." },
  { "en": "Karakteristik Penguat Common Base (Common Base) (CB)?", "id": "Penguatan Arus ≈1, Tegangan Tinggi." },
  { "en": "Apa Itu Garis Beban DC (DC Load Line)?", "id": "Grafik Hubungan Ic Vs Vce Statis." },
  { "en": "Apa Itu Titik Q (Quiescent Point)?", "id": "Titik Operasi DC Transistor (Bias)." },
  { "en": "Mengapa Perlu Pembiasan (Biasing) Transistor?", "id": "Menempatkan Titik Q Daerah Aktif." },
  { "en": "Metode Pembiasan (Biasing) BJT (Bipolar Junction Transistor) Umum?", "id": "Bias Tetap, Pembagi Tegangan, Umpan Balik Emitor." },
  { "en": "Metode Pembiasan (Biasing) Mana Paling Stabil?", "id": "Bias Pembagi Tegangan." },
  { "en": "Apa Itu Kestabilan Termal (Thermal Stability)?", "id": "Kemampuan Jaga Titik Q Stabil Suhu." },
  { "en": "Apa Itu Thermal Runaway (Lolos Termal)?", "id": "Peningkatan Suhu Rusak Transistor." },
  { "en": "Apa Itu Analisis Sinyal Kecil (Small Signal)?", "id": "Analisis Respon Penguat Sinyal AC Kecil." },
  { "en": "Apa Itu Model Hybrid-pi (Hybrid-pi Model)?", "id": "Model Ekuivalen Transistor Sinyal Kecil." },
  { "en": "Apa Itu Parameter h (h-parameters)?", "id": "Parameter Jaringan Dua Port (Sinyal Kecil)." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency) Penguat?", "id": "Frekuensi Penguatan Turun 3dB." },
  { "en": "Apa Itu Penguat Multitingkat (Multistage Amplifier)?", "id": "Beberapa Tingkat Penguat Dihubungkan Kaskade." },
  { "en": "Bagaimana Penguatan Total (Total Gain) Kaskade?", "id": "Hasil Kali Penguatan Tiap Tingkat." },
  { "en": "Apa Jenis Kopling (Coupling) Antar Tingkat?", "id": "Kopling RC, Kopling Trafo, Kopling Langsung." },
  { "en": "Apa Keuntungan Kopling RC (RC Coupling)?", "id": "Sederhana, Respons Frekuensi Baik." },
  { "en": "Apa Keuntungan Kopling Trafo (Transformer Coupling)?", "id": "Pencocokan Impedansi, Isolasi DC." },
  { "en": "Apa Keuntungan Kopling Langsung (Direct Coupling)?", "id": "Respons Frekuensi Rendah Baik (DC)." },
  { "en": "Apa Itu Penguat Daya (Power Amplifier)?", "id": "Menguatkan Daya Sinyal Ke Beban." },
  { "en": "Kelas Penguat Daya (Power Amplifier) Berdasarkan Bias?", "id": "Kelas A, B, AB, C, D." },
  { "en": "Penguat Kelas A (Class A Amplifier)?", "id": "Konduksi 360 Derajat (Linearitas Baik)." },
  { "en": "Efisiensi (Efficiency) Penguat Kelas A?", "id": "Sangat Rendah (Maksimum 50%)." },
  { "en": "Penguat Kelas B (Class B Amplifier)?", "id": "Konduksi 180 Derajat (Push-Pull)." },
  { "en": "Efisiensi (Efficiency) Penguat Kelas B?", "id": "Lebih Tinggi Dari Kelas A (Maks 78.5%)." },
  { "en": "Masalah Penguat Kelas B (Class B)?", "id": "Distorsi Crossover (Crossover Distortion)." },
  { "en": "Penguat Kelas AB (Class AB Amplifier)?", "id": "Konduksi Sedikit Lebih 180 Derajat." },
  { "en": "Tujuan Penguat Kelas AB (Class AB)?", "id": "Mengurangi Distorsi Crossover Kelas B." },
  { "en": "Penguat Kelas C (Class C Amplifier)?", "id": "Konduksi Kurang Dari 180 Derajat." },
  { "en": "Efisiensi (Efficiency) Penguat Kelas C?", "id": "Sangat Tinggi (Tapi Non-Linear)." },
  { "en": "Dimana Penguat Kelas C (Class C) Digunakan?", "id": "Penguat RF (Frekuensi Radio) Terdistel." },
  { "en": "Penguat Kelas D (Class D Amplifier)?", "id": "Penguat Switching (PWM)." },
  { "en": "Efisiensi (Efficiency) Penguat Kelas D?", "id": "Sangat Tinggi (Bisa Di Atas 90%)." },
  { "en": "Dimana Penguat Kelas D (Class D) Digunakan?", "id": "Penguat Audio Daya Tinggi." },
  { "en": "Apa Itu Heat Sink (Heat Sink)?", "id": "Komponen Pembuang Panas Transistor Daya." },
  { "en": "Mengapa Transistor Daya Perlu Heat Sink?", "id": "Mencegah Kerusakan Akibat Overheating." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Mengurangi Gain, Tingkatkan Stabilitas, Bandwidth." },
  { "en": "Apa Itu Umpan Balik Positif (Positive Feedback)?", "id": "Meningkatkan Gain (Bisa Osilasi)." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Penguat Dengan Umpan Balik Positif." },
  { "en": "Apa Kriteria Osilasi Barkhausen (Barkhausen Criterion)?", "id": "Gain Loop = 1, Fasa Loop = 0°." },
  { "en": "Apa Itu Osilator (Oscillator) LC?", "id": "Osilator Gunakan Induktor, Kapasitor." },
  { "en": "Contoh Osilator (Oscillator) LC?", "id": "Hartley, Colpitts, Clapp." },
  { "en": "Apa Itu Osilator (Oscillator) RC?", "id": "Osilator Gunakan Resistor, Kapasitor." },
  { "en": "Contoh Osilator (Oscillator) RC?", "id": "Geser Fasa, Jembatan Wien." },
  { "en": "Apa Itu Osilator (Oscillator) Kristal?", "id": "Osilator Sangat Stabil Frekuensinya." },
  { "en": "Apa Bahan Dasar Kristal (Crystal)?", "id": "Kuarsa (Quartz) Piezoelektrik." },
  { "en": "Apa Itu Efek Piezoelektrik (Piezoelectric Effect)?", "id": "Tekanan Mekanis Hasilkan Listrik." },
  { "en": "Apa Itu Frekuensi Resonansi (Resonance Frequency) Kristal?", "id": "Frekuensi Getaran Mekanis Kristal." },
  { "en": "Apa Keunggulan Osilator (Oscillator) Kristal?", "id": "Stabilitas Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem Umpan Balik Kunci Fasa." },
  { "en": "Apa Fungsi Utama PLL (Phase-Locked Loop)?", "id": "Sintesis Frekuensi, Demodulasi FM." },
  { "en": "Komponen Utama PLL (Phase-Locked Loop)?", "id": "Detektor Fasa, Filter Loop, VCO." },
  { "en": "Apa Itu Filter Aktif (Active Filter)?", "id": "Filter Gunakan Komponen Aktif (Op-Amp)." },
  { "en": "Apa Tipe Filter (Filter) Aktif Umum?", "id": "Sallen-Key, Multiple Feedback." },
  { "en": "Apa Itu Topologi Sallen-Key (Sallen-Key Topology)?", "id": "Topologi Filter Aktif Populer." },
  { "en": "Apa Itu Topologi Multiple Feedback (MFB)?", "id": "Topologi Filter Aktif Lainnya." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Konverter Analog Ke Digital." },
  { "en": "Apa Itu Proses Sampling (Sampling) ADC?", "id": "Mengambil Nilai Sinyal Interval Waktu." },
  { "en": "Apa Itu Proses Kuantisasi (Quantization) ADC?", "id": "Pembulatan Nilai Sampel Level Diskrit." },
  { "en": "Apa Itu Resolusi (Resolution) ADC?", "id": "Jumlah Bit Output Digital." },
  { "en": "Resolusi (Resolution) Lebih Tinggi Berarti Apa?", "id": "Representasi Digital Lebih Akurat." },
  { "en": "Apa Itu Laju Sampel (Sample Rate) ADC?", "id": "Jumlah Sampel Diambil Per Detik." },
  { "en": "Apa Itu Teorema Nyquist (Nyquist Theorem)?", "id": "Fs Harus Lebih Besar 2Fmax." },
  { "en": "Apa Itu Aliasing (Aliasing) ADC?", "id": "Frekuensi Tinggi Muncul Frekuensi Rendah." },
  { "en": "Bagaimana Mencegah Aliasing (Aliasing)?", "id": "Gunakan Filter Anti-Aliasing (Low-Pass)." },
  { "en": "Apa Tipe ADC (Analog-to-Digital Converter) Utama?", "id": "Flash, SAR, Dual-Slope, Sigma-Delta." },
  { "en": "ADC (Analog-to-Digital Converter) Tipe Flash (Flash ADC)?", "id": "Sangat Cepat (Banyak Komparator)." },
  { "en": "ADC (Analog-to-Digital Converter) Tipe SAR (Successive Approximation)?", "id": "Keseimbangan Kecepatan, Resolusi Baik." },
  { "en": "ADC (Analog-to-Digital Converter) Tipe Dual-Slope (Dual-Slope ADC)?", "id": "Lambat Tapi Akurasi Tinggi." },
  { "en": "ADC (Analog-to-Digital Converter) Tipe Sigma-Delta (Sigma-Delta ADC)?", "id": "Resolusi Sangat Tinggi (Oversampling)." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Konverter Digital Ke Analog." },
  { "en": "Apa Tipe DAC (Digital-to-Analog Converter) Utama?", "id": "Weighted Resistor, R-2R Ladder." },
  { "en": "DAC (Digital-to-Analog Converter) Tipe R-2R Ladder (R-2R Ladder DAC)?", "id": "Menggunakan Jaringan Resistor R, 2R." },
  { "en": "Apa Itu Resolusi (Resolution) DAC?", "id": "Jumlah Bit Input Digital." },
  { "en": "Apa Itu Linearitas (Linearity) ADC/DAC?", "id": "Seberapa Lurus Respon Konversi." },
  { "en": "Apa Itu Monotonisitas (Monotonicity) DAC?", "id": "Output Selalu Naik Sesuai Input." },
  { "en": "Apa Itu Settling Time (Waktu Turun) DAC?", "id": "Waktu Output Capai Nilai Stabil." },
  { "en": "Apa Itu Glitch (Glitch) DAC?", "id": "Spike Output Sesaat Transisi Kode." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "CPU (Central Processing Unit) Dalam Satu Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Satu Chip (CPU, Memori)." },
  { "en": "Apa Perbedaan Utama Mikroprosesor, Mikrokontroler?", "id": "Mikrokontroler Lebih Terintegrasi Periferal." },
  { "en": "Apa Itu Arsitektur Von Neumann (Von Neumann Architecture)?", "id": "Memori Bersama Data, Instruksi." },
  { "en": "Apa Itu Arsitektur Harvard (Harvard Architecture)?", "id": "Memori Terpisah Data, Instruksi." },
  { "en": "Arsitektur Mana Umum Di Mikrokontroler?", "id": "Arsitektur Harvard (Harvard Architecture)." },
  { "en": "Apa Itu Memori Program (Program Memory)?", "id": "Menyimpan Kode Instruksi (Flash/ROM)." },
  { "en": "Apa Itu Memori Data (Data Memory)?", "id": "Menyimpan Variabel Data (RAM)." },
  { "en": "Apa Itu Peripheral (Peripheral) Mikrokontroler?", "id": "Modul Fungsi Tambahan (Timer, ADC)." },
  { "en": "Apa Itu GPIO (General Purpose Input Output)?", "id": "Pin Serbaguna (Input Atau Output)." },
  { "en": "Apa Itu Timer (Timer) Mikrokontroler?", "id": "Penghitung Waktu Internal." },
  { "en": "Apa Itu Counter (Counter) Mikrokontroler?", "id": "Penghitung Pulsa Eksternal." },
  { "en": "Apa Itu PWM (Pulse Width Modulation) Output?", "id": "Sinyal Kotak Siklus Kerja Variabel." },
  { "en": "Apa Fungsi PWM (Pulse Width Modulation) Mikrokontroler?", "id": "Kontrol Kecerahan LED, Kecepatan Motor." },
  { "en": "Apa Itu Komunikasi Serial (Serial Communication)?", "id": "Pengiriman Data Bit Per Bit." },
  { "en": "Apa Itu UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Komunikasi Serial Asinkron." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Komunikasi Serial Sinkron Cepat." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Komunikasi Serial Dua Kawat Multi-Perangkat." },
  { "en": "Apa Itu Interrupt (Interupsi)?", "id": "Sinyal Meminta Perhatian CPU." },
  { "en": "Apa Itu Watchdog Timer (WDT)?", "id": "Timer Reset Jika Program Macet." },
  { "en": "Apa Itu Bahasa Assembly (Assembly Language)?", "id": "Bahasa Pemrograman Level Rendah Mnemonik." },
  { "en": "Apa Itu Bahasa C (C Language)?", "id": "Bahasa Pemrograman Populer Mikrokontroler." },
  { "en": "Apa Itu Kompilator (Compiler)?", "id": "Penerjemah Kode Sumber Ke Mesin." },
  { "en": "Apa Itu Debugger (Debugger) In-Circuit?", "id": "Alat Debug Hardware/Software Tertanam." },
  { "en": "Apa Itu IDE (Integrated Development Environment)?", "id": "Lingkungan Pengembangan Terpadu Software." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Mengubah Besaran Fisik Ke Sinyal Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Mengubah Sinyal Listrik Ke Aksi Fisik." },
  { "en": "Apa Itu Sensor Suhu (Temperature Sensor)?", "id": "Mengukur Panas Atau Dingin." },
  { "en": "Jenis Sensor Suhu (Temperature Sensor)?", "id": "Termokopel, RTD, Termistor, IC Sensor." },
  { "en": "Apa Itu Sensor Cahaya (Light Sensor)?", "id": "Mengukur Intensitas Cahaya." },
  { "en": "Jenis Sensor Cahaya (Light Sensor)?", "id": "LDR, Fotodioda, Fototransistor." },
  { "en": "Apa Itu Sensor Jarak (Distance Sensor)?", "id": "Mengukur Jarak Ke Objek." },
  { "en": "Jenis Sensor Jarak (Distance Sensor)?", "id": "Ultrasonik, Inframerah (IR), Laser." },
  { "en": "Apa Itu Motor DC (Direct Current)?", "id": "Motor Penggerak Arus Searah." },
  { "en": "Bagaimana Mengontrol Kecepatan Motor DC?", "id": "Mengubah Tegangan Atau PWM." },
  { "en": "Bagaimana Mengontrol Arah Motor DC?", "id": "Membalik Polaritas Tegangan (H-Bridge)." },
  { "en": "Apa Itu H-Bridge (Jembatan H)?", "id": "Rangkaian Driver Kontrol Arah Motor." },
  { "en": "Apa Itu Motor Servo (Servo Motor)?", "id": "Motor Kontrol Posisi Sudut Presisi." },
  { "en": "Bagaimana Sinyal Kontrol Motor Servo RC?", "id": "Sinyal PWM (Pulse Width Modulation) Lebar Pulsa Variabel." },
  { "en": "Apa Itu Motor Stepper (Stepper Motor)?", "id": "Motor Bergerak Dalam Langkah Sudut." },
  { "en": "Bagaimana Mengontrol Motor Stepper (Stepper Motor)?", "id": "Memberi Urutan Pulsa Ke Fasa." },
  { "en": "Apa Itu Driver (Driver) Motor Stepper?", "id": "IC (Integrated Circuit) Pengontrol Urutan Pulsa Stepper." },
  { "en": "Apa Itu Relay (Relay)?", "id": "Saklar Elektromekanis Kontrol Beban Besar." },
  { "en": "Apa Itu Solenoida (Solenoid)?", "id": "Aktuator Elektromagnetik Gerak Linear." },
  { "en": "Apa Itu Elektromagnetisme (Electromagnetism)?", "id": "Hubungan Antara Listrik Dan Magnet." },
  { "en": "Siapa Menemukan Hubungan Listrik Magnet?", "id": "Hans Christian Oersted." },
  { "en": "Apa Itu Aturan Tangan Kanan (Right Hand Rule)?", "id": "Menentukan Arah Medan Magnet Sekitar Arus." },
  { "en": "Apa Itu Induksi Elektromagnetik (Electromagnetic Induction)?", "id": "Pembangkitan Listrik Medan Magnet Berubah." },
  { "en": "Siapa Penemu Induksi Elektromagnetik (Electromagnetic Induction)?", "id": "Michael Faraday." },
  { "en": "Apa Itu Hukum Faraday (Faraday's Law) Induksi?", "id": "GGL Induksi Proposional Laju Perubahan Fluks." },
  { "en": "Apa Itu Hukum Lenz (Lenz's Law)?", "id": "Arah Arus Induksi Melawan Perubahan Penyebab." },
  { "en": "Apa Aplikasi Induksi Elektromagnetik (Electromagnetic Induction)?", "id": "Generator, Transformator, Induktor." },
  { "en": "Apa Itu Fluks (Flux) Magnetik?", "id": "Jumlah Garis Medan Magnet Menembus Luas." },
  { "en": "Apa Satuan Fluks (Flux) Magnetik?", "id": "Weber (Wb)." },
  { "en": "Apa Itu Kerapatan Fluks (Flux Density) Magnetik (B)?", "id": "Kekuatan Medan Magnet Per Luas." },
  { "en": "Apa Satuan Kerapatan Fluks (Flux Density) Magnetik (B)?", "id": "Tesla (T)." },
  { "en": "Apa Itu Permeabilitas (Permeability) Magnetik (μ)?", "id": "Kemampuan Bahan Mendukung Medan Magnet." },
  { "en": "Apa Itu Bahan Feromagnetik (Ferromagnetic Material)?", "id": "Bahan Mudah Dimagnetisasi (Besi)." },
  { "en": "Apa Itu Loop Histeresis (Hysteresis Loop)?", "id": "Grafik Hubungan B Vs H Bahan Feromagnetik." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Alat Statis Ubah Tegangan AC." },
  { "en": "Prinsip Kerja Transformator (Transformer)?", "id": "Induksi Mutual Antara Dua Kumparan." },
  { "en": "Apa Itu Inti (Core) Transformator?", "id": "Bahan Feromagnetik Jalur Fluks Magnet." },
  { "en": "Mengapa Inti (Core) Trafo Berlaminasi?", "id": "Mengurangi Rugi Arus Eddy." },
  { "en": "Apa Itu Rugi-Rugi (Losses) Transformator?", "id": "Rugi Tembaga Dan Rugi Inti." },
  { "en": "Apa Itu Efisiensi (Efficiency) Transformator?", "id": "Rasio Daya Output Terhadap Input." },
  { "en": "Apa Itu Autotransformator (Autotransformer)?", "id": "Transformator Hanya Satu Belitan." },
  { "en": "Apa Itu Instrument Transformer (Trafo Instrumen)?", "id": "Trafo Arus (CT), Trafo Tegangan (PT)." },
  { "en": "Apa Fungsi Trafo Arus (CT)?", "id": "Menurunkan Arus Tinggi Pengukuran/Proteksi." },
  { "en": "Apa Fungsi Trafo Tegangan (PT)?", "id": "Menurunkan Tegangan Tinggi Pengukuran/Proteksi." },
  { "en": "Apa Itu Mesin Listrik (Electrical Machine)?", "id": "Konverter Energi Listrik-Mekanik." },
  { "en": "Apa Dua Jenis Utama Mesin Listrik?", "id": "Motor Dan Generator." },
  { "en": "Apa Itu Motor Induksi (Induction Motor)?", "id": "Motor AC Paling Umum Industri." },
  { "en": "Apa Itu Slip (Slip) Motor Induksi?", "id": "Perbedaan Kecepatan Rotor, Medan Putar." },
  { "en": "Apa Itu Kecepatan Sinkron (Synchronous Speed)?", "id": "Kecepatan Putaran Medan Magnet Stator." },
  { "en": "Apa Itu Rotor Sangkar Tupai (Squirrel Cage)?", "id": "Jenis Rotor Motor Induksi Umum." },
  { "en": "Apa Itu Motor Sinkron (Synchronous Motor)?", "id": "Motor Berputar Kecepatan Sinkron." },
  { "en": "Apa Fungsi Motor Sinkron (Synchronous Motor)?", "id": "Beban Kecepatan Konstan, Koreksi Faktor Daya." },
  { "en": "Apa Itu Motor DC (Direct Current)?", "id": "Motor Sumber Arus Searah." },
  { "en": "Apa Bagian Utama Motor DC (Direct Current)?", "id": "Stator (Medan), Rotor (Jangkar), Komutator." },
  { "en": "Apa Fungsi Komutator (Commutator) Motor DC?", "id": "Membalik Arus Jangkar Hasilkan Torsi Searah." },
  { "en": "Apa Jenis Motor DC (Direct Current) Utama?", "id": "Shunt, Seri, Kompon." },
  { "en": "Apa Karakteristik Motor DC (Direct Current) Seri?", "id": "Torsi Start Tinggi, Kecepatan Variabel." },
  { "en": "Apa Karakteristik Motor DC (Direct Current) Shunt?", "id": "Kecepatan Relatif Konstan." },
  { "en": "Apa Itu Generator (Generator)?", "id": "Mengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Jenis Generator (Generator) Utama?", "id": "Generator DC, Generator AC (Alternator)." },
  { "en": "Apa Itu Alternator (Alternator)?", "id": "Generator Arus Bolak-Balik (AC)." },
  { "en": "Apa Itu Sistem Tenaga Listrik (Power System)?", "id": "Pembangkitan, Transmisi, Distribusi Listrik." },
  { "en": "Apa Itu Jaringan Transmisi (Transmission Grid)?", "id": "Saluran Tegangan Tinggi Jarak Jauh." },
  { "en": "Apa Itu Jaringan Distribusi (Distribution Grid)?", "id": "Saluran Tegangan Menengah/Rendah Ke Konsumen." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Transformasi Tegangan, Switching." },
  { "en": "Apa Itu Sistem Proteksi (Protection System)?", "id": "Melindungi Peralatan Dari Gangguan Listrik." },
  { "en": "Apa Komponen Sistem Proteksi (Protection System)?", "id": "Rele (Relay), Pemutus Sirkuit (Circuit Breaker)." },
  { "en": "Apa Fungsi Rele Proteksi (Protection Relay)?", "id": "Mendeteksi Kondisi Abnormal (Gangguan)." },
  { "en": "Apa Fungsi Pemutus Sirkuit (Circuit Breaker)?", "id": "Memutus Aliran Arus Saat Gangguan." },
  { "en": "Apa Itu Gangguan Hubung Singkat (Short Circuit Fault)?", "id": "Kontak Langsung Antar Fasa/Fasa-Tanah." },
  { "en": "Apa Itu Gangguan Beban Lebih (Overload Fault)?", "id": "Arus Melebihi Kapasitas Peralatan." },
  { "en": "Apa Itu Sistem Grounding (Grounding System)?", "id": "Koneksi Ke Tanah Untuk Keamanan." },
  { "en": "Apa Itu Elektrostatika (Electrostatics)?", "id": "Studi Muatan Listrik Diam." },
  { "en": "Apa Itu Magnetostatika (Magnetostatics)?", "id": "Studi Medan Magnet Arus Konstan." },
  { "en": "Apa Itu Teknik Tegangan Tinggi (High Voltage)?", "id": "Studi Pembangkitan, Pengukuran, Isolasi HV." },
  { "en": "Apa Itu Isolator (Insulator) Tegangan Tinggi?", "id": "Bahan Menahan Tegangan Tinggi (Porselen, Kaca)." },
  { "en": "Apa Itu Bushing (Bushing) Tegangan Tinggi?", "id": "Isolator Lewatkan Konduktor Tembok/Tanki." },
  { "en": "Apa Itu Corona Discharge (Pelepasan Korona)?", "id": "Ionisasai Udara Sekitar Konduktor HV." },
  { "en": "Apa Itu Flashover (Loncatan Api) Isolator?", "id": "Busur Api Melompati Permukaan Isolator." },
  { "en": "Apa Itu Breakdown (Tembus) Isolasi?", "id": "Kegagalan Material Isolasi Tahan Tegangan." },
  { "en": "Apa Itu Pengujian Tegangan Tinggi (High Voltage Testing)?", "id": "Menguji Ketahanan Isolasi Peralatan HV." },
  { "en": "Apa Itu Pengujian Impuls (Impulse Testing)?", "id": "Menguji Ketahanan Terhadap Tegangan Surja Petir." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Aplikasi Elektronika Kontrol Daya Listrik." },
  { "en": "Apa Komponen Utama Elektronika Daya?", "id": "Saklar Semikonduktor (Dioda, Thyristor, IGBT)." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Mengubah AC Ke DC." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Mengubah DC Ke AC." },
  { "en": "Apa Itu Chopper (Chopper) DC?", "id": "Mengubah Level Tegangan DC (Konverter DC-DC)." },
  { "en": "Apa Itu Cycloconverter (Cycloconverter)?", "id": "Mengubah Frekuensi AC Langsung." },
  { "en": "Apa Itu Thyristor (Thyristor) / SCR?", "id": "Saklar Daya Terkontrol Gerbang (Searah)." },
  { "en": "Apa Itu TRIAC (Triode for AC)?", "id": "Saklar Daya AC Dua Arah Terkontrol." },
  { "en": "Apa Itu GTO (Gate Turn-Off Thyristor)?", "id": "Thyristor Bisa Dimatikan Gerbang." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Saklar Daya Gabungan MOSFET, BJT." },
  { "en": "Apa Keunggulan IGBT (Insulated Gate Bipolar Transistor)?", "id": "Kemampuan Daya Tinggi, Switching Cepat." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Kontrol Saklar Elektronika Daya." },
  { "en": "Apa Fungsi PWM (Pulse Width Modulation) Elektronika Daya?", "id": "Mengatur Tegangan/Arus Output Rata-Rata." },
  { "en": "Apa Itu Konverter Buck (Buck Converter)?", "id": "Penurun Tegangan DC Switching." },
  { "en": "Apa Itu Konverter Boost (Boost Converter)?", "id": "Penaik Tegangan DC Switching." },
  { "en": "Apa Itu Konverter Buck-Boost (Buck-Boost Converter)?", "id": "Penaik/Penurun Tegangan DC (Inverting)." },
  { "en": "Apa Itu Konverter Flyback (Flyback Converter)?", "id": "Konverter DC-DC Terisolasi (Topologi Buck-Boost)." },
  { "en": "Apa Itu Konverter Forward (Forward Converter)?", "id": "Konverter DC-DC Terisolasi (Topologi Buck)." },
  { "en": "Apa Itu SMPS (Switch Mode Power Supply)?", "id": "Catu Daya Switching Efisiensi Tinggi." },
  { "en": "Apa Keuntungan SMPS (Switch Mode Power Supply)?", "id": "Efisiensi Tinggi, Ukuran Kecil." },
  { "en": "Apa Kerugian SMPS (Switch Mode Power Supply)?", "id": "Noise Output Lebih Tinggi." },
  { "en": "Apa Itu Filter EMI (Electromagnetic Interference) SMPS?", "id": "Mengurangi Gangguan Elektromagnetik." },
  { "en": "Apa Itu Koreksi Faktor Daya (PFC)?", "id": "Memperbaiki Faktor Daya Input AC." },
  { "en": "Mengapa PFC (Power Factor Correction) Penting?", "id": "Mengurangi Harmonisa, Meningkatkan Efisiensi." },
  { "en": "Apa Itu Variable Frequency Drive (VFD)?", "id": "Pengatur Kecepatan Motor AC (Ubah Frekuensi)." },
  { "en": "Apa Aplikasi VFD (Variable Frequency Drive)?", "id": "Pompa, Kipas, Konveyor, Mesin Industri." },
  { "en": "Apa Itu Soft Starter (Soft Starter)?", "id": "Mengurangi Arus Start Motor AC." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply)?", "id": "Catu Daya Tak Terputus (Cadangan Baterai)." },
  { "en": "Apa Fungsi UPS (Uninterruptible Power Supply)?", "id": "Memberi Daya Saat Listrik Padam." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengelola Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Sistem Loop Terbuka (Open Loop)?", "id": "Kontrol Tanpa Umpan Balik." },
  { "en": "Apa Itu Sistem Loop Tertutup (Closed Loop)?", "id": "Kontrol Dengan Umpan Balik Output." },
  { "en": "Apa Keuntungan Loop Tertutup (Closed Loop)?", "id": "Lebih Akurat, Tahan Gangguan." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Mengukur Output, Bandingkan Setpoint." },
  { "en": "Apa Itu Setpoint (Setpoint)?", "id": "Nilai Target Yang Diinginkan." },
  { "en": "Apa Itu Kesalahan (Error)?", "id": "Selisih Antara Setpoint, Output Aktual." },
  { "en": "Apa Itu Kontroler (Controller)?", "id": "Memproses Error, Hasilkan Sinyal Kontrol." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Perangkat Mengubah Sinyal Kontrol Aksi Fisik." },
  { "en": "Apa Itu Proses (Process) / Plant?", "id": "Sistem Fisik Yang Dikontrol." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Mengukur Variabel Proses (Output)." },
  { "en": "Apa Itu Kontroler On-Off (On-Off Controller)?", "id": "Kontroler Sederhana Dua Keadaan." },
  { "en": "Apa Itu Kontroler PID (Proportional Integral Derivative)?", "id": "Kontroler Umpan Balik Paling Umum." },
  { "en": "Apa Aksi Kontrol Proporsional (P)?", "id": "Output Proporsional Terhadap Error." },
  { "en": "Apa Aksi Kontrol Integral (I)?", "id": "Menghilangkan Error Keadaan Tunak." },
  { "en": "Apa Aksi Kontrol Derivatif (D)?", "id": "Memberi Respon Antisipatif Perubahan Error." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function)?", "id": "Representasi Matematis Sistem Domain-s." },
  { "en": "Apa Itu Kestabilan (Stability) Sistem Kontrol?", "id": "Kemampuan Sistem Kembali Seimbang." },
  { "en": "Metode Analisis Kestabilan (Stability Analysis)?", "id": "Routh-Hurwitz, Root Locus, Nyquist, Bode." },
  { "en": "Apa Itu Respon Transien (Transient Response)?", "id": "Perilaku Sistem Menuju Keadaan Tunak." },
  { "en": "Parameter Respon Transien (Transient Response)?", "id": "Rise Time, Settling Time, Overshoot." },
  { "en": "Apa Itu Error Keadaan Tunak (Steady-State Error)?", "id": "Error Setelah Sistem Stabil." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response)?", "id": "Respon Sistem Terhadap Input Sinusoidal." },
  { "en": "Apa Itu Diagram Bode (Bode Plot)?", "id": "Plot Magnitudo, Fasa Respon Frekuensi." },
  { "en": "Apa Itu Gain Margin (GM)?", "id": "Ukuran Kestabilan Relatif (Gain)." },
  { "en": "Apa Itu Phase Margin (PM)?", "id": "Ukuran Kestabilan Relatif (Fasa)." },
  { "en": "Apa Itu Kontrol Digital (Digital Control)?", "id": "Implementasi Kontroler Komputer Digital." },
  { "en": "Apa Itu Pencuplikan (Sampling) Kontrol Digital?", "id": "Mengambil Sampel Sinyal Kontinu." },
  { "en": "Apa Itu Penahan Orde Nol (Zero Order Hold)?", "id": "Rekonstruksi Sinyal Output DAC." },
  { "en": "Apa Itu Transformasi Z (Z-Transform)?", "id": "Alat Analisis Sistem Waktu Diskrit." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Digital Industri." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Pengawasan, Akuisisi Data Terpusat." },
  { "en": "Apa Itu DCS (Distributed Control System)?", "id": "Sistem Kontrol Terdistribusi Proses Industri." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Bidang Terkait Desain, Operasi Robot." },
  { "en": "Apa Itu Derajat Kebebasan (DOF) Robot?", "id": "Jumlah Gerakan Independen." },
  { "en": "Apa Itu Kinematika Maju (Forward Kinematics)?", "id": "Hitung Posisi End-Effector Dari Sendi." },
  { "en": "Apa Itu Kinematika Balik (Inverse Kinematics)?", "id": "Hitung Sudut Sendi Dari Posisi End-Effector." },
  { "en": "Apa Itu Sensor Robot (Robot Sensor)?", "id": "Memberi Informasi Lingkungan, Internal." },
  { "en": "Apa Itu Aktuator Robot (Robot Actuator)?", "id": "Penggerak Sendi Robot (Motor)." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Pemahaman Komputer Terhadap Gambar." },
  { "en": "Aplikasi Visi Komputer (Computer Vision) Robot?", "id": "Navigasi, Pengenalan Objek, Inspeksi." },
  { "en": "Apa Itu Pemrosesan Sinyal (Signal Processing)?", "id": "Analisis Dan Modifikasi Sinyal." },
  { "en": "Apa Itu Sinyal Analog (Analog Signal)?", "id": "Sinyal Dengan Nilai Kontinu." },
  { "en": "Apa Itu Sinyal Digital (Digital Signal)?", "id": "Sinyal Dengan Nilai Diskrit." },
  { "en": "Apa Itu Pencuplikan (Sampling)?", "id": "Mengambil Nilai Sinyal Analog Interval Tertentu." },
  { "en": "Apa Itu Teorema Nyquist (Nyquist Theorem)?", "id": "Frekuensi Sampling Minimal 2x Frekuensi Maksimum." },
  { "en": "Apa Itu Aliasing (Aliasing)?", "id": "Distorsi Akibat Frekuensi Sampling Terlalu Rendah." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Pembulatan Nilai Sampel Ke Level Diskrit." },
  { "en": "Apa Itu Filter Digital (Digital Filter)?", "id": "Algoritma Memproses Sinyal Digital." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform)?", "id": "Mengubah Sinyal Domain Waktu Ke Frekuensi." },
  { "en": "Apa Itu FFT (Fast Fourier Transform)?", "id": "Algoritma Cepat Menghitung Transformasi Fourier Diskrit." },
  { "en": "Apa Itu Konvolusi (Convolution)?", "id": "Operasi Matematis Untuk Output Sistem LTI." },
  { "en": "Apa Itu Korelasi (Correlation)?", "id": "Ukuran Kemiripan Antar Sinyal." },
  { "en": "Apa Itu Sistem LTI (Linear Time-Invariant)?", "id": "Sistem Linear Dan Tidak Berubah Waktu." },
  { "en": "Apa Itu Respon Impuls (Impulse Response)?", "id": "Output Sistem LTI Terhadap Input Impuls." },
  { "en": "Apa Itu Persamaan Maxwell (Maxwell's Equations)?", "id": "Hukum Dasar Medan Elektromagnetik." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Perambatan Gangguan Medan Listrik Dan Magnet." },
  { "en": "Apa Itu Kecepatan Cahaya (Speed of Light) (c)?", "id": "Kecepatan Gelombang EM Di Vakum." },
  { "en": "Apa Itu Spektrum Elektromagnetik (Electromagnetic Spectrum)?", "id": "Rentang Frekuensi Gelombang EM." },
  { "en": "Apa Itu Propagasi (Propagation) Gelombang?", "id": "Perambatan Gelombang Melalui Medium." },
  { "en": "Apa Itu Atenuasi (Attenuation)?", "id": "Pelemahan Kekuatan Sinyal." },
  { "en": "Apa Itu Refleksi (Reflection) Gelombang?", "id": "Pemantulan Gelombang Pada Batas Medium." },
  { "en": "Apa Itu Refraksi (Refraction) Gelombang?", "id": "Pembelokan Gelombang Antar Medium." },
  { "en": "Apa Itu Difraksi (Diffraction) Gelombang?", "id": "Penyebaran Gelombang Melewati Celah/Ujung." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Konverter Antara Gelombang EM Dan Arus Listrik." },
  { "en": "Apa Itu Penguatan (Gain) Antena?", "id": "Ukuran Kemampuan Antena Memfokuskan Energi." },
  { "en": "Apa Itu Pola Radiasi (Radiation Pattern)?", "id": "Pola Pancaran Energi Antena." },
  { "en": "Apa Itu Impedansi Input (Input Impedance) Antena?", "id": "Impedansi Pada Terminal Antena." },
  { "en": "Apa Itu Polarisasi (Polarization) Antena?", "id": "Orientasi Medan Listrik Gelombang." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Media Pemandu Gelombang EM." },
  { "en": "Apa Itu Impedansi Karakteristik (Characteristic Impedance)?", "id": "Impedansi Khas Saluran Transmisi." },
  { "en": "Apa Itu Koefisien Refleksi (Reflection Coefficient)?", "id": "Rasio Amplitudo Gelombang Pantul Dan Datang." },
  { "en": "Apa Itu VSWR (Voltage Standing Wave Ratio)?", "id": "Ukuran Rasio Gelombang Berdiri." },
  { "en": "Apa Itu Pencocokan Impedansi (Impedance Matching)?", "id": "Menyesuaikan Impedansi Untuk Transfer Daya Maksimal." },
  { "en": "Apa Itu Smith Chart (Bagan Smith)?", "id": "Alat Grafis Analisis Saluran Transmisi." },
  { "en": "Apa Itu Parameter S (Scattering Parameters)?", "id": "Parameter Karakterisasi Jaringan Frekuensi Tinggi." },
  { "en": "Apa Itu Microwave (Gelombang Mikro)?", "id": "Gelombang EM Frekuensi 300 MHz - 300 GHz." },
  { "en": "Apa Itu Waveguide (Pemandu Gelombang)?", "id": "Struktur Logam Pemandu Gelombang Mikro." },
  { "en": "Apa Itu Komunikasi Serat Optik (Fiber Optic)?", "id": "Transmisi Informasi Menggunakan Cahaya Dalam Serat." },
  { "en": "Apa Prinsip Dasar Serat Optik?", "id": "Pemantulan Internal Total (Total Internal Reflection)." },
  { "en": "Apa Itu Komunikasi Data (Data Communication)?", "id": "Pertukaran Data Digital Antar Perangkat." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komputer." },
  { "en": "Apa Itu Protokol (Protocol) Jaringan?", "id": "Aturan Komunikasi Jaringan." },
  { "en": "Apa Itu Model OSI (Open Systems Interconnection)?", "id": "Model Konseptual Lapisan Jaringan (7 Lapisan)." },
  { "en": "Apa Itu Model TCP/IP (TCP/IP Model)?", "id": "Model Praktis Lapisan Jaringan Internet (4/5 Lapisan)." },
  { "en": "Apa Itu Alamat IP (IP Address)?", "id": "Pengenal Numerik Perangkat Di Jaringan IP." },
  { "en": "Apa Itu Alamat MAC (MAC Address)?", "id": "Pengenal Fisik Unik Antarmuka Jaringan." },
  { "en": "Apa Itu Ethernet (Ethernet)?", "id": "Standar Jaringan Area Lokal (LAN) Berkabel." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Standar Jaringan Area Lokal (LAN) Nirkabel." },
  { "en": "Apa Itu Router (Router)?", "id": "Perangkat Penghubung Antar Jaringan." },
  { "en": "Apa Itu Switch (Saklar)?", "id": "Perangkat Penghubung Perangkat Dalam Satu Jaringan LAN." },
  { "en": "Apa Itu Listrik (Electricity)?", "id": "Fenomena Terkait Kehadiran Dan Aliran Muatan." },
  { "en": "Apa Itu Muatan (Charge) Listrik?", "id": "Sifat Dasar Materi (Positif/Negatif)." },
  { "en": "Apa Satuan Muatan (Charge)?", "id": "Coulomb (C)." },
  { "en": "Apa Itu Elektron (Electron)?", "id": "Partikel Subatomik Bermuatan Negatif." },
  { "en": "Apa Itu Proton (Proton)?", "id": "Partikel Subatomik Bermuatan Positif." },
  { "en": "Apa Itu Arus (Current) Listrik?", "id": "Laju Aliran Muatan Listrik." },
  { "en": "Apa Satuan Arus (Current)?", "id": "Ampere (A)." },
  { "en": "Apa Itu Tegangan (Voltage)?", "id": "Perbedaan Potensial Listrik Antara Dua Titik." },
  { "en": "Apa Satuan Tegangan (Voltage)?", "id": "Volt (V)." },
  { "en": "Apa Itu Resistansi (Resistance)?", "id": "Hambatan Terhadap Aliran Arus." },
  { "en": "Apa Satuan Resistansi (Resistance)?", "id": "Ohm (Ω)." },
  { "en": "Apa Itu Hukum Ohm (Ohm's Law)?", "id": "Hubungan Antara Tegangan, Arus, Resistansi (V=IR)." },
  { "en": "Apa Itu Daya (Power) Listrik?", "id": "Laju Transfer Energi Listrik." },
  { "en": "Apa Satuan Daya (Power)?", "id": "Watt (W)." },
  { "en": "Apa Itu Rangkaian Seri (Series Circuit)?", "id": "Komponen Terhubung Dalam Satu Jalur." },
  { "en": "Apa Itu Rangkaian Paralel (Parallel Circuit)?", "id": "Komponen Terhubung Pada Titik Cabang Yang Sama." },
  { "en": "Apa Itu Hukum Arus Kirchhoff (KCL)?", "id": "Jumlah Arus Masuk Simpul Sama Dengan Keluar." },
  { "en": "Apa Itu Hukum Tegangan Kirchhoff (KVL)?", "id": "Jumlah Tegangan Dalam Loop Tertutup Adalah Nol." },
  { "en": "Apa Itu Arus Searah (Direct Current) (DC)?", "id": "Arus Mengalir Satu Arah." },
  { "en": "Apa Itu Arus Bolak-Balik (Alternating Current) (AC)?", "id": "Arus Berubah Arah Secara Periodik." },
  { "en": "Apa Itu Frekuensi (Frequency) AC?", "id": "Jumlah Siklus Per Detik (Hz)." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Komponen Penyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktor (Induktor)?", "id": "Komponen Penyimpan Energi Magnetik." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Hambatan Komponen Kapasitif Atau Induktif Terhadap AC." },
  { "en": "Apa Itu Impedansi (Impedance)?", "id": "Hambatan Total Rangkaian Terhadap AC (Termasuk Resistansi)." },
  { "en": "Apa Itu Semikonduktor (Semiconductor)?", "id": "Bahan Dengan Konduktivitas Antara Konduktor Dan Isolator." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Semikonduktor Mengalirkan Arus Satu Arah." },
  { "en": "Apa Itu Transistor (Transistor)?", "id": "Komponen Semikonduktor Penguat Atau Saklar." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit) (IC)?", "id": "Rangkaian Elektronik Kompleks Dalam Satu Chip." },
  { "en": "Apa Itu Elektronika Analog (Analog Electronics)?", "id": "Elektronika Berurusan Dengan Sinyal Kontinu." },
  { "en": "Apa Itu Elektronika Digital (Digital Electronics)?", "id": "Elektronika Berurusan Dengan Sinyal Diskrit." },
  { "en": "Apa Itu Penguat (Amplifier)?", "id": "Rangkaian Menaikkan Kekuatan Sinyal." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Menghasilkan Sinyal Periodik." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Rangkaian Memilih Frekuensi Tertentu." },
  { "en": "Apa Itu Catu Daya (Power Supply)?", "id": "Mengubah Sumber Listrik Menjadi Bentuk Yang Sesuai." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Mengubah Level Tegangan AC." },
  { "en": "Apa Itu Motor Listrik (Electric Motor)?", "id": "Mengubah Energi Listrik Menjadi Energi Mekanik." },
  { "en": "Apa Itu Generator Listrik (Electric Generator)?", "id": "Mengubah Energi Mekanik Menjadi Energi Listrik." },
  { "en": "Apa Itu Pengukuran (Measurement) Listrik?", "id": "Menentukan Nilai Besaran Listrik." },
  { "en": "Apa Itu Voltmeter (Voltmeter)?", "id": "Alat Ukur Tegangan." },
  { "en": "Apa Itu Amperemeter (Ammeter)?", "id": "Alat Ukur Arus." },
  { "en": "Apa Itu Ohmmeter (Ohmmeter)?", "id": "Alat Ukur Resistansi." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Kombinasi (V, A, Ω)." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Menampilkan Bentuk Gelombang Tegangan Terhadap Waktu." },
  { "en": "Apa Itu Sistem Tenaga Listrik (Power System)?", "id": "Infrastruktur Pembangkitan, Transmisi, Distribusi Listrik." },
  { "en": "Apa Itu Pembangkit Listrik (Power Plant)?", "id": "Fasilitas Menghasilkan Energi Listrik." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Menyalurkan Listrik Jarak Jauh Tegangan Tinggi." },
  { "en": "Apa Itu Jaringan Distribusi (Distribution Network)?", "id": "Menyalurkan Listrik Ke Pelanggan Tegangan Rendah." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Mengubah Level Tegangan." },
  { "en": "Apa Itu Sistem Proteksi (Protection System)?", "id": "Melindungi Sistem Tenaga Dari Gangguan." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Otomatis Memutus Arus Gangguan." },
  { "en": "Apa Itu Rele (Relay) Proteksi?", "id": "Mendeteksi Gangguan Dan Memberi Sinyal Trip." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Menghubungkan Sistem Ke Tanah Untuk Keamanan." },
  { "en": "Apa Itu Teknik Kontrol (Control Engineering)?", "id": "Merancang Sistem Untuk Mengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Sistem Loop Terbuka (Open Loop)?", "id": "Kontrol Tanpa Umpan Balik." },
  { "en": "Apa Itu Sistem Loop Tertutup (Closed Loop)?", "id": "Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Mengukur Besaran Fisik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Menghasilkan Aksi Fisik Berdasarkan Sinyal Kontrol." },
  { "en": "Apa Itu Kontroler (Controller)?", "id": "Menghitung Sinyal Kontrol Berdasarkan Error." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Digital Untuk Otomatisasi Industri." },
  { "en": "Apa Simbol Skematik Untuk Resistor?", "id": "Garis Zig-Zag Atau Persegi Panjang." },
  { "en": "Apa Simbol Skematik Untuk Kapasitor Non-Polar?", "id": "Dua Garis Paralel Sama Panjang." },
  { "en": "Apa Simbol Skematik Untuk Kapasitor Polar?", "id": "Satu Garis Lurus, Satu Melengkung (+)." },
  { "en": "Apa Simbol Skematik Untuk Induktor?", "id": "Garis Melingkar Seperti Kumparan." },
  { "en": "Apa Simbol Skematik Untuk Sumber Tegangan DC?", "id": "Garis Panjang (+), Pendek (-)." },
  { "en": "Apa Simbol Skematik Untuk Sumber Tegangan AC?", "id": "Lingkaran Dengan Gelombang Sinus Didalamnya." },
  { "en": "Apa Simbol Skematik Untuk Sumber Arus?", "id": "Lingkaran Dengan Panah Didalamnya." },
  { "en": "Apa Simbol Skematik Untuk Dioda?", "id": "Segitiga Menunjuk Garis Tegak Lurus." },
  { "en": "Apa Simbol Skematik Untuk LED?", "id": "Simbol Dioda Dengan Panah Keluar." },
  { "en": "Apa Simbol Skematik Untuk Transistor NPN?", "id": "Basis, Kolektor, Emitor (Panah Keluar)." },
  { "en": "Apa Simbol Skematik Untuk Transistor PNP?", "id": "Basis, Kolektor, Emitor (Panah Masuk)." },
  { "en": "Apa Simbol Skematik Untuk Saklar SPST?", "id": "Dua Titik Dengan Garis Membuka/Menutup." },
  { "en": "Apa Simbol Skematik Untuk Ground (Tanah)?", "id": "Tiga Garis Horizontal Makin Pendek." },
  { "en": "Apa Satuan Muatan Listrik (Charge)?", "id": "Coulomb (C)." },
  { "en": "Apa Satuan Potensial Listrik (Potential)?", "id": "Volt (V)." },
  { "en": "Apa Satuan Medan Listrik (Electric Field)?", "id": "Volt Per Meter (V/m)." },
  { "en": "Apa Satuan Kapasitansi (Capacitance)?", "id": "Farad (F)." },
  { "en": "Apa Satuan Induktansi (Inductance)?", "id": "Henry (H)." },
  { "en": "Apa Satuan Konduktansi (Conductance)?", "id": "Siemens (S)." },
  { "en": "Apa Satuan Fluks Magnet (Magnetic Flux)?", "id": "Weber (Wb)." },
  { "en": "Apa Satuan Kerapatan Fluks Magnet?", "id": "Tesla (T)." },
  { "en": "Apa Hukum Kekekalan Muatan (Charge Conservation)?", "id": "Muatan Total Sistem Terisolasi Tetap." },
  { "en": "Apa Hukum Kekekalan Energi (Energy Conservation)?", "id": "Energi Tidak Diciptakan Atau Dimusnahkan." },
  { "en": "Apa Itu Daya Sesaat (Instantaneous Power)?", "id": "Daya Pada Suatu Waktu Tertentu." },
  { "en": "Rumus Daya Sesaat (Instantaneous Power)?", "id": "p(t) = v(t) * i(t)." },
  { "en": "Apa Itu Nilai Rata-Rata (Average Value) Sinyal?", "id": "Komponen DC Sinyal Periodik." },
  { "en": "Apa Itu Nilai RMS (Root Mean Square)?", "id": "Nilai Efektif Sinyal AC." },
  { "en": "Mengapa Nilai RMS (Root Mean Square) Penting?", "id": "Berkaitan Dengan Pemanasan (Daya)." },
  { "en": "Apa Itu Faktor Bentuk (Form Factor)?", "id": "Rasio Nilai RMS Terhadap Rata-Rata." },
  { "en": "Apa Itu Faktor Puncak (Crest Factor)?", "id": "Rasio Nilai Puncak Terhadap RMS." },
  { "en": "Apa Itu Rangkaian Linear (Linear Circuit)?", "id": "Output Proporsional Terhadap Inputnya." },
  { "en": "Apa Itu Prinsip Superposisi (Superposition Principle)?", "id": "Respon Total Jumlah Respon Individual." },
  { "en": "Komponen Apa Yang Bersifat Linear?", "id": "Resistor, Kapasitor, Induktor Ideal." },
  { "en": "Komponen Apa Yang Bersifat Non-Linear?", "id": "Dioda, Transistor." },
  { "en": "Apa Itu Resistansi (Resistance) Ekuivalen?", "id": "Satu Resistor Pengganti Kombinasi." },
  { "en": "Apa Itu Kapasitansi (Capacitance) Ekuivalen?", "id": "Satu Kapasitor Pengganti Kombinasi." },
  { "en": "Apa Itu Induktansi (Inductance) Ekuivalen?", "id": "Satu Induktor Pengganti Kombinasi." },
  { "en": "Apa Itu Respon Transien (Transient Response)?", "id": "Respon Sementara Saat Rangkaian Berubah." },
  { "en": "Apa Penyebab Respon Transien (Transient Response)?", "id": "Adanya Penyimpan Energi (L, C)." },
  { "en": "Apa Itu Keadaan Tunak (Steady State)?", "id": "Kondisi Rangkaian Setelah Transien Hilang." },
  { "en": "Apa Itu Konstanta Waktu (Time Constant)?", "id": "Ukuran Kecepatan Respon Transien." },
  { "en": "Konstanta Waktu (Time Constant) Apa Untuk Rangkaian RC?", "id": "Tau = RC." },
  { "en": "Konstanta Waktu (Time Constant) Apa Untuk Rangkaian RL?", "id": "Tau = L/R." },
  { "en": "Berapa Lama Mencapai Keadaan Tunak (Teoretis)?", "id": "Lima Kali Konstanta Waktu (5τ)." },
  { "en": "Apa Itu Impedansi (Impedance) Kompleks?", "id": "Representasi Fasor Hambatan AC." },
  { "en": "Impedansi (Impedance) Resistor (R)?", "id": "Z = R (Riil)." },
  { "en": "Impedansi (Impedance) Induktor (L)?", "id": "Z = jωL (Imajiner Positif)." },
  { "en": "Impedansi (Impedance) Kapasitor (C)?", "id": "Z = 1/(jωC) = -j/(ωC)." },
  { "en": "Apa Itu Admitansi (Admittance) (Y)?", "id": "Kebalikan Impedansi (Y = 1/Z)." },
  { "en": "Apa Itu Konduktansi (Conductance) (G)?", "id": "Bagian Riil Admitansi." },
  { "en": "Apa Itu Suseptansi (Susceptance) (B)?", "id": "Bagian Imajiner Admitansi." },
  { "en": "Apa Itu Daya Kompleks (Complex Power) (S)?", "id": "S = P + jQ." },
  { "en": "Apa Itu Daya Nyata (Real Power) (P)?", "id": "Daya Yang Melakukan Kerja." },
  { "en": "Apa Itu Daya Reaktif (Reactive Power) (Q)?", "id": "Daya Untuk Medan Listrik/Magnet." },
  { "en": "Apa Itu Daya Semu (Apparent Power) (|S|)?", "id": "Magnitudo Daya Kompleks." },
  { "en": "Apa Itu Faktor Daya (Power Factor)?", "id": "Cos(θ) = P / |S|." },
  { "en": "Apa Itu Segitiga Daya (Power Triangle)?", "id": "Representasi Grafis P, Q, S." },
  { "en": "Beban Apa Memiliki Faktor Daya (Power Factor) Lagging?", "id": "Beban Induktif (Motor)." },
  { "en": "Beban Apa Memiliki Faktor Daya (Power Factor) Leading?", "id": "Beban Kapasitif." },
  { "en": "Mengapa Faktor Daya (Power Factor) Rendah Tidak Diinginkan?", "id": "Menyebabkan Arus Tinggi, Rugi Besar." },
  { "en": "Bagaimana Memperbaiki Faktor Daya (Power Factor) Lagging?", "id": "Pasang Kapasitor Paralel." },
  { "en": "Apa Itu Resonansi (Resonance)?", "id": "Kondisi Reaktansi Induktif = Kapasitif." },
  { "en": "Apa Frekuensi Resonansi (Resonance Frequency) RLC?", "id": "f₀ = 1 / (2π√(LC))." },
  { "en": "Apa Sifat RLC Seri Saat Resonansi?", "id": "Impedansi Minimum (Z = R)." },
  { "en": "Apa Sifat RLC Paralel Saat Resonansi?", "id": "Impedansi Maksimum (Z = R)." },
  { "en": "Apa Itu Faktor Kualitas (Q Factor)?", "id": "Rasio Energi Tersimpan Per Siklus." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Rangkaian Resonansi?", "id": "Rentang Frekuensi Sekitar Resonansi." },
  { "en": "Apa Itu Rangkaian Tertala (Tuned Circuit)?", "id": "Rangkaian Resonansi Pilih Frekuensi." },
  { "en": "Apa Itu Filter Pasif (Passive Filter)?", "id": "Filter Terbuat Dari R, L, C." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency) Filter?", "id": "Frekuensi Batas (-3 dB)." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Mengubah Tegangan AC Induksi Mutual." },
  { "en": "Apa Itu Rasio Belitan (Turns Ratio)?", "id": "Perbandingan Lilitan Primer, Sekunder." },
  { "en": "Apa Itu Trafo Step-Up (Penaik)?", "id": "Menaikkan Tegangan (Ns > Np)." },
  { "en": "Apa Itu Trafo Step-Down (Penurun)?", "id": "Menurunkan Tegangan (Ns < Np)." },
  { "en": "Apa Itu Efisiensi (Efficiency) Trafo?", "id": "Rasio Daya Output Terhadap Input." },
  { "en": "Apa Rugi Utama (Main Loss) Trafo?", "id": "Rugi Tembaga Dan Rugi Inti." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Semikonduktor Searah Arus." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Rangkaian Pengubah AC Ke DC." },
  { "en": "Apa Itu Filter Kapasitor (Capacitor Filter)?", "id": "Menghaluskan Output Penyearah." },
  { "en": "Apa Itu Regulator Tegangan (Voltage Regulator)?", "id": "Menstabilkan Tegangan Output DC." },
  { "en": "Apa Itu Transistor (Transistor)?", "id": "Komponen Aktif Penguat/Saklar." },
  { "en": "Apa Itu Penguat (Amplifier)?", "id": "Meningkatkan Amplitudo Sinyal." },
  { "en": "Apa Itu Penguatan (Gain)?", "id": "Faktor Penguatan Sinyal." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Mengembalikan Output Ke Input." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Penghasil Sinyal Periodik." },
  { "en": "Apa Itu Sirkuit Digital (Digital Circuit)?", "id": "Sirkuit Operasi Level Logika." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Elemen Dasar Sirkuit Digital." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Simpan Satu Bit." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Pembangkitan Hingga Distribusi Listrik." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Mengamankan Sistem Dari Gangguan." },
  { "en": "Apa Itu Pengukuran (Measurement) Listrik?", "id": "Menentukan Nilai Besaran Listrik." },
  { "en": "Apa Itu Keselamatan (Safety) Listrik?", "id": "Praktik Aman Bekerja Listrik." },
  { "en": "Apa Itu Ground Fault Circuit Interrupter (GFCI)?", "id": "Pelindung Sengatan Listrik Kebocoran Tanah." },
  { "en": "Apa Kepanjangan GFCI (Ground Fault Circuit Interrupter)?", "id": "Interuptor Sirkuit Gangguan Tanah." },
  { "en": "Apa Itu Penguat Operasional (Operational Amplifier) Ideal?", "id": "Gain Tak Hingga, Impedansi Input Tak Hingga." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Menstabilkan Penguatan, Mengurangi Distorsi." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Output Terbalik Fasa 180 Derajat." },
  { "en": "Apa Rumus Gain Penguat Inverting (Inverting Amplifier)?", "id": "Av = -Rf / Rin." },
  { "en": "Apa Itu Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Rumus Gain Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Av = 1 + (Rf / Rin)." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Penguat Non-Inverting Gain Satu." },
  { "en": "Apa Fungsi Utama Pengikut Tegangan (Voltage Follower)?", "id": "Sebagai Penyangga (Buffer) Impedansi." },
  { "en": "Apa Impedansi Input (Input Impedance) Pengikut Tegangan?", "id": "Sangat Tinggi (Ideal Tak Hingga)." },
  { "en": "Apa Impedansi Output (Output Impedance) Pengikut Tegangan?", "id": "Sangat Rendah (Ideal Nol)." },
  { "en": "Apa Itu Penguat Penjumlah (Summing Amplifier)?", "id": "Menjumlahkan Beberapa Sinyal Input." },
  { "en": "Apa Itu Penguat Diferensial (Differential Amplifier)?", "id": "Menguatkan Selisih Dua Sinyal Input." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Sinyal Mode Sama." },
  { "en": "Apa Itu Integrator (Integrator) Op-Amp?", "id": "Output Adalah Integral Sinyal Input." },
  { "en": "Apa Itu Diferensiator (Differentiator) Op-Amp?", "id": "Output Adalah Turunan Sinyal Input." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Membandingkan Dua Tegangan Input." },
  { "en": "Apa Output Komparator (Comparator)?", "id": "Tegangan Saturasi Positif Atau Negatif." },
  { "en": "Apa Itu Schmitt Trigger (Schmitt Trigger)?", "id": "Komparator Dengan Histeresis (Dua Threshold)." },
  { "en": "Apa Itu Filter Aktif (Active Filter)?", "id": "Filter Menggunakan Op-Amp (Bisa Gain)." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Sinyal Periodik." },
  { "en": "Apa Itu Multivibrator Astabil (Astable Multivibrator)?", "id": "Osilator Gelombang Kotak (Tanpa Stabil)." },
  { "en": "Apa Itu Multivibrator Monostabil (Monostable Multivibrator)?", "id": "Penghasil Pulsa Tunggal (Satu Stabil)." },
  { "en": "Apa Itu Timer (Timer) IC 555?", "id": "IC Serbaguna Timer Dan Osilator." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Pengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Resolusi (Resolution) ADC/DAC?", "id": "Jumlah Bit Representasi Digital." },
  { "en": "Apa Itu Laju Sampel (Sample Rate)?", "id": "Seberapa Cepat Sinyal Dicicipi (ADC)." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "Unit Pemroses Pusat (CPU) Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Chip Tunggal." },
  { "en": "Apa Saja Bagian Mikrokontroler (Microcontroller)?", "id": "CPU, Memori, I/O Peripheral." },
  { "en": "Apa Itu Memori Flash (Flash Memory)?", "id": "Memori Non-Volatil Penyimpan Program." },
  { "en": "Apa Itu Memori RAM (Random Access Memory)?", "id": "Memori Volatil Penyimpan Data Sementara." },
  { "en": "Apa Itu GPIO (General Purpose Input Output)?", "id": "Pin Digital Serbaguna Mikrokontroler." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Modulasi Lebar Pulsa." },
  { "en": "Apa Aplikasi PWM (Pulse Width Modulation)?", "id": "Kontrol Kecerahan LED, Kecepatan Motor." },
  { "en": "Apa Itu Komunikasi Serial (Serial Communication)?", "id": "Pengiriman Data Bit Per Bit." },
  { "en": "Apa Itu UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Antarmuka Komunikasi Serial Asinkron." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Antarmuka Komunikasi Serial Sinkron." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Bus Komunikasi Serial Dua Kawat." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Perangkat Mengubah Besaran Fisik Ke Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Perangkat Mengubah Sinyal Listrik Ke Aksi." },
  { "en": "Apa Itu Motor DC (Direct Current)?", "id": "Motor Penggerak Arus Searah." },
  { "en": "Apa Itu Motor Servo (Servo Motor)?", "id": "Motor Dengan Kontrol Posisi Sudut." },
  { "en": "Apa Itu Motor Stepper (Stepper Motor)?", "id": "Motor Bergerak Dalam Langkah Sudut." },
  { "en": "Apa Itu Driver (Driver) Motor?", "id": "Rangkaian Penguat Pengendali Motor." },
  { "en": "Apa Itu H-Bridge (Jembatan H)?", "id": "Rangkaian Kontrol Arah Putaran Motor DC." },
  { "en": "Apa Itu Relay (Relay)?", "id": "Saklar Elektromekanis Kontrol Beban Besar." },
  { "en": "Apa Itu Solenoida (Solenoid)?", "id": "Aktuator Elektromagnetik Gerak Linear." },
  { "en": "Apa Itu Magnet Permanen (Permanent Magnet)?", "id": "Bahan Menghasilkan Medan Magnet Sendiri." },
  { "en": "Apa Itu Elektromagnet (Electromagnet)?", "id": "Magnet Dibuat Aliran Arus Listrik." },
  { "en": "Apa Itu Induksi Elektromagnetik (Electromagnetic Induction)?", "id": "Pembangkitan GGL Medan Magnet Berubah." },
  { "en": "Apa Aplikasi Induksi Elektromagnetik (Electromagnetic Induction)?", "id": "Generator, Transformator." },
  { "en": "Apa Itu Fluks (Flux) Magnetik?", "id": "Ukuran Medan Magnet Menembus Luas." },
  { "en": "Apa Satuan Fluks (Flux) Magnetik?", "id": "Weber (Wb)." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Mengubah Tegangan AC Tanpa Kontak Fisik." },
  { "en": "Apa Itu Inti (Core) Transformator?", "id": "Media Jalur Fluks Magnetik." },
  { "en": "Mengapa Inti (Core) Trafo Berlaminasi?", "id": "Mengurangi Arus Eddy (Eddy Current)." },
  { "en": "Apa Itu Efisiensi (Efficiency) Transformator?", "id": "Rasio Daya Output Terhadap Input." },
  { "en": "Apa Itu Motor Listrik (Electric Motor)?", "id": "Konverter Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator Listrik (Electric Generator)?", "id": "Konverter Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Stator (Stator)?", "id": "Bagian Diam Mesin Listrik." },
  { "en": "Apa Itu Rotor (Rotor)?", "id": "Bagian Berputar Mesin Listrik." },
  { "en": "Apa Itu Motor Induksi (Induction Motor)?", "id": "Motor AC Asinkron Paling Umum." },
  { "en": "Apa Itu Slip (Slip) Motor Induksi?", "id": "Beda Kecepatan Rotor, Medan Putar." },
  { "en": "Apa Itu Motor Sinkron (Synchronous Motor)?", "id": "Motor AC Berputar Kecepatan Sinkron." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Pembangkitan Hingga Penggunaan Listrik." },
  { "en": "Apa Itu Transmisi (Transmission) Listrik?", "id": "Penyaluran Jarak Jauh Tegangan Tinggi." },
  { "en": "Apa Itu Distribusi (Distribution) Listrik?", "id": "Penyaluran Ke Konsumen Tegangan Rendah." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Pusat Transformasi Dan Switching." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Pengamanan Sistem Dari Gangguan." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Otomatis Pemutus Arus Gangguan." },
  { "en": "Apa Itu Rele Proteksi (Protection Relay)?", "id": "Pendeteksi Gangguan Pemicu Pemutus." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Tanah Untuk Keamanan." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Penggunaan Semikonduktor Kontrol Daya." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Konverter AC Ke DC." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Konverter DC Ke AC." },
  { "en": "Apa Itu Konverter DC-DC (DC-DC Converter)?", "id": "Pengubah Level Tegangan DC." },
  { "en": "Apa Itu Thyristor (Thyristor)/SCR?", "id": "Saklar Semikonduktor Daya Terkontrol." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Saklar Daya Kecepatan Tinggi Populer." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Kontrol Saklar Elektronika Daya." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Loop Tertutup (Closed Loop)?", "id": "Kontrol Menggunakan Umpan Balik." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Pengukur Besaran Fisik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Penggerak Berdasarkan Sinyal Kontrol." },
  { "en": "Apa Itu Kontroler PID (PID Controller)?", "id": "Kontroler Umpan Balik Industri Umum." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Kontrol Industri Tangguh." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Pemantauan, Kontrol Jarak Jauh." },
  { "en": "Apa Itu HMI (Human Machine Interface)?", "id": "Antarmuka Grafis Operator Mesin." },
  { "en": "Apa Itu Komunikasi Data (Data Communication)?", "id": "Pertukaran Informasi Digital." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komputer." },
  { "en": "Apa Itu Protokol (Protocol) Jaringan?", "id": "Aturan Standar Komunikasi." },
  { "en": "Apa Itu Ethernet (Ethernet)?", "id": "Standar Jaringan Kabel LAN." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Standar Jaringan Nirkabel LAN." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Global." },
  { "en": "Apa Itu IP Address (Alamat IP)?", "id": "Alamat Logis Perangkat Di Jaringan." },
  { "en": "Apa Itu Router (Router)?", "id": "Penghubung Antar Jaringan Berbeda." },
  { "en": "Apa Itu Switch (Saklar)?", "id": "Penghubung Perangkat Dalam Satu LAN." },
  { "en": "Apa Itu Kabel UTP (Unshielded Twisted Pair)?", "id": "Kabel Pasangan Terpilin Jaringan." },
  { "en": "Apa Itu Serat Optik (Fiber Optic)?", "id": "Media Transmisi Data Cahaya." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Kuantifikasi Besaran." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Pengukuran Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Keterulangan Hasil Pengukuran." },
  { "en": "Apa Itu Resolusi (Resolution)?", "id": "Perubahan Terkecil Terdeteksi Alat Ukur." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Dasar (V, A, Ω)." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Visualisasi Sinyal Listrik." },
  { "en": "Apa Nama Lain Untuk Filter Band-Stop?", "id": "Filter Penolak Pita (Band-Reject)." },
  { "en": "Apa Fungsi Utama Dioda Zener?", "id": "Menstabilkan Tegangan Output DC." },
  { "en": "Bagaimana Simbol Dioda Zener Berbeda?", "id": "Garis Katoda Berbentuk Z." },
  { "en": "Apa Itu Regulator Tegangan Linear?", "id": "Menstabilkan Tegangan Buang Kelebihan Panas." },
  { "en": "Contoh IC Regulator Tegangan Linear?", "id": "LM7805 (Output 5 Volt)." },
  { "en": "Apa Itu Heat Sink (Penyerap Panas)?", "id": "Komponen Membantu Pembuangan Panas." },
  { "en": "Mengapa Regulator Linear Perlu Heat Sink?", "id": "Karena Menghasilkan Panas Berlebih." },
  { "en": "Apa Itu Efisiensi Regulator Linear?", "id": "Relatif Rendah Dibandingkan Switching." },
  { "en": "Apa Itu Regulator Switching (SMPS)?", "id": "Regulator Efisiensi Tinggi (Saklar Cepat)." },
  { "en": "Apa Komponen Utama Regulator Switching?", "id": "Saklar (Transistor), Induktor, Dioda, Kapasitor." },
  { "en": "Apa Keuntungan Regulator Switching?", "id": "Efisiensi Sangat Tinggi." },
  { "en": "Apa Kerugian Regulator Switching?", "id": "Menghasilkan Noise (Derau) Elektromagnetik." },
  { "en": "Apa Itu Konverter Buck (Step-Down)?", "id": "Regulator Switching Menurunkan Tegangan DC." },
  { "en": "Apa Itu Konverter Boost (Step-Up)?", "id": "Regulator Switching Menaikkan Tegangan DC." },
  { "en": "Apa Itu Logika Positif (Positive Logic)?", "id": "Tegangan Tinggi Mewakili Logika 1." },
  { "en": "Apa Itu Logika Negatif (Negative Logic)?", "id": "Tegangan Tinggi Mewakili Logika 0." },
  { "en": "Apa Itu Tabel Kebenaran (Truth Table)?", "id": "Menunjukkan Output Gerbang Setiap Input." },
  { "en": "Berapa Input Gerbang NOT (Inverter)?", "id": "Satu Input." },
  { "en": "Output AND Bernilai 1 Jika?", "id": "Semua Input Bernilai 1." },
  { "en": "Output OR Bernilai 1 Jika?", "id": "Salah Satu Input Bernilai 1." },
  { "en": "Output NAND Bernilai 0 Jika?", "id": "Semua Input Bernilai 1." },
  { "en": "Output NOR Bernilai 1 Jika?", "id": "Semua Input Bernilai 0." },
  { "en": "Output XOR Bernilai 1 Jika?", "id": "Jumlah Input 1 Ganjil." },
  { "en": "Output XNOR Bernilai 1 Jika?", "id": "Jumlah Input 1 Genap." },
  { "en": "Gerbang Apa Disebut Gerbang Universal?", "id": "Gerbang NAND Dan NOR." },
  { "en": "Mengapa Disebut Gerbang Universal?", "id": "Bisa Membentuk Semua Gerbang Lain." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Matematika Untuk Analisis Logika Digital." },
  { "en": "Siapa Mengembangkan Aljabar Boolean (Boolean Algebra)?", "id": "George Boole." },
  { "en": "Apa Hukum Komutatif (Commutative Law) Boolean?", "id": "A + B = B + A." },
  { "en": "Apa Hukum Asosiatif (Associative Law) Boolean?", "id": "A + (B + C) = (A + B) + C." },
  { "en": "Apa Hukum Distributif (Distributive Law) Boolean?", "id": "A . (B + C) = AB + AC." },
  { "en": "Apa Hukum Identitas (Identity Law) OR?", "id": "A + 0 = A." },
  { "en": "Apa Hukum Identitas (Identity Law) AND?", "id": "A . 1 = A." },
  { "en": "Apa Hukum Dominasi (Dominance Law) OR?", "id": "A + 1 = 1." },
  { "en": "Apa Hukum Dominasi (Dominance Law) AND?", "id": "A . 0 = 0." },
  { "en": "Apa Hukum Idempoten (Idempotent Law) OR?", "id": "A + A = A." },
  { "en": "Apa Hukum Idempoten (Idempotent Law) AND?", "id": "A . A = A." },
  { "en": "Apa Hukum Negasi Ganda (Double Negation)?", "id": "(A')' = A." },
  { "en": "Apa Hukum Komplemen (Complement Law) OR?", "id": "A + A' = 1." },
  { "en": "Apa Hukum Komplemen (Complement Law) AND?", "id": "A . A' = 0." },
  { "en": "Apa Teorema De Morgan (De Morgan's Theorem) Pertama?", "id": "(A + B)' = A' . B'." },
  { "en": "Apa Teorema De Morgan (De Morgan's Theorem) Kedua?", "id": "(A . B)' = A' + B'." },
  { "en": "Apa Itu Peta Karnaugh (Karnaugh Map)?", "id": "Metode Grafis Penyederhanaan Fungsi Boolean." },
  { "en": "Apa Tujuan Penyederhanaan Fungsi Boolean?", "id": "Mengurangi Jumlah Gerbang Logika." },
  { "en": "Apa Itu Sirkuit Kombinasional (Combinational Circuit)?", "id": "Output Tergantung Kombinasi Input Saat Ini." },
  { "en": "Contoh Sirkuit Kombinasional (Combinational Circuit)?", "id": "Adder, Decoder, Multiplexer." },
  { "en": "Apa Itu Sirkuit Sekuensial (Sequential Circuit)?", "id": "Output Tergantung Input, Keadaan Internal." },
  { "en": "Contoh Sirkuit Sekuensial (Sequential Circuit)?", "id": "Flip-Flop, Counter, Register." },
  { "en": "Apa Elemen Dasar Sirkuit Sekuensial?", "id": "Elemen Memori (Flip-Flop/Latch)." },
  { "en": "Apa Itu Sinyal Clock (Clock Signal)?", "id": "Sinyal Sinkronisasi Rangkaian Sekuensial." },
  { "en": "Apa Itu Tepi Naik (Rising Edge)?", "id": "Transisi Clock Dari Low Ke High." },
  { "en": "Apa Itu Tepi Turun (Falling Edge)?", "id": "Transisi Clock Dari High Ke Low." },
  { "en": "Apa Itu Latch (Latch) SR?", "id": "Elemen Memori Set-Reset Dasar." },
  { "en": "Apa Itu Kondisi Terlarang Latch SR?", "id": "Saat Input S=1 Dan R=1 Bersamaan." },
  { "en": "Apa Itu Latch (Latch) D Bergerbang?", "id": "Menyimpan Data D Saat Enable Aktif." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Dipicu Tepi Clock." },
  { "en": "Apa Perbedaan Latch Dan Flip-Flop?", "id": "Latch (Level-Triggered), Flip-Flop (Edge-Triggered)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) D?", "id": "Menyimpan Nilai D Pada Tepi Clock." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) T?", "id": "Mengubah (Toggle) Output Tepi Clock Jika T=1." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) JK?", "id": "Flip-Flop Fleksibel (Set, Reset, Toggle)." },
  { "en": "Apa Itu Kondisi Toggle Flip-Flop JK?", "id": "Saat Input J=1 Dan K=1." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Menyimpan Data Biner." },
  { "en": "Apa Itu Register Geser (Shift Register)?", "id": "Register Menggeser Data Bit Per Bit." },
  { "en": "Aplikasi Register Geser (Shift Register)?", "id": "Konversi Serial-Paralel, Penundaan Data." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Sekuensial Mencacah Urutan Biner." },
  { "en": "Apa Itu Counter (Pencacah) Asinkron?", "id": "Output Flip-Flop Memicu Clock Berikutnya (Ripple)." },
  { "en": "Apa Nama Lain Counter (Pencacah) Asinkron?", "id": "Ripple Counter (Pencacah Riak)." },
  { "en": "Apa Itu Counter (Pencacah) Sinkron?", "id": "Semua Flip-Flop Digerakkan Clock Sama." },
  { "en": "Mana Lebih Cepat, Sinkron Atau Asinkron?", "id": "Counter Sinkron Lebih Cepat." },
  { "en": "Apa Itu Modulus Counter (Pencacah)?", "id": "Jumlah Keadaan (State) Berbeda Counter." },
  { "en": "Apa Itu Counter (Pencacah) Dekade (Decade Counter)?", "id": "Counter Dengan Sepuluh Keadaan (0-9)." },
  { "en": "Apa Itu Counter (Pencacah) Cincin (Ring Counter)?", "id": "Register Geser Output Terhubung Input." },
  { "en": "Apa Itu Counter (Pencacah) Johnson (Johnson Counter)?", "id": "Counter Cincin Dengan Output Terbalik." },
  { "en": "Apa Itu Memori (Memory) Komputer?", "id": "Perangkat Penyimpanan Data Digital." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Baca-Tulis Akses Acak (Volatil)." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Hanya Baca (Non-Volatil)." },
  { "en": "Apa Perbedaan RAM Dan ROM?", "id": "RAM Volatil, ROM Non-Volatil." },
  { "en": "Apa Itu Memori Flash (Flash Memory)?", "id": "Jenis Memori Non-Volatil (EEPROM Cepat)." },
  { "en": "Apa Itu Memori Cache (Cache Memory)?", "id": "Memori Kecil, Cepat Antara CPU, RAM." },
  { "en": "Apa Itu Memori Virtual (Virtual Memory)?", "id": "Teknik Gunakan Disk Perluas RAM." },
  { "en": "Apa Itu Bit (Bit)?", "id": "Unit Dasar Memori (0 Atau 1)." },
  { "en": "Apa Itu Byte (Byte)?", "id": "Delapan Bit." },
  { "en": "Apa Itu Word (Word) Memori?", "id": "Jumlah Bit Diproses CPU Sekaligus." },
  { "en": "Apa Itu Alamat (Address) Memori?", "id": "Lokasi Unik Penyimpanan Data Memori." },
  { "en": "Apa Itu Bus Alamat (Address Bus)?", "id": "Jalur Penentu Alamat Memori." },
  { "en": "Apa Itu Bus Data (Data Bus)?", "id": "Jalur Transfer Data Memori." },
  { "en": "Apa Itu Bus Kontrol (Control Bus)?", "id": "Jalur Sinyal Kontrol Memori (Read/Write)." },
  { "en": "Apa Itu Waktu Akses (Access Time) Memori?", "id": "Waktu Membaca Data Dari Memori." },
  { "en": "Apa Itu Siklus Waktu (Cycle Time) Memori?", "id": "Waktu Minimum Antar Akses Memori." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Memori?", "id": "Laju Transfer Data Maksimum Memori." },
  { "en": "Apa Itu Memori Statis (Static Memory) (SRAM)?", "id": "Gunakan Flip-Flop (Tidak Perlu Refresh)." },
  { "en": "Apa Itu Memori Dinamis (Dynamic Memory) (DRAM)?", "id": "Gunakan Kapasitor (Perlu Refresh)." },
  { "en": "Mana Lebih Cepat, SRAM Atau DRAM?", "id": "SRAM (Static Random Access Memory) Lebih Cepat." },
  { "en": "Mana Lebih Murah Per Bit, SRAM Atau DRAM?", "id": "DRAM (Dynamic Random Access Memory) Lebih Murah." },
  { "en": "Apa Itu Refresh (Refresh) DRAM?", "id": "Membaca Ulang Data Jaga Muatan Kapasitor." },
  { "en": "Apa Itu PROM (Programmable ROM)?", "id": "ROM (Read Only Memory) Bisa Diprogram Sekali." },
  { "en": "Apa Itu EPROM (Erasable PROM)?", "id": "PROM (Programmable ROM) Dihapus Sinar Ultraviolet." },
  { "en": "Apa Itu EEPROM (Electrically Erasable PROM)?", "id": "PROM (Programmable ROM) Dihapus Secara Elektrik." },
  { "en": "Apa Itu SSD (Solid State Drive)?", "id": "Penyimpanan Non-Volatil Berbasis Flash." },
  { "en": "Apa Keunggulan SSD (Solid State Drive) Dibanding HDD?", "id": "Lebih Cepat, Tahan Guncangan, Hening." },
  { "en": "Apa Itu Antarmuka Paralel (Parallel Interface)?", "id": "Transfer Data Multi Bit Bersamaan." },
  { "en": "Apa Itu Antarmuka Serial (Serial Interface)?", "id": "Transfer Data Satu Bit Bergantian." },
  { "en": "Apa Keuntungan Antarmuka Serial (Serial Interface)?", "id": "Kabel Lebih Sedikit, Jarak Jauh." },
  { "en": "Apa Itu Elektron Valensi (Valence Electron)?", "id": "Elektron Di Kulit Terluar Atom." },
  { "en": "Apa Peran Elektron Valensi (Valence Electron)?", "id": "Berperan Dalam Ikatan Kimia, Konduksi." },
  { "en": "Apa Itu Pita Energi (Energy Band)?", "id": "Rentang Energi Elektron Dalam Padatan." },
  { "en": "Apa Itu Pita Valensi (Valence Band)?", "id": "Pita Energi Terisi Elektron Valensi." },
  { "en": "Apa Itu Pita Konduksi (Conduction Band)?", "id": "Pita Energi Elektron Bebas Bergerak." },
  { "en": "Apa Itu Celah Energi (Energy Gap)?", "id": "Jarak Energi Antara Pita Valensi, Konduksi." },
  { "en": "Bagaimana Celah Energi (Energy Gap) Konduktor?", "id": "Sangat Kecil Atau Tumpang Tindih." },
  { "en": "Bagaimana Celah Energi (Energy Gap) Isolator?", "id": "Sangat Lebar (Besar)." },
  { "en": "Bagaimana Celah Energi (Energy Gap) Semikonduktor?", "id": "Lebih Kecil Dari Isolator." },
  { "en": "Apa Itu Semikonduktor Intrinsik (Intrinsic Semiconductor)?", "id": "Semikonduktor Murni Tanpa Doping." },
  { "en": "Apa Itu Semikonduktor Ekstrinsik (Extrinsic Semiconductor)?", "id": "Semikonduktor Dengan Penambahan Doping." },
  { "en": "Apa Itu Pembawa Muatan Mayoritas (Majority Carrier)?", "id": "Pembawa Muatan Paling Banyak (N/P)." },
  { "en": "Apa Pembawa Mayoritas (Majority Carrier) Tipe-N?", "id": "Elektron Bebas." },
  { "en": "Apa Pembawa Mayoritas (Majority Carrier) Tipe-P?", "id": "Lubang (Hole)." },
  { "en": "Apa Itu Pembawa Muatan Minoritas (Minority Carrier)?", "id": "Pembawa Muatan Paling Sedikit." },
  { "en": "Apa Itu Arus Drift (Drift Current)?", "id": "Aliran Muatan Akibat Medan Listrik." },
  { "en": "Apa Itu Arus Difusi (Diffusion Current)?", "id": "Aliran Muatan Akibat Perbedaan Konsentrasi." },
  { "en": "Apa Itu Dioda Sambungan P-N (P-N Junction)?", "id": "Komponen Dasar Penyearah." },
  { "en": "Apa Itu Potensial Kontak (Contact Potential)?", "id": "Tegangan Internal Sambungan P-N." },
  { "en": "Nama Lain Potensial Kontak (Contact Potential)?", "id": "Potensial Penghalang (Barrier Potential)." },
  { "en": "Apa Itu Karakteristik I-V (I-V Characteristic) Dioda?", "id": "Grafik Hubungan Arus, Tegangan Dioda." },
  { "en": "Apa Itu Model Dioda Ideal (Ideal Diode)?", "id": "Saklar Sempurna (On/Off)." },
  { "en": "Apa Itu Model Tegangan Konstan (Constant Voltage)?", "id": "Dioda On Jika V > Vf." },
  { "en": "Apa Itu Resistansi Dinamis (Dynamic Resistance) Dioda?", "id": "Resistansi Dioda Saat Bias Maju." },
  { "en": "Apa Itu Aplikasi Penyearah (Rectifier) Dioda?", "id": "Mengubah AC Menjadi DC Berdenyut." },
  { "en": "Apa Itu Aplikasi Clipper (Pemotong) Dioda?", "id": "Memotong Sebagian Sinyal AC." },
  { "en": "Apa Itu Aplikasi Clamper (Penjepit) Dioda?", "id": "Menggeser Level DC Sinyal AC." },
  { "en": "Apa Itu Aplikasi Pengganda Tegangan (Voltage Multiplier)?", "id": "Menghasilkan Tegangan DC Kelipatan AC." },
  { "en": "Apa Itu Dioda Zener (Zener Diode)?", "id": "Stabilisator Tegangan Bias Mundur." },
  { "en": "Apa Itu Tegangan Zener (Zener Voltage) (Vz)?", "id": "Tegangan Breakdown Stabil Dioda Zener." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Memancarkan Cahaya." },
  { "en": "Apa Proses Pancaran Cahaya LED?", "id": "Rekombinasi Elektron, Hole Hasilkan Foton." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Sensor Cahaya (Bias Mundur)." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Penguat Arus Terkontrol Arus Basis." },
  { "en": "Apa Itu Daerah Aktif (Active Region) BJT?", "id": "Daerah Operasi Penguatan Linear." },
  { "en": "Apa Itu Daerah Saturasi (Saturation Region) BJT?", "id": "Daerah Operasi Saklar Tertutup (On)." },
  { "en": "Apa Itu Daerah Cutoff (Cutoff Region) BJT?", "id": "Daerah Operasi Saklar Terbuka (Off)." },
  { "en": "Apa Itu Penguatan Beta (Beta Gain) (β/hfe)?", "id": "Rasio Arus Kolektor Terhadap Basis." },
  { "en": "Apa Itu Bias (Biasing) Transistor?", "id": "Menetapkan Titik Operasi DC (Q-Point)." },
  { "en": "Apa Itu Garis Beban DC (DC Load Line)?", "id": "Grafik Hubungan Ic, Vce Statis." },
  { "en": "Apa Itu Model Sinyal Kecil (Small Signal Model)?", "id": "Model Ekuivalen Analisis Sinyal AC." },
  { "en": "Apa Itu Penguat Common Emitter (Common Emitter) (CE)?", "id": "Penguat BJT (Bipolar Junction Transistor) Paling Umum." },
  { "en": "Apa Karakteristik Penguat CE (Common Emitter)?", "id": "Gain Tegangan, Arus Tinggi, Fasa Balik." },
  { "en": "Apa Itu Penguat Common Collector (Common Collector) (CC)?", "id": "Pengikut Emitor (Emitter Follower)." },
  { "en": "Apa Karakteristik Penguat CC (Common Collector)?", "id": "Gain Tegangan ≈1, Arus Tinggi (Buffer)." },
  { "en": "Apa Itu Penguat Common Base (Common Base) (CB)?", "id": "Penguat Frekuensi Tinggi, Impedansi Input Rendah." },
  { "en": "Apa Itu FET (Field Effect Transistor)?", "id": "Transistor Terkontrol Tegangan Medan Listrik." },
  { "en": "Apa Itu JFET (Junction FET)?", "id": "FET (Field Effect Transistor) Sambungan P-N Gerbang." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "FET (Field Effect Transistor) Gerbang Terisolasi Oksida." },
  { "en": "Apa Keuntungan Utama FET (Field Effect Transistor)?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Apa Itu Mode Deplesi (Depletion Mode) MOSFET?", "id": "Saluran Ada Awal (Normally ON)." },
  { "en": "Apa Itu Mode Peningkatan (Enhancement Mode) MOSFET?", "id": "Saluran Dibentuk Tegangan Gate (Normally OFF)." },
  { "en": "Apa Itu Tegangan Threshold (Threshold Voltage) (Vt) MOSFET?", "id": "Tegangan Gate Minimum Mulai Konduksi." },
  { "en": "Apa Itu Penguat Common Source (Common Source) (CS)?", "id": "Penguat FET (Field Effect Transistor) Paling Umum." },
  { "en": "Apa Karakteristik Penguat CS (Common Source)?", "id": "Gain Tegangan Tinggi, Fasa Balik." },
  { "en": "Apa Itu Penguat Common Drain (Common Drain) (CD)?", "id": "Pengikut Source (Source Follower)." },
  { "en": "Apa Karakteristik Penguat CD (Common Drain)?", "id": "Gain Tegangan ≈1 (Buffer)." },
  { "en": "Apa Itu Penguat Common Gate (Common Gate) (CG)?", "id": "Penguat Frekuensi Tinggi, Impedansi Input Rendah." },
  { "en": "Apa Itu CMOS (Complementary MOS)?", "id": "Logika Kombinasi NMOS Dan PMOS." },
  { "en": "Apa Keunggulan Logika CMOS (Complementary MOS)?", "id": "Daya Statis Sangat Rendah." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Karakteristik Op-Amp (Operational Amplifier) Ideal?", "id": "Gain Tak Hingga, Input Z Tak Hingga." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Menstabilkan Operasi Penguat Op-Amp." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Output Beda Fasa 180° Dari Input." },
  { "en": "Apa Itu Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Penguat Non-Inverting Gain = 1." },
  { "en": "Apa Fungsi Pengikut Tegangan (Voltage Follower)?", "id": "Penyangga Impedansi (Impedance Buffer)." },
  { "en": "Apa Itu Penguat Penjumlah (Summing Amplifier)?", "id": "Menjumlahkan Beberapa Sinyal Input." },
  { "en": "Apa Itu Penguat Diferensial (Differential Amplifier)?", "id": "Menguatkan Selisih Antara Dua Input." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Ukuran Penolakan Sinyal Mode Sama." },
  { "en": "Apa Itu Integrator (Integrator) Op-Amp?", "id": "Output Adalah Integral Input." },
  { "en": "Apa Itu Diferensiator (Differentiator) Op-Amp?", "id": "Output Adalah Turunan Input." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Membandingkan Dua Tegangan, Output Digital." },
  { "en": "Apa Itu Schmitt Trigger (Schmitt Trigger)?", "id": "Komparator Dengan Histeresis." },
  { "en": "Apa Fungsi Histeresis (Hysteresis)?", "id": "Mencegah Osilasi Output Dekat Threshold." },
  { "en": "Apa Itu Filter Aktif (Active Filter)?", "id": "Filter Gunakan Op-Amp (Bisa Gain)." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Penghasil Sinyal Periodik." },
  { "en": "Apa Syarat Osilasi Barkhausen (Barkhausen Criterion)?", "id": "Gain Loop = 1, Fasa Loop = 0°." },
  { "en": "Apa Itu Osilator Jembatan Wien (Wien Bridge)?", "id": "Penghasil Gelombang Sinus Relatif Murni." },
  { "en": "Apa Itu Osilator Kristal (Crystal Oscillator)?", "id": "Osilator Frekuensi Sangat Stabil." },
  { "en": "Apa Itu Timer (Timer) IC 555?", "id": "IC Serbaguna Timer Dan Osilator." },
  { "en": "Apa Mode Operasi IC 555?", "id": "Astabil, Monostabil, Bistabil." },
  { "en": "Apa Fungsi Mode Astabil (Astable Mode)?", "id": "Sebagai Osilator Gelombang Kotak." },
  { "en": "Apa Fungsi Mode Monostabil (Monostable Mode)?", "id": "Sebagai Timer Satu Pulsa." },
  { "en": "Apa Itu Regulasi Tegangan (Voltage Regulation)?", "id": "Menjaga Tegangan Output Konstan." },
  { "en": "Apa Itu Regulator Linear (Linear Regulator)?", "id": "Regulator Buang Kelebihan Energi Panas." },
  { "en": "Apa Itu Regulator Switching (Switching Regulator)?", "id": "Regulator Efisiensi Tinggi (Saklar Cepat)." },
  { "en": "Apa Itu Konverter Buck (Buck Converter)?", "id": "Regulator Switching Penurun Tegangan." },
  { "en": "Apa Itu Konverter Boost (Boost Converter)?", "id": "Regulator Switching Penaik Tegangan." },
  { "en": "Apa Itu Konverter Buck-Boost (Buck-Boost Converter)?", "id": "Regulator Switching Penaik/Penurun (Inverting)." },
  { "en": "Apa Itu Efisiensi (Efficiency) Konverter?", "id": "Rasio Daya Output Terhadap Input." },
  { "en": "Apa Itu Riak (Ripple) Tegangan Output?", "id": "Variasi AC Kecil Pada Output DC." },
  { "en": "Apa Itu Elektronika Digital (Digital Electronics)?", "id": "Elektronika Sinyal Diskrit (0, 1)." },
  { "en": "Apa Itu Sistem Bilangan Biner (Binary System)?", "id": "Basis Dua." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Unit Dasar Operasi Logika Digital." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Matematika Operasi Logika." },
  { "en": "Apa Itu Sirkuit Kombinasional (Combinational Circuit)?", "id": "Output Tergantung Input Saat Ini." },
  { "en": "Apa Itu Sirkuit Sekuensial (Sequential Circuit)?", "id": "Output Tergantung Input, Keadaan Lalu." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Sekuensial Dasar." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Penyimpan Data." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Pencacah Urutan Pulsa." },
  { "en": "Apa Itu Memori (Memory)?", "id": "Perangkat Penyimpan Informasi Digital." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Akses Acak (Volatil)." },
  { "en": "Apa Sifat RAM (Random Access Memory)?", "id": "Volatil (Data Hilang Tanpa Daya)." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Hanya Baca (Non-Volatil)." },
  { "en": "Apa Sifat ROM (Read Only Memory)?", "id": "Non-Volatil (Data Tetap Tersimpan)." },
  { "en": "Apa Itu Multiplexer (Multiplexer) (MUX)?", "id": "Pemilih Data (Banyak Ke Satu)." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) (DEMUX)?", "id": "Distributor Data (Satu Ke Banyak)." },
  { "en": "Apa Itu Encoder (Encoder)?", "id": "Pengubah Input Aktif Ke Kode Biner." },
  { "en": "Apa Itu Decoder (Decoder)?", "id": "Pengubah Kode Biner Ke Output Aktif." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Pengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Proses Utama ADC (Analog-to-Digital Converter)?", "id": "Sampling Dan Kuantisasi." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Resolusi (Resolution) ADC/DAC?", "id": "Jumlah Bit Representasi Digital/Analog." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "Unit Pemroses Pusat (CPU) Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Sistem Komputer Chip Tunggal." },
  { "en": "Apa Bagian Utama Mikrokontroler?", "id": "CPU, Memori, I/O Peripheral." },
  { "en": "Apa Itu Bus (Bus) Sistem?", "id": "Jalur Komunikasi Internal Komputer." },
  { "en": "Jenis Bus (Bus) Sistem Utama?", "id": "Alamat, Data, Kontrol." },
  { "en": "Apa Itu Komunikasi Serial (Serial Communication)?", "id": "Transfer Data Bit Per Bit." },
  { "en": "Apa Itu Komunikasi Paralel (Parallel Communication)?", "id": "Transfer Data Multi Bit Bersamaan." },
  { "en": "Apa Itu UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Antarmuka Serial Asinkron." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Antarmuka Serial Sinkron." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Bus Serial Dua Kawat." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Pengubah Besaran Fisik Ke Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Pengubah Sinyal Listrik Ke Aksi Fisik." },
  { "en": "Apa Itu Motor DC (Direct Current)?", "id": "Motor Penggerak Arus Searah." },
  { "en": "Apa Itu Motor Servo (Servo Motor)?", "id": "Motor Kontrol Posisi Sudut." },
  { "en": "Apa Itu Motor Stepper (Stepper Motor)?", "id": "Motor Bergerak Dalam Langkah Sudut." },
  { "en": "Apa Itu Medan Listrik (Electric Field)?", "id": "Daerah Pengaruh Gaya Listrik." },
  { "en": "Apa Itu Medan Magnet (Magnetic Field)?", "id": "Daerah Pengaruh Gaya Magnet." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Perambatan Gangguan Medan Listrik, Magnet." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Konverter Gelombang EM Ke Sinyal Listrik." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Menumpangkan Informasi Ke Gelombang Pembawa." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Memisahkan Informasi Dari Gelombang Pembawa." },
  { "en": "Apa Itu Amplifier (Penguat)?", "id": "Rangkaian Meningkatkan Kekuatan Sinyal." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Menghasilkan Sinyal Periodik." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Rangkaian Memilih Frekuensi Tertentu." },
  { "en": "Apa Itu Catu Daya (Power Supply)?", "id": "Sumber Energi Listrik Rangkaian." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Mengubah Level Tegangan AC." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Mengubah Arus AC Menjadi DC." },
  { "en": "Apa Itu Regulator (Regulator) Tegangan?", "id": "Menjaga Tegangan Output Tetap Stabil." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Infrastruktur Listrik Dari Pembangkit." },
  { "en": "Apa Itu Transmisi (Transmission)?", "id": "Penyaluran Listrik Jarak Jauh." },
  { "en": "Apa Itu Distribusi (Distribution)?", "id": "Penyaluran Listrik Ke Pengguna Akhir." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Pengamanan Sistem Dari Gangguan." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Pengaman Otomatis Arus Lebih." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Tanah Untuk Keamanan." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Kontrol Aliran Daya Listrik." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Pengubah Daya DC Ke AC." },
  { "en": "Apa Itu Konverter (Converter) DC-DC?", "id": "Pengubah Level Tegangan DC." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Menggunakan Output Mengatur Input." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Otomatisasi Industri." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Pengawasan Dan Akuisisi Data." },
  { "en": "Apa Itu HMI (Human Machine Interface)?", "id": "Antarmuka Grafis Manusia-Mesin." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Kuantifikasi Besaran." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Ukuran Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Keterulangan Hasil Pengukuran." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Serbaguna." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Visualisasi Bentuk Gelombang." },
  { "en": "Apa Itu Resistansi Seri Ekuivalen (ESR)?", "id": "Equivalent Series Resistance Kapasitor." },
  { "en": "Apa Kepanjangan ESR (Equivalent Series Resistance)?", "id": "Resistansi Seri Ekuivalen." },
  { "en": "Apa Itu Induktansi Seri Ekuivalen (ESL)?", "id": "Equivalent Series Inductance Kapasitor." },
  { "en": "Apa Kepanjangan ESL (Equivalent Series Inductance)?", "id": "Induktansi Seri Ekuivalen." },
  { "en": "Apa Itu Impedansi (Impedance) Speaker?", "id": "Hambatan Speaker Terhadap Arus AC (Ohm)." },
  { "en": "Apa Itu Crossover (Crossover) Audio?", "id": "Filter Frekuensi Pembagi Sinyal Speaker." },
  { "en": "Apa Fungsi Crossover (Crossover) Audio?", "id": "Kirim Frekuensi Tepat Ke Driver Speaker." },
  { "en": "Apa Itu Tweeter (Tweeter) Speaker?", "id": "Driver Speaker Frekuensi Tinggi." },
  { "en": "Apa Itu Midrange (Midrange) Speaker?", "id": "Driver Speaker Frekuensi Menengah." },
  { "en": "Apa Itu Woofer (Woofer) Speaker?", "id": "Driver Speaker Frekuensi Rendah (Bass)." },
  { "en": "Apa Itu Subwoofer (Subwoofer)?", "id": "Speaker Khusus Frekuensi Sangat Rendah." },
  { "en": "Apa Itu Respons Frekuensi (Frequency Response) Speaker?", "id": "Rentang Frekuensi Dapat Direproduksi Speaker." },
  { "en": "Apa Itu Sensitivitas (Sensitivity) Speaker?", "id": "Tingkat Tekanan Suara (SPL) Daya 1W." },
  { "en": "Apa Satuan Sensitivitas (Sensitivity) Speaker?", "id": "Desibel (dB) SPL/Watt/Meter." },
  { "en": "Apa Itu Daya Penanganan (Power Handling) Speaker?", "id": "Daya Listrik Maksimum Dapat Diterima." },
  { "en": "Apa Itu Distorsi Harmonik Total (THD) Audio?", "id": "Total Harmonic Distortion (THD)." },
  { "en": "Apa Kepanjangan THD (Total Harmonic Distortion)?", "id": "Total Distorsi Harmonik." },
  { "en": "Apa Itu Rasio Sinyal-Derau (SNR) Audio?", "id": "Ukuran Kebersihan Sinyal Audio." },
  { "en": "Apa Itu Mikrofon Dinamis (Dynamic Microphone)?", "id": "Mikrofon Berbasis Induksi Elektromagnetik." },
  { "en": "Apa Itu Mikrofon Kondensor (Condenser Microphone)?", "id": "Mikrofon Berbasis Kapasitor." },
  { "en": "Mikrofon Mana Butuh Daya Phantom?", "id": "Mikrofon Kondensor (Condenser Microphone)." },
  { "en": "Apa Itu Pola Kutub (Polar Pattern) Mikrofon?", "id": "Arah Sensitivitas Pengambilan Suara." },
  { "en": "Contoh Pola Kutub (Polar Pattern) Mikrofon?", "id": "Cardioid, Omnidirectional, Bidirectional." },
  { "en": "Apa Itu Kabel Audio Seimbang (Balanced Cable)?", "id": "Tiga Konduktor (Positif, Negatif, Ground)." },
  { "en": "Apa Keuntungan Kabel Audio Seimbang (Balanced Cable)?", "id": "Menolak Noise Mode Bersama." },
  { "en": "Contoh Konektor Audio Seimbang (Balanced Connector)?", "id": "Konektor XLR (XLR Connector)." },
  { "en": "Apa Itu Kabel Audio Tidak Seimbang (Unbalanced Cable)?", "id": "Dua Konduktor (Sinyal, Ground)." },
  { "en": "Contoh Konektor Audio Tidak Seimbang (Unbalanced Connector)?", "id": "Konektor TS (Tip-Sleeve), RCA." },
  { "en": "Apa Itu Ground Loop (Loop Tanah) Audio?", "id": "Menyebabkan Dengung (Hum) Frekuensi Rendah." },
  { "en": "Bagaimana Mengatasi Ground Loop (Loop Tanah) Audio?", "id": "Gunakan Ground Lift Atau Isolator." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp) Audio?", "id": "Op-Amp (Operational Amplifier) Dirancang Noise Rendah." },
  { "en": "Apa Itu Equalizer (Equalizer) Audio?", "id": "Mengatur Tingkat Frekuensi Tertentu Audio." },
  { "en": "Apa Itu Kompresor (Compressor) Audio?", "id": "Mengurangi Rentang Dinamis Sinyal Audio." },
  { "en": "Apa Itu Limiter (Limiter) Audio?", "id": "Membatasi Amplitudo Puncak Sinyal Audio." },
  { "en": "Apa Itu Noise Gate (Gerbang Derau) Audio?", "id": "Memotong Sinyal Dibawah Level Threshold." },
  { "en": "Apa Itu Efek Reverb (Reverb Effect)?", "id": "Simulasi Gema Akustik Ruangan." },
  { "en": "Apa Itu Efek Delay (Delay Effect)?", "id": "Mengulang Sinyal Audio Setelah Penundaan." },
  { "en": "Apa Itu Elektronika (Electronics)?", "id": "Studi Aliran Kontrol Elektron." },
  { "en": "Apa Itu Sirkuit (Circuit) Listrik?", "id": "Jalur Tertutup Aliran Arus Listrik." },
  { "en": "Apa Komponen Pasif (Passive Component) Utama?", "id": "Resistor, Kapasitor, Dan Induktor." },
  { "en": "Apa Komponen Aktif (Active Component) Utama?", "id": "Dioda Dan Transistor." },
  { "en": "Apa Satuan Muatan (Charge) Listrik?", "id": "Coulomb (C)." },
  { "en": "Apa Partikel Pembawa Muatan Negatif?", "id": "Elektron." },
  { "en": "Apa Satuan Arus (Current) Listrik?", "id": "Ampere (A)." },
  { "en": "Apa Satuan Tegangan (Voltage) Listrik?", "id": "Volt (V)." },
  { "en": "Apa Satuan Resistansi (Resistance) Listrik?", "id": "Ohm (Ω)." },
  { "en": "Apa Bunyi Hukum Ohm (Ohm's Law)?", "id": "V Sama Dengan I Kali R." },
  { "en": "Apa Satuan Daya (Power) Listrik?", "id": "Watt (W)." },
  { "en": "Apa Rumus Daya (Power) Listrik?", "id": "P Sama Dengan V Kali I." },
  { "en": "Apa Itu Arus Searah (DC)?", "id": "Direct Current (Aliran Satu Arah)." },
  { "en": "Apa Sumber Umum Arus DC (Direct Current)?", "id": "Baterai, Catu Daya DC." },
  { "en": "Apa Itu Arus Bolak-Balik (AC)?", "id": "Alternating Current (Arah Berubah)." },
  { "en": "Apa Sumber Umum Arus AC (Alternating Current)?", "id": "Jaringan Listrik (PLN), Generator." },
  { "en": "Apa Bentuk Gelombang AC (Alternating Current) Umum?", "id": "Gelombang Sinusoidal." },
  { "en": "Apa Itu Frekuensi (Frequency) Sinyal AC?", "id": "Jumlah Siklus Per Detik (Hz)." },
  { "en": "Apa Frekuensi (Frequency) Listrik Standar Indonesia?", "id": "Lima Puluh Hertz (50 Hz)." },
  { "en": "Apa Itu Periode (Period) Sinyal AC?", "id": "Waktu Untuk Satu Siklus Lengkap." },
  { "en": "Apa Hubungan Periode (T) Frekuensi (f)?", "id": "T Sama Dengan 1 / f." },
  { "en": "Apa Itu Amplitudo (Amplitude) Sinyal AC?", "id": "Nilai Puncak Maksimum Gelombang." },
  { "en": "Apa Itu Nilai RMS (Root Mean Square)?", "id": "Nilai Efektif Sinyal AC." },
  { "en": "Apa Fungsi Resistor (Resistor)?", "id": "Menghambat Aliran Arus Listrik." },
  { "en": "Apa Itu Kode Warna Resistor?", "id": "Menentukan Nilai Resistansi, Toleransi." },
  { "en": "Apa Itu Toleransi (Tolerance) Resistor?", "id": "Penyimpangan Nilai Dari Nominal." },
  { "en": "Apa Fungsi Kapasitor (Capacitor)?", "id": "Menyimpan Energi Medan Listrik." },
  { "en": "Satuan Apa Kapasitor (Capacitor)?", "id": "Farad (F)." },
  { "en": "Apa Itu Dielektrik (Dielectric) Kapasitor?", "id": "Bahan Isolator Antar Pelat." },
  { "en": "Apa Fungsi Induktor (Inductor)?", "id": "Menyimpan Energi Medan Magnet." },
  { "en": "Satuan Apa Induktor (Inductor)?", "id": "Henry (H)." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Hambatan AC Kapasitor/Induktor." },
  { "en": "Apa Itu Reaktansi Kapasitif (Capacitive Reactance) (Xc)?", "id": "Hambatan Kapasitor Terhadap AC." },
  { "en": "Apa Itu Reaktansi Induktif (Inductive Reactance) (Xl)?", "id": "Hambatan Induktor Terhadap AC." },
  { "en": "Apa Itu Impedansi (Impedance) (Z)?", "id": "Hambatan Total Rangkaian AC." },
  { "en": "Apa Satuan Impedansi (Impedance)?", "id": "Ohm (Ω)." },
  { "en": "Apa Itu Rangkaian Seri (Series Circuit)?", "id": "Komponen Terhubung Satu Jalur." },
  { "en": "Bagaimana Arus (Current) Rangkaian Seri?", "id": "Sama Di Setiap Komponen." },
  { "en": "Bagaimana Tegangan (Voltage) Rangkaian Seri?", "id": "Terbagi Antar Komponen." },
  { "en": "Apa Itu Rangkaian Paralel (Parallel Circuit)?", "id": "Komponen Terhubung Titik Sama." },
  { "en": "Bagaimana Tegangan (Voltage) Rangkaian Paralel?", "id": "Sama Di Setiap Komponen." },
  { "en": "Bagaimana Arus (Current) Rangkaian Paralel?", "id": "Terbagi Antar Cabang." },
  { "en": "Apa Itu Hukum Arus Kirchhoff (KCL)?", "id": "Jumlah Arus Masuk Simpul = Keluar." },
  { "en": "Apa Itu Hukum Tegangan Kirchhoff (KVL)?", "id": "Jumlah Tegangan Loop Tertutup = Nol." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Mengalirkan Arus Satu Arah." },
  { "en": "Apa Itu Anoda (Anode) Dioda?", "id": "Terminal Positif Dioda." },
  { "en": "Apa Itu Katoda (Cathode) Dioda?", "id": "Terminal Negatif Dioda." },
  { "en": "Apa Itu Bias Maju (Forward Bias)?", "id": "Memungkinkan Arus Lewat Dioda." },
  { "en": "Apa Itu Bias Mundur (Reverse Bias)?", "id": "Menghambat Arus Lewat Dioda." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Rangkaian Mengubah AC Ke DC." },
  { "en": "Apa Fungsi Dioda Zener (Zener Diode)?", "id": "Menstabilkan Tegangan Mundur Tertentu." },
  { "en": "Apa Fungsi LED (Light Emitting Diode)?", "id": "Memancarkan Cahaya Saat Dialiri Arus." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Saklar/Penguat Terkontrol Arus Basis." },
  { "en": "Apa Tiga Kaki BJT (Bipolar Junction Transistor)?", "id": "Basis, Kolektor, Emitor." },
  { "en": "Apa Fungsi Transistor BJT (Bipolar Junction Transistor)?", "id": "Sebagai Penguat Atau Saklar." },
  { "en": "Apa Itu Transistor FET (Field Effect Transistor)?", "id": "Saklar/Penguat Terkontrol Tegangan Gate." },
  { "en": "Apa Tiga Kaki FET (Field Effect Transistor)?", "id": "Gate, Drain, Source." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Jenis FET Paling Umum Digunakan." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Menstabilkan Gain Op-Amp." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Rangkaian Melewatkan Frekuensi Tertentu." },
  { "en": "Apa Itu Filter Lolos Rendah (Low Pass)?", "id": "Melewatkan Frekuensi Rendah." },
  { "en": "Apa Itu Filter Lolos Tinggi (High Pass)?", "id": "Melewatkan Frekuensi Tinggi." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Menghasilkan Gelombang Periodik." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Serbaguna." },
  { "en": "Apa Yang Diukur Multimeter (Multimeter)?", "id": "Tegangan, Arus, Resistansi." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Menampilkan Bentuk Gelombang." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Dasar Sirkuit Digital." },
  { "en": "Apa Itu Logika Biner (Binary Logic)?", "id": "Logika Dua Keadaan (0 Atau 1)." },
  { "en": "Apa Fungsi Gerbang AND?", "id": "Output 1 Jika Semua Input 1." },
  { "en": "Apa Fungsi Gerbang OR?", "id": "Output 1 Jika Ada Input 1." },
  { "en": "Apa Fungsi Gerbang NOT?", "id": "Membalikkan Input Logika (Inverter)." },
  { "en": "Apa Fungsi Gerbang NAND?", "id": "Gerbang AND Diikuti Inverter." },
  { "en": "Apa Fungsi Gerbang NOR?", "id": "Gerbang OR Diikuti Inverter." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Pembangkitan Hingga Distribusi Listrik." },
  { "en": "Apa Itu Generator (Generator)?", "id": "Mengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Motor (Motor)?", "id": "Mengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Mengubah Level Tegangan AC." },
  { "en": "Apa Itu Transmisi (Transmission) Listrik?", "id": "Penyaluran Jarak Jauh Tegangan Tinggi." },
  { "en": "Apa Itu Distribusi (Distribution) Listrik?", "id": "Penyaluran Ke Konsumen Tegangan Rendah." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Tanah Untuk Keamanan." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Pelindung Arus Lebih Sekali Pakai." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Pelindung Arus Lebih Bisa Direset." },
  { "en": "Apa Itu Isolator (Insulator)?", "id": "Bahan Penghambat Aliran Listrik." },
  { "en": "Apa Itu Konduktor (Conductor)?", "id": "Bahan Penghantar Aliran Listrik." },
  { "en": "Apa Itu Semikonduktor (Semiconductor)?", "id": "Bahan Antara Konduktor, Isolator." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Kontrol Aliran Daya Listrik." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Mengubah AC Menjadi DC." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Mengubah DC Menjadi AC." },
  { "en": "Apa Itu Konverter DC-DC (DC-DC Converter)?", "id": "Mengubah Level Tegangan DC." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Modulasi Lebar Pulsa." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Loop Tertutup (Closed Loop)?", "id": "Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Mengukur Besaran Fisik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Menghasilkan Aksi Fisik." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Logika Terprogram Industri." },
  { "en": "Apa Itu Komunikasi Data (Data Communication)?", "id": "Transfer Informasi Digital." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komputer." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Global Komputer." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Penentuan Nilai Besaran." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Ukuran Nilai Benar." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Konsistensi Hasil Pengukuran." },
  { "en": "Apa Itu Medan Elektromagnetik (Electromagnetic Field)?", "id": "Medan Listrik Dan Magnet." },
  { "en": "Apa Itu Gelombang Radio (Radio Wave)?", "id": "Gelombang EM Komunikasi Nirkabel." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Menumpangkan Informasi Ke Pembawa." },
  { "en": "Apa Itu Osilasi (Oscillation)?", "id": "Gerak Bolak-Balik Sekitar Titik Setimbang." },
  { "en": "Apa Itu Frekuensi (Frequency) Osilasi?", "id": "Jumlah Getaran Per Detik (Hz)." },
  { "en": "Apa Itu Periode (Period) Osilasi?", "id": "Waktu Satu Getaran Lengkap (Detik)." },
  { "en": "Apa Itu Amplitudo (Amplitude) Osilasi?", "id": "Simpangan Maksimum Dari Titik Setimbang." },
  { "en": "Apa Itu Gerak Harmonik Sederhana (GHS)?", "id": "Osilasi Gaya Pemulih Proporsional Simpangan." },
  { "en": "Contoh Gerak Harmonik Sederhana (GHS)?", "id": "Gerak Pegas Ideal, Bandul Matematis." },
  { "en": "Apa Itu Redaman (Damping) Osilasi?", "id": "Berkurangnya Amplitudo Osilasi Seiring Waktu." },
  { "en": "Apa Itu Osilasi Teredam (Damped Oscillation)?", "id": "Osilasi Amplitudonya Mengecil." },
  { "en": "Apa Itu Osilasi Paksa (Forced Oscillation)?", "id": "Osilasi Akibat Gaya Eksternal Periodik." },
  { "en": "Apa Itu Resonansi (Resonance)?", "id": "Amplitudo Maksimal Frekuensi Paksa = Alami." },
  { "en": "Apa Itu Gelombang (Wave)?", "id": "Getaran Merambat Membawa Energi." },
  { "en": "Apa Itu Gelombang Mekanik (Mechanical Wave)?", "id": "Gelombang Butuh Medium Perambatan." },
  { "en": "Contoh Gelombang Mekanik (Mechanical Wave)?", "id": "Gelombang Suara, Gelombang Tali." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Gelombang Tidak Butuh Medium." },
  { "en": "Contoh Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Cahaya, Gelombang Radio." },
  { "en": "Apa Itu Gelombang Transversal (Transverse Wave)?", "id": "Getaran Tegak Lurus Arah Rambat." },
  { "en": "Apa Itu Gelombang Longitudinal (Longitudinal Wave)?", "id": "Getaran Sejajar Arah Rambat." },
  { "en": "Gelombang Apa Yang Transversal?", "id": "Gelombang Cahaya, Gelombang Tali." },
  { "en": "Gelombang Apa Yang Longitudinal?", "id": "Gelombang Suara." },
  { "en": "Apa Itu Panjang Gelombang (Wavelength) (λ)?", "id": "Jarak Satu Siklus Penuh Gelombang." },
  { "en": "Apa Itu Kecepatan Rambat (Wave Speed) (v)?", "id": "Kecepatan Perambatan Gelombang." },
  { "en": "Hubungan v (Kecepatan), f (Frekuensi), λ (Panjang Gelombang)?", "id": "v Sama Dengan f Kali λ." },
  { "en": "Apa Itu Superposisi (Superposition) Gelombang?", "id": "Penjumlahan Simpangan Gelombang Bertemu." },
  { "en": "Apa Itu Interferensi (Interference) Gelombang?", "id": "Hasil Superposisi Dua Gelombang Koheren." },
  { "en": "Apa Itu Interferensi Konstruktif (Constructive Interference)?", "id": "Gelombang Saling Menguatkan (Amplitudo Bertambah)." },
  { "en": "Apa Itu Interferensi Destruktif (Destructive Interference)?", "id": "Gelombang Saling Melemahkan (Amplitudo Berkurang)." },
  { "en": "Apa Itu Gelombang Berdiri (Standing Wave)?", "id": "Hasil Interferensi Gelombang Datang, Pantul." },
  { "en": "Apa Itu Simpul (Node) Gelombang Berdiri?", "id": "Titik Amplitudo Nol Tetap." },
  { "en": "Apa Itu Perut (Antinode) Gelombang Berdiri?", "id": "Titik Amplitudo Maksimum Getaran." },
  { "en": "Apa Itu Difraksi (Diffraction)?", "id": "Pelenturan Gelombang Melewati Celah Sempit." },
  { "en": "Apa Itu Efek Doppler (Doppler Effect)?", "id": "Perubahan Frekuensi Akibat Gerak Relatif." },
  { "en": "Apa Itu Bunyi (Sound)?", "id": "Gelombang Mekanik Longitudinal Terdengar." },
  { "en": "Apa Medium Rambat Bunyi (Sound)?", "id": "Padat, Cair, Dan Gas." },
  { "en": "Bisakah Bunyi (Sound) Merambat Vakum?", "id": "Tidak Bisa, Butuh Medium." },
  { "en": "Apa Itu Kecepatan Bunyi (Speed of Sound)?", "id": "Tergantung Medium Dan Suhu." },
  { "en": "Medium Mana Kecepatan Bunyi Tercepat?", "id": "Medium Padat." },
  { "en": "Apa Itu Nada (Pitch) Bunyi?", "id": "Tinggi Rendahnya Bunyi (Tergantung Frekuensi)." },
  { "en": "Apa Itu Keras Lemah (Loudness) Bunyi?", "id": "Kuat Lemahnya Bunyi (Tergantung Amplitudo)." },
  { "en": "Apa Satuan Keras Lemah (Loudness) Bunyi?", "id": "Desibel (dB)." },
  { "en": "Apa Itu Timbre (Timbre) Bunyi?", "id": "Warna Bunyi (Kualitas Pembeda)." },
  { "en": "Apa Itu Resonansi (Resonance) Bunyi?", "id": "Ikut Bergetarnya Benda Akibat Bunyi." },
  { "en": "Apa Itu Gema (Echo)?", "id": "Pantulan Bunyi Terdengar Setelah Bunyi Asli." },
  { "en": "Apa Itu Gaung (Reverberation)?", "id": "Pantulan Bunyi Terdengar Hampir Bersamaan Asli." },
  { "en": "Apa Itu Cahaya (Light)?", "id": "Gelombang Elektromagnetik Dapat Terlihat." },
  { "en": "Apa Sifat Dualisme (Dualism) Cahaya?", "id": "Bisa Bersifat Gelombang, Bisa Partikel." },
  { "en": "Apa Nama Partikel (Particle) Cahaya?", "id": "Foton (Photon)." },
  { "en": "Apa Itu Kecepatan Cahaya (Speed of Light) Vakum (c)?", "id": "Sekitar 3 x 10⁸ Meter/Detik." },
  { "en": "Apa Itu Pemantulan (Reflection) Cahaya?", "id": "Cahaya Memantul Dari Permukaan." },
  { "en": "Apa Itu Hukum Pemantulan (Law of Reflection)?", "id": "Sudut Datang Sama Dengan Sudut Pantul." },
  { "en": "Apa Itu Pembiasan (Refraction) Cahaya?", "id": "Pembelokan Cahaya Antar Medium Berbeda." },
  { "en": "Apa Itu Hukum Snellius (Snell's Law) Pembiasan?", "id": "Hubungan Sudut, Indeks Bias Medium." },
  { "en": "Apa Itu Indeks Bias (Refractive Index)?", "id": "Ukuran Kelajuan Cahaya Dalam Medium." },
  { "en": "Apa Itu Dispersi (Dispersion) Cahaya?", "id": "Penguraian Cahaya Putih Menjadi Warna." },
  { "en": "Apa Penyebab Dispersi (Dispersion) Cahaya?", "id": "Perbedaan Indeks Bias Setiap Warna." },
  { "en": "Contoh Dispersi (Dispersion) Cahaya Alami?", "id": "Pelangi (Rainbow)." },
  { "en": "Apa Itu Interferensi (Interference) Cahaya?", "id": "Perpaduan Dua Gelombang Cahaya Koheren." },
  { "en": "Apa Itu Difraksi (Diffraction) Cahaya?", "id": "Pelenturan Cahaya Melewati Celah Sempit." },
  { "en": "Apa Itu Polarisasi (Polarization) Cahaya?", "id": "Penyerapan Arah Getar Cahaya Tertentu." },
  { "en": "Cahaya Termasuk Gelombang Apa (Polarisasi)?", "id": "Gelombang Transversal." },
  { "en": "Apa Itu Lensa (Lens)?", "id": "Benda Bening Membiaskan Cahaya." },
  { "en": "Apa Itu Lensa Cembung (Convex Lens)?", "id": "Mengumpulkan Sinar (Konvergen)." },
  { "en": "Apa Nama Lain Lensa Cembung (Convex Lens)?", "id": "Lensa Positif." },
  { "en": "Apa Itu Lensa Cekung (Concave Lens)?", "id": "Menyebarkan Sinar (Divergen)." },
  { "en": "Apa Nama Lain Lensa Cekung (Concave Lens)?", "id": "Lensa Negatif." },
  { "en": "Apa Itu Jarak Fokus (Focal Length) Lensa?", "id": "Jarak Titik Fokus Dari Pusat Lensa." },
  { "en": "Apa Itu Kekuatan Lensa (Lens Power)?", "id": "Kemampuan Lensa Fokuskan Cahaya (Dioptri)." },
  { "en": "Apa Itu Cermin (Mirror)?", "id": "Permukaan Memantulkan Cahaya." },
  { "en": "Apa Itu Cermin Datar (Plane Mirror)?", "id": "Cermin Permukaan Rata." },
  { "en": "Sifat Bayangan Cermin Datar (Plane Mirror)?", "id": "Maya, Tegak, Sama Besar." },
  { "en": "Apa Itu Cermin Cekung (Concave Mirror)?", "id": "Cermin Permukaan Melengkung Ke Dalam." },
  { "en": "Sifat Cermin Cekung (Concave Mirror)?", "id": "Mengumpulkan Sinar (Konvergen)." },
  { "en": "Apa Itu Cermin Cembung (Convex Mirror)?", "id": "Cermin Permukaan Melengkung Ke Luar." },
  { "en": "Sifat Cermin Cembung (Convex Mirror)?", "id": "Menyebarkan Sinar (Divergen)." },
  { "en": "Sifat Bayangan Cermin Cembung (Convex Mirror)?", "id": "Selalu Maya, Tegak, Diperkecil." },
  { "en": "Apa Itu Alat Optik (Optical Instrument)?", "id": "Alat Menggunakan Prinsip Optik." },
  { "en": "Contoh Alat Optik (Optical Instrument)?", "id": "Mata, Kamera, Lup, Mikroskop, Teleskop." },
  { "en": "Apa Fungsi Lensa Mata Manusia?", "id": "Memfokuskan Cahaya Ke Retina." },
  { "en": "Apa Itu Retina (Retina)?", "id": "Lapisan Peka Cahaya Belakang Mata." },
  { "en": "Apa Itu Lup (Magnifying Glass)?", "id": "Lensa Cembung Tunggal Perbesar Objek." },
  { "en": "Apa Itu Mikroskop (Microscope)?", "id": "Alat Lihat Objek Sangat Kecil." },
  { "en": "Lensa Apa Digunakan Mikroskop (Microscope)?", "id": "Lensa Objektif Dan Lensa Okuler." },
  { "en": "Apa Itu Teleskop (Telescope)?", "id": "Alat Lihat Benda Sangat Jauh." },
  { "en": "Apa Itu Listrik Statis (Static Electricity)?", "id": "Muatan Listrik Diam." },
  { "en": "Apa Itu Medan Listrik (Electric Field)?", "id": "Daerah Pengaruh Muatan Listrik." },
  { "en": "Apa Itu Hukum Coulomb (Coulomb's Law)?", "id": "Gaya Antar Dua Muatan." },
  { "en": "Apa Itu Potensial Listrik (Electric Potential)?", "id": "Energi Potensial Per Muatan." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Komponen Penyimpan Muatan Listrik." },
  { "en": "Apa Itu Arus Listrik (Electric Current)?", "id": "Aliran Muatan Listrik." },
  { "en": "Apa Itu Resistansi (Resistance)?", "id": "Hambatan Aliran Arus." },
  { "en": "Apa Itu Hukum Ohm (Ohm's Law)?", "id": "Hubungan V, I, R." },
  { "en": "Apa Itu Rangkaian Seri (Series Circuit)?", "id": "Satu Jalur Arus." },
  { "en": "Apa Itu Rangkaian Paralel (Parallel Circuit)?", "id": "Banyak Jalur Arus." },
  { "en": "Apa Itu Daya (Power) Listrik?", "id": "Laju Energi Listrik." },
  { "en": "Apa Itu Medan Magnet (Magnetic Field)?", "id": "Daerah Pengaruh Magnet." },
  { "en": "Arus Listrik Menghasilkan Medan Apa?", "id": "Medan Magnet (Elektromagnet)." },
  { "en": "Apa Itu Induksi Elektromagnetik (Electromagnetic Induction)?", "id": "Perubahan Medan Magnet Hasilkan Listrik." },
  { "en": "Aplikasi Induksi Elektromagnetik (Electromagnetic Induction)?", "id": "Generator Dan Transformator." },
  { "en": "Apa Itu Arus Bolak-Balik (AC)?", "id": "Arus Arah Berubah Periodik." },
  { "en": "Apa Itu Arus Searah (DC)?", "id": "Arus Arah Tetap." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Getaran Medan E, B Merambat." },
  { "en": "Apa Itu Spektrum Gelombang Elektromagnetik?", "id": "Urutan Gelombang EM Berdasarkan Frekuensi." },
  { "en": "Apa Satuan Dasar Untuk Muatan Listrik?", "id": "Coulomb (C)." },
  { "en": "Apa Satuan Dasar Untuk Energi Listrik?", "id": "Joule (J) Atau Kilowatt-Hour (kWh)." },
  { "en": "Apa Satuan Dasar Untuk Kapasitansi?", "id": "Farad (F)." },
  { "en": "Apa Satuan Dasar Untuk Induktansi?", "id": "Henry (H)." },
  { "en": "Apa Alat Untuk Mengukur Tegangan?", "id": "Voltmeter." },
  { "en": "Apa Alat Untuk Mengukur Arus?", "id": "Amperemeter." },
  { "en": "Apa Alat Untuk Mengukur Resistansi?", "id": "Ohmmeter." },
  { "en": "Apa Nama Lain Tegangan Listrik?", "id": "Beda Potensial." },
  { "en": "Apa Partikel Pembawa Arus Di Logam?", "id": "Elektron Bebas." },
  { "en": "Apa Hukum Dasar Rangkaian Listrik?", "id": "Hukum Ohm, Hukum Kirchhoff." },
  { "en": "Apa Rumus Daya Dengan Tegangan, Resistansi?", "id": "P = V² / R." },
  { "en": "Apa Rumus Daya Dengan Arus, Resistansi?", "id": "P = I² * R." },
  { "en": "Apa Itu Sumber Tegangan Ideal?", "id": "Tegangan Konstan, Resistansi Internal Nol." },
  { "en": "Apa Itu Sumber Arus Ideal?", "id": "Arus Konstan, Resistansi Internal Tak Hingga." },
  { "en": "Arus Mengalir Dari Potensial Mana?", "id": "Potensial Tinggi Ke Potensial Rendah." },
  { "en": "Apa Itu Rangkaian Terbuka (Open Circuit)?", "id": "Jalur Arus Terputus (Resistansi Tak Hingga)." },
  { "en": "Apa Arus Dalam Rangkaian Terbuka?", "id": "Nol Ampere." },
  { "en": "Apa Itu Hubungan Singkat (Short Circuit)?", "id": "Jalur Arus Resistansi Sangat Rendah." },
  { "en": "Apa Tegangan Antara Hubungan Singkat Ideal?", "id": "Nol Volt." },
  { "en": "Apa Fungsi Kapasitor Dalam Rangkaian DC?", "id": "Memblokir Arus DC Setelah Terisi Penuh." },
  { "en": "Apa Fungsi Induktor Dalam Rangkaian DC?", "id": "Menjadi Hubungan Singkat Setelah Tunak." },
  { "en": "Apa Itu Konstanta Waktu (Time Constant)?", "id": "Ukuran Kecepatan Respon Rangkaian RC/RL." },
  { "en": "Rumus Konstanta Waktu (Time Constant) RC?", "id": "Tau = R Kali C." },
  { "en": "Rumus Konstanta Waktu (Time Constant) RL?", "id": "Tau = L Dibagi R." },
  { "en": "Apa Itu Frekuensi (Frequency) Arus AC?", "id": "Jumlah Siklus Per Detik." },
  { "en": "Apa Satuan Frekuensi (Frequency)?", "id": "Hertz (Hz)." },
  { "en": "Apa Itu Periode (Period) Arus AC?", "id": "Waktu Untuk Satu Siklus Lengkap." },
  { "en": "Satuan Periode (Period)?", "id": "Detik (s)." },
  { "en": "Apa Itu Amplitudo (Amplitude) Sinyal AC?", "id": "Nilai Puncak Maksimum Gelombang." },
  { "en": "Apa Itu Nilai RMS (Root Mean Square)?", "id": "Nilai Efektif Setara DC." },
  { "en": "Apa Itu Reaktansi Kapasitif (Xc)?", "id": "Hambatan Kapasitor Terhadap Arus AC." },
  { "en": "Apa Itu Reaktansi Induktif (Xl)?", "id": "Hambatan Induktor Terhadap Arus AC." },
  { "en": "Bagaimana Reaktansi (Reactance) Berubah Dengan Frekuensi?", "id": "Xc Turun, Xl Naik." },
  { "en": "Apa Itu Impedansi (Impedance) (Z)?", "id": "Hambatan Total Rangkaian AC." },
  { "en": "Apa Itu Resonansi (Resonance) Rangkaian?", "id": "Kondisi Xl Sama Dengan Xc." },
  { "en": "Apa Itu Faktor Daya (Power Factor)?", "id": "Rasio Daya Nyata, Daya Semu." },
  { "en": "Nilai Faktor Daya (Power Factor) Ideal?", "id": "Satu (Resistif Murni)." },
  { "en": "Apa Itu Daya Nyata (Real Power) (P)?", "id": "Daya Yang Melakukan Kerja (Watt)." },
  { "en": "Apa Itu Daya Reaktif (Reactive Power) (Q)?", "id": "Daya Medan Listrik/Magnet (VAR)." },
  { "en": "Apa Itu Daya Semu (Apparent Power) (S)?", "id": "Magnitudo Daya Total (VA)." },
  { "en": "Apa Fungsi Dioda (Diode)?", "id": "Menyearahkan Arus Listrik." },
  { "en": "Apa Itu Penyearah (Rectifier) Setengah Gelombang?", "id": "Gunakan Satu Dioda." },
  { "en": "Apa Itu Penyearah (Rectifier) Gelombang Penuh?", "id": "Gunakan Dua Atau Empat Dioda." },
  { "en": "Apa Fungsi Transistor (Transistor)?", "id": "Sebagai Penguat Atau Saklar." },
  { "en": "Apa Itu Penguatan (Gain)?", "id": "Rasio Output Terhadap Input Penguat." },
  { "en": "Apa Itu Sirkuit Digital (Digital Circuit)?", "id": "Sirkuit Operasi Logika Biner." },
  { "en": "Apa Dua Level Logika Digital?", "id": "Tinggi (High/1), Rendah (Low/0)." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate) AND?", "id": "Output Tinggi Jika Semua Input Tinggi." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate) OR?", "id": "Output Tinggi Jika Ada Input Tinggi." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate) NOT?", "id": "Membalikkan Level Logika Input." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Aman Ke Potensial Bumi." },
  { "en": "Apa Tujuan Utama Grounding (Pembumian)?", "id": "Keselamatan Dari Sengatan Listrik." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Pelindung Arus Lebih (Meleleh Putus)." },
  { "en": "Apa Itu MCB (Miniature Circuit Breaker)?", "id": "Pemutus Arus Lebih (Bisa Reset)." },
  { "en": "Apa Itu Isolator (Insulator)?", "id": "Bahan Sulit Hantarkan Listrik." },
  { "en": "Contoh Isolator (Insulator) Yang Baik?", "id": "Karet, Plastik, Kaca, Udara." },
  { "en": "Apa Itu Konduktor (Conductor)?", "id": "Bahan Mudah Hantarkan Listrik." },
  { "en": "Contoh Konduktor (Conductor) Yang Baik?", "id": "Tembaga, Perak, Emas, Aluminium." },
  { "en": "Apa Itu Medan Magnet (Magnetic Field)?", "id": "Daerah Sekitar Magnet/Arus." },
  { "en": "Arus Listrik Menghasilkan Apa?", "id": "Medan Magnet Di Sekitarnya." },
  { "en": "Apa Itu Induksi Elektromagnetik (Electromagnetic Induction)?", "id": "Perubahan Medan Magnet Hasilkan Listrik." },
  { "en": "Siapa Menemukan Induksi Elektromagnetik (Electromagnetic Induction)?", "id": "Michael Faraday." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Mengubah Tegangan AC Pakai Induksi." },
  { "en": "Apa Itu Motor (Motor)?", "id": "Ubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator)?", "id": "Ubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Pengukuran (Measurement) Listrik?", "id": "Menentukan Nilai Besaran Listrik." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Ukuran Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Keterulangan Hasil Pengukuran Sama." },
  { "en": "Alat Apa Ukur Bentuk Gelombang?", "id": "Osiloskop (Oscilloscope)." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Pembangkitan Hingga Penggunaan Listrik." },
  { "en": "Mengapa Listrik Ditransmisikan Tegangan Tinggi?", "id": "Mengurangi Rugi-Rugi Daya (I²R)." },
  { "en": "Apa Fungsi Gardu Induk (Substation)?", "id": "Mengubah Level Tegangan Listrik." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Mengamankan Peralatan Dari Gangguan." },
  { "en": "Apa Itu Hubungan Singkat (Short Circuit)?", "id": "Jalur Arus Resistansi Sangat Rendah." },
  { "en": "Apa Akibat Hubungan Singkat (Short Circuit)?", "id": "Arus Sangat Besar, Bisa Merusak." },
  { "en": "Apa Itu Beban Lebih (Overload)?", "id": "Arus Melebihi Kapasitas Normal Peralatan." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Menggunakan Output Mengatur Input Kontrol." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Alat Ukur Besaran Fisik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Penggerak Berdasarkan Sinyal Kontrol." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Kontrol Aliran Daya Semikonduktor." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Mengubah AC Menjadi DC." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Mengubah DC Menjadi AC." },
  { "en": "Apa Itu Komunikasi Data (Data Communication)?", "id": "Transfer Informasi Digital." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komputer." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Global." },
  { "en": "Apa Itu IP Address (Alamat IP)?", "id": "Alamat Logis Perangkat Jaringan." },
  { "en": "Apa Itu Router (Router)?", "id": "Penghubung Antar Jaringan Berbeda." },
  { "en": "Apa Itu Sinyal (Signal)?", "id": "Besaran Fisik Membawa Informasi." },
  { "en": "Apa Itu Sinyal Analog (Analog Signal)?", "id": "Sinyal Dengan Nilai Kontinu." },
  { "en": "Apa Itu Sinyal Digital (Digital Signal)?", "id": "Sinyal Dengan Nilai Diskrit." },
  { "en": "Apa Itu Noise (Derau)?", "id": "Gangguan Acak Pada Sinyal." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Rangkaian Seleksi Frekuensi Sinyal." },
  { "en": "Apa Itu Bandwidth (Bandwidth)?", "id": "Rentang Frekuensi Sinyal/Sistem." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Proses Menumpangkan Informasi Ke Pembawa." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Proses Memisahkan Informasi Dari Pembawa." },
  { "en": "Apa Itu Listrik Statis (Static Electricity)?", "id": "Ketidakseimbangan Muatan Listrik Diam." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis Tiba-Tiba." },
  { "en": "Mengapa ESD (Electrostatic Discharge) Berbahaya Elektronik?", "id": "Dapat Merusak Komponen Sensitif." },
  { "en": "Apa Itu Resistivitas (Resistivity)?", "id": "Sifat Intrinsik Hambatan Bahan." },
  { "en": "Apa Satuan Resistivitas (Resistivity)?", "id": "Ohm Meter (Ω·m)." },
  { "en": "Apa Itu Konduktivitas (Conductivity)?", "id": "Sifat Intrinsik Kemudahan Hantar Listrik." },
  { "en": "Apa Satuan Konduktivitas (Conductivity)?", "id": "Siemens Per Meter (S/m)." },
  { "en": "Apa Hubungan Konduktivitas, Resistivitas?", "id": "Konduktivitas Adalah Kebalikan Resistivitas." },
  { "en": "Apa Itu Efek Suhu Pada Resistansi Logam?", "id": "Resistansi Umumnya Meningkat Dengan Suhu." },
  { "en": "Apa Itu Koefisien Suhu Resistansi (TCR)?", "id": "Ukuran Perubahan Resistansi Per Derajat." },
  { "en": "Apa Itu Superkonduktor (Superconductor)?", "id": "Bahan Resistansi Nol Dibawah Suhu Kritis." },
  { "en": "Apa Itu Termokopel (Thermocouple)?", "id": "Sensor Suhu Berbasis Efek Seebeck." },
  { "en": "Apa Itu Efek Seebeck (Seebeck Effect)?", "id": "Tegangan Timbul Akibat Beda Suhu Dua Logam." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu Berbasis Perubahan Resistansi Logam." },
  { "en": "Logam Apa Umum Digunakan RTD?", "id": "Platina (Platinum)." },
  { "en": "Apa Itu Termistor (Thermistor)?", "id": "Resistor Semikonduktor Sangat Peka Suhu." },
  { "en": "Apa Itu Termistor NTC (Negative Temperature Coefficient)?", "id": "Resistansi Turun Saat Suhu Naik." },
  { "en": "Apa Itu Termistor PTC (Positive Temperature Coefficient)?", "id": "Resistansi Naik Saat Suhu Naik." },
  { "en": "Apa Itu Efek Hall (Hall Effect)?", "id": "Tegangan Timbul Konduktor Medan Magnet." },
  { "en": "Apa Aplikasi Sensor Efek Hall (Hall Effect)?", "id": "Deteksi Posisi, Kecepatan, Arus." },
  { "en": "Apa Itu Efek Piezoelektrik (Piezoelectric Effect)?", "id": "Tekanan Mekanis Hasilkan Muatan Listrik." },
  { "en": "Bahan Apa Menunjukkan Efek Piezoelektrik?", "id": "Kuarsa (Quartz), Keramik PZT." },
  { "en": "Aplikasi Sensor Piezoelektrik (Piezoelectric Sensor)?", "id": "Sensor Tekanan, Getaran, Akselerometer." },
  { "en": "Apa Itu Efek Fotolistrik (Photoelectric Effect)?", "id": "Pelepasan Elektron Logam Disinari Cahaya." },
  { "en": "Siapa Menjelaskan Efek Fotolistrik (Photoelectric Effect)?", "id": "Albert Einstein." },
  { "en": "Apa Itu Sel Surya (Solar Cell)?", "id": "Mengubah Energi Cahaya Menjadi Listrik." },
  { "en": "Prinsip Apa Digunakan Sel Surya?", "id": "Efek Fotovoltaik (Photovoltaic Effect)." },
  { "en": "Bahan Apa Umum Digunakan Sel Surya?", "id": "Silikon (Silicon)." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Memancarkan Cahaya." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Sensor Cahaya." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Nilainya Tergantung Cahaya." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit) (IC)?", "id": "Rangkaian Kompleks Dalam Chip Silikon." },
  { "en": "Apa Skala Integrasi (Scale of Integration)?", "id": "Ukuran Kepadatan Komponen Dalam IC." },
  { "en": "Apa Itu SSI (Small Scale Integration)?", "id": "Beberapa Gerbang Logika." },
  { "en": "Apa Itu MSI (Medium Scale Integration)?", "id": "Puluhan Hingga Ratusan Gerbang." },
  { "en": "Apa Itu LSI (Large Scale Integration)?", "id": "Ribuan Gerbang (Mikroprosesor Awal)." },
  { "en": "Apa Itu VLSI (Very Large Scale Integration)?", "id": "Ratusan Ribu Hingga Jutaan Gerbang." },
  { "en": "Apa Itu ULSI (Ultra Large Scale Integration)?", "id": "Lebih Dari Satu Juta Gerbang." },
  { "en": "Apa Itu Hukum Moore (Moore's Law)?", "id": "Prediksi Jumlah Transistor Chip Gandakan." },
  { "en": "Apa Itu Wafer (Wafer) Silikon?", "id": "Lembaran Dasar Pembuatan IC." },
  { "en": "Apa Itu Die (Die) IC?", "id": "Potongan Chip Individual Dari Wafer." },
  { "en": "Apa Itu Kemasan (Packaging) IC?", "id": "Wadah Pelindung Dengan Pin Koneksi." },
  { "en": "Apa Itu Pinout (Pinout)?", "id": "Diagram Fungsi Kaki-Kaki IC." },
  { "en": "Apa Itu Datasheet (Lembar Data)?", "id": "Dokumen Spesifikasi Teknis Komponen." },
  { "en": "Apa Itu Logika TTL (Transistor-Transistor Logic)?", "id": "Keluarga Logika Berbasis BJT." },
  { "en": "Berapa Tegangan Catu Daya (Supply Voltage) TTL Standar?", "id": "Lima Volt (5V)." },
  { "en": "Apa Itu Logika CMOS (Complementary MOS)?", "id": "Keluarga Logika Berbasis MOSFET." },
  { "en": "Apa Keunggulan Logika CMOS (Complementary MOS)?", "id": "Konsumsi Daya Statis Rendah." },
  { "en": "Apa Itu Noise Margin (Margin Derau)?", "id": "Toleransi Level Tegangan Terhadap Noise." },
  { "en": "Apa Itu Fan-Out (Fan-Out)?", "id": "Jumlah Maksimal Input Dapat Digerakkan Output." },
  { "en": "Apa Itu Waktu Propagasi Tunda (Propagation Delay)?", "id": "Waktu Sinyal Merambat Lewat Gerbang." },
  { "en": "Apa Itu Output Open Collector (Kolektor Terbuka)?", "id": "Output BJT Tanpa Resistor Pull-Up Internal." },
  { "en": "Apa Itu Output Open Drain (Drain Terbuka)?", "id": "Output MOSFET Tanpa Resistor Pull-Up Internal." },
  { "en": "Apa Kegunaan Output Open Collector/Drain?", "id": "Implementasi Wired Logic, Level Shifting." },
  { "en": "Apa Itu Resistor Pull-Up (Pull-Up Resistor)?", "id": "Resistor Ke VCC Jaga Level High Default." },
  { "en": "Apa Itu Resistor Pull-Down (Pull-Down Resistor)?", "id": "Resistor Ke Ground Jaga Level Low Default." },
  { "en": "Apa Itu Buffer (Buffer) Tri-State?", "id": "Output Bisa High, Low, Impedansi Tinggi." },
  { "en": "Apa Kegunaan Keadaan Impedansi Tinggi (Hi-Z)?", "id": "Berbagi Jalur Bus Bersama." },
  { "en": "Apa Itu Bus (Bus) Data?", "id": "Sekumpulan Jalur Paralel Transfer Data." },
  { "en": "Apa Itu Latch (Latch)?", "id": "Elemen Memori Sensitif Level." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Sensitif Tepi Clock." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Simpan Data Biner." },
  { "en": "Apa Itu Register Geser (Shift Register)?", "id": "Menggeser Data Bit Per Bit." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Sekuensial Menghitung Pulsa." },
  { "en": "Apa Itu Multiplexer (Multiplexer) (MUX)?", "id": "Memilih Satu Input Dari Banyak Input." },
  { "en": "Apa Sinyal Kontrol Multiplexer (Multiplexer)?", "id": "Sinyal Pilih (Select Lines)." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) (DEMUX)?", "id": "Mengarahkan Satu Input Ke Banyak Output." },
  { "en": "Apa Itu Encoder (Encoder)?", "id": "Mengubah Input Aktif Menjadi Kode Biner." },
  { "en": "Apa Itu Decoder (Decoder)?", "id": "Mengubah Kode Biner Menjadi Output Aktif." },
  { "en": "Contoh Decoder (Decoder) Umum?", "id": "Decoder BCD Ke 7-Segmen." },
  { "en": "Apa Itu Memori (Memory) Semikonduktor?", "id": "Penyimpanan Data Berbasis IC." },
  { "en": "Apa Itu RAM Statis (SRAM)?", "id": "Gunakan Flip-Flop (Cepat, Mahal)." },
  { "en": "Apa Itu RAM Dinamis (DRAM)?", "id": "Gunakan Kapasitor (Perlu Refresh, Murah)." },
  { "en": "Apa Itu Memori Flash (Flash Memory)?", "id": "Memori Non-Volatil Dapat Dihapus Elektrik." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Konverter Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Konverter Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Arsitektur Harvard (Harvard Architecture)?", "id": "Bus Terpisah Data Dan Instruksi." },
  { "en": "Apa Itu Arsitektur Von Neumann (Von Neumann Architecture)?", "id": "Bus Bersama Data Dan Instruksi." },
  { "en": "Apa Itu GPIO (General Purpose Input Output)?", "id": "Pin Digital Serbaguna Mikrokontroler." },
  { "en": "Apa Itu Timer/Counter (Timer/Counter) Mikrokontroler?", "id": "Peripheral Penghitung Waktu/Pulsa." },
  { "en": "Apa Itu PWM (Pulse Width Modulation) Mikrokontroler?", "id": "Output Sinyal Kotak Lebar Variabel." },
  { "en": "Apa Itu Interrupt (Interupsi) Mikrokontroler?", "id": "Mekanisme Respon Cepat Kejadian." },
  { "en": "Apa Itu Watchdog Timer (WDT) Mikrokontroler?", "id": "Pengawas Program Agar Tidak Macet." },
  { "en": "Apa Itu Komunikasi UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Komunikasi Serial Asinkron (TX, RX)." },
  { "en": "Apa Itu Komunikasi SPI (Serial Peripheral Interface)?", "id": "Komunikasi Serial Sinkron (MOSI, MISO, SCK, SS)." },
  { "en": "Apa Itu Komunikasi I2C (Inter-Integrated Circuit)?", "id": "Komunikasi Serial Dua Kawat (SDA, SCL)." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Pengubah Besaran Fisik Ke Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Pengubah Sinyal Listrik Ke Aksi Fisik." },
  { "en": "Apa Itu Sistem Tertanam (Embedded System)?", "id": "Sistem Komputer Dedikasi Fungsi Khusus." },
  { "en": "Apa Itu RTOS (Real-Time Operating System)?", "id": "Sistem Operasi Respon Waktu Terjamin." },
  { "en": "Apa Itu FPGA (Field Programmable Gate Array)?", "id": "IC Logika Dapat Dikonfigurasi Ulang." },
  { "en": "Apa Unit Logika Dasar FPGA?", "id": "LUT (Look-Up Table) Dan Flip-Flop." },
  { "en": "Apa Bahasa Deskripsi Perangkat Keras (HDL)?", "id": "Bahasa Pemrograman Deskripsi Hardware." },
  { "en": "Contoh Bahasa HDL (Hardware Description Language)?", "id": "VHDL, Verilog." },
  { "en": "Apa Itu Sintesis (Synthesis) HDL?", "id": "Menerjemahkan Kode HDL Ke Struktur Logika." },
  { "en": "Apa Itu Simulasi (Simulation) HDL?", "id": "Memverifikasi Fungsionalitas Desain HDL." },
  { "en": "Apa Itu ASIC (Application Specific Integrated Circuit)?", "id": "IC Dirancang Khusus Aplikasi Tertentu." },
  { "en": "Apa Perbedaan FPGA Dan ASIC?", "id": "FPGA Fleksibel, ASIC Kinerja/Efisiensi Tinggi." },
  { "en": "Apa Itu Pengolahan Sinyal Digital (DSP)?", "id": "Pemrosesan Sinyal Domain Digital." },
  { "en": "Apa Itu Filter Digital (Digital Filter)?", "id": "Algoritma Modifikasi Spektrum Sinyal." },
  { "en": "Apa Itu Transformasi Fourier Cepat (FFT)?", "id": "Algoritma Cepat Analisis Frekuensi." },
  { "en": "Apa Itu Pemrosesan Citra Digital (Digital Image Processing)?", "id": "Manipulasi Gambar Digital Komputer." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Interpretasi Komputer Terhadap Gambar/Video." },
  { "en": "Apa Itu Pembelajaran Mesin (Machine Learning)?", "id": "Algoritma Belajar Dari Data." },
  { "en": "Apa Itu Jaringan Saraf Tiruan (Artificial Neural Network)?", "id": "Model Komputasi Terinspirasi Otak." },
  { "en": "Apa Itu Pembelajaran Mendalam (Deep Learning)?", "id": "Pembelajaran Mesin Jaringan Saraf Dalam." },
  { "en": "Apa Itu Kecerdasan Buatan (Artificial Intelligence)?", "id": "Kemampuan Mesin Meniru Kecerdasan Manusia." },
  { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Perangkat Fisik Terhubung Internet." },
  { "en": "Apa Sensor (Sensor) Dalam IoT?", "id": "Mengumpulkan Data Dari Lingkungan Fisik." },
  { "en": "Apa Aktuator (Actuator) Dalam IoT?", "id": "Mengontrol Perangkat Fisik Jarak Jauh." },
  { "en": "Protokol Komunikasi IoT (Internet of Things) Umum?", "id": "MQTT, CoAP, HTTP." },
  { "en": "Apa Itu MQTT (Message Queuing Telemetry Transport)?", "id": "Protokol Pesan Ringan IoT (Pub/Sub)." },
  { "en": "Apa Itu Publisher (Penerbit) MQTT?", "id": "Klien Mengirim Pesan Ke Topik." },
  { "en": "Apa Itu Subscriber (Pelanggan) MQTT?", "id": "Klien Menerima Pesan Dari Topik." },
  { "en": "Apa Itu Broker (Broker) MQTT?", "id": "Server Pusat Penyalur Pesan MQTT." },
  { "en": "Apa Itu Topik (Topic) MQTT?", "id": "Label Hierarkis Alamat Pesan." },
  { "en": "Apa Itu Quality of Service (QoS) MQTT?", "id": "Tingkat Jaminan Pengiriman Pesan (0, 1, 2)." },
  { "en": "QoS (Quality of Service) 0 MQTT?", "id": "At Most Once (Paling Banyak Sekali)." },
  { "en": "QoS (Quality of Service) 1 MQTT?", "id": "At Least Once (Minimal Sekali)." },
  { "en": "QoS (Quality of Service) 2 MQTT?", "id": "Exactly Once (Tepat Sekali)." },
  { "en": "Apa Itu CoAP (Constrained Application Protocol)?", "id": "Protokol Aplikasi Ringan IoT (Request/Response)." },
  { "en": "Apa Kepanjangan CoAP (Constrained Application Protocol)?", "id": "Protokol Aplikasi Terbatas." },
  { "en": "Protokol Transport Apa Digunakan CoAP?", "id": "UDP (User Datagram Protocol)." },
  { "en": "Apa Itu Bluetooth Low Energy (BLE)?", "id": "Versi Bluetooth Hemat Daya IoT." },
  { "en": "Apa Kepanjangan BLE (Bluetooth Low Energy)?", "id": "Bluetooth Energi Rendah." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Layanan Komputasi Melalui Internet." },
  { "en": "Apa Model Layanan Cloud (Cloud Service Model) Utama?", "id": "IaaS, PaaS, SaaS." },
  { "en": "Apa Itu IaaS (Infrastructure as a Service)?", "id": "Menyediakan Infrastruktur IT Virtual." },
  { "en": "Apa Itu PaaS (Platform as a Service)?", "id": "Menyediakan Platform Pengembangan Aplikasi." },
  { "en": "Apa Itu SaaS (Software as a Service)?", "id": "Menyediakan Perangkat Lunak Lewat Web." },
  { "en": "Apa Itu Komputasi Tepi (Edge Computing)?", "id": "Pemrosesan Data Dekat Sumber Data." },
  { "en": "Apa Keuntungan Komputasi Tepi (Edge Computing)?", "id": "Latensi Rendah, Hemat Bandwidth." },
  { "en": "Apa Itu Keamanan Siber (Cybersecurity)?", "id": "Perlindungan Sistem Komputer Dari Serangan." },
  { "en": "Apa Itu Enkripsi (Encryption)?", "id": "Mengubah Data Jadi Tidak Terbaca." },
  { "en": "Apa Itu Dekripsi (Decryption)?", "id": "Mengembalikan Data Terenkripsi Jadi Asli." },
  { "en": "Apa Itu Kriptografi Kunci Simetris?", "id": "Gunakan Kunci Sama Enkripsi, Dekripsi." },
  { "en": "Apa Itu Kriptografi Kunci Asimetris (Publik)?", "id": "Gunakan Pasangan Kunci Publik, Privat." },
  { "en": "Apa Kunci Publik (Public Key)?", "id": "Kunci Disebar Untuk Enkripsi." },
  { "en": "Apa Kunci Privat (Private Key)?", "id": "Kunci Rahasia Untuk Dekripsi." },
  { "en": "Algoritma Enkripsi Simetris Contoh?", "id": "AES (Advanced Encryption Standard)." },
  { "en": "Algoritma Enkripsi Asimetris Contoh?", "id": "RSA (Rivest–Shamir–Adleman)." },
  { "en": "Apa Itu Fungsi Hash (Hash Function)?", "id": "Fungsi Satu Arah Hasilkan Hash Tetap." },
  { "en": "Apa Sifat Fungsi Hash (Hash Function)?", "id": "Deterministik, Sulit Dibalik, Tahan Kolisi." },
  { "en": "Apa Itu Tanda Tangan Digital (Digital Signature)?", "id": "Verifikasi Otentikasi, Integritas Data." },
  { "en": "Apa Itu Sertifikat Digital (Digital Certificate)?", "id": "Mengikat Identitas Dengan Kunci Publik." },
  { "en": "Siapa Penerbit Sertifikat Digital?", "id": "Otoritas Sertifikat (Certificate Authority - CA)." },
  { "en": "Apa Itu Firewall (Firewall)?", "id": "Sistem Keamanan Memfilter Lalu Lintas Jaringan." },
  { "en": "Apa Itu Intrusion Detection System (IDS)?", "id": "Mendeteksi Aktivitas Mencurigakan Jaringan." },
  { "en": "Apa Itu Intrusion Prevention System (IPS)?", "id": "Mendeteksi Dan Memblokir Serangan." },
  { "en": "Apa Itu Malware (Malicious Software)?", "id": "Perangkat Lunak Berbahaya." },
  { "en": "Jenis Malware (Malicious Software) Umum?", "id": "Virus, Worm, Trojan, Ransomware." },
  { "en": "Apa Itu Virus Komputer (Computer Virus)?", "id": "Malware (Malicious Software) Menempel Program Lain." },
  { "en": "Apa Itu Worm (Worm) Komputer?", "id": "Malware (Malicious Software) Menyebar Sendiri Jaringan." },
  { "en": "Apa Itu Trojan Horse (Kuda Troya)?", "id": "Malware (Malicious Software) Menyamar Program Sah." },
  { "en": "Apa Itu Ransomware (Ransomware)?", "id": "Malware (Malicious Software) Enkripsi Data Minta Tebusan." },
  { "en": "Apa Itu Phishing (Phishing)?", "id": "Upaya Penipuan Dapatkan Informasi Sensitif." },
  { "en": "Apa Itu Serangan Man-in-the-Middle (MITM)?", "id": "Penyadap Komunikasi Antara Dua Pihak." },
  { "en": "Apa Itu Serangan Denial of Service (DoS)?", "id": "Membuat Layanan Tidak Tersedia Pengguna." },
  { "en": "Apa Itu Serangan Distributed DoS (DDoS)?", "id": "DoS (Denial of Service) Dari Banyak Sumber." },
  { "en": "Apa Itu Jaringan Pribadi Virtual (VPN)?", "id": "Membuat Koneksi Aman Jaringan Publik." },
  { "en": "Apa Kepanjangan VPN (Virtual Private Network)?", "id": "Jaringan Pribadi Virtual." },
  { "en": "Apa Itu Otentikasi (Authentication)?", "id": "Proses Verifikasi Identitas Pengguna." },
  { "en": "Apa Itu Otorisasi (Authorization)?", "id": "Pemberian Hak Akses Setelah Otentikasi." },
  { "en": "Apa Itu Otentikasi Dua Faktor (2FA)?", "id": "Membutuhkan Dua Jenis Bukti Identitas." },
  { "en": "Contoh Faktor Otentikasi (Authentication Factor)?", "id": "Password, Token Fisik, Biometrik." },
  { "en": "Apa Itu Biometrik (Biometrics)?", "id": "Pengenalan Berdasarkan Ciri Fisik/Perilaku." },
  { "en": "Contoh Biometrik (Biometrics)?", "id": "Sidik Jari, Pengenalan Wajah, Retina." },
  { "en": "Apa Itu Cadangan Data (Data Backup)?", "id": "Menyalin Data Untuk Pemulihan Bencana." },
  { "en": "Apa Strategi Cadangan (Backup Strategy) Umum?", "id": "Full Backup, Incremental, Differential." },
  { "en": "Apa Itu Pemulihan Bencana (Disaster Recovery)?", "id": "Proses Memulihkan Sistem Setelah Bencana." },
  { "en": "Apa Itu Rencana Pemulihan Bencana (DRP)?", "id": "Dokumen Prosedur Pemulihan Bencana." },
  { "en": "Apa Itu Ketersediaan Tinggi (High Availability)?", "id": "Desain Sistem Meminimalkan Downtime." },
  { "en": "Apa Itu Redundansi (Redundancy)?", "id": "Duplikasi Komponen Kritis Sistem." },
  { "en": "Apa Itu Failover (Failover)?", "id": "Pengalihan Otomatis Ke Sistem Cadangan." },
  { "en": "Apa Itu Load Balancing (Penyeimbangan Beban)?", "id": "Distribusi Beban Kerja Antar Server." },
  { "en": "Apa Itu Skalabilitas (Scalability)?", "id": "Kemampuan Sistem Menangani Peningkatan Beban." },
  { "en": "Apa Itu Skala Vertikal (Vertical Scaling)?", "id": "Menambah Sumber Daya Server Tunggal." },
  { "en": "Apa Itu Skala Horizontal (Horizontal Scaling)?", "id": "Menambah Jumlah Server." },
  { "en": "Apa Itu Kontainerisasi (Containerization)?", "id": "Mengemas Aplikasi Beserta Dependensinya." },
  { "en": "Platform Kontainerisasi (Containerization) Populer?", "id": "Docker (Docker)." },
  { "en": "Apa Itu Orkestrasi Kontainer (Container Orchestration)?", "id": "Manajemen Otomatis Kontainer Skala Besar." },
  { "en": "Platform Orkestrasi Kontainer Populer?", "id": "Kubernetes (Kubernetes)." },
  { "en": "Apa Itu Virtualisasi (Virtualization)?", "id": "Membuat Versi Virtual Sumber Daya Komputasi." },
  { "en": "Apa Itu Mesin Virtual (Virtual Machine) (VM)?", "id": "Simulasi Sistem Komputer Lengkap." },
  { "en": "Apa Itu Hypervisor (Hypervisor)?", "id": "Perangkat Lunak Pembuat, Pengelola VM." },
  { "en": "Apa Beda Kontainer, VM (Virtual Machine)?", "id": "Kontainer Berbagi OS Kernel Host." },
  { "en": "Mana Lebih Ringan, Kontainer Atau VM?", "id": "Kontainer (Container)." },
  { "en": "Apa Itu Komputasi Tanpa Server (Serverless)?", "id": "Eksekusi Kode Tanpa Kelola Infrastruktur Server." },
  { "en": "Apa Itu Function as a Service (FaaS)?", "id": "Model Komputasi Serverless Berbasis Fungsi." },
  { "en": "Contoh Platform FaaS (Function as a Service)?", "id": "AWS Lambda, Google Cloud Functions." },
  { "en": "Apa Itu Infrastruktur Sebagai Kode (IaC)?", "id": "Mengelola Infrastruktur Menggunakan Kode." },
  { "en": "Alat IaC (Infrastructure as Code) Populer?", "id": "Terraform, Ansible, CloudFormation." },
  { "en": "Apa Itu Continuous Integration (CI)?", "id": "Integrasi Kode Otomatis Secara Berkala." },
  { "en": "Apa Itu Continuous Delivery (CD)?", "id": "Pengiriman Perubahan Kode Otomatis Ke Produksi." },
  { "en": "Apa Itu Pipeline CI/CD (CI/CD Pipeline)?", "id": "Alur Kerja Otomatis Build, Test, Deploy." },
  { "en": "Alat CI/CD (CI/CD Pipeline) Populer?", "id": "Jenkins, GitLab CI, GitHub Actions." },
  { "en": "Apa Itu Pengujian Otomatis (Automated Testing)?", "id": "Menjalankan Skrip Tes Secara Otomatis." },
  { "en": "Apa Itu Pemantauan (Monitoring)?", "id": "Mengamati Kinerja, Kesehatan Sistem." },
  { "en": "Apa Itu Pencatatan Log (Logging)?", "id": "Merekam Kejadian (Event) Sistem." },
  { "en": "Apa Itu Metrik (Metrics)?", "id": "Pengukuran Kuantitatif Kinerja Sistem." },
  { "en": "Apa Itu Pelacakan (Tracing)?", "id": "Melacak Alur Permintaan Sistem Terdistribusi." },
  { "en": "Apa Itu Manajemen Konfigurasi (Configuration Management)?", "id": "Mengelola Konfigurasi Sistem Konsisten." },
  { "en": "Alat Manajemen Konfigurasi (Configuration Management) Populer?", "id": "Ansible, Chef, Puppet, SaltStack." },
  { "en": "Apa Itu Devops (Devops)?", "id": "Kolaborasi Antara Tim Pengembangan, Operasi." },
  { "en": "Tujuan Utama Devops (Devops)?", "id": "Mempercepat Siklus Hidup Pengembangan Software." },
  { "en": "Apa Itu SRE (Site Reliability Engineering)?", "id": "Pendekatan Rekayasa Keandalan Operasi Skala Besar." },
  { "en": "Apa Itu SLO (Service Level Objective)?", "id": "Target Kuantitatif Keandalan Layanan." },
  { "en": "Apa Itu SLI (Service Level Indicator)?", "id": "Metrik Pengukuran Pencapaian SLO." },
  { "en": "Apa Itu Error Budget (Anggaran Kesalahan)?", "id": "Batas Toleransi Ketidakandalan (1 - SLO)." },
  { "en": "Apa Itu Lisensi Perangkat Lunak (Software License)?", "id": "Izin Legal Penggunaan Perangkat Lunak." },
  { "en": "Apa Itu Perangkat Lunak Bebas (Free Software)?", "id": "Kebebasan Menjalankan, Mempelajari, Modifikasi, Distribusi." },
  { "en": "Apa Itu Perangkat Lunak Open Source (Sumber Terbuka)?", "id": "Kode Sumber Tersedia Publik." },
  { "en": "Apakah Semua Open Source Itu Bebas?", "id": "Tidak Selalu (Tergantung Lisensi)." },
  { "en": "Apa Itu Lisensi Copyleft (Copyleft License)?", "id": "Wajibkan Karya Turunan Tetap Bebas/Terbuka." },
  { "en": "Contoh Lisensi Copyleft (Copyleft License)?", "id": "GNU GPL (General Public License)." },
  { "en": "Apa Itu Lisensi Permisif (Permissive License)?", "id": "Batasan Minimal Penggunaan Kode." },
  { "en": "Contoh Lisensi Permisif (Permissive License)?", "id": "MIT, BSD, Apache License." },
  { "en": "Apa Itu Perangkat Lunak Proprietary (Proprietary Software)?", "id": "Perangkat Lunak Sumber Tertutup (Komersial)." },
  { "en": "Apa Itu Pembajakan Perangkat Lunak (Software Piracy)?", "id": "Penggunaan, Distribusi Software Ilegal." },
  { "en": "Apa Itu Manajemen Aset Perangkat Lunak (SAM)?", "id": "Mengelola Lisensi Perangkat Lunak Organisasi." },
  { "en": "Apa Itu Kompatibilitas Elektromagnetik (EMC)?", "id": "Kemampuan Perangkat Berfungsi Tanpa Gangguan EM." },
  { "en": "Apa Kepanjangan EMC (Electromagnetic Compatibility)?", "id": "Kompatibilitas Elektromagnetik." },
  { "en": "Apa Itu Interferensi Elektromagnetik (EMI)?", "id": "Gangguan Elektromagnetik Merusak Kinerja." },
  { "en": "Apa Kepanjangan EMI (Electromagnetic Interference)?", "id": "Interferensi Elektromagnetik." },
  { "en": "Apa Dua Aspek Utama EMC (Electromagnetic Compatibility)?", "id": "Emisi (Emission) Dan Imunitas (Immunity)." },
  { "en": "Apa Itu Emisi (Emission) EMI?", "id": "Pancaran Energi EM Tak Diinginkan Perangkat." },
  { "en": "Apa Itu Imunitas (Immunity) EMI?", "id": "Kemampuan Perangkat Tahan Gangguan EM." },
  { "en": "Apa Itu Emisi Konduksi (Conducted Emission)?", "id": "Gangguan Merambat Lewat Kabel Daya/Sinyal." },
  { "en": "Apa Itu Emisi Radiasi (Radiated Emission)?", "id": "Gangguan Merambat Lewat Udara (Gelombang EM)." },
  { "en": "Apa Itu Imunitas Konduksi (Conducted Immunity)?", "id": "Ketahanan Terhadap Gangguan Lewat Kabel." },
  { "en": "Apa Itu Imunitas Radiasi (Radiated Immunity)?", "id": "Ketahanan Terhadap Gangguan Gelombang EM." },
  { "en": "Apa Sumber Umum EMI (Electromagnetic Interference)?", "id": "Switching Power Supply, Motor, Radio." },
  { "en": "Teknik Mengurangi Emisi (Emission) EMI?", "id": "Filtering, Shielding, Grounding Yang Baik." },
  { "en": "Teknik Meningkatkan Imunitas (Immunity) EMI?", "id": "Filtering, Shielding, Layout PCB Hati-Hati." },
  { "en": "Apa Fungsi Filter EMI (Electromagnetic Interference)?", "id": "Menekan Noise Konduksi Frekuensi Tinggi." },
  { "en": "Komponen Apa Digunakan Filter EMI?", "id": "Kapasitor X, Kapasitor Y, Common Mode Choke." },
  { "en": "Apa Itu Kapasitor X (X Capacitor)?", "id": "Kapasitor Dipasang Antar Saluran (Line-Line)." },
  { "en": "Apa Itu Kapasitor Y (Y Capacitor)?", "id": "Kapasitor Dipasang Saluran Ke Ground." },
  { "en": "Apa Fungsi Kapasitor Y (Y Capacitor)?", "id": "Menekan Noise Mode Bersama (Common Mode)." },
  { "en": "Apa Itu Common Mode Choke (CMC)?", "id": "Induktor Menekan Noise Mode Bersama." },
  { "en": "Apa Itu Noise Mode Bersama (Common Mode)?", "id": "Noise Sama Arah Pada Dua Saluran." },
  { "en": "Apa Itu Noise Mode Diferensial (Differential Mode)?", "id": "Noise Beda Arah Antara Dua Saluran." },
  { "en": "Apa Itu Shielding (Perisaian) EMI?", "id": "Mengurung Sumber EMI Atau Melindungi Sirkuit." },
  { "en": "Bahan Apa Digunakan Shielding (Perisaian)?", "id": "Bahan Konduktif (Logam)." },
  { "en": "Apa Itu Gasket (Gasket) EMI?", "id": "Seal Konduktif Celah Sambungan Shield." },
  { "en": "Apa Itu Ferrite Bead (Manik Ferit)?", "id": "Komponen Pasif Redam Noise Frekuensi Tinggi." },
  { "en": "Bagaimana Ferrite Bead (Manik Ferit) Bekerja?", "id": "Bertindak Induktor Impedansi Tinggi Di HF." },
  { "en": "Apa Itu Grounding (Pembumian) Untuk EMC?", "id": "Menyediakan Jalur Impedansi Rendah Ke Referensi." },
  { "en": "Mengapa Layout PCB Penting EMC?", "id": "Loop Arus, Panjang Jalur Pengaruhi EMI." },
  { "en": "Apa Itu Loop Arus (Current Loop)?", "id": "Luas Area Jalur Arus Pergi Kembali." },
  { "en": "Loop Arus Kecil Atau Besar Lebih Baik EMC?", "id": "Loop Arus Kecil (Minimalkan Radiasi)." },
  { "en": "Apa Fungsi Ground Plane (Bidang Tanah) EMC?", "id": "Return Path Impedansi Rendah, Shielding." },
  { "en": "Apa Itu Standar EMC (Electromagnetic Compatibility)?", "id": "Regulasi Batas Emisi, Tingkat Imunitas." },
  { "en": "Contoh Badan Standar EMC (Electromagnetic Compatibility)?", "id": "FCC (Federal Communications Commission), CISPR (Comité International Spécial des Perturbations Radioélectriques), IEC (International Electrotechnical Commission)." },
  { "en": "Apa Itu Pengujian EMC (Electromagnetic Compatibility)?", "id": "Memverifikasi Kepatuhan Perangkat Standar EMC." },
  { "en": "Dimana Pengujian EMC (Electromagnetic Compatibility) Dilakukan?", "id": "Ruang Anechoic Atau Sel TEM." },
  { "en": "Apa Itu Ruang Anechoic (Anechoic Chamber)?", "id": "Ruangan Tanpa Gema Elektromagnetik." },
  { "en": "Apa Itu Sel TEM (Transverse Electromagnetic Cell)?", "id": "Struktur Pengujian EMC Skala Kecil." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis Cepat." },
  { "en": "Mengapa ESD (Electrostatic Discharge) Berbahaya Elektronik?", "id": "Dapat Merusak Komponen Sensitif." },
  { "en": "Bagaimana Pencegahan ESD (Electrostatic Discharge)?", "id": "Grounding Personil, Area Kerja EPA." },
  { "en": "Apa Itu Area EPA (ESD Protected Area)?", "id": "Area Kerja Dikontrol Mencegah ESD." },
  { "en": "Apa Itu Gelang Anti-Statis (Wrist Strap)?", "id": "Menghubungkan Tubuh Ke Ground." },
  { "en": "Apa Itu Alas Anti-Statis (Anti-Static Mat)?", "id": "Alas Kerja Disipatif Muatan Statis." },
  { "en": "Apa Itu Kantong Anti-Statis (Anti-Static Bag)?", "id": "Kemasan Melindungi Komponen Dari ESD." },
  { "en": "Apa Itu Model Tubuh Manusia (HBM)?", "id": "Human Body Model (HBM) (Simulasi ESD Orang)." },
  { "en": "Apa Itu Model Mesin (MM)?", "id": "Machine Model (MM) (Simulasi ESD Mesin)." },
  { "en": "Apa Itu Model Perangkat Bermuatan (CDM)?", "id": "Charged Device Model (CDM)." },
  { "en": "Komponen Apa Yang Sangat Sensitif ESD?", "id": "MOSFET, IC CMOS Modern." },
  { "en": "Apa Itu Proteksi ESD (ESD Protection) Pada IC?", "id": "Sirkuit Internal Melindungi Pin Dari ESD." },
  { "en": "Apa Itu Dioda TVS (Transient Voltage Suppressor)?", "id": "Komponen Proteksi Lonjakan Tegangan Cepat." },
  { "en": "Apa Fungsi Dioda TVS (Transient Voltage Suppressor)?", "id": "Menjepit (Clamp) Tegangan Berlebih ESD." },
  { "en": "Apa Itu Varistor (Varistor) (MOV)?", "id": "Resistor Tergantung Tegangan (Proteksi Surja)." },
  { "en": "Apa Itu GDT (Gas Discharge Tube)?", "id": "Tabung Pelepasan Gas (Proteksi Surja Kuat)." },
  { "en": "Apa Itu Surja (Surge) Petir?", "id": "Lonjakan Tegangan/Arus Akibat Sambaran Petir." },
  { "en": "Apa Itu SPD (Surge Protective Device)?", "id": "Perangkat Pelindung Surja." },
  { "en": "Apa Fungsi Utama SPD (Surge Protective Device)?", "id": "Mengalihkan Energi Surja Ke Ground." },
  { "en": "Apa Itu Keamanan Fungsional (Functional Safety)?", "id": "Bagian Keamanan Tergantung Fungsi Sistem." },
  { "en": "Standar Apa Terkait Keamanan Fungsional?", "id": "IEC 61508 (Industri Umum)." },
  { "en": "Apa Itu Tingkat Integritas Keselamatan (SIL)?", "id": "Ukuran Kinerja Fungsi Keselamatan." },
  { "en": "Apa Itu Analisis Bahaya Dan Risiko?", "id": "Identifikasi, Evaluasi Potensi Bahaya." },
  { "en": "Apa Itu Probabilitas Kegagalan Berbahaya (PFD)?", "id": "Probability of Failure on Demand (PFD)." },
  { "en": "Apa Kepanjangan PFD (Probability of Failure on Demand)?", "id": "Probabilitas Kegagalan Saat Dibutuhkan." },
  { "en": "Apa Itu Laju Kegagalan Berbahaya (PFH)?", "id": "Probability of Failure per Hour (PFH)." },
  { "en": "Apa Kepanjangan PFH (Probability of Failure per Hour)?", "id": "Probabilitas Kegagalan Per Jam." },
  { "en": "Apa Itu Diagnostik (Diagnostics) Keselamatan?", "id": "Kemampuan Sistem Deteksi Kegagalan Internal." },
  { "en": "Apa Itu Cakupan Diagnostik (Diagnostic Coverage)?", "id": "Persentase Kegagalan Dapat Dideteksi." },
  { "en": "Apa Itu Fraksi Kegagalan Aman (SFF)?", "id": "Safe Failure Fraction (SFF)." },
  { "en": "Apa Kepanjangan SFF (Safe Failure Fraction)?", "id": "Fraksi Kegagalan Aman." },
  { "en": "Apa Itu Toleransi Kesalahan Perangkat Keras (HFT)?", "id": "Hardware Fault Tolerance (HFT)." },
  { "en": "Apa Kepanjangan HFT (Hardware Fault Tolerance)?", "id": "Toleransi Kesalahan Perangkat Keras." },
  { "en": "HFT (Hardware Fault Tolerance) = 1 Berarti Apa?", "id": "Sistem Tahan Satu Kegagalan Hardware." },
  { "en": "Apa Itu Kegagalan Mode Bersama (Common Cause Failure)?", "id": "Satu Penyebab Gagal Multi Komponen." },
  { "en": "Apa Itu Siklus Hidup Keselamatan (Safety Lifecycle)?", "id": "Tahapan Desain, Operasi, Pemeliharaan SIS." },
  { "en": "Apa Itu Validasi Keselamatan (Safety Validation)?", "id": "Konfirmasi SIS Penuhi Spesifikasi Keselamatan." },
  { "en": "Apa Itu Penilaian Keselamatan (Safety Assessment)?", "id": "Evaluasi Independen Pencapaian SIL." },
  { "en": "Apa Itu RAMS (Reliability Availability Maintainability Safety)?", "id": "Aspek Penting Sistem Kritis." },
  { "en": "Apa Kepanjangan RAMS?", "id": "Keandalan, Ketersediaan, Kemudahan Pemeliharaan, Keselamatan." },
  { "en": "Apa Itu Teknik Linear (Linear Engineering)?", "id": "Aplikasi Konsep Linearitas Desain." },
  { "en": "Kapan Sistem Dapat Dianggap Linear?", "id": "Jika Berlaku Superposisi, Homogenitas." },
  { "en": "Apa Itu Homogenitas (Homogeneity)?", "id": "Scaling Input Menghasilkan Scaling Output." },
  { "en": "Apa Itu Linearitas (Linearity) Penguat?", "id": "Kemampuan Menguatkan Tanpa Distorsi." },
  { "en": "Apa Ukuran Non-Linearitas (Non-Linearity)?", "id": "Distorsi Harmonik (THD), Intermodulasi (IMD)." },
  { "en": "Apa Itu Titik Kompresi 1-dB (P1dB)?", "id": "Daya Output Saat Gain Turun 1 dB." },
  { "en": "Apa Itu Titik Intersep Orde Ketiga (IP3)?", "id": "Ukuran Teoritis Linearitas Orde Ketiga." },
  { "en": "IP3 (Third-Order Intercept Point) Lebih Tinggi Berarti Apa?", "id": "Linearitas Lebih Baik." },
  { "en": "Apa Itu Rentang Dinamis (Dynamic Range)?", "id": "Rasio Sinyal Terbesar Terhadap Terkecil." },
  { "en": "Apa Itu Rentang Dinamis Bebas Spurious (SFDR)?", "id": "Spurious Free Dynamic Range (SFDR)." },
  { "en": "Apa Kepanjangan SFDR (Spurious Free Dynamic Range)?", "id": "Rentang Dinamis Bebas Spurious." },
  { "en": "Apa Itu Derau Kuantisasi (Quantization Noise)?", "id": "Error Akibat Proses Kuantisasi ADC." },
  { "en": "Bagaimana SNR (Signal-to-Noise Ratio) ADC Terkait Resolusi?", "id": "SNR Meningkat Sekitar 6 dB Per Bit." },
  { "en": "Apa Itu Jumlah Bit Efektif (ENOB)?", "id": "Effective Number of Bits (ENOB)." },
  { "en": "Apa Kepanjangan ENOB (Effective Number of Bits)?", "id": "Jumlah Bit Efektif." },
  { "en": "Apa Makna ENOB (Effective Number of Bits)?", "id": "Resolusi Aktual ADC Memperhitungkan Noise, Distorsi." },
  { "en": "Apa Itu Dithering (Dithering) ADC?", "id": "Penambahan Noise Acak Tingkatkan Resolusi Efektif." },
  { "en": "Apa Itu Oversampling (Oversampling)?", "id": "Sampling Jauh Di Atas Laju Nyquist." },
  { "en": "Apa Manfaat Oversampling (Oversampling)?", "id": "Meningkatkan SNR, Memudahkan Filter Anti-Aliasing." },
  { "en": "Apa Itu Pembentukan Derau (Noise Shaping)?", "id": "Memindahkan Noise Kuantisasi Ke Frekuensi Tinggi." },
  { "en": "Teknik Apa Menggunakan Oversampling, Noise Shaping?", "id": "ADC Sigma-Delta." },
  { "en": "Apa Itu Penguat Sampel Tahan (Sample and Hold)?", "id": "Mencuplik, Menahan Tegangan Input ADC." },
  { "en": "Apa Itu Aperture Jitter (Jitter Apertur)?", "id": "Variasi Waktu Saat Pencuplikan." },
  { "en": "Efek Aperture Jitter (Jitter Apertur)?", "id": "Membatasi SNR Pengukuran Frekuensi Tinggi." },
  { "en": "Apa Itu Penguat Instrumentasi (Instrumentation Amplifier)?", "id": "Penguat Diferensial Presisi Tinggi." },
  { "en": "Apa Karakteristik Penguat Instrumentasi?", "id": "CMRR Tinggi, Impedansi Input Tinggi, Gain Akurat." },
  { "en": "Apa Itu Jembatan Wheatstone (Wheatstone Bridge)?", "id": "Rangkaian Ukur Resistansi Tidak Diketahui." },
  { "en": "Kapan Jembatan Wheatstone (Wheatstone Bridge) Seimbang?", "id": "Tegangan Output (Antar Titik Tengah) Nol." },
  { "en": "Aplikasi Jembatan Wheatstone (Wheatstone Bridge)?", "id": "Pengukuran Sensor Resistif (Strain Gauge)." },
  { "en": "Apa Itu Strain Gauge (Pengukur Regangan)?", "id": "Sensor Resistansi Berubah Akibat Regangan." },
  { "en": "Apa Itu Termokopel (Thermocouple)?", "id": "Sensor Suhu Dua Logam Berbeda." },
  { "en": "Apa Itu Sambungan Dingin (Cold Junction) Termokopel?", "id": "Sambungan Referensi Yang Perlu Dikompensasi." },
  { "en": "Apa Itu Efek Termoelektrik (Thermoelectric Effect)?", "id": "Konversi Langsung Panas Ke Listrik." },
  { "en": "Apa Tiga Efek Termoelektrik (Thermoelectric Effect) Utama?", "id": "Seebeck, Peltier, Dan Thomson." },
  { "en": "Apa Aplikasi Efek Peltier (Peltier Effect)?", "id": "Pendingin Termoelektrik (TEC)." },
  { "en": "Apa Itu Konduktivitas Termal (Thermal Conductivity)?", "id": "Kemampuan Bahan Hantarkan Panas." },
  { "en": "Apa Itu Resistansi Termal (Thermal Resistance)?", "id": "Hambatan Bahan Terhadap Aliran Panas." },
  { "en": "Satuan Apa Konduktivitas Termal (Thermal Conductivity)?", "id": "Watt Per Meter Kelvin (W/mK)." },
  { "en": "Satuan Apa Resistansi Termal (Thermal Resistance)?", "id": "Kelvin Per Watt (K/W)." },
  { "en": "Apa Itu Radiasi Benda Hitam (Blackbody Radiation)?", "id": "Radiasi EM Dipancarkan Benda Ideal." },
  { "en": "Apa Hukum Stefan-Boltzmann (Stefan-Boltzmann Law)?", "id": "Daya Radiasi Total Proporsional T⁴." },
  { "en": "Apa Hukum Pergeseran Wien (Wien's Displacement Law)?", "id": "Panjang Gelombang Puncak Invers Suhu." },
  { "en": "Apa Itu Emisivitas (Emissivity)?", "id": "Efektivitas Permukaan Memancarkan Radiasi Termal." },
  { "en": "Nilai Emisivitas (Emissivity) Benda Hitam?", "id": "Satu (ε = 1)." },
  { "en": "Apa Itu Pirometer (Pyrometer)?", "id": "Termometer Non-Kontak Ukur Radiasi." },
  { "en": "Apa Itu Kamera Termal (Thermal Camera)?", "id": "Kamera Visualisasi Distribusi Suhu (Inframerah)." },
  { "en": "Apa Itu Efek Rumah Kaca (Greenhouse Effect)?", "id": "Pemanasan Atmosfer Akibat Gas Tertentu." },
  { "en": "Apa Itu Arus Permukaan (Surface Current)?", "id": "Arus Mengalir Di Permukaan Konduktor." },
  { "en": "Apa Itu Kedalaman Kulit (Skin Depth)?", "id": "Kedalaman Penetrasi Arus AC Konduktor." },
  { "en": "Bagaimana Kedalaman Kulit (Skin Depth) Berubah Frekuensi?", "id": "Berkurang Seiring Kenaikan Frekuensi." },
  { "en": "Apa Itu Persamaan Gelombang Helmholtz (Helmholtz Equation)?", "id": "Persamaan Gelombang Domain Frekuensi." },
  { "en": "Apa Itu Bilangan Gelombang (Wave Number) (k)?", "id": "k = ω / v = 2π / λ." },
  { "en": "Apa Itu Impedansi Intrinsik (Intrinsic Impedance) Medium?", "id": "Rasio Medan E Terhadap H Gelombang." },
  { "en": "Impedansi Intrinsik (Intrinsic Impedance) Ruang Hampa?", "id": "Sekitar Tiga Ratus Tujuh Puluh Tujuh Ohm (377 Ω)." },
  { "en": "Apa Itu Medium Lossless (Tanpa Rugi)?", "id": "Medium Tanpa Atenuasi Gelombang EM." },
  { "en": "Apa Itu Medium Lossy (Berugi)?", "id": "Medium Menyebabkan Atenuasi Gelombang EM." },
  { "en": "Apa Itu Konstanta Atenuasi (Attenuation Constant) (α)?", "id": "Ukuran Pelemahan Amplitudo Gelombang." },
  { "en": "Apa Itu Konstanta Fasa (Phase Constant) (β)?", "id": "Ukuran Perubahan Fasa Gelombang." },
  { "en": "Apa Itu Kecepatan Fasa (Phase Velocity)?", "id": "Kecepatan Perambatan Titik Fasa Konstan." },
  { "en": "Apa Itu Kecepatan Grup (Group Velocity)?", "id": "Kecepatan Perambatan Paket Gelombang (Energi)." },
  { "en": "Kapan Kecepatan Fasa Sama Grup?", "id": "Dalam Medium Non-Dispersif." },
  { "en": "Apa Itu Medium Dispersif (Dispersive Medium)?", "id": "Kecepatan Gelombang Tergantung Frekuensi." },
  { "en": "Apa Itu Kondisi Batas (Boundary Condition) Konduktor Sempurna?", "id": "Medan Listrik Tangensial Nol." },
  { "en": "Apa Itu Metode Gambar (Method of Images)?", "id": "Teknik Solusi Masalah Elektrostatika Dekat Konduktor." },
  { "en": "Apa Itu Analogi Rangkaian Saluran Transmisi?", "id": "Model Saluran Pakai R, L, G, C." },
  { "en": "Apa Itu Parameter R Saluran Transmisi?", "id": "Resistansi Per Satuan Panjang." },
  { "en": "Apa Itu Parameter L Saluran Transmisi?", "id": "Induktansi Per Satuan Panjang." },
  { "en": "Apa Itu Parameter G Saluran Transmisi?", "id": "Konduktansi Shunt Per Satuan Panjang." },
  { "en": "Apa Itu Parameter C Saluran Transmisi?", "id": "Kapasitansi Shunt Per Satuan Panjang." },
  { "en": "Apa Persamaan Telegrapher (Telegrapher's Equations)?", "id": "Persamaan Diferensial Tegangan, Arus Saluran." },
  { "en": "Apa Itu Saluran Transmisi Tanpa Distorsi?", "id": "R/L Sama Dengan G/C." },
  { "en": "Apa Itu Beban Cocok (Matched Load)?", "id": "Impedansi Beban Sama Impedansi Karakteristik." },
  { "en": "Apa Koefisien Refleksi (Reflection Coefficient) Beban Cocok?", "id": "Nol (Tidak Ada Pantulan)." },
  { "en": "Apa Itu Trafo Lambda Perempat (Quarter-Wave Transformer)?", "id": "Saluran λ/4 Pencocokan Impedansi." },
  { "en": "Apa Rumus Impedansi Trafo Lambda Perempat?", "id": "Z_t = √(Z₀ * Z_L)." },
  { "en": "Apa Itu Stub (Stub) Tunggal?", "id": "Satu Saluran Pendek Paralel/Seri Matching." },
  { "en": "Apa Itu Stub (Stub) Ganda?", "id": "Dua Saluran Pendek Paralel Matching." },
  { "en": "Apa Itu Mode Pemandu Gelombang (Waveguide Mode)?", "id": "Pola Medan EM Dalam Pemandu." },
  { "en": "Apa Itu Mode Dominan (Dominant Mode)?", "id": "Mode Frekuensi Cutoff Terendah." },
  { "en": "Mode Dominan (Dominant Mode) Pemandu Persegi Panjang?", "id": "Mode TE₁₀." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency)?", "id": "Frekuensi Minimum Propagasi Mode." },
  { "en": "Apa Itu Resonator Rongga (Cavity Resonator)?", "id": "Rongga Logam Resonansi Gelombang Mikro." },
  { "en": "Apa Aplikasi Resonator Rongga (Cavity Resonator)?", "id": "Filter Microwave, Osilator Frekuensi Tinggi." },
  { "en": "Apa Itu Antena Celah (Slot Antenna)?", "id": "Antena Dibuat Celah Konduktor." },
  { "en": "Apa Itu Antena Tanduk (Horn Antenna)?", "id": "Antena Pemandu Gelombang Bentuk Corong." },
  { "en": "Apa Itu Antena Mikrostrip (Microstrip Antenna)?", "id": "Antena Dicetak Di Atas PCB." },
  { "en": "Nama Lain Antena Mikrostrip (Microstrip Antenna)?", "id": "Antena Patch (Patch Antenna)." },
  { "en": "Keunggulan Antena Patch (Patch Antenna)?", "id": "Ringan, Murah, Mudah Diintegrasikan." },
  { "en": "Apa Itu Antena Array (Antenna Array)?", "id": "Kumpulan Elemen Antena." },
  { "en": "Apa Itu Faktor Array (Array Factor)?", "id": "Kontribusi Pola Radiasi Akibat Susunan Array." },
  { "en": "Apa Itu Beam Steering (Pengarahan Berkas)?", "id": "Mengubah Arah Pancaran Utama Array." },
  { "en": "Bagaimana Beam Steering (Pengarahan Berkas) Dilakukan?", "id": "Mengatur Fasa Sinyal Tiap Elemen." },
  { "en": "Apa Itu Antena Cerdas (Smart Antenna)?", "id": "Antena Array Dengan Pemrosesan Sinyal." },
  { "en": "Apa Itu Sistem MIMO (Multiple Input Multiple Output)?", "id": "Banyak Antena Pemancar, Penerima." },
  { "en": "Apa Keuntungan MIMO (Multiple Input Multiple Output)?", "id": "Kapasitas Tinggi, Keandalan Lebih Baik." },
  { "en": "Apa Itu Multiplexing Spasial (Spatial Multiplexing)?", "id": "Mengirim Stream Data Berbeda Bersamaan." },
  { "en": "Apa Itu Keanekaragaman Spasial (Spatial Diversity)?", "id": "Mengirim Sinyal Sama Lewat Antena Berbeda." },
  { "en": "Apa Itu Radar (Radar)?", "id": "Sistem Deteksi Objek Gelombang Radio." },
  { "en": "Apa Itu Radar Pulsa (Pulse Radar)?", "id": "Radar Memancarkan Pulsa Pendek." },
  { "en": "Apa Itu Radar Gelombang Kontinu (CW)?", "id": "Continuous Wave (CW) Radar." },
  { "en": "Apa Kepanjangan CW (Continuous Wave)?", "id": "Gelombang Kontinu." },
  { "en": "Apa Fungsi Radar Doppler (Doppler Radar)?", "id": "Mengukur Kecepatan Objek (Efek Doppler)." },
  { "en": "Apa Itu Radar FMCW (Frequency Modulated Continuous Wave)?", "id": "Radar CW Frekuensi Dimodulasi." },
  { "en": "Apa Kepanjangan FMCW (Frequency Modulated Continuous Wave)?", "id": "Gelombang Kontinu Termodulasi Frekuensi." },
  { "en": "Apa Itu Radar Apertur Sintetis (SAR)?", "id": "Synthetic Aperture Radar (SAR)." },
  { "en": "Apa Kepanjangan SAR (Synthetic Aperture Radar)?", "id": "Radar Apertur Sintetis." },
  { "en": "Apa Fungsi SAR (Synthetic Aperture Radar)?", "id": "Pencitraan Resolusi Tinggi Permukaan Bumi." },
  { "en": "Apa Itu Penampang Lintang Radar (RCS)?", "id": "Radar Cross Section (RCS)." },
  { "en": "Apa Kepanjangan RCS (Radar Cross Section)?", "id": "Penampang Lintang Radar." },
  { "en": "Apa Itu RCS (Radar Cross Section)?", "id": "Ukuran Kemampuan Objek Pantulkan Sinyal Radar." },
  { "en": "Apa Itu Teknologi Siluman (Stealth Technology)?", "id": "Mengurangi Deteksi Radar (RCS Rendah)." },
  { "en": "Apa Itu Clutter (Clutter) Radar?", "id": "Gema Tak Diinginkan (Hujan, Tanah)." },
  { "en": "Apa Itu Chaff (Chaff) Radar?", "id": "Material Pantul Disebar Ganggu Radar." },
  { "en": "Apa Itu Peperangan Elektronik (EW)?", "id": "Electronic Warfare (EW)." },
  { "en": "Apa Kepanjangan EW (Electronic Warfare)?", "id": "Peperangan Elektronik." },
  { "en": "Apa Itu Pengacauan (Jamming) Radar/Komunikasi?", "id": "Memancarkan Sinyal Ganggu Penerima Musuh." },
  { "en": "Apa Itu Penipuan (Deception) Elektronik?", "id": "Memancarkan Sinyal Palsu Menipu Musuh." },
  { "en": "Apa Itu Dukungan Elektronik (ESM)?", "id": "Electronic Support Measures (ESM)." },
  { "en": "Apa Kepanjangan ESM (Electronic Support Measures)?", "id": "Tindakan Dukungan Elektronik." },
  { "en": "Apa Fungsi ESM (Electronic Support Measures)?", "id": "Mendeteksi, Mengidentifikasi Sinyal Elektronik Musuh." },
  { "en": "Apa Itu Komunikasi Optik (Optical Communication)?", "id": "Transmisi Informasi Menggunakan Cahaya." },
  { "en": "Apa Itu Fotodetektor (Photodetector)?", "id": "Sensor Mengubah Cahaya Jadi Sinyal Listrik." },
  { "en": "Apa Itu Modulasi Intensitas (Intensity Modulation)?", "id": "Mengubah Kecerahan Cahaya Sesuai Sinyal." },
  { "en": "Apa Itu Direct Detection (Deteksi Langsung)?", "id": "Fotodetektor Langsung Ukur Intensitas Cahaya." },
  { "en": "Apa Itu Deteksi Koheren (Coherent Detection)?", "id": "Deteksi Optik Menggunakan Osilator Lokal Optik." },
  { "en": "Apa Keuntungan Deteksi Koheren (Coherent Detection)?", "id": "Sensitivitas Lebih Tinggi." },
  { "en": "Apa Itu Jaringan Optik Pasif (PON)?", "id": "Passive Optical Network (PON)." },
  { "en": "Apa Kepanjangan PON (Passive Optical Network)?", "id": "Jaringan Optik Pasif." },
  { "en": "Apa Itu Splitter (Splitter) Optik?", "id": "Membagi Daya Cahaya Tanpa Komponen Aktif." },
  { "en": "Apa Itu Transmisi Optik Soliton (Soliton)?", "id": "Pulsa Cahaya Jaga Bentuk Jarak Jauh." },
  { "en": "Apa Itu Efek Non-Linear (Non-Linear Effect) Serat Optik?", "id": "Perilaku Non-Linear Cahaya Intensitas Tinggi." },
  { "en": "Contoh Efek Non-Linear (Non-Linear Effect)?", "id": "Self-Phase Modulation (SPM), Cross-Phase Modulation (XPM)." },
  { "en": "Apa Itu Dispersi Kromatik (Chromatic Dispersion)?", "id": "Pelebaran Pulsa Akibat Beda Kecepatan Lambda." },
  { "en": "Apa Itu Dispersi Mode Polarisasi (PMD)?", "id": "Polarization Mode Dispersion (PMD)." },
  { "en": "Apa Kepanjangan PMD (Polarization Mode Dispersion)?", "id": "Dispersi Mode Polarisasi." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
