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

    { "en": "Menghambat dan mengatur arus listrik dalam sebuah rangkaian?", "id": "Resistor." },
    { "en": "Menyimpan muatan listrik sementara dalam bentuk medan listrik?", "id": "Kapasitor." },
    { "en": "Menyimpan energi dalam bentuk medan magnet saat dialiri arus listrik?", "id": "Induktor." },
    { "en": "Mengubah arus bolak-balik (AC) menjadi arus searah (DC)?", "id": "Dioda Penyearah." },
    { "en": "Berfungsi sebagai saklar elektronik atau penguat sinyal listrik?", "id": "Transistor." },
    { "en": "Sirkuit terpadu yang berisi jutaan komponen untuk melakukan fungsi kompleks?", "id": "IC (Integrated Circuit)." },
    { "en": "Menurunkan atau menaikkan level tegangan AC melalui induksi elektromagnetik?", "id": "Transformator (Trafo)." },
    { "en": "Menstabilkan tegangan pada nilai tertentu dalam rangkaian catu daya?", "id": "Dioda Zener." },
    { "en": "Mengubah energi listrik menjadi cahaya tampak?", "id": "LED (Light Emitting Diode)." },
    { "en": "Komponen yang nilai hambatannya dapat diubah-ubah secara manual dengan memutar poros?", "id": "Potensiometer." },
    { "en": "Menyaring atau meratakan tegangan DC yang beriak (ripple)?", "id": "Kapasitor Elektrolit (Elco)." },
    { "en": "Saklar yang dioperasikan secara elektromagnetik oleh sebuah kumparan?", "id": "Relai (Relay)." },
    { "en": "Komponen yang resistansinya menurun drastis saat terkena cahaya?", "id": "LDR (Light Dependent Resistor)." },
    { "en": "Melindungi rangkaian dari kelebihan arus dengan cara putus?", "id": "Sekring (Fuse)." },
    { "en": "Sumber energi listrik portabel dengan tegangan DC?", "id": "Baterai." },
    { "en": "Mengubah sinyal listrik menjadi getaran suara?", "id": "Speaker / Loudspeaker." },
    { "en": "Menguatkan sinyal diferensial dengan gain yang sangat tinggi?", "id": "Op-Amp (Operational Amplifier)." },
    { "en": "Gerbang logika digital yang outputnya akan HIGH jika semua inputnya HIGH?", "id": "Gerbang AND." },
    { "en": "Gerbang logika digital yang outputnya akan HIGH jika salah satu inputnya HIGH?", "id": "Gerbang OR." },
    { "en": "Gerbang logika digital yang outputnya merupakan kebalikan dari inputnya?", "id": "Gerbang NOT (Inverter)." },
    { "en": "Menghasilkan sinyal clock atau pulsa dengan frekuensi yang dapat diatur?", "id": "IC Timer (Contoh: NE555)." },
    { "en": "Menurunkan dan meregulasi tegangan DC ke level yang tetap dan stabil (misal 5V)?", "id": "IC Regulator Tegangan (Contoh: 7805)." },
    { "en": "Transistor yang dikendalikan oleh tegangan pada gerbang (gate) untuk mengatur aliran arus?", "id": "MOSFET." },
    { "en": "Komponen semikonduktor dengan empat lapis (P-N-P-N) yang berfungsi sebagai saklar daya?", "id": "SCR (Silicon Controlled Rectifier)." },
    { "en": "Mengubah getaran suara menjadi sinyal listrik?", "id": "Mikrofon." },
    { "en": "Menghasilkan bunyi 'bip' atau nada saat diberi tegangan DC?", "id": "Buzzer." },
    { "en": "Papan rangkaian tempat menyusun dan menyolder komponen elektronika?", "id": "PCB (Printed Circuit Board)." },
    { "en": "Menyambungkan dan memutuskan aliran listrik secara manual?", "id": "Saklar (Switch)." },
    { "en": "Kapasitor yang nilainya dapat diubah untuk memilih frekuensi radio?", "id": "Kapasitor Variabel (Varco)." },
    { "en": "Mendeteksi cahaya dan mengubahnya menjadi arus atau tegangan listrik?", "id": "Fotodioda." },
    { "en": "Membagi tegangan menjadi dua atau lebih level yang lebih rendah?", "id": "Pembagi Tegangan (Voltage Divider)." },
    { "en": "Menahan perubahan arus yang tiba-tiba dalam rangkaian?", "id": "Induktor (Choke)." },
    { "en": "Dioda dengan waktu pemulihan sangat cepat untuk aplikasi frekuensi tinggi?", "id": "Dioda Schottky." },
    { "en": "Komponen yang resistansinya menurun drastis saat suhunya naik?", "id": "NTC (Negative Temperature Coefficient Thermistor)." },
    { "en": "Komponen yang resistansinya meningkat drastis saat suhunya naik?", "id": "PTC (Positive Temperature Coefficient Thermistor)." },
    { "en": "Sirkuit terpadu yang dapat diprogram untuk mengendalikan sistem elektronik?", "id": "Mikrokontroler." },
    { "en": "Unit pemrosesan utama dalam sistem komputer?", "id": "CPU (Central Processing Unit) / Mikroprosesor." },
    { "en": "Menampilkan angka atau karakter menggunakan segmen-segmen LED?", "id": "Seven Segment Display." },
    { "en": "Menggabungkan sinyal input dari beberapa jalur ke satu jalur output?", "id": "Multiplexer (MUX)." },
    { "en": "Mendistribusikan sinyal input dari satu jalur ke beberapa jalur output?", "id": "Demultiplexer (DEMUX)." },
    { "en": "Melewatkan sinyal AC dari satu tingkat rangkaian ke tingkat berikutnya?", "id": "Kapasitor Kopling." },
    { "en": "Menghubungkan sinyal frekuensi radio dari antena ke penerima?", "id": "Konektor Antena." },
    { "en": "Menghasilkan getaran frekuensi yang sangat stabil untuk aplikasi waktu?", "id": "Kristal Osiator (Crystal Oscillator)." },
    { "en": "Gerbang logika yang outputnya adalah kebalikan dari gerbang AND?", "id": "Gerbang NAND." },
    { "en": "Gerbang logika yang outputnya adalah kebalikan dari gerbang OR?", "id": "Gerbang NOR." },
    { "en": "Resistor yang nilai hambatannya sangat presisi dan stabil terhadap suhu?", "id": "Resistor Film Logam (Metal Film Resistor)." },
    { "en": "Menyimpan data digital secara sementara selama komputer beroperasi?", "id": "RAM (Random Access Memory)." },
    { "en": "Menyimpan data firmware atau program yang tidak dapat diubah?", "id": "ROM (Read-Only Memory)." },
    { "en": "SCR versi dua arah yang dapat mengalirkan arus AC?", "id": "TRIAC." },
    { "en": "Transistor yang menggunakan efek medan untuk mengontrol konduktivitas?", "id": "FET (Field-Effect Transistor)." },
    { "en": "Menampilkan gambar dan teks pada perangkat elektronik?", "id": "LCD (Liquid Crystal Display)." },
    { "en": "Mengisolasi dua bagian rangkaian secara elektrik tetapi menghubungkannya secara optik?", "id": "Optocoupler / Optoisolator." },
    { "en": "Menghasilkan gelombang periodik seperti sinus, kotak, atau segitiga?", "id": "Generator Fungsi (Function Generator)." },
    { "en": "Mengubah sinyal analog menjadi data digital?", "id": "ADC (Analog-to-Digital Converter)." },
    { "en": "Mengubah data digital menjadi sinyal analog?", "id": "DAC (Digital-to-Analog Converter)." },
    { "en": "Mengukur dan menampilkan nilai tegangan listrik?", "id": "Voltmeter." },
    { "en": "Mengukur dan menampilkan nilai arus listrik?", "id": "Amperemeter." },
    { "en": "Mengukur dan menampilkan nilai hambatan listrik?", "id": "Ohmmeter." },
    { "en": "Alat ukur gabungan untuk tegangan, arus, dan hambatan?", "id": "Multimeter / AVO Meter." },
    { "en": "Resistor yang nilai resistansinya diatur dengan obeng kecil?", "id": "Trimmer Potensiometer (Trimpot)." },
    { "en": "Melindungi rangkaian dari lonjakan tegangan berlebih?", "id": "Varistor." },
    { "en": "Menyaring frekuensi tertentu dan melewatkan frekuensi lainnya?", "id": "Filter Pasif (R-C / R-L)." },
    { "en": "Kapasitor dengan polaritas positif dan negatif yang harus dipasang dengan benar?", "id": "Kapasitor Elektrolit (Elco)." },
    { "en": "Dioda yang berfungsi sebagai kapasitor yang nilainya dikendalikan oleh tegangan?", "id": "Dioda Varactor (Varicap)." },
    { "en": "Mengubah energi listrik menjadi gerakan putar?", "id": "Motor DC." },
    { "en": "Gerbang logika yang outputnya HIGH jika jumlah input HIGH ganjil?", "id": "Gerbang XOR." },
    { "en": "Sirkuit digital yang dapat menyimpan satu bit informasi?", "id": "Flip-Flop." },
    { "en": "Menghubungkan beberapa titik dalam rangkaian tanpa solder untuk prototyping?", "id": "Breadboard / Project Board." },
    { "en": "Menghilangkan panas berlebih dari komponen seperti IC atau transistor daya?", "id": "Heatsink (Pendingin)." },
    { "en": "Kabel fleksibel yang menghubungkan dua papan sirkuit?", "id": "Kabel Pita (Ribbon Cable)." },
    { "en": "Menghubungkan satu jalur ke jalur lainnya pada PCB?", "id": "Jumper." },
    { "en": "Kapasitor non-polar yang cocok untuk aplikasi frekuensi tinggi?", "id": "Kapasitor Keramik." },
    { "en": "Transistor yang terdiri dari dua jenis: NPN dan PNP?", "id": "Transistor Bipolar (BJT)." },
    { "en": "Menghasilkan frekuensi radio (RF) untuk pemancar?", "id": "Osilator RF." },
    { "en": "Menggabungkan sinyal audio dari beberapa sumber?", "id": "Mixer Audio." },
    { "en": "Memisahkan sinyal audio menjadi beberapa jalur frekuensi (bass, treble)?", "id": "Crossover." },
    { "en": "Mengubah energi listrik menjadi cahaya inframerah?", "id": "LED Inframerah (IR LED)." },
    { "en": "Mendeteksi radiasi inframerah dari lingkungan atau pemancar?", "id": "Sensor Inframerah (IR Receiver)." },
    { "en": "Komponen yang memancarkan cahaya laser saat dialiri arus?", "id": "Dioda Laser." },
    { "en": "Menyearahkan tegangan AC dari kedua siklus (positif dan negatif)?", "id": "Jembatan Dioda (Diode Bridge)." },
    { "en": "Menyimpan program dan data bahkan saat catu daya dimatikan?", "id": "Memori Non-Volatile (Contoh: Flash Memory)." },
    { "en": "Menggeser level tegangan DC suatu sinyal?", "id": "Rangkaian Clamper." },
    { "en": "Memotong bagian atas atau bawah dari suatu sinyal gelombang?", "id": "Rangkaian Clipper." },
    { "en": "Meningkatkan amplitudo sinyal audio sebelum masuk ke speaker?", "id": "Amplifier Daya (Power Amplifier)." },
    { "en": "Mengatur kecerahan lampu atau kecepatan motor AC?", "id": "Dimmer / Rangkaian TRIAC." },
    { "en": "Memberikan jeda waktu sebelum suatu aksi terjadi dalam rangkaian?", "id": "Rangkaian Penunda Waktu (Time Delay Circuit)." },
    { "en": "Berfungsi sebagai penstabil impedansi antara dua tingkat rangkaian?", "id": "Rangkaian Buffer." },
    { "en": "Mendeteksi suhu dan mengubahnya menjadi sinyal listrik?", "id": "Sensor Suhu." },
    { "en": "Mendeteksi kelembaban udara dan mengubahnya menjadi sinyal listrik?", "id": "Sensor Kelembaban." },
    { "en": "Mendeteksi tekanan dan mengubahnya menjadi sinyal listrik?", "id": "Sensor Tekanan." },
    { "en": "Mendeteksi gerakan dan mengubahnya menjadi sinyal listrik?", "id": "Sensor Gerak (Contoh: PIR)." },
    { "en": "Mendeteksi jarak ke suatu objek menggunakan gelombang suara?", "id": "Sensor Ultrasonik." },
    { "en": "Mengatur volume suara pada perangkat audio?", "id": "Potensiometer." },
    { "en": "Mengubah getaran mekanis menjadi sinyal listrik?", "id": "Sensor Getar (Vibration Sensor)." },
    { "en": "Mendeteksi keberadaan medan magnet?", "id": "Sensor Efek Hall (Hall Effect Sensor)." },
    { "en": "Menghitung jumlah pulsa atau kejadian dalam periode waktu tertentu?", "id": "Counter / Pencacah." },
    { "en": "Meningkatkan tegangan DC ke level yang lebih tinggi?", "id": "Boost Converter." },
    { "en": "Menurunkan tegangan DC ke level yang lebih rendah secara efisien?", "id": "Buck Converter." },
    { "en": "Menstabilkan catu daya pada IC dengan meredam noise frekuensi tinggi?", "id": "Kapasitor Bypass (Decoupling Capacitor)." },
    { "en": "Rangkaian terpadu yang logikanya dapat dikonfigurasi oleh pengguna?", "id": "FPGA (Field-Programmable Gate Array)." },
    { "en": "Mengukur percepatan, kemiringan, dan getaran suatu objek?", "id": "Akselerometer." },
    { "en": "Mengukur kecepatan dan orientasi sudut?", "id": "Giroskop (Gyroscope)." },
    { "en": "Menghubungkan sinyal audio/video digital definisi tinggi dengan satu kabel?", "id": "Konektor HDMI." },
    { "en": "Menghubungkan kabel jaringan Ethernet ke perangkat?", "id": "Konektor RJ45." },
    { "en": "Meradiasikan atau menerima gelombang elektromagnetik (sinyal radio)?", "id": "Antena." },
    { "en": "Rangkaian terpadu yang dirancang khusus untuk satu aplikasi spesifik?", "id": "ASIC (Application-Specific Integrated Circuit)." },
    { "en": "Memberikan informasi waktu (detik, menit, jam, tanggal) secara akurat?", "id": "RTC (Real-Time Clock)." },
    { "en": "Sepasang transistor yang terhubung untuk menghasilkan penguatan arus yang sangat tinggi?", "id": "Pasangan Darlington (Darlington Pair)." },
    { "en": "Gerbang logika yang inputnya memiliki histeresis untuk membersihkan sinyal digital yang 'kotor'?", "id": "Schmitt Trigger." },
    { "en": "Menerima dan mengirim data melalui gelombang radio pada frekuensi 2.4 GHz?", "id": "Modul Wi-Fi." },
    { "en": "Menjalin komunikasi nirkabel jarak pendek antar perangkat?", "id": "Modul Bluetooth." },
    { "en": "Menentukan lokasi geografis di bumi menggunakan sinyal satelit?", "id": "Modul GPS." },
    { "en": "Membaca dan menulis data ke kartu identifikasi frekuensi radio?", "id": "Modul RFID." },
    { "en": "Memori non-volatile yang dapat dihapus dan diprogram ulang secara elektrik?", "id": "EEPROM (Electrically Erasable PROM)." },
    { "en": "Kapasitor dengan dielektrikum Tantalum yang memiliki kepadatan tinggi?", "id": "Kapasitor Tantalum." },
    { "en": "Filter yang hanya melewatkan sinyal dengan frekuensi di atas titik potong (cut-off)?", "id": "Filter Lolos Atas (High-Pass Filter)." },
    { "en": "Filter yang hanya melewatkan sinyal dalam rentang frekuensi tertentu?", "id": "Filter Lolos Pita (Band-Pass Filter)." },
    { "en": "Filter yang menolak sinyal dalam rentang frekuensi tertentu?", "id": "Filter Tolak Pita (Band-Stop/Notch Filter)." },
    { "en": "Induktor dengan inti berbentuk donat untuk efisiensi medan magnet?", "id": "Induktor Toroidal." },
    { "en": "Dioda yang digunakan sebagai saklar atau attenuator frekuensi radio?", "id": "Dioda PIN." },
    { "en": "Melindungi rangkaian dari kerusakan akibat ESD (Electrostatic Discharge)?", "id": "Dioda TVS (Transient Voltage Suppression)." },
    { "en": "Transistor yang aktif ketika mendeteksi cahaya?", "id": "Fototransistor." },
    { "en": "Menyimpan dan menggeser data bit secara berurutan?", "id": "Register Geser (Shift Register)." },
    { "en": "Mengubah kode biner menjadi sinyal output tertentu (misal: ke 7-segment)?", "id": "Decoder." },
    { "en": "Mengubah beberapa jalur input menjadi kode biner?", "id": "Encoder." },
    { "en": "Memori yang datanya hilang saat catu daya dimatikan?", "id": "Memori Volatile (Contoh: RAM)." },
    { "en": "Konektor standar untuk sinyal audio pada headphone atau mikrofon?", "id": "Konektor Jack Audio (3.5mm)." },
    { "en": "Menyesuaikan impedansi antara kabel koaksial dan antena?", "id": "Balun (Balanced to Unbalanced)." },
    { "en": "Sekelompok saklar kecil dalam satu kemasan untuk mengatur konfigurasi?", "id": "DIP Switch." },
    { "en": "Saklar yang aktif ketika ada objek yang menekannya secara mekanis?", "id": "Limit Switch." },
    { "en": "Saklar yang diaktifkan oleh medan magnet?", "id": "Saklar Buluh (Reed Switch)." },
    { "en": "Jenis layar yang menghasilkan cahayanya sendiri, tidak perlu backlight?", "id": "OLED (Organic Light Emitting Diode)." },
    { "en": "Memori RAM dinamis yang perlu di-refresh secara berkala?", "id": "DRAM (Dynamic RAM)." },
    { "en": "Memori RAM statis yang lebih cepat tetapi lebih mahal dari DRAM?", "id": "SRAM (Static RAM)." },
    { "en": "Regulator tegangan dengan dropout voltage yang sangat rendah?", "id": "LDO (Low-Dropout) Regulator." },
    { "en": "Catu daya yang mengubah tegangan dengan metode pensaklaran frekuensi tinggi?", "id": "SMPS (Switch-Mode Power Supply)." },
    { "en": "Sirkuit yang menghasilkan dua output yang saling berkebalikan (Q dan Q-bar)?", "id": "Flip-Flop." },
    { "en": "Mengukur orientasi absolut dengan mengacu pada medan magnet bumi?", "id": "Magnetometer." },
    { "en": "Mengukur tekanan atmosfer?", "id": "Sensor Barometer." },
    { "en": "Mendeteksi keberadaan gas tertentu di udara?", "id": "Sensor Gas." },
    { "en": "Mengukur tingkat keasaman atau kebasaan (pH) suatu cairan?", "id": "Sensor pH." },
    { "en": "Komponen yang terdiri dari beberapa resistor dalam satu kemasan?", "id": "Jaringan Resistor (Resistor Network)." },
    { "en": "Resistor yang dipasang di permukaan PCB (tanpa kaki kawat)?", "id": "Resistor SMD (Surface-Mount Device)." },
    { "en": "Induktor yang dirancang khusus untuk menekan noise frekuensi radio?", "id": "RF Choke." },
    { "en": "Kapasitor yang menggunakan lapisan film tipis sebagai dielektrik?", "id": "Kapasitor Film (Mylar, Polyester)." },
    { "en": "Mengubah tegangan DC menjadi tegangan AC?", "id": "Inverter." },
    { "en": "Transistor yang dikendalikan oleh arus basis untuk mengatur aliran kolektor-emitor?", "id": "Transistor Bipolar (BJT)." },
    { "en": "Konfigurasi amplifier transistor yang paling umum dengan gain tegangan tinggi?", "id": "Common Emitter Amplifier." },
    { "en": "Konfigurasi amplifier transistor yang digunakan sebagai buffer dengan gain tegangan mendekati 1?", "id": "Common Collector (Emitter Follower)." },
    { "en": "Komponen pasif yang menggabungkan fungsi induktor dan kapasitor dalam satu paket?", "id": "Resonator Keramik." },
    { "en": "Menghubungkan dan memutuskan jalur sinyal frekuensi tinggi dengan isolasi yang baik?", "id": "Saklar RF." },
    { "en": "Konektor standar industri untuk sinyal video analog?", "id": "Konektor VGA (DB-15)." },
    { "en": "Konektor multi-pin yang sering digunakan untuk komunikasi serial (RS-232)?", "id": "Konektor DB9." },
    { "en": "Konektor daya standar yang digunakan pada hard drive dan periferal komputer lama?", "id": "Konektor Molex." },
    { "en": "Berfungsi sebagai referensi tegangan yang sangat akurat dan stabil?", "id": "IC Referensi Tegangan (Voltage Reference)." },
    { "en": "Konektor coaxial untuk sinyal frekuensi radio, sering untuk antena atau video?", "id": "Konektor BNC." },
    { "en": "Kabel yang dirancang untuk mentransmisikan sinyal frekuensi tinggi dengan gangguan minimal?", "id": "Kabel Koaksial (Coaxial Cable)." },
    { "en": "Sirkuit yang membandingkan dua tegangan input dan mengeluarkan sinyal digital?", "id": "Komparator (Comparator)." },
    { "en": "Menggandakan frekuensi sinyal input?", "id": "Pengganda Frekuensi (Frequency Doubler)." },
    { "en": "Membagi frekuensi sinyal input menjadi lebih rendah?", "id": "Pembagi Frekuensi (Frequency Divider)." },
    { "en": "Motor yang berputar dalam langkah-langkah sudut yang presisi?", "id": "Motor Stepper." },
    { "en": "Motor DC yang dilengkapi dengan sensor posisi untuk kontrol loop tertutup?", "id": "Motor Servo." },
    { "en": "Mengatur daya ke beban dengan memotong sebagian dari gelombang AC?", "id": "Phase Control Circuit." },
    { "en": "Sensor yang mendeteksi objek tanpa kontak fisik, menggunakan medan elektromagnetik?", "id": "Sensor Proximity Induktif." },
    { "en": "Sensor yang mendeteksi objek tanpa kontak fisik, menggunakan kapasitansi?", "id": "Sensor Proximity Kapasitif." },
    { "en": "Lampu indikator neon kecil yang menyala pada tegangan tinggi?", "id": "Lampu Neon." },
    { "en": "Mengukur fluks magnet?", "id": "Fluxmeter." },
    { "en": "Menampilkan informasi dalam bentuk matriks titik-titik cahaya?", "id": "Dot Matrix Display." },
    { "en": "Menyimpan pengaturan atau data dalam jumlah kecil yang perlu bertahan saat listrik mati?", "id": "Memori CMOS Baterai." },
    { "en": "Memori yang dapat diprogram sekali saja oleh pengguna?", "id": "PROM (Programmable Read-Only Memory)." },
    { "en": "Memori yang dapat dihapus dengan sinar ultraviolet?", "id": "EPROM (Erasable Programmable ROM)." },
    { "en": "Soket untuk memasang dan melepas IC dengan mudah dari PCB?", "id": "Soket IC." },
    { "en": "Mengubah gerakan putar poros menjadi pulsa digital?", "id": "Rotary Encoder." },
    { "en": "Menghantarkan gelombang mikro pada frekuensi sangat tinggi?", "id": "Waveguide." },
    { "en": "Komponen pasif RF yang mengarahkan sinyal dari satu port ke port lain secara berurutan?", "id": "Sirkulator (Circulator)." },
    { "en": "Penyambung listrik yang terdiri dari pin jantan dan soket betina?", "id": "Header Pin & Soket." },
    { "en": "Menghasilkan tegangan tinggi dari tegangan rendah melalui induksi?", "id": "Flyback Transformer." },
    { "en": "Elemen pemanas yang mengubah listrik menjadi panas?", "id": "Elemen Pemanas (Heating Element)." },
    { "en": "Mendeteksi tingkat kemiringan suatu permukaan?", "id": "Sensor Kemiringan (Tilt Sensor)." },
    { "en": "Perangkat elektromekanis yang menghasilkan suara 'klik' atau getaran?", "id": "Solenoid." },
    { "en": "Gabungan giroskop dan akselerometer untuk stabilisasi gambar atau navigasi?", "id": "IMU (Inertial Measurement Unit)." },
    { "en": "Dioda yang menunjukkan perilaku resistansi negatif pada daerah tertentu?", "id": "Dioda Tunnel." },
    { "en": "Menghasilkan sinyal gelombang mikro?", "id": "Dioda Gunn." },
    { "en": "Komponen yang dapat menahan tegangan sangat tinggi saat mati (off)?", "id": "Dioda Penyearah Cepat (Fast Recovery Diode)." },
    { "en": "Mengukur arus listrik AC/DC tanpa memutus sirkuit?", "id": "Sensor Arus (Current Sensor)." },
    { "en": "Mengubah besaran fisika menjadi resistansi yang berubah-ubah?", "id": "Strain Gauge." },
    { "en": "Saklar yang dioperasikan dengan sentuhan kulit?", "id": "Saklar Sentuh Kapasitif." },
    { "en": "Konektor kecil yang umum digunakan untuk baterai atau motor pada PCB?", "id": "Konektor JST." },
    { "en": "Kapasitor yang dirancang khusus untuk menahan tegangan AC tinggi pada motor?", "id": "Kapasitor Motor." },
    { "en": "Menggabungkan beberapa sinyal menjadi satu sinyal komposit?", "id": "Rangkaian Penjumlah (Summing Amplifier)." },
    { "en": "Sirkuit yang mengintegrasikan sinyal input terhadap waktu?", "id": "Integrator." },
    { "en": "Sirkuit yang mendiferensiasikan sinyal input terhadap waktu?", "id": "Differentiator." },
    { "en": "Mengubah sinyal audio stereo menjadi sinyal FM komposit untuk transmisi?", "id": "Encoder Stereo." },
    { "en": "Mendeteksi perubahan sudut putaran poros?", "id": "Sensor Sudut (Angle Sensor)." },
    { "en": "Elemen semikonduktor yang mendinginkan atau memanaskan suatu sisi saat dialiri arus?", "id": "Elemen Peltier." },
    { "en": "Kabel yang terdiri dari banyak helai tipis untuk fleksibilitas?", "id": "Kabel Serabut." },
    { "en": "Kabel yang terdiri dari satu inti padat?", "id": "Kabel Pejal (Solid Cable)." },
    { "en": "Menampilkan bentuk gelombang sinyal listrik terhadap waktu secara visual?", "id": "Osiloskop (Oscilloscope)." },
    { "en": "Komponen semikonduktor daya yang menggabungkan keunggulan BJT dan MOSFET?", "id": "IGBT (Insulated Gate Bipolar Transistor)." },
    { "en": "Sensor gambar yang mengubah foton menjadi muatan listrik di setiap piksel secara individual?", "id": "Sensor Gambar CMOS." },
    { "en": "Menguatkan sinyal frekuensi radio (RF) yang sangat lemah tanpa menambah banyak derau?", "id": "LNA (Low-Noise Amplifier)." },
    { "en": "Menekan interferensi elektromagnetik (EMI) pada kabel?", "id": "Manik Ferit (Ferrite Bead)." },
    { "en": "Menghasilkan tegangan output yang merupakan kebalikan polaritas dari tegangan input?", "id": "Inverting Amplifier." },
    { "en": "Menciptakan arus output yang merupakan salinan presisi dari arus input?", "id": "Cermin Arus (Current Mirror)." },
    { "en": "Melindungi kontak saklar dari lonjakan tegangan saat memutus beban induktif?", "id": "Rangkaian Snubber." },
    { "en": "Sinyal periodik yang menyinkronkan operasi antar bagian dalam sistem digital?", "id": "Sinyal Clock." },
    { "en": "Sekumpulan jalur paralel untuk mentransfer data antara CPU, memori, dan periferal?", "id": "Bus Data." },
    { "en": "Sekumpulan jalur paralel yang menentukan lokasi memori atau periferal yang diakses?", "id": "Bus Alamat." },
    { "en": "Filter yang menggunakan gelombang akustik permukaan untuk selektivitas frekuensi tinggi?", "id": "Filter SAW (Surface Acoustic Wave)." },
    { "en": "Dioda yang sangat sensitif terhadap cahaya dan beroperasi di daerah breakdown terbalik?", "id": "APD (Avalanche Photodiode)." },
    { "en": "Menghasilkan tegangan DC yang dapat diatur dari sumber AC, menggunakan SCR atau TRIAC?", "id": "Penyearah Terkendali (Controlled Rectifier)." },
    { "en": "Menyediakan catu daya DC yang stabil dan dapat diatur untuk pengujian rangkaian?", "id": "Power Supply Unit (PSU) Laboratorium." },
    { "en": "Menampilkan kandungan frekuensi dari suatu sinyal?", "id": "Spectrum Analyzer." },
    { "en": "Menangkap dan menampilkan beberapa sinyal digital secara bersamaan untuk analisis waktu?", "id": "Logic Analyzer." },
    { "en": "Mengubah energi matahari menjadi energi listrik DC?", "id": "Sel Surya (Solar Cell)." },
    { "en": "Memori akses acak sinkron yang umum digunakan di komputer modern?", "id": "SDRAM (Synchronous DRAM)." },
    { "en": "Mikrofon yang dibuat menggunakan teknologi fabrikasi micro-electromechanical system?", "id": "Mikrofon MEMS." },
    { "en": "Speaker kecil yang dirancang khusus untuk mereproduksi frekuensi tinggi (treble)?", "id": "Tweeter." },
    { "en": "Speaker yang dirancang khusus untuk mereproduksi frekuensi rendah (bass)?", "id": "Woofer." },
    { "en": "Speaker yang dirancang untuk mereproduksi frekuensi sangat rendah (sub-bass)?", "id": "Subwoofer." },
    { "en": "Sirkuit yang menghasilkan tegangan output yang sama dengan input tetapi dengan kemampuan arus lebih besar?", "id": "Voltage Follower / Buffer." },
    { "en": "Sirkuit yang outputnya tidak mengubah polaritas sinyal input?", "id": "Non-inverting Amplifier." },
    { "en": "Rangkaian op-amp yang outputnya adalah hasil penjumlahan matematis dari beberapa input?", "id": "Summing Amplifier (Adder)." },
    { "en": "Komponen elektromekanis yang digunakan untuk memilih satu dari beberapa posisi putar?", "id": "Rotary Switch." },
    { "en": "Menghubungkan dua poros mesin untuk mentransfer torsi?", "id": "Coupling." },
    { "en": "Papan sirkuit tipis dan lentur yang dapat ditekuk?", "id": "PCB Fleksibel (Flex PCB)." },
    { "en": "Resistor yang dirancang untuk menangani daya listrik yang besar?", "id": "Resistor Daya (Power Resistor)." },
    { "en": "Kapasitor yang menggunakan lapisan oksida pada logam sebagai dielektrik?", "id": "Kapasitor Oksida Logam." },
    { "en": "Induktor yang nilai induktansinya dapat diatur?", "id": "Induktor Variabel." },
    { "en": "Dioda yang terbuat dari Germanium, memiliki tegangan maju yang lebih rendah dari Silikon?", "id": "Dioda Germanium." },
    { "en": "Transistor yang menggabungkan dua jenis semikonduktor (misal: Si dan Ge) dalam satu chip?", "id": "Heterojunction Bipolar Transistor (HBT)." },
    { "en": "IC yang berisi seluruh sirkuit penerima atau pemancar radio?", "id": "IC Transceiver." },
    { "en": "Mengubah sinyal frekuensi radio (RF) ke frekuensi yang lebih rendah (IF)?", "id": "Mixer." },
    { "en": "Memodulasi sinyal pembawa (carrier) dengan sinyal informasi?", "id": "Modulator." },
    { "en": "Mengekstrak sinyal informasi asli dari sinyal pembawa (carrier)?", "id": "Demodulator." },
    { "en": "Perangkat lunak yang tertanam secara permanen di dalam perangkat keras?", "id": "Firmware." },
    { "en": "Sistem program dasar untuk memulai komputer dan mengelola perangkat keras?", "id": "BIOS / UEFI." },
    { "en": "Mengukur resistansi isolasi dengan tegangan tinggi?", "id": "Megohmmeter (Megger)." },
    { "en": "Menghasilkan sinyal dengan berbagai bentuk gelombang dan frekuensi untuk pengujian?", "id": "Arbitrary Waveform Generator (AWG)." },
    { "en": "Beban elektronik yang dapat diatur untuk menguji catu daya?", "id": "Electronic Load." },
    { "en": "Pelindung sirkuit yang dapat direset setelah trip, tidak seperti sekring?", "id": "Circuit Breaker." },
    { "en": "Pelindung dari arus bocor ke tanah untuk keselamatan manusia?", "id": "GFCI / ELCB." },
    { "en": "Komponen yang nilai resistansinya berubah sesuai medan magnet?", "id": "Magnetoresistor." },
    { "en": "Sensor untuk mendeteksi keberadaan air atau cairan konduktif?", "id": "Sensor Air (Water Sensor)." },
    { "en": "Kamera yang dapat melihat radiasi panas (inframerah)?", "id": "Kamera Termal." },
    { "en": "Menampilkan gambar dengan memanipulasi cermin-cermin mikro?", "id": "DLP (Digital Light Processing) Chip." },
    { "en": "Membaca data dari strip magnetik pada kartu?", "id": "Magnetic Stripe Reader." },
    { "en": "Konektor berbentuk bulat yang umum untuk catu daya DC?", "id": "Konektor DC Barrel." },
    { "en": "Konektor multi-pin yang digunakan untuk menghubungkan hard drive SATA?", "id": "Konektor SATA." },
    { "en": "Slot pada motherboard untuk memasang kartu ekspansi?", "id": "Slot PCI Express (PCIe)." },
    { "en": "Soket pada motherboard tempat memasang prosesor?", "id": "Soket CPU." },
    { "en": "Kipas kecil yang digunakan untuk mendinginkan komponen elektronik?", "id": "Kipas Pendingin (Cooling Fan)." },
    { "en": "Transformator yang digunakan untuk mengukur arus tinggi dengan mengubahnya menjadi arus rendah?", "id": "Transformator Arus (Current Transformer)." },
    { "en": "Transformator yang digunakan untuk mengukur tegangan tinggi dengan mengubahnya menjadi tegangan rendah?", "id":- "Transformator Tegangan (Potential Transformer)." },
    { "en": "Mengubah tegangan DC dari satu level ke level lain (bisa naik atau turun)?", "id": "Buck-Boost Converter." },
    { "en": "Perangkat pasif dua arah yang memungkinkan sinyal RF lewat hanya dalam satu arah?", "id": "Isolator RF." },
    { "en": "Perangkat pasif tiga port yang membagi daya dan memberikan isolasi antar port?", "id": "Directional Coupler." },
    { "en": "Mengatur dan membatasi jumlah arus yang mengalir ke LED?", "id": "Driver LED." },
    { "en": "Perangkat yang menyerap energi RF dan mengubahnya menjadi panas?", "id": "Attenuator." },
    { "en": "Sirkuit yang mengunci frekuensi osilator ke frekuensi referensi?", "id": "PLL (Phase-Locked Loop)." },
    { "en": "Osilator yang tegangannya mengontrol frekuensi output?", "id": "VCO (Voltage-Controlled Oscillator)." },
    { "en": "Transistor efek medan yang terbuat dari Gallium Nitride untuk efisiensi daya tinggi?", "id": "GaN FET." },
    { "en": "Sirkuit terpadu yang dirancang untuk mengelola daya baterai?", "id": "IC Manajemen Baterai (BMS)." },
    { "en": "Penguat operasional yang dirancang untuk menggerakkan beban berat seperti motor atau speaker?", "id": "Power Op-Amp." },
    { "en": "Penguat operasional dengan noise sangat rendah untuk sinyal sensor yang lemah?", "id": "Precision Op-Amp." },
    { "en": "Mengukur kapasitansi sebuah kapasitor?", "id": "Capacitance Meter." },
    { "en": "Mengukur induktansi sebuah induktor?", "id": "Inductance Meter (LCR Meter)." },
    { "en": "Sirkuit yang digunakan untuk mendeteksi sinyal AM radio?", "id": "Detektor Selubung (Envelope Detector)." },
    { "en": "Konektor video dan audio digital yang lebih kecil dari HDMI?", "id": "DisplayPort." },
    { "en": "Sensor sidik jari untuk otentikasi biometrik?", "id": "Sensor Sidik Jari (Fingerprint Sensor)." },
    { "en": "Konektor standar yang digunakan pada panel surya?", "id": "Konektor MC4." },
    { "en": "Kabel yang dirancang untuk melindungi sinyal dari interferensi luar?", "id": "Kabel Terlindung (Shielded Cable)." },
    { "en": "Menyalurkan dan menerima data melalui serat optik?", "id": "Transceiver Optik." },
    { "en": "Mengkonversi data serial menjadi paralel?", "id": "Serial-to-Parallel Converter." },
    { "en": "Mengkonversi data paralel menjadi serial?", "id": "Parallel-to-Serial Converter." },
    { "en": "Gerbang logika yang dapat diprogram untuk menjadi AND, OR, XOR, dll?", "id": "Universal Logic Gate (NAND/NOR)." },
    { "en": "Transistor tunggal yang dikemas dalam satu chip?", "id": "Transistor Diskrit." },
    { "en": "Menghasilkan tegangan negatif dari tegangan positif?", "id": "Charge Pump." },
    { "en": "Komparator dengan dua ambang batas (threshold) untuk kekebalan noise?", "id": "Window Comparator." },
    { "en": "Mengukur intensitas cahaya?", "id": "Lux Meter." },
    { "en": "Mengukur tingkat suara dalam desibel?", "id": "Sound Level Meter." },
    { "en": "Penyambung yang memungkinkan beberapa kabel dihubungkan pada satu titik?", "id": "Blok Terminal (Terminal Block)." },
    { "en": "Sekrup kecil yang digunakan untuk mengamankan PCB ke chassis?", "id": "Standoff / Spacer." },
    { "en": "Menghubungkan sinyal antara dua PCB yang ditumpuk?", "id": "Board-to-Board Connector." },
    { "en": "Konektor untuk menghubungkan kabel ke PCB?", "id": "Wire-to-Board Connector." },
    { "en": "Sirkuit yang menggandakan tegangan DC input?", "id": "Pengganda Tegangan (Voltage Doubler)." },
    { "en": "Resistor yang digunakan untuk mengukur arus dengan mengukur penurunan tegangan di atasnya?", "id": "Resistor Shunt." },
    { "en": "Memori yang menggabungkan kecepatan SRAM dan kepadatan DRAM?", "id": "MRAM (Magnetoresistive RAM)." },
    { "en": "Kapasitor yang dirancang khusus untuk aplikasi frekuensi radio (RF)?", "id": "Kapasitor RF." },
    { "en": "Dioda yang memancarkan cahaya ultraviolet (UV)?", "id": "LED UV." },
    { "en": "Mengemulasi perilaku sirkuit digital sebelum dibuat secara fisik?", "id": "Simulator Logika." },
    { "en": "Program untuk merancang layout papan sirkuit cetak (PCB)?", "id": "Software Desain PCB (Contoh: Eagle, KiCad)." },
    { "en": "Perangkat yang memprogram chip seperti mikrokontroler atau EEPROM?", "id": "Programmer." },
    { "en": "Tabung vakum penguat sinyal dengan tiga elektroda (katoda, grid, anoda)?", "id": "Tabung Triode." },
    { "en": "Menembakkan berkas elektron ke layar berlapis fosfor untuk menciptakan gambar?", "id": "CRT (Cathode Ray Tube)." },
    { "en": "Menghasilkan gelombang mikro berdaya tinggi untuk radar atau oven?", "id": "Magnetron." },
    { "en": "Memastikan level logika default (HIGH) pada pin input digital saat tidak terhubung?", "id": "Resistor Pull-up." },
    { "en": "Memastikan level logika default (LOW) pada pin input digital saat tidak terhubung?", "id": "Resistor Pull-down." },
    { "en": "Melindungi saklar dari tegangan induktif balik saat mematikan beban seperti relay?", "id": "Dioda Flyback." },
    { "en": "Mengosongkan muatan pada kapasitor catu daya setelah perangkat dimatikan demi keamanan?", "id": "Resistor Bleeder." },
    { "en": "Program kecil yang berjalan pertama kali untuk memuat sistem operasi?", "id": "Bootloader." },
    { "en": "Inti dari sebuah sistem operasi yang mengelola sumber daya perangkat keras?", "id": "Kernel." },
    { "en": "Perangkat lunak yang memungkinkan sistem operasi berkomunikasi dengan perangkat keras spesifik?", "id": "Device Driver." },
    { "en": "Protokol komunikasi serial dua kabel (SDA, SCL) untuk jarak dekat antar IC?", "id": "Protokol I2C (Inter-Integrated Circuit)." },
    { "en": "Protokol komunikasi serial sinkron empat kabel untuk transfer data cepat?", "id": "Protokol SPI (Serial Peripheral Interface)." },
    { "en": "Protokol komunikasi serial asinkron (TX, RX) yang umum untuk komunikasi antar perangkat?", "id": "Protokol UART." },
    { "en": "Protokol komunikasi yang tangguh dan umum digunakan dalam otomotif?", "id": "CAN Bus." },
    { "en": "Kelas amplifier audio dengan fidelitas tertinggi namun efisiensi terendah?", "id": "Amplifier Kelas A." },
    { "en": "Kelas amplifier yang menggunakan dua transistor (push-pull) tetapi mengalami crossover distortion?", "id": "Amplifier Kelas B." },
    { "en": "Kelas amplifier push-pull yang mengatasi crossover distortion dengan sedikit bias?", "id": "Amplifier Kelas AB." },
    { "en": "Kelas amplifier yang sangat efisien karena menggunakan pensaklaran (PWM)?", "id": "Amplifier Kelas D." },
    { "en": "Jenis topologi DC-DC converter yang dapat menaikkan atau menurunkan tegangan?", "id": "Konverter Cuk." },
    { "en": "Bahan dasar PCB yang paling umum, terbuat dari fiberglass dan resin epoksi?", "id": "Substrat FR-4." },
    { "en": "Cairan kimia untuk membersihkan permukaan logam dari oksidasi sebelum menyolder?", "id": "Fluks (Flux)." },
    { "en": "Logam paduan bertitik leleh rendah untuk menyambungkan kaki komponen ke PCB?", "id": "Timah Solder." },
    { "en": "Jalinan kawat tembaga untuk mengangkat kelebihan timah solder?", "id": "Solder Wick." },
    { "en": "Perangkat yang digunakan untuk menyedot timah solder cair?", "id": "Atraktor Solder (Solder Sucker)." },
    { "en": "Sensor yang sangat sensitif terhadap medan magnet, berbasis efek kuantum?", "id": "SQUID (Superconducting Quantum Interference Device)." },
    { "en": "Komponen elektromekanis yang mengubah sinyal listrik menjadi gerakan linier pendek?", "id": "Aktuator Linier." },
    { "en": "Penggerak speaker mini yang sangat efisien, sering digunakan di alat bantu dengar?", "id": "Driver Balanced Armature." },
    { "en": "Elemen yang menghasilkan listrik ketika mengalami tekanan mekanis, atau sebaliknya?", "id": "Elemen Piezoelektrik." },
    { "en": "Tabung berisi gas yang berfungsi sebagai saklar tegangan tinggi?", "id": "Thyratron." },
    { "en": "Celah udara yang dirancang untuk memicu percikan listrik pada tegangan tertentu?", "id": "Spark Gap." },
    { "en": "Antarmuka yang mendefinisikan cara perangkat lunak berkomunikasi satu sama lain?", "id": "API (Application Programming Interface)." },
    { "en": "Satu set alat dan pustaka perangkat lunak untuk mengembangkan aplikasi pada platform tertentu?", "id": "SDK (Software Development Kit)." },
    { "en": "Jenis osilator LC yang menggunakan dua kapasitor dan satu induktor?", "id": "Osilator Colpitts." },
    { "en": "Jenis osilator LC yang menggunakan dua induktor dan satu kapasitor?", "id": "Osilator Hartley." },
    { "en": "Jenis osilator yang menggunakan kristal kuarsa dan sangat stabil?", "id": "Osilator Pierce." },
    { "en": "Rangkaian op-amp yang menguatkan selisih antara dua tegangan input?", "id": "Amplifier Diferensial." },
    { "en": "Rangkaian yang mendeteksi kapan sinyal input berada di atas atau di bawah level referensi?", "id": "Detektor Ambang (Threshold Detector)." },
    { "en": "Menyearahkan hanya setengah dari siklus gelombang AC?", "id": "Penyearah Setengah Gelombang." },
    { "en": "Menyearahkan kedua siklus gelombang AC menggunakan trafo center-tap?", "id": "Penyearah Gelombang Penuh (Center-Tapped)." },
    { "en": "Baterai yang tidak dapat diisi ulang?", "id": "Baterai Primer." },
    { "en": "Baterai yang dapat diisi ulang berulang kali?", "id": "Baterai Sekunder." },
    { "en": "Jenis baterai isi ulang yang umum pada laptop dan ponsel?", "id": "Baterai Lithium-Ion (Li-ion)." },
    { "en": "Jenis baterai isi ulang dengan kepadatan energi lebih tinggi dari Li-ion?", "id": "Baterai Lithium-Polymer (Li-Po)." },
    { "en": "Memori non-volatile yang ditulis saat manufaktur dan tidak bisa diubah?", "id": "Mask ROM." },
    { "en": "Konektor daya internal pada PC untuk CPU?", "id": "Konektor ATX 12V." },
    { "en": "Konektor daya utama 24-pin untuk motherboard PC?", "id": "Konektor ATX." },
    { "en": "Sirkuit terpadu yang menggabungkan CPU dengan periferal lain seperti memori dan I/O?", "id": "SoC (System on a Chip)." },
    { "en": "Papan sirkuit kecil yang menampung mikrokontroler dan komponen pendukung?", "id": "Modul Mikrokontroler (Contoh: Arduino, ESP32)." },
    { "en": "Sirkuit yang menghasilkan tegangan output konstan terlepas dari perubahan beban atau input?", "id": "Regulator Tegangan." },
    { "en": "Perangkat lunak yang digunakan untuk menulis dan mengkompilasi kode untuk mikrokontroler?", "id": "IDE (Integrated Development Environment)." },
    { "en": "Proses mentransfer program (firmware) ke dalam memori mikrokontroler?", "id": "Flashing / Programming." },
    { "en": "Mode operasi mikrokontroler dengan konsumsi daya sangat rendah?", "id": "Mode Tidur (Sleep Mode)." },
    { "en": "Sinyal yang digunakan untuk 'membangunkan' mikrokontroler dari mode tidur?", "id": "Sinyal Interrupt." },
    { "en": "Konektor standar untuk kartu memori portabel?", "id": "Slot Kartu SD." },
    { "en": "Lapisan tembaga pada PCB yang berfungsi sebagai bidang referensi ground?", "id": "Ground Plane." },
    { "en": "Lapisan fotosensitif yang digunakan untuk mentransfer pola sirkuit ke PCB?", "id": "Photoresist." },
    { "en": "Proses menghilangkan tembaga yang tidak diinginkan dari PCB menggunakan bahan kimia?", "id": "Etching." },
    { "en": "Lapisan pelindung berwarna (biasanya hijau) pada PCB?", "id": "Solder Mask." },
    { "en": "Label teks putih pada PCB untuk menandai komponen?", "id": "Silkscreen." },
    { "en": "Lubang pada PCB yang dilapisi logam untuk menghubungkan antar lapisan?", "id": "Via." },
    { "en": "Jalur konduktif tembaga pada PCB?", "id": "Jejak (Trace)." },
    { "en": "Titik solder pada PCB tempat kaki komponen dipasang?", "id": "Pad." },
    { "en": "Sistem bilangan berbasis dua (0 dan 1) yang digunakan dalam elektronika digital?", "id": "Sistem Biner." },
    { "en": "Satuan informasi digital terkecil?", "id": "Bit." },
    { "en": "Sekelompok 8 bit?", "id": "Byte." },
    { "en": "Representasi numerik dari sebuah karakter?", "id": "Kode ASCII." },
    { "en": "Variabel yang hanya bisa bernilai BENAR atau SALAH (1 atau 0)?", "id": "Variabel Boolean." },
    { "en": "Sensor yang mendeteksi warna suatu objek?", "id": "Sensor Warna." },
    { "en": "Sensor yang mendeteksi nyala api?", "id": "Sensor Api." },
    { "en": "Mengukur laju aliran suatu fluida?", "id": "Sensor Aliran (Flow Sensor)." },
    { "en": "Koil kawat yang menghasilkan medan magnet saat dialiri listrik?", "id": "Elektromagnet." },
    { "en": "Penguat yang dirancang untuk sinyal video?", "id": "Video Amplifier." },
    { "en": "Peralatan untuk melihat jejak inframerah dari remote control?", "id": "IR Detector." },
    { "en": "Menghasilkan suara dengan frekuensi di luar jangkauan pendengaran manusia?", "id": "Transduser Ultrasonik." },
    { "en": "Mengubah sinyal listrik menjadi gerakan mekanis presisi tinggi?", "id": "Aktuator Piezoelektrik." },
    { "en": "Layar yang menggunakan tabung vakum kecil untuk setiap piksel?", "id": "VFD (Vacuum Fluorescent Display)." },
    { "en": "Resistor yang nilai hambatannya sangat rendah untuk pengukuran arus?", "id": "Current Sense Resistor." },
    { "en": "Kapasitor dengan nilai yang sangat stabil dan toleransi rendah?", "id": "Kapasitor Presisi." },
    { "en": "Dioda yang bisa menangani arus sangat besar?", "id": "Dioda Daya (Power Diode)." },
    { "en": "Transistor yang dirancang untuk beroperasi pada frekuensi radio?", "id": "Transistor RF." },
    { "en": "IC yang berisi beberapa gerbang logika dalam satu chip?", "id": "IC Logika Seri 74xx." },
    { "en": "Gerbang logika yang outputnya HIGH hanya jika semua inputnya LOW?", "id": "Gerbang NOR." },
    { "en": "Gerbang logika yang outputnya HIGH jika ada input yang LOW?", "id": "Gerbang NAND." },
    { "en": "IC yang membandingkan dua sinyal analog?", "id": "IC Komparator (Contoh: LM393)." },
    { "en": "Rangkaian untuk memilih satu dari banyak sumber sinyal analog?", "id": "Saklar Analog (Analog Switch)." },
    { "en": "Sirkuit untuk menahan nilai tegangan analog sesaat?", "id": "Sample and Hold Circuit." },
    { "en": "Menghasilkan output gelombang persegi (square wave)?", "id": "Astable Multivibrator." },
    { "en": "Memiliki satu keadaan stabil dan satu keadaan tidak stabil?", "id": "Monostable Multivibrator." },
    { "en": "Memiliki dua keadaan stabil yang dapat dipicu bolak-balik?", "id": "Bistable Multivibrator (Flip-Flop)." },
    { "en": "Mengatur putaran motor agar tetap konstan?", "id": "Kontrol Kecepatan Motor." },
    { "en": "Kabel yang dirancang untuk mentransfer data dengan kecepatan sangat tinggi?", "id": "Kabel Twinax." },
    { "en": "Mengurangi amplitudo sinyal tanpa mengubah bentuk gelombangnya?", "id": "Attenuator." },
    { "en": "Sistem umpan balik negatif untuk menstabilkan output suatu sistem?", "id": "Loop Umpan Balik (Feedback Loop)." },
    { "en": "Mengubah impedansi suatu rangkaian agar cocok dengan rangkaian lain?", "id": "Penyerasian Impedansi (Impedance Matching)." },
    { "en": "Perangkat lunak yang mensimulasikan perilaku sirkuit elektronik?", "id": "SPICE Simulator." },
    { "en": "Konektor serbaguna yang umum pada peralatan audio profesional?", "id": "Konektor XLR." },
    { "en": "Kaca yang dilapisi bahan konduktif transparan untuk layar sentuh atau LCD?", "id": "Kaca ITO (Indium Tin Oxide)." },
    { "en": "Melindungi catu daya dari tegangan lebih dengan sengaja menciptakan hubung singkat?", "id": "Rangkaian Linggis (Crowbar Circuit)." },
    { "en": "Unit di dalam CPU yang melakukan operasi aritmetika dan logika?", "id": "ALU (Arithmetic Logic Unit)." },
    { "en": "Tabung vakum yang berfungsi sebagai saklar atau penguat dengan satu grid kontrol?", "id": "Tabung Triode." },
    { "en": "Mendeteksi gelombang radio pada era awal telegrafi nirkabel?", "id": "Koherer (Coherer)." },
    { "en": "Menghasilkan pulsa tegangan sangat tinggi menggunakan tumpukan kapasitor?", "id": "Generator Marx." },
    { "en": "Mikrofon yang digunakan untuk mendengarkan suara di bawah air?", "id": "Hidrofon (Hydrophone)." },
    { "en": "Sensor yang mendeteksi getaran tanah, digunakan dalam seismologi?", "id": "Geofon (Geophone)." },
    { "en": "Mengukur radiasi matahari pada permukaan datar?", "id": "Piranometer (Pyranometer)." },
    { "en": "Perangkat superkonduktor untuk mengukur medan magnet yang sangat lemah?", "id": "SQUID (Superconducting Quantum Interference Device)." },
    { "en": "Konfigurasi amplifier dengan dua transistor bertumpuk untuk meningkatkan bandwidth?", "id": "Amplifier Cascode." },
    { "en": "Osilator yang menggunakan jembatan resistor-kapasitor untuk menghasilkan gelombang sinus?", "id": "Osilator Jembatan Wien." },
    { "en": "Osilator RC yang menggunakan beberapa tingkat untuk menggeser fasa sinyal?", "id": "Osilator Geser Fasa (Phase-Shift Oscillator)." },
    { "en": "Pipa plastik atau akrilik untuk memandu cahaya dari LED di PCB ke panel?", "id": "Pipa Cahaya (Light Pipe)." },
    { "en": "Wadah atau kotak pelindung untuk menempatkan sirkuit elektronik?", "id": "Enclosure / Chassis." },
    { "en": "Segel pelindung untuk mencegah masuknya debu atau cairan ke dalam enclosure?", "id": "Gasket." },
    { "en": "Program yang menerjemahkan kode assembly menjadi kode mesin?", "id": "Assembler." },
    { "en": "Program yang menerjemahkan seluruh kode sumber menjadi kode mesin sebelum eksekusi?", "id": "Compiler." },
    { "en": "Program yang menerjemahkan dan mengeksekusi kode sumber baris per baris?", "id": "Interpreter." },
    { "en": "Menambahkan bit redundan ke data untuk mendeteksi dan memperbaiki kesalahan transmisi?", "id": "Kode Koreksi Kesalahan (Error Correction Code - ECC)." },
    { "en": "Sirkuit digital yang dapat menggeser data beberapa posisi bit dalam satu siklus?", "id": "Barrel Shifter." },
    { "en": "Tampilan elektromekanis yang menggunakan segmen terbalik untuk membentuk karakter?", "id": "Flip-Dot Display." },
    { "en": "Penyearah tegangan tinggi menggunakan uap merkuri dalam tabung hampa?", "id": "Penyearah Busur Merkuri." },
    { "en": "Komponen historis yang berfungsi sebagai penguat menggunakan medan magnet?", "id": "Amplifier Magnetik." },
    { "en": "Penyearah solid-state awal yang terbuat dari lempengan selenium?", "id": "Penyearah Selenium." },
    { "en": "Konektor coaxial yang lebih kecil dari BNC dengan mekanisme sekrup?", "id": "Konektor SMA." },
    { "en": "Konektor pita datar tipis yang umum pada laptop dan ponsel?", "id": "Konektor FPC (Flexible Printed Circuit)." },
    { "en": "Membaca kartu pintar dengan kontak fisik?", "id": "Smart Card Reader." },
    { "en": "Mengomunikasikan data jarak sangat pendek (~10 cm) secara nirkabel?", "id": "NFC (Near-Field Communication)." },
    { "en": "Antena berbentuk persegi pada PCB untuk aplikasi frekuensi tinggi?", "id": "Antena Patch." },
    { "en": "Antena omnidirectional dasar yang terdiri dari dua elemen?", "id": "Antena Dipol." },
    { "en": "Antena directional dengan banyak elemen untuk gain tinggi?", "id": "Antena Yagi-Uda." },
    { "en": "Filter RF yang sangat selektif dan berukuran kecil, sering di ponsel?", "id": "Filter BAW (Bulk Acoustic Wave)." },
    { "en": "Jenis topologi DC-DC converter yang membalikkan polaritas tegangan?", "id": "Inverting Buck-Boost Converter." },
    { "en": "Sirkuit terpadu yang didedikasikan untuk menghasilkan berbagai suara atau musik?", "id": "IC Synthesizer Suara." },
    { "en": "Penguat yang menjaga impedansi input tinggi dan impedansi output rendah?", "id": "Rangkaian Buffer." },
    { "en": "Saklar solid-state untuk sinyal AC, terdiri dari dua MOSFET back-to-back?", "id": "Saklar Analog / Solid-State Relay." },
    { "en": "Menghubungkan jalur ground analog dan digital pada satu titik untuk mengurangi noise?", "id": "Star Grounding Point." },
    { "en": "Mengukur rasio gelombang berdiri pada jalur transmisi RF?", "id": "SWR Meter." },
    { "en": "Menghasilkan sinyal RF dengan frekuensi dan amplitudo yang diketahui untuk pengujian?", "id": "Signal Generator." },
    { "en": "Memberikan umpan balik posisi pada motor atau poros berputar?", "id": "Resolver." },
    { "en": "Mengukur regangan atau deformasi pada suatu benda?", "id": "Strain Gauge." },
    { "en": "Menghasilkan pulsa listrik singkat saat mendeteksi partikel radiasi pengion?", "id": "Geiger-Müller Tube." },
    { "en": "Mendeteksi foton tunggal?", "id": "Photomultiplier Tube (PMT)." },
    { "en": "Tabung vakum yang digunakan sebagai penguat sinyal frekuensi sangat tinggi?", "id": "Traveling-Wave Tube (TWT)." },
    { "en": "Tabung vakum dengan empat elektroda?", "id": "Tabung Tetrode." },
    { "en": "Tabung vakum dengan lima elektroda?", "id": "Tabung Pentode." },
    { "en": "Tampilan digital yang menggunakan filamen panas dalam vakum?", "id": "Nixie Tube." },
    { "en": "Baterai yang menggunakan seng dan karbon sebagai elektroda?", "id": "Baterai Seng-Karbon." },
    { "en": "Baterai alkalin sekali pakai?", "id": "Baterai Alkaline." },
    { "en": "Baterai isi ulang berbasis nikel dan kadmium?", "id": "Baterai NiCd." },
    { "en": "Baterai isi ulang berbasis nikel dan logam hidrida?", "id": "Baterai NiMH." },
    { "en": "Elemen sirkuit ideal yang resistansinya nol dan induktansinya nol?", "id": "Hubung Singkat (Short Circuit)." },
    { "en": "Elemen sirkuit ideal yang tidak mengizinkan arus mengalir sama sekali?", "id": "Rangkaian Terbuka (Open Circuit)." },
    { "en": "Satu set instruksi yang dapat dieksekusi oleh prosesor komputer?", "id": "Kode Mesin (Machine Code)." },
    { "en": "Bahasa pemrograman level rendah yang menggunakan mnemonik?", "id": "Bahasa Assembly." },
    { "en": "Area penyimpanan sementara berkecepatan tinggi di dalam CPU?", "id": "Register CPU." },
    { "en": "Sirkuit yang melakukan penambahan dua bilangan biner?", "id": "Adder." },
    { "en": "Sirkuit yang membandingkan besaran dua bilangan biner?", "id": "Magnitude Comparator." },
    { "en": "Mengatur waktu penyalaan SCR atau TRIAC untuk mengontrol daya?", "id": "Sirkuit Pemicu Fasa (Phase Trigger Circuit)." },
    { "en": "Mengisolasi catu daya dari jala-jala listrik untuk keamanan?", "id": "Transformator Isolasi." },
    { "en": "Menyesuaikan impedansi antara amplifier dan speaker?", "id": "Transformator Output." },
    { "en": "Komponen non-linear dua terminal dengan karakteristik I-V tertentu?", "id": "Memristor." },
    { "en": "Sirkuit yang outputnya sebanding dengan logaritma dari inputnya?", "id": "Logarithmic Amplifier." },
    { "en": "Konektor D-subminiature dengan 25 pin, dulu sering untuk port paralel?", "id": "Konektor DB25." },
    { "en": "Lapisan pada permukaan semikonduktor yang berfungsi sebagai isolator?", "id": "Lapisan Oksida Silikon (SiO2)." },
    { "en": "Proses menambahkan impuritas ke semikonduktor murni untuk mengubah sifat listriknya?", "id": "Doping." },
    { "en": "Wilayah deplesi pada sambungan P-N dioda?", "id": "Daerah Deplesi (Depletion Region)." },
    { "en": "Tegangan minimum yang diperlukan agar dioda mulai menghantar arus?", "id": "Tegangan Maju (Forward Voltage)." },
    { "en": "Arus bocor kecil yang mengalir saat dioda diberi bias mundur?", "id": "Arus Balik (Reverse Current)." },
    { "en": "Kondisi dioda zener saat menghantarkan arus dalam bias mundur?", "id": "Kondisi Avalanche Breakdown." },
    { "en": "Proses rekombinasi elektron dan lubang yang menghasilkan foton pada LED?", "id": "Elektroluminesensi." },
    { "en": "Mengukur sudut absolut dari suatu poros?", "id": "Absolute Encoder." },
    { "en": "Mengukur perubahan sudut atau gerakan relatif dari suatu poros?", "id": "Incremental Encoder." },
    { "en": "Mendeteksi ada atau tidaknya objek menggunakan berkas cahaya?", "id": "Sensor Fotoelektrik." },
    { "en": "Menghasilkan listrik dari perbedaan suhu antara dua sisi?", "id": "Generator Termoelektrik (Efek Seebeck)." },
    { "en": "Kabel yang terdiri dari dua konduktor yang dipilin bersama untuk mengurangi interferensi?", "id": "Kabel Pasangan Berpilin (Twisted Pair)." },
    { "en": "Menampilkan karakter alfanumerik?", "id": "Layar Alfanumerik." },
    { "en": "Terminal pada IC atau komponen untuk menerima atau mengirim sinyal?", "id": "Pin." },
    { "en": "Representasi skematis dari sebuah rangkaian elektronik?", "id": "Diagram Sirkuit." },
    { "en": "Bentuk fisik dan susunan pin dari sebuah komponen elektronik?", "id": "Kemasan (Package)." },
    { "en": "Teknologi pemasangan komponen dengan memasukkan kaki ke lubang PCB?", "id": "Teknologi Through-Hole." },
    { "en": "Teknologi pemasangan komponen dengan menyolder langsung di permukaan PCB?", "id": "Teknologi Surface-Mount (SMT)." },
    { "en": "Alat untuk memegang PCB saat bekerja?", "id": "PCB Holder." },
    { "en": "Lampu dengan lensa untuk membantu melihat komponen kecil?", "id": "Lampu Pembesar (Magnifying Lamp)." },
    { "en": "Menguji kontinuitas atau hubungan singkat dalam sebuah sirkuit?", "id": "Fungsi Kontinuitas (Continuity Test)." },
    { "en": "Penguat yang dirancang khusus untuk sinyal dari instrumen atau sensor?", "id": "Instrumentation Amplifier." },
    { "en": "Sirkuit yang dapat mempertahankan keadaan outputnya bahkan setelah input dilepas?", "id": "Latch." },
    { "en": "Menguji dan memverifikasi fungsionalitas sirkuit terpadu?", "id": "IC Tester." },
    { "en": "Menghasilkan medan listrik yang seragam di antara dua pelat?", "id": "Kapasitor Pelat Paralel." },
    { "en": "Mengubah energi kimia menjadi energi listrik?", "id": "Sel Galvani." },
    { "en": "Menggunakan energi listrik untuk mendorong reaksi kimia yang tidak spontan?", "id": "Sel Elektrolitik." },
    { "en": "Elektronika yang menggunakan putaran (spin) elektron selain muatannya?", "id": "Spintronik." },
    { "en": "Bidang yang mempelajari interaksi cahaya dengan material elektronik?", "id": "Fotonik." },
    { "en": "Saklar yang tidak memiliki bagian bergerak?", "id": "Saklar Solid-State." },
    { "en": "Resistor variabel yang dikendalikan oleh cahaya?", "id": "Photoresistor (LDR)." },
    { "en": "Resistor variabel yang dikendalikan oleh tegangan?", "id": "Varistor (Voltage Dependent Resistor)." },
    { "en": "Fenomena di mana bahan tertentu menghasilkan tegangan saat disinari cahaya?", "id": "Efek Fotovoltaik." },
    { "en": "Efek di mana medan magnet memengaruhi resistansi suatu material?", "id": "Efek Magnetoresistif." },
    { "en": "Emisi elektron dari suatu permukaan saat dipanaskan?", "id": "Emisi Termionik." },
    { "en": "Menjaga agar kedua terminal input op-amp berada pada tegangan yang sama secara virtual?", "id": "Virtual Ground." },
    { "en": "Daftar semua komponen dan jumlahnya yang dibutuhkan untuk merakit sebuah PCB?", "id": "Bill of Materials (BOM)." },
    { "en": "Representasi fisik dan pola pad sebuah komponen dalam software desain PCB?", "id": "Footprint / Land Pattern." },
    { "en": "Proses memeriksa apakah layout PCB telah memenuhi aturan desain yang ditetapkan?", "id": "Design Rule Check (DRC)." },
    { "en": "Register di CPU yang menunjuk ke alamat instruksi berikutnya?", "id": "Program Counter (PC)." },
    { "en": "Register di CPU yang menyimpan instruksi yang sedang dieksekusi?", "id": "Instruction Register (IR)." },
    { "en": "Memori cache khusus di CPU untuk mempercepat penerjemahan alamat virtual ke fisik?", "id": "TLB (Translation Lookaside Buffer)." },
    { "en": "Teknik eksekusi prosesor di mana beberapa instruksi tumpang tindih dalam tahapannya?", "id": "Pipelining." },
    { "en": "Alat sederhana untuk memeriksa level logika (high/low/pulse) pada sirkuit digital?", "id": "Logic Probe." },
    { "en": "Oven khusus untuk menyolder komponen SMT menggunakan pasta solder?", "id": "Reflow Oven." },
    { "en": "Mesin otomatis yang mengambil dan menempatkan komponen SMT ke PCB?", "id": "Pick-and-Place Machine." },
    { "en": "Gelang yang dipakai untuk mencegah kerusakan komponen akibat listrik statis?", "id": "Gelang Antistatik (Antistatic Wrist Strap)." },
    { "en": "Proses matematika untuk menguraikan sinyal menjadi komponen-komponen frekuensinya?", "id": "Transformasi Fourier." },
    { "en": "Operasi matematika yang menggabungkan dua sinyal untuk menghasilkan sinyal ketiga?", "id": "Konvolusi." },
    { "en": "Kondisi di mana sinyal output amplifier terpotong karena melebihi batas tegangan catu daya?", "id": "Clipping." },
    { "en": "Kegagalan semikonduktor akibat tegangan mundur yang berlebihan?", "id": "Avalanche Breakdown." },
    { "en": "Kondisi di mana kenaikan suhu pada transistor menyebabkan kenaikan arus, yang semakin menaikkan suhu?", "id": "Thermal Runaway." },
    { "en": "Sirkuit yang menghasilkan tegangan output beberapa kali lipat dari tegangan input puncaknya?", "id": "Pengganda Tegangan (Voltage Multiplier)." },
    { "en": "Menggunakan kapasitor untuk menaikkan level tegangan referensi secara dinamis?", "id": "Kapasitor Bootstrap." },
    { "en": "Menghilangkan komponen DC dari sebuah sinyal agar hanya AC yang lewat?", "id": "DC Block." },
    { "en": "Penguat yang dirancang khusus untuk sinyal dengan frekuensi sangat rendah (mendekati DC)?", "id": "Chopper Amplifier." },
    { "en": "Saklar elektronik empat kuadran yang dikendalikan oleh sinyal digital?", "id": "Gerbang Transmisi (Transmission Gate)." },
    { "en": "Sirkuit yang menghasilkan output pulsa tunggal dengan durasi tertentu saat dipicu?", "id": "One-Shot Multivibrator." },
    { "en": "Sirkuit osilator yang tegangannya dapat diatur untuk mengubah frekuensi?", "id": "VCO (Voltage-Controlled Oscillator)." },
    { "en": "Sirkuit osilator yang arusnya dapat diatur untuk mengubah frekuensi?", "id": "CCO (Current-Controlled Oscillator)." },
    { "en": "Osilator yang dikunci fasanya ke sinyal referensi eksternal?", "id": "PLL (Phase-Locked Loop)." },
    { "en": "Resistor dengan empat terminal untuk pengukuran resistansi yang sangat presisi?", "id": "Kelvin Resistor." },
    { "en": "Konverter DC-DC non-inverting yang dapat menaikkan atau menurunkan tegangan?", "id": "Konverter SEPIC." },
    { "en": "Metode modulasi dengan mengubah amplitudo sinyal pembawa?", "id": "Modulasi Amplitudo (AM)." },
    { "en": "Metode modulasi dengan mengubah frekuensi sinyal pembawa?", "id": "Modulasi Frekuensi (FM)." },
    { "en": "Metode modulasi dengan mengubah fasa sinyal pembawa?", "id": "Modulasi Fasa (PM)." },
    { "en": "Metode modulasi digital dengan menggeser fasa?", "id": "PSK (Phase-Shift Keying)." },
    { "en": "Metode modulasi digital dengan menggeser frekuensi?", "id": "FSK (Frequency-Shift Keying)." },
    { "en": "Representasi visual dari daya sinyal terhadap frekuensi?", "id": "Spektrum Frekuensi." },
    { "en": "Lebar rentang frekuensi yang dapat dilewatkan oleh filter atau dimiliki sinyal?", "id": "Bandwidth." },
    { "en": "Rasio antara daya sinyal yang diinginkan dan daya derau latar belakang?", "id": "SNR (Signal-to-Noise Ratio)." },
    { "en": "Cacat pada sinyal akibat pemrosesan non-linear?", "id": "Distorsi Harmonik." },
    { "en": "Menghasilkan pulsa pendek dengan daya puncak yang sangat tinggi?", "id": "Generator Pulsa." },
    { "en": "Komponen yang impedansinya dapat diatur secara elektronik?", "id": "Impedansi Variabel." },
    { "en": "Mengukur impedansi kompleks suatu komponen pada berbagai frekuensi?", "id": "Impedance Analyzer." },
    { "en": "Dioda yang terbuat dari Silikon Karbida untuk aplikasi suhu dan daya tinggi?", "id": "Dioda SiC." },
    { "en": "Elemen sirkuit yang perilakunya dijelaskan oleh fluks dan muatan?", "id": "Memristor." },
    { "en": "Titik dalam rangkaian yang memiliki tegangan referensi nol?", "id": "Ground / Tanah." },
    { "en": "Pemisahan antara ground sirkuit analog dan digital untuk mengurangi noise?", "id": "Pemisahan Ground." },
    { "en": "Jalur konduktif yang tidak diinginkan antara dua titik dalam sirkuit?", "id": "Hubung Singkat (Short Circuit)." },
    { "en": "Koneksi yang terputus secara tidak sengaja dalam sebuah sirkuit?", "id": "Rangkaian Terbuka (Open Circuit)." },
    { "en": "Karakteristik non-ideal dari sumber tegangan yang menyebabkan tegangannya turun saat dibebani?", "id": "Resistansi Internal." },
    { "en": "Kemampuan kapasitor untuk menahan perubahan tegangan?", "id": "Kapasitansi." },
    { "en": "Kemampuan induktor untuk menahan perubahan arus?", "id": "Induktansi." },
    { "en": "Hambatan total terhadap arus AC, gabungan dari resistansi dan reaktansi?", "id": "Impedansi." },
    { "en": "Hambatan terhadap arus AC yang disebabkan oleh kapasitor atau induktor?", "id": "Reaktansi." },
    { "en": "Peralatan untuk memprogram perangkat logika seperti CPLD atau FPGA?", "id": "JTAG Programmer." },
    { "en": "Sirkuit yang mendeteksi penurunan tegangan catu daya dan mereset mikrokontroler?", "id": "Brown-out Detector." },
    { "en": "Timer yang akan mereset sistem jika program utama berhenti berjalan (hang)?", "id": "Watchdog Timer." },
    { "en": "Teknik mengubah sinyal analog menjadi sinyal PWM?", "id": "Modulasi Lebar Pulsa (PWM)." },
    { "en": "Konektor yang digunakan untuk menghubungkan antena Wi-Fi eksternal?", "id": "Konektor RP-SMA." },
    { "en": "Catu daya yang dapat menyediakan daya ke perangkat melalui kabel Ethernet?", "id": "PoE (Power over Ethernet) Injector." },
    { "en": "Perangkat yang menerima daya dari kabel Ethernet?", "id": "PoE Splitter." },
    { "en": "Sirkuit pengaman yang memutus daya jika terjadi polaritas terbalik?", "id": "Perlindungan Polaritas Terbalik." },
    { "en": "Konverter DC-DC yang menggunakan satu induktor dan topologi jembatan?", "id": "Full-Bridge Converter." },
    { "en": "Penguat yang menggunakan transformator untuk kopling antar tingkat?", "id": "Transformer-Coupled Amplifier." },
    { "en": "Penguat yang terhubung langsung tanpa kapasitor kopling?", "id": "Direct-Coupled Amplifier." },
    { "en": "Mengukur resistansi tanah untuk sistem grounding?", "id": "Earth Resistance Tester." },
    { "en": "Simulasi komputer terhadap perilaku termal sebuah sirkuit?", "id": "Analisis Termal." },
    { "en": "Lem konduktif yang digunakan untuk merekatkan komponen?", "id": "Epoksi Konduktif." },
    { "en": "Bahan yang digunakan untuk menyerap panas dari komponen dan memindahkannya ke heatsink?", "id": "Thermal Paste." },
    { "en": "Pita perekat dengan konduktivitas termal yang baik?", "id": "Pita Termal (Thermal Tape)." },
    { "en": "Sensor untuk mendeteksi keberadaan medan listrik tanpa sentuhan?", "id": "Sensor Medan Listrik." },
    { "en": "Perangkat yang dapat 'mempelajari' dan meniru sinyal remote control inframerah?", "id": "IR Learner." },
    { "en": "Mengubah satu jenis level logika tegangan ke jenis lain (misal: 5V ke 3.3V)?", "id": "Level Shifter." },
    { "en": "Memori non-volatile yang menggunakan medan magnet untuk menyimpan data?", "id": "Memori Inti Magnetik (Magnetic Core Memory)." },
    { "en": "Memori non-volatile berbasis ferroelektrik?", "id": "FeRAM (Ferroelectric RAM)." },
    { "en": "Sirkuit yang dapat mengenali urutan pulsa digital tertentu?", "id": "Sequence Detector." },
    { "en": "Protokol standar untuk komunikasi nirkabel berdaya rendah di area personal?", "id": "Zigbee." },
    { "en": "Jaringan area luas berdaya rendah untuk perangkat IoT?", "id": "LPWAN (Contoh: LoRaWAN)." },
    { "en": "Sel bahan bakar yang menghasilkan listrik dari reaksi hidrogen dan oksigen?", "id": "Fuel Cell." },
    { "en": "Kapasitor dengan kapasitansi sangat besar, digunakan sebagai pengganti baterai kecil?", "id": "Superkapasitor (Ultracapacitor)." },
    { "en": "Mengukur medan elektromagnetik di sekitar perangkat?", "id": "EMF Meter." },
    { "en": "Komponen yang dirancang untuk gagal pada kondisi tertentu demi melindungi sisa rangkaian?", "id": "Elemen Korban (Sacrificial Component)." },
    { "en": "Menghubungkan sinyal antara sirkuit dengan ground yang berbeda?", "id": "Isolator Digital." },
    { "en": "Catu daya yang tidak terganggu oleh pemadaman listrik singkat?", "id": "UPS (Uninterruptible Power Supply)." },
    { "en": "Memvalidasi sinyal input berada dalam rentang tegangan yang diharapkan?", "id": "Window Comparator." },
    { "en": "Menghasilkan pulsa output setiap kali input melewati level nol?", "id": "Zero-Crossing Detector." },
    { "en": "Sistem umpan balik untuk mengontrol posisi suatu aktuator?", "id": "Sistem Kontrol Servo." },
    { "en": "Mencocokkan satu sinyal dengan template yang tersimpan?", "id": "Matched Filter." },
    { "en": "Cairan pelindung yang disemprotkan pada PCB untuk melindunginya dari lingkungan?", "id": "Conformal Coating." },
    { "en": "Pengujian fungsional pada PCB rakitan?", "id": "In-Circuit Testing (ICT)." },
    { "en": "Representasi grafis dari hubungan antara tegangan dan arus dalam komponen?", "id": "Kurva I-V." },
    { "en": "Rasio antara perubahan output dan perubahan input dalam suatu sistem?", "id": "Gain / Penguatan." },
    { "en": "Kemampuan sistem untuk menolak sinyal yang tidak diinginkan?", "id": "Rejeksi (Rejection)." },
    { "en": "Waktu yang dibutuhkan sinyal untuk merambat dari input ke output?", "id": "Waktu Tunda Perambatan (Propagation Delay)." },
    { "en": "Kecenderungan osilator untuk frekuensinya bergeser karena suhu atau penuaan?", "id": "Drift Frekuensi." },
    { "en": "Menentukan kecepatan maksimum perubahan tegangan output pada op-amp?", "id": "Slew Rate." },
    { "en": "Tegangan DC kecil yang harus diterapkan pada input op-amp untuk membuat outputnya nol?", "id": "Tegangan Offset Input." },
    { "en": "Arus DC kecil yang mengalir ke dalam terminal input op-amp?", "id": "Arus Bias Input." },
    { "en": "Batas tegangan gerbang-sumber (Vgs) pada MOSFET agar mulai menghantar?", "id": "Tegangan Ambang (Threshold Voltage)." },
    { "en": "Resistansi antara terminal Drain dan Source saat MOSFET dalam keadaan ON?", "id": "RDS(on)." },
    { "en": "File standar industri yang menggambarkan setiap lapisan dari desain PCB?", "id": "File Gerber." },
    { "en": "Daftar koneksi logis antara pin-pin komponen dalam sebuah desain skematik?", "id": "Netlist." },
    { "en": "Proses menyolder komponen SMT secara massal menggunakan udara panas?", "id": "Reflow Soldering." },
    { "en": "Proses menyolder komponen through-hole secara massal menggunakan gelombang timah cair?", "id": "Wave Soldering." },
    { "en": "Pasta yang terdiri dari bola timah kecil dan fluks, digunakan untuk SMT?", "id": "Pasta Solder." },
    { "en": "Algoritma kontrol umpan balik untuk meminimalkan kesalahan antara setpoint dan output?", "id": "Kontroler PID." },
    { "en": "Algoritma untuk mengestimasi keadaan suatu sistem dari serangkaian pengukuran yang bernoise?", "id": "Filter Kalman." },
    { "en": "Nilai sederhana yang dihitung dari blok data untuk mendeteksi kesalahan?", "id": "Checksum." },
    { "en": "Teknik untuk menjalankan tugas-tugas berbeda secara bersamaan pada prosesor?", "id": "Multitasking." },
    { "en": "Penguat berdaya tinggi yang digunakan pada tahap akhir sebuah pemancar RF?", "id": "HPA (High-Power Amplifier)." },
    { "en": "Antena yang terdiri dari beberapa elemen identik untuk membentuk pola radiasi terarah?", "id": "Antenna Array." },
    { "en": "Struktur fisik yang digunakan untuk memandu gelombang elektromagnetik?", "id": "Waveguide." },
    { "en": "Komponen RF yang memisahkan atau menggabungkan sinyal berdasarkan polarisasinya?", "id": "Orthomode Transducer." },
    { "en": "Memori di dalam FPGA yang mengimplementasikan fungsi logika kombinasional?", "id": "Look-up Table (LUT)." },
    { "en": "Sirkuit logika yang dapat diprogram untuk mengimplementasikan fungsi boolean?", "id": "PLA (Programmable Logic Array)." },
    { "en": "Tegangan AC sisa yang kecil pada output catu daya DC?", "id": "Tegangan Ripple." },
    { "en": "Kemampuan catu daya untuk mempertahankan tegangan outputnya meskipun beban berubah?", "id": "Regulasi Beban (Load Regulation)." },
    { "en": "Kemampuan catu daya untuk mempertahankan outputnya meskipun tegangan input berubah?", "id": "Regulasi Jalur (Line Regulation)." },
    { "en": "Rangkaian yang mengubah bentuk sinyal, misalnya dari sinus ke kotak?", "id": "Waveform Shaper." },
    { "en": "Sirkuit yang menghasilkan tegangan output yang sebanding dengan akar kuadrat dari input?", "id": "Square Root Circuit." },
    { "en": "Elektronika yang beroperasi pada suhu sangat rendah (kriogenik)?", "id": "Cryoelectronics." },
    { "en": "Fenomena kuantum tunneling yang digunakan dalam SQUID dan dioda tunnel?", "id": "Efek Josephson." },
    { "en": "Perangkat lunak yang memungkinkan satu komputer meniru sistem komputer lain?", "id": "Emulator." },
    { "en": "Area pada die semikonduktor yang didedikasikan untuk sambungan kawat?", "id": "Bonding Pad." },
    { "en": "Proses menyambungkan kawat emas atau aluminium yang sangat tipis dari IC ke pin kemasan?", "id": "Wire Bonding." },
    { "en": "Metode untuk memverifikasi desain sirkuit digital menggunakan bahasa deskripsi perangkat keras?", "id": "Simulasi RTL (Register-Transfer Level)." },
    { "en": "Bahasa standar untuk mendeskripsikan perilaku sirkuit digital?", "id": "VHDL atau Verilog." },
    { "en": "Sirkuit pengaman yang memutus daya jika arus melebihi ambang batas untuk waktu singkat?", "id": "Overcurrent Protection." },
    { "en": "Sirkuit pengaman yang memutus daya jika tegangan input melebihi ambang batas?", "id": "Overvoltage Protection." },
    { "en": "Teknik transmisi data di mana beberapa sinyal berbagi satu media dengan membagi slot waktu?", "id": "TDM (Time-Division Multiplexing)." },
    { "en": "Teknik transmisi data di mana beberapa sinyal berbagi satu media dengan membagi pita frekuensi?", "id": "FDM (Frequency-Division Multiplexing)." },
    { "en": "Karakteristik amplifier yang menggambarkan rentang frekuensi di mana gain-nya konstan?", "id": "Gain-Bandwidth Product." },
    { "en": "Rasio antara gain loop terbuka dan gain loop tertutup dalam amplifier umpan balik?", "id": "Faktor Umpan Balik (Feedback Factor)." },
    { "en": "Ukuran seberapa baik amplifier diferensial menolak sinyal yang sama di kedua inputnya?", "id": "CMRR (Common-Mode Rejection Ratio)." },
    { "en": "Ukuran seberapa baik regulator tegangan menolak variasi pada inputnya?", "id": "PSRR (Power Supply Rejection Ratio)." },
    { "en": "Struktur berulang dalam desain IC, seperti sel memori atau gerbang logika?", "id": "Sel Standar (Standard Cell)." },
    { "en": "Jaringan yang mendistribusikan sinyal clock ke seluruh bagian chip dengan tunda yang seragam?", "id": "Pohon Clock (Clock Tree)." },
    { "en": "Distorsi sinyal digital di mana tepi sinyal tidak menentu dalam waktu?", "id": "Jitter." },
    { "en": "Perbedaan waktu kedatangan sinyal clock pada titik-titik yang berbeda dalam sirkuit?", "id": "Clock Skew." },
    { "en": "Proses fabrikasi untuk membuat struktur berukuran nanometer?", "id": "Nanofabrikasi." },
    { "en": "Teknik menumbuhkan lapisan tipis kristal pada substrat?", "id": "Epitaksi." },
    { "en": "Proses mentransfer pola ke wafer semikonduktor menggunakan cahaya?", "id": "Fotolitografi." },
    { "en": "Mengukur ketebalan lapisan tipis menggunakan elipsometri?", "id": "Ellipsometer." },
    { "en": "Mikroskop yang menggunakan elektron untuk menghasilkan gambar beresolusi sangat tinggi?", "id": "Mikroskop Elektron." },
    { "en": "Alat untuk mengkarakterisasi permukaan material pada skala atom?", "id": "AFM (Atomic Force Microscope)." },
    { "en": "Titik referensi tegangan pada sirkuit yang tidak terhubung ke ground bumi?", "id": "Ground Mengambang (Floating Ground)." },
    { "en": "Kopling yang tidak diinginkan antara sirkuit melalui kapasitansi parasit?", "id": "Kopling Kapasitif." },
    { "en": "Kopling yang tidak diinginkan antara sirkuit melalui induktansi parasit?", "id": "Kopling Induktif." },
    { "en": "Bahan yang tidak menghantarkan listrik sama sekali?", "id": "Isolator / Dielektrik." },
    { "en": "Bahan yang sifat konduktivitasnya dapat diubah?", "id": "Semikonduktor." },
    { "en": "Bahan yang menghantarkan listrik dengan sangat baik?", "id": "Konduktor." },
    { "en": "Bahan yang menjadi superkonduktor pada suhu yang relatif tinggi?", "id": "Superkonduktor Suhu Tinggi." },
    { "en": "Perangkat yang mengukur daya optik dari sinar laser atau sumber cahaya lain?", "id": "Optical Power Meter." },
    { "en": "Menghasilkan pulsa cahaya yang sangat singkat (femtosekon atau pikosekon)?", "id": "Laser Ultrafast." },
    { "en": "Peralatan untuk menyambungkan dua ujung serat optik secara permanen?", "id": "Fusion Splicer." },
    { "en": "Elektronika yang dapat diregangkan atau ditekuk tanpa rusak?", "id": "Elektronika Fleksibel." },
    { "en": "Elektronika yang dicetak di atas substrat seperti plastik atau kertas?", "id": "Elektronika Cetak (Printed Electronics)." },
    { "en": "Komponen elektronik yang dapat terurai secara hayati?", "id": "Elektronika Biodegradable." },
    { "en": "Bidang yang menggabungkan biologi dan elektronika?", "id": "Bioelektronika." },
    { "en": "Sensor yang menggunakan reaksi biologis untuk mendeteksi zat kimia?", "id": "Biosensor." },
    { "en": "Implan elektronik yang berinteraksi dengan sistem saraf?", "id": "Antarmuka Saraf (Neural Interface)." },
    { "en": "Prosesor sinyal digital yang dioptimalkan untuk pemrosesan sinyal real-time?", "id": "DSP (Digital Signal Processor)." },
    { "en": "Jenis memori flash yang lebih cepat dan tahan lama?", "id": "SLC (Single-Level Cell) NAND Flash." },
    { "en": "Jenis memori flash dengan kepadatan lebih tinggi tetapi kecepatan lebih rendah?", "id": "MLC/TLC/QLC NAND Flash." },
    { "en": "Jalur transmisi yang dibuat pada PCB untuk sinyal frekuensi sangat tinggi?", "id": "Microstrip." },
    { "en": "Mode operasi transistor di mana ia beroperasi di antara sepenuhnya ON dan OFF?", "id": "Daerah Linier / Aktif." },
    { "en": "Mode operasi transistor di mana ia sepenuhnya ON seperti saklar tertutup?", "id": "Daerah Saturasi." },
    { "en": "Mode operasi transistor di mana ia sepenuhnya OFF seperti saklar terbuka?", "id": "Daerah Cut-off." },
    { "en": "Kapasitansi parasit antara dua konduktor yang berdekatan?", "id": "Kapasitansi liar (Stray Capacitance)." },
    { "en": "Model rangkaian ekuivalen dari komponen nyata untuk analisis?", "id": "Model Rangkaian." },
    { "en": "Representasi matematis dari perilaku input-output suatu sistem?", "id": "Fungsi Transfer." },
    { "en": "Respons frekuensi suatu sistem dalam bentuk magnitudo dan fasa?", "id": "Bode Plot." },
    { "en": "Grafik yang menunjukkan lokasi pole dan zero dari suatu fungsi transfer?", "id": "Pole-Zero Plot." },
    { "en": "Sistem kontrol yang menggunakan informasi dari output untuk mengatur input?", "id": "Sistem Loop Tertutup." },
    { "en": "Sistem kontrol yang operasinya tidak bergantung pada output?", "id": "Sistem Loop Terbuka." },
    { "en": "Soket pada CPU atau FPGA dengan banyak pin kecil yang menggunakan pegas?", "id": "LGA (Land Grid Array)." },
    { "en": "Kemasan IC di mana pin-pinnya tersusun dalam matriks bola solder di bawahnya?", "id": "BGA (Ball Grid Array)." },
    { "en": "Kemasan IC dengan pin di keempat sisinya?", "id": "QFP (Quad Flat Package)." },
    { "en": "Kemasan IC dengan dua baris pin paralel?", "id": "DIP (Dual In-line Package)." },
    { "en": "Konektor industri berbentuk bulat yang tahan lama?", "id": "Konektor M12/M8." },
    { "en": "Protokol komunikasi nirkabel untuk otomatisasi rumah?", "id": "Z-Wave." },
    { "en": "Standar IEEE untuk Jaringan Area Lokal Nirkabel (WLAN)?", "id": "Standar 802.11." },
    { "en": "Standar IEEE untuk Jaringan Area Personal Nirkabel (WPAN)?", "id": "Standar 802.15 (Contoh: Bluetooth)." },
    { "en": "Standar IEEE untuk Jaringan Area Lokal (LAN) berkabel?", "id": "Standar 802.3 (Ethernet)." },
    { "en": "Membaca kode baris satu dimensi?", "id": "Barcode Scanner." },
    { "en": "Membaca kode matriks dua dimensi?", "id": "QR Code Reader." },
    { "en": "Menghasilkan medan magnet bolak-balik untuk memanaskan logam?", "id": "Pemanas Induksi." },
    { "en": "Perangkat yang mengubah gerakan manusia menjadi energi listrik?", "id": "Energy Harvester." },
    { "en": "Baterai yang sangat tipis dan fleksibel?", "id": "Thin-Film Battery." },
    { "en": "Sirkuit terpadu yang dirancang untuk beroperasi di lingkungan radiasi tinggi?", "id": "Radiation-Hardened IC." },
    { "en": "Catu daya yang diisolasi secara medis untuk digunakan pada pasien?", "id": "Medical-Grade Power Supply." },
    { "en": "Unit perangkat keras yang mengelola permintaan akses memori dari CPU?", "id": "MMU (Memory Management Unit)." },
    { "en": "Unit yang memungkinkan periferal mengakses memori utama secara langsung tanpa melibatkan CPU?", "id": "DMA (Direct Memory Access) Controller." },
    { "en": "Sirkuit yang digunakan untuk memperbaiki faktor daya sebuah catu daya?", "id": "PFC (Power Factor Correction) Circuit." },
    { "en": "Sirkuit pada input catu daya untuk membatasi lonjakan arus saat pertama kali dinyalakan?", "id": "Inrush Current Limiter." },
    { "en": "Kemampuan material untuk menghasilkan tegangan sebagai respons terhadap perubahan suhu?", "id": "Efek Piroelektrik." },
    { "en": "Perubahan resistivitas suatu semikonduktor ketika dikenai regangan mekanis?", "id": "Efek Piezoresistif." },
    { "en": "Sensor radiasi termal sensitif yang mengukur pemanasan pada elemen penyerap?", "id": "Bolometer." },
    { "en": "Satuan dasar informasi dalam komputasi kuantum?", "id": "Qubit." },
    { "en": "Gangguan akibat sinyal pada satu jalur yang menginduksi sinyal pada jalur di sebelahnya?", "id": "Crosstalk." },
    { "en": "Resistor yang ditempatkan di ujung jalur transmisi untuk mencegah pantulan sinyal?", "id": "Resistor Terminator." },
    { "en": "Tanda referensi optik pada PCB untuk kalibrasi mesin perakitan otomatis?", "id": "Fiducial Mark." },
    { "en": "Keadaan output digital yang bukan HIGH atau LOW (impedansi tinggi)?", "id": "Keadaan Tri-state." },
    { "en": "Proses menyalakan berbagai bagian sirkuit dalam urutan yang telah ditentukan?", "id": "Power-Up Sequencing." },
    { "en": "Cincin konduktif pada isolator tegangan tinggi untuk mengontrol medan listrik?", "id": "Cincin Korona (Corona Ring)." },
    { "en": "Isolator yang memungkinkan konduktor bertegangan tinggi melewati partisi yang di-ground?", "id": "Bushing." },
    { "en": "Osilator kristal yang suhunya dikontrol oleh oven untuk stabilitas frekuensi maksimum?", "id": "OCXO (Oven-Controlled Crystal Oscillator)." },
    { "en": "Osilator kristal yang diberi kompensasi suhu secara elektronik?", "id": "TCXO (Temperature-Compensated Crystal Oscillator)." },
    { "en": "Metode untuk merepresentasikan angka negatif dalam sistem biner?", "id": "Komplemen Dua (Two's Complement)." },
    { "en": "Sirkuit logika yang dapat menjalankan beberapa fungsi berbeda tergantung sinyal kontrol?", "id": "Arithmetic Logic Unit (ALU)." },
    { "en": "Memori yang dapat diakses dalam urutan apapun dengan waktu akses yang hampir sama?", "id": "RAM (Random Access Memory)." },
    { "en": "Memori yang harus dibaca secara berurutan dari awal?", "id": "Sequential Access Memory (Contoh: Pita Magnetik)." },
    { "en": "Cache memori yang terletak di dalam chip prosesor itu sendiri?", "id": "Cache L1/L2/L3." },
    { "en": "Teknik menggunakan disk penyimpanan sebagai perpanjangan dari RAM?", "id": "Memori Virtual." },
    { "en": "Sirkuit yang menyegarkan sel-sel memori DRAM secara periodik?", "id": "Kontroler Refresh DRAM." },
    { "en": "Menghasilkan spektrum sinyal palsu akibat laju sampling yang terlalu rendah?", "id": "Aliasing." },
    { "en": "Frekuensi minimum yang diperlukan untuk sampling sinyal tanpa aliasing?", "id": "Frekuensi Nyquist." },
    { "en": "Filter analog yang digunakan sebelum proses sampling ADC untuk mencegah aliasing?", "id": "Filter Anti-aliasing." },
    { "en": "Jumlah bit yang digunakan untuk merepresentasikan sampel sinyal analog?", "id": "Resolusi ADC / Kuantisasi." },
    { "en": "Kesalahan yang timbul akibat pembulatan sinyal analog ke level digital terdekat?", "id": "Derau Kuantisasi (Quantization Noise)." },
    { "en": "Gelombang kejut sonik yang dihasilkan oleh objek yang bergerak lebih cepat dari suara?", "id": "Sonic Boom." },
    { "en": "Perangkat yang mengubah satu bentuk energi ke bentuk energi lain?", "id": "Transduser." },
    { "en": "Perangkat yang merasakan suatu besaran fisika dan meresponsnya?", "id": "Sensor." },
    { "en": "Perangkat yang menghasilkan gerakan atau aksi berdasarkan sinyal input?", "id": "Aktuator." },
    { "en": "Karakteristik sirkuit yang menunjukkan seberapa cepat ia merespons perubahan mendadak?", "id": "Respons Transien." },
    { "en": "Karakteristik sirkuit setelah semua efek transien menghilang?", "id": "Respons Keadaan Stabil (Steady-State)." },
    { "en": "Perangkat lunak yang digunakan untuk merancang dan mensimulasikan sirkuit?", "id": "EDA (Electronic Design Automation) Tool." },
    { "en": "Bahasa deskripsi perangkat keras untuk memodelkan sirkuit digital?", "id": "HDL (Hardware Description Language)." },
    { "en": "Tingkat abstraksi desain digital yang berfokus pada aliran data antar register?", "id": "RTL (Register-Transfer Level)." },
    { "en": "Tingkat abstraksi desain digital yang berfokus pada interkoneksi gerbang logika?", "id": "Gate-Level." },
    { "en": "Model matematis dari komputasi?", "id": "Mesin Turing." },
    { "en": "Sifat suatu material untuk menahan pembentukan medan listrik?", "id": "Permitivitas." },
    { "en": "Sifat suatu material untuk mendukung pembentukan medan magnet?", "id": "Permeabilitas." },
    { "en": "Bahan dengan permeabilitas magnetik negatif?", "id": "Bahan Diamagnetik." },
    { "en": "Bahan yang ditarik lemah oleh medan magnet?", "id": "Bahan Paramagnetik." },
    { "en": "Bahan yang dapat menjadi magnet permanen?", "id": "Bahan Feromagnetik." },
    { "en": "Suhu di mana bahan feromagnetik kehilangan sifat magnet permanennya?", "id": "Suhu Curie." },
    { "en": "Pembangkitan arus listrik dalam konduktor yang bergerak melalui medan magnet?", "id": "Induksi Elektromagnetik." },
    { "en": "Pembangkitan tegangan pada konduktor yang dialiri arus dalam medan magnet?", "id": "Efek Hall." },
    { "en": "Perangkat yang memanfaatkan efek Hall untuk mendeteksi medan magnet?", "id": "Sensor Efek Hall." },
    { "en": "Jenis motor listrik yang menggunakan elektromagnet pada stator dan rotor?", "id": "Motor Universal." },
    { "en": "Jenis motor DC tanpa sikat karbon?", "id": "Motor DC Tanpa Sikat (BLDC)." },
    { "en": "Representasi grafis dari impedansi sirkuit terhadap frekuensi?", "id": "Diagram Smith." },
    { "en": "Parameter yang menggambarkan pantulan sinyal pada jaringan RF?", "id": "Parameter S (S-parameters)." },
    { "en": "Penguat RF yang dirancang untuk beroperasi pada daya output yang bervariasi?", "id": "Variable Gain Amplifier (VGA)." },
    { "en": "Penguat RF yang dirancang untuk menahan sinyal input yang sangat besar tanpa distorsi?", "id": "High-Linearity Amplifier." },
    { "en": "Sirkuit yang mendeteksi daya sinyal RF?", "id": "Detektor RF." },
    { "en": "Penguat yang digunakan untuk menggerakkan sinyal ke jalur transmisi?", "id": "Line Driver." },
    { "en": "Penerima yang digunakan untuk menerima sinyal dari jalur transmisi?", "id": "Line Receiver." },
    { "en": "Standar komunikasi serial diferensial untuk jarak jauh dan kecepatan tinggi?", "id": "RS-485." },
    { "en": "Lapisan terendah dari model OSI yang berhubungan dengan transmisi bit fisik?", "id": "Lapisan Fisik (Physical Layer)." },
    { "en": "Lapisan model OSI yang mengelola framing data dan kontrol akses media?", "id": "Lapisan Tautan Data (Data Link Layer)." },
    { "en": "Alamat unik yang ditetapkan pada kartu jaringan (NIC)?", "id": "Alamat MAC." },
    { "en": "Proses menumpuk beberapa chip semikonduktor dalam satu kemasan?", "id": "3D IC Stacking." },
    { "en": "Wafer semikonduktor tipis yang menjadi dasar pembuatan IC?", "id": "Substrat Silikon." },
    { "en": "Ruangan yang sangat bersih untuk fabrikasi semikonduktor?", "id": "Cleanroom." },
    { "en": "Proses pengemasan dan penyegelan die IC untuk melindunginya?", "id": "Enkapsulasi." },
    { "en": "Metode untuk memverifikasi fungsionalitas desain sebelum fabrikasi?", "id": "Verifikasi Desain." },
    { "en": "Proses memeriksa kesesuaian fisik layout IC dengan aturan manufaktur?", "id": "Verifikasi Fisik." },
    { "en": "Komponen yang dapat menyimpan dan melepaskan energi mekanis seperti pegas?", "id": "Resonator Mekanis." },
    { "en": "Resonator berukuran mikro yang dibuat dengan teknologi MEMS?", "id": "Resonator MEMS." },
    { "en": "Teknik untuk mengukur jarak dengan mengirimkan pulsa cahaya dan mengukur waktu pantulnya?", "id": "LiDAR (Light Detection and Ranging)." },
    { "en": "Teknik untuk mengukur jarak dengan mengirimkan gelombang radio dan mengukur waktu pantulnya?", "id": "RADAR (Radio Detection and Ranging)." },
    { "en": "Sistem audio yang menciptakan ilusi suara tiga dimensi?", "id": "Audio Spasial." },
    { "en": "Respons suatu komponen terhadap pulsa yang sangat singkat?", "id": "Respons Impuls." },
    { "en": "Sistem yang propertinya tidak berubah seiring waktu?", "id": "Sistem Invarian Waktu (Time-Invariant System)." },
    { "en": "Sistem di mana outputnya sebanding langsung dengan inputnya?", "id": "Sistem Linier." },
    { "en": "Teknik memodulasi beberapa sinyal ke dalam satu pembawa gelombang?", "id": "Modulasi Multipleks." },
    { "en": "Keluaran yang tidak diinginkan dari sebuah mixer selain frekuensi jumlah dan selisih?", "id": "Produk Intermodulasi." },
    { "en": "Kemampuan penerima untuk membedakan antara sinyal yang diinginkan dan sinyal lain di frekuensi terdekat?", "id": "Selektivitas." },
    { "en": "Ukuran seberapa lemah sinyal yang masih bisa dideteksi oleh penerima?", "id": "Sensitivitas." },
    { "en": "Rentang antara sinyal terlemah dan terkuat yang dapat diolah sistem tanpa distorsi?", "id": "Rentang Dinamis (Dynamic Range)." },
    { "en": "Papan sirkuit tambahan yang dipasang di atas papan utama untuk menambah fungsionalitas?", "id": "Mezzanine Board / Shield." },
    { "en": "Papan sirkuit besar dengan banyak slot untuk menampung beberapa papan sirkuit lainnya?", "id": "Backplane." },
    { "en": "Cairan non-konduktif yang digunakan untuk mendinginkan komponen dengan merendamnya?", "id": "Pendinginan Imersi." },
    { "en": "Baterai yang diaktifkan dengan menambahkan air?", "id": "Water-Activated Battery." },
    { "en": "Baterai yang menggunakan logam cair sebagai elektroda?", "id": "Liquid Metal Battery." },
    { "en": "Baterai yang menggunakan seng dan udara sebagai reaktan?", "id": "Baterai Seng-Udara." },
    { "en": "Struktur pada semikonduktor yang membatasi pergerakan partikel dalam satu atau lebih dimensi?", "id": "Quantum Well / Quantum Dot." },
    { "en": "Bahan yang dirancang untuk memiliki sifat elektromagnetik yang tidak ditemukan di alam?", "id": "Metamaterial." },
    { "en": "Penggunaan elektronika untuk mengendalikan daya dan aliran listrik?", "id": "Elektronika Daya." },
    { "en": "Studi tentang elektron dalam vakum, bukan dalam material padat?", "id": "Elektronika Vakum." },
    { "en": "Sirkuit yang dapat melakukan perhitungan menggunakan variabel kontinu?", "id": "Komputer Analog." },
    { "en": "Arus parasit yang mengalir antara komponen pada substrat semikonduktor?", "id": "Latch-up." },
    { "en": "Perangkat lunak yang memungkinkan kontrol jarak jauh dan pemantauan sistem?", "id": "SCADA (Supervisory Control and Data Acquisition)." },
    { "en": "Sistem terdistribusi yang bekerja bersama untuk melakukan tugas komputasi tunggal?", "id": "Cluster Computing." },
    { "en": "Model komputasi yang menggunakan sumber daya terdistribusi melalui jaringan?", "id": "Grid Computing." },
    { "en": "Rangkaian jembatan untuk mengukur nilai resistansi yang tidak diketahui secara presisi?", "id": "Jembatan Wheatstone." },
    { "en": "Proses menumbuhkan lapisan tipis pada substrat dengan mereaksikan gas-gas kimia?", "id": "CVD (Chemical Vapor Deposition)." },
    { "en": "Proses meratakan permukaan wafer semikonduktor menggunakan kombinasi aksi kimia dan mekanis?", "id": "CMP (Chemical-Mechanical Polishing)." },
    { "en": "Proses menembakkan ion ke dalam substrat semikonduktor untuk doping?", "id": "Implantasi Ion." },
    { "en": "Filter digital yang outputnya bergantung pada input saat ini dan sebelumnya?", "id": "Filter FIR (Finite Impulse Response)." },
    { "en": "Filter digital yang outputnya bergantung pada input dan juga output sebelumnya (rekursif)?", "id": "Filter IIR (Infinite Impulse Response)." },
    { "en": "Arsitektur set instruksi dengan instruksi-instruksi sederhana dan berukuran tetap?", "id": "RISC (Reduced Instruction Set Computer)." },
    { "en": "Arsitektur set instruksi dengan instruksi-instruksi kompleks yang dapat melakukan beberapa operasi?", "id": "CISC (Complex Instruction Set Computer)." },
    { "en": "Mekanisme prosesor untuk menebak arah percabangan dalam kode sebelum dieksekusi?", "id": "Prediksi Cabang (Branch Prediction)." },
    { "en": "Menyediakan tegangan DC melalui kabel audio untuk memberi daya pada mikrofon kondensor?", "id": "Phantom Power." },
    { "en": "Mengubah sinyal audio tidak seimbang (unbalanced) dari instrumen menjadi sinyal seimbang (balanced)?", "id": "DI Box (Direct Box)." },
    { "en": "Waktu rata-rata yang diperkirakan sebelum sebuah sistem yang dapat diperbaiki mengalami kegagalan?", "id": "MTBF (Mean Time Between Failures)." },
    { "en": "Perpindahan material pada konduktor akibat aliran elektron, yang menyebabkan kegagalan?", "id": "Elektromigrasi." },
    { "en": "Amplifier yang dapat mengekstrak sinyal dari lingkungan yang sangat bernoise dengan menggunakan frekuensi referensi?", "id": "Lock-in Amplifier." },
    { "en": "Blok silikon individual dari sebuah wafer setelah dipotong?", "id": "Die." },
    { "en": "Proses memotong wafer semikonduktor menjadi chip-chip individual?", "id": "Dicing." },
    { "en": "Kerangka logam internal pada kemasan IC yang menghubungkan die ke pin eksternal?", "id": "Leadframe." },
    { "en": "Metode pemasangan chip semikonduktor secara terbalik langsung ke substrat?", "id": "Flip-chip." },
    { "en": "Jaringan distribusi daya pada PCB untuk memastikan tegangan stabil di seluruh papan?", "id": "PDN (Power Distribution Network)." },
    { "en": "Protokol untuk menjaga konsistensi data antara berbagai cache dalam sistem multiprosesor?", "id": "Protokol Koherensi Cache." },
    { "en": "Sinyal audio yang menggunakan tiga konduktor (positif, negatif, ground) untuk menolak noise?", "id": "Sinyal Audio Seimbang (Balanced Audio)." },
    { "en": "Osilator yang menghasilkan dua gelombang sinus yang berbeda fasa 90 derajat?", "id": "Osilator Kuadratur." },
    { "en": "Sirkuit yang menghasilkan arus konstan terlepas dari perubahan tegangan atau beban?", "id": "Sumber Arus (Current Source)." },
    { "en": "Sirkuit yang menghasilkan tegangan konstan terlepas dari perubahan arus?", "id": "Sumber Tegangan (Voltage Source)." },
    { "en": "Transformasi matematika untuk menganalisis sirkuit dalam domain frekuensi kompleks?", "id": "Transformasi Laplace." },
    { "en": "Transformasi matematika untuk menganalisis sistem waktu-diskrit?", "id": "Transformasi Z." },
    { "en": "Sifat material untuk menghasilkan medan magnet sebagai respons terhadap medan listrik yang berubah?", "id": "Hukum Induksi Faraday." },
    { "en": "Satu set persamaan fundamental yang mendeskripsikan elektromagnetisme?", "id": "Persamaan Maxwell." },
    { "en": "Emisi cahaya yang dirangsang oleh foton yang lewat, dasar kerja laser?", "id": "Emisi Terstimulasi (Stimulated Emission)." },
    { "en": "Kondisi di mana lebih banyak elektron berada pada tingkat energi yang lebih tinggi daripada yang lebih rendah?", "id": "Inversi Populasi (Population Inversion)." },
    { "en": "Rongga optik yang terdiri dari dua cermin untuk memperkuat cahaya dalam laser?", "id": "Resonator Optik." },
    { "en": "Mendeteksi partikel bermuatan dengan mengamati jejak uap yang terkondensasi?", "id": "Kamar Kabut (Cloud Chamber)." },
    { "en": "Bahan yang memancarkan cahaya saat terkena radiasi pengion?", "id": "Sintilator." },
    { "en": "Teknik untuk mendinginkan atom menggunakan sinar laser?", "id": "Pendinginan Laser." },
    { "en": "Perangkat yang mengukur spektrum massa dari suatu sampel?", "id": "Spektrometer Massa." },
    { "en": "Perangkat untuk mengukur spektrum cahaya?", "id": "Spektrometer." },
    { "en": "Jaringan saraf tiruan yang digunakan dalam pemrosesan data sekuensial?", "id": "RNN (Recurrent Neural Network)." },
    { "en": "Arsitektur jaringan saraf untuk pengenalan gambar?", "id": "CNN (Convolutional Neural Network)." },
    { "en": "Sistem komputasi yang meniru otak manusia?", "id": "Komputasi Neuromorfik." },
    { "en": "Penggunaan superposisi dan keterkaitan kuantum untuk melakukan komputasi?", "id": "Komputasi Kuantum." },
    { "en": "Lapisan tipis anti-pantul pada lensa atau sel surya?", "id": "Lapisan Anti-Refleksi." },
    { "en": "Sinyal yang level tegangannya hanya bisa berupa salah satu dari beberapa nilai diskrit?", "id": "Sinyal Digital." },
    { "en": "Sinyal yang level tegangannya bisa memiliki nilai kontinu dalam suatu rentang?", "id": "Sinyal Analog." },
    { "en": "Pita frekuensi yang dialokasikan untuk penggunaan tanpa lisensi (misal: Wi-Fi, Bluetooth)?", "id": "Pita ISM (Industrial, Scientific, and Medical)." },
    { "en": "Teknik menyebar sinyal radio ke pita frekuensi yang lebih lebar untuk ketahanan noise?", "id": "Spektrum Tersebar (Spread Spectrum)." },
    { "en": "Sirkuit terpadu yang menggabungkan fungsi analog dan digital dalam satu chip?", "id": "IC Sinyal Campuran (Mixed-Signal IC)." },
    { "en": "Perangkat lunak yang memungkinkan interaksi dan debugging perangkat keras secara real-time?", "id": "In-Circuit Emulator (ICE)." },
    { "en": "Mode pengujian pada IC yang memungkinkan akses langsung ke internal flip-flop?", "id": "Boundary Scan (JTAG)." },
    { "en": "Mekanisme fisik untuk mengunci konektor agar tidak mudah terlepas?", "id": "Mekanisme Penguncian Konektor." },
    { "en": "Lapisan konduktif transparan pada layar sentuh?", "id": "Lapisan ITO (Indium Tin Oxide)." },
    { "en": "Tampilan yang menggunakan partikel bermuatan dalam kapsul mikro?", "id": "Tampilan E-Ink (Kertas Elektronik)." },
    { "en": "Sensor yang mendeteksi perubahan orientasi absolut?", "id": "Sensor Orientasi." },
    { "en": "Perangkat yang menghasilkan aliran udara terionisasi untuk menetralkan listrik statis?", "id": "Ionizer." },
    { "en": "Alat yang digunakan untuk menyolder atau melepas komponen SMT dengan udara panas?", "id": "Hot Air Rework Station." },
    { "en": "Proses pemanasan awal PCB sebelum menyolder untuk mengurangi kejutan termal?", "id": "Preheating." },
    { "en": "Sirkuit elektronik yang dibangun menggunakan tabung vakum?", "id": "Elektronika Tabung." },
    { "en": "Sirkuit elektronik yang dibangun menggunakan transistor dan komponen solid-state lainnya?", "id": "Elektronika Solid-State." },
    { "en": "Komponen yang berperilaku seperti dioda tetapi dengan empat lapisan (P-N-P-N)?", "id": "Diac." },
    { "en": "Thyristor yang dapat dimatikan dengan sinyal gerbang negatif?", "id": "GTO (Gate Turn-Off Thyristor)." },
    { "en": "Sistem yang menggunakan pulsa cahaya untuk komunikasi data?", "id": "Komunikasi Optik." },
    { "en": "Penguat yang menggunakan serat optik yang didoping untuk memperkuat sinyal cahaya?", "id": "EDFA (Erbium-Doped Fiber Amplifier)." },
    { "en": "Struktur pada serat optik yang memantulkan panjang gelombang tertentu?", "id": "Fiber Bragg Grating." },
    { "en": "Metode menggabungkan beberapa sinyal optik dengan panjang gelombang berbeda ke dalam satu serat?", "id": "WDM (Wavelength-Division Multiplexing)." },
    { "en": "Perangkat yang mengukur daya dalam satuan dBm atau Watt?", "id": "Power Meter." },
    { "en": "Pembangkitan pasangan elektron-lubang dalam semikonduktor oleh foton?", "id": "Generasi Fotolistrik." },
    { "en": "Penggabungan kembali elektron dan lubang yang melepaskan energi?", "id": "Rekombinasi." },
    { "en": "Pergerakan pembawa muatan akibat medan listrik?", "id": "Arus Drift." },
    { "en": "Pergerakan pembawa muatan dari area konsentrasi tinggi ke rendah?", "id": "Arus Difusi." },
    { "en": "Waktu rata-rata sebuah pembawa muatan minoritas bertahan sebelum berekombinasi?", "id": "Waktu Hidup Pembawa Muatan (Carrier Lifetime)." },
    { "en": "Kemudahan pembawa muatan bergerak melalui material di bawah pengaruh medan listrik?", "id": "Mobilitas Pembawa Muatan (Carrier Mobility)." },
    { "en": "Energi minimum yang dibutuhkan untuk melepaskan elektron dari pita valensi ke pita konduksi?", "id": "Celah Pita (Band Gap)." },
    { "en": "Bahan semikonduktor murni tanpa doping?", "id": "Semikonduktor Intrinsik." },
    { "en": "Bahan semikonduktor yang telah didoping dengan impuritas?", "id": "Semikonduktor Ekstrinsik." },
    { "en": "Semikonduktor tipe-N dengan kelebihan elektron?", "id": "Semikonduktor Tipe-N." },
    { "en": "Semikonduktor tipe-P dengan kelebihan lubang (hole)?", "id": "Semikonduktor Tipe-P." },
    { "en": "Sambungan antara semikonduktor tipe-P dan tipe-N?", "id": "Sambungan P-N (P-N Junction)." },
    { "en": "Tingkat energi Fermi dalam semikonduktor?", "id": "Level Fermi." },
    { "en": "Kondisi sirkuit tanpa sinyal input yang diterapkan?", "id": "Keadaan Diam (Quiescent State)." },
    { "en": "Analisis perilaku sirkuit terhadap sinyal kecil yang berosilasi?", "id": "Analisis Sinyal Kecil." },
    { "en": "Analisis perilaku sirkuit terhadap perubahan sinyal DC yang besar?", "id": "Analisis Sinyal Besar." },
    { "en": "Garis pada grafik karakteristik I-V yang merepresentasikan batasan sirkuit eksternal?", "id": "Garis Beban (Load Line)." },
    { "en": "Titik operasi DC dari sebuah transistor atau komponen aktif?", "id": "Titik Q (Quiescent Point)." },
    { "en": "Pengujian perangkat di bawah kondisi stres untuk mendeteksi kegagalan awal?", "id": "Burn-in Testing." },
    { "en": "Tingkat kegagalan yang lebih tinggi pada awal masa pakai produk?", "id": "Mortalitas Dini (Infant Mortality)." },
    { "en": "Tingkat kegagalan yang meningkat seiring usia produk akibat keausan?", "id": "Aus (Wear-out)." },
    { "en": "Konduktor berbentuk batang atau tabung tebal untuk distribusi daya tinggi?", "id": "Busbar." },
    { "en": "Saklar besar untuk mengisolasi bagian dari sistem tenaga listrik untuk pemeliharaan?", "id": "Saklar Pemisah (Isolator Switch)." },
    { "en": "Mekanisme pada trafo daya untuk mengubah rasio lilitan?", "id": "Tap Changer." },
    { "en": "Konektor yang dirancang untuk menyalurkan sinyal dan daya secara bersamaan?", "id": "Konektor Hibrida." },
    { "en": "Penggunaan getaran ultrasonik untuk membersihkan komponen elektronik?", "id": "Pembersihan Ultrasonik." },
    { "en": "Menghubungkan sasis beberapa peralatan elektronik ke titik ground yang sama?", "id": "Common Grounding." },
    { "en": "Pelindung logam di sekitar sirkuit untuk memblokir interferensi RF?", "id": "Perisai RF (RF Shield)." },
    { "en": "Ruangan yang dirancang untuk memblokir semua medan elektromagnetik?", "id": "Ruang Anechoic." },
    { "en": "Kemampuan suatu sistem untuk terus beroperasi meskipun ada kegagalan?", "id": "Toleransi Kesalahan (Fault Tolerance)." },
    { "en": "Sistem dengan komponen cadangan yang mengambil alih jika komponen utama gagal?", "id": "Redundansi." },
    { "en": "Menggunakan beberapa catu daya secara paralel untuk keandalan atau kapasitas?", "id": "Catu Daya Redundan." },
    { "en": "Sistem pendingin yang menggunakan sirkulasi cairan?", "id": "Pendinginan Cair." },
    { "en": "Menyatakan bahwa arus melalui konduktor sebanding dengan tegangan di atasnya?", "id": "Hukum Ohm." },
    { "en": "Menyatakan bahwa jumlah aljabar arus pada sebuah simpul rangkaian adalah nol?", "id": "Hukum Arus Kirchhoff (KCL)." },
    { "en": "Menyatakan bahwa jumlah aljabar tegangan dalam sebuah loop tertutup adalah nol?", "id": "Hukum Tegangan Kirchhoff (KVL)." },
    { "en": "Menyederhanakan sirkuit linier menjadi satu sumber tegangan dan satu resistor seri?", "id": "Teorema Thevenin." },
    { "en": "Menyederhanakan sirkuit linier menjadi satu sumber arus dan satu resistor paralel?", "id": "Teorema Norton." },
    { "en": "Menghitung transfer daya maksimum dari sumber ke beban?", "id": "Teorema Transfer Daya Maksimum." },
    { "en": "Representasi matematis dari sistem fisik menggunakan variabel keadaan?", "id": "Representasi Ruang Keadaan (State-Space)." },
    { "en": "Plot dari lokasi akar-akar persamaan karakteristik saat parameter sistem bervariasi?", "id": "Lokus Akar (Root Locus)." },
    { "en": "Kriteria stabilitas grafis untuk sistem umpan balik berdasarkan plot Nyquist?", "id": "Kriteria Stabilitas Nyquist." },
    { "en": "Ukuran 'cadangan' stabilitas dari sebuah sistem loop tertutup?", "id": "Margin Fasa & Margin Gain." },
    { "en": "Sistem operasi yang menjamin pemrosesan data dalam batasan waktu yang ketat?", "id": "RTOS (Real-Time Operating System)." },
    { "en": "Rutin kode yang dieksekusi sebagai respons terhadap sinyal interupsi?", "id": "ISR (Interrupt Service Routine)." },
    { "en": "Proses menyimpan keadaan suatu tugas untuk memungkinkan tugas lain berjalan?", "id": "Context Switch." },
    { "en": "Kondisi di mana dua atau lebih proses saling menunggu sumber daya tanpa batas waktu?", "id": "Deadlock." },
    { "en": "Pasangan kuasipartikel elektron-lubang yang terikat oleh gaya Coulomb?", "id": "Exciton." },
    { "en": "Kuantum dari getaran dalam kisi kristal?", "id": "Fonon (Phonon)." },
    { "en": "Kuantum dari osilasi plasma?", "id": "Plasmon." },
    { "en": "Atom impuritas yang menyumbangkan elektron bebas pada semikonduktor?", "id": "Atom Donor." },
    { "en": "Atom impuritas yang menerima elektron (menciptakan 'hole') pada semikonduktor?", "id": "Atom Akseptor." },
    { "en": "Sambungan antara dua material semikonduktor yang berbeda jenisnya?", "id": "Heterojunction." },
    { "en": "Struktur kristal tunggal yang sempurna tanpa cacat atau batas butir?", "id": "Kristal Tunggal (Single Crystal)." },
    { "en": "Bahan padat yang terdiri dari banyak kristalit kecil?", "id": "Bahan Polikristalin." },
    { "en": "Bahan padat yang tidak memiliki struktur kristal teratur?", "id": "Bahan Amorf." },
    { "en": "Batas antara dua kristalit dalam material polikristalin?", "id": "Batas Butir (Grain Boundary)." },
    { "en": "Teknik modulasi digital yang menggabungkan perubahan amplitudo dan fasa?", "id": "QAM (Quadrature Amplitude Modulation)." },
    { "en": "Teknik spektrum tersebar menggunakan urutan pseudorandom untuk menyebar sinyal?", "id": "DSSS (Direct-Sequence Spread Spectrum)." },
    { "en": "Teknik spektrum tersebar dengan melompat-lompat antar frekuensi?", "id": "FHSS (Frequency-Hopping Spread Spectrum)." },
    { "en": "Metode transmisi dengan membagi sinyal ke banyak sub-pembawa ortogonal?", "id": "OFDM (Orthogonal Frequency-Division Multiplexing)." },
    { "en": "Kapasitas teoretis maksimum dari sebuah kanal komunikasi?", "id": "Kapasitas Kanal (Teorema Shannon-Hartley)." },
    { "en": "Perangkat yang mengukur parameter-S dari jaringan RF?", "id": "Vector Network Analyzer (VNA)." },
    { "en": "Perangkat yang menganalisis pantulan untuk menemukan gangguan pada kabel?", "id": "Time-Domain Reflectometer (TDR)." },
    { "en": "Perangkat yang menangkap dan mendekode data pada bus komunikasi serial atau paralel?", "id": "Protocol Analyzer." },
    { "en": "Mekanisme sinkronisasi antar perangkat dalam sistem terdistribusi?", "id": "Sinkronisasi Clock." },
    { "en": "Protokol untuk sinkronisasi clock yang akurat melalui jaringan komputer?", "id": "NTP (Network Time Protocol)." },
    { "en": "Sinyal yang digunakan untuk memberi tahu perangkat penerima kapan harus membaca data?", "id": "Sinyal Strobe." },
    { "en": "Proses 'berjabat tangan' antar dua perangkat untuk memulai komunikasi?", "id": "Handshaking." },
    { "en": "Metode kontrol aliran data untuk mencegah buffer meluap?", "id": "Kontrol Aliran (Flow Control)." },
    { "en": "Sirkuit yang menghasilkan penundaan waktu yang dapat diprogram?", "id": "Programmable Delay Line." },
    { "en": "Penguat yang gain-nya dapat dikontrol secara digital?", "id": "Programmable Gain Amplifier (PGA)." },
    { "en": "Filter yang frekuensi cut-off nya dapat diatur?", "id": "Filter Tertala (Tunable Filter)." },
    { "en": "Sinyal yang digunakan untuk mengaktifkan atau menonaktifkan sebuah chip IC?", "id": "Sinyal Chip Select (CS) / Chip Enable (CE)." },
    { "en": "Dioda yang dirancang untuk perlindungan dari tegangan berlebih pada jalur data?", "id": "Dioda ESD." },
    { "en": "Penggunaan umpan balik positif untuk menciptakan histeresis?", "id": "Regenerative Feedback." },
    { "en": "Kondisi di mana sirkuit mulai berosilasi secara tidak terkendali?", "id": "Osilasi Parasit." },
    { "en": "Kemampuan suatu bahan untuk ditarik ke dalam medan magnet?", "id": "Suseptibilitas Magnetik." },
    { "en": "Resistansi sisa pada superkonduktor terhadap arus AC?", "id": "Impedansi Permukaan." },
    { "en": "Model rangkaian yang menyederhanakan perilaku antena?", "id": "Rangkaian Ekuivalen Antena." },
    { "en": "Ukuran efisiensi antena dalam memfokuskan daya ke satu arah?", "id": "Gain Antena." },
    { "en": "Pola radiasi tiga dimensi dari sebuah antena?", "id": "Pola Radiasi." },
    { "en": "Bagian utama dari pola radiasi antena yang memiliki intensitas maksimum?", "id": "Main Lobe." },
    { "en": "Lobe radiasi yang lebih kecil dan tidak diinginkan pada pola antena?", "id": "Side Lobe." },
    { "en": "Sirkuit terpadu yang dibuat menggunakan teknologi Gallium Arsenide?", "id": "GaAs IC." },
    { "en": "Elektronika yang dirancang untuk beroperasi pada suhu di atas 125°C?", "id": "Elektronika Suhu Tinggi." },
    { "en": "Sirkuit terpadu tiga dimensi yang dibuat dengan menumpuk beberapa lapisan wafer?", "id": "3D IC." },
    { "en": "Teknologi pengemasan IC di mana beberapa die digabungkan dalam satu modul?", "id": "SiP (System in Package)." },
    { "en": "Pemeriksaan fungsional dasar saat perangkat pertama kali dinyalakan?", "id": "POST (Power-On Self-Test)." },
    { "en": "Tipe data abstrak yang berfungsi sebagai antrian 'pertama masuk, pertama keluar'?", "id": "Buffer FIFO." },
    { "en": "Tipe data abstrak yang berfungsi sebagai tumpukan 'terakhir masuk, pertama keluar'?", "id": "Buffer LIFO." },
    { "en": "Teknik untuk mengurangi daya yang dikonsumsi oleh sirkuit digital?", "id": "Manajemen Daya." },
    { "en": "Pengaturan tegangan dan frekuensi prosesor secara dinamis untuk menghemat daya?", "id": "DVFS (Dynamic Voltage and Frequency Scaling)." },
    { "en": "Mematikan daya ke bagian chip yang tidak digunakan?", "id": "Power Gating." },
    { "en": "Mengurangi frekuensi clock ke bagian chip yang tidak memerlukan kecepatan penuh?", "id": "Clock Gating." },
    { "en": "Perangkat yang mengubah sinyal listrik menjadi pulsa cahaya?", "id": "Pemancar Optik." },
    { "en": "Perangkat yang mengubah pulsa cahaya kembali menjadi sinyal listrik?", "id": "Penerima Optik." },
    { "en": "Pelemahan sinyal saat merambat melalui media?", "id": "Atenuasi." },
    { "en": "Penyebaran pulsa sinyal saat merambat, yang membatasi bandwidth?", "id": "Dispersi." },
    { "en": "Penggunaan ruang fisik yang sama oleh beberapa sirkuit?", "id": "Integrasi Skala Besar (LSI/VLSI)." },
    { "en": "Hukum empiris yang menyatakan jumlah transistor pada IC berlipat ganda setiap dua tahun?", "id": "Hukum Moore." },
    { "en": "Simulasi perilaku rangkaian pada level transistor?", "id": "Simulasi SPICE." },
    { "en": "Perangkat lunak untuk menggambar diagram skematik?", "id": "Schematic Capture Tool." },
    { "en": "Representasi abstrak dari fungsi logika tanpa mempertimbangkan implementasi gerbang?", "id": "Aljabar Boolean." },
    { "en": "Penyederhanaan fungsi logika menggunakan peta visual?", "id": "Peta Karnaugh." },
    { "en": "Mesin keadaan terbatas yang outputnya hanya bergantung pada keadaan saat ini?", "id": "Mesin Moore." },
    { "en": "Mesin keadaan terbatas yang outputnya bergantung pada keadaan saat ini dan input?", "id": "Mesin Mealy." },
    { "en": "Perangkat yang digunakan untuk menghapus chip EPROM dengan sinar UV?", "id": "EPROM Eraser." },
    { "en": "Proses menulis data ke memori non-volatile?", "id": "Programming." },
    { "en": "Jumlah siklus tulis/hapus yang dapat ditangani oleh sel memori flash?", "id": "Daya Tahan (Endurance)." },
    { "en": "Lama waktu data dapat disimpan dalam memori non-volatile tanpa kehilangan?", "id": "Retensi Data." },
    { "en": "Penguat yang menggunakan umpan balik untuk mencapai gain yang sangat presisi?", "id": "Precision Amplifier." },
    { "en": "Penguat yang dirancang untuk mengisolasi sinyal dari ground loop?", "id": "Isolation Amplifier." },
    { "en": "Sirkuit yang mendeteksi perbedaan fasa antara dua sinyal?", "id": "Detektor Fasa." },
    { "en": "Komponen yang nilai resistansinya dapat diatur secara digital?", "id": "Potensiometer Digital." },
    { "en": "Kapasitor yang nilai kapasitansinya dapat diatur secara digital?", "id": "Kapasitor Variabel Digital." },
    { "en": "Teknik untuk mengukur jarak dengan menghitung langkah dan arah?", "id": "Dead Reckoning." },
    { "en": "Sekumpulan sensor (IMU, GPS, dll) yang datanya digabungkan untuk estimasi posisi yang lebih baik?", "id": "Sensor Fusion." },
    { "en": "Perangkat yang mengukur hambatan dan reaktansi komponen?", "id": "LCR Meter." },
    { "en": "Mengukur karakteristik suatu bahan dielektrik?", "id": "Dielectric Analyzer." },
    { "en": "Pengujian produk elektronik dalam kondisi suhu dan kelembaban ekstrem?", "id": "Pengujian Lingkungan (Environmental Testing)." },
    { "en": "Pengujian ketahanan produk terhadap getaran dan guncangan mekanis?", "id": "Pengujian Getaran." },
    { "en": "Sertifikasi yang menunjukkan bahwa produk tidak akan mengganggu perangkat lain secara elektromagnetik?", "id": "Sertifikasi EMC (Electromagnetic Compatibility)." },
    { "en": "Sertifikasi keselamatan produk elektronik?", "id": "Sertifikasi UL/CE." },
    { "en": "Proses verifikasi bahwa produk yang diproduksi memenuhi spesifikasi desain?", "id": "Pengujian Manufaktur." },
    { "en": "Struktur fisik pada PCB untuk pengujian otomatis menggunakan probe berpegas?", "id": "Test Point." },
    { "en": "Bahan penyerap radiasi elektromagnetik?", "id": "Bahan Penyerap Radar (RAM)." },
    { "en": "Bahan yang menunjukkan sifat piezoelektrik yang kuat?", "id": "Keramik PZT." },
    { "en": "Kaca yang diperkuat secara kimia untuk layar perangkat elektronik?", "id": "Gorilla Glass." },
    { "en": "Paduan logam dengan memori bentuk?", "id": "Nitinol." },
    { "en": "Cairan yang partikelnya dapat disejajarkan oleh medan magnet?", "id": "Ferrofluid." },
    { "en": "Dokumen teknis yang merinci semua spesifikasi dan karakteristik sebuah komponen?", "id": "Lembar Data (Datasheet)." },
    { "en": "Desain sirkuit teruji yang disediakan oleh produsen IC sebagai referensi?", "id": "Desain Referensi (Reference Design)." },
    { "en": "Analisis statistik untuk memprediksi bagaimana variasi komponen akan mempengaruhi kinerja sirkuit?", "id": "Analisis Monte Carlo." },
    { "en": "Proses mengoperasikan komponen di bawah rating maksimumnya untuk meningkatkan keandalan?", "id": "Derating." },
    { "en": "Waktu harapan hidup rata-rata untuk komponen yang tidak dapat diperbaiki?", "id": "MTTF (Mean Time To Failure)." },
    { "en": "Kecenderungan arus AC untuk mengalir di dekat permukaan konduktor pada frekuensi tinggi?", "id": "Efek Kulit (Skin Effect)." },
    { "en": "Generator tegangan tinggi DC menggunakan tumpukan dioda dan kapasitor?", "id": "Generator Cockcroft-Walton." },
    { "en": "Komponen optik yang hanya melewatkan cahaya dengan polarisasi tertentu?", "id": "Polarisator." },
    { "en": "Komponen optik yang memutar bidang polarisasi cahaya?", "id": "Pelat Gelombang (Waveplate)." },
    { "en": "Saklar yang menggunakan setetes merkuri cair untuk menyambungkan kontak?", "id": "Saklar Merkuri." },
    { "en": "Bagian dari sistem operasi yang menjadwalkan tugas mana yang akan dijalankan CPU?", "id": "Penjadwal (Scheduler)." },
    { "en": "Area memori yang digunakan bersama oleh beberapa proses untuk bertukar data?", "id": "Memori Bersama (Shared Memory)." },
    { "en": "Mekanisme sinkronisasi yang hanya memungkinkan satu thread mengakses sumber daya pada satu waktu?", "id": "Mutex (Mutual Exclusion)." },
    { "en": "Mekanisme sinkronisasi yang membatasi jumlah thread yang dapat mengakses sumber daya?", "id": "Semaphore." },
    { "en": "Sirkuit terpadu yang seluruhnya terbuat dari material transparan?", "id": "Elektronika Transparan." },
    { "en": "Teknik menggunakan medan magnet untuk menahan plasma panas di dalam reaktor fusi?", "id": "Kurungan Magnetik." },
    { "en": "Dioda yang digunakan dalam sirkuit pencampur frekuensi?", "id": "Dioda Mixer." },
    { "en": "Amplifier yang dirancang untuk memiliki distorsi intermodulasi yang sangat rendah?", "id": "Amplifier Linier." },
    { "en": "Efek memori pada baterai NiCd di mana kapasitasnya seolah berkurang jika tidak dikosongkan sepenuhnya?", "id": "Efek Memori." },
    { "en": "Kondisi kelebihan muatan pada baterai?", "id": "Overcharging." },
    { "en": "Kondisi pengosongan muatan baterai di bawah level aman?", "id": "Over-discharging." },
    { "en": "Sirkuit elektronik yang mensimulasikan neuron biologis?", "id": "Neuron Artifisial." },
    { "en": "Perangkat lunak atau perangkat keras yang dirancang untuk meniru fungsi otak manusia?", "id": "Jaringan Saraf (Neural Network)." },
    { "en": "Elektronika yang menggunakan molekul tunggal sebagai komponen?", "id": "Elektronika Molekuler." },
    { "en": "Standar untuk pengujian dan verifikasi sirkuit terpadu?", "id": "Standar IEEE 1149.1 (JTAG)." },
    { "en": "Diagram yang menunjukkan keadaan dan transisi dalam mesin keadaan terbatas?", "id": "Diagram Keadaan (State Diagram)." },
    { "en": "Alat pengembangan yang memungkinkan debugging kode langsung pada perangkat keras target?", "id": "In-Circuit Debugger (ICD)." },
    { "en": "Proses verifikasi formal untuk membuktikan kebenaran matematis dari sebuah desain?", "id": "Verifikasi Formal." },
    { "en": "Tingkat tegangan yang memisahkan logika '0' dan '1'?", "id": "Ambang Logika (Logic Threshold)." },
    { "en": "Rentang tegangan di mana input logika tidak terdefinisi (bukan HIGH atau LOW)?", "id": "Daerah Tak Tentu (Undefined Region)." },
    { "en": "Kemampuan output gerbang logika untuk menggerakkan sejumlah input gerbang lain?", "id": "Fan-out." },
    { "en": "Jumlah input gerbang yang terhubung ke sebuah output?", "id": "Fan-in." },
    { "en": "Struktur kristal yang cacat atau tidak sempurna?", "id": "Dislokasi Kristal." },
    { "en": "Bahan yang dapat mengubah bentuk sebagai respons terhadap medan magnet?", "id": "Bahan Magnetostriktif." },
    { "en": "Bahan yang dapat menghasilkan listrik dari getaran?", "id": "Generator Piezoelektrik." },
    { "en": "Perangkat yang mengukur komposisi kimia suatu zat?", "id": "Analisis Spektral." },
    { "en": "Penggunaan gelombang suara untuk membuat gambar bagian dalam suatu objek?", "id": "Pencitraan Ultrasonik." },
    { "en": "Teknik pendinginan yang menggunakan ekspansi gas?", "id": "Pendinginan Joule-Thomson." },
    { "en": "Peralatan laboratorium untuk mendinginkan sampel ke suhu sangat rendah?", "id": "Kriostat." },
    { "en": "Sirkuit yang dirancang untuk beroperasi pada tegangan sangat rendah?", "id": "Elektronika Daya Rendah." },
    { "en": "Jalur konduktif yang menghubungkan dua atau lebih lapisan dalam sebuah chip IC?", "id": "Interkoneksi." },
    { "en": "Kapasitansi yang tidak diinginkan antara jalur-jalur interkoneksi?", "id": "Kapasitansi Interkoneksi." },
    { "en": "Penundaan waktu yang disebabkan oleh resistansi dan kapasitansi jalur interkoneksi?", "id": "Tunda RC." },
    { "en": "Model matematis yang mendeskripsikan perilaku sebuah transistor?", "id": "Model Transistor (Contoh: Model Ebers-Moll)." },
    { "en": "Analisis sirkuit dengan mengasumsikan komponen bersifat ideal?", "id": "Analisis Orde Pertama." },
    { "en": "Analisis sirkuit yang memperhitungkan efek-efek non-ideal sekunder?", "id": "Analisis Orde Kedua." },
    { "en": "Karakteristik suatu sistem yang dapat kembali ke keadaan stabil setelah gangguan?", "id": "Stabilitas." },
    { "en": "Kemampuan suatu sistem untuk mencapai dan mempertahankan nilai yang diinginkan?", "id": "Akurasi." },
    { "en": "Kemampuan suatu pengukuran untuk menghasilkan hasil yang sama secara konsisten?", "id": "Presisi / Keterulangan." },
    { "en": "Perbedaan antara nilai terukur dan nilai sebenarnya?", "id": "Kesalahan Pengukuran." },
    { "en": "Kesalahan yang disebabkan oleh keterbatasan instrumen?", "id": "Kesalahan Sistematis." },
    { "en": "Kesalahan yang disebabkan oleh fluktuasi acak?", "id": "Kesalahan Acak." },
    { "en": "Proses mengurangi atau menghilangkan kesalahan sistematis?", "id": "Kalibrasi." },
    { "en": "Standar referensi yang digunakan untuk kalibrasi?", "id": "Standar Kalibrasi." },
    { "en": "Kemampuan untuk melacak suatu pengukuran kembali ke standar internasional?", "id": "Ketertelusuran (Traceability)." },
    { "en": "Catu daya yang dapat diprogram output tegangan dan arusnya?", "id": "Catu Daya Terprogram." },
    { "en": "Satu set peralatan uji yang dikendalikan oleh satu komputer?", "id": "Sistem Uji Otomatis (ATE)." },
    { "en": "Bahasa perintah standar untuk instrumen yang dapat diprogram?", "id": "SCPI (Standard Commands for Programmable Instruments)." },
    { "en": "Bus antarmuka standar untuk menghubungkan instrumen uji?", "id": "GPIB (IEEE-488)." },
    { "en": "Perangkat lunak yang berfungsi sebagai panel instrumen virtual di komputer?", "id": "Instrumen Virtual (Contoh: LabVIEW)." },
    { "en": "Rangkaian untuk melindungi input dari tegangan yang melebihi level catu daya?", "id": "Klem Proteksi Input." },
    { "en": "Penggunaan dioda untuk membatasi ayunan tegangan suatu sinyal?", "id": "Rangkaian Klem Dioda." },
    { "en": "Sirkuit yang dapat menyimpan nilai tegangan analog?", "id": "Memori Analog." },
    { "en": "Teknik mengurangi noise dengan merata-ratakan beberapa pengukuran?", "id": "Perataan Sinyal (Signal Averaging)." },
    { "en": "Algoritma untuk menemukan komponen frekuensi utama dalam sebuah sinyal?", "id": "FFT (Fast Fourier Transform)." },
    { "en": "Filter digital yang meniru respons filter analog?", "id": "Filter Digital." },
    { "en": "Perangkat yang mengukur laju kesalahan bit dalam transmisi data digital?", "id": "BERT (Bit Error Rate Tester)." },
    { "en": "Diagram visual yang digunakan untuk menganalisis sinyal komunikasi digital?", "id": "Diagram Mata (Eye Diagram)." },
    { "en": "Kondisi di mana bit-bit yang berdekatan dalam aliran data saling mengganggu?", "id": "Inter-Symbol Interference (ISI)." },
    { "en": "Proses menyamakan karakteristik kanal transmisi untuk mengurangi distorsi?", "id": "Ekualisasi." },
    { "en": "Konektor listrik yang dirancang untuk transmisi data kecepatan sangat tinggi?", "id": "Konektor High-Speed." },
    { "en": "Bahan PCB dengan konstanta dielektrik rendah untuk aplikasi frekuensi tinggi?", "id": "Substrat RF (Contoh: Rogers)." },
    { "en": "Metode untuk membuat lubang pada PCB menggunakan laser?", "id": "Pengeboran Laser." },
    { "en": "Proses pelapisan PCB dengan lapisan logam tipis?", "id": "Pelapisan Logam (Plating)." },
    { "en": "Pengujian kontinuitas elektrik pada PCB kosong?", "id": "Bare-Board Testing." },
    { "en": "Pemeriksaan visual otomatis pada PCB menggunakan kamera?", "id": "AOI (Automated Optical Inspection)." },
    { "en": "Pemeriksaan PCB menggunakan Sinar-X untuk melihat sambungan solder di bawah komponen?", "id": "AXI (Automated X-ray Inspection)." },
    { "en": "Metode pendinginan menggunakan perubahan fasa suatu cairan?", "id": "Pipa Panas (Heat Pipe)." },
    { "en": "Sistem pendingin yang menggunakan uap cairan untuk mentransfer panas?", "id": "Vapor Chamber." },
    { "en": "Material yang mengubah sifat optiknya saat dikenai medan listrik?", "id": "Material Elektro-optik." },
    { "en": "Material yang mengubah sifat optiknya saat dikenai medan magnet?", "id": "Material Magneto-optik." },
    { "en": "Perangkat yang memodulasi intensitas sinar cahaya?", "id": "Modulator Optik." },
    { "en": "Perangkat yang membelokkan arah sinar cahaya menggunakan sinyal listrik?", "id": "Deflektor Optik." },
    { "en": "Komponen yang membagi satu sinar cahaya menjadi dua atau lebih?", "id": "Beam Splitter." },
    { "en": "Lensa berukuran mikro yang dibuat pada skala chip?", "id": "Microlens." },
    { "en": "Sistem mekanis dan optis berukuran mikro?", "id": "MOEMS (Micro-opto-electro-mechanical systems)." },
    { "en": "Elektronika yang dapat dimakan atau larut dalam tubuh?", "id": "Elektronika Transient." },
    { "en": "Penggunaan cahaya untuk mentransmisikan data di dalam chip atau antar chip?", "id": "Interkoneksi Optik." },
    { "en": "Komputasi yang menggunakan foton (cahaya) sebagai pengganti elektron?", "id": "Komputasi Fotonik." },
    { "en": "Bahan yang memancarkan elektron saat dipanaskan?", "id": "Katoda Termionik." },
    { "en": "Bahan yang memancarkan elektron saat disinari cahaya?", "id": "Fotokatoda." },
    { "en": "Jaringan resistor dan kapasitor yang digunakan untuk memodelkan jalur transmisi?", "id": "Model R-C." },
    { "en": "Kemampuan sirkuit untuk berfungsi dengan benar meskipun ada variasi dalam komponen atau lingkungan?", "id": "Ketahanan (Robustness)." },
    { "en": "Desain yang dioptimalkan untuk kemudahan pembuatan?", "id": "Design for Manufacturability (DFM)." },
    { "en": "Desain yang dioptimalkan untuk kemudahan pengujian?", "id": "Design for Testability (DFT)." },
    { "en": "Penggunaan kembali blok desain yang sudah ada dalam proyek baru?", "id": "IP Core Reuse." },
    { "en": "Tingkat abstraksi tertinggi dalam desain sistem, berfokus pada fungsionalitas?", "id": "Level Sistem (System-Level)." },
    { "en": "Proses mengubah deskripsi level tinggi menjadi implementasi gerbang logika?", "id": "Sintesis Logika." }




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
            }, 7500);
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
