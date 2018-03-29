---
title: "Bekerja dengan File dan Direktori"
teaching: 25
exercise: 15
questions:
- "Bagaimana membuat, menyalin dan menghapus file dan direktori?"
- "Bagaimana mengedit file?"
objectives:
- "Membuat hierarki direktori yang match dengan pola yang diberikan"
- "Membuat file dalam direktori dengan editor dengan mengcopy dan merename file yang ada."
- "Menampilkan daftar dan konten dari direktori dengan menggunakan perintah shell."
- "Menghapus file dan/atau direktori tertentu."
keypoints:
- "`cp lama baru` mengcopy file *lama* ke *baru*."
- "`mkdir path` membuat direktori baru."
- "`mv old new` me-rename/memindah file atau direktori."
- "`rm path` menghapus file atau direktori."
- " `rmdir direktorikosong` menghapus direktori kosong."
- " `touch namafile.txt` membuat dan mengedit file baru namafile.txt"
- "Shell tidak mempunyai trash bin (recycle bin): artinya sekali dihapus hilang selamanya."

---

Dengan perintah `ls` dan `cd` kita bisa mengeksplorasi isi dari sebuah direktori.
Namun bagaimana cara membuat direktori tersebut
Kembali pada direktori `data-shell` pada `/home` dan gunakan `ls -F` untuk melihat isinya:

~~~
$ pwd
~~~
{: .bash}

~~~
/home/bagustris/data-shell
~~~
{: .output}

~~~
$ ls -F
~~~
{: .bash}

~~~
creatures/  molecules/           pizza.cfg
data/       north-pacific-gyre/  solar.pdf
Desktop/    notes.txt            writing/
~~~
{: .output}

Mari kita buat sebuah direktori yang bernama `thesis` dengan menggunakan perintah `mkdir thesis`
(yang tidak memiliki output):

~~~
$ mkdir thesis
~~~
{: .bash}

Anda mungkin bisa menebak apa kegunaan `mkdir` dari namanya,
`mkdir` merupakan "make directory" : membuat direktori.
Karena `thesis` merupakan relative path
(yakni tidak dipisahkan oleh slash),
maka direktori baru bermana `thesis` dibuat dalam direktori saat ini.:

~~~
$ ls -F
~~~
{: .bash}

~~~
creatures/  north-pacific-gyre/  thesis/
data/       notes.txt            writing/
Desktop/    pizza.cfg
molecules/  solar.pdf
~~~
{: .output}

> ## Two ways of doing the same thing
> 
> Menggunakan shell untuk membuat direktori tidak adanya ketika anda meng-klik kanan pada file explorer (nautilus).
> Jika anda membuka file explorer maka akan ada direktori `thesis` disana.
> Meskipun ada dua cara berbeda membuat file dan direktori (GUI file explorer dan CLI terminal),
> kedua direktori ataupun file tersebut sama saja.
{: .callout}

> ## Penamaan file dan direktori
>
> Nama file dan direktori yang tidak jelas akan mengakibatkan kita kesulitan 
> ketika bekerja dengan command line. Beberapa tips berikut sangat berguna
> untuk menamai file dan direktori:
>
> 1. Jangan menggunakan spasi
>
>    Menggunakan spasi mungkin akan terlihat bermakna,
>    namun karena spasi digunakan untuk memisahkan argumen pada command line
>    akan sangat berguna jika kita tidak menggunakannya dalam menamai file.
>    Sebagai gantinya, kita bisa menggunakan `-` atau `_`.
>    Nama file/direktori dengan spasi tetap bisa dipanggil dalam perintah Linux
>    dengan menggunakan tanda petik (" ").
>
> 2. Jangan mengawali nama file/direktori dengan `-` (dash).
>
>    Perintah Unix/Linux menggunakan tanda dash untuk menandai option 
>    Contohnya: `ls -F`
>
> 3. Hanya gunakan huruf, angka, titik, dash (`-`) dan underscore (`_`).
>    
>    Beberapa karakter memiliki makna dalam perintah Linux/Unix,
>    jadi sebisa mungkin dihindari untuk menggunakan karakter (+,*, &, %, *, dst)
>    dalam menamai file atau direktori. 
>    Jika ada karakter dalan nama file atau direktori, bisa jadi
>    perintah yang kita gunakan akan error atau
>    disalah artikan oleh komputer.
>    
>
> {: .callout}

