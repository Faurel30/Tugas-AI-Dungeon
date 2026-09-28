# Tugas-AI-Dungeon
Tugas Algoritma Pathfinding dan AI Musuh di Dungeon.

# Algoritma Deteksi & Pathfinding Musuh (Enemy AI) di Dungeon

## 1. Identifikasi Algoritma yang Digunakan

Untuk mengimplementasikan perilaku musuh yang mendeteksi player, mengecek jangkauan, mencari jalur, dan bergerak menuju player, digunakan kombinasi beberapa algoritma berikut:

* **Deteksi Player & Jangkauan (Detection & Range Checking):**
  * **Euclidean Distance:** Memperhitungkan jarak antara posisi Musuh dan Player untuk menentukan apakah player berada dalam jangkauan deteksi atau area serang.
  * **Raycasting / Line of Sight (LoS):** Menembakkan sinar imajiner dari musuh ke arah player untuk mengecek apakah ada rintangan/tembok dungeon yang menghalangi pandangan musuh.
* **Pencarian Jalur (Pathfinding):**
  * **Algoritma A\* (A-Star):** Algoritma pathfinding paling optimal untuk grid/navigasi dungeon. Algoritma ini memadukan biaya langkah ($g(n)$) dan estimasi jarak ke tujuan ($h(n)$ / heuristic) untuk menemukan rute terpendek dengan efisien tanpa menabrak tembok.
* **Pergerakan Musuh (Movement Steering):**
  * **Vector Movement / Steering Behaviors (misal: NavMeshAgent):** Memperbarui posisi musuh langkah demi langkah mengikuti titik-titik jalur (*waypoints*) hasil dari pathfinding.

---

## 2. Flowchart Algoritma

mermaid
flowchart TD
    Start([Mulai Frame / Update Tick]) --> GetPos[Ambil Posisi Enemy & Player]
    GetPos --> CalcDist[Hitung Jarak Euclidean ke Player]
    
    CalcDist --> CheckDist{Jarak <= Jangkauan Deteksi?}
    
    CheckDist -- Tidak --> Idle[Musuh Tetap Diam / Patroli] --> End([Selesai Tick])
    CheckDist -- Ya --> CheckLoS{Line of Sight Clear?<br>Tembok Menghalangi?}
    
    CheckLoS -- Ya / Terhalang --> Pathfinding[Jalankan Algoritma A* Pathfinding]
    CheckLoS -- Tidak / Langsung --> DirectMove[Tentukan Jalur Langsung ke Player]
    
    Pathfinding --> GenPath[Hasilkan Daftar Waypoints / Jalur Terpendek]
    DirectMove --> GenPath
    
    GenPath --> MoveEnemy[Bergerak 1 Langkah ke Waypoint Berikutnya]
    
    MoveEnemy --> CheckAttack{Apakah Player dalam Jangkauan Serang?}
    CheckAttack -- Ya --> Attack[Lakukan Serangan / Attack State]
    CheckAttack -- Tidak --> End
    Attack --> End
