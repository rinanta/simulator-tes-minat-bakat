# Product Requirements Document (PRD)
**Nama Proyek:** Simulasi Tes Minat & Bakat Interaktif (Web-Based Aptitude & Interest Simulator)  
**Target Konteks:** Technical Assessment / Interview Challenge (Frontend / Web Developer)  
**Format Output:** Single-File Web Application (`index.html` dengan integrasi Tailwind CSS CDN & Vanilla JavaScript)  

---

## 1. Ringkasan Eksekutif & Tujuan Produk
Aplikasi ini adalah simulator web interaktif yang dirancang untuk membantu pengguna mengenali potensi minat dan bakat mereka melalui serangkaian pertanyaan psikometrik sederhana, lalu menyajikan analisis hasil beserta rekomendasi jalur karier/pengembangan diri secara visual dan interaktif.

**Tujuan Utama (Konteks Interview Perekrut):**
* Mendemonstrasikan penguasaan *fundamental web development* (HTML5, CSS3/Tailwind, Vanilla JS) tanpa bergantung pada *framework* berat, dikemas dalam satu file yang rapi dan mudah dijalankan.
* Menampilkan kemampuan **UI/UX Design**, khususnya dalam pemilihan palet warna (*color psychology*), *micro-interactions* (animasi kuis), dan visualisasi data hasil akhir.
* Menunjukkan logika pemrograman yang bersih dalam mengelola *state* kuis, perhitungan skor multi-dimensi, dan manipulasi DOM yang dinamis.

---

## 2. Target Pengguna & Persona
* **Pengguna Akhir (User):** Pelajar, mahasiswa, atau *job seeker* yang ingin mengetahui kecenderungan minat dan bakat mereka dengan cara yang menyenangkan dan tidak membosankan.
* **Penilai (Perekrut / Interviewer):** Tech Lead atau HR yang mengevaluasi kerapian kode, responsivitas desain, kreativitas animasi, dan kelengkapan alur *end-to-end* aplikasi.

---

## 3. Arsitektur & Batasan Teknis (Technical Constraints)
Untuk memenuhi syarat dari perekrut agar mudah diuji dan di-deploy:
1. **Single-File Architecture:** Seluruh struktur, gaya, dan logika berada dalam satu file `index.html` (atau terpisah maksimal 1 file HTML jika menggunakan CDN).
2. **Styling:** Menggunakan **Tailwind CSS (via Play CDN)** untuk *layouting* cepat dan responsif, dikombinasikan dengan blok `<style>` kustom untuk animasi *keyframe* kompleks dan efek *glassmorphism*.
3. **Logic & Interactivity:** Menggunakan **Vanilla JavaScript (ES6+)** untuk *state management*, kalkulasi skor, dan transisi layar.
4. **Visualisasi Grafik (Opsional namun Nilai Plus):** Menggunakan **Chart.js (via CDN)** atau *Custom SVG/CSS Animated Progress Bars* untuk menampilkan grafik radar/batang pada halaman hasil tanpa memerlukan *build tools*.
5. **Zero-Backend / Client-Side Only:** Semua data pertanyaan, bobot nilai, dan logika rekomendasi disimpan dalam struktur objek JSON di dalam JavaScript, serta memanfaatkan `localStorage` untuk menyimpan riwayat tes terakhir.

---

## 4. Konsep Desain, Warna, & Animasi (UI/UX Specs)

### A. Permainan Warna (Dynamic Color Palette)
Aplikasi harus terasa hidup, modern, dan ramah secara psikologis:
* **Latar Belakang Utama:** Gradasi lembut (*Soft Mesh Gradient*) menggunakan kombinasi warna *Indigo*, *Violet*, dan *Sky Blue* muda agar nyaman di mata.
* **Aksen Utama (Primary):** `#6366F1` (Indigo 500) hingga `#8B5CF6` (Violet 500) untuk tombol aksi dan indikator progres.
* **Warna Kategori Bakat (Dynamic Theme Shifting):** Saat hasil keluar, aksen warna kartu hasil akan berubah dinamis menyesuaikan tipe kepribadian/bakat dominan pengguna:
  * **Analitis & Logis (Tech/Data):** Biru Cyan (`#06B6D4` - `#3B82F6`)
  * **Kreatif & Artistik (Design/Media):** Magenta & Oranye (`#EC4899` - `#F97316`)
  * **Sosial & Komunikasi (Leadership/HR):** Hijau Emerald (`#10B981` - `#14B8A6`)
  * **Praktis & Eksploratif (Engineering/Field):** Kuning Amber (`#F59E0B` - `#EAB308`)

### B. Animasi Kuis & Micro-Interactions
1. **Transisi Kartu Soal:** Efek *slide-in* dari kanan dan *fade-out* ke kiri saat berpindah antar pertanyaan menggunakan CSS transitions/keyframes.
2. **Hover & Click Feedback:** Pilihan jawaban membesar sedikit (`scale: 1.02`), memiliki efek *border glow*, dan memberikan umpan balik visual instan saat diklik sebelum otomatis lanjut ke soal berikutnya.
3. **Animated Progress Bar:** Bilah kemajuan di bagian atas yang bergerak mulus (*smooth width transition*) disertai indikator persentase atau nomor soal (misal: *Pertanyaan 3 dari 10*).
4. **Loading / Analyzing Screen:** Animasi *pulse* atau *spinner* selama 1.5–2 detik setelah soal terakhir dijawab dengan teks dinamis (misal: *"Menganalisis pola jawabanmu..."*, *"Mencocokkan dengan profil karier..."*) untuk membangun antisipasi pengguna.
5. **Result Reveal Animation:** Kartu hasil muncul dengan efek *pop-up* (scale & fade) serta animasi *confetti* sederhana (menggunakan JS/CSS) untuk merayakan selesainya tes.