Kita sudah membuat direktori `thesis`, namun belum ada isinya.

~~~
$ ls -F thesis
~~~
{: .bash}

Kemudian, pindah ke direktori `thesis` dengan perintah `cd`,
jalankan editor nano untu membuat file yang dinamakan `draft.txt`:

~~~
$ cd thesis
$ nano draft.txt
~~~
{: .bash}

> ## Which Editor?
>
> Berbicara tentang editor, ada dua mazhab besar: VIM dan Emacs, namun keduanya
> terlalu sulit untuk pemula. Sebagai pemula, saya sarankan anda untuk
> menggunakan Nano atau Gedit. Gedit cukup mudah dipelajari dan difahami, tinggal
> buka file yang akan diedit, edit, dan save. Untuk Nano, anda perlu mengetahui 
> shortcut untuk menyimpan dll. 
{: .callout}

Silahkan ketik beberapa baris teks. Kalau anda sudah puas
dengan apa yang anda ketik, tekan `Ctrl-O` (tekan tombol 
Control dan O bersamaan) untuk menyimpan teks tersebut ke
dalam file `draft.txt`.


![Nano in Action](../fig/nano-screenshot.png)

Gunakan `Ctrl-X` untuk keluar dari nano, dan ketika ada pertanyaan
apakah ingin menyimpannya, tekan `y` kemudian `Enter`.

> ## Control, Ctrl, or ^ Key
>
> Tombol Control sering disingkat dengan "Ctrl" sebagaimana tertulis 
> pada keyboard. Ada beberapa cara penulisan dimana
> kita diminta menekan tombol Control dan, sambil menahannya, kita tekan
> tombol lainnya, yakni tombol X. Berikut beberapa versi penulisannya.
>
> * `Control-X`
> * `Control+X`
> * `Ctrl-X`
> * `Ctrl+X`
> * `^X`
> * `C-x`
>
> Pada nano, pada bagian bawah anda akan melihat tulisan
> `^G Get Help ^O WriteOut` dst. Artinya tekan `Control-G` 
> untuk menampikan help dan `Control-O` untuk menyimpan file.
{: .callout}

`nano` tidak akan menampilkan output lagi, namun teks yang tadi
diketik sudah disimpan dalam file `draft.txt`, cek dengan `ls`.

~~~
$ ls
~~~
{: .bash}

~~~
draft.txt
~~~
{: .output}

Let's tidy up by running `rm draft.txt`:

~~~
$ rm draft.txt
~~~
{: .bash}

Perintah ini akan menghapus file (`rm` merupakan kependekran dari "remove").
Jika kita run ls sekali lagi, file draft tersebut sudah tidak ada.

~~~
$ ls
~~~
{: .bash}

Setelah anda bisa menggunanan nano, sangat saya sarankan 
untuk segera mempelajari **VIM**, text editor dengan kemampuan tercepat,
secepat apa kecepatan jari anda.

> ## Deleting Is Forever
> 
> Shell Linux/Unix tidak memiliki tempat sampah (trash bin) untuk me-recover
> file yang telah kita hapus. Ini artinya, apa yang telah kita hapus melalui
> perintah di terminal akan hilang selamanya. Berbeda dengan ketika kita
> menghapus dengan GUI pada file explorer (nautilus). Pada file explorer,
> ketika kita menghapus sebenarnya kita hanya memindahkan file/direktori tersebut
> ke direktori tempat sampah (Move to trash).
> Ada beberapa tools yang bisa digunakan untuk merecover file yang telah
> kita hapus seperti `testdisk` namun untuk pemula, tools tersebut tidak
> menjamin apa yang telah kita hapus bisa kembali.
{: .callout}

Mari kita buat lagi file tersebut dan memindahkannya ke direktori atasnya,
yakni ke direktori `/home/bagustris/data-shell` dengan perintah `cd ..`:

~~~
$ pwd
~~~
{: .bash}

~~~
/home/bagustris/data-shell/thesis
~~~
{: .output}

~~~
$ nano draft.txt
$ ls
~~~
{: .bash}

~~~
draft.txt
~~~
{: .output}

~~~
$ cd ..
~~~
{: .bash}

