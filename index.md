---
layout: lesson
root: .
---

Linux/Unix shell banyak digunakan karena kemampuannya yang **powerful**. Mampu menggunakan perintah dasar pada shell Linux merupakan suatu skill dasar yang harus dimiliki bagi mereka yang ingin menjadi programmer, sysAdmin dan, bahkan, (data) scientist. Shell merupakan jendela ke bahasa pemrograman lainnya seperti python, perl, awk, c/c++. Menguasai shell artinya anda bisa memadukan antar bahasa pemrograman karena shell saat ini dijadikan glue (perekat) antar bahasa pemrograman yang saat ini tidak bisa berdiri sendiri-sendiri. Contoh nyata penggunaan shell adalah pada riset speech synthesis, speech recognition dan data science. Dan hampir semua bidang yang menggunakan komputasi memakai shell pada systemnya. Shell pada Linux dapat digunakan melalui (gnome) terminal, buka dengan Ctrl + Alt + T.


***Kenapa memakai shell...?***  
Ada banyak alasan, diantaranya adalah sbb:  
1. **Kecepatan**. Jika anda sudah terbiasa, dan *expert*, menggunakan shell, anda akan lebih banyak menghemat klik dan menghemat waktu karena akses shell lebih cepat.
2. **Fleksibilitas (keleluasaan)**. Dengan shell, kita bisa memberi argumen pada suatu program, misalnya kita ingin membuka program tanpa plugin, membuka program tanpa menggunkan Java, dll.
3. **Otomasi**. Shell scripting banyak digunakan untuk meng-otomasi pekerjaan yang berulang-ulang, yang akan sangat menyita waktu kalau kita mengerjakan satu-satu. Misal, me-rename 100 file dalam satu folder, memodifikasi ukuran 1000 file, dst.
4. **Program-program tertentu hanya bisa diakses lewat shell**. Ya, hanya lewat shell, misal: ssh, rsync, curl, setting proxy, kompilasi, instalasi (./configure, make, make install), dll. Memang telah dibuat versi GUI dari beberapa program tsb,tapi tidak semua.


> ## Persyaratan
>
> Tutorial ini akan memandu anda mempelajari dasar file system dan Linux shell.
> Jika anda telah bisa menggunakan komputer, menyimpan, membuat folder atau direktori (nama yang lebih umum di Linux)
> maka tutorial ini untuk anda.
> 
> Anda akan membutuhkan sebuah mesin (komputer) berbasis Linux dimana shell yang digunakan adalah Bash shell.
> Anda bisa saja menggunakan jenis shell yang lain: csh, zsh atau ksh, namun tidak
> bisa dipastikan material pada tutorial ini akan berjalan pada shell lain selain bash tersebut.
>
> Jika anda sudah bisa memanipulasi file dan direktori seperti menggunakan perintah `grep` dan `find`
> atau menulis loop sederhana dalam bash, maka tutorial ini tidak ditunjukkan untuk anda,
> silahkan lanjut ke tutorial `shell-ekstra`.
>
{: .prereq}

### Metode pembelajaran 
Metode pembalajan tutorial/workshop ini adalah sbb: 
1. **Berilmu sebelum beramal**, artinya anda harus memahami sebelum mempraktekkannya.
2. **Learning by coding**, anda harus mempraktekkan (coding) apa yang disampaikan tutor, *no passive acitivy*.
3. **Synchronizing typing** (*khusus workshop*), apa yang anda ketik harus sama dan menghasilkan output yang sama dengan yang ditunjukkan pada tutorial ini atau didemokan tutor/lecturer (synchronized). 
