# Tasks 2

Ceritakan pengalaman paling berkesan terkait keamanan informasi. Sertakan:
- Masalah yang dihadapi dan dampaknya terhadap bisnis.
- Strategi untuk memitigasi risiko keamanan tersebut.

## Jawaban

- saya pernah mengalami masalah kebocoran data di database, jadi ketika saya awal join di PT Darmawan Group, kondisi lapangan sangat memperihatinkan. jadi di DAG ada 1 bisnis unit terkait
payment gateway, yang dimana transaksi harian mencapai miliaran namun ternyata sistem dan flow control yang diterapkan masih kurang, salah satunya akses database dan akses dashboard.
jadi ada beberapa engineer yang punya akses ke database full access, jadi bisa connect direct langsung ke database dan bebas untuk query apapun. sesuai best practice hal ini sangat tidak 
dianjurkan karena dapat berpotensi FRAUD SYSTEM, saya pengambil inisiatif untuk membuat services terpusat (dbclient) dan membangun environment staging. dbclient sendiri dirancang untuk engineer
bisa cek database dari server namun hanya sebatas read only, tidak bisa run query atau CRUD dan saya disable export data nya. engineer hanya bisa bebas melakukan query dan CRUD di environment staging.