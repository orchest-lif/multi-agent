# PRD — Dashboard Pengelolaan Multi-Agent

- Status: draf diskusi, belum disetujui untuk implementasi
- Versi: 0.5
- Tanggal: 8 Oktober 2026
- Nama produk: belum ditentukan
- Rencana implementasi: [PLAN.md](PLAN.md)

## 1. Ringkasan

Aplikasi berbasis web untuk mengelola empat agent Muse.ai, satu Hermes, dan satu Nanobot melalui satu dashboard. Pengguna dapat memilih agent, mengirim instruksi, memantau pekerjaan, membaca hasil, dan meneruskan hasil ke agent pada platform lain.

Hermes telah diidentifikasi sebagai NousResearch/hermes-agent dan Nanobot sebagai HKUDS/nanobot. Dokumentasi upstream menyediakan kandidat integrasi CLI untuk Hermes dan API lokal untuk Nanobot. Versi yang terpasang di PC belum diketahui dan belum diuji. Pengguna menyebut muse.ai sebagai layanan cloud; dukungan pengendalian empat agent di layanan tersebut belum terverifikasi.

## 2. Kebutuhan yang sudah diketahui

- Antarmuka berbasis web.
- Inventaris awal terdiri dari enam agent: empat Muse.ai, satu Hermes, satu Nanobot.
- Empat koneksi Muse.ai merupakan empat akun dengan empat workspace berbeda, diakses melalui https://muse.ai/. Autentikasi, sesi, konteks, dan riwayat setiap koneksi harus terpisah.
- Hermes dan Nanobot dijalankan melalui CMD pada PC Windows pribadi pengguna. Rancangan awal mengikuti peluncuran dari CMD; lokasi executable dan konfigurasi diverifikasi saat implementasi.
- Hermes: NousResearch/hermes-agent, sesuai tautan dokumentasi CLI pengguna.
- Nanobot: HKUDS/nanobot, sesuai tautan repositori pengguna.
- Digunakan sendiri oleh pemilik.
- Prioritas MVP: satu dashboard untuk memilih agent dan mengirim tugas lintas platform.
- Diskusi dan penetapan kebutuhan mendahului implementasi.

## 3. Asumsi sementara

Asumsi berikut merupakan usulan untuk dibahas, bukan keputusan pengguna:

- Keenam agent sudah ada dan tetap berjalan di lingkungan masing-masing.
- Setiap agent diregistrasikan sebagai koneksi terpisah, termasuk empat agent yang memakai platform sama.
- Alur MVP berupa tugas ke satu agent. Penerusan hasil antarsistem diusulkan untuk tahap berikutnya.
- Pengguna mengakses dashboard melalui laptop; tampilan dasar tetap dapat digunakan di ponsel.
- Aplikasi tidak mengganti model atau runtime agent yang sudah digunakan.

## 4. Masalah dan tujuan

Pengguna perlu berpindah antarmuka untuk memberi instruksi kepada agent dari beberapa platform. Status, percakapan, dan hasil pekerjaan tersebar, sehingga sulit melacak siapa mengerjakan apa dan meneruskan konteks secara konsisten.

Tujuan produk:

1. Menyediakan daftar keenam agent beserta kemampuan dan kondisi koneksinya.
2. Menyediakan satu alur untuk mengirim tugas dan melihat progresnya.
3. Menjaga riwayat instruksi, hasil, dan hubungan antartugas.
4. Memungkinkan hasil satu agent digunakan sebagai input agent lain dengan kendali pengguna.
5. Menampilkan keterbatasan setiap platform secara jelas.

## 5. Batas MVP

### Termasuk

- Login pemilik dan pengaturan koneksi agent.
- Daftar agent, detail agent, dan pemeriksaan koneksi.
- Tugas berbasis teks kepada satu agent.
- Riwayat tugas dan pembaruan status.
- Hasil berupa teks serta referensi berkas apabila platform mendukungnya.
- Antrean tugas dan batas jumlah tugas bersamaan per agent.
- Pencatatan kegagalan, timeout, dan status yang belum dapat dipastikan.
- Pembatalan untuk platform yang mendukungnya.

### Tahap berikutnya

- Penerusan hasil secara manual ke agent pada platform lain.
- Pengiriman satu instruksi ke beberapa agent sekaligus dan perbandingan hasil.
- Workflow otomatis berurutan, percabangan, dan perulangan terbatas.
- Penjadwalan, notifikasi eksternal, dan pemilihan agent otomatis.
- Akun tim, pembagian peran, dan workspace tim dalam dashboard. Pemisahan empat workspace Muse sudah termasuk MVP.
- Penghitungan biaya jika data penggunaan tersedia dari penyedia.

