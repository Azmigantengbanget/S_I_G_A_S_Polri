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


  { "en": "Kepanjangan SIG (Sistem Informasi Geografis)?", "id": "Sistem Informasi Geografis." },
  { "en": "Definisi SIG (Sistem Informasi Geografis)?", "id": "Sistem untuk mengelola data geospasial." },
  { "en": "Lima komponen utama SIG?", "id": "Hardware, software, data, manusia, metode." },
  { "en": "Fungsi utama SIG?", "id": "Capture, store, query, analyze, display." },
  { "en": "Apa itu data spasial?", "id": "Data yang memiliki referensi lokasi." },
  { "en": "Dua model data spasial utama?", "id": "Vektor dan Raster." },
  { "en": "Model data berbasis titik, garis, poligon?", "id": "Model data Vektor." },
  { "en": "Model data berbasis grid piksel?", "id": "Model data Raster." },
  { "en": "Contoh data titik (point)?", "id": "Lokasi tower, ATM, pohon." },
  { "en": "Contoh data garis (line)?", "id": "Jalan, sungai, pipa." },
  { "en": "Contoh data poligon (area)?", "id": "Batas negara, danau, kavling tanah." },
  { "en": "Contoh data raster?", "id": "Citra satelit, foto udara, DEM." },
  { "en": "Informasi deskriptif data spasial?", "id": "Data atribut." },
  { "en": "Data atribut disimpan dalam?", "id": "Tabel atribut." },
  { "en": "Definisi peta?", "id": "Representasi visual permukaan bumi." },
  { "en": "Sistem koordinat berbasis sudut?", "id": "Sistem Koordinat Geografis." },
  { "en": "Sistem koordinat berbasis bidang datar?", "id": "Sistem Koordinat Terproyeksi." },
  { "en": "Garis vertikal di bola bumi?", "id": "Garis bujur (Longitude)." },
  { "en": "Garis horizontal di bola bumi?", "id": "Garis lintang (Latitude)." },
  { "en": "Apa itu datum?", "id": "Model matematis referensi bentuk bumi." },
  { "en": "Datum global yang umum digunakan?", "id": "WGS 1984." },
  { "en": "Kepanjangan WGS (World Geodetic System)?", "id": "World Geodetic System." },
  { "en": "Apa itu proyeksi peta?", "id": "Transformasi 3D bumi ke bidang 2D." },
  { "en": "Distorsi pada proyeksi peta?", "id": "Bentuk, area, jarak, atau arah." },
  { "en": "Proyeksi yang umum di Indonesia?", "id": "UTM (Universal Transverse Mercator)." },
  { "en": "Kepanjangan UTM (Universal Transverse Mercator)?", "id": "Universal Transverse Mercator." },
  { "en": "Apa itu skala peta?", "id": "Perbandingan jarak peta dan sebenarnya." },
  { "en": "Contoh skala peta besar?", "id": "1:5.000 (detail tinggi)." },
  { "en": "Contoh skala peta kecil?", "id": "1:1.000.000 (cakupan luas)." },
  { "en": "Definisi analisis spasial?", "id": "Proses memeriksa data spasial." },
  { "en": "Tujuan analisis spasial?", "id": "Menemukan pola dan hubungan geografis." },
  { "en": "Apa itu 'query' spasial?", "id": "Proses menyeleksi data spasial." },
  { "en": "Contoh 'query' spasial?", "id": "Cari semua sekolah di Jakarta Pusat." },
  { "en": "Apa itu analisis 'buffer'?", "id": "Analisis area dalam jarak tertentu." },
  { "en": "Contoh analisis 'buffer'?", "id": "Area terdampak dalam radius 1 km." },
  { "en": "Apa itu analisis 'overlay'?", "id": "Menggabungkan dua layer peta berbeda." },
  { "en": "Contoh analisis 'overlay'?", "id": "Peta lahan tumpang tindih peta banjir." },
  { "en": "Kepanjangan GPS (Global Positioning System)?", "id": "Global Positioning System." },
  { "en": "Fungsi utama GPS (Global Positioning System)?", "id": "Menentukan posisi di permukaan bumi." },
  { "en": "Prinsip kerja GPS (Global Positioning System)?", "id": "Trilaterasi dari sinyal beberapa satelit." },
  { "en": "Berapa satelit minimum untuk posisi 3D?", "id": "Minimal empat satelit." },
  { "en": "Sistem GPS (Global Positioning System) alternatif?", "id": "GLONASS, Galileo, BeiDou." },
  { "en": "Apa itu 'remote sensing'?", "id": "Ilmu perolehan informasi tanpa kontak." },
  { "en": "Sinonim 'remote sensing'?", "id": "Penginderaan Jauh." },
  { "en": "Contoh platform 'remote sensing'?", "id": "Satelit, drone, pesawat terbang." },
  { "en": "Apa itu 'citra satelit'?", "id": "Gambar permukaan bumi dari satelit." },
  { "en": "Resolusi pada citra satelit?", "id": "Spasial, spektral, temporal, radiometrik." },
  { "en": "Apa itu 'resolusi spasial'?", "id": "Ukuran objek terkecil yang terlihat." },
  { "en": "Apa itu 'resolusi spektral'?", "id": "Kemampuan sensor membedakan panjang gelombang." },
  { "en": "Apa itu 'resolusi temporal'?", "id": "Seberapa sering satelit merekam area sama." },
  { "en": "Apa itu 'resolusi radiometrik'?", "id": "Kemampuan sensor membedakan kecerahan." },
  { "en": "Citra 'multispektral' punya?", "id": "Beberapa band spektral (3-10)." },
  { "en": "Citra 'hiperspektral' punya?", "id": "Sangat banyak band spektral." },
  { "en": "Software SIG (Sistem Informasi Geografis) populer?", "id": "ArcGIS, QGIS." },
  { "en": "QGIS adalah software?", "id": "Gratis dan open-source." },
  { "en": "Apa itu 'layer' dalam SIG?", "id": "Kumpulan data geografis tematik." },
  { "en": "Contoh 'layer'?", "id": "Layer jalan, layer sungai, layer gedung." },
  { "en": "Apa itu 'topologi'?", "id": "Hubungan spasial antar fitur." },
  { "en": "Contoh hubungan topologi?", "id": "Adjacency, connectivity, containment." },
  { "en": "Apa itu 'geodatabase'?", "id": "Database untuk menyimpan data spasial." },
  { "en": "Apa itu 'digitasi'?", "id": "Proses konversi data analog ke digital." },
  { "en": "Contoh 'digitasi'?", "id": "Menggambar ulang peta kertas di komputer." },
  { "en": "Apa itu 'georeferencing'?", "id": "Memberi koordinat pada data raster." },
  { "en": "Apa itu 'geocoding'?", "id": "Mengubah alamat menjadi koordinat." },
  { "en": "Apa itu 'reverse geocoding'?", "id": "Mengubah koordinat menjadi alamat." },
  { "en": "Apa itu 'network analysis'?", "id": "Analisis pada jaringan jalan/pipa." },
  { "en": "Contoh 'network analysis'?", "id": "Mencari rute terpendek." },
  { "en": "Apa itu 'least-cost path analysis'?", "id": "Mencari rute dengan biaya terendah." },
  { "en": "Apa itu 'service area analysis'?", "id": "Menentukan area jangkauan layanan." },
  { "en": "Kepanjangan DEM (Digital Elevation Model)?", "id": "Digital Elevation Model." },
  { "en": "Fungsi DEM (Digital Elevation Model)?", "id": "Representasi digital ketinggian permukaan." },
  { "en": "Analisis dari DEM (Digital Elevation Model)?", "id": "Analisis kemiringan (slope), aspek." },
  { "en": "Apa itu 'slope'?", "id": "Tingkat kemiringan suatu lereng." },
  { "en": "Apa itu 'aspect'?", "id": "Arah hadap suatu lereng." },
  { "en": "Apa itu 'viewshed analysis'?", "id": "Menentukan area yang terlihat." },
  { "en": "Aplikasi 'viewshed analysis'?", "id": "Penempatan menara pengawas atau CCTV." },
  { "en": "Apa itu 'watershed analysis'?", "id": "Menentukan daerah aliran sungai." },
  { "en": "Peta tematik menampilkan apa?", "id": "Tema atau topik tertentu." },
  { "en": "Contoh peta tematik?", "id": "Peta kepadatan penduduk, peta curah hujan." },
  { "en": "Apa itu 'kartografi'?", "id": "Seni dan ilmu pembuatan peta." },
  { "en": "Elemen penting pada peta?", "id": "Judul, legenda, skala, arah utara." },
  { "en": "Fungsi 'legenda' peta?", "id": "Menjelaskan arti simbol-simbol peta." },
  { "en": "Apa itu 'web mapping'?", "id": "Menampilkan peta interaktif di web." },
  { "en": "Contoh 'web mapping'?", "id": "Google Maps, OpenStreetMap." },
  { "en": "Kepanjangan KML/KMZ?", "id": "Keyhole Markup Language." },
  { "en": "File KML/KMZ digunakan untuk?", "id": "Google Earth." },
  { "en": "Format file vektor umum?", "id": "Shapefile (.shp), GeoJSON, KML." },
  { "en": "Format file raster umum?", "id": "GeoTIFF, JPEG 2000, MrSID." },
  { "en": "Apa itu 'geoprocessing'?", "id": "Operasi untuk memanipulasi data SIG." },
  { "en": "Contoh alat 'geoprocessing'?", "id": "Buffer, Clip, Intersect, Union." },
  { "en": "Fungsi 'Clip'?", "id": "Memotong layer sesuai batas lain." },
  { "en": "Fungsi 'Intersect'?", "id": "Menemukan area yang tumpang tindih." },
  { "en": "Fungsi 'Union'?", "id": "Menggabungkan semua fitur dari dua layer." },
  { "en": "Apa itu 'spatial join'?", "id": "Menggabungkan tabel atribut berbasis lokasi." },
  { "en": "Apa itu 'spatial interpolation'?", "id": "Mengestimasi nilai di lokasi tak terukur." },
  { "en": "Metode 'spatial interpolation'?", "id": "IDW, Kriging, Spline." },
  { "en": "Kepanjangan IDW (Inverse Distance Weighted)?", "id": "Inverse Distance Weighted." },
  { "en": "Apa itu 'kernel density estimation'?", "id": "Analisis untuk hitung kepadatan fitur." },
  { "en": "Hasil dari 'kernel density'?", "id": "Peta panas (heat map)." },
  { "en": "Aplikasi 'heat map' Polri?", "id": "Memetakan area rawan kejahatan." },
  { "en": "Apa itu 'hotspot analysis'?", "id": "Mengidentifikasi cluster spasial yang signifikan." },
  { "en": "Statistik untuk 'hotspot analysis'?", "id": "Getis-Ord Gi* (Gi-star)." },
  { "en": "Apa itu 'autokorelasi spasial'?", "id": "Ukuran kemiripan fitur dengan tetangganya." },
  { "en": "Hukum pertama geografi Tobler?", "id": "Semuanya berhubungan, yang dekat lebih berhubungan." },
  { "en": "Statistik untuk autokorelasi spasial?", "id": "Moran's I." },
  { "en": "Nilai Moran's I positif artinya?", "id": "Data cenderung mengelompok (clustered)." },
  { "en": "Nilai Moran's I negatif artinya?", "id": "Data cenderung menyebar (dispersed)." },
  { "en": "Apa itu 'Kriging'?", "id": "Metode interpolasi geostatistik." },
  { "en": "Keuntungan 'Kriging'?", "id": "Memberikan estimasi error pada prediksi." },
  { "en": "Apa itu 'variogram'?", "id": "Alat untuk deskripsikan autokorelasi spasial." },
  { "en": "Apa itu analisis 'proximity'?", "id": "Analisis kedekatan atau jarak." },
  { "en": "Contoh analisis 'proximity'?", "id": "Menghitung jarak ke fasilitas terdekat." },
  { "en": "Apa itu klasifikasi citra?", "id": "Proses mengelompokkan piksel ke kelas." },
  { "en": "Klasifikasi 'supervised' butuh?", "id": "Data latih (training samples)." },
  { "en": "Klasifikasi 'unsupervised' butuh?", "id": "Tidak butuh data latih." },
  { "en": "Algoritma klasifikasi 'supervised'?", "id": "Maximum Likelihood, SVM, Random Forest." },
  { "en": "Algoritma klasifikasi 'unsupervised'?", "id": "ISODATA, K-Means." },
  { "en": "Kepanjangan NDVI (Normalized Difference Vegetation Index)?", "id": "Normalized Difference Vegetation Index." },
  { "en": "Fungsi NDVI (Normalized Difference Vegetation Index)?", "id": "Mengukur kehijauan atau kesehatan vegetasi." },
  { "en": "Band yang digunakan NDVI?", "id": "Near-Infrared (NIR) dan Merah (Red)." },
  { "en": "Apa itu 'change detection'?", "id": "Mendeteksi perubahan area dari waktu ke waktu." },
  { "en": "Apa itu data LiDAR (Light Detection and Ranging)?", "id": "Data awan titik (point cloud)." },
  { "en": "Keunggulan LiDAR (Light Detection and Ranging)?", "id": "Menghasilkan data ketinggian sangat detail." },
  { "en": "Kepanjangan SAR (Synthetic Aperture Radar)?", "id": "Synthetic Aperture Radar." },
  { "en": "Kelebihan SAR (Synthetic Aperture Radar)?", "id": "Bisa menembus awan dan malam hari." },
  { "en": "Apa itu 'metadata' SIG?", "id": "Informasi tentang data spasial." },
  { "en": "Standar metadata SIG internasional?", "id": "ISO 19115." },
  { "en": "Contoh database spasial?", "id": "PostGIS, Oracle Spatial, SQL Server Spatial." },
  { "en": "Apa itu 'versioning' data?", "id": "Manajemen histori perubahan data." },
  { "en": "Kepanjangan SDI (Spatial Data Infrastructure)?", "id": "Spatial Data Infrastructure." },
  { "en": "Tujuan SDI (Spatial Data Infrastructure)?", "id": "Memfasilitasi berbagi pakai data spasial." },
  { "en": "Kepanjangan WMS (Web Map Service)?", "id": "Web Map Service." },
  { "en": "Fungsi WMS (Web Map Service)?", "id": "Menampilkan gambar peta via web." },
  { "en": "Kepanjangan WFS (Web Feature Service)?", "id": "Web Feature Service." },
  { "en": "Fungsi WFS (Web Feature Service)?", "id": "Mengakses dan mengedit data vektor." },
  { "en": "Beda WMS (Web Map Service) dan WFS (Web Feature Service)?", "id": "WMS gambar, WFS data asli." },
  { "en": "Apa itu 'crime mapping'?", "id": "Pemetaan dan analisis pola kejahatan." },
  { "en": "Apa itu 'geographic profiling'?", "id": "Memprediksi area tinggal pelaku kejahatan." },
  { "en": "Prinsip 'geographic profiling'?", "id": "Pelaku cenderung beraksi dekat rumah." },
  { "en": "Apa itu 'suitability analysis'?", "id": "Analisis untuk menentukan lokasi terbaik." },
  { "en": "Apa itu 'site selection'?", "id": "Proses memilih lokasi berdasarkan kriteria." },
  { "en": "Aplikasi 'site selection' Polri?", "id": "Menentukan lokasi pos polisi baru." },
  { "en": "Apa itu 'digital terrain model' (DTM)?", "id": "Model ketinggian permukaan tanah." },
  { "en": "Apa itu 'digital surface model' (DSM)?", "id": "Model ketinggian termasuk objek di atasnya." },
  { "en": "Beda DTM (Digital Terrain Model) dan DSM (Digital Surface Model)?", "id": "DSM termasuk gedung dan pohon." },
  { "en": "Apa itu TIN (Triangulated Irregular Network)?", "id": "Model permukaan dari segitiga-segitiga." },
  { "en": "Kepanjangan TIN?", "id": "Triangulated Irregular Network." },
  { "en": "Apa itu 'contour'?", "id": "Garis yang hubungkan titik ketinggian sama." },
  { "en": "Fungsi 'contour'?", "id": "Visualisasi bentuk topografi." },
  { "en": "Apa itu 'topology rules'?", "id": "Aturan hubungan spasial antar fitur." },
  { "en": "Contoh 'topology rule'?", "id": "Poligon tidak boleh tumpang tindih." },
  { "en": "Apa itu 'raster calculator'?", "id": "Alat untuk operasi matematis data raster." },
  { "en": "Apa itu 'reclassification'?", "id": "Mengelompokkan kembali nilai sel raster." },
  { "en": "Apa itu 'zonal statistics'?", "id": "Menghitung statistik raster dalam suatu zona." },
  { "en": "Apa itu 'mosaic'?", "id": "Menggabungkan beberapa data raster bersebelahan." },
  { "en": "Apa itu 'vectorization'?", "id": "Mengubah data raster menjadi vektor." },
  { "en": "Apa itu 'rasterization'?", "id": "Mengubah data vektor menjadi raster." },
  { "en": "Apa itu 'coordinate transformation'?", "id": "Mengubah sistem koordinat suatu data." },
  { "en": "Kepanjangan GNSS (Global Navigation Satellite System)?", "id": "Global Navigation Satellite System." },
  { "en": "GPS (Global Positioning System) adalah bagian dari?", "id": "GNSS (Global Navigation Satellite System)." },
  { "en": "Apa itu 'differential GPS' (DGPS)?", "id": "Teknik meningkatkan akurasi GPS." },
  { "en": "Cara kerja DGPS (Differential GPS)?", "id": "Gunakan stasiun referensi di darat." },
  { "en": "Kepanjangan RTK (Real-Time Kinematic)?", "id": "Real-Time Kinematic." },
  { "en": "Fungsi RTK (Real-Time Kinematic)?", "id": "Memberikan akurasi posisi level sentimeter." },
  { "en": "Apa itu 'band' spektral?", "id": "Rentang panjang gelombang tertentu." },
  { "en": "Band biru, hijau, merah termasuk?", "id": "Panjang gelombang tampak (visible)." },
  { "en": "Apa itu 'false color composite'?", "id": "Kombinasi band citra non-RGB." },
  { "en": "Tujuan 'false color composite'?", "id": "Menonjolkan fitur tertentu (misal: vegetasi)." },
  { "en": "Apa itu 'pan-sharpening'?", "id": "Mempertajam citra multispektral." },
  { "en": "Cara kerja 'pan-sharpening'?", "id": "Gabungkan citra multispektral dan pankromatik." },
  { "en": "Apa itu 'atmospheric correction'?", "id": "Koreksi gangguan atmosfer pada citra." },
  { "en": "Apa itu 'geometric correction'?", "id": "Koreksi distorsi geometris pada citra." },
  { "en": "Sumber distorsi geometris?", "id": "Sensor, kelengkungan bumi, topografi." },
  { "en": "Apa itu 'ground control point' (GCP)?", "id": "Titik di darat dengan koordinat pasti." },
  { "en": "Fungsi GCP (Ground Control Point)?", "id": "Untuk proses georeferencing dan koreksi." },
  { "en": "Apa itu 'orthorectification'?", "id": "Koreksi geometris yang hapus distorsi." },
  { "en": "Hasil dari 'orthorectification'?", "id": "Orthophoto atau orthomosaic." },
  { "en": "Apa itu ' Volunteered Geographic Information' (VGI)?", "id": "Informasi geografis dari sukarelawan." },
  { "en": "Contoh VGI (Volunteered Geographic Information)?", "id": "Data OpenStreetMap." },
  { "en": "Apa itu 'crowdsourcing' peta?", "id": "Pengumpulan data peta dari banyak orang." },
  { "en": "Kepanjangan OGC (Open Geospatial Consortium)?", "id": "Open Geospatial Consortium." },
  { "en": "Peran OGC (Open Geospatial Consortium)?", "id": "Mengembangkan standar terbuka data geospasial." },
  { "en": "Contoh standar OGC?", "id": "WMS, WFS, GeoPackage." },
  { "en": "Apa itu 'mobile GIS'?", "id": "Penggunaan SIG di perangkat mobile." },
  { "en": "Aplikasi 'mobile GIS'?", "id": "Survei lapangan, pengumpulan data." },
  { "en": "Apa itu 'augmented reality' (AR) SIG?", "id": "Menampilkan data SIG di dunia nyata." },
  { "en": "Apa itu 'geofencing'?", "id": "Membuat batas virtual di dunia nyata." },
  { "en": "Aplikasi 'geofencing'?", "id": "Notifikasi saat masuk/keluar area." },
  { "en": "Apa itu 'real-time GIS'?", "id": "SIG yang memproses data langsung." },
  { "en": "Contoh 'real-time GIS'?", "id": "Pelacakan kendaraan patroli secara langsung." },
  { "en": "Apa itu 'big data' spasial?", "id": "Data spasial volume sangat besar." },
  { "en": "Tantangan 'big data' spasial?", "id": "Penyimpanan, pemrosesan, dan visualisasi." },
  { "en": "Apa itu 'spatiotemporal' data?", "id": "Data yang memiliki komponen ruang-waktu." },
  { "en": "Analisis 'spatiotemporal'?", "id": "Menganalisis pola dalam ruang dan waktu." },
  { "en": "Apa itu 'fuzzy logic' SIG?", "id": "Logika untuk menangani ketidakpastian spasial." },
  { "en": "Contoh 'fuzzy logic' SIG?", "id": "Klasifikasi lahan dengan batas tidak tegas." },
  { "en": "Apa itu 'ModelBuilder' di ArcGIS?", "id": "Alat untuk membuat alur kerja visual." },
  { "en": "Bahasa skrip di ArcGIS?", "id": "ArcPy (berbasis Python)." },
  { "en": "Apa itu 'Agent-Based Modeling' (ABM)?", "id": "Simulasi agen otonom yang berinteraksi." },
  { "en": "Kepanjangan ABM (Agent-Based Modeling)?", "id": "Agent-Based Modeling." },
  { "en": "Aplikasi ABM (Agent-Based Modeling) forensik?", "id": "Simulasi pergerakan massa atau penyebaran." },
  { "en": "Apa itu 'Cellular Automata'?", "id": "Model grid dengan sel-sel berevolusi." },
  { "en": "Aplikasi 'Cellular Automata'?", "id": "Simulasi pertumbuhan kota, kebakaran hutan." },
  { "en": "Apa itu 'voxel layer'?", "id": "Layer data 3D berbasis kubus." },
  { "en": "Analisis 'line of sight' 3D?", "id": "Menentukan visibilitas antar dua titik 3D." },
  { "en": "Analisis 'skyline'?", "id": "Menganalisis cakrawala dari suatu titik." },
  { "en": "Klasifikasi 'point cloud' LiDAR?", "id": "Memberi label (tanah, gedung, vegetasi)." },
  { "en": "Kepanjangan BIM (Building Information Modeling)?", "id": "Building Information Modeling." },
  { "en": "Integrasi BIM-GIS untuk apa?", "id": "Analisis dari skala gedung ke kota." },
  { "en": "Apa itu ArcGIS Online?", "id": "Platform SIG berbasis cloud dari Esri." },
  { "en": "Apa itu Google Earth Engine?", "id": "Platform analisis data geospasial cloud." },
  { "en": "Apa itu 'serverless GIS'?", "id": "Menjalankan fungsi SIG tanpa kelola server." },
  { "en": "Apa itu REST (Representational State Transfer) API?", "id": "Standar arsitektur untuk layanan web." },
  { "en": "Format data ringan untuk web mapping?", "id": "GeoJSON." },
  { "en": "Standar geospasial Eropa?", "id": "INSPIRE directive." },
  { "en": "Keuntungan GeoPackage dari Shapefile?", "id": "Satu file, tidak ada batasan ukuran." },
  { "en": "Kepanjangan WMTS (Web Map Tile Service)?", "id": "Web Map Tile Service." },
  { "en": "Fungsi WMTS (Web Map Tile Service)?", "id": "Menyajikan peta dalam bentuk 'ubin' (tiles)." },
  { "en": "Apa itu 'slippy map'?", "id": "Peta online yang bisa di-geser/zoom." },
  { "en": "Isu privasi data lokasi?", "id": "Pelacakan individu tanpa izin." },
  { "en": "Apa itu 'geospatial ethics'?", "id": "Prinsip etis dalam penggunaan data spasial." },
  { "en": "Apa itu lisensi 'Open Database License' (ODbL)?", "id": "Lisensi data terbuka (misal: OpenStreetMap)." },
  { "en": "Apa itu 'spatial data quality'?", "id": "Kualitas dari data spasial." },
  { "en": "Elemen 'spatial data quality'?", "id": "Akurasi posisi, akurasi atribut, kelengkapan." },
  { "en": "Apa itu 'topological consistency'?", "id": "Kepatuhan data terhadap aturan topologi." },
  { "en": "Apa itu 'logical consistency'?", "id": "Ketiadaan kontradiksi dalam data." },
  { "en": "Apa itu 'temporal accuracy'?", "id": "Akurasi waktu dari data." },
  { "en": "Apa itu 'network topology'?", "id": "Struktur konektivitas dalam jaringan." },
  { "en": "Aturan dalam 'network topology'?", "id": "Arah jalan, batasan belok." },
  { "en": "Apa itu 'linear referencing'?", "id": "Sistem lokasi berbasis jarak di garis." },
  { "en": "Aplikasi 'linear referencing'?", "id": "Manajemen data jalan raya atau rel." },
  { "en": "Apa itu 'dynamic segmentation'?", "id": "Membuat fitur dari 'linear referencing'." },
  { "en": "Apa itu 'fuzzy overlay'?", "id": "Overlay yang tangani batas tidak tegas." },
  { "en": "Apa itu 'spatial regression'?", "id": "Model regresi yang pertimbangkan lokasi." },
  { "en": "Contoh model 'spatial regression'?", "id": "Geographically Weighted Regression (GWR)." },
  { "en": "Apa itu 'cluster and outlier analysis'?", "id": "Mengidentifikasi kelompok dan data pencilan." },
  { "en": "Statistik untuk 'cluster/outlier analysis'?", "id": "Anselin Local Moran's I." },
  { "en": "Apa itu 'colocation analysis'?", "id": "Menganalisis apakah dua fitur saling berdekatan." },
  { "en": "Apa itu 'location-allocation'?", "id": "Menentukan lokasi fasilitas optimal." },
  { "en": "Tujuan 'location-allocation'?", "id": "Meminimalkan jarak, memaksimalkan cakupan." },
  { "en": "Apa itu 'geodesy'?", "id": "Ilmu tentang pengukuran dan bentuk bumi." },
  { "en": "Bentuk bumi sebenarnya?", "id": "Geoid." },
  { "en": "Model matematis bumi?", "id": "Ellipsoid atau spheroid." },
  { "en": "Apa itu 'map algebra'?", "id": "Aljabar untuk menganalisis data raster." },
  { "en": "Operasi dalam 'map algebra'?", "id": "Lokal, fokal, zonal, global." },
  { "en": "Apa itu 'terrain analysis'?", "id": "Analisis terhadap data ketinggian." },
  { "en": "Contoh 'terrain analysis'?", "id": "Analisis slope, aspect, dan hillshade." },
  { "en": "Apa itu 'hillshade'?", "id": "Visualisasi relief permukaan dengan bayangan." },
  { "en": "Apa itu 'hydrological modeling'?", "id": "Pemodelan aliran air di permukaan." },
  { "en": "Apa itu 'stream network'?", "id": "Jaringan aliran sungai." },
  { "en": "Apa itu 'point cloud'?", "id": "Kumpulan titik data dalam ruang 3D." },
  { "en": "Klasifikasi 'point cloud'?", "id": "Tanah, vegetasi, bangunan, dll." },
  { "en": "Apa itu 'digital photogrammetry'?", "id": "Pengukuran dari foto digital." },
  { "en": "Apa itu 'stereo pair'?", "id": "Dua foto dari sudut sedikit berbeda." },
  { "en": "Fungsi 'stereo pair'?", "id": "Menciptakan persepsi kedalaman (3D)." },
  { "en": "Apa itu 'unmanned aerial vehicle' (UAV)?", "id": "Pesawat tanpa awak." },
  { "en": "Sinonim UAV (Unmanned Aerial Vehicle)?", "id": "Drone." },
  { "en": "Penggunaan drone untuk pemetaan?", "id": "Membuat orthophoto dan DSM resolusi tinggi." },
  { "en": "Kepanjangan WCS (Web Coverage Service)?", "id": "Web Coverage Service." },
  { "en": "Fungsi WCS (Web Coverage Service)?", "id": "Mengakses data raster via web." },
  { "en": "Kepanjangan WPS (Web Processing Service)?", "id": "Web Processing Service." },
  { "en": "Fungsi WPS (Web Processing Service)?", "id": "Menjalankan geoprocessing via web." },
  { "en": "Apa itu 'tile cache'?", "id": "Peta yang sudah di-render jadi gambar." },
  { "en": "Tujuan 'tile cache'?", "id": "Mempercepat waktu pemuatan peta." },
  { "en": "Format 'tile' peta?", "id": "PNG atau JPEG." },
  { "en": "Apa itu 'vector tiles'?", "id": "Tile yang berisi data vektor." },
  { "en": "Keuntungan 'vector tiles'?", "id": "Ukuran kecil, styling bisa diubah." },
  { "en": "Apa itu 'geoportal'?", "id": "Portal web untuk akses data geospasial." },
  { "en": "Contoh geoportal nasional Indonesia?", "id": "Ina-Geoportal." },
  { "en": "Apa itu 'metadata catalog'?", "id": "Katalog untuk mencari metadata." },
  { "en": "Protokol untuk 'metadata catalog'?", "id": "CSW (Catalog Service for the Web)." },
  { "en": "Apa itu 'geotagging'?", "id": "Menambahkan informasi lokasi ke media." },
  { "en": "Contoh 'geotagging'?", "id": "Foto dengan koordinat GPS." },
  { "en": "Apa itu 'indoor positioning system' (IPS)?", "id": "Sistem penentuan posisi di dalam ruangan." },
  { "en": "Teknologi untuk IPS (Indoor Positioning System)?", "id": "Wi-Fi, Bluetooth, UWB." },
  { "en": "Kepanjangan UWB (Ultra-Wideband)?", "id": "Ultra-Wideband." },
  { "en": "Apa itu 'principal component analysis' (PCA)?", "id": "Analisis statistik untuk reduksi dimensi." },
  { "en": "Aplikasi PCA (Principal Component Analysis) di 'remote sensing'?", "id": "Mengurangi redundansi antar band citra." },
  { "en": "Apa itu 'spectral signature'?", "id": "Pola pantulan objek pada panjang gelombang." },
  { "en": "Tujuan 'spectral signature'?", "id": "Untuk mengidentifikasi jenis objek." },
  { "en": "Apa itu 'ground truthing'?", "id": "Verifikasi lapangan untuk data citra." },
  { "en": "Pentingnya 'ground truthing'?", "id": "Memastikan akurasi hasil klasifikasi." },
  { "en": "Apa itu 'accuracy assessment'?", "id": "Penilaian akurasi hasil klasifikasi." },
  { "en": "Metrik 'accuracy assessment'?", "id": "Overall accuracy, Kappa coefficient." },
  { "en": "Apa itu 'error matrix'?", "id": "Tabel untuk 'accuracy assessment'." },
  { "en": "Sinonim 'error matrix'?", "id": "Confusion matrix." },
  { "en": "Software server peta open-source?", "id": "GeoServer, MapServer." },
  { "en": "Kepanjangan COG (Cloud-Optimized GeoTIFF)?", "id": "Cloud-Optimized GeoTIFF." },
  { "en": "Keuntungan COG (Cloud-Optimized GeoTIFF)?", "id": "Efisien akses data raster via web." },
  { "en": "Kepanjangan STAC (Spatiotemporal Asset Catalog)?", "id": "Spatiotemporal Asset Catalog." },
  { "en": "Fungsi STAC (Spatiotemporal Asset Catalog)?", "id": "Standar untuk mengkatalog data geospasial." },
  { "en": "Standar API (Application Programming Interface) OGC (Open Geospatial Consortium) modern?", "id": "OGC API (Features, Maps, Tiles)." },
  { "en": "Apa itu 'cokriging'?", "id": "Kriging yang gunakan variabel sekunder." },
  { "en": "Apa itu 'indicator kriging'?", "id": "Kriging untuk estimasi probabilitas." },
  { "en": "Apa itu 'anisotropy' geostatistik?", "id": "Autokorelasi bervariasi tergantung arah." },
  { "en": "Model semivariogram umum?", "id": "Spherical, Exponential, Gaussian." },
  { "en": "Komponen semivariogram?", "id": "Nugget, sill, dan range." },
  { "en": "Apa itu 'nugget' effect?", "id": "Variasi pada jarak sangat kecil." },
  { "en": "Apa itu 'sill'?", "id": "Batas atas dari semivariogram." },
  { "en": "Apa itu 'range'?", "id": "Jarak dimana data tidak lagi berkorelasi." },
  { "en": "Apa itu GWR (Geographically Weighted Regression)?", "id": "Model regresi dengan parameter lokal." },
  { "en": "Masalah autokorelasi spasial di ML?", "id": "Melanggar asumsi independensi data." },
  { "en": "Peran CNN (Convolutional Neural Network) di SIG?", "id": "Ekstraksi fitur otomatis dari citra." },
  { "en": "Segmentasi 'point cloud' dengan AI?", "id": "Memberi label pada setiap titik 3D." },
  { "en": "Apa itu 'Quadtree'?", "id": "Struktur data spasial berbasis rekursi." },
  { "en": "Apa itu 'R-tree'?", "id": "Struktur data pohon untuk indeks data." },
  { "en": "Keuntungan 'R-tree'?", "id": "Efisien untuk query jangkauan (range query)." },
  { "en": "Dasar pembuatan TIN (Triangulated Irregular Network)?", "id": "Delaunay triangulation." },
  { "en": "Apa itu 'Delaunay triangulation'?", "id": "Segitiga dimana lingkaran luar kosong." },
  { "en": "Apa itu 'Voronoi diagram'?", "id": "Partisi ruang berdasarkan titik terdekat." },
  { "en": "Apa itu 'digital divide' spasial?", "id": "Kesenjangan akses teknologi geospasial." },
  { "en": "Apa itu 'neogeography'?", "id": "Penggunaan alat SIG oleh non-ahli." },
  { "en": "Apa itu 'critical GIS'?", "id": "Kajian kritis dampak sosial SIG." },
  { "en": "Apa itu 'data provenance'?", "id": "Riwayat asal-usul dan pengolahan data." },
  { "en": "Proyeksi 'conformal' mempertahankan?", "id": "Bentuk atau sudut." },
  { "en": "Proyeksi 'equal-area' mempertahankan?", "id": "Luas area." },
  { "en": "Proyeksi 'equidistant' mempertahankan?", "id": "Jarak dari satu atau dua titik." },
  { "en": "Proyeksi Mercator mendistorsi apa?", "id": "Luas area di dekat kutub." },
  { "en": "Apa itu 'great circle'?", "id": "Lingkaran terbesar di permukaan bola." },
  { "en": "Jarak terpendek di bumi?", "id": "Mengikuti busur 'great circle'." },
  { "en": "Apa itu 'rhumb line' atau 'loxodrome'?", "id": "Garis dengan arah kompas konstan." },
  { "en": "Apa itu 'geoid undulation'?", "id": "Perbedaan tinggi antara geoid-ellipsoid." },
  { "en": "Tinggi ortometrik mengacu pada?", "id": "Geoid (rata-rata permukaan laut)." },
  { "en": "Tinggi elipsoidal mengacu pada?", "id": "Ellipsoid referensi." },
  { "en": "GPS (Global Positioning System) mengukur tinggi?", "id": "Tinggi elipsoidal." },
  { "en": "Apa itu 'map generalization'?", "id": "Proses penyederhanaan fitur peta." },
  { "en": "Tujuan 'map generalization'?", "id": "Agar peta tetap terbaca di skala kecil." },
  { "en": "Apa itu 'cadastre'?", "id": "Catatan resmi kepemilikan tanah." },
  { "en": "Peta untuk 'cadastre'?", "id": "Peta kadastral." },
  { "en": "Apa itu 'land use'?", "id": "Fungsi atau aktivitas di atas lahan." },
  { "en": "Apa itu 'land cover'?", "id": "Penutup fisik permukaan bumi." },
  { "en": "Contoh 'land use'?", "id": "Permukiman, industri, pertanian." },
  { "en": "Contoh 'land cover'?", "id": "Hutan, air, perkotaan." },
  { "en": "Apa itu 'spatial statistics'?", "id": "Statistik yang mempertimbangkan lokasi." },
  { "en": "Apa itu 'point pattern analysis'?", "id": "Analisis pola sebaran titik." },
  { "en": "Tipe pola titik?", "id": "Clustered, dispersed, random." },
  { "en": "Apa itu 'nearest neighbor analysis'?", "id": "Menganalisis jarak ke tetangga terdekat." },
  { "en": "Apa itu 'quadrat analysis'?", "id": "Analisis sebaran titik dalam grid." },
  { "en": "Apa itu 'kernel' dalam analisis densitas?", "id": "Fungsi matematis untuk sebar nilai." },
  { "en": "Apa itu 'bandwidth' dalam analisis densitas?", "id": "Radius pencarian di sekitar titik." },
  { "en": "Apa itu 'cost distance analysis'?", "id": "Analisis jarak dengan memperhitungkan biaya." },
  { "en": "Contoh 'biaya' dalam analisis?", "id": "Kemiringan lereng, tipe jalan." },
  { "en": "Apa itu 'friction surface'?", "id": "Raster yang merepresentasikan biaya perjalanan." },
  { "en": "Apa itu 'multi-criteria evaluation' (MCE)?", "id": "Analisis yang menggabungkan banyak kriteria." },
  { "en": "Aplikasi MCE (Multi-Criteria Evaluation) dalam SIG?", "id": "Analisis kesesuaian lahan, pemilihan lokasi." },
  { "en": "Apa itu 'Analytical Hierarchy Process' (AHP)?", "id": "Metode pembobotan kriteria dalam MCE." },
  { "en": "Apa itu 'temporal GIS'?", "id": "SIG yang fokus pada dimensi waktu." },
  { "en": "Tantangan 'temporal GIS'?", "id": "Visualisasi dan analisis data waktu." },
  { "en": "Apa itu 'space-time cube'?", "id": "Visualisasi data 3D (x, y, waktu)." },
  { "en": "Apa itu 'geocollaboration'?", "id": "Kolaborasi menggunakan alat geospasial." },
  { "en": "Apa itu 'Public Participation GIS' (PPGIS)?", "id": "Partisipasi publik dalam pemetaan." },
  { "en": "Tujuan PPGIS (Public Participation GIS)?", "id": "Mengumpulkan pengetahuan lokal." },
  { "en": "Apa itu 'citizen science'?", "id": "Ilmu pengetahuan yang melibatkan warga." },
  { "en": "Kaitan 'citizen science' & SIG?", "id": "Pengumpulan data geografis oleh publik." },
  { "en": "Sensor pasif 'remote sensing'?", "id": "Mendeteksi energi pantulan matahari." },
  { "en": "Contoh sensor pasif?", "id": "Kamera digital, sensor satelit Landsat." },
  { "en": "Sensor aktif 'remote sensing'?", "id": "Memancarkan dan menerima energi sendiri." },
  { "en": "Contoh sensor aktif?", "id": "Radar, LiDAR." },
  { "en": "Apa itu 'spectral library'?", "id": "Database 'spectral signature' berbagai material." },
  { "en": "Apa itu 'image fusion'?", "id": "Menggabungkan informasi dari banyak gambar." },
  { "en": "Contoh 'image fusion'?", "id": "Pan-sharpening." },
  { "en": "Apa itu 'object-based image analysis' (OBIA)?", "id": "Klasifikasi citra berbasis objek." },
  { "en": "Beda OBIA (Object-Based Image Analysis) & 'pixel-based'?", "id": "OBIA kelompokkan piksel jadi objek." },
  { "en": "Apa itu 'decision tree classification'?", "id": "Klasifikasi menggunakan aturan 'if-then'." },
  { "en": "Apa itu 'support vector machine' (SVM)?", "id": "Algoritma klasifikasi berbasis hyperplane." },
  { "en": "Apa itu 'artificial neural network' (ANN)?", "id": "Model komputasi terinspirasi otak." },
  { "en": "Penggunaan ANN (Artificial Neural Network) di SIG?", "id": "Klasifikasi citra, pemodelan spasial." },
  { "en": "Apa itu 'geodesign'?", "id": "Desain yang mengintegrasikan ilmu geografi." },
  { "en": "Apa itu 'map series'?", "id": "Kumpulan peta dengan layout sama." },
  { "en": "Fungsi 'map series'?", "id": "Membuat atlas atau peta per wilayah." },
  { "en": "Apa itu 'symbology'?", "id": "Studi atau penggunaan simbol di peta." },
  { "en": "Tipe 'symbology'?", "id": "Simbol titik, garis, poligon." },
  { "en": "Apa itu 'color ramp'?", "id": "Gradasi warna untuk simbologi." },
  { "en": "Apa itu 'choropleth map'?", "id": "Peta yang gunakan warna/arsir." },
  { "en": "Apa itu 'isoline map'?", "id": "Peta yang gunakan garis kontur." },
  { "en": "Contoh 'isoline'?", "id": "Kontur (ketinggian), isoterm (suhu)." },
  { "en": "Apa itu 'cartogram'?", "id": "Peta yang ukurannya diubah." },
  { "en": "Ukuran di 'cartogram' merepresentasikan?", "id": "Variabel statistik (misal: populasi)." },
  { "en": "Apa itu 'flow map'?", "id": "Peta yang menunjukkan pergerakan." },
  { "en": "Elemen di 'flow map'?", "id": "Garis dengan ketebalan bervariasi." },
  { "en": "Apa itu 'geospatial data science'?", "id": "Ilmu data yang fokus pada data lokasi." },
  { "en": "Library Python untuk SIG (Sistem Informasi Geografis)?", "id": "GeoPandas, Rasterio, PySAL." },
  { "en": "Kepanjangan NDWI (Normalized Difference Water Index)?", "id": "Normalized Difference Water Index." },
  { "en": "Fungsi NDWI (Normalized Difference Water Index)?", "id": "Mendeteksi badan air." },
  { "en": "Kepanjangan NDBI (Normalized Difference Built-up Index)?", "id": "Normalized Difference Built-up Index." },
  { "en": "Fungsi NDBI (Normalized Difference Built-up Index)?", "id": "Mendeteksi area terbangun." },
  { "en": "Klasifikasi LiDAR (Light Detection and Ranging) standar?", "id": "Standar ASPRS." },
  { "en": "Contoh kode kelas ASPRS (American Society for Photogrammetry and Remote Sensing)?", "id": "Kelas 2 untuk tanah (ground)." },
  { "en": "Apa itu 'polarimetry' SAR (Synthetic Aperture Radar)?", "id": "Analisis polarisasi gelombang radar." },
  { "en": "Apa itu database NoSQL?", "id": "Database non-relasional." },
  { "en": "Penggunaan NoSQL untuk data SIG?", "id": "Penyimpanan data spasial tidak terstruktur." },
  { "en": "Contoh database NoSQL spasial?", "id": "MongoDB dengan GeoJSON." },
  { "en": "Apa itu 'distributed GIS'?", "id": "Pemrosesan SIG di banyak mesin." },
  { "en": "Framework untuk 'distributed GIS'?", "id": "GeoSpark, Apache Sedona." },
  { "en": "Apa itu 'data cube' spasial?", "id": "Struktur data multi-dimensi (x,y,t)." },
  { "en": "Standar akurasi posisi AS (Amerika Serikat)?", "id": "NSSDA (National Standard for Spatial Data Accuracy)." },
  { "en": "Apa itu 'error propagation'?", "id": "Perambatan error melalui analisis." },
  { "en": "Apa itu 'Network K-function'?", "id": "Analisis cluster pada jaringan." },
  { "en": "Apa itu 'spatial optimization'?", "id": "Menemukan solusi lokasi/alokasi terbaik." },
  { "en": "Apa itu 'geocomputation'?", "id": "Komputasi untuk masalah geografi kompleks." },
  { "en": "Apa itu 'reproducibility' dalam SIG?", "id": "Kemampuan mereproduksi hasil analisis." },
  { "en": "Pentingnya 'reproducibility'?", "id": "Validasi dan transparansi penelitian." },
  { "en": "Apa itu 'geoprivacy'?", "id": "Privasi terkait dengan data lokasi." },
  { "en": "Teknik untuk 'geoprivacy'?", "id": "Agregasi, obfuscation, generalisasi." },
  { "en": "Apa itu 'obfuscation' lokasi?", "id": "Menambah noise acak pada koordinat." },
  { "en": "Apa itu 'geoparsing'?", "id": "Mengekstrak informasi geografis dari teks." },
  { "en": "Apa itu 'toponym resolution'?", "id": "Menentukan lokasi dari nama tempat." },
  { "en": "Apa itu 'digital earth'?", "id": "Representasi virtual 3D dari bumi." },
  { "en": "Konsep 'digital earth' dipopulerkan oleh?", "id": "Al Gore." },
  { "en": "Apa itu 'Gazetteer'?", "id": "Kamus atau direktori nama geografis." },
  { "en": "Apa itu 'conflation'?", "id": "Proses menggabungkan data dari sumber berbeda." },
  { "en": "Tantangan 'conflation'?", "id": "Perbedaan skala, akurasi, dan waktu." },
  { "en": "Apa itu 'raster pyramid'?", "id": "Versi resolusi rendah dari raster." },
  { "en": "Tujuan 'raster pyramid'?", "id": "Mempercepat tampilan data raster besar." },
  { "en": "Apa itu 'data model' SIG?", "id": "Struktur konseptual untuk data geografis." },
  { "en": "Apa itu 'object-oriented data model'?", "id": "Data sebagai objek dengan properti-perilaku." },
  { "en": "Apa itu 'field-based model'?", "id": "Melihat ruang sebagai kumpulan field." },
  { "en": "Model data raster termasuk?", "id": "Field-based model." },
  { "en": "Apa itu 'tessellation'?", "id": "Partisi ruang menjadi bentuk-bentuk." },
  { "en": "Contoh 'tessellation'?", "id": "Grid persegi, heksagonal, segitiga." },
  { "en": "Apa itu 'spatial autocorrelation' positif?", "id": "Nilai yang mirip cenderung berdekatan." },
  { "en": "Apa itu 'spatial heterogeneity'?", "id": "Proses/hubungan bervariasi antar lokasi." },
  { "en": "GWR (Geographically Weighted Regression) menangani masalah?", "id": "Spatial heterogeneity." },
  { "en": "Apa itu 'modifiable areal unit problem' (MAUP)?", "id": "Hasil analisis berubah dengan batas area." },
  { "en": "Apa itu 'ecological fallacy'?", "id": "Asumsi individu sama dengan grupnya." },
  { "en": "Apa itu 'uncertainty analysis'?", "id": "Analisis dampak ketidakpastian pada hasil." },
  { "en": "Apa itu 'sensitivity analysis'?", "id": "Analisis dampak perubahan input." },
  { "en": "Apa itu 'exploratory spatial data analysis' (ESDA)?", "id": "Analisis data untuk temukan pola awal." },
  { "en": "Apa itu 'confirmatory spatial data analysis' (CSDA)?", "id": "Analisis untuk menguji hipotesis spasial." },
  { "en": "Apa itu 'spatial econometrics'?", "id": "Ekonometrika yang pertimbangkan efek spasial." },
  { "en": "Kepanjangan GeoAI?", "id": "Geospatial Artificial Intelligence." },
  { "en": "Fungsi GeoAI?", "id": "Integrasi AI dengan analisis geospasial." },
  { "en": "Apa itu 'indoor mapping'?", "id": "Pemetaan bagian dalam sebuah gedung." },
  { "en": "Teknologi untuk 'indoor mapping'?", "id": "LiDAR, SLAM, photogrammetry." },
  { "en": "Apa itu 'digital twin' kota?", "id": "Model digital 3D kota." },
  { "en": "Manfaat 'digital twin' kota?", "id": "Simulasi perkotaan, perencanaan, manajemen." },
  { "en": "Apa itu 'geofencing'?", "id": "Membuat batas virtual di dunia nyata." },
  { "en": "Aplikasi 'geofencing'?", "id": "Notifikasi saat masuk/keluar area." },
  { "en": "Apa itu 'routing' multimodal?", "id": "Pencarian rute dengan banyak moda transportasi." },
  { "en": "Contoh 'routing' multimodal?", "id": "Jalan kaki, lalu bus, lalu kereta." },
  { "en": "Apa itu 'map matching'?", "id": "Mencocokkan data GPS ke jaringan jalan." },
  { "en": "Tujuan 'map matching'?", "id": "Memperbaiki data lintasan yang tidak akurat." },
  { "en": "Apa itu 'conformal projection'?", "id": "Proyeksi peta yang mempertahankan bentuk." },
  { "en": "Apa itu 'equal-area projection'?", "id": "Proyeksi peta yang mempertahankan luas." },
  { "en": "Apa itu 'equidistant projection'?", "id": "Proyeksi peta yang mempertahankan jarak." },
  { "en": "Apa itu 'azimuthal projection'?", "id": "Proyeksi peta yang mempertahankan arah." },
  { "en": "Apa itu 'cylindrical projection'?", "id": "Proyeksi peta ke permukaan silinder." },
  { "en": "Apa itu 'conic projection'?", "id": "Proyeksi peta ke permukaan kerucut." },
  { "en": "Proyeksi UTM (Universal Transverse Mercator) termasuk jenis?", "id": "Cylindrical (Transverse)." },
  { "en": "Apa itu 'state plane coordinate system'?", "id": "Sistem koordinat per negara bagian AS." },
  { "en": "Apa itu 'cadastral mapping'?", "id": "Pemetaan persil atau bidang tanah." },
  { "en": "Tujuan 'cadastral mapping'?", "id": "Administrasi pertanahan dan pajak." },
  { "en": "Apa itu 'parish map'?", "id": "Peta yang menunjukkan batas-batas paroki." },
  { "en": "Apa itu 'chorochromatic map'?", "id": "Peta yang menunjukkan area nominal." },
  { "en": "Apa itu 'dasymetric mapping'?", "id": "Metode pemetaan tematik lebih akurat." },
  { "en": "Apa itu 'visual hierarchy' peta?", "id": "Pengaturan elemen peta agar penting." },
  { "en": "Elemen paling penting di hierarki?", "id": "Figur atau subjek utama peta." },
  { "en": "Elemen paling tidak penting?", "id": "Latar belakang atau informasi sekunder." },
  { "en": "Apa itu 'figure-ground relationship'?", "id": "Membedakan objek dari latar belakangnya." },
  { "en": "Apa itu 'map layout'?", "id": "Tata letak semua elemen peta." },
  { "en": "Elemen 'map layout'?", "id": "Peta utama, judul, legenda, skala." },
  { "en": "Apa itu 'inset map'?", "id": "Peta kecil di dalam peta utama." },
  { "en": "Fungsi 'inset map'?", "id": "Tampilkan detail atau lokasi konteks." },
  { "en": "Apa itu 'cartographic generalization'?", "id": "Proses penyederhanaan peta." },
  { "en": "Contoh 'cartographic generalization'?", "id": "Simplification, smoothing, aggregation." },
  { "en": "Apa itu 'data quality'?", "id": "Tingkat kesesuaian data dengan kenyataan." },
  { "en": "Dimensi 'data quality'?", "id": "Akurasi, presisi, kelengkapan, konsistensi." },
  { "en": "Apa itu 'lineage'?", "id": "Riwayat hidup sebuah data." }









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
