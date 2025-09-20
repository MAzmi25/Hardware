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


  { "en": "Apa Itu Perangkat Keras (Hardware)?", "id": "Komponen Fisik Dari Sistem Komputer." },
  { "en": "Apa Lawan Dari Perangkat Keras?", "id": "Perangkat Lunak (Software)." },
  { "en": "Sebutkan Empat Komponen Utama Komputer?", "id": "CPU RAM Motherboard Dan Penyimpanan." },
  { "en": "Apa Kepanjangan Dari CPU?", "id": "Central Processing Unit." },
  { "en": "Apa Fungsi Utama CPU?", "id": "Sebagai Otak Dari Komputer." },
  { "en": "Apa Yang Dilakukan CPU?", "id": "Memproses Instruksi Dan Menjalankan Program." },
  { "en": "Apa Itu Core Pada CPU?", "id": "Unit Pemrosesan Individual Di Dalam CPU." },
  { "en": "Apa Keuntungan Banyak Core?", "id": "Dapat Mengerjakan Banyak Tugas Sekaligus." },
  { "en": "Apa Itu Clock Speed CPU?", "id": "Kecepatan CPU Dalam Menjalankan Instruksi." },
  { "en": "Apa Satuan Clock Speed?", "id": "Hertz (Hz) Gigahertz (GHz)." },
  { "en": "Merek CPU Apa Yang Paling Populer?", "id": "Intel Dan AMD." },
  { "en": "Apa Kepanjangan Dari RAM?", "id": "Random Access Memory." },
  { "en": "Apa Fungsi Utama RAM?", "id": "Menyimpan Data Sementara Untuk Akses Cepat." },
  { "en": "Apakah Data Di RAM Bersifat Volatil?", "id": "Ya Data Hilang Jika Listrik Mati." },
  { "en": "Apa Satuan Kapasitas RAM?", "id": "Gigabyte (GB)." },
  { "en": "Apa Beda RAM Dan Penyimpanan?", "id": "RAM Sementara Penyimpanan Permanen." },
  { "en": "Apa Itu Motherboard?", "id": "Papan Sirkuit Utama Yang Menghubungkan Komponen." },
  { "en": "Apa Nama Lain Motherboard?", "id": "Mainboard Atau Mobo." },
  { "en": "Apa Fungsi Motherboard?", "id": "Menghubungkan Semua Perangkat Keras Komputer." },
  { "en": "Apa Itu Chipset?", "id": "Kumpulan Sirkuit Terpadu Di Motherboard." },
  { "en": "Apa Itu BIOS?", "id": "Basic Input/Output System." },
  { "en": "Apa Fungsi BIOS?", "id": "Memulai Komputer Dan Menguji Perangkat Keras." },
  { "en": "Apa Pengganti BIOS Modern?", "id": "UEFI (Unified Extensible Firmware Interface)." },
  { "en": "Apa Itu Perangkat Penyimpanan?", "id": "Tempat Menyimpan Data Secara Permanen." },
  { "en": "Sebutkan Dua Jenis Penyimpanan Utama?", "id": "HDD Dan SSD." },
  { "en": "Apa Kepanjangan Dari HDD?", "id": "Hard Disk Drive." },
  { "en": "Bagaimana Cara Kerja HDD?", "id": "Menggunakan Piringan Magnetik Yang Berputar." },
  { "en": "Apa Kelebihan HDD?", "id": "Kapasitas Besar Dengan Harga Murah." },
  { "en": "Apa Kepanjangan Dari SSD?", "id": "Solid State Drive." },
  { "en": "Bagaimana Cara Kerja SSD?", "id": "Menggunakan Chip Memori Flash Tanpa Bagian Bergerak." },
  { "en": "Apa Kelebihan SSD?", "id": "Jauh Lebih Cepat Dari HDD." },
  { "en": "Manakah Yang Lebih Tahan Guncangan?", "id": "SSD Karena Tidak Ada Bagian Bergerak." },
  { "en": "Apa Kepanjangan Dari GPU?", "id": "Graphics Processing Unit." },
  { "en": "Apa Fungsi Utama GPU?", "id": "Memproses Dan Merender Grafis Atau Gambar." },
  { "en": "Apa Nama Lain GPU?", "id": "Kartu Grafis Atau Kartu Video." },
  { "en": "Apa Itu GPU Terintegrasi (Integrated)?", "id": "GPU Yang Tertanam Di Dalam CPU." },
  { "en": "Apa Itu GPU Diskrit (Dedicated)?", "id": "Kartu Grafis Terpisah Yang Lebih Kuat." },
  { "en": "Kapan GPU Diskrit Dibutuhkan?", "id": "Untuk Bermain Game Dan Desain Grafis." },
  { "en": "Merek GPU Apa Yang Paling Populer?", "id": "NVIDIA Dan AMD." },
  { "en": "Apa Kepanjangan Dari PSU?", "id": "Power Supply Unit." },
  { "en": "Apa Fungsi PSU?", "id": "Menyuplai Daya Listrik Ke Semua Komponen." },
  { "en": "Listrik Apa Yang Diubah Oleh PSU?", "id": "Mengubah Listrik AC Dari Dinding Ke DC." },
  { "en": "Apa Satuan Daya PSU?", "id": "Watt (W)." },
  { "en": "Apa Itu Perangkat Input?", "id": "Perangkat Untuk Memasukkan Data Ke Komputer." },
  { "en": "Sebutkan Tiga Perangkat Input Utama?", "id": "Keyboard Mouse Dan Mikrofon." },
  { "en": "Apa Itu Perangkat Output?", "id": "Perangkat Untuk Menampilkan Hasil Dari Komputer." },
  { "en": "Sebutkan Tiga Perangkat Output Utama?", "id": "Monitor Printer Dan Speaker." },
  { "en": "Apa Itu Monitor?", "id": "Layar Untuk Menampilkan Output Visual." },
  { "en": "Apa Itu Keyboard?", "id": "Papan Ketik Untuk Memasukkan Teks." },
  { "en": "Apa Itu Mouse?", "id": "Perangkat Penunjuk Untuk Mengontrol Kursor." },
  { "en": "Apa Itu Casing Komputer?", "id": "Kotak Yang Melindungi Semua Komponen Internal." },
  { "en": "Apa Fungsi Lain Casing?", "id": "Membantu Aliran Udara Untuk Pendinginan." },
  { "en": "Apa Itu Kipas Pendingin (Cooling Fan)?", "id": "Kipas Untuk Mendinginkan Komponen Komputer." },
  { "en": "Komponen Apa Yang Paling Membutuhkan Pendingin?", "id": "CPU Dan GPU." },
  { "en": "Apa Itu Heatsink?", "id": "Logam Beralur Untuk Menyerap Panas." },
  { "en": "Bagaimana Cara Kerja Heatsink?", "id": "Menyebarkan Panas Ke Udara Sekitar." },
  { "en": "Apa Itu Thermal Paste?", "id": "Pasta Penghantar Panas Antara CPU Dan Heatsink." },
  { "en": "Apa Itu Port?", "id": "Konektor Fisik Untuk Menghubungkan Perangkat Eksternal." },
  { "en": "Apa Kepanjangan Dari USB?", "id": "Universal Serial Bus." },
  { "en": "Apa Fungsi Port USB?", "id": "Menghubungkan Berbagai Macam Perangkat." },
  { "en": "Port Apa Yang Digunakan Untuk Menghubungkan Monitor?", "id": "HDMI DisplayPort Atau VGA." },
  { "en": "Apa Kepanjangan HDMI?", "id": "High-Definition Multimedia Interface." },
  { "en": "Port Apa Yang Digunakan Untuk Jaringan Kabel?", "id": "Port Ethernet Atau RJ-45." },
  { "en": "Apa Itu Periferal (Peripheral)?", "id": "Perangkat Tambahan Yang Terhubung Ke Komputer." },
  { "en": "Apa Itu Webcam?", "id": "Kamera Video Untuk Komputer." },
  { "en": "Apa Itu Printer?", "id": "Mencetak Dokumen Digital Ke Media Fisik." },
  { "en": "Apa Itu Scanner?", "id": "Memindai Dokumen Fisik Menjadi File Digital." },
  { "en": "Apa Itu Firmware?", "id": "Perangkat Lunak Permanen Yang Tertanam Di Perangkat Keras." },
  { "en": "Di Mana Firmware Ditemukan?", "id": "Di BIOS Motherboard Atau SSD." },
  { "en": "Apa Itu Driver?", "id": "Perangkat Lunak Yang Mengontrol Perangkat Keras." },
  { "en": "Mengapa Driver Diperlukan?", "id": "Agar Sistem Operasi Mengenali Perangkat Keras." },
  { "en": "Apa Itu Bus Dalam Komputer?", "id": "Jalur Komunikasi Untuk Transfer Data." },
  { "en": "Apa Itu Slot Ekspansi?", "id": "Slot Di Motherboard Untuk Menambah Kartu." },
  { "en": "Contoh Kartu Ekspansi?", "id": "Kartu Grafis Kartu Suara Kartu Jaringan." },
  { "en": "Apa Itu Slot PCIe?", "id": "Peripheral Component Interconnect Express." },
  { "en": "Slot Mana Yang Paling Cepat Untuk GPU?", "id": "Slot PCIe x16." },
  { "en": "Apa Itu SATA?", "id": "Serial AT Attachment." },
  { "en": "Perangkat Apa Yang Menggunakan Konektor SATA?", "id": "HDD SSD Dan Optical Drive." },
  { "en": "Apa Itu Optical Drive?", "id": "Drive Untuk Membaca Atau Menulis CD DVD." },
  { "en": "Apakah Optical Drive Masih Umum?", "id": "Sudah Jarang Ditemukan Di Komputer Modern." },
  { "en": "Apa Itu Memori Cache?", "id": "Memori Sangat Cepat Di Dalam CPU." },
  { "en": "Apa Fungsi Cache CPU?", "id": "Menyimpan Data Yang Sering Diakses CPU." },
  { "en": "Manakah Yang Paling Cepat L1 L2 Atau L3 Cache?", "id": "L1 Cache Adalah Yang Paling Cepat." },
  { "en": "Apa Itu Bit?", "id": "Unit Data Terkecil Dalam Komputasi." },
  { "en": "Berapa Nilai Sebuah Bit?", "id": "Nol Atau Satu." },
  { "en": "Apa Itu Byte?", "id": "Sekelompok Delapan Bit." },
  { "en": "Berapa Kilobyte Dalam Megabyte?", "id": "Sekitar Seribu Kilobyte." },
  { "en": "Apa Itu Kartu Suara (Sound Card)?", "id": "Memproses Dan Menghasilkan Audio." },
  { "en": "Apakah Motherboard Modern Punya Kartu Suara?", "id": "Ya Sudah Terintegrasi (Onboard)." },
  { "en": "Apa Itu Kartu Jaringan (Network Card)?", "id": "Memungkinkan Komputer Terhubung Ke Jaringan." },
  { "en": "Apa Itu Wi-Fi?", "id": "Teknologi Jaringan Nirkabel." },
  { "en": "Apa Itu Bluetooth?", "id": "Teknologi Nirkabel Jarak Pendek." },
  { "en": "Apa Perbedaan Antara Wi-Fi Dan Bluetooth?", "id": "Wi-Fi Untuk Internet Bluetooth Untuk Perangkat." },
  { "en": "Apa Itu Form Factor?", "id": "Ukuran Dan Bentuk Standar Perangkat Keras." },
  { "en": "Contoh Form Factor Motherboard?", "id": "ATX Micro-ATX Mini-ITX." },
  { "en": "Manakah Form Factor Yang Paling Besar?", "id": "ATX." },
  { "en": "Apa Itu Laptop?", "id": "Komputer Portabel Yang Terintegrasi." },
  { "en": "Apa Itu PC Desktop?", "id": "Komputer Pribadi Untuk Diletakkan Di Meja." },
  { "en": "Apa Beda USB 2.0 Dan 3.0?", "id": "USB 3.0 Jauh Lebih Cepat." },
  { "en": "Apa Warna Port USB 3.0?", "id": "Biasanya Berwarna Biru." },
  { "en": "Apa Kelebihan USB Type-C?", "id": "Reversibel Dan Mendukung Banyak Protokol." },
  { "en": "Port Audio Apa Yang Paling Umum?", "id": "Jack Audio 3.5mm." },
  { "en": "Apa Warna Port Mikrofon?", "id": "Biasanya Berwarna Merah Muda (Pink)." },
  { "en": "Apa Warna Port Speaker?", "id": "Biasanya Berwarna Hijau (Lime)." },
  { "en": "Port Display Mana Yang Kualitasnya Analog?", "id": "VGA (Video Graphics Array)." },
  { "en": "Port Display Mana Yang Sudah Digital?", "id": "HDMI Dan DisplayPort." },
  { "en": "Manakah Yang Lebih Canggih HDMI Atau DisplayPort?", "id": "DisplayPort Umumnya Mendukung Refresh Rate Tinggi." },
  { "en": "Apa Itu RAM DDR4?", "id": "Generasi Keempat Dari Memori DDR SDRAM." },
  { "en": "Apa Perbedaan Fisik RAM DDR3 Dan DDR4?", "id": "Posisi Celah (Notch) Yang Berbeda." },
  { "en": "Apa Itu Mode Dual-Channel RAM?", "id": "Menggunakan Dua Keping RAM Untuk Kinerja Ganda." },
  { "en": "Apa Syarat Untuk Mode Dual-Channel?", "id": "Memasang RAM Di Slot Dengan Warna Sama." },
  { "en": "Apa Itu RAM SODIMM?", "id": "RAM Berukuran Kecil Untuk Laptop." },
  { "en": "Apa Itu SSD M.2?", "id": "SSD Dengan Form Factor Kecil." },
  { "en": "Apa Keuntungan SSD M.2?", "id": "Ukuran Kompak Dan Kecepatan Sangat Tinggi." },
  { "en": "Apa Itu NVMe?", "id": "Non-Volatile Memory Express." },
  { "en": "Apa Hubungan NVMe Dengan M.2?", "id": "NVMe Adalah Protokol Cepat Yang Digunakan SSD M.2." },
  { "en": "Apa Itu RPM Pada HDD?", "id": "Rotations Per Minute (Putaran Per Menit)." },
  { "en": "Semakin Tinggi RPM HDD Berarti Apa?", "id": "Semakin Cepat Kinerja Baca Tulisnya." },
  { "en": "Apa Fungsi Baterai CMOS Di Motherboard?", "id": "Menyimpan Pengaturan Waktu Dan BIOS." },
  { "en": "Apa Tanda Baterai CMOS Lemah?", "id": "Pengaturan Waktu Dan Tanggal Selalu Salah." },
  { "en": "Apa Itu Soket CPU?", "id": "Tempat Memasang CPU Di Motherboard." },
  { "en": "Sebutkan Dua Tipe Soket CPU?", "id": "LGA (Intel) Dan PGA (AMD)." },
  { "en": "Apa Kepanjangan LGA?", "id": "Land Grid Array." },
  { "en": "Apa Itu Pendingin Cair AIO?", "id": "All-In-One Liquid Cooling." },
  { "en": "Apa Komponen Utama Pendingin Cair AIO?", "id": "Radiator Pompa Dan Water Block." },
  { "en": "Manakah Yang Umumnya Lebih Baik Air Atau Liquid Cooling?", "id": "Liquid Cooling Menawarkan Performa Lebih Tinggi." },
  { "en": "Apa Itu Resolusi Monitor?", "id": "Jumlah Piksel Yang Ditampilkan Layar." },
  { "en": "Apa Nama Umum Resolusi 1920x1080?", "id": "Full HD Atau 1080p." },
  { "en": "Apa Nama Umum Resolusi 3840x2160?", "id": "4K Atau Ultra HD." },
  { "en": "Apa Itu Refresh Rate Monitor?", "id": "Seberapa Sering Gambar Diperbarui Per Detik." },
  { "en": "Apa Satuan Refresh Rate?", "id": "Hertz (Hz)." },
  { "en": "Refresh Rate Berapa Yang Baik Untuk Gaming?", "id": "120 Hz Atau Lebih Tinggi." },
  { "en": "Apa Itu Panel Monitor IPS?", "id": "In-Plane Switching." },
  { "en": "Apa Kelebihan Panel IPS?", "id": "Warna Akurat Dan Sudut Pandang Luas." },
  { "en": "Apa Beda Keyboard Mekanikal Dan Membran?", "id": "Mekanikal Menggunakan Switch Fisik Per Tombol." },
  { "en": "Apa Kelebihan Keyboard Mekanikal?", "id": "Lebih Tahan Lama Dan Memberi Umpan Balik." },
  { "en": "Apa Itu DPI Pada Mouse?", "id": "Dots Per Inch." },
  { "en": "Apa Pengaruh DPI Pada Mouse?", "id": "Mengatur Sensitivitas Gerakan Kursor." },
  { "en": "Apa Itu Server?", "id": "Komputer Kuat Untuk Melayani Jaringan." },
  { "en": "Apa Beda PC Dan Server?", "id": "Server Dirancang Untuk Bekerja 24/7." },
  { "en": "Apa Itu Workstation?", "id": "PC Sangat Kuat Untuk Tugas Profesional." },
  { "en": "Apa Itu Mainframe?", "id": "Komputer Sangat Besar Untuk Pemrosesan Data Massal." },
  { "en": "Apa Itu VRAM?", "id": "Video RAM." },
  { "en": "Apa Fungsi VRAM?", "id": "Memori Khusus Untuk Kartu Grafis." },
  { "en": "Apa Itu Antarmuka (Interface)?", "id": "Titik Koneksi Antara Dua Komponen." },
  { "en": "Apa Itu Bandwidth?", "id": "Kapasitas Transfer Data Maksimal." },
  { "en": "Apa Itu Laten (Latency)?", "id": "Waktu Tunda Dalam Transfer Data." },
  { "en": "Mana Yang Lebih Baik Bandwidth Tinggi Atau Latensi Rendah?", "id": "Keduanya Penting Tergantung Aplikasi." },
  { "en": "Apa Itu Northbridge Pada Motherboard Lama?", "id": "Menghubungkan CPU RAM Dan GPU." },
  { "en": "Apa Itu Southbridge Pada Motherboard Lama?", "id": "Menghubungkan Perangkat Input Output." },
  { "en": "Di Mana Fungsi Northbridge Sekarang?", "id": "Sudah Terintegrasi Di Dalam CPU." },
  { "en": "Apa Itu VRM?", "id": "Voltage Regulator Module." },
  { "en": "Apa Fungsi VRM Di Motherboard?", "id": "Menstabilkan Dan Menyuplai Tegangan Ke CPU." },
  { "en": "Apa Itu Heatsink VRM?", "id": "Pendingin Khusus Untuk Komponen VRM." },
  { "en": "Apa Itu Port PS/2?", "id": "Port Lama Untuk Keyboard Dan Mouse." },
  { "en": "Apa Warna Port PS/2 Untuk Keyboard?", "id": "Ungu." },
  { "en": "Apa Warna Port PS/2 Untuk Mouse?", "id": "Hijau." },
  { "en": "Apa Itu Port Serial?", "id": "Port Lama Untuk Komunikasi Serial." },
  { "en": "Apa Itu Port Paralel?", "id": "Port Lama Yang Umumnya Untuk Printer." },
  { "en": "Apa Itu Touchpad?", "id": "Pengganti Mouse Pada Laptop." },
  { "en": "Apa Itu Trackpoint?", "id": "Titik Merah Kecil Di Keyboard Laptop." },
  { "en": "Apa Itu Choke Pada Perangkat Elektronik?", "id": "Komponen Untuk Menekan Noise Frekuensi Tinggi." },
  { "en": "Apa Itu Kapasitor?", "id": "Komponen Untuk Menyimpan Energi Listrik." },
  { "en": "Apa Fungsi Kapasitor Pada Motherboard?", "id": "Menstabilkan Aliran Daya Listrik." },
  { "en": "Apa Itu Overclocking?", "id": "Menjalankan Komponen Lebih Cepat Dari Standar." },
  { "en": "Komponen Apa Yang Biasa Di-Overclock?", "id": "CPU GPU Dan RAM." },
  { "en": "Apa Risiko Overclocking?", "id": "Panas Berlebih Dan Kerusakan Komponen." },
  { "en": "Apa Itu Bottleneck?", "id": "Komponen Lambat Yang Menghambat Kinerja Sistem." },
  { "en": "Contoh Bottleneck?", "id": "CPU Lambat Dengan GPU Yang Sangat Cepat." },
  { "en": "Apa Itu Benchmarking?", "id": "Menguji Dan Mengukur Kinerja Perangkat Keras." },
  { "en": "Apa Itu FPS Dalam Game?", "id": "Frames Per Second." },
  { "en": "Apa Yang Diukur Oleh FPS?", "id": "Kelancaran Tampilan Gambar Game." },
  { "en": "Perangkat Keras Apa Yang Paling Mempengaruhi FPS?", "id": "GPU (Kartu Grafis)." },
  { "en": "Apa Itu Komputer Rakitan (Custom PC)?", "id": "Komputer Yang Dibangun Dari Komponen Pilihan." },
  { "en": "Apa Itu Komputer Branded (Pre-built)?", "id": "Komputer Siap Pakai Dari Merek Tertentu." },
  { "en": "Manakah Yang Lebih Fleksibel Rakitan Atau Branded?", "id": "Komputer Rakitan Jauh Lebih Fleksibel." },
  { "en": "Apa Itu Sistem Operasi (OS)?", "id": "Perangkat Lunak Utama Pengelola Perangkat Keras." },
  { "en": "Sebutkan Tiga Sistem Operasi Populer?", "id": "Windows MacOS Dan Linux." },
  { "en": "Apa Itu Arsitektur CPU?", "id": "Desain Fundamental Dari Sebuah CPU." },
  { "en": "Sebutkan Dua Arsitektur CPU Populer?", "id": "x86 Dan ARM." },
  { "en": "Arsitektur Mana Yang Umum Di PC Desktop?", "id": "x86 (Termasuk x86-64)." },
  { "en": "Arsitektur Mana Yang Umum Di Smartphone?", "id": "ARM." },
  { "en": "Apa Itu Thunderbolt?", "id": "Antarmuka Super Cepat Dari Intel." },
  { "en": "Port Fisik Apa Yang Digunakan Thunderbolt?", "id": "Menggunakan Port USB Type-C." },
  { "en": "Apa Itu RAID?", "id": "Redundant Array of Independent Disks." },
  { "en": "Apa Fungsi RAID?", "id": "Menggabungkan Beberapa Drive Untuk Kinerja Atau Redundansi." },
  { "en": "Apa Itu RAID 0?", "id": "Menggabungkan Drive Untuk Kecepatan Maksimal." },
  { "en": "Apa Itu RAID 1?", "id": "Mencerminkan Data Antar Drive Untuk Keamanan." },
  { "en": "Apa Itu Hot Swapping?", "id": "Mengganti Komponen Saat Komputer Menyala." },
  { "en": "Komponen Apa Yang Biasanya Bisa Di-Hot Swap?", "id": "Hard Drive Pada Server." },
  { "en": "Apa Itu Port eSATA?", "id": "Port SATA Eksternal Untuk Drive." },
  { "en": "Apa Itu Sistem Pendingin Pasif?", "id": "Pendinginan Tanpa Menggunakan Kipas." },
  { "en": "Di Mana Pendingin Pasif Digunakan?", "id": "Pada Komponen Berdaya Rendah." },
  { "en": "Apa Itu Resolusi Layar?", "id": "Jumlah Piksel Horizontal Dan Vertikal." },
  { "en": "Apa Itu Piksel?", "id": "Titik Terkecil Yang Membentuk Gambar Digital." },
  { "en": "Apa Itu Kerapatan Piksel (Pixel Density)?", "id": "Jumlah Piksel Per Inci Layar." },
  { "en": "Apa Satuan Kerapatan Piksel?", "id": "PPI (Pixels Per Inch)." },
  { "en": "Apa Pengaruh PPI Tinggi?", "id": "Gambar Terlihat Lebih Tajam Dan Jelas." },
  { "en": "Apa Itu Aspek Rasio (Aspect Ratio)?", "id": "Rasio Perbandingan Lebar Dan Tinggi Layar." },
  { "en": "Berapa Aspek Rasio Layar Lebar (Widescreen)?", "id": "16:9." },
  { "en": "Apa Itu Waktu Respons (Response Time)?", "id": "Kecepatan Piksel Berubah Warna." },
  { "en": "Apa Satuan Waktu Respons?", "id": "Milidetik (Ms)." },
  { "en": "Mengapa Waktu Respons Cepat Penting?", "id": "Mengurangi Efek Ghosting Atau Buram Gerak." },
  { "en": "Apa Itu Panel Monitor TN?", "id": "Twisted Nematic." },
  { "en": "Apa Kelebihan Panel TN?", "id": "Waktu Respons Sangat Cepat." },
  { "en": "Apa Kekurangan Panel TN?", "id": "Warna Dan Sudut Pandang Kurang Baik." },
  { "en": "Apa Itu Panel Monitor VA?", "id": "Vertical Alignment." },
  { "en": "Apa Kelebihan Panel VA?", "id": "Kontras Sangat Tinggi Warna Hitam Pekat." },
  { "en": "Apa Itu Kontras Rasio?", "id": "Perbandingan Antara Warna Paling Terang Dan Gelap." },
  { "en": "Apa Itu Switch Keyboard Mekanikal?", "id": "Mekanisme Di Bawah Setiap Tombol." },
  { "en": "Sebutkan Tiga Merek Switch Populer?", "id": "Cherry MX Gateron Dan Kailh." },
  { "en": "Apa Karakteristik Switch Biru (Blue)?", "id": "Tactile Dan Clicky (Berbunyi Klik)." },
  { "en": "Apa Karakteristik Switch Cokelat (Brown)?", "id": "Tactile Tapi Tidak Berisik." },
  { "en": "Apa Karakteristik Switch Merah (Red)?", "id": "Linear Dan Halus Tanpa Benjolan." },
  { "en": "Apa Itu Polling Rate Mouse?", "id": "Seberapa Sering Mouse Melaporkan Posisinya." },
  { "en": "Apa Satuan Polling Rate?", "id": "Hertz (Hz)." },
  { "en": "Apa Itu Thin Client?", "id": "Komputer Sederhana Yang Bergantung Pada Server." },
  { "en": "Apa Itu Single-Board Computer (SBC)?", "id": "Komputer Lengkap Dalam Satu Papan Sirkuit." },
  { "en": "Apa Contoh SBC Yang Paling Populer?", "id": "Raspberry Pi." },
  { "en": "Apa Itu Sistem Pendingin Udara (Air Cooling)?", "id": "Menggunakan Heatsink Dan Kipas." },
  { "en": "Apa Itu Menara Pendingin CPU (Tower Cooler)?", "id": "Heatsink Besar Dengan Pipa Panas Vertikal." },
  { "en": "Apa Itu Pipa Panas (Heat Pipe)?", "id": "Pipa Tembaga Berisi Cairan Untuk Transfer Panas." },
  { "en": "Apa Itu Pendingin Cair Kustom (Custom Loop)?", "id": "Sistem Pendingin Air Yang Dirakit Sendiri." },
  { "en": "Manakah Yang Paling Rumit Dipasang?", "id": "Pendingin Cair Kustom." },
  { "en": "Apa Itu Aliran Udara (Airflow) Casing?", "id": "Arah Pergerakan Udara Di Dalam Casing." },
  { "en": "Bagaimana Aliran Udara Yang Baik?", "id": "Udara Dingin Masuk Dari Depan Keluar Dari Belakang." },
  { "en": "Apa Itu Tekanan Udara Positif?", "id": "Lebih Banyak Kipas Masuk Daripada Keluar." },
  { "en": "Apa Itu Tekanan Udara Negatif?", "id": "Lebih Banyak Kipas Keluar Daripada Masuk." },
  { "en": "Manakah Yang Lebih Baik Untuk Mencegah Debu?", "id": "Tekanan Udara Positif Dengan Filter." },
  { "en": "Apa Itu Filter Debu (Dust Filter)?", "id": "Saringan Untuk Mencegah Debu Masuk Casing." },
  { "en": "Apa Itu Kabel Manajemen (Cable Management)?", "id": "Praktik Merapikan Kabel Di Dalam Casing." },
  { "en": "Mengapa Kabel Manajemen Penting?", "id": "Memperbaiki Aliran Udara Dan Estetika." },
  { "en": "Apa Itu PSU Modular?", "id": "PSU Dengan Kabel Yang Bisa Dilepas." },
  { "en": "Apa Keuntungan PSU Modular?", "id": "Hanya Menggunakan Kabel Yang Diperlukan." },
  { "en": "Apa Itu PSU Non-Modular?", "id": "Semua Kabel Terpasang Secara Permanen." },
  { "en": "Apa Itu Sertifikasi 80 Plus?", "id": "Standar Efisiensi Untuk Power Supply." },
  { "en": "Tingkatan Apa Yang Paling Efisien?", "id": "80 Plus Titanium." },
  { "en": "Mengapa Efisiensi PSU Penting?", "id": "Mengurangi Panas Dan Menghemat Listrik." },
  { "en": "Apa Itu Jumper?", "id": "Konektor Kecil Untuk Mengubah Pengaturan Papan Sirkuit." },
  { "en": "Apa Fungsi Jumper Clear CMOS?", "id": "Mereset Pengaturan BIOS Ke Default." },
  { "en": "Apa Itu Front Panel Header?", "id": "Pin Di Motherboard Untuk Kabel Casing." },
  { "en": "Kabel Apa Saja Yang Terhubung Ke Front Panel Header?", "id": "Tombol Power Reset Lampu LED." },
  { "en": "Apa Itu POST?", "id": "Power-On Self-Test." },
  { "en": "Apa Yang Dilakukan Komputer Saat POST?", "id": "Memeriksa Komponen Perangkat Keras Dasar." },
  { "en": "Apa Itu Kode Beep (Beep Code)?", "id": "Sinyal Suara Untuk Menunjukkan Masalah Saat POST." },
  { "en": "Apa Itu Sistem on a Chip (SoC)?", "id": "Mengintegrasikan Banyak Komponen Ke Satu Chip." },
  { "en": "Di Mana SoC Umum Ditemukan?", "id": "Di Smartphone Dan Tablet." },
  { "en": "Apa Itu IPC (Instructions Per Clock)?", "id": "Jumlah Instruksi Yang Dijalankan CPU Per Siklus." },
  { "en": "Manakah Yang Lebih Penting Clock Speed Atau IPC?", "id": "Kombinasi Keduanya Menentukan Kinerja." },
  { "en": "Apa Itu Thread Pada CPU?", "id": "Urutan Instruksi Yang Dikelola Core." },
  { "en": "Apa Itu Hyper-Threading?", "id": "Teknologi Intel Untuk Menjalankan Dua Thread Per Core." },
  { "en": "Apa Itu Cache Hit?", "id": "Saat Data Yang Dicari Ditemukan Di Cache." },
  { "en": "Apa Itu Cache Miss?", "id": "Saat Data Tidak Ditemukan Dan Harus Mengambil Dari RAM." },
  { "en": "Apa Itu APU (Accelerated Processing Unit)?", "id": "Istilah AMD Untuk CPU Dengan Grafis Terintegrasi." },
  { "en": "Apa Itu Chipset Driver?", "id": "Perangkat Lunak Untuk Komunikasi Komponen Motherboard." },
  { "en": "Apa Itu Flashing BIOS?", "id": "Proses Memperbarui Firmware BIOS Motherboard." },
  { "en": "Mengapa Perlu Memperbarui BIOS?", "id": "Mendukung CPU Baru Dan Memperbaiki Bug." },
  { "en": "Apa Itu Dual BIOS?", "id": "Motherboard Dengan Dua Chip BIOS Cadangan." },
  { "en": "Apa Itu RTC (Real-Time Clock)?", "id": "Chip Yang Menjaga Waktu Sistem." },
  { "en": "Daya Apa Yang Digunakan RTC?", "id": "Ditenagai Oleh Baterai CMOS." },
  { "en": "Apa Itu Komputer Barebone?", "id": "Kit Komputer Setengah Jadi." },
  { "en": "Apa Isi Komputer Barebone?", "id": "Biasanya Casing Motherboard Dan PSU." },
  { "en": "Apa Itu Small Form Factor (SFF) PC?", "id": "PC Dengan Ukuran Casing Sangat Kecil." },
  { "en": "Apa Itu I/O Shield?", "id": "Pelat Logam Pelindung Port Belakang Motherboard." },
  { "en": "Apa Itu Standoff Motherboard?", "id": "Sekrup Pengganjal Antara Motherboard Dan Casing." },
  { "en": "Mengapa Standoff Diperlukan?", "id": "Mencegah Hubung Singkat Motherboard Dengan Casing." },
  { "en": "Apa Itu Breadboard?", "id": "Papan Untuk Membuat Prototipe Sirkuit Elektronik." },
  { "en": "Apa Itu Jaringan Komputer?", "id": "Dua Atau Lebih Komputer Yang Terhubung." },
  { "en": "Apa Itu LAN?", "id": "Local Area Network." },
  { "en": "Apa Itu Router?", "id": "Perangkat Yang Menghubungkan Jaringan Berbeda." },
  { "en": "Apa Itu Switch Jaringan?", "id": "Menghubungkan Perangkat Dalam Jaringan Lokal." },
  { "en": "Apa Beda Router Dan Switch?", "id": "Router Antar Jaringan Switch Dalam Jaringan." },
  { "en": "Apa Itu Modem?", "id": "Modulator-Demodulator." },
  { "en": "Apa Fungsi Modem?", "id": "Menghubungkan Jaringan Rumah Ke Internet." },
  { "en": "Apa Itu Alamat IP?", "id": "Alamat Unik Perangkat Di Jaringan." },
  { "en": "Apa Itu Alamat MAC?", "id": "Alamat Fisik Unik Kartu Jaringan." },
  { "en": "Apa Beda Alamat IP Dan MAC?", "id": "IP Logis Dan Bisa Berubah MAC Fisik." },
  { "en": "Apa Itu NAS (Network Attached Storage)?", "id": "Perangkat Penyimpanan Yang Terhubung Ke Jaringan." },
  { "en": "Apa Itu Hot Plug?", "id": "Menghubungkan Perangkat Tanpa Mematikan Sistem." },
  { "en": "Apakah USB Mendukung Hot Plug?", "id": "Ya USB Dirancang Untuk Hot Plugging." },
  { "en": "Apa Itu KVM Switch?", "id": "Mengontrol Beberapa Komputer Dengan Satu Set Keyboard Monitor Mouse." },
  { "en": "Apa Itu Docking Station?", "id": "Memperluas Port Laptop Dengan Satu Koneksi." },
  { "en": "Apa Itu Power Brick?", "id": "Adaptor Daya Eksternal Untuk Laptop." },
  { "en": "Apa Itu Touchscreen?", "id": "Layar Yang Berfungsi Sebagai Perangkat Input." },
  { "en": "Apa Itu Stylus?", "id": "Pena Digital Untuk Layar Sentuh." },
  { "en": "Apa Itu Drawing Tablet?", "id": "Perangkat Input Untuk Menggambar Digital." },
  { "en": "Apa Itu Firmware?", "id": "Perangkat Lunak Yang Tertanam Dalam Perangkat Keras." },
  { "en": "Apa Beda Firmware Dan Driver?", "id": "Firmware Di Perangkat Driver Di Sistem Operasi." },
  { "en": "Apa Itu Update Firmware?", "id": "Memperbarui Perangkat Lunak Internal Perangkat Keras." },
  { "en": "Apa Itu Chipset Motherboard?", "id": "Chip Yang Mengatur Aliran Data." },
  { "en": "Apa Fungsi Chipset?", "id": "Menentukan Kompatibilitas CPU Dan Fitur Lain." },
  { "en": "Apa Itu SATA Controller?", "id": "Sirkuit Yang Mengelola Perangkat SATA." },
  { "en": "Apa Itu Konektor Daya Molex?", "id": "Konektor Daya Lama Untuk Kipas Dan HDD." },
  { "en": "Konektor Apa Yang Menggantikan Molex?", "id": "Konektor Daya SATA." },
  { "en": "Apa Itu Konektor Daya CPU?", "id": "Kabel Khusus Dari PSU Ke Motherboard." },
  { "en": "Berapa Pin Konektor Daya CPU?", "id": "Biasanya 4 Atau 8 Pin." },
  { "en": "Apa Itu Konektor Daya Motherboard Utama?", "id": "Konektor Terbesar Dari PSU." },
  { "en": "Berapa Pin Konektor Daya Motherboard?", "id": "24 Pin." },
  { "en": "Apa Itu Konektor Daya PCIe?", "id": "Kabel Daya Tambahan Untuk Kartu Grafis." },
  { "en": "Berapa Pin Konektor Daya PCIe?", "id": "6 Atau 8 Pin." },
  { "en": "Apa Itu Backplate CPU?", "id": "Pelat Logam Di Belakang Soket CPU." },
  { "en": "Apa Fungsi Backplate CPU?", "id": "Mendukung Beban Berat Pendingin CPU." },
  { "en": "Apa Itu Integrated Heat Spreader (IHS)?", "id": "Penutup Logam Di Atas Chip CPU." },
  { "en": "Apa Fungsi IHS?", "id": "Melindungi Chip Dan Menyebarkan Panas." },
  { "en": "Apa Itu Delidding CPU?", "id": "Proses Melepas IHS Dari Chip CPU." },
  { "en": "Mengapa Orang Melakukan Delidding?", "id": "Untuk Mengganti Thermal Paste Internal." },
  { "en": "Apa Itu Liquid Metal Thermal Paste?", "id": "Pasta Termal Berbasis Logam Cair." },
  { "en": "Apa Kelebihan Liquid Metal?", "id": "Daya Hantar Panas Jauh Lebih Baik." },
  { "en": "Apa Kekurangan Liquid Metal?", "id": "Bersifat Konduktif Dan Korosif." },
  { "en": "Apa Itu RGB?", "id": "Red Green Blue." },
  { "en": "Apa Fungsi RGB Pada Perangkat Keras?", "id": "Memberikan Pencahayaan Dekoratif Warna-Warni." },
  { "en": "Apa Itu ARGB?", "id": "Addressable RGB." },
  { "en": "Apa Beda RGB Dan ARGB?", "id": "ARGB Memungkinkan Kontrol Warna Setiap LED." },
  { "en": "Apa Itu Port Header ARGB?", "id": "Konektor 3-Pin 5V Di Motherboard." },
  { "en": "Apa Itu Port Header RGB?", "id": "Konektor 4-Pin 12V Di Motherboard." },
  { "en": "Apa Itu ESD?", "id": "Electrostatic Discharge." },
  { "en": "Mengapa ESD Berbahaya Untuk Perangkat Keras?", "id": "Dapat Merusak Komponen Elektronik Sensitif." },
  { "en": "Bagaimana Cara Mencegah ESD?", "id": "Menggunakan Gelang Anti-Statis." },
  { "en": "Apa Itu Gelang Anti-Statis?", "id": "Gelang Untuk Menyalurkan Listrik Statis Ke Tanah." },
  { "en": "Apa Itu Optical Mouse?", "id": "Mouse Yang Menggunakan Sensor Cahaya." },
  { "en": "Apa Itu Laser Mouse?", "id": "Mouse Yang Menggunakan Laser Untuk Pelacakan." },
  { "en": "Manakah Yang Lebih Presisi Optical Atau Laser?", "id": "Laser Mouse Umumnya Lebih Presisi." },
  { "en": "Apa Itu Palm Grip?", "id": "Gaya Memegang Mouse Dengan Seluruh Telapak Tangan." },
  { "en": "Apa Itu Claw Grip?", "id": "Gaya Memegang Mouse Seperti Cakar." },
  { "en": "Apa Itu Refresh Rate Keyboard?", "id": "Seberapa Cepat Keyboard Mengirim Sinyal." },
  { "en": "Apa Itu N-Key Rollover (NKRO)?", "id": "Kemampuan Keyboard Mendeteksi Semua Tombol Ditekan." },
  { "en": "Apa Itu Anti-Ghosting?", "id": "Mencegah Tombol Salah Terdeteksi Saat Ditekan." },
  { "en": "Apa Itu PBT Keycaps?", "id": "Tutup Tombol Keyboard Dari Bahan PBT." },
  { "en": "Apa Kelebihan PBT Dibanding ABS?", "id": "Lebih Tahan Lama Dan Tidak Mudah Mengkilap." },
  { "en": "Apa Itu RAM Disk?", "id": "Menggunakan Sebagian RAM Sebagai Drive Virtual." },
  { "en": "Apa Keuntungan RAM Disk?", "id": "Kecepatan Sangat Ekstrem." },
  { "en": "Apa Itu Virtual Memory?", "id": "Menggunakan Penyimpanan Sebagai Perluasan RAM." },
  { "en": "Apa Nama File Virtual Memory Di Windows?", "id": "Page File (Pagefile.sys)." },
  { "en": "Apa Itu Defragmentasi?", "id": "Proses Merapikan File Terfragmentasi Di HDD." },
  { "en": "Mengapa Defragmentasi Penting Untuk HDD?", "id": "Meningkatkan Kecepatan Baca Data." },
  { "en": "Apakah SSD Perlu Di-Defrag?", "id": "Tidak Dan Tidak Dianjurkan." },
  { "en": "Apa Itu TRIM Pada SSD?", "id": "Perintah Untuk Menjaga Kinerja SSD." },
  { "en": "Apa Itu Wear Leveling?", "id": "Teknik Meratakan Penggunaan Sel Memori SSD." },
  { "en": "Apa Itu S.M.A.R.T.?", "id": "Self-Monitoring Analysis and Reporting Technology." },
  { "en": "Apa Fungsi S.M.A.R.T.?", "id": "Memprediksi Kegagalan Drive Penyimpanan." },
  { "en": "Apa Itu Read/Write Head HDD?", "id": "Komponen Untuk Membaca Dan Menulis Data." },
  { "en": "Apa Itu Platter HDD?", "id": "Piringan Magnetik Tempat Data Disimpan." },
  { "en": "Apa Itu Spindle HDD?", "id": "Motor Yang Memutar Platter." },
  { "en": "Apa Itu Actuator Arm HDD?", "id": "Lengan Yang Menggerakkan Read/Write Head." },
  { "en": "Mengapa HDD Berisik?", "id": "Karena Adanya Suara Putaran Dan Gerakan Lengan." },
  { "en": "Apa Itu Partisi Drive?", "id": "Membagi Satu Drive Fisik Menjadi Beberapa Drive Logis." },
  { "en": "Apa Itu Sistem File (File System)?", "id": "Struktur Untuk Mengatur File Di Drive." },
  { "en": "Sistem File Apa Yang Digunakan Windows?", "id": "NTFS (New Technology File System)." },
  { "en": "Apa Itu Formating Drive?", "id": "Mempersiapkan Drive Dengan Sistem File Tertentu." },
  { "en": "Apa Itu Booting?", "id": "Proses Menghidupkan Komputer Dan Memuat OS." },
  { "en": "Apa Itu Boot Drive?", "id": "Drive Tempat Sistem Operasi Diinstal." },
  { "en": "Apa Itu Dual Boot?", "id": "Menginstal Dua Sistem Operasi Di Satu Komputer." },
  { "en": "Apa Itu Virtual Machine (VM)?", "id": "Menjalankan Komputer Virtual Di Dalam Komputer Fisik." },
  { "en": "Apa Itu Hypervisor?", "id": "Perangkat Lunak Untuk Membuat Dan Menjalankan VM." },
  { "en": "Apa Itu CPU Cooler?", "id": "Perangkat Pendingin Khusus Untuk CPU." },
  { "en": "Apa Itu All-in-One (AIO) PC?", "id": "PC Desktop Dengan Komponen Di Belakang Monitor." },
  { "en": "Apa Itu Laptop Convertible?", "id": "Laptop Yang Layarnya Bisa Dilipat Menjadi Tablet." },
  { "en": "Apa Itu Ultrabook?", "id": "Laptop Sangat Tipis Dan Ringan." },
  { "en": "Apa Itu Chromebook?", "id": "Laptop Yang Menjalankan Chrome OS." },
  { "en": "Apa Itu Sound Blaster?", "id": "Merek Kartu Suara Populer." },
  { "en": "Apa Itu DAC?", "id": "Digital-to-Analog Converter." },
  { "en": "Apa Fungsi DAC?", "id": "Mengubah Sinyal Audio Digital Menjadi Analog." },
  { "en": "Apa Itu Amplifier Audio?", "id": "Menguatkan Sinyal Audio Untuk Speaker." },
  { "en": "Apa Itu Resistansi (Resistance)?", "id": "Hambatan Terhadap Aliran Arus Listrik." },
  { "en": "Apa Itu Tegangan (Voltage)?", "id": "Perbedaan Potensial Listrik." },
  { "en": "Apa Itu Arus (Current)?", "id": "Aliran Muatan Listrik." },
  { "en": "Apa Itu Hukum Ohm?", "id": "Hubungan Antara Tegangan Arus Dan Resistansi." },
  { "en": "Apa Itu Sirkuit Terpadu (IC)?", "id": "Kumpulan Komponen Elektronik Dalam Satu Chip." },
  { "en": "Apa Itu Transistor?", "id": "Komponen Dasar Pembangun Sirkuit Digital." },
  { "en": "Apa Fungsi Transistor?", "id": "Sebagai Saklar Atau Penguat Sinyal." },
  { "en": "Apa Itu Hukum Moore?", "id": "Jumlah Transistor Pada Chip Berlipat Ganda." },
  { "en": "Apakah Hukum Moore Masih Berlaku?", "id": "Lajunya Mulai Melambat." },
  { "en": "Apa Itu Die CPU?", "id": "Potongan Silikon Yang Menjadi Chip CPU." },
  { "en": "Apa Itu Fabrikasi Chip?", "id": "Proses Manufaktur Sirkuit Terpadu." },
  { "en": "Apa Satuan Ukuran Proses Fabrikasi?", "id": "Nanometer (Nm)." },
  { "en": "Semakin Kecil Ukuran Fabrikasi Berarti Apa?", "id": "Chip Lebih Efisien Dan Kuat." },
  { "en": "Apa Itu VGA Cooler?", "id": "Sistem Pendingin Untuk Kartu Grafis." },
  { "en": "Apa Itu Blower Style Cooler?", "id": "Pendingin GPU Yang Meniup Udara Keluar Casing." },
  { "en": "Apa Itu Open Air Cooler?", "id": "Pendingin GPU Dengan Banyak Kipas." },
  { "en": "Manakah Yang Lebih Dingin Blower Atau Open Air?", "id": "Open Air Cooler Umumnya Lebih Dingin." },
  { "en": "Apa Itu Kecerahan (Brightness) Monitor?", "id": "Tingkat Intensitas Cahaya Yang Dihasilkan Layar." },
  { "en": "Apa Satuan Kecerahan Monitor?", "id": "Nits Atau Candela Per Meter Persegi." },
  { "en": "Apa Itu Gamut Warna?", "id": "Rentang Warna Yang Dapat Ditampilkan Perangkat." },
  { "en": "Gamut Warna Apa Yang Umum Untuk Web?", "id": "sRGB (Standard Red Green Blue)." },
  { "en": "Apa Itu DisplayHDR?", "id": "Standar Kualitas Untuk Tampilan High Dynamic Range." },
  { "en": "Apa Itu High Dynamic Range (HDR)?", "id": "Meningkatkan Kontras Dan Rentang Warna." },
  { "en": "Apa Itu G-Sync?", "id": "Teknologi NVIDIA Untuk Sinkronisasi Refresh Rate." },
  { "en": "Apa Itu FreeSync?", "id": "Teknologi AMD Untuk Sinkronisasi Refresh Rate." },
  { "en": "Apa Fungsi G-Sync Dan FreeSync?", "id": "Menghilangkan Screen Tearing Dan Stuttering." },
  { "en": "Apa Itu Screen Tearing?", "id": "Gambar Terlihat Sobek Saat Gerakan Cepat." },
  { "en": "Apa Itu Speaker?", "id": "Perangkat Output Untuk Menghasilkan Suara." },
  { "en": "Apa Itu Subwoofer?", "id": "Speaker Khusus Untuk Suara Frekuensi Rendah (Bass)." },
  { "en": "Apa Itu Sistem Suara 2.1?", "id": "Dua Speaker Satelit Dan Satu Subwoofer." },
  { "en": "Apa Itu Headphone?", "id": "Perangkat Audio Pribadi Yang Dikenakan Di Telinga." },
  { "en": "Apa Beda Headphone Dan Headset?", "id": "Headset Memiliki Mikrofon Terintegrasi." },
  { "en": "Apa Itu Noise-Cancelling Headphone?", "id": "Mengurangi Suara Bising Dari Luar." },
  { "en": "Apa Itu Open-Back Headphone?", "id": "Headphone Dengan Bagian Belakang Terbuka." },
  { "en": "Apa Kelebihan Open-Back Headphone?", "id": "Menghasilkan Soundstage Yang Luas Dan Alami." },
  { "en": "Apa Itu Closed-Back Headphone?", "id": "Headphone Dengan Bagian Belakang Tertutup." },
  { "en": "Apa Kelebihan Closed-Back Headphone?", "id": "Isolasi Suara Baik Tidak Bocor Keluar." },
  { "en": "Apa Itu Motherboard Standoff?", "id": "Nama Lain Untuk Sekrup Pengganjal Motherboard." },
  { "en": "Apa Itu Kabel SATA?", "id": "Kabel Data Untuk Menghubungkan Drive SATA." },
  { "en": "Apa Itu Kabel Power SATA?", "id": "Kabel Daya Dari PSU Untuk Drive SATA." },
  { "en": "Apa Beda Kabel SATA Data Dan Power?", "id": "Bentuk Konektornya Sangat Berbeda." },
  { "en": "Apa Itu PCB?", "id": "Printed Circuit Board." },
  { "en": "Apa Fungsi PCB?", "id": "Menghubungkan Komponen Elektronik Menggunakan Jalur Tembaga." },
  { "en": "Apa Warna Umum PCB?", "id": "Hijau Biru Merah Atau Hitam." },
  { "en": "Apa Itu Solder?", "id": "Proses Menyambung Komponen Ke PCB." },
  { "en": "Apa Itu Socket LGA 1200?", "id": "Soket Intel Untuk CPU Generasi Ke-10 Dan Ke-11." },
  { "en": "Apa Itu Socket AM4?", "id": "Soket AMD Untuk CPU Ryzen." },
  { "en": "Apakah CPU Intel Bisa Dipasang Di Motherboard AMD?", "id": "Tidak Soketnya Sepenuhnya Berbeda." },
  { "en": "Apa Itu Pin CPU Bengkok?", "id": "Kerusakan Fisik Umum Pada CPU Tipe PGA." },
  { "en": "Apa Itu Integrated I/O Shield?", "id": "Pelindung Port Belakang Yang Menyatu Dengan Motherboard." },
  { "en": "Apa Itu Heatsink Chipset?", "id": "Pendingin Khusus Untuk Chipset Motherboard." },
  { "en": "Apa Itu Kompatibilitas RAM?", "id": "Kecocokan Jenis Dan Kecepatan RAM Dengan Motherboard." },
  { "en": "Apa Itu QVL (Qualified Vendor List)?", "id": "Daftar RAM Yang Teruji Kompatibel." },
  { "en": "Apa Itu XMP (Extreme Memory Profile)?", "id": "Profil Overclocking RAM Otomatis Dari Intel." },
  { "en": "Apa Nama Fitur Serupa XMP Dari AMD?", "id": "DOCP (Direct Over Clock Profile)." },
  { "en": "Apa Itu Timing RAM?", "id": "Ukuran Latensi Atau Penundaan Memori RAM." },
  { "en": "Semakin Rendah Timing RAM Berarti Apa?", "id": "Semakin Cepat Kinerja Memori RAM." },
  { "en": "Apa Itu Backplate GPU?", "id": "Pelat Logam Di Sisi Belakang Kartu Grafis." },
  { "en": "Apa Fungsi Backplate GPU?", "id": "Melindungi PCB Dan Membantu Pendinginan." },
  { "en": "Apa Itu GPU Sag?", "id": "Saat Kartu Grafis Berat Melengkung Ke Bawah." },
  { "en": "Bagaimana Cara Mencegah GPU Sag?", "id": "Menggunakan Penyangga Kartu Grafis (GPU Bracket)." },
  { "en": "Apa Itu SLI?", "id": "Scalable Link Interface (NVIDIA)." },
  { "en": "Apa Itu CrossFire?", "id": "Teknologi Serupa SLI Dari AMD." },
  { "en": "Apa Fungsi SLI Dan CrossFire?", "id": "Menggabungkan Dua Atau Lebih GPU." },
  { "en": "Apakah SLI Masih Populer?", "id": "Sudah Jarang Digunakan Di Gaming Modern." },
  { "en": "Apa Itu Ray Tracing?", "id": "Teknik Rendering Grafis Realistis." },
  { "en": "Bagaimana Ray Tracing Bekerja?", "id": "Mensimulasikan Perilaku Fisik Cahaya." },
  { "en": "Perangkat Keras Apa Yang Dibutuhkan Untuk Ray Tracing?", "id": "GPU Modern Dengan Core Khusus." },
  { "en": "Apa Itu Tensor Core?", "id": "Core Khusus Di GPU NVIDIA Untuk AI." },
  { "en": "Apa Itu DLSS (Deep Learning Super Sampling)?", "id": "Teknologi NVIDIA Untuk Meningkatkan FPS." },
  { "en": "Bagaimana DLSS Bekerja?", "id": "Menggunakan AI Untuk Merender Gambar Resolusi Rendah." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Mengakses Layanan Komputer Melalui Internet." },
  { "en": "Apa Hubungannya Dengan Perangkat Keras?", "id": "Menggunakan Perangkat Keras Server Di Datacenter." },
  { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Perangkat Fisik Dengan Sensor." },
  { "en": "Contoh Perangkat Keras IoT?", "id": "Lampu Pintar Sensor Suhu Jam Tangan Pintar." },
  { "en": "Apa Itu Arduino?", "id": "Platform Mikrokontroler Open-Source." },
  { "en": "Untuk Apa Arduino Digunakan?", "id": "Membuat Proyek Elektronik Interaktif." },
  { "en": "Apa Beda Raspberry Pi Dan Arduino?", "id": "Raspberry Pi Komputer Arduino Mikrokontroler." },
  { "en": "Apa Itu Mikrokontroler?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Wearable Device?", "id": "Perangkat Komputer Yang Dapat Dikenakan." },
  { "en": "Contoh Wearable Device?", "id": "Smartwatch Dan Fitness Tracker." },
  { "en": "Apa Itu Augmented Reality (AR)?", "id": "Menggabungkan Objek Virtual Ke Dunia Nyata." },
  { "en": "Apa Itu Virtual Reality (VR)?", "id": "Menciptakan Lingkungan Sepenuhnya Virtual." },
  { "en": "Perangkat Keras Apa Yang Dibutuhkan Untuk VR?", "id": "Headset VR Dan Kontroler Gerak." },
  { "en": "Apa Itu Haptic Feedback?", "id": "Umpan Balik Berupa Getaran Atau Gerakan." },
  { "en": "Di Mana Haptic Feedback Digunakan?", "id": "Kontroler Game Dan Smartphone." },
  { "en": "Apa Itu Ergonomi?", "id": "Ilmu Desain Produk Agar Nyaman Digunakan." },
  { "en": "Contoh Perangkat Keras Ergonomis?", "id": "Keyboard Terpisah Dan Mouse Vertikal." },
  { "en": "Apa Itu CRT Monitor?", "id": "Cathode Ray Tube." },
  { "en": "Monitor Jenis Apa Yang Digantikan CRT?", "id": "Monitor Tabung Gendut Yang Sangat Berat." },
  { "en": "Apa Itu Layar LCD?", "id": "Liquid Crystal Display." },
  { "en": "Apa Itu Backlight?", "id": "Sumber Cahaya Di Belakang Layar LCD." },
  { "en": "Apa Itu Layar LED?", "id": "Layar LCD Yang Menggunakan LED Sebagai Backlight." },
  { "en": "Apa Beda LED Dan OLED?", "id": "OLED Tidak Membutuhkan Backlight." },
  { "en": "Apa Kepanjangan OLED?", "id": "Organic Light Emitting Diode." },
  { "en": "Apa Kelebihan Layar OLED?", "id": "Warna Hitam Sempurna Dan Kontras Tak Terbatas." },
  { "en": "Apa Itu Screen Burn-In?", "id": "Bayangan Permanen Gambar Statis Di Layar." },
  { "en": "Layar Jenis Apa Yang Rentan Burn-In?", "id": "Layar OLED." },
  { "en": "Apa Itu Kabel Power?", "id": "Kabel Yang Menghubungkan PSU Ke Stop Kontak." },
  { "en": "Apa Itu Surge Protector?", "id": "Melindungi Perangkat Dari Lonjakan Listrik." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply)?", "id": "Memberikan Cadangan Daya Baterai Jangka Pendek." },
  { "en": "Apa Fungsi Utama UPS?", "id": "Memberi Waktu Untuk Menyimpan Pekerjaan Saat Mati Listrik." },
  { "en": "Apa Itu Keyboard Nirkabel (Wireless)?", "id": "Keyboard Tanpa Kabel." },
  { "en": "Teknologi Apa Yang Digunakan Keyboard Nirkabel?", "id": "Bluetooth Atau Dongle USB 2.4GHz." },
  { "en": "Apa Itu Dongle?", "id": "Adaptor Kecil Yang Dicolokkan Ke Port USB." },
  { "en": "Apa Itu Slot M.2 Key E?", "id": "Slot Untuk Modul Wi-Fi Dan Bluetooth." },
  { "en": "Apa Itu Heatsink M.2?", "id": "Pendingin Khusus Untuk SSD M.2." },
  { "en": "Mengapa SSD M.2 Perlu Heatsink?", "id": "Karena Bisa Menjadi Sangat Panas." },
  { "en": "Apa Itu Thermal Throttling?", "id": "Penurunan Kinerja Otomatis Akibat Panas Berlebih." },
  { "en": "Komponen Mana Yang Sering Mengalami Thermal Throttling?", "id":"CPU GPU Dan SSD NVMe." },
  { "en": "Apa Itu Slot DIMM?", "id": "Dual In-Line Memory Module." },
  { "en": "Apa Fungsi Slot DIMM?", "id": "Tempat Memasang Modul RAM Di Motherboard." },
  { "en": "Berapa Jumlah Pin Pada RAM DDR4?", "id": "288 Pin." },
  { "en": "Berapa Jumlah Pin Pada RAM DDR5?", "id": "288 Pin Sama Seperti DDR4." },
  { "en": "Apa Beda Tegangan Operasi DDR4 Dan DDR5?", "id": "DDR5 Menggunakan Tegangan Lebih Rendah." },
  { "en": "Apa Itu ECC RAM?", "id": "Error-Correcting Code RAM." },
  { "en": "Apa Fungsi ECC RAM?", "id": "Mendeteksi Dan Memperbaiki Kesalahan Data." },
  { "en": "Di Mana ECC RAM Umum Digunakan?", "id": "Di Server Dan Workstation." },
  { "en": "Apakah Motherboard Biasa Mendukung ECC RAM?", "id": "Tidak Membutuhkan Motherboard Dan CPU Khusus." },
  { "en": "Apa Itu Registered RAM (RDIMM)?", "id": "RAM Dengan Register Untuk Menstabilkan Sinyal." },
  { "en": "Apa Itu Unbuffered RAM (UDIMM)?", "id": "RAM Standar Tanpa Register." },
  { "en": "Manakah Yang Digunakan Di PC Biasa?", "id": "Unbuffered RAM (UDIMM)." },
  { "en": "Apa Itu TFX Form Factor?", "id": "Form Factor PSU Yang Tipis Dan Panjang." },
  { "en": "Apa Itu SFX Form Factor?", "id": "Form Factor PSU Kecil Untuk Casing SFF." },
  { "en": "Apa Itu Fan Hub?", "id": "Perangkat Untuk Menambah Jumlah Konektor Kipas." },
  { "en": "Apa Itu Fan Controller?", "id": "Mengatur Kecepatan Putaran Kipas." },
  { "en": "Apa Itu PWM Fan?", "id": "Kipas Dengan Kontrol Kecepatan Empat Pin." },
  { "en": "Apa Beda Kipas 3-Pin Dan 4-Pin?", "id": "4-Pin (PWM) Memiliki Kontrol Kecepatan Lebih Presisi." },
  { "en": "Apa Itu Static Pressure Fan?", "id": "Kipas Yang Dioptimalkan Untuk Mendorong Udara." },
  { "en": "Kapan Static Pressure Fan Dibutuhkan?", "id": "Untuk Radiator Dan Heatsink Padat." },
  { "en": "Apa Itu Airflow Fan?", "id": "Kipas Yang Dioptimalkan Untuk Memindahkan Volume Udara." },
  { "en": "Kapan Airflow Fan Digunakan?", "id": "Sebagai Kipas Intake Dan Exhaust Casing." },
  { "en": "Apa Satuan Aliran Udara Kipas?", "id": "CFM (Cubic Feet per Minute)." },
  { "en": "Apa Satuan Tekanan Statis Kipas?", "id": "mmH2O." },
  { "en": "Apa Itu Tingkat Kebisingan Kipas?", "id": "Tingkat Suara Yang Dihasilkan Kipas." },
  { "en": "Apa Satuan Kebisingan Kipas?", "id": "Desibel (dBA)." },
  { "en": "Apa Itu Solder Mask?", "id": "Lapisan Pelindung Berwarna Pada PCB." },
  { "en": "Apa Fungsi Solder Mask?", "id": "Mencegah Hubung Singkat Antar Jalur Tembaga." },
  { "en": "Apa Itu Silkscreen?", "id": "Label Teks Dan Simbol Putih Pada PCB." },
  { "en": "Apa Fungsi Silkscreen?", "id": "Menunjukkan Lokasi Dan Identitas Komponen." },
  { "en": "Apa Itu Laptop Gaming?", "id": "Laptop Dengan Perangkat Keras Berkinerja Tinggi." },
  { "en": "Apa Ciri Khas Laptop Gaming?", "id": "GPU Diskrit Kuat Dan Sistem Pendingin Canggih." },
  { "en": "Apa Itu Desktop Replacement (DTR) Laptop?", "id": "Laptop Sangat Besar Dan Berat." },
  { "en": "Apa Itu Port Audio Optik (S/PDIF)?", "id": "Mengirim Sinyal Audio Digital Melalui Cahaya." },
  { "en": "Apa Itu Port LAN 2.5G?", "id": "Port Ethernet Dengan Kecepatan 2.5 Gigabit." },
  { "en": "Apa Itu Wi-Fi 6 (802.11ax)?", "id": "Standar Wi-Fi Modern Yang Cepat." },
  { "en": "Apa Keuntungan Wi-Fi 6?", "id": "Kecepatan Lebih Tinggi Dan Kinerja Lebih Baik." },
  { "en": "Apa Itu Bluetooth 5.0?", "id": "Versi Bluetooth Dengan Jangkauan Dan Kecepatan Lebih." },
  { "en": "Apa Itu Fingerprint Scanner?", "id": "Sensor Sidik Jari Untuk Keamanan Biometrik." },
  { "en": "Apa Itu TPM?", "id": "Trusted Platform Module." },
  { "en": "Apa Fungsi Chip TPM?", "id": "Menyimpan Kunci Enkripsi Secara Aman." },
  { "en": "Mengapa TPM Penting Untuk Windows 11?", "id": "Merupakan Persyaratan Keamanan Minimum." },
  { "en": "Apa Itu Secure Boot?", "id": "Fitur Keamanan Untuk Mencegah Malware Saat Booting." },
  { "en": "Apa Itu RAID Perangkat Keras (Hardware RAID)?", "id": "RAID Yang Dikelola Oleh Kartu Khusus." },
  { "en": "Apa Itu RAID Perangkat Lunak (Software RAID)?", "id": "RAID Yang Dikelola Oleh Sistem Operasi." },
  { "en": "Manakah Yang Kinerjanya Lebih Baik?", "id": "Hardware RAID Umumnya Lebih Cepat." },
  { "en": "Apa Itu Cache Drive?", "id": "SSD Kecil Untuk Mempercepat Kinerja HDD." },
  { "en": "Apa Itu SSHD (Solid State Hybrid Drive)?", "id": "HDD Dengan Cache SSD Terintegrasi." },
  { "en": "Apa Itu Teknologi Optane Intel?", "id": "Memori Super Cepat Untuk Caching." },
  { "en": "Apa Itu Barcode Scanner?", "id": "Perangkat Input Untuk Membaca Barcode." },
  { "en": "Apa Itu QR Code?", "id": "Barcode Dua Dimensi." },
  { "en": "Apa Itu Proyektor?", "id": "Perangkat Output Untuk Menampilkan Gambar Di Permukaan." },
  { "en": "Apa Itu Lumen?", "id": "Satuan Kecerahan Untuk Proyektor." },
  { "en": "Apa Itu Smart TV?", "id": "Televisi Dengan Kemampuan Terhubung Ke Internet." },
  { "en": "Apa Itu TV Box?", "id": "Mengubah TV Biasa Menjadi Smart TV." },
  { "en": "Apa Itu Chromecast?", "id": "Perangkat Streaming Media Dari Google." },
  { "en": "Apa Itu Raspberry Pi OS?", "id": "Sistem Operasi Resmi Untuk Raspberry Pi." },
  { "en": "Apa Itu GPIO Pins?", "id": "General-Purpose Input/Output Pins." },
  { "en": "Di Mana GPIO Pins Ditemukan?", "id": "Pada Single-Board Computer Seperti Raspberry Pi." },
  { "en": "Apa Itu Sensor?", "id": "Perangkat Yang Mendeteksi Perubahan Di Lingkungan." },
  { "en": "Contoh Sensor?", "id": "Sensor Suhu Cahaya Dan Gerak." },
  { "en": "Apa Itu Aktuator?", "id": "Perangkat Yang Menghasilkan Gerakan Atau Aksi." },
  { "en": "Contoh Aktuator?", "id": "Motor Servo Dan Solenoid." },
  { "en": "Apa Itu Open Source Hardware?", "id": "Desain Perangkat Keras Yang Tersedia Bebas." },
  { "en": "Apa Itu PCI Slot?", "id": "Slot Ekspansi Lama Sebelum PCIe." },
  { "en": "Apa Itu AGP Slot?", "id": "Slot Khusus Kartu Grafis Sebelum PCIe." },
  { "en": "Apa Itu Jumperless Motherboard?", "id": "Motherboard Dengan Pengaturan Di BIOS." },
  { "en": "Apa Itu Plug and Play (PnP)?", "id": "Sistem Mengenali Perangkat Keras Secara Otomatis." },
  { "en": "Apa Itu IRQ (Interrupt Request)?", "id": "Sinyal Dari Perangkat Keras Ke CPU." },
  { "en": "Apa Itu DMA (Direct Memory Access)?", "id": "Memungkinkan Perangkat Mengakses RAM Langsung." },
  { "en": "Apa Keuntungan DMA?", "id": "Mengurangi Beban Kerja CPU." },
  { "en": "Apa Itu Casing Full Tower?", "id": "Casing Terbesar Untuk PC Desktop." },
  { "en": "Apa Itu Casing Mid Tower?", "id": "Ukuran Casing Paling Umum Untuk PC." },
  { "en": "Apa Itu Casing Mini Tower?", "id": "Casing Lebih Kecil Untuk Motherboard Micro-ATX." },
  { "en": "Apa Itu Casing Test Bench?", "id": "Casing Terbuka Untuk Pengujian Komponen." },
  { "en": "Apa Itu Tempered Glass Panel?", "id": "Panel Samping Casing Dari Kaca Keras." },
  { "en": "Apa Itu PSU Shroud?", "id": "Penutup Untuk Menyembunyikan PSU Dan Kabel." },
  { "en": "Apa Itu Drive Bay?", "id": "Tempat Memasang Drive Penyimpanan Di Casing." },
  { "en": "Apa Itu Drive Bay 5.25 Inci?", "id": "Untuk Optical Drive." },
  { "en": "Apa Itu Drive Bay 3.5 Inci?", "id": "Untuk Hard Disk Drive (HDD)." },
  { "en": "Apa Itu Drive Bay 2.5 Inci?", "id": "Untuk Solid State Drive (SSD)." },
  { "en": "Apa Itu Riser Cable PCIe?", "id": "Kabel Ekstensi Untuk Slot PCIe." },
  { "en": "Untuk Apa Riser Cable Digunakan?", "id": "Memasang GPU Secara Vertikal." },
  { "en": "Apa Itu Laptop Baterai Tanam (Internal)?", "id": "Baterai Yang Tidak Mudah Dilepas." },
  { "en": "Apa Itu Laptop Baterai Eksternal?", "id": "Baterai Yang Dapat Dilepas Dengan Mudah." },
  { "en": "Apa Itu Magsafe?", "id": "Konektor Daya Magnetik Pada MacBook Lama." },
  { "en": "Apa Itu Kensington Lock Slot?", "id": "Lubang Untuk Mengunci Laptop Secara Fisik." },
  { "en": "Apa Itu Digital Pen?", "id": "Nama Lain Untuk Stylus." },
  { "en": "Apa Itu Backlit Keyboard?", "id": "Keyboard Dengan Lampu Latar." },
  { "en": "Apa Itu Bezel?", "id": "Bingkai Di Sekeliling Layar." },
  { "en": "Apa Itu Bezel-less Display?", "id": "Layar Dengan Bingkai Sangat Tipis." },
  { "en": "Apa Itu SD Card Reader?", "id": "Slot Untuk Membaca Kartu Memori SD." },
  { "en": "Apa Itu SIM Card Tray?", "id": "Tempat Memasang Kartu SIM Di Laptop." },
  { "en": "Apa Itu WWAN (Wireless Wide Area Network)?", "id": "Koneksi Internet Seluler Untuk Laptop." },
  { "en": "Apa Itu Antena Wi-Fi?", "id": "Komponen Untuk Menerima Dan Mengirim Sinyal Wi-Fi." },
  { "en": "Mengapa Beberapa Router Punya Banyak Antena?", "id": "Untuk Teknologi MIMO Dan Jangkauan Lebih Baik." },
  { "en": "Apa Itu MIMO?", "id": "Multiple-Input Multiple-Output." },
  { "en": "Apa Itu Chipset Audio Onboard?", "id": "Chip Sirkuit Suara Yang Terintegrasi Di Motherboard." },
  { "en": "Merek Chipset Audio Apa Yang Populer?", "id": "Realtek." },
  { "en": "Apa Itu Chipset Jaringan Onboard?", "id": "Chip Pengontrol Jaringan Di Motherboard." },
  { "en": "Merek Chipset Jaringan Apa Yang Populer?", "id": "Intel Dan Realtek." },
  { "en": "Apa Itu TDP (Thermal Design Power)?", "id": "Jumlah Panas Maksimal Yang Dihasilkan Komponen." },
  { "en": "Apa Satuan TDP?", "id": "Watt (W)." },
  { "en": "Mengapa TDP Penting?", "id": "Untuk Memilih Sistem Pendingin Yang Tepat." },
  { "en": "Apa Itu Binning CPU?", "id": "Proses Memilah Chip Berdasarkan Kualitasnya." },
  { "en": "Chip Terbaik Dijual Sebagai Apa?", "id": "Model CPU Kelas Atas." },
  { "en": "Apa Itu 'Silicon Lottery'?", "id": "Peluang Mendapatkan Chip Dengan Potensi Overclocking Tinggi." },
  { "en": "Apa Itu Lapisan PCB?", "id": "Jumlah Lapisan Sirkuit Tembaga Di PCB." },
  { "en": "Berapa Lapisan PCB Motherboard Berkualitas?", "id": "Enam Lapisan Atau Lebih." },
  { "en": "Apa Itu Thru-Hole Component?", "id": "Komponen Dengan Kaki Yang Menembus PCB." },
  { "en": "Apa Itu Surface-Mount Device (SMD)?", "id": "Komponen Yang Disolder Di Permukaan PCB." },
  { "en": "Manakah Yang Lebih Umum Di Perangkat Modern?", "id": "SMD Karena Ukurannya Jauh Lebih Kecil." },
  { "en": "Apa Itu Resistansi (Resistance)?", "id": "Hambatan Terhadap Aliran Listrik." },
  { "en": "Apa Itu Dioda?", "id": "Komponen Yang Memungkinkan Arus Mengalir Satu Arah." },
  { "en": "Apa Itu LED?", "id": "Light-Emitting Diode." },
  { "en": "Apa Itu LCD?", "id": "Liquid Crystal Display." },
  { "en": "Apa Itu Konektor FPC?", "id": "Flexible Printed Circuit Connector." },
  { "en": "Di Mana Konektor FPC Ditemukan?", "id": "Menghubungkan Layar Keyboard Di Laptop." },
  { "en": "Apa Itu Inverter Laptop?", "id": "Memberi Daya Pada Backlight Layar LCD Lama." },
  { "en": "Apakah Layar LED Laptop Butuh Inverter?", "id": "Tidak Karena LED Tidak Butuh Tegangan Tinggi." },
  { "en": "Apa Itu Engsel (Hinge) Laptop?", "id": "Mekanisme Untuk Membuka Dan Menutup Layar." },
  { "en": "Apa Itu Bezel Layar?", "id": "Bingkai Plastik Di Sekeliling Layar." },
  { "en": "Apa Itu Palm Rest?", "id": "Area Tempat Telapak Tangan Beristirahat." },
  { "en": "Apa Itu Caddy HDD/SSD?", "id": "Adaptor Untuk Memasang Drive Di Slot Optical." },
  { "en": "Apa Itu Konektor ZIF?", "id": "Zero Insertion Force Connector." },
  { "en": "Apa Ciri Konektor ZIF?", "id": "Memiliki Latch Untuk Mengunci Kabel Pita." },
  { "en": "Apa Itu Kabel Pita (Ribbon Cable)?", "id": "Kabel Datar Lebar Dengan Banyak Konduktor." },
  { "en": "Apa Itu IDE/PATA?", "id": "Standar Antarmuka Drive Lama Sebelum SATA." },
  { "en": "Apa Beda Utama IDE Dan SATA?", "id": "IDE Paralel Dan Lebar SATA Serial." },
  { "en": "Apa Itu Master/Slave Setting?", "id": "Konfigurasi Jumper Pada Drive IDE." },
  { "en": "Apa Itu SCSI?", "id": "Small Computer System Interface." },
  { "en": "Di Mana SCSI Dulu Digunakan?", "id": "Di Server Dan Workstation." },
  { "en": "Apa Itu SAS (Serial Attached SCSI)?", "id": "Penerus SCSI Yang Menggunakan Koneksi Serial." },
  { "en": "Apa Itu FireWire (IEEE 1394)?", "id": "Antarmuka Transfer Data Cepat Pesaing USB." },
  { "en": "Apakah FireWire Masih Populer?", "id": "Tidak Sudah Digantikan Oleh USB Dan Thunderbolt." },
  { "en": "Apa Itu ExpressCard Slot?", "id": "Slot Ekspansi Untuk Laptop." },
  { "en": "Apa Itu PC Card (PCMCIA)?", "id": "Standar Slot Ekspansi Laptop Sebelum ExpressCard." },
  { "en": "Apa Itu Refresh Rate Layar Laptop?", "id": "Sama Seperti Monitor Desktop." },
  { "en": "Apa Itu Layar Glossy?", "id": "Layar Dengan Permukaan Mengkilap." },
  { "en": "Apa Kelebihan Layar Glossy?", "id": "Warna Terlihat Lebih Cerah Dan Hidup." },
  { "en": "Apa Kekurangan Layar Glossy?", "id": "Sangat Memantulkan Cahaya Sekitar." },
  { "en": "Apa Itu Layar Matte (Anti-Glare)?", "id": "Layar Dengan Lapisan Anti Silau." },
  { "en": "Apa Kelebihan Layar Matte?", "id": "Nyaman Digunakan Di Tempat Terang." },
  { "en": "Apa Itu Optical Switch Keyboard?", "id": "Menggunakan Sinar Cahaya Untuk Memicu Tombol." },
  { "en": "Apa Kelebihan Optical Switch?", "id": "Lebih Cepat Dan Tahan Lama." },
  { "en": "Apa Itu Hall Effect Switch?", "id": "Menggunakan Sensor Magnetik Untuk Memicu." },
  { "en": "Apa Itu Joystick?", "id": "Perangkat Input Berbentuk Tongkat Untuk Game." },
  { "en": "Apa Itu Gamepad?", "id": "Kontroler Game Yang Dipegang Dua Tangan." },
  { "en": "Apa Itu Setir Balap (Racing Wheel)?", "id": "Kontroler Khusus Untuk Game Balap." },
  { "en": "Apa Itu Light Pen?", "id": "Perangkat Input Pena Cahaya Untuk Layar CRT." },
  { "en": "Apa Itu Plotter?", "id": "Printer Khusus Untuk Mencetak Grafik Vektor." },
  { "en": "Di Mana Plotter Digunakan?", "id": "Arsitektur Dan Desain Teknik." },
  { "en": "Apa Itu Printer Dot Matrix?", "id": "Printer Yang Menggunakan Pin Untuk Membentuk Huruf." },
  { "en": "Apa Itu Printer Inkjet?", "id": "Printer Yang Menyemprotkan Tinta Cair." },
  { "en": "Apa Itu Printer Laser?", "id": "Printer Yang Menggunakan Bubuk Toner Dan Pemanas." },
  { "en": "Manakah Yang Paling Cepat Untuk Mencetak Teks?", "id": "Printer Laser." },
  { "en": "Manakah Yang Terbaik Untuk Mencetak Foto?", "id": "Printer Inkjet Berkualitas Tinggi." },
  { "en": "Apa Itu CMYK?", "id": "Cyan Magenta Yellow Key (Black)." },
  { "en": "Di Mana Model Warna CMYK Digunakan?", "id": "Dalam Proses Pencetakan." },
  { "en": "Apa Itu Motherboard E-ATX?", "id": "Form Factor Lebih Besar Dari ATX." },
  { "en": "Apa Itu Motherboard Mini-STX?", "id": "Form Factor Sangat Kecil Dengan Soket CPU." },
  { "en": "Apa Itu Intel NUC?", "id": "Next Unit of Computing." },
  { "en": "Apa Bentuk Intel NUC?", "id": "Komputer Desktop Mini Berperforma Tinggi." },
  { "en": "Apa Itu Raspberry Pi Compute Module?", "id": "Versi Raspberry Pi Untuk Sistem Tertanam." },
  { "en": "Apa Itu Sistem Tertanam (Embedded System)?", "id": "Komputer Khusus Di Dalam Perangkat Lebih Besar." },
  { "en": "Contoh Sistem Tertanam?", "id": "Mikrokontroler Di Microwave Atau Mobil." },
  { "en": "Apa Itu FPGA?", "id": "Field-Programmable Gate Array." },
  { "en": "Apa Fungsi FPGA?", "id": "Chip Logika Yang Dapat Diprogram Ulang." },
  { "en": "Apa Itu ASIC?", "id": "Application-Specific Integrated Circuit." },
  { "en": "Apa Fungsi ASIC?", "id": "Chip Yang Dirancang Untuk Satu Tugas Khusus." },
  { "en": "Manakah Yang Lebih Cepat FPGA Atau ASIC?", "id": "ASIC Jauh Lebih Cepat Dan Efisien." },
  { "en": "Di Mana ASIC Digunakan?", "id": "Penambangan Bitcoin Dan Perangkat Jaringan." },
  { "en": "Apa Itu Komputasi Kuantum?", "id": "Menggunakan Prinsip Kuantum Untuk Komputasi." },
  { "en": "Apa Itu Qubit?", "id": "Unit Dasar Informasi Kuantum." },
  { "en": "Apa Itu Komputer Neuromorphic?", "id": "Komputer Yang Meniru Struktur Otak Manusia." },
  { "en": "Apa Itu Lithography Dalam Fabrikasi Chip?", "id": "Proses Mencetak Pola Sirkuit Ke Silikon." },
  { "en": "Apa Itu Wafer Silikon?", "id": "Lempengan Tipis Silikon Dasar Pembuatan Chip." },
  { "en": "Apa Itu Clean Room?", "id": "Ruangan Sangat Bersih Untuk Fabrikasi Chip." },
  { "en": "Mengapa Clean Room Dibutuhkan?", "id": "Satu Partikel Debu Dapat Merusak Chip." },
  { "en": "Apa Itu Yield Dalam Produksi Chip?", "id": "Persentase Chip Yang Berfungsi Dari Satu Wafer." },
  { "en": "Apa Itu Thermal Pad?", "id": "Bantalan Lembut Penghantar Panas." },
  { "en": "Di Mana Thermal Pad Digunakan?", "id": "Pada VRAM Dan VRM." },
  { "en": "Apa Beda Thermal Pad Dan Thermal Paste?", "id": "Pad Padat Paste Cair." },
  { "en": "Apa Itu Laptop Fan?", "id": "Kipas Pendingin Di Dalam Laptop." },
  { "en": "Apa Itu Vapor Chamber?", "id": "Sistem Pendingin Canggih Yang Menggunakan Penguapan." },
  { "en": "Di Mana Vapor Chamber Digunakan?", "id": "Kartu Grafis Dan Laptop High-End." },
  { "en": "Apa Itu Keyboard Chiclet?", "id": "Keyboard Dengan Tombol Terpisah Seperti Permen." },
  { "en": "Apa Itu Keyboard TKL (Tenkeyless)?", "id": "Keyboard Tanpa Bagian Numpad." },
  { "en": "Apa Itu Firmware UEFI?", "id": "Unified Extensible Firmware Interface." },
  { "en": "Apa Keuntungan UEFI Dibanding BIOS?", "id": "Mendukung Drive Besar Dan Boot Lebih Cepat." },
  { "en": "Apa Itu CSM (Compatibility Support Module)?", "id": "Fitur UEFI Untuk Kompatibilitas BIOS Lama." },
  { "en": "Apa Itu GPT Partition Style?", "id": "GUID Partition Table." },
  { "en": "Apa Kelebihan GPT Dibanding MBR?", "id": "Mendukung Lebih Banyak Partisi Dan Kapasitas." },
  { "en": "Gaya Partisi Apa Yang Digunakan UEFI?", "id": "GPT." },
  { "en": "Apa Itu MBR?", "id": "Master Boot Record." },
  { "en": "Apa Itu RTC?", "id": "Real-Time Clock." },
  { "en": "Apa Fungsi RTC?", "id": "Menjaga Waktu Sistem Tetap Berjalan." },
  { "en": "Daya Apa Yang Digunakan RTC?", "id": "Ditenagai Oleh Baterai CMOS." },
  { "en": "Apa Itu 'Case Modding'?", "id": "Memodifikasi Tampilan Casing Komputer." },
  { "en": "Apa Itu Panel Samping Akrilik?", "id": "Panel Samping Casing Transparan Dari Akrilik." },
  { "en": "Apa Itu Sleeved Cables?", "id": "Kabel PSU Dengan Selongsong Dekoratif." },
  { "en": "Apa Itu 'Cable Combs'?", "id": "Sisir Kecil Untuk Merapikan Sleeved Cables." },
  { "en": "Apa Itu 'Form Factor' PSU?", "id": "Standar Ukuran Dan Bentuk Power Supply." },
  { "en": "Sebutkan Dua Form Factor PSU Paling Umum?", "id": "ATX Dan SFX." },
  { "en": "Apa Itu 'Zero RPM Mode'?", "id": "Fitur Kipas PSU Yang Berhenti Berputar." },
  { "en": "Kapan Kipas PSU Berhenti Berputar?", "id": "Saat Beban Rendah Dan Suhu Dingin." },
  { "en": "Apa Keuntungan Zero RPM Mode?", "id": "Mengurangi Kebisingan Saat Komputer Idle." },
  { "en": "Apa Itu 'Inrush Current'?", "id": "Lonjakan Arus Awal Saat Perangkat Dinyalakan." },
  { "en": "Apa Itu 'Power Good Signal'?", "id": "Sinyal Dari PSU Ke Motherboard." },
  { "en": "Apa Arti Sinyal Power Good?", "id": "Menandakan Bahwa Tegangan Sudah Stabil." },
  { "en": "Apa Itu 'Rail' Pada PSU?", "id": "Jalur Tegangan Keluaran (Contoh: +12V Rail)." },
  { "en": "Apa Itu Single-Rail PSU?", "id": "PSU Dengan Satu Jalur +12V Kuat." },
  { "en": "Apa Itu Multi-Rail PSU?", "id": "PSU Dengan Beberapa Jalur +12V Terpisah." },
  { "en": "Manakah Yang Umumnya Lebih Aman?", "id": "Multi-Rail Dengan Proteksi Arus Berlebih." },
  { "en": "Apa Itu OCP (Over Current Protection)?", "id": "Proteksi Terhadap Arus Yang Berlebihan." },
  { "en": "Apa Itu OVP (Over Voltage Protection)?", "id": "Proteksi Terhadap Tegangan Yang Berlebihan." },
  { "en": "Apa Itu SCP (Short Circuit Protection)?", "id": "Proteksi Terhadap Hubung Singkat." },
  { "en": "Apa Itu 'Ripple' Pada PSU?", "id": "Fluktuasi Kecil Pada Tegangan Keluaran DC." },
  { "en": "Apakah Ripple Diinginkan?", "id": "Tidak Semakin Rendah Ripple Semakin Baik." },
  { "en": "Apa Itu 'Hold-Up Time'?", "id": "Waktu PSU Bertahan Setelah Listrik AC Mati." },
  { "en": "Apa Itu 'Power Factor Correction' (PFC)?", "id": "Meningkatkan Efisiensi Penggunaan Daya Listrik." },
  { "en": "Apa Itu PFC Aktif?", "id": "Sirkuit Elektronik Untuk Koreksi Faktor Daya." },
  { "en": "Apakah PSU Modern Menggunakan PFC Aktif?", "id": "Ya Hampir Semua PSU Modern." },
  { "en": "Apa Itu 'Coil Whine'?", "id": "Suara Mendenging Frekuensi Tinggi Dari Komponen." },
  { "en": "Komponen Apa Yang Sering Mengalami Coil Whine?", "id": "Kartu Grafis Dan Power Supply." },
  { "en": "Apakah Coil Whine Berbahaya?", "id": "Tidak Berbahaya Tapi Bisa Mengganggu." },
  { "en": "Apa Itu 'Burn-in Test'?", "id": "Menguji Komponen Baru Dengan Beban Penuh." },
  { "en": "Apa Tujuan Burn-in Test?", "id": "Mendeteksi Kegagalan Dini Komponen." },
  { "en": "Apa Itu 'Mean Time Between Failures' (MTBF)?", "id": "Waktu Rata-Rata Antara Kegagalan." },
  { "en": "Apa Yang Ditunjukkan MTBF Tinggi?", "id": "Komponen Tersebut Sangat Andal." },
  { "en": "Apa Itu 'Endurance Rating' SSD?", "id": "Jumlah Data Yang Bisa Ditulis Seumur Hidup." },
  { "en": "Apa Satuan Endurance Rating?", "id": "Terabytes Written (TBW)." },
  { "en": "Apa Itu 'Garbage Collection' Pada SSD?", "id": "Proses Internal Untuk Mengoptimalkan Ruang." },
  { "en": "Apa Itu 'DRAM Cache' Pada SSD?", "id": "Cache RAM Cepat Untuk Mempercepat Kinerja." },
  { "en": "Apa Itu SSD 'DRAM-less'?", "id": "SSD Tanpa DRAM Cache." },
  { "en": "Bagaimana Kinerja SSD DRAM-less?", "id": "Umumnya Lebih Lambat Untuk Tugas Berat." },
  { "en": "Apa Itu Memori SLC, MLC, TLC, QLC?", "id": "Jenis-Jenis Sel Memori Flash NAND." },
  { "en": "Apa Kepanjangan SLC?", "id": "Single-Level Cell." },
  { "en": "Apa Kelebihan SLC?", "id": "Paling Cepat Dan Paling Tahan Lama." },
  { "en": "Apa Kepanjangan QLC?", "id": "Quad-Level Cell." },
  { "en": "Apa Kelebihan QLC?", "id": "Kapasitas Sangat Besar Dengan Harga Murah." },
  { "en": "Manakah Yang Paling Umum Di SSD Konsumen?", "id": "TLC (Triple-Level Cell) Dan QLC." },
  { "en": "Apa Itu '3D NAND'?", "id": "Teknologi Menumpuk Sel Memori Secara Vertikal." },
  { "en": "Apa Keuntungan 3D NAND?", "id": "Meningkatkan Kepadatan Dan Kinerja." },
  { "en": "Apa Itu 'System on Module' (SoM)?", "id": "Mirip SBC Tapi Perlu Papan Tambahan." },
  { "en": "Apa Itu 'Power over Ethernet' (PoE)?", "id": "Menyalurkan Daya Listrik Melalui Kabel Ethernet." },
  { "en": "Perangkat Apa Yang Menggunakan PoE?", "id": "Kamera IP Dan Titik Akses Nirkabel." },
  { "en": "Apa Itu 'Powerline Adapter'?", "id": "Menggunakan Jaringan Listrik Rumah Sebagai Jaringan Data." },
  { "en": "Apa Itu 'Mesh Wi-Fi System'?", "id": "Sistem Wi-Fi Dengan Beberapa Titik Akses." },
  { "en": "Apa Keuntungan Mesh Wi-Fi?", "id": "Jangkauan Sinyal Lebih Luas Dan Stabil." },
  { "en": "Apa Itu 'Wi-Fi Extender'?", "id": "Memperluas Jangkauan Sinyal Wi-Fi Yang Ada." },
  { "en": "Apa Beda Mesh Dan Extender?", "id": "Mesh Menciptakan Satu Jaringan Extender Jaringan Terpisah." },
  { "en": "Apa Itu 'Band Steering'?", "id": "Fitur Router Memindahkan Perangkat Antar Frekuensi." },
  { "en": "Frekuensi Wi-Fi Apa Yang Umum?", "id": "2.4 GHz Dan 5 GHz." },
  { "en": "Manakah Yang Jangkauannya Lebih Jauh?", "id": "2.4 GHz." },
  { "en": "Manakah Yang Kecepatannya Lebih Tinggi?", "id": "5 GHz." },
  { "en": "Apa Itu 'Channel' Wi-Fi?", "id": "Pita Frekuensi Sempit Yang Digunakan Router." },
  { "en": "Mengapa Memilih Channel Yang Tepat Penting?", "id": "Untuk Menghindari Interferensi Dengan Jaringan Lain." },
  { "en": "Apa Itu 'Heat Spreader' RAM?", "id": "Selubung Logam Dekoratif Pada Modul RAM." },
  { "en": "Apa Fungsi Sebenarnya Heat Spreader RAM?", "id": "Membantu Melepas Sedikit Panas." },
  { "en": "Apakah RAM Standar Membutuhkan Heat Spreader?", "id": "Tidak Sebenarnya Lebih Untuk Estetika." },
  { "en": "Apa Itu 'Binned' RAM?", "id": "Kit RAM Yang Diuji Untuk Bekerja Bersama." },
  { "en": "Apa Itu Laptop 'Thin and Light'?", "id": "Kategori Laptop Tipis Dan Ringan." },
  { "en": "Apa Itu '2-in-1' Laptop?", "id": "Nama Lain Untuk Laptop Convertible." },
  { "en": "Apa Itu 'Fanless' Laptop?", "id": "Laptop Tanpa Kipas Pendingin." },
  { "en": "Bagaimana Laptop Fanless Didinginkan?", "id": "Melalui Pendinginan Pasif." },
  { "en": "CPU Jenis Apa Yang Digunakan Laptop Fanless?", "id": "CPU Berdaya Sangat Rendah." },
  { "en": "Apa Itu 'eGPU'?", "id": "External Graphics Processing Unit." },
  { "en": "Apa Fungsi eGPU?", "id": "Menambah Kemampuan Grafis Laptop." },
  { "en": "Bagaimana eGPU Terhubung Ke Laptop?", "id": "Melalui Port Thunderbolt." },
  { "en": "Apa Itu 'Proprietary Connector'?", "id": "Konektor Khusus Yang Dibuat Satu Perusahaan." },
  { "en": "Contoh Proprietary Connector?", "id": "Port Lightning Apple." },
  { "en": "Apa Itu 'Legacy Port'?", "id": "Port Lama Yang Sudah Jarang Digunakan." },
  { "en": "Contoh Legacy Port?", "id": "Port Serial Paralel Dan PS/2." },
  { "en": "Apa Itu 'Digital Crown'?", "id": "Tombol Putar Di Apple Watch." },
  { "en": "Apa Itu 'Taptic Engine'?", "id": "Sistem Haptic Feedback Canggih Dari Apple." },
  { "en": "Apa Itu 'Liquid Cooling Loop'?", "id": "Sirkuit Tertutup Cairan Dalam Sistem Pendingin." },
  { "en": "Cairan Apa Yang Digunakan?", "id": "Cairan Khusus Atau Air Suling." },
  { "en": "Apa Itu 'Reservoir'?", "id": "Wadah Untuk Menampung Cairan Pendingin." },
  { "en": "Apa Itu 'Pump'?", "id": "Pompa Untuk Mensirkulasikan Cairan Pendingin." },
  { "en": "Apa Itu 'Radiator'?", "id": "Komponen Untuk Melepas Panas Dari Cairan." },
  { "en": "Apa Itu 'Fittings'?", "id": "Konektor Untuk Menghubungkan Tabung." },
  { "en": "Apa Itu 'Hard Tubing'?", "id": "Tabung Keras Dari Akrilik Atau PETG." },
  { "en": "Apa Itu 'Soft Tubing'?", "id": "Tabung Fleksibel Dari PVC Atau Silikon." },
  { "en": "Manakah Yang Lebih Mudah Dipasang?", "id": "Soft Tubing." },
  { "en": "Apa Itu 'Daisy Chaining'?", "id": "Menghubungkan Beberapa Perangkat Secara Berantai." },
  { "en": "Perangkat Apa Yang Mendukung Daisy Chaining?", "id": "Monitor Thunderbolt Dan Beberapa Monitor DisplayPort." },
  { "en": "Apa Itu 'BIOS Beep Code'?", "id": "Nama Lain Untuk Kode Beep POST." },
  { "en": "Satu Beep Pendek Biasanya Berarti Apa?", "id": "Semua Sistem Normal Dan Siap Booting." },
  { "en": "Apa Itu 'CMOS Checksum Error'?", "id": "Kesalahan Yang Menunjukkan Data BIOS Rusak." },
  { "en": "Penyebab Umum CMOS Checksum Error?", "id": "Baterai CMOS Lemah Atau Gagal." },
  { "en": "Apa Itu 'Peripheral Device'?", "id": "Istilah Lain Untuk Perangkat Periferal." },
  { "en": "Apa Itu 'Input Lag'?", "id": "Penundaan Antara Input Dan Respons Di Layar." },
  { "en": "Di Mana Input Lag Paling Terasa?", "id": "Dalam Game Online Kompetitif." },
  { "en": "Faktor Apa Yang Menyebabkan Input Lag?", "id": "Monitor Keyboard Mouse Dan Jaringan." },
  { "en": "Apa Itu 'Polling Rate'?", "id": "Seberapa Sering Perangkat Melaporkan Posisinya." },
  { "en": "Apa Satuan Polling Rate?", "id": "Hertz (Hz)." },
  { "en": "Polling Rate Mouse Gaming Umumnya Berapa?", "id": "1000 Hz." },
  { "en": "Apa Itu 'Actuation Point' Keyboard?", "id": "Titik Di Mana Tombol Mulai Mendaftar." },
  { "en": "Apa Itu 'Key Travel'?", "id": "Jarak Total Pergerakan Tombol Dari Atas Ke Bawah." },
  { "en": "Apa Itu 'Bottoming Out'?", "id": "Menekan Tombol Keyboard Sampai Mendasar." },
  { "en": "Apa Itu 'Scan Rate' Keyboard?", "id": "Seberapa Cepat Keyboard Memindai Penekanan Tombol." },
  { "en": "Apa Itu 'Debounce Delay'?", "id": "Penundaan Singkat Untuk Menghindari Penekanan Ganda." },
  { "en": "Apa Itu 'Ergonomic Keyboard'?", "id": "Keyboard Yang Didesain Untuk Posisi Tangan Alami." },
  { "en": "Apa Itu 'Split Keyboard'?", "id": "Keyboard Yang Terbelah Menjadi Dua Bagian." },
  { "en": "Apa Itu 'Ortholinear Keyboard'?", "id": "Keyboard Dengan Tombol Tersusun Dalam Grid Lurus." },
  { "en": "Apa Itu 'Mouse Bungee'?", "id": "Penyangga Untuk Mengatur Kabel Mouse." },
  { "en": "Apa Itu 'Mouse Skates' Atau 'Feet'?", "id": "Bantalan Licin Di Bawah Mouse." },
  { "en": "Bahan Apa Yang Digunakan Untuk Mouse Skates?", "id": "PTFE (Teflon)." },
  { "en": "Apa Itu 'Lift-Off Distance' (LOD)?", "id": "Tinggi Mouse Berhenti Melacak Saat Diangkat." },
  { "en": "Mengapa LOD Rendah Diinginkan Gamer?", "id": "Mencegah Kursor Bergerak Saat Mengatur Ulang Mouse." },
  { "en": "Apa Itu 'Angle Snapping'?", "id": "Fitur Mouse Yang Membantu Membuat Garis Lurus." },
  { "en": "Apa Itu 'Acceleration' Mouse?", "id": "Kursor Bergerak Lebih Jauh Saat Mouse Digeser Cepat." },
  { "en": "Apa Itu 'Pixel Pitch'?", "id": "Jarak Antara Pusat Dua Piksel." },
  { "en": "Semakin Kecil Pixel Pitch Berarti Apa?", "id": "Gambar Semakin Tajam Dan Padat." },
  { "en": "Apa Itu 'Color Depth'?", "id": "Jumlah Bit Yang Digunakan Untuk Setiap Warna." },
  { "en": "Apa Itu '8-bit Color'?", "id": "Dapat Menampilkan Sekitar 16.7 Juta Warna." },
  { "en": "Apa Itu '10-bit Color'?", "id": "Dapat Menampilkan Lebih Dari 1 Miliar Warna." },
  { "en": "Apa Itu 'Local Dimming'?", "id": "Fitur Backlight Untuk Meningkatkan Kontras." },
  { "en": "Bagaimana Cara Kerja Local Dimming?", "id": "Mematikan LED Di Area Gelap Gambar." },
  { "en": "Apa Itu 'VESA Mount'?", "id": "Standar Pemasangan Monitor Di Dinding Atau Lengan." },
  { "en": "Apa Itu 'Monitor Arm'?", "id": "Lengan Fleksibel Untuk Memegang Monitor." },
  { "en": "Apa Itu 'Cable Tidy' Atau 'Sleeve'?", "id": "Selongsong Untuk Merapikan Kumpulan Kabel." },
  { "en": "Apa Itu 'Grommet Hole'?", "id": "Lubang Di Meja Untuk Jalur Kabel." },
  { "en": "Apa Itu 'Under-Desk Cable Management Tray'?", "id": "Nampan Di Bawah Meja Untuk Kabel." },
  { "en": "Apa Itu 'Power Strip'?", "id": "Stop Kontak Tambahan Dalam Satu Baris." },
  { "en": "Apa Beda Power Strip Dan Surge Protector?", "id": "Surge Protector Memberikan Perlindungan Lonjakan." },
  { "en": "Apa Itu 'Battery Backup'?", "id": "Nama Lain Untuk UPS." },
  { "en": "Apa Itu 'Form Factor' Hard Drive?", "id": "Ukuran Fisik Hard Drive." },
  { "en": "Berapa Ukuran HDD Desktop?", "id": "3.5 Inci." },
  { "en": "Berapa Ukuran HDD Laptop?", "id": "2.5 Inci." },
  { "en": "Apa Itu 'External Hard Drive'?", "id": "Hard Drive Dalam Casing Portabel." },
  { "en": "Bagaimana External Hard Drive Terhubung?", "id": "Biasanya Melalui Kabel USB." },
  { "en": "Apa Itu 'Flash Drive'?", "id": "Perangkat Penyimpanan Portabel Berbasis Flash." },
  { "en": "Apa Nama Lain Flash Drive?", "id": "USB Drive Thumb Drive." },
  { "en": "Apa Itu 'Write Speed'?", "id": "Kecepatan Menulis Data Ke Perangkat." },
  { "en": "Apa Itu 'Read Speed'?", "id": "Kecepatan Membaca Data Dari Perangkat." },
  { "en": "Apa Itu 'Sequential Read/Write'?", "id": "Membaca Atau Menulis File Besar Berurutan." },
  { "en": "Apa Itu 'Random Read/Write'?", "id": "Membaca Atau Menulis File Kecil Acak." },
  { "en": "Kinerja Apa Yang Penting Untuk Booting OS?", "id": "Kecepatan Baca Acak (Random Read)." },
  { "en": "Apa Itu 'IOPS'?", "id": "Input/Output Operations Per Second." },
  { "en": "Apa Yang Diukur IOPS?", "id": "Kinerja Untuk File-File Kecil." },
  { "en": "Apa Itu 'Server Rack'?", "id": "Kerangka Standar Untuk Memasang Peralatan Server." },
  { "en": "Apa Satuan Tinggi Dalam Server Rack?", "id": "Unit (U)." },
  { "en": "Berapa Tinggi Satu Unit (1U)?", "id": "1.75 Inci." },
  { "en": "Apa Itu 'Rackmount Server'?", "id": "Server Yang Didesain Untuk Dipasang Di Rak." },
  { "en": "Apa Itu 'Blade Server'?", "id": "Server Modular Sangat Tipis." },
  { "en": "Apa Itu 'Blade Enclosure'?", "id": "Sasis Yang Menampung Banyak Blade Server." },
  { "en": "Apa Itu 'Datacenter'?", "id": "Fasilitas Besar Tempat Menyimpan Banyak Server." },
  { "en": "Apa Itu 'Colocation'?", "id": "Menyewa Ruang Di Datacenter." },
  { "en": "Apa Itu 'Cloud Server'?", "id": "Server Virtual Yang Dijalankan Di Cloud." },
  { "en": "Apa Itu 'Embedded PC'?", "id": "PC Industri Tanpa Kipas Untuk Aplikasi Khusus." },
  { "en": "Apa Itu 'Panel PC'?", "id": "PC Dengan Monitor Terintegrasi Untuk Industri." },
  { "en": "Apa Itu 'IP Rating'?", "id": "Standar Tingkat Proteksi Terhadap Debu Dan Air." },
  { "en": "Apa Arti IP65?", "id": "Tahan Debu Dan Semburan Air." },
  { "en": "Apa Itu 'Capacitive Touchscreen'?", "id": "Layar Sentuh Yang Merespons Konduktivitas Jari." },
  { "en": "Apa Itu 'Resistive Touchscreen'?", "id": "Layar Sentuh Yang Merespons Tekanan." },
  { "en": "Manakah Yang Lebih Umum Di Smartphone Modern?", "id": "Capacitive Touchscreen." },
  { "en": "Apa Itu 'Gorilla Glass'?", "id": "Merek Kaca Tahan Gores Untuk Perangkat." },
  { "en": "Apa Itu 'Oleophobic Coating'?", "id": "Lapisan Anti Minyak Sidik Jari." },
  { "en": "Apa Itu 'Ambient Light Sensor'?", "id": "Sensor Untuk Mengatur Kecerahan Layar Otomatis." },
  { "en": "Apa Itu 'Proximity Sensor'?", "id": "Sensor Untuk Mematikan Layar Saat Telepon." },
  { "en": "Apa Itu 'Accelerometer'?", "id": "Sensor Untuk Mendeteksi Gerakan Dan Orientasi." },
  { "en": "Apa Itu 'Gyroscope'?", "id": "Sensor Untuk Mendeteksi Rotasi Perangkat." },
  { "en": "Apa Itu 'Magnetometer'?", "id": "Sensor Yang Berfungsi Sebagai Kompas Digital." },
  { "en": "Apa Itu 'GPS Receiver'?", "id": "Penerima Sinyal Untuk Menentukan Lokasi." },
  { "en": "Apa Itu 'NFC'?", "id": "Near Field Communication." },
  { "en": "Apa Fungsi NFC?", "id": "Komunikasi Jarak Sangat Dekat Untuk Pembayaran." },
  { "en": "Apa Itu 'Qi Wireless Charging'?", "id": "Standar Pengisian Daya Nirkabel Induktif." },
  { "en": "Apa Itu 'Lithography'?", "id": "Proses Pencetakan Sirkuit Pada Chip." },
  { "en": "Apa Itu 'Wafer'?", "id": "Lempengan Silikon Dasar Pembuatan Chip." },
  { "en": "Apa Itu 'Die'?", "id": "Satu Potongan Chip Dari Wafer." },
  { "en": "Apa Itu 'Packaging' Chip?", "id": "Proses Membungkus Die Menjadi Chip Final." },
  { "en": "Apa Itu 'Pin' Chip?", "id": "Kaki-Kaki Logam Untuk Koneksi." },
  { "en": "Apa Itu 'Socket'?", "id": "Konektor Tempat Memasang Chip." },
  { "en": "Apa Itu 'Trace' Pada PCB?", "id": "Jalur Tembaga Konduktif Di Papan Sirkuit." },
  { "en": "Apa Itu 'Via' Pada PCB?", "id": "Lubang Konduktif Yang Menghubungkan Lapisan PCB." },
  { "en": "Apa Itu 'Breadboard'?", "id": "Papan Untuk Merakit Prototipe Sirkuit Sementara." },
  { "en": "Apa Itu 'Jumper Wire'?", "id": "Kabel Untuk Menghubungkan Komponen Di Breadboard." },
  { "en": "Apa Itu 'Multimeter'?", "id": "Alat Ukur Listrik Serbaguna." },
  { "en": "Apa Yang Bisa Diukur Multimeter?", "id": "Tegangan Arus Dan Resistansi." },
  { "en": "Apa Itu 'Oscilloscope'?", "id": "Alat Untuk Melihat Bentuk Sinyal Listrik." },
  { "en": "Apa Itu 'Power State'?", "id": "Tingkat Konsumsi Daya Komputer." },
  { "en": "Apa Itu Mode 'Sleep'?", "id": "Menyimpan Sesi Kerja Ke RAM." },
  { "en": "Apa Keuntungan Mode Sleep?", "id": "Proses Bangun Kembali Sangat Cepat." },
  { "en": "Apa Itu Mode 'Hibernate'?", "id": "Menyimpan Sesi Kerja Ke Hard Drive." },
  { "en": "Apa Keuntungan Mode Hibernate?", "id": "Tidak Mengonsumsi Listrik Sama Sekali." },
  { "en": "Manakah Yang Lebih Cepat Bangun?", "id": "Mode Sleep." },
  { "en": "Apa Itu 'Hybrid Sleep'?", "id": "Gabungan Mode Sleep Dan Hibernate." },
  { "en": "Apa Itu 'Wake-on-LAN'?", "id": "Menghidupkan Komputer Dari Jarak Jauh." },
  { "en": "Apa Yang Dibutuhkan Untuk Wake-on-LAN?", "id": "Koneksi Jaringan Kabel Dan Pengaturan BIOS." },
  { "en": "Apa Itu 'Case Fan'?", "id": "Kipas Yang Dipasang Di Casing." },
  { "en": "Apa Itu 'Intake Fan'?", "id": "Kipas Yang Menarik Udara Dingin Masuk." },
  { "en": "Apa Itu 'Exhaust Fan'?", "id": "Kipas Yang Mendorong Udara Panas Keluar." },
  { "en": "Di Mana Posisi Umum Intake Fan?", "id": "Di Bagian Depan Atau Bawah Casing." },
  { "en": "Di Mana Posisi Umum Exhaust Fan?", "id": "Di Bagian Belakang Atau Atas Casing." },
  { "en": "Apa Itu 'Fan Curve'?", "id": "Grafik Pengaturan Kecepatan Kipas Berdasarkan Suhu." },
  { "en": "Di Mana Fan Curve Diatur?", "id": "Di Dalam BIOS Atau Perangkat Lunak." },
  { "en": "Apa Itu 'Bearing' Kipas?", "id": "Mekanisme Yang Membuat Kipas Berputar Halus." },
  { "en": "Apa Jenis Bearing Kipas Yang Paling Umum?", "id": "Sleeve Bearing Dan Ball Bearing." },
  { "en": "Manakah Yang Lebih Tahan Lama?", "id": "Ball Bearing." },
  { "en": "Manakah Yang Lebih Hening?", "id": "Sleeve Bearing Awalnya Tapi Ball Bearing Konsisten." },
  { "en": "Apa Itu 'Fluid Dynamic Bearing' (FDB)?", "id": "Jenis Bearing Canggih Yang Hening Dan Awet." },
  { "en": "Apa Itu 'Magnetic Levitation Fan'?", "id": "Kipas Yang Menggunakan Magnet Untuk Melayang." },
  { "en": "Apa Itu 'Cable Tie' Atau 'Zip Tie'?", "id": "Pengikat Plastik Untuk Merapikan Kabel." },
  { "en": "Apa Itu 'Velcro Strap'?", "id": "Tali Perekat Untuk Merapikan Kabel." },
  { "en": "Manakah Yang Dapat Digunakan Kembali?", "id": "Velcro Strap." },
  { "en": "Apa Itu 'PSU Tester'?", "id": "Alat Untuk Menguji Tegangan Keluaran PSU." },
  { "en": "Apa Itu 'Thermal Grizzly Kryonaut'?", "id": "Merek Thermal Paste Berkinerja Tinggi." },
  { "en": "Kapan Perlu Mengganti Thermal Paste?", "id": "Saat Suhu CPU Meningkat Atau Ganti Pendingin." },
  { "en": "Apa Itu 'Integrated Graphics'?", "id": "Nama Lain Untuk GPU Terintegrasi." },
  { "en": "Apa Itu 'Discrete Graphics'?", "id": "Nama Lain Untuk GPU Diskrit." },
  { "en": "Apa Itu 'Low-Profile' GPU?", "id": "Kartu Grafis Dengan Ukuran PCB Rendah." },
  { "en": "Di Mana Low-Profile GPU Digunakan?", "id": "Di Casing Komputer Tipis (Slim)." },
  { "en": "Apa Itu 'Single-Slot' GPU?", "id": "GPU Yang Hanya Memakan Satu Slot Ekspansi." },
  { "en": "Apa Itu 'Dual-Slot' GPU?", "id": "GPU Yang Memakan Dua Slot Ekspansi." },
  { "en": "Mengapa GPU Modern Berukuran Besar?", "id": "Membutuhkan Sistem Pendingin Yang Besar." },
  { "en": "Apa Itu 'Founders Edition' (FE)?", "id": "Versi Kartu Grafis Referensi Dari NVIDIA." },
  { "en": "Apa Itu Kartu 'AIB' (Add-in Board)?", "id": "Kartu Grafis Dari Merek Pihak Ketiga." },
  { "en": "Contoh Merek AIB?", "id": "ASUS MSI Gigabyte." },
  { "en": "Apa Beda Kartu Founders Edition Dan AIB?", "id": "Kartu AIB Sering Punya Pendingin Kustom." },
  { "en": "Apa Itu 'GPU Binning'?", "id": "Sama Seperti Binning CPU Tapi Untuk Chip GPU." },
  { "en": "Apa Itu 'VBIOS' Atau 'Video BIOS'?", "id": "Firmware Untuk Kartu Grafis." },
  { "en": "Apa Itu 'Headless' System?", "id": "Komputer Atau Server Tanpa Monitor." },
  { "en": "Bagaimana Cara Mengakses Headless System?", "id": "Melalui Jaringan (Remote Desktop Atau SSH)." },
  { "en": "Apa Itu 'Dongle'?", "id": "Adaptor Kecil Yang Menambah Fungsi." },
  { "en": "Contoh Dongle?", "id": "Adaptor USB ke Ethernet." },
  { "en": "Apa Itu 'DRAM'?", "id": "Dynamic Random Access Memory." },
  { "en": "Mengapa Disebut 'Dynamic'?", "id": "Karena Perlu Di-Refresh Terus Menerus." },
  { "en": "Apa Itu 'SRAM'?", "id": "Static Random Access Memory." },
  { "en": "Mengapa Disebut 'Static'?", "id": "Tidak Perlu Di-Refresh Selama Ada Daya." },
  { "en": "Manakah Yang Lebih Cepat DRAM Atau SRAM?", "id": "SRAM Jauh Lebih Cepat." },
  { "en": "Di Mana SRAM Digunakan?", "id": "Sebagai Memori Cache Di Dalam CPU." },
  { "en": "Di Mana DRAM Digunakan?", "id": "Sebagai Memori Utama Sistem (RAM)." },
  { "en": "Apa Itu 'Memory Controller'?", "id": "Sirkuit Yang Mengelola Aliran Data Ke RAM." },
  { "en": "Di Mana Letak Memory Controller Modern?", "id": "Terintegrasi Di Dalam CPU." },
  { "en": "Apa Itu 'DIMM Slot'?", "id": "Nama Lengkap Untuk Slot RAM." },
  { "en": "Apa Itu 'Memory Rank'?", "id": "Blok Data Independen Pada Modul RAM." },
  { "en": "Apa Itu 'Single-Rank' RAM?", "id": "Modul RAM Dengan Satu Set Chip." },
  { "en": "Apa Itu 'Dual-Rank' RAM?", "id": "Modul RAM Dengan Dua Set Chip." },
  { "en": "Manakah Yang Kinerjanya Sedikit Lebih Baik?", "id": "Dual-Rank Seringkali Sedikit Lebih Cepat." },
  { "en": "Apa Itu 'Lithium-Ion' Battery?", "id": "Jenis Baterai Isi Ulang Paling Umum." },
  { "en": "Apa Itu 'Battery Cycle'?", "id": "Satu Siklus Pengisian Penuh Dan Pengosongan." },
  { "en": "Bagaimana Cara Merawat Baterai Laptop?", "id": "Hindari Panas Berlebih Dan Pengosongan Penuh." },
  { "en": "Apa Itu 'Calibration' Baterai?", "id": "Mengkalibrasi Ulang Sensor Pengukur Baterai." },
  { "en": "Apa Itu 'Power Bank'?", "id": "Baterai Portabel Untuk Mengisi Daya Perangkat." },
  { "en": "Apa Itu 'Laptop Charger'?", "id": "Adaptor Daya Untuk Laptop." },
  { "en": "Apa Itu 'Universal Charger'?", "id": "Charger Dengan Banyak Ujung Konektor." },
  { "en": "Apa Itu 'USB Hub'?", "id": "Perangkat Untuk Menambah Jumlah Port USB." },
  { "en": "Apa Itu 'Powered USB Hub'?", "id": "USB Hub Dengan Adaptor Daya Sendiri." },
  { "en": "Kapan Powered USB Hub Dibutuhkan?", "id": "Saat Menghubungkan Perangkat Yang Butuh Daya." },
  { "en": "Apa Itu 'Card Reader'?", "id": "Pembaca Kartu Memori." },
  { "en": "Apa Itu 'CompactFlash' (CF)?", "id": "Jenis Kartu Memori Lama Untuk Kamera." },
  { "en": "Apa Itu 'SD Card'?", "id": "Secure Digital Card." },
  { "en": "Apa Beda SD Dan MicroSD?", "id": "MicroSD Berukuran Jauh Lebih Kecil." },
  { "en": "Apa Itu 'Speed Class' Kartu Memori?", "id": "Menunjukkan Kecepatan Tulis Minimum." },
  { "en": "Apa Itu 'UHS'?", "id": "Ultra High Speed." },
  { "en": "Apa Itu 'eMMC' Storage?", "id": "Embedded MultiMediaCard." },
  { "en": "Di Mana eMMC Digunakan?", "id": "Smartphone Dan Laptop Murah." },
  { "en": "Bagaimana Kinerja eMMC Dibanding SSD?", "id": "eMMC Jauh Lebih Lambat Dari SSD." },
  { "en": "Apa Itu 'Chip Creep'?", "id": "Chip Perlahan Keluar Dari Soketnya." },
  { "en": "Apa Penyebab Chip Creep?", "id": "Siklus Panas Dan Dingin Berulang." },
  { "en": "Apa Itu 'Gold Fingers'?", "id": "Konektor Berlapis Emas Pada RAM Atau Kartu." },
  { "en": "Mengapa Konektor Dilapisi Emas?", "id": "Karena Emas Tidak Korosi." },
  { "en": "Apa Itu 'Data Cable'?", "id": "Kabel Untuk Mentransfer Data." },
  { "en": "Apa Itu 'Power Cable'?", "id": "Kabel Untuk Menyalurkan Daya." },
  { "en": "Apa Itu 'Shielding' Pada Kabel?", "id": "Lapisan Pelindung Untuk Mencegah Interferensi." },
  { "en": "Apa Itu 'Electromagnetic Interference' (EMI)?", "id": "Gangguan Elektromagnetik Dari Perangkat Lain." },
  { "en": "Apa Itu 'Ferrite Bead'?", "id": "Benjolan Pada Kabel Untuk Menekan Noise." },
  { "en": "Apa Itu 'Motherboard Tray'?", "id": "Pelat Di Casing Tempat Motherboard Dipasang." },
  { "en": "Apa Itu 'PSU Shroud'?", "id": "Penutup Di Dalam Casing Untuk PSU." },
  { "en": "Apa Itu 'Drive Sled' Atau 'Tray'?", "id": "Wadah Untuk Memasang Drive Tanpa Sekrup." },
  { "en": "Apa Itu 'Tool-less Design'?", "id": "Desain Casing Yang Tidak Memerlukan Obeng." },
  { "en": "Apa Itu 'Thumbscrew'?", "id": "Sekrup Yang Bisa Diputar Dengan Tangan." },
  { "en": "Apa Itu 'Expansion Slot Cover'?", "id": "Penutup Slot Ekspansi Yang Tidak Terpakai." },
  { "en": "Mengapa Slot Perlu Ditutup?", "id": "Menjaga Aliran Udara Dan Mencegah Debu." },
  { "en": "Apa Itu 'Cable Grommet'?", "id": "Karet Pelindung Di Lubang Jalur Kabel." },
  { "en": "Apa Itu 'Front Panel Audio Connector'?", "id": "Konektor Untuk Port Audio Depan Casing." },
  { "en": "Apa Itu 'Front Panel USB Connector'?", "id": "Konektor Untuk Port USB Depan Casing." },
  { "en": "Apa Itu 'Northbridge'?", "id": "Chipset Lama Yang Menghubungkan CPU Ke Komponen Cepat." },
  { "en": "Komponen Apa Yang Terhubung Ke Northbridge?", "id": "RAM Dan Kartu Grafis (AGP/PCIe)." },
  { "en": "Apa Itu 'Southbridge'?", "id": "Chipset Lama Yang Menghubungkan Komponen Lambat." },
  { "en": "Komponen Apa Yang Terhubung Ke Southbridge?", "id": "USB SATA Dan Slot PCI." },
  { "en": "Di Mana Fungsi Northbridge Sekarang?", "id": "Sudah Terintegrasi Langsung Ke Dalam CPU." },
  { "en": "Apa Nama Modern Untuk Southbridge?", "id": "Platform Controller Hub (PCH) Di Platform Intel." },
  { "en": "Apa Itu 'Bus Speed'?", "id": "Kecepatan Transfer Data Antar Komponen." },
  { "en": "Apa Itu 'Front Side Bus' (FSB)?", "id": "Bus Lama Antara CPU Dan Northbridge." },
  { "en": "Teknologi Apa Yang Menggantikan FSB?", "id": "HyperTransport (AMD) Dan QuickPath Interconnect (Intel)." },
  { "en": "Apa Itu 'Printed Circuit Board' (PCB)?", "id": "Papan Dasar Untuk Semua Sirkuit Elektronik." },
  { "en": "Berapa Lapisan PCB Pada Motherboard Modern?", "id": "Bisa Mencapai Enam Hingga Dua Belas Lapisan." },
  { "en": "Apa Itu 'Trace' Pada PCB?", "id": "Jalur Tembaga Yang Berfungsi Sebagai Kabel." },
  { "en": "Apa Itu 'Form Factor' Casing?", "id": "Standar Ukuran Casing Komputer." },
  { "en": "Sebutkan Tiga Form Factor Casing Paling Umum?", "id": "Full Tower Mid Tower Dan Mini Tower." },
  { "en": "Apa Itu 'Optical Drive Bay'?", "id": "Slot Di Casing Untuk Drive CD/DVD." },
  { "en": "Apa Ukuran Standar Optical Drive Bay?", "id": "5.25 Inci." },
  { "en": "Apa Itu 'Hard Drive Cage'?", "id": "Kerangka Di Casing Untuk Memasang HDD." },
  { "en": "Apa Itu 'PSU Shroud'?", "id": "Penutup Di Casing Untuk Menyembunyikan PSU." },
  { "en": "Apa Itu 'Cable Grommet'?", "id": "Lubang Berlapis Karet Untuk Jalur Kabel." },
  { "en": "Apa Itu 'Thumbscrew'?", "id": "Sekrup Yang Bisa Dikencangkan Dengan Tangan." },
  { "en": "Apa Itu 'Riser Cable'?", "id": "Kabel Ekstensi Untuk Slot PCIe." },
  { "en": "Apa Fungsi Riser Cable?", "id": "Memungkinkan Pemasangan GPU Secara Vertikal." },
  { "en": "Apa Itu 'Addressable RGB' (ARGB)?", "id": "LED RGB Yang Setiap Lampunya Bisa Diatur." },
  { "en": "Apa Beda ARGB Dan RGB Biasa?", "id": "RGB Biasa Hanya Bisa Satu Warna Sekaligus." },
  { "en": "Berapa Tegangan Header ARGB?", "id": "5 Volt." },
  { "en": "Berapa Tegangan Header RGB Biasa?", "id": "12 Volt." },
  { "en": "Apa Itu 'Light Strip'?", "id": "Pita Fleksibel Dengan Lampu LED." },
  { "en": "Apa Itu 'Light Bar' RAM?", "id": "Lampu LED Dekoratif Di Atas Modul RAM." },
  { "en": "Apa Itu 'Infinity Mirror'?", "id": "Efek Visual Cermin Tak Terbatas." },
  { "en": "Di Perangkat Keras Apa Efek Ini Ditemukan?", "id": "Pendingin Cair AIO Dan Casing Modern." },
  { "en": "Apa Itu 'Sound Dampening Material'?", "id": "Busa Peredam Suara Di Dalam Casing." },
  { "en": "Apa Itu 'Chassis Intrusion Detection'?", "id": "Fitur BIOS Yang Mendeteksi Casing Dibuka." },
  { "en": "Apa Itu 'Case Speaker' Atau 'Buzzer'?", "id": "Speaker Kecil Untuk Kode Beep BIOS." },
  { "en": "Apa Itu 'Power Button'?", "id": "Tombol Untuk Menghidupkan Komputer." },
  { "en": "Apa Itu 'Reset Button'?", "id": "Tombol Untuk Memulai Ulang Komputer Paksa." },
  { "en": "Apa Itu 'Power LED'?", "id": "Lampu Indikator Komputer Menyala." },
  { "en": "Apa Itu 'HDD LED'?", "id": "Lampu Indikator Aktivitas Drive Penyimpanan." },
  { "en": "Apa Itu 'Konektor Panel Depan'?", "id": "Sekelompok Pin Untuk Tombol Dan LED Casing." },
  { "en": "Apa Itu 'USB Header'?", "id": "Pin Di Motherboard Untuk Port USB Depan." },
  { "en": "Apa Itu 'HD Audio Header'?", "id": "Pin Untuk Port Audio Depan Casing." },
  { "en": "Apa Itu 'Kensington Lock'?", "id": "Lubang Pengaman Untuk Mengunci Perangkat." },
  { "en": "Apa Itu 'VESA Mount'?", "id": "Standar Lubang Sekrup Di Belakang Monitor." },
  { "en": "Apa Fungsi VESA Mount?", "id": "Untuk Pemasangan Di Dinding Atau Lengan Monitor." },
  { "en": "Apa Itu 'Monitor Stand'?", "id": "Kaki Penyangga Bawaan Monitor." },
  { "en": "Apa Itu 'Cable Management Arm'?", "id": "Lengan Untuk Merapikan Kabel Di Server Rack." },
  { "en": "Apa Itu 'Server Rail Kit'?", "id": "Rel Untuk Memasang Server Ke Dalam Rak." },
  { "en": "Apa Itu 'Hot-Pluggable'?", "id": "Perangkat Yang Bisa Dicabut Pasang Saat Menyala." },
  { "en": "Apakah Perangkat USB Hot-Pluggable?", "id": "Ya Benar." },
  { "en": "Apa Itu 'Legacy Device'?", "id": "Perangkat Keras Dengan Teknologi Lama." },
  { "en": "Contoh Legacy Device?", "id": "Printer Dot Matrix Atau Floppy Drive." },
  { "en": "Apa Itu 'Floppy Disk Drive'?", "id": "Drive Untuk Membaca Disket." },
  { "en": "Apa Itu 'Zip Drive'?", "id": "Drive Disk Portabel Kapasitas Lebih Besar." },
  { "en": "Apa Itu 'SCSI Terminator'?", "id": "Resistor Di Ujung Rantai Perangkat SCSI." },
  { "en": "Apa Itu 'Lithium-Ion Polymer' (Li-Po)?", "id": "Jenis Baterai Isi Ulang Fleksibel." },
  { "en": "Apa Kelebihan Baterai Li-Po?", "id": "Bentuk Fleksibel Dan Bobot Ringan." },
  { "en": "Apa Itu 'Battery Swelling'?", "id": "Saat Baterai Menggembung Akibat Kerusakan." },
  { "en": "Apakah Baterai Yang Menggembung Aman?", "id": "Tidak Sangat Berbahaya Dan Harus Diganti." },
  { "en": "Apa Itu 'Thermal Paste Applicator'?", "id": "Alat Bantu Untuk Meratakan Thermal Paste." },
  { "en": "Metode Aplikasi Thermal Paste Apa Yang Populer?", "id": "Metode Sebutir Nasi Atau Tanda Silang." },
  { "en": "Apa Itu 'Burn-In' Layar?", "id": "Bayangan Permanen Akibat Gambar Statis." },
  { "en": "Layar Jenis Apa Yang Rentan Burn-In?", "id": "OLED Dan Plasma." },
  { "en": "Apa Itu 'Dead Pixel'?", "id": "Piksel Di Layar Yang Mati Total." },
  { "en": "Apa Itu 'Stuck Pixel'?", "id": "Piksel Yang Terjebak Di Satu Warna." },
  { "en": "Bisakah Stuck Pixel Diperbaiki?", "id": "Kadang-Kadang Bisa Dengan Perangkat Lunak." },
  { "en": "Apa Itu 'Backlight Bleed'?", "id": "Cahaya Latar Bocor Dari Tepi Layar." },
  { "en": "Pada Layar Jenis Apa Backlight Bleed Terjadi?", "id": "Layar LCD Dengan Backlight LED." },
  { "en": "Apa Itu 'IPS Glow'?", "id": "Cahaya Samar Terlihat Dari Sudut." },
  { "en": "Apa Itu 'Color Calibration'?", "id": "Proses Menyesuaikan Warna Layar Agar Akurat." },
  { "en": "Alat Apa Yang Digunakan Untuk Kalibrasi?", "id": "Colorimeter." },
  { "en": "Apa Itu 'LGA Socket Lever'?", "id": "Tuas Pengunci CPU Pada Soket Intel." },
  { "en": "Apa Itu 'CPU Retention Arm'?", "id": "Tuas Pengunci Pada Soket AMD." },
  { "en": "Apa Itu 'CPU Cover'?", "id": "Penutup Plastik Pelindung Pin Soket CPU." },
  { "en": "Apa Itu 'Anti-Static Bag'?", "id": "Kantong Khusus Untuk Melindungi Komponen Dari ESD." },
  { "en": "Apa Itu 'Heat Gun'?", "id": "Alat Pemanas Untuk Perbaikan Elektronik." },
  { "en": "Apa Itu 'Soldering Iron'?", "id": "Alat Untuk Melelehkan Timah Solder." },
  { "en": "Apa Itu 'Desoldering Pump'?", "id": "Alat Untuk Menghisap Timah Solder Cair." },
  { "en": "Apa Itu 'Flux'?", "id": "Cairan Kimia Untuk Membantu Proses Solder." },
  { "en": "Apa Itu 'Printed Circuit Board Assembly' (PCBA)?", "id": "PCB Yang Sudah Terpasang Komponen." },
  { "en": "Apa Itu 'Bill of Materials' (BOM)?", "id": "Daftar Semua Komponen Untuk Merakit Sesuatu." },
  { "en": "Apa Itu 'Gerber File'?", "id": "File Desain Standar Untuk Manufaktur PCB." },
  { "en": "Apa Itu 'Pick-and-Place Machine'?", "id": "Mesin Otomatis Untuk Memasang Komponen SMD." },
  { "en": "Apa Itu 'Reflow Oven'?", "id": "Oven Untuk Menyolder Semua Komponen SMD Sekaligus." },
  { "en": "Apa Itu 'Wave Soldering'?", "id": "Metode Solder Untuk Komponen Thru-Hole." },
  { "en": "Apa Itu 'Conformal Coating'?", "id": "Lapisan Pelindung Tipis Pada PCB." },
  { "en": "Apa Fungsi Conformal Coating?", "id": "Melindungi Dari Kelembaban Debu Dan Korosi." },
  { "en": "Apa Itu 'Potting'?", "id": "Menanam Sirkuit Sepenuhnya Dalam Resin." },
  { "en": "Apa Itu 'Reverse Engineering'?", "id": "Membongkar Perangkat Untuk Memahami Cara Kerjanya." },
  { "en": "Apa Itu 'Bill of Lading'?", "id": "Dokumen Pengiriman Barang." },
  { "en": "Apa Itu 'OEM'?", "id": "Original Equipment Manufacturer." },
  { "en": "Apa Maksud Produk OEM?", "id": "Produk Dibuat Satu Perusahaan Dijual Merek Lain." },
  { "en": "Apa Itu 'End of Life' (EOL)?", "id": "Saat Produk Tidak Lagi Diproduksi." },
  { "en": "Apa Itu 'Firmware Update'?", "id": "Memperbarui Perangkat Lunak Internal Perangkat." },
  { "en": "Apa Itu 'Bricking'?", "id": "Membuat Perangkat Gagal Total Akibat Update Firmware." },
  { "en": "Apa Itu 'Dongle'?", "id": "Perangkat Keras Kecil Untuk Otentikasi." }



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