### Di luar cakupan awal

- Melatih model atau membuat runtime agent baru.
- Mengelola provisioning mesin tempat agent berjalan.
- Menganggap semua agent dapat menjalankan perintah shell.
- Otomatisasi browser sebagai pengganti integrasi resmi tanpa kajian terpisah.
- Percakapan otonom antarseluruh agent tanpa batas langkah atau kendali pengguna.

Prioritas MVP sudah dikonfirmasi: memilih agent dan mengirim tugas. Cakupan integrasi Muse bergantung pada verifikasi antarmuka layanan tersebut.

## 6. Alur utama pengguna

### A. Menghubungkan agent

1. Pengguna menambahkan nama, platform, dan konfigurasi koneksi.
2. Pengguna memasukkan kredensial melalui formulir aman bila diperlukan.
3. Aplikasi memeriksa koneksi dan mencatat kemampuan yang didukung.
4. Agent muncul di dashboard dengan waktu pemeriksaan terakhir.

### B. Memberikan tugas

1. Pengguna memilih agent dan menuliskan instruksi.
2. Pengguna meninjau konteks atau hasil sebelumnya yang akan disertakan.
3. Aplikasi membuat tugas dan menempatkannya dalam antrean.
4. Adapter mengirim tugas dan menyimpan ID pekerjaan dari platform, jika tersedia.
5. Pengguna melihat status, pembaruan yang tersedia, dan hasil akhir.

### C. Meneruskan tugas lintas platform (tahap berikutnya)

Contoh ilustratif: Muse-1 menyusun bahan riset, Hermes mengolah hasil tersebut, lalu Nanobot menindaklanjuti instruksi yang dipilih pengguna. Peran ini hanya contoh, bukan kemampuan yang sudah diverifikasi.

1. Pengguna membuka hasil tugas yang sudah selesai.
2. Pengguna memilih tindakan “Teruskan ke agent”.
3. Pengguna memilih agent tujuan, bagian hasil yang disertakan, dan instruksi baru.
4. Aplikasi menampilkan pratinjau data yang akan dikirim.
5. Setelah pengguna mengirim, aplikasi membuat tugas baru yang terhubung ke tugas asal.

Konteks antarsistem dikirim sebagai data terpilih. Sesi internal dan memori platform tidak diasumsikan dapat dipindahkan langsung.

## 7. Kebutuhan fungsional dan kriteria penerimaan

| ID | Kebutuhan | Kriteria penerimaan |
| --- | --- | --- |
| FR-01 | Registrasi agent | Enam agent dapat disimpan sebagai identitas berbeda; mengubah satu koneksi tidak mengubah koneksi lain. |
| FR-02 | Pemeriksaan koneksi | Dashboard membedakan koneksi berhasil, gagal, belum diperiksa, dan informasi kedaluwarsa; menampilkan waktu pemeriksaan. |
| FR-03 | Kemampuan agent | Fitur pengiriman, streaming, berkas, pembatalan, dan pemeriksaan status mengikuti kemampuan adapter yang terverifikasi. |
| FR-04 | Pengiriman instruksi | Instruksi tersimpan sebelum pengiriman; pengguna dapat melihat agent tujuan, waktu, dan statusnya. |
| FR-05 | Antrean | Batas tugas bersamaan dapat ditetapkan per agent; tugas berikutnya menunggu saat batas tercapai. |
| FR-06 | Pemantauan | Status dapat diperbarui melalui webhook atau polling sesuai dukungan platform; hilangnya koneksi tidak ditampilkan sebagai keberhasilan. |
| FR-07 | Riwayat | Pengguna dapat mencari dan menyaring tugas berdasarkan agent, status, serta tanggal. |
| FR-08 | Handoff (tahap berikutnya) | Hasil terpilih dapat diteruskan ke platform lain; tugas baru menyimpan tautan ke tugas asal dan instruksi tambahan. |
| FR-09 | Penanganan kegagalan | Pesan kegagalan dapat dipahami dan membedakan koneksi, autentikasi, batas penggunaan, serta kegagalan pekerjaan. |
| FR-10 | Pembatalan | Tugas antrean dapat dibatalkan; pembatalan pekerjaan berjalan hanya dinyatakan berhasil setelah ada konfirmasi yang memadai. |
| FR-11 | Pencegahan duplikasi | Pengiriman ganda dari UI tidak membuat dua tugas; retry eksternal tidak dilakukan otomatis jika ada risiko tugas sudah diterima. |
| FR-12 | Audit | Pengiriman, perubahan koneksi, penerusan, retry, dan pembatalan tercatat tanpa menyimpan kredensial dalam log. |

