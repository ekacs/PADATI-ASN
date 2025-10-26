# PADATI-ASN
Platform Agregator Data Talenta Indonesia (PADATI) dan Sistem Pendukung Keputusan Rekomendasi Talenta ASN

## Rancangan MVP Technical Specification (built with Stack Google Gratis/Firebase)

### **1. Executive Summary**

Platform PADATI adalah sistem manajemen talenta ASN *serverless* yang dirancang untuk mengintegrasikan data talenta K/LB dengan Sistem Informasi Nasional BKN. Platform ini dibangun di atas ekosistem **Google Firebase** dan  **Google Cloud (Free Tier)** , serta dilengkapi Sistem Pendukung Keputusan yang diperkaya dengan **Google AI Studio (Gemini API)** untuk analisis suksesi dan pengembangan SDM.

### **2. Tujuan Platform**

- Membangun Platform Agregator Data untuk mengotomatisasi pengumpulan, pengolahan, dan standardisasi data talenta sesuai model penilaian nasional BKN
- Menjamin kesiapan teknis dan data untuk integrasi penuh dengan sistem BKN
- Mengembangkan SPK Suksesi Strategis berbasis AI sebagai alat bantu analisis bagi Forum Pimpinan
- Memperkuat implementasi manajemen talenta yang strategis dan selaras dengan kebijakan nasional.

### **3. Fitur Utama MVP**

#### **3.1 Mesin Agregator dan Transformasi Data**

- Konektor ke berbagai sumber data internal (HRIS, e-Kinerja, dll)[1]
- Algoritma untuk menghitung skor Kinerja dan Potensial secara otomatis berdasarkan standar Kepmen BKN 411/2025[1]
- Fitur validasi dan pembersihan data sebelum dikirim ke sistem BKN[1]

#### **3.2 Dashboard SPK Suksesi Strategis**

- Visualisasi 9 Kotak Talenta yang bersumber dari data terintegrasi[1]
- Modul simulasi penempatan talenta dan analisis dampak[1]
- Fitur proyeksi kandidat suksesor untuk Jabatan Target dengan pemeringkatan otomatis[1]
- Modul peta kebutuhan jabatan fungsional dan struktural pada setiap unit kerja Kementerian/Lembaga/Badan

#### **3.3 Modul Personalisasi Pengembangan Diri**

- Menampilkan profil talenta individu yang terintegrasi[1]
- Rekomendasi program pengembangan (pelatihan, proyek, mentoring) yang relevan dengan hasil pemetaan dan kebutuhan strategis[1]

### **4. Arsitektur Sistem (Prototipe)**

#### **4.1 Technology Stack (Google Free Tier)**

**Frontend:**

* Framework: React.js atau Vue.js.
* Hosting: **Firebase Hosting** (Menyediakan hosting global, SSL otomatis, dan *free tier* yang besar).
* UI Library: Material-UI (MUI) (Native Google, atau Ant Design).

**Backend (Serverless):**

* API Framework: **Cloud Functions for Firebase** (Menjalankan kode *backend* Node.js atau Python tanpa perlu mengelola server. Menggantikan Node.js/Express.js).
* Authentication: **Firebase Authentication** (Mengelola login ASN via Google, email, dll. Sudah mencakup OAuth 2.0 dan aman by default).

**Data Processing Layer (ETL - MVP Free):**

* ETL Pipeline: **Google Colab (Python/Pandas)** atau  **Google Sheets + Apps Script** .
  * **Metode 1 (Rekomendasi):** Gunakan **Google Colab** untuk menjalankan skrip Python/Pandas gratis. Skrip ini menarik data dari berbagai sumber (CSV/API), melakukan *cleansing* & *scoring* (sesuai standar BKN), lalu mengirim hasilnya ke Firestore.
  * **Metode 2 (Simpel):** Gunakan **Google Sheets** sebagai *staging area* data mentah. **Google Apps Script** (gratis) dipicu untuk memproses data dan mendorongnya ke Firestore.

**Database (Serverless):**

* Primary Database: **Firestore (Firebase)** (Database NoSQL yang  *realtime* , fleksibel, dan  *scalable* . Menggantikan PostgreSQL untuk MVP).
* File Storage: **Cloud Storage for Firebase** (Untuk menyimpan file (misal: CSV data mentah) dengan *free tier* yang besar).

**Data Visualization (Dashboard SPK):**

* **Looker Studio (Gratis)** : Untuk membuat *dashboard* SPK (termasuk 9-Box) secara cepat. *Dashboard* ini dapat di-*embed* langsung ke dalam aplikasi Frontend (React/Vue) yang di-hosting di Firebase Hosting.

**AI/ML Components (Prototipe):**

