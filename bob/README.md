# IBM Bob Configuration & Custom Skill: `datasight-copilot`

Direktori ini berisi konfigurasi agen kognitif **IBM Bob**, termasuk definisi **Custom Skill** resmi (`datasight-copilot`), skema **Model Context Protocol (MCP)**, dan system prompt.

---

## 🧠 Mengapa IBM Bob Membutuhkan Custom Skill?

Dalam arsitektur **IBM Bob IDE**, **Skills** adalah unit modular resmi yang digunakan untuk memperluas kapabilitas kognitif agen AI. Berbeda dari prompt percakapan biasa:
- **Skill dimuat secara persisten** oleh Bob saat memulai sesi kerja.
- **Skill mengatur perilaku agen (Agent Behavior & Guardrails)**, termasuk kapan Bob boleh memanggil tools eksternal dan bagaimana menyajikan data kepada pengguna.
- **Skill menyediakan domain expertise spesifik** tanpa membebani konteks prompt secara statis.

Pada proyek ini, custom skill **`datasight-copilot`** bertindak sebagai *orchestrator and decision controller* dengan 4 pilar utama:

### 1. Persona & Standar Nasional Pendidikan (OBE & SN-Dikti)
Menetapkan Bob sebagai **Principal Academic Decision Support Auditor**. Bob memahami terminologi evaluasi kurikulum perguruan tinggi di Indonesia:
- **Standar Nasional Dikti (Permendikbudristek No. 53/2023)** untuk tata kelola mutu kurikulum.
- **Outcome-Based Education (OBE)** untuk ketercapaian CPL/CPMK.
- **Indikator Kinerja Utama (IKU 7 Kemendikbudristek)** yang mewajibkan $\ge 50\%$ mata kuliah menerapkan metode evaluasi *Case Method* atau *Team-Based Project*.

### 2. Strict Input Gate (Responsible AI & Anti-Halusinasi)
Keputusan akademik menyangkut masa depan ribuan mahasiswa. Bob **diprogram secara ketat untuk menolak menggunakan data simulasi/dummy**.
- Jika Kaprodi menyapa tanpa melampirkan berkas, Bob mengunci alur eksekusi dan mewajibkan pengguna melampirkan data CSV performa riil.
- Bob membaca berkas CSV lokal secara aman tanpa mengunggah identitas pribadi (Zero-PII).

### 3. Orkestrasi Tool MCP (`datasight_academic_decision_su`)
Skill memandu Bob untuk:
- Mengekstrak parameter ambang batas kritis (default `5.0%` s/d `30.0%`).
- Memanggil tool MCP Langflow dengan meneruskan payload data CSV aktual.
- Menangkap respons analitik terstruktur dan memformatnya menjadi *Executive Action Table*.

### 4. Human-in-the-Loop (HITL) Decision Cards & Tactical Playbooks
AI tidak mengambil alih wewenang Kaprodi secara otonom. Skill ini menginstruksikan Bob untuk menyajikan **3 Kartu Skenario Keputusan Box-Drawing**:
- **[Opsi A]** Investigasi Silabus Materi Fondasi ➔ Menurunkan *4-Week Syllabus Realignment Blueprint*.
- **[Opsi B]** Audit Metode Asesmen OBE & IKU 7 ➔ Menurunkan *Assessment Weighting Matrix Rebalance*.
- **[Opsi C]** Klinik Asistensi Mahasiswa D/E ➔ Menurunkan *Peer-Tutoring & Remedial Framework*.

---

## 📂 Struktur Berkas di Direktori Ini

```text
bob/
├── README.md                      # Dokumentasi ini
├── mcp_tool.json                  # Spesifikasi schema tool Model Context Protocol (MCP)
├── prompt.txt                     # System prompt fallback untuk sesi obrolan
└── skills/
    └── datasight-copilot/
        └── SKILL.md               # Definisi resmi skill IBM Bob (YAML Frontmatter + Rules)
```

---

## 🚀 Cara Memasang Custom Skill ke IBM Bob

1. Buka workspace IBM Bob IDE Anda.
2. Salin folder `skills/datasight-copilot/` ke dalam folder konfigurasi skill Bob Anda:
   - Path lokal workspace: `.bob/skills/datasight-copilot/`  
   - Atau path global Bob: `~/.bob/skills/datasight-copilot/`
3. Pastikan file `SKILL.md` terbaca di menu **Settings ➔ Skills** pada IBM Bob IDE.
4. Buka jendela chat Bob dan ketik:
   ```
   Halo Bob, aktifkan DataSight Copilot. Tolong jalankan audit kurikulum semester ini.
   ```
5. IBM Bob akan langsung merespons dengan persona formal dan mengaktifkan *Strict Input Gate* untuk meminta berkas CSV kurikulum Anda.