---

## 5. Alur Pengguna & Spesifikasi Fitur (User Flow)

### Tahap 1: Landing / Welcome Screen
* **Hero Section:** Judul menarik, subjudul singkat yang menjelaskan manfaat tes (estimasi waktu pengerjaan: 2 menit).
* **Input Nama (Opsional):** Input field sederhana agar hasil terasa lebih personal (misal: *"Halo, Budi! Ini adalah peta potensimu"*).
* **Tombol CTA:** Tombol *"Mulai Tes Sekarang"* dengan efek *hover* interaktif.
* **Fitur Tambahan:** Tombol *"Lihat Hasil Terakhir"* jika data tersimpan di `localStorage`.

### Tahap 2: Sesi Kuis Interaktif (Quiz Engine)
* **Struktur Soal:** Terdiri dari 8 hingga 12 pertanyaan situasional/preferensi (cukup singkat untuk demo interview, namun cukup akurat untuk kalkulasi).
* **Format Jawaban:** Pilihan ganda (4 opsi per soal) atau Skala Likert interaktif (Sangat Tidak Setuju s/d Sangat Setuju) dengan ikon/emoji ekspresif.
* **Navigasi:**
  * Otomatis lanjut (*auto-advance*) setelah pengguna memilih jawaban (dengan jeda 300ms agar pengguna melihat pilihan yang diklik).
  * Tombol *"Kembali"* (*Previous*) jika pengguna ingin mengubah jawaban sebelumnya.

### Tahap 3: Layar Analisis (Processing State)
* Transisi otomatis setelah pertanyaan terakhir dijawab.
* Menampilkan animasi visual pemrosesan data selama ±2 detik sebelum menampilkan hasil.

### Tahap 4: Halaman Hasil & Rekomendasi Interaktif (Result Dashboard)
1. **Kartu Persona Utama (Hero Card):**
   * Menampilkan julukan/tipe bakat utama (contoh: *"The Visionary Creator"* atau *"The Strategic Problem Solver"*).
   * Deskripsi singkat mengenai kekuatan utama dan gaya kerja pengguna.
2. **Visualisasi Skor Multi-Dimensi:**
   * Grafik batang horizontal beranimasi atau *Radar Chart* yang menunjukkan persentase kecenderungan pada 4 kategori utama (Analitis, Kreatif, Sosial, Praktis).
3. **Rekomendasi Karier & Skill (Tab/Accordion Interaktif):**
   * **Rekomendasi Profesi:** 3–4 peran pekerjaan yang cocok (misal: *UI/UX Designer, Data Analyst, Product Manager*).
   * **Saran Pengembangan Skill:** Daftar keahlian yang disarankan untuk dipelajari selanjutnya.
4. **Aksi Pengguna (Action Buttons):**
   * **Ulangi Tes:** Mereset *state* dan kembali ke awal dengan mulus.
   * **Salin / Bagikan Hasil:** Fitur *Copy to Clipboard* yang menyalin ringkasan teks hasil tes untuk dibagikan, disertai notifikasi *toast* ("Hasil berhasil disalin!").

---

## 6. Struktur Data & Logika Penilaian (Scoring Logic)

Setiap opsi jawaban memiliki bobot poin untuk masing-masing dimensi kategori (`analytical`, `creative`, `social`, `practical`).

**Contoh Struktur Objek Soal (JavaScript):**
```javascript
const questions = [
  {
    id: 1,
    question: "Saat memulai proyek baru, hal pertama yang paling membuatmu bersemangat adalah...",
    options: [
      { text: "Mencari data dan menyusun struktur rencana yang logis", scores: { analytical: 3, creative: 0, social: 1, practical: 1 } },
      { text: "Membayangkan konsep visual dan ide-ide unik yang belum pernah ada", scores: { analytical: 0, creative: 3, social: 1, practical: 0 } },
      { text: "Berdiskusi dan membagi peran dengan anggota tim", scores: { analytical: 1, creative: 1, social: 3, practical: 0 } },
      { text: "Langsung membuat prototipe atau mencoba alatnya secara langsung", scores: { analytical: 1, creative: 1, social: 0, practical: 3 } }
    ]
  }
];
```

**Logika Penentuan Hasil:**
1. Akumulasi seluruh skor dari setiap jawaban yang dipilih pengguna.
2. Konversi skor masing-masing kategori menjadi persentase berdasarkan skor maksimal yang mungkin diraih.
3. Cari kategori dengan nilai tertinggi sebagai **Primary Talent**, dan nilai tertinggi kedua sebagai **Secondary Talent** (untuk kombinasi deskripsi yang lebih kaya).

---

## 7. Kriteria Keberhasilan (Acceptance Criteria untuk Interview)
* [ ] Aplikasi berjalan lancar saat file `index.html` dibuka langsung di browser tanpa error di *Console*.
* [ ] Tampilan sepenuhnya responsif (rapi di layar desktop maupun mobile).
* [ ] Semua animasi transisi (antar soal, progress bar, dan kemunculan hasil) berjalan mulus (*60fps*, tidak patah-patah).
* [ ] Pengguna dapat kembali ke soal sebelumnya tanpa merusak perhitungan skor akhir.
* [ ] Terdapat perubahan tema warna dinamis pada layar hasil sesuai dengan kategori bakat yang didapatkan.
* [ ] Kode JavaScript terstruktur dengan baik (memisahkan data soal, fungsi render UI, dan fungsi kalkulasi skor).