## 8. Halaman aplikasi

### Dashboard

Ringkasan enam agent, kondisi koneksi, tugas aktif, antrean, dan kegagalan terbaru. Tombol utama: “Buat tugas”. Informasi yang tidak tersedia ditampilkan sebagai “Belum diketahui”.

### Agent

Daftar dan detail agent: nama, platform, deskripsi peran, kemampuan, kondisi koneksi, tugas terakhir, serta konfigurasi batas pekerjaan bersamaan.

### Tugas

Daftar tugas dengan filter. Halaman detail menampilkan instruksi, agent tujuan, lini waktu status, hasil, hubungan tugas asal/lanjutan, dan tindakan yang didukung.

### Pengaturan

Pengelolaan koneksi, kredensial, timeout, serta kebijakan penyimpanan riwayat. Nilai rahasia tidak dapat dibaca kembali melalui UI.

## 9. Rancangan integrasi

Satu backend menyediakan API internal yang konsisten. Adapter terpisah menerjemahkan operasi ke protokol masing-masing platform.

| Platform | Jumlah agent | Informasi yang perlu dikonfirmasi | Status |
| --- | --- | --- | --- |
| Muse.ai | 4 akun / 4 workspace, cloud | Contoh tugas, API, autentikasi per akun, model sesi | Identitas dan pemisahan akun/workspace dikonfirmasi; kemampuan integrasi belum diverifikasi |
| NousResearch/hermes-agent | 1, Windows | Versi terpasang, executable CMD, profil, format hasil dan persetujuan tool | CLI noninteraktif dan dukungan Windows didokumentasikan; belum diuji pada PC pengguna |
| HKUDS/nanobot | 1, Windows | Versi terpasang, executable CMD, ketersediaan plugin API | API lokal dan dukungan Windows didokumentasikan; belum diuji pada PC pengguna |

### Temuan dokumentasi dan pilihan adapter

**Hermes:** dokumentasi CLI mencantumkan `hermes chat -q "Hello"` untuk satu permintaan noninteraktif, serta `--query-file` untuk input dari berkas atau stdin. Kandidat MVP adalah worker lokal yang memanggil CLI menggunakan argumen proses terpisah, tanpa merangkai prompt menjadi perintah shell. Pengambilan hasil terstruktur, kelanjutan sesi, dan perilaku persetujuan tool harus diuji terhadap versi terpasang. Exit code saja belum cukup untuk membuktikan tugas pengguna berhasil. Jika agent menunggu persetujuan tool, dashboard perlu menampilkan kondisi menunggu pengguna; persetujuan tidak dilewati otomatis.

README Hermes saat diperiksa mendokumentasikan Windows native maupun WSL2. Pengguna mengonfirmasi peluncuran melalui CMD; adapter dirancang mengikuti executable dan konfigurasi yang digunakan dari CMD. Tidak ada kebutuhan WSL2 yang ditetapkan.

**Nanobot:** dokumentasi pada branch `main` mendokumentasikan plugin API, `nanobot serve`, endpoint `GET /health`, `GET /v1/models`, dan `POST /v1/chat/completions`. API secara default menggunakan loopback port 8900. Ini kandidat adapter utama; CLI menjadi opsi bila API tidak tersedia pada versi terpasang.

API Nanobot mendokumentasikan `session_id`, streaming SSE, dan tepat satu pesan `user` per permintaan. Adapter harus memberi ID sesi eksplisit agar tugas berbeda tidak bercampur dalam sesi default. Riwayat format OpenAI tidak boleh diteruskan begitu saja sebagai banyak pesan. Dokumentasi ini belum membuktikan dukungan pemulihan job atau pembatalan; keduanya perlu pemeriksaan tersendiri.

**Muse:** pengguna telah mengonfirmasi https://muse.ai/. Belum ada dasar untuk memilih endpoint atau metode autentikasi. Empat agent yang dimaksud pengguna adalah empat akun dengan empat workspace berbeda. Contoh tugas yang biasa dikirim dan akses API masih perlu diperjelas. Setiap koneksi harus menggunakan autentikasi serta konteks akun/workspace miliknya sendiri. Dukungan kontrol agent tidak boleh disimpulkan hanya dari alamat situs. Jangan mengganti identitas layanan dengan produk lain berdasarkan kemiripan nama.