* ML Framework: **Cloud Functions for Firebase** (untuk menjalankan algoritma *scoring* deterministik).
* Generative AI: **Google AI Studio (Gemini API)** (Menggantikan TensorFlow/PyTorch untuk  *recommendation engine* ).

**Infrastructure (Serverless):**

* Containerization / Orchestration:  **Tidak Diperlukan** . Digantikan oleh arsitektur *serverless* Firebase (Cloud Functions, Firebase Hosting).
* CI/CD: **Firebase CLI** diintegrasikan dengan **GitHub Actions (Free Tier)** (Menggantikan GitLab CI/Jenkins).
* Monitoring & Logging:  **Firebase Performance Monitoring** ,  **Crashlytics** , dan **Google Cloud Logging** (*Free tier* sudah terintegrasi, menggantikan ELK Stack/Prometheus).

#### **4.2 Security & Compliance**

* **Authentication & Authorization:** Dikelola oleh **Firebase Authentication** dan **Aturan Keamanan Firestore (Security Rules)** untuk RBAC (Admin, Pimpinan, ASN).
* (Lainnya tetap sama: Enkripsi TLS & AES-256 sudah *default* di Firebase).

### **5. Arsitektur Data**

#### **5.1 Data Sources**

* (Tetap sama) HRIS, e-Kinerja, Sistem Assessment.
* **Prototipe MVP:** Data mentah dapat diunggah sebagai CSV ke **Cloud Storage for Firebase** untuk diproses oleh **Cloud Function** atau  **Colab** .

#### **5.2 Data Model (Core Entities)**

* (Tetap sama) Employee/ASN, Talent Assessment, Succession Planning, Development Program.
* **Catatan:** Skema ini akan diimplementasikan sebagai *Collections* dan *Documents* di  **Firestore** .

#### **5.3 Data Flow (Revisi Stack Firebase)**

1. **Extract:** **Cloud Function** (dijadwalkan) atau **Google Colab** (dijalankan manual) menarik data dari API *source system* atau mengambil file CSV dari  **Cloud Storage** .
2. **Transform:** Skrip yang sama (Python/Node.js) melakukan  *cleansing* ,  *mapping* , dan menjalankan **Scoring Engine** (sesuai standar BKN).
3. **Validate:** Aturan validasi dijalankan.
4. **Load:** Data talenta yang sudah matang disimpan ke  **Firestore** .
5. **Analyze:**
   * **Looker Studio** membaca data dari Firestore (via konektor) untuk *dashboard* SPK.
   * **Cloud Function** memanggil **Gemini API** untuk  *development recommendations* .

### **6. API Architecture**

* **Internal APIs:** Digantikan oleh  **Cloud Functions for Firebase (Callable Functions)** . Frontend (React) akan memanggil *functions* ini secara langsung dan aman.
* **Integration APIs:** Tetap sama, akan dipanggil dari dalam  **Cloud Functions** .

### **7. AI/ML Components (Revisi Detail)**

#### **7.1 Scoring Engine (Tetap Deterministik)**

* **Input:** Data kinerja, kompetensi, assessment, pengalaman.
* **Algorithm:** Algoritma pembobotan standar Kepmen BKN 411/2025, ditulis dalam Python atau Node.js.
* **Lokasi:** Berjalan di dalam  **Cloud Function for Firebase** .

#### **7.2 Recommendation Engine (Generative AI)**

* **Input:** Profil ASN, posisi 9-Box, *competency gaps* dari Firestore.
* **Algorithm:** Menggunakan  **Google AI Studio (Gemini API)** .
* **Proses:** Sebuah **Cloud Function** dipanggil oleh aplikasi. *Function* ini merancang *prompt* (misal: "Anda adalah pakar SDM. Berikan 3 rekomendasi pengembangan karir untuk ASN dengan profil [data_profil] yang berada di Kotak [X]..."), memanggil Gemini API, dan mengembalikan hasilnya ke  *frontend* .

#### **7.3 Succession Planning AI (Natural Language Query)**

* **Input:** Pertanyaan bahasa natural dari pimpinan di *dashboard* SPK (misal: "Siapa talenta di Box 9 yang siap promosi ke JPT Pratama?").
* **Algorithm:** **Google AI Studio (Gemini API)** digunakan untuk mengubah pertanyaan ini menjadi kueri terstruktur (JSON) untuk Firestore.
* **Output:** Daftar kandidat suksesor yang telah difilter.

### **8. User Interface Design**

(*backend* -nya menggunakan Firebase).

- **Overview:** KPI manajemen talenta, compliance status
- **9-Box Visualization:** Interactive talent matrix dengan drill-down capability
- **Succession Pipeline:** Visual representation kandidat suksesor per jabatan
- **Analytics:** Trend analysis, gap analysis, scenario simulation

