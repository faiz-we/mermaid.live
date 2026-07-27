sequenceDiagram
participant mahasiswa
participant website
participant database
participant dosen

mahasiswa->>website:login
website->>database:cek data
alt cek role
website->>dosen:dashboard dosen
else jika mahasiswa
website->>mahasiswa:dashboard mahasiswa
end
mahasiswa->>website:lihat mata kuliah
website->>database:minta data mata pelajaran
database->>website:mengirim data mata pelajaran
website->>mahasiswa:menampilkan mata pelajaran
mahasiswa->>website:memilih mata pelajaran
website->>database:cek kuota
alt jika kuota penuh
database->>website:data kuota
website->>mahasiswa:menolak pengambilan
else jika masih tersedia
website->>database:menyimpan krs
dosen->>mahasiswa:menerima daftar mahasiswa
mahasiswa->>dosen:mahasiswa mendapat konfirmasi
website->>database:menyimpan kuota
database->>database:menambah kuota
end