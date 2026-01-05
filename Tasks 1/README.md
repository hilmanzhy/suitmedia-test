# Tasks 1

Ceritakan pengalaman paling berkesan saat membangun arsitektur sistem yang highly scalable. Bahas secara ringkas:
- Objective arsitektur dan strategi untuk mencapainya.
- Kendala yang muncul saat mengejar high availability beserta solusi yang dipakai.
- Hasil kuantitatif dibanding KPI atau benchmark industri.

## Jawaban

- Objective: membangun platform HA dengan konsep serverless dan microservices dari scratch. Saya mulai dengan mendesain topologi (VPC, subnet privat, NAT, LB, Nodes, Pods, Cluster) lalu seluruh resource dibuat via Terraform agar konsisten dan bisa di-review.

- Strategi: workload utama di GKE (User Managed) dengan Helm chart untuk standardisasi service (HPA, PDB, Ingress, Services Account, etc). Deployment strategy menggunakan rolling update, dibungkus pipeline Cloud Build (trigger via github) yang menjalankan test, build image, push ke registry, lalu deploy dari helm chart yang telah dibungkus menjadi OCI (helm upgrade --install). Credentials disimpan di Secret Manager dan di inject ke pod dengan Workload Identity. Database pakai CloudSQL PostgreSQL HA, akses melalui connector/proxy dan VPC peering.

- Kendala & solusi: batas koneksi PostgreSQL diatasi dengan connection pooling di sisi aplikasi/proxy; perubahan skema saya pisahkan ke job migrasi sebelum helm release. Variasi beban ditangani autoscaling (HPA + cluster autoscaler), sementara PDB dan readiness probe menjaga rolling update zero downtime. Audit/logging terintegrasi di Cloud Build dan GKE untuk monitoring changes.

- Hasil: rilis jadi otomatis end-to-end tanpa manual SSH, deploy rolling berjalan zero downtime, dan costs tetap managed karena autoscaling (pengalaman serupa di GCP sebelumnya menekan biaya up to 40%). Infrastruktur dapat direplikasi lintas environment karena seluruhnya didefinisikan sebagai kode.