### Topologi Windows yang diusulkan

Untuk pemakaian pribadi dari PC yang sama, dashboard lokal adalah usulan awal: browser → backend lokal → CLI Hermes/API Nanobot, serta backend → layanan Muse melalui jaringan. Jika akses di luar PC diperlukan, evaluasi hosting cloud dan bridge keluar dari PC. Pilihan ini belum disetujui pengguna.

Adapter lokal mengikuti konteks peluncuran CMD yang sudah digunakan: lokasi executable, direktori kerja, dan konfigurasi runtime dicatat saat pemeriksaan integrasi. Prompt dikirim melalui argumen proses atau stdin yang didukung, bukan disisipkan ke string `cmd /c`. Kredensial model yang sudah dikonfigurasi pada agent tetap digunakan melalui runtime tersebut. Tidak perlu meminta kunci model baru hanya untuk membuat dashboard.

Kontrak adapter yang diusulkan:

- `checkConnection`: menguji koneksi tanpa memulai pekerjaan.
- `getCapabilities`: mengembalikan operasi yang benar-benar didukung.
- `submitTask`: mengirim tugas dan mendapatkan tanda penerimaan.
- `getTaskStatus`: membaca status bila platform mendukungnya.
- `getTaskResult`: mengambil hasil.
- `cancelTask`: opsional, berdasarkan kemampuan platform.
- `receiveEvent`: opsional, menerima webhook atau event streaming.

Prioritas metode integrasi: API/SDK resmi, kemudian CLI resmi melalui proses terkontrol atau bridge dekat runtime. Jika agent hanya dapat diakses melalui UI, dukungan platform tersebut menjadi pertanyaan kelayakan, bukan fitur yang diasumsikan selesai.

Hermes dan Nanobot berjalan di PC Windows pribadi. Dua opsi deployment perlu dipertimbangkan:

1. Dashboard dan backend berjalan di PC yang sama: adapter lokal mengakses runtime agent dan adapter Muse menghubungi layanan cloud. Ini opsi awal yang lebih sederhana jika akses dari luar rumah tidak diperlukan.
2. Dashboard dan backend berjalan di cloud: bridge kecil di PC membuka koneksi keluar yang terautentikasi untuk mengambil tugas dan mengirim hasil. Metode ini perlu dibuktikan terhadap API/CLI agent yang sebenarnya.

Pilihan deployment belum diputuskan. Untuk kedua opsi, dashboard menampilkan waktu koneksi terakhir. Jika PC terputus, tugas baru ke agent lokal ditolak dengan penjelasan pada MVP agar tidak tiba-tiba dieksekusi ketika PC menyala kembali. Tugas yang sudah dikirim direkonsiliasi setelah koneksi pulih; pemutusan koneksi tidak membuktikan pekerjaan berhenti.

## 10. Siklus tugas dan keandalan

Status internal yang diusulkan: `queued`, `dispatching`, `running`, `succeeded`, `failed`, `cancel_requested`, `cancelled`, `waiting_for_user`, dan `unknown`.

- `succeeded` hanya digunakan setelah penyelesaian terkonfirmasi.
- `waiting_for_user` digunakan bila adapter dapat mendeteksi kebutuhan persetujuan atau input interaktif. Mekanisme melanjutkan pekerjaan harus dibuktikan; bila tidak tersedia, keterbatasan ini ditampilkan kepada pengguna.
- Timeout pengiriman dapat berarti tugas sudah diterima tetapi respons hilang. Kondisi ini masuk rekonsiliasi atau `unknown`, bukan langsung dikirim ulang.
- Timeout pemantauan tidak otomatis menghentikan agent di platform asal.
- Retry otomatis hanya untuk operasi yang aman atau ketika platform mendukung idempotensi.
- Restart backend memulihkan antrean dan memeriksa kembali pekerjaan berjalan dari data persisten.
- Pembaruan yang datang dua kali atau tidak berurutan diproses tanpa menggandakan tugas atau menimpa hasil akhir dengan status lama.
- Jika pembatalan tidak didukung, UI menjelaskan bahwa aplikasi tidak dapat menghentikan pekerjaan tersebut.

## 11. Data utama

