---
name: deep-investigate
description: Use when the user reports a bug, error, abnormal behavior, or requests creating, modifying, or deleting a feature that is not yet established in the current context window. Enforces a strict 5-step investigative protocol: pure location clarification, graphify-first context search, in-depth questioning with user self-check, visible streaming hypothesis verification, and structured RCA execution with todo tracking.
---

# Deep Investigate Skill

Protokol terarah untuk menginvestigasi masalah, laporan bug, atau permintaan fitur baru secara terstruktur, kolaboratif, dan berbasis data kode nyata.

## 📌 Kriteria Pemicu (Kapan Harus Digunakan)
Skill ini **WAJIB DIAKTIFKAN** saat:
1. **Permintaan Fitur (Create / Edit / Delete):** User meminta dibuatkan fitur baru, mengubah alur fitur, atau menghapus fitur yang **belum ada di context window** percakapan saat ini (lokasi, arsitektur, dan alurnya belum jelas).
2. **Keluhan Bug / Error / Perilaku Aneh:** User mengeluhkan bug, kendala, atau hal tidak terduga yang tidak jelas asal-muasalnya dan **tidak ada di context window** (contoh: *"ini kok gak bisa ya"*, *"ngebug nih"*, *"kok tampilannya kosong"*, *"kenapa tombol ini gak jalan"*, dsb.).

## 🚫 Kriteria Pengecualian (Kapan DILARANG Menggunakan Skill Ini)
Jangan gunakan skill ini jika:
1. **Pertanyaan Direct Spesifik:** Pertanyaan eksploratif langsung ke hal teknis tertentu (contoh: *"apa saja list endpoint project ini khususnya module chat"* -> langsung cari via `graphify` dan jawab langsung tanpa proses interview).
2. **Perintah Direct Berkelanjutan (Konteks Sudah Jelas):** Langkah instruksi langsung yang berkaitan erat dengan percakapan di context window sebelumnya (contoh: user meminta *"tombol 'edit' diwarnai biru"*, dan sebelumnya baru saja membahas/mengubah modal *"tambah template"* -> langsung eksekusi tanpa men-trigger skill ini).

---

## 🔄 Protokol 5-Langkah Wajib

### Langkah 1: Klarifikasi Letak Fitur & Problem Utama (Murni Tanpa Asumsi)
- Pertanyaan awal Agent **WAJIB murni mengonfirmasi letak fitur dan problem utamanya terlebih dahulu** (contoh: di halaman mana, menu apa, URL/route apa, dan apa yang diharapkan vs apa yang terjadi).
- **HINDARI DAN DILARANG KERAS** langsung menanyakan hal-hal berbasis asumsi teknis (seperti menebak status code API 200/500, error console browser, format payload, dsb.) sebelum letak fitur dipastikan dengan jelas.
- Gunakan dialog/tool interaktif bawaan agent app (seperti `ask_question`).

### Langkah 2: Pencarian Cepat Konteks Kode via Graphify (Quick Context Search)
- Setelah user mengonfirmasi letak fitur pada Langkah 1, Agent **WAJIB melakukan pencarian cepat di knowledge graph menggunakan `graphify`** (`graphify query "<nama fitur / menu / route>"`, `graphify path`, atau `graphify explain`) untuk:
  - Memahami arsitektur dan relasi komponen frontend / route backend terkait.
  - Memetakan alur data dan fungsi yang relevan.
  - Membangun analisis dugaan awal / hipotesis penyebab masalah berdasarkan kode nyata.
- **Catatan:** SEMUA perintah `graphify` langsung dijalankan secara otomatis tanpa meminta persetujuan user.

### Langkah 3: Pertanyaan Mendalam Terkait Fitur & Pengecekan Mandiri User
- Berbekal konteks arsitektur dari Langkah 2, Agent **HARUS mengajukan pertanyaan lanjutan yang mendalam** dan relevan dengan alur fitur tersebut sesuai hipotesa yang dibangun.
- Ajak user untuk mengecek hal-hal spesifik secara mandiri (misal: memeriksa filter aktif, input tertentu, atau tab Console/Network pada bagian fitur yang sudah dipetakan). User tidak boleh melulu pasif.

### Langkah 4: Paparan Hipotesis di Chat (Visible Regular Text) & Tanya Konfirmasi User
- Agent **WAJIB mencetak secara eksplisit melalui pesan teks reguler chat (visible streaming text)** — **DILARANG HANYA DITARUH DI INTERNAL THOUGHT / ACCORDION**:
  - **DUGAAN / HIPOTESIS:** Analisis dugaan penyebab masalah berdasarkan hasil klarifikasi dan temuan konteks kode di Langkah 1-3.
  - **RENCANA LANGKAH INVESTIGASI:** Alur penelusuran yang akan dilakukan untuk menemukan akar masalah (*root cause*).
- **WAJIB Konfirmasi ke User:** Agent **HARUS menanyakan kembali kepada user**:
  > *"Apakah Anda setuju dengan hipotesis dan rencana langkah investigasi di atas untuk kita lanjutkan, atau Anda memiliki HIPOTESIS / DUGAAN LAIN?"*
- Tunggu respons dan persetujuan user sebelum melangkah ke langkah berikutnya.

### Langkah 5: Pembuatan Todo List & Eksekusi Root Cause Analysis (Setelah Disetujui)
- Setelah user menyatakan setuju (atau setelah menyesuaikan rencana jika user memberikan hipotesa lain), **BARULAH Agent diperbolehkan membuat Todo List** (menggunakan fitur task tracking / artifact bawaan aplikasi).
- Lakukan eksekusi penelusuran mendalam untuk mencari *Root Cause Analysis* (RCA) secara komprehensif, dan perbarui checklist todo list secara real-time seiring berjalannya investigasi hingga tuntas.