#### **8.2 Dashboard Admin**

- **Data Management:** Monitor data quality, integration status
- **User Management:** Role assignment, access control
- **System Monitoring:** Performance metrics, error logs
- **Configuration:** Scoring parameters, business rules

#### **8.3 Portal ASN**

- **My Profile:** Visualisasi profil talenta individu
- **My Development:** Rekomendasi program, progress tracking
- **Career Path:** Visualisasi career progression opportunities
- **Feedback:** Mechanism untuk input feedback

* **Dashboard Pimpinan:** Akan dibuat di React/Vue dan di-hosting di  **Firebase Hosting** , dengan *chart* yang di-*embed* dari  **Looker Studio** .
* **Dashboard Admin:** (Sama).
* **Portal ASN:** (Sama).

### **9. Development Phases**

(perkiraan pekerjaan setiap pada fase, menyesuaikan sarana/prasarana).

#### **Fase 1:(2-3 minggu)**

Harmonisasi dan Desain Skema  **Firestore**

- Analisis mendalam model data BKN vs Kemenhub
- Mapping parameter dan bobot penilaian
- Identifikasi gap dan transformation rules
- Design data model dan schema database
- **Deliverables:** Data mapping document, database schema, transformation rules

#### **Fase 2: (6-8 minggu)**

Pembangunan **Cloud Functions** (Scoring) dan ETL via  **Colab/Apps Script**

- Setup infrastructure dan development environment
- Develop ETL pipeline dan data connectors
- Implement scoring algorithm sesuai standar BKN
- Build data validation dan cleansing engine
- Develop API untuk integrasi BKN
- **Deliverables:** Working aggregator platform, API documentation, unit tests

#### **Fase 3: (6-8 minggu)**

Pengembangan Frontend (React/Vue) di  **Firebase Hosting** , integrasi  **Gemini API** , dan pembuatan *dashboard*  **Looker Studio**

- Develop frontend dashboard untuk Pimpinan
- Build 9-Box visualization dan simulation tools
- Implement recommendation engine
- Develop ASN self-service portal
- Integration dengan aggregator backend
- **Deliverables:** Complete web application, user guides, integration tests

#### **Fase 4: (4-6 minggu)**

Pengujian *end-to-end* di ekosistem Firebase

- System integration testing (SIT)
- User acceptance testing (UAT)
- Security testing dan audit
- Performance testing dan optimization
- Training untuk admin dan users
- Soft launch dan monitoring
- Integration testing dengan sistem BKN
- Go-live dan cutover
- **Deliverables:** Production-ready system, training materials, deployment documentation

**Total Timeline:** 18-25 minggu (4.5-6 bulan)

### **10. MVP Success Criteria (Attention)**

#### **Functional Requirements:**

- ✓ Berhasil mengagregasi data dari minimal 2 source systems
- ✓ Scoring algorithm menghasilkan output sesuai standar BKN dengan akurasi >95%
- ✓ Dashboard menampilkan 9-Box dengan real-time data
- ✓ Recommendation engine memberikan minimal 3 suggestions per user
- ✓ Sukses melakukan test integration dengan sistem BKN (sandbox/dev environment)
- ✓ Seluruh infrastruktur prototipe (hosting, database, functions, AI calls) berhasil berjalan 100% dalam batasan  **Google Free Tier** .

#### **Non-Functional Requirements:**

- **Performance:** Response time <2 detik untuk 95% requests
- **Availability:** Uptime 99% (excluding scheduled maintenance)
- **Scalability:** Support minimal 5,000 ASN concurrent users
- **Security:** Pass security audit untuk data sensitivity level II
- **Usability:** User satisfaction score >4/5 dalam UAT
- **Kriteria Bisnis:** Kepatuhan *readiness* integrasi BKN tercapai sebelum  *deadline* .

### **11. Risk Mitigation (Attention)**

* **Risiko Baru:** Ketergantungan pada *free tier* (Firebase/AI Studio) yang memiliki limitasi kuota.
* **Mitigasi:** Desain **Cloud Functions** agar efisien, implementasi *caching* sederhana di *functions* (jika perlu), dan siapkan rencana *upgrade* ke paket *pay-as-you-go* (Blaze) jika kuota terlampaui.

### **12. Deliverables**

Rancangan MVP ini dirancang modular dan scalable, memungkinkan pengembangan iteratif dengan prioritas pada core functionality yang critical untuk integrasi dengan BKN dan kepatuhan regulasi. Setiap fase dapat di-review dan di-adjust berdasarkan feedback dan learning dari fase sebelumnya.

###### ***created by ekacs***