| Entitas | Data utama |
| --- | --- |
| Agent | ID, nama, platform, konfigurasi nonrahasia, referensi kredensial, kemampuan, kondisi koneksi |
| Task | ID, agent tujuan, instruksi, konteks, status, ID eksternal, waktu, tugas asal |
| TaskAttempt | Percobaan pengiriman, kunci idempotensi jika didukung, respons penerimaan, kegagalan |
| TaskEvent | Perubahan status, sumber pembaruan, waktu platform, waktu diterima |
| Artifact | Teks hasil atau referensi berkas, tipe, ukuran, tugas sumber |
| AuditEvent | Aktor, tindakan, objek, waktu, ringkasan tanpa rahasia |

Retensi data dan batas ukuran konteks/berkas ditetapkan sebelum implementasi fitur penyimpanan. Riwayat penuh tidak otomatis disertakan ke setiap tugas baru.

## 12. Keamanan dan kendali akses

- Dashboard menggunakan autentikasi, termasuk bila hanya ada satu pengguna.
- Kredensial disimpan di sisi server melalui penyimpanan rahasia atau enkripsi yang sesuai lingkungan deployment.
- Kredensial tidak dikirim ke browser atau dimasukkan ke prompt agent.
- Koneksi eksternal menggunakan HTTPS bila tersedia; webhook diverifikasi sesuai mekanisme platform.
- Endpoint koneksi hanya dapat dikonfigurasi pemilik; backend membatasi tujuan jaringan yang dapat diakses adapter.
- Hasil agent dirender sebagai konten tidak tepercaya dan tidak dieksekusi sebagai skrip atau perintah backend.
- Pengiriman tugas adalah instruksi kepada agent, bukan izin umum menjalankan shell pada server dashboard.
- Saat fitur handoff ditambahkan, data hanya diteruskan ke agent lain setelah tindakan eksplisit pengguna.

## 13. Arsitektur dan opsi teknologi

Arsitektur awal: browser → backend → antrean/worker → adapter platform → agent. Database menyimpan konfigurasi nonrahasia, tugas, dan riwayat. Pembaruan UI menggunakan SSE atau polling; protokol akhir mengikuti kebutuhan nyata.

Usulan teknologi, belum diputuskan:

- Frontend: Next.js dan TypeScript.
- Backend serta worker: TypeScript agar kontrak adapter konsisten.
- Database: SQLite diusulkan dalam PLAN.md untuk aplikasi lokal satu pengguna; PostgreSQL tetap menjadi opsi jika kebutuhan deployment membenarkannya. Pilihan final ditentukan pada tahap desain.
- Deployment: satu server/container stack pada tahap awal, setelah lokasi agent dan kebutuhan jaringan diketahui.

Jika SDK utama agent lebih matang di Python, worker Python dapat dipilih. Pemilihan stack dilakukan setelah pemeriksaan integrasi, agar tidak menambah komponen tanpa kebutuhan.

## 14. Sasaran kualitas

Sasaran awal berikut perlu diuji; belum merupakan hasil pengukuran:

- Halaman utama dan riwayat merespons dalam dua detik pada beban enam agent dan sekitar 1.000 tugas tersimpan.
- Dengan event streaming, pembaruan ditampilkan dalam dua detik setelah diterima backend.
- Untuk polling, interval awal 5–15 detik disesuaikan dengan rate limit penyedia.
- Tugas yang sudah diterima aplikasi tetap tercatat setelah backend restart.
- Kegagalan satu adapter tidak menghentikan pekerjaan agent lain.
- Tidak ada target waktu selesai untuk pekerjaan agent sebelum karakteristik platform diketahui.

## 15. Validasi sebelum dinyatakan selesai

1. Catat versi, executable, dan konfigurasi Hermes/Nanobot yang digunakan melalui CMD, serta metode integrasi Muse; cocokkan kemampuan dokumentasi dengan versi terpasang.
2. Hubungkan satu agent per platform dan buktikan instruksi sederhana menghasilkan respons yang dapat diambil.
3. Registrasikan empat Muse.ai secara terpisah dan pastikan tugas diterima agent yang dipilih.
4. Jalankan tugas melalui dashboard pada keenam agent.
5. Putuskan koneksi PC dan pastikan agent lokal ditampilkan tidak tersedia serta pengiriman baru ditolak dengan penjelasan. Uji handoff dan hubungan riwayat pada tahap berikutnya.
6. Uji kredensial salah, koneksi terputus, timeout pengiriman, event duplikat, dan restart worker.
7. Uji pembatalan hanya pada adapter yang mendukungnya; cek tampilan keterbatasan pada adapter lainnya.
8. Pastikan kredensial tidak muncul di browser, log, atau hasil ekspor.

