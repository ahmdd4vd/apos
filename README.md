# APOS

[![skills.sh](https://skills.sh/b/ahmdd4vd/apos)](https://skills.sh/ahmdd4vd/apos)

**APOS (AI Project Operating System)** adalah skill governance untuk membantu coding agent menjaga kesinambungan proyek software. APOS membuat pekerjaan agent lebih terarah dengan memelihara konteks proyek, requirements, arsitektur, keputusan teknis, task ownership, changelog, dan pengetahuan yang perlu bertahan lintas sesi.

> APOS adalah lapisan panduan dan sinkronisasi proyek. APOS tidak menggantikan coding agent dan tidak membuat perubahan irreversible tanpa otorisasi yang sesuai.

## Fitur utama

- Membaca state repository sebelum pekerjaan non-trivial dimulai.
- Mengklasifikasikan perubahan berdasarkan risiko: trivial, routine, significant, atau critical.
- Menjaga keterkaitan antara goals, requirements, tasks, architecture, decisions, changelog, dan memory.
- Menggunakan workflow proporsional: `READ → ANALYZE → PLAN → EXECUTE → VALIDATE → UPDATE STATE → REPORT`.
- Mencegah keputusan arsitektur dan dokumentasi diam-diam menyimpang dari implementasi tervalidasi.
- Mendukung audit kesehatan proyek dan pelaporan drift berdasarkan severity.
- Menyediakan format laporan akhir yang ringkas dan dapat ditindaklanjuti.

## Instalasi melalui skills.sh

Instal CLI `skills` secara langsung dengan `npx`, lalu tambahkan skill APOS dari repository ini:

```bash
npx skills add ahmdd4vd/apos --skill apos
```

Perintah tersebut memasang skill untuk project saat ini. Untuk memasangnya secara global agar tersedia di berbagai project, tambahkan flag `-g`:

```bash
npx skills add ahmdd4vd/apos --skill apos -g
```

Untuk memilih agent tertentu, gunakan opsi `--agent`. Contoh untuk Claude Code:

```bash
npx skills add ahmdd4vd/apos --skill apos --agent claude-code
```

Untuk melihat skill yang tersedia tanpa memasang:

```bash
npx skills add ahmdd4vd/apos --list
```

Untuk instalasi non-interaktif:

```bash
npx skills add ahmdd4vd/apos --skill apos -y
```

Dokumentasi lengkap tersedia di [skills.sh/docs](https://www.skills.sh/docs).

## Cara penggunaan

Setelah terpasang, agent akan memuat APOS ketika tugas berkaitan dengan governance dan kesinambungan proyek, misalnya:

- merencanakan atau mengimplementasikan perubahan software;
- menyelaraskan dokumentasi dengan code yang tervalidasi;
- melacak task, ownership, dan keputusan teknis;
- mengaudit project drift;
- mengoordinasikan beberapa agent atau worktree.

Untuk pemakaian manual, sebutkan APOS atau minta agent mengikuti workflow APOS pada prompt. Skill ini akan mengarahkan agent untuk membaca state aktual, memilih proses yang sesuai risiko, memvalidasi hasil, memperbarui artifact yang terdampak, dan membuat laporan akhir.

## Struktur project yang didukung

Jika diperlukan, APOS menggunakan direktori `.apos/` di root repository:

```text
.apos/
├── goals/
├── prd/
├── architecture/
├── decisions/
├── tasks/
├── changelog/
└── memory/
```

Direktori tambahan seperti `worktrees/`, `agents/`, dan `reports/` hanya dibuat ketika benar-benar dibutuhkan. APOS tidak menganjurkan pembuatan struktur administrasi kosong.

## Prinsip desain

1. Lindungi intent proyek dan realitas implementasi.
2. Gunakan proses terkecil yang tetap aman.
3. Jadikan perubahan material dapat ditelusuri.
4. Pertahankan keputusan penting.
5. Jelaskan ownership dan batas tanggung jawab.
6. Perlakukan perubahan arsitektur sebagai perubahan berisiko.
7. Buat pengetahuan penting bertahan lintas sesi.
8. Hindari duplikasi sumber kebenaran.
9. Utamakan konsistensi tanpa birokrasi yang tidak perlu.
10. Tinggalkan proyek dalam kondisi yang mudah dipahami developer baru.

## File

- [`SKILL.md`](./SKILL.md) — instruksi utama yang dibaca oleh coding agent.
- [`plan.md`](./plan.md) — roadmap pengembangan APOS Beta.

## Status

APOS saat ini berada pada tahap **Beta / governance skill**. Repository ini berisi skill yang dapat dipasang melalui skills.sh; implementasi API, console, CLI, MCP adapter, dan database APOS mengikuti roadmap di `plan.md`.

## Kontribusi

Sebelum mengubah `SKILL.md`, pastikan instruksi tetap singkat, dapat diterapkan lintas repository, dan tidak menduplikasi dokumentasi yang tidak diperlukan agent. Untuk perubahan workflow yang material, perbarui `plan.md` atau catat keputusan teknis yang relevan.

## Lisensi

Lisensi belum ditetapkan di repository ini.