Jika kita coba untuk menghapus direktori `thesis` dengan `rm thesis`,
maka akan terjadi error:

~~~
$ rm thesis
~~~
{: .bash}

~~~
rm: cannot remove `thesis`: Is a directory
~~~
{: .error}

Ini terjadi karena, _by default_, perintah `rm` hanya bekerja pada file bukan direktori.
Kita gunakan `rmdir` untuk menghapus direktori.

~~~
$ rmdir thesis
rmdir: failed to remove 'thesis/': Directory not empty
~~~
{: .error}

Ini terjadi karena `rmdir` digunakan **hanya** untuk menghapus direktori kosong. Hal ini masuk akal,
karena `rm` menghapus selamanya, jadi Linux memastikan bahwa direktori yang dihapus kosong.

Untuk menghapus direktori `thesis` yang berisi file `draft.txt`, dimana artinya
kita juga ikut menghapus file di dalam direktori tersebut, maka kita gunakan argumen 
[recursive](https://en.wikipedia.org/wiki/Recursion) yakni `-r` untuk `rm`:

~~~
$ rm -r thesis
~~~
{: .bash}

> ## With Great Power Comes Great Responsibility
>
> Menghapus file-file dalam direktori secara rekursif bisa jadi 
> sangat berbahaya. Jika kita ingin meyakinkan diri tentang apa yang akan dihapus
> kita bisa menambahkan flah `-i` yang merupakan singkatan dari "interactive"
> pada perintah rm sehingga menjadi `rm -i file_yang_akan_dihapus`.
>
>
> ~~~
> $ rm -r -i thesis
> rm: descend into directory ‘thesis’? y
> rm: remove regular file ‘thesis/draft.txt’? y
> rm: remove directory ‘thesis’? y
> ~~~
> {: .bash}
>
> Ini akan menghapus file di dalam direktori, kemudian direktori 
> itu sendiri. Jika anda yakin ingin menghapusnya, tekan "y", jika tidak
> ingin menghapusnya tekan "n".
{: .callout}

Mari kita buat direktori dan file tersebut sekali lagi.
(file draft kita edit dari direktori saat ini alih-alih berpindah ke direktori `thesis`.)

~~~
$ pwd
~~~
{: .bash}

~~~
/home/bagustris/data-shell
~~~
{: .output}

~~~
$ mkdir thesis
$ nano thesis/draft.txt
$ ls thesis
~~~
{: .bash}

~~~
draft.txt
~~~
{: .output}

## MV
Nama file `draft.txt` sangat tidak informatif, bisa berarti draft apa saja, ya kan..?
Baiklah, kita ganti namanya menjadi lebih spesifik dengan `mv`, kepanjangan dari "move".

~~~
$ mv thesis/draft.txt thesis/quotes.txt
~~~
{: .bash}

`mv` memiliki dua parameter/argumen, parameter pertama adalah file yang akan dipindah/nama-lama 
parameter kedua adalah tujuan/nama_baru. Jadi kita mengganti nama file dari `draft.txt` menjadi
`quotes.txt` dalam folder `thesis`.
Cek dengan ls untuk membuktikannya.

~~~
$ ls thesis
~~~
{: .bash}

~~~
quotes.txt
~~~
{: .output}

Jika kita ingin memindah/merename file `draft.txt` tidak didalam direktori `thesis`
namun di direktori lain, misal direktori saat ini, maka kita mengganti parameter tujuan 
menjadi, misalnya, `./quotes`.

Seperti halnya `rm`, perintah `mv` juga bisa dijalankan secara interaktif dengan flag `-i`, 
atau `--interactive`.

Perintah `mv` ini bisa dijakankan baik untuk file maupun direktori.

Sekarang pindahkan file quotes.txt dalam direktori thesis ke direktori saat ini,

~~~
$ mv thesis/quotes.txt .
~~~
{: .bash}

Cek dengan ls untuk melihat hasilnya

~~~
$ ls thesis
$ ls
creatures  Desktop    north-pacific-gyre  pizza.cfg   solar.pdf  writing
data       molecules  notes.txt           quotes.txt  thesis
~~~
{: .bash}

Lebih jauh, perintah `ls` disertai nama file atau direktori akan memberikan output
nama file/direktori tersebut jika ada.

~~~
$ ls quotes.txt
~~~
{: .bash}

~~~
quotes.txt
~~~
{: .output}

## CP
Perintah `cp` bekerja mirip dengan `mv`, bedanya jika `mv` dapat kita artikan cut-paste 
maka `cp` berarti copy atau copy-paste. Artinya, akan ada dua file atau direktori, file/direktori lama
dan file/direktori baru. Sekali lagi cek dengan `ls` untuk melihat hasilnya

  Perintah: `cp file nama-baru` bisa juga `cp lokasi/file lokasi-baru/nama-baru`.  
Berikut contohnya.

~~~
$ cp quotes.txt thesis/quotations.txt
$ ls quotes.txt thesis/quotations.txt
~~~
{: .bash}

~~~
quotes.txt   thesis/quotations.txt
~~~
{: .output}

Untuk membuktikan bahwa kita telah mengcopy file quotes menjadi quotations.txt (bukan symbolic link)
silahkan hapus file `quotes.txt` dan jalankan `ls` lagi.

~~~
$ rm quotes.txt
$ ls quotes.txt thesis/quotations.txt
~~~
{: .bash}

~~~
ls: cannot access quotes.txt: No such file or directory
thesis/quotations.txt
~~~
{: .error}

File `quotes.txt` tidak ditemukan dalam direktori saat ini karena telah dihapus, 
namun file `quotations.txt` tetap ada pada direktori `thesis`.


> ## Ada apa dibalik nama file?
>
> Penamaan file, seperti telah disinggu sebelumnya, mengikuti konvensi,
> **namafile.ekstensi**, misalnya **draft.txt**. Draft adalah nama filenya
> dan txt adalah ekstensinya. Bisa saja kita menulis nama filenya
> hanya dengan **draft** saja, namun umumnya digunakan ekstensi untuk
> mengidentifikasi jenis file tersebut.
> Ekstensi ditulis setelah tanda titik (dot). Beberapa ekstensi berikut
> adalah yang umum digunakan:
> 1. `.txt`: file teks
> 2. `.pdf`: file pdf (portable document format)
> 3. `.cfg`: file untuk konfigurasi
> 4. `.png`: file gambar png (portable network graphics)
> 5. dst
>
> Bisa saja anda merename sebuah file gambar, `image01.png` menjadi
> `image01.mp3`. Filenya akan tetap sama file gambar, bukan berubah
> menjadi file suara/lagu. Namun, ketika membuka dengan GUI file eksplorer,
> komputer akan membukanya dengan program pemutar musik, bukan
> pembuka gambar, karena ekstensinya adalah **.mp3**, bukan **.png**.
{: .callout}

## Latihan
> ## Renaming Files
>
> Suppose that you created a `.txt` file in your current directory to contain a list of the
> statistical tests you will need to do to analyze your data, and named it: `statstics.txt`
>
> After creating and saving this file you realize you misspelled the filename! You want to
> correct the mistake, which of the following commands could you use to do so?
>
> 1. `cp statstics.txt statistics.txt`
> 2. `mv statstics.txt statistics.txt`
> 3. `mv statstics.txt .`
> 4. `cp statstics.txt .`
>
> > ## Solution
> > 1. No.  While this would create a file with the correct name, the incorrectly named file still exists in the directory
> > and would need to be deleted.
> > 2. Yes, this would work to rename the file.
> > 3. No, the period(.) indicates where to move the file, but does not provide a new file name; identical file names
> > cannot be created.
> > 4. No, the period(.) indicates where to copy the file, but does not provide a new file name; identical file names
> > cannot be created.
> {: .solution}
{: .challenge}

> ## Moving and Copying
>
> What is the output of the closing `ls` command in the sequence shown below?
>
> ~~~
> $ pwd
> ~~~
> {: .bash}
> ~~~
> /Users/jamie/data
> ~~~
> {: .output}
> ~~~
> $ ls
> ~~~
> {: .bash}
> ~~~
> proteins.dat
> ~~~
> {: .output}
> ~~~
> $ mkdir recombine
> $ mv proteins.dat recombine
> $ cp recombine/proteins.dat ../proteins-saved.dat
> $ ls
> ~~~
> {: .bash}
>
> 1.   `proteins-saved.dat recombine`
> 2.   `recombine`
> 3.   `proteins.dat recombine`
> 4.   `proteins-saved.dat`
>
> > ## Solution
> > We start in the `/Users/jamie/data` directory, and create a new folder called `recombine`.
> > The second line moves (`mv`) the file `proteins.dat` to the new folder (`recombine`).
> > The third line makes a copy of the file we just moved.  The tricky part here is where the file was
> > copied to.  Recall that `..` means "go up a level", so the copied file is now in `/Users/jamie`.
> > Notice that `..` is interpreted with respect to the current working
> > directory, **not** with respect to the location of the file being copied.
> > So, the only thing that will show using ls (in `/Users/jamie/data`) is the recombine folder.
> >
> > 1. No, see explanation above.  `proteins-saved.dat` is located at `/Users/jamie`
> > 2. Yes
> > 3. No, see explanation above.  `proteins.dat` is located at `/Users/jamie/data/recombine`
> > 4. No, see explanation above.  `proteins-saved.dat` is located at `/Users/jamie`
> {: .solution}
{: .challenge}

> ## Organizing Directories and Files
>
> Jamie sedang bekerja pada sebuah project dan dia melihat bahwa file-file miliknya
> tidak teroganisir dengan baik.
>
> ~~~
> $ ls -F
> ~~~
> {: .bash}
> ~~~
> analyzed/  fructose.dat    raw/   sucrose.dat
> ~~~
> {: .output}
>
> File `fructose.dat` dan `sucrose.dat` berisi output dari analisis datanya.
> Perintah apa yang dijelaskan pada bab ini yang harus dijalankan sehingga 
> perintah berikut menghasilkan output seperti ditunjukkan?
>
>
> ~~~
> $ ls -F
> ~~~
> {: .bash}
> ~~~
> analyzed/   raw/
> ~~~
> {: .output}
> ~~~
> $ ls analyzed
> ~~~
> {: .bash}
> ~~~
> fructose.dat    sucrose.dat
> ~~~
> {: .output}
>
> > ## Solution
> > ```
> > mv *.dat analyzed
> > ```
> > {: .bash}
> > Jamie needs to move her files `fructose.dat` and `sucrose.dat` to the `analyzed` directory.
> > The shell will expand *.dat to match all .dat files in the current directory.
> > The `mv` command then moves the list of .dat files to the "analyzed" directory.
> {: .solution}
{: .challenge}

> ## Copy with Multiple Filenames
>
> For this exercise, you can test the commands in the `data-shell/data directory`.
>
> In the example below, what does `cp` do when given several filenames and a directory name?
>
> ~~~
> $ mkdir backup
> $ cp amino-acids.txt animals.txt backup/
> ~~~
> {: .bash}
>
> In the example below, what does `cp` do when given three or more file names?
>
> ~~~
> $ ls -F
> ~~~
> {: .bash}
> ~~~
> amino-acids.txt  animals.txt  backup/  elements/  morse.txt  pdb/  planets.txt  salmon.txt  sunspot.txt
> ~~~
> {: .output}
> ~~~
> $ cp amino-acids.txt animals.txt morse.txt 
> ~~~
> {: .bash}
>
> > ## Solution
> > If given more than one file name followed by a directory name (i.e. the destination directory must 
> > be the last argument), `cp` copies the files to the named directory.
> >
> > If given three file names, `cp` throws an error because it is expecting a directory
> > name as the last argument.
> >
> > ```
> > cp: target ‘morse.txt’ is not a directory
> > ```
> > {: .output}
> {: .solution}
{: .challenge}

> ## Listing Recursively and By Time
>
> The command `ls -R` lists the contents of directories recursively,
> i.e., lists their sub-directories, sub-sub-directories, and so on
> in alphabetical order at each level.
> The command `ls -t` lists things by time of last change,
> with most recently changed files or directories first.
> In what order does `ls -R -t` display things?
> > ## Solution
> > The command `ls -R -t` displays the directories recursively in 
> > alphabetical order at each level, but the files in each directory
> > are displayed chronologically.
> {: .solution}
{: .challenge}

> ## Creating Files a Different Way
>
> We have seen how to create text files using the `nano` editor.
> Now, try the following command in your home directory:
>
> ~~~
> $ cd                  # go to your home directory
> $ touch my_file.txt
> ~~~
> {: .bash}
>
> 1.  What did the touch command do?
>     When you look at your home directory using the GUI file explorer,
>     does the file show up?
>
> 2.  Use `ls -l` to inspect the files.  How large is `my_file.txt`?
>
> 3.  When might you want to create a file this way?
>
> > ## Solution
> > 1.  The touch command generates a new file called 'my_file.txt' in
> >     your home directory.  If you are in your home directory, you
> >     can observe this newly generated file by typing 'ls' at the 
> >     command line prompt.  'my_file.txt' can also be viewed in your
> >     GUI file explorer.
> >
> > 2.  When you inspect the file with 'ls -l', note that the size of
> >     'my_file.txt' is 0kb.  In other words, it contains no data.
> >     If you open 'my_file.txt' using your text editor it is blank.
> >
> > 3.  Some programs do not generate output files themselves, but
> >     instead require that empty files have already been generated.
> >     When the program is run, it searches for an existing file to
> >     populate with its output.  The touch command allows you to
> >     efficiently generate a blank text file to be used by such
> >     programs.
> {: .solution}
{: .challenge}

> ## Moving to the Current Folder
>
> After running the following commands,
> Jamie realizes that she put the files `sucrose.dat` and `maltose.dat` into the wrong folder:
>
> ~~~
> $ ls -F
> raw/ analyzed/
> $ ls -F analyzed
> fructose.dat glucose.dat maltose.dat sucrose.dat
> $ cd raw/
> ~~~
> {: .bash}
>
> Fill in the blanks to move these files to the current folder
> (i.e., the one she is currently in):
>
> ~~~
> $ mv ___/sucrose.dat  ___/maltose.dat ___
> ~~~
> {: .bash}
> > ## Solution
> > ```
> > $ mv ../analyzed/sucrose.dat ../analyzed/maltose.dat .
> > ```
> > {: .bash}
> > Recall that `..` refers to the parent directory (i.e. one above the current directory)
> > and that `.` refers to the current directory.
> {: .solution}
{: .challenge}

> ## Using `rm` Safely
>
> What happens when we type `rm -i thesis/quotations.txt`?
> Why would we want this protection when using `rm`?
>
> > ## Solution
> > ```
> > $ rm: remove regular file 'thesis/quotations.txt'?
> > ```
> > {: .bash} 
> > The -i option will prompt before every removal. 
> > The Unix shell doesn't have a trash bin, so all the files removed will disappear forever. 
> > By using the -i flag, we have the chance to check that we are deleting only the files that we want to remove.
> {: .solution}
{: .challenge}

> ## Copy a folder structure sans files
>
> You're starting a new experiment, and would like to duplicate the file
> structure from your previous experiment without the data files so you can
> add new data.
>
> Assume that the file structure is in a folder called '2016-05-18-data',
> which contains folders named 'raw' and 'processed' that contain data files.
> The goal is to copy the file structure of the `2016-05-18-data` folder
> into a folder called `2016-05-20-data` and remove the data files from
> the directory you just created.
>
> Which of the following set of commands would achieve this objective?
> What would the other commands do?
>
> ~~~
> $ cp -r 2016-05-18-data/ 2016-05-20-data/
> $ rm 2016-05-20-data/data/raw/*
> $ rm 2016-05-20-data/data/processed/*
> ~~~
> {: .bash}
> ~~~
> $ rm 2016-05-20-data/data/raw/*
> $ rm 2016-05-20-data/data/processed/*
> $ cp -r 2016-05-18-data/ 2016-5-20-data/
> ~~~
> {: .bash}
> ~~~
> $ cp -r 2016-05-18-data/ 2016-05-20-data/
> $ rm -r -i 2016-05-20-data/
> ~~~
> {: .bash}
> >
> > ## Solution
> > The first set of commands achieves this objective.
> > First we have a recursive copy of a data folder.
> > Then two `rm` commands which remove all files in the specified directories.
> > The shell expands the '*' wild card to match all files and subdirectories.
> >
> > The second set of commands have the wrong order: 
> > attempting to delete files which haven't yet been copied,
> > followed by the recursive copy command which would copy them.
> >
> > The third set of commands would achieve the objective, but in a time-consuming way:
> > the first command copies the directory recursively, but the second command deletes
> > interactively, prompting for confirmation for each file and directory.
> {: .solution}
{: .challenge}