Pengujian dengan adapter simulasi digunakan untuk pengembangan, tetapi tidak menggantikan bukti integrasi nyata. MVP yang mencakup semua platform belum selesai jika salah satu platform wajib belum dapat menerima tugas dan mengembalikan hasil.

## 16. Tahapan pengerjaan yang diusulkan

### Tahap 0 — Diskusi dan kelayakan

Lokasi agent, Windows dengan CMD, identitas Hermes/Nanobot, URL Muse, empat akun dengan empat workspace terpisah, penggunaan pribadi, dan prioritas memilih agent sudah dikonfirmasi. Berikutnya perjelas akses API Muse, versi agent lokal, serta kebutuhan akses dashboard dari luar PC. Hasilnya: PRD revisi dan keputusan ruang lingkup MVP.

### Tahap 1 — Pembuktian integrasi

Uji pengiriman tugas dan pengambilan hasil pada satu agent untuk setiap platform. Catat keterbatasan sebelum membangun dashboard lengkap.

### Tahap 2 — Dashboard MVP

Bangun autentikasi, inventaris enam agent, tugas, antrean, pemantauan, dan riwayat.

### Tahap 3 — Ketahanan MVP

Tambahkan pemulihan setelah restart, penanganan koneksi PC terputus, penanganan duplikasi, dan pengujian menyeluruh.

### Tahap 4 — Perluasan

Evaluasi handoff manual, broadcast, workflow otomatis, penjadwalan, biaya, dan kolaborasi berdasarkan kebutuhan penggunaan nyata.

Estimasi waktu dan biaya belum ditetapkan sebelum akses integrasi terverifikasi.

## 17. Pertanyaan diskusi

Pertanyaan penentu ruang lingkup:

1. Apa contoh tugas yang biasa diberikan pada masing-masing akun/workspace Muse?
2. Bagaimana pengguna memberi perintah kepada masing-masing agent saat ini?
3. Apa versi Hermes dan Nanobot yang terpasang? Ketersediaan CLI/API perlu dicocokkan dengan versi tersebut.
4. Saat masuk tahap implementasi, apa perintah peluncuran dan lokasi executable Hermes/Nanobot yang digunakan di CMD? Jangan menyertakan kredensial.
5. Apakah dashboard cukup diakses dari PC/jaringan rumah, atau perlu dapat dibuka dari luar?

Pertanyaan lanjutan setelah integrasi jelas:

- Apa peran masing-masing agent dan satu contoh pekerjaan nyata yang ingin dipermudah?
- Apakah tugas perlu berkas, akses repositori, atau hanya teks?
- Apakah dashboard akan dijalankan lokal, di VPS, atau layanan hosting tertentu?
- Apakah ada tindakan yang perlu persetujuan sebelum agent menjalankannya?
- Berapa lama riwayat dan hasil pekerjaan perlu disimpan?

## 18. Status keputusan

Belum ada implementasi aplikasi atau konfigurasi integrasi yang disetujui. Dokumen ini adalah dasar diskusi; jawaban pengguna akan digunakan untuk mengubah asumsi menjadi kebutuhan final.


## 19. Sumber dan batas verifikasi

Diperiksa pada 8 Oktober 2026, hanya untuk riset PRD:

- [Dokumentasi CLI Hermes dari pengguna](https://hermes-agent.nousresearch.com/docs/user-guide/cli). Situs dokumentasi ditolak proxy jaringan lingkungan ini; isi dokumentasi dibaca melalui [sumber resmi di GitHub](https://github.com/NousResearch/hermes-agent/blob/main/website/docs/user-guide/cli.md).
- [README resmi Hermes](https://github.com/NousResearch/hermes-agent/blob/main/README.md): mode instalasi Windows native dan WSL2.
- [Repositori Nanobot dari pengguna](https://github.com/HKUDS/nanobot): dukungan Windows dan jalur integrasi.
- [Dokumentasi API Nanobot](https://github.com/HKUDS/nanobot/blob/main/docs/openai-api.md): endpoint, sesi, batas pesan, dan streaming.
- [Muse.ai](https://muse.ai): disebut pengguna; akses situs ditolak proxy jaringan lingkungan ini, sehingga kemampuan integrasinya belum dapat diverifikasi.

Dokumentasi branch `main` dapat berubah dan berbeda dari rilis yang terpasang. Belum ada instalasi, eksekusi agent, akses PC pengguna, atau pengujian integrasi nyata. Pemeriksaan jaringan yang gagal tidak membuktikan layanan Muse tidak menyediakan API.
