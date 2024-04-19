# #!/usr/bin/env Sebagai Shebang

Tanda #! disebut sebagai shebang. Ini terdiri dari tanda pagar dan karakter tanda seru (#!), diikuti dengan jalur lengkap ke interpreter seperti /bin/bash. Semua skrip di bawah Linux, *BSD, macOS, dan sistem mirip Unix lainnya dieksekusi menggunakan interpreter yang ditentukan pada baris pertama. Namun, ada masalah kecil. BASH atau Perl tidak selalu berada di lokasi yang sama (PATH) seperti /bin/bash atau /usr/bin/perl. Jika Anda ingin memastikan bahwa skrip tersebut dapat dipindahkan ke berbagai sistem operasi mirip UNIX yang berbeda, Anda perlu menggunakan perintah /usr/bin/env sebagai shebang.
Membuat Skrip Linux/Unix Portabel Dengan #!/usr/bin/env Sebagai Shebang

Gambar.01: Skrip Perl saya dimulai dengan /usr/bin/env
Membuat Skrip Linux/Unix Portabel Dengan #!/usr/bin/env Sebagai Shebang

Perintah env memungkinkan untuk menjalankan program dalam lingkungan yang dimodifikasi. Pertama, temukan baris

    #!/bin/bash

Ganti dengan
    
    #!/usr/bin/env bash

Sebagai contoh, berikut adalah skrip shell bash kecil:

```
#!/usr/bin/env bash
x=5
y=10
echo "$x dan $y"
```

Keuntungan dari #!/usr/bin/env bash adalah bahwa ini akan menggunakan eksekutor bash apa pun yang muncul pertama dalam variabel $PATH pengguna yang sedang berjalan. Jika Anda memiliki dua versi bash terpasang seperti berikut dan PATH disetel ke /home/vivek/bin:/usr/local/bin:/usr/bin:/bin:/usr/games/bin:/bin:/usr/bin, maka bash4 akan mengeksekusi skrip:

    /bin/bash # <-- bash3
    /usr/local/bin/bash # <-- bash4

Menggunakan env dalam shebang suatu skrip

env adalah perintah shell untuk sistem operasi mirip Unix. Kita dapat menggunakannya untuk berbagai tujuan. Sebagai contoh:

 -  Menampilkan daftar variabel lingkungan
 -  Menjalankan perintah lain dalam lingkungan yang dimodifikasi
 -  Menambahkan atau menghapus variabel
 -  Mengubah nilai variabel yang ada

Untuk menampilkan variabel lingkungan saat ini yang ditentukan oleh shell Anda, jalankan perintah env:
env

Kita dapat menyaring informasi menggunakan perintah grep/egrep sebagai berikut:

    env | grep HOME
    env | grep -E 'HOME|USER|VERSION|SHELL|PWD'

Output:

    SHELL=/bin/bash
    GNOME_SHELL_SESSION_MODE=ubuntu
    PWD=/tmp
    HOME=/home/vivek
    USERNAME=vivek
    VTE_VERSION=6003
    USER=vivek
    OLDPWD=/tmp

Mari kita lihat beberapa contoh lagi.
Contoh Perl

```shell
#!/usr/bin/env perl
use warnings;
print "Halo " x 5;
print "\\n";
```

Contoh Python2.x:

```
#!/usr/bin/env python
x=10
y=20
z=x+y
print z
```

Contoh lain dengan python3:

```
#!/usr/bin/env python3
import boto3
t = boto3.resource('s3')
for b in t.buckets.all():
     print(b.name)
```

Apakah `env` selalu berada di `/usr/bin/env`

Anda dapat menggunakan perintah type atau perintah command untuk menemukan jalur yang tepat untuk perintah env:

    type env
    command -V env

Output sampel:

    env is /usr/bin/env

Beberapa info dari berbagai sistem *nix:


| OS 	| env path |  
|---|---|
OpenBSD 	| /usr/bin/env |   
FreeBSD 	| /usr/bin/env
Debian 	| /usr/bin/env
Ubuntu 	| /usr/bin/env
CentOS 	| /usr/bin/env
macOS 	| /usr/bin/env
SUSE 	| /usr/bin/env
RHEL 	| /usr/bin/env
NetBSD 	| /usr/bin/env
Solaris 	| /usr/bin/env


## Menggunakan alat shellcheck

ShellCheck adalah alat analisis statis untuk skrip shell. Anda dapat menggunakannya untuk menemukan bug pada skrip shell Anda. Ini ditulis dalam Haskell. Anda dapat menemukan peringatan dan saran untuk skrip shell bash/sh dengan alat ini. Mari kita lihat bagaimana cara menginstal dan menggunakan ShellCheck pada sistem Linux atau mirip Unix untuk meningkatkan skrip shell Anda, menghindari kesalahan, dan meningkatkan produktivitas.
Kesimpulan

Saya baru saja membuat skrip Linux/Unix saya menjadi portabel dengan #!/usr/bin/env. Mekanisme env sangat meningkatkan kenyamanan, dan hampir semua sistem Unix saat ini menyediakan /usr/bin/env secara default. Saya sering menggunakan FreeBSD, CentOS/RHEL/Ubuntu/Debian/SUSE Linux, macOS, dan OpenBSD dan semuanya menyediakan /usr/bin/env secara default. Sekarang Anda tidak perlu mencari program melalui variabel lingkungan PATH. Ini membuat skrip lebih portabel. Selain itu, perhatikan bahwa ini bukan metode yang sangat andal. Selalu pastikan Anda memiliki /usr/bin/env yang ada atau gunakan tautan simbolik untuk menunjukkannya ke jalur yang benar untuk perintah env. Selain itu, pekerjaan atau skrip Anda terlihat lebih profesional dengan trik sederhana ini. Lihat halaman man perintah env (dokumentasi) atau ketik perintah man/info/help berikut:

    man bash
    info bash
    man env
