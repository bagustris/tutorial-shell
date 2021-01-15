---
title: "Skrip Shell"
teaching: 30
exercises: 15
questions:
- "Bagaimana cara menyimpan dan menggunakan kembali perintah shell?"
- "Bagaimana membuat shell skrip?"
objectives:
- "Menulis skrip shell yang menjalankan sebuah atau sekumpulan perintah untuk sekumpulan file."
- "Menjalankan skrip shell dari baris perintah."
- "Menulis sebuah skrip shell yang mengoperasikan sekumpulan file yang didefinisikan oleh user pada baris perintah lainnya."
- "Membuat pipelines yang memasukkan skirp shell yang ditulis olehmu dan user lainnya."
keypoints:
- "Menyimpan perintah dalam file (disebut **skrip shell**) agar dapat di re-use."
- "Menjalankan perintah yang disimpan dalam file dengan perintah `bash filename`"
- " `$@` merefer semua parameter skripp shell."
- " `$1`, `$2`, dll merefer nilai pertama parameter, nilai kedua dst."
- "Gunakan tanda quote untuk nilai yang memiliki spasi."
---

Agar dapat menjalankan perintah yang sama berulang-ulang, 
kita bisa menggunakan fitur history (Ctrl+R) untuk menampilkan 
perintah-perintah sebelumnya yang ingin kita ulang? Namun bagaimana 
jika perintah-perintah tersebut cukup panjang dan untuk ukuran file 
yang banyak? Kita dapat menyimpan perintah-perintah tersebut dalam sebuah file, 
disebut **skrip shell**.

Sebagai contoh, mari kita kembali pada directory `molecules/` dan membuah file baru 
dengan nama `middle.sh` yang akan menjadi skrip shell. Ekstensi `.sh` menunjukkan bahwa 
file tersebut adalah file skrip shell.

~~~
$ cd molecules
$ nano middle.sh
~~~
{: .bash}

Perintah `nano middle.sh` ini akan membuka file `middle.sh` dengan text editor "nano"
(yang berjalan di dalam shell).
Jika filenya (`middle.sh`) tidak ada, makan akan dibuat file baru.
Perintah berikut kita masukkan dalam nano dan disimpan dengan nama `middle.sh`.

~~~
head -n 15 octane.pdb | tail -n 5
~~~
{: .source}

Perintah yang kita simpan dalam file `middle.sh` tersebut merupakan 
variasi dari perintah sebelumnya yang telah kita buat. 
Kita menampilkan 15 baris pertama file `octane.pdb`, 
dan dari 15 baris tersebut kita ambil 5 terakhir dengan filter `tail`.

Perlu diingat untuk menyimpan file gunakan `Ctrl-O`, 
untuk keluarkan gunakan `Ctrl-X`. Atau lebih singkatnya, gunakan 
`Ctrl-X` untuk keluarkan kemudian tekan `y` untuk menyimpannnya. 
Sekarang coba cek apakah ada file `middle.sh` dalam direktori 
`molecules` dan tampilkan isinya.

Setelah memastikan ada, jalankan dengan perintah `bash`.

~~~
$ bash middle.sh
~~~
{: .bash}

~~~
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
~~~
{: .output}

Dengan cara ini, kita bisa menyimpan perintah-perintah yang kompleks dan berulang, 
akan sangat memudahkan bila bekerja dengan banyak file dan data.

> ## Text Editor vs. Word Processor
>
> Yang dimaksud dengan text editor disini adalah nano, vim, emacs dan semisalnya, 
> bukan Libreoffice atau Microsoft Word. Untuk dua yang disebut terakhir tadi 
> kita menggunakan word processor, bukan text editor. Kenapa?
> Karena Libreoffice dan Microsoft Word tidak hanya digunakan mengedit file,
> namun program tersebut juga menyimpan format file, type, heading dan informasi 
> lainnya. Sehingga, program seperti `head` tidak bisa berjalan pada 
> format `.odt` maupun `.docx` ataupun dokumen yang diolah 
> dengan program word processor tadi. Karenanya, kita harus menggunakan 
> text editor untuk mengedit file teks dan menyimpannya dalam 
> *plain text*, baik dengan format .txt maupun yang lainnya.
{: .callout}

Bagaimana jika kita ingin memilih baris dari sebuah file tertentu?
Kita dapat melakukannya dengan mengedit file `middle.sh` untuk tiap 
file yang berbeda. Namun hal ini akan memakan waktu yang lebih banyak 
(ingat tujuan kita menggunkan shell skrip adalah efisiensi kerja).
Sebaliknya, kita juga bisa mengedit file `middle.sh` agar menjadi 
lebih portable.

~~~
$ nano middle.sh
~~~
{: .bash}

Sekarang, ganti nama file `octave.pdb` menjadi `$1` seperti berikut:

~~~
head -n 15 "$1" | tail -n 5
~~~
{: .output}

Untuk menjalankannya, kita butuh satu argumen tambahan, yakni nama file yang 
kita proses, misalnya `octave.pdb` seperti berikut.
~~~
$ bash middle.sh octane.pdb
~~~
{: .bash}

~~~
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
~~~
{: .output}

Atau file lainnya seperti `pentane.pdb`. Artinya, kode kita menjadi lebih 
portable dan bisa bekerja untuk file-file lainnya, tidak hanya satu file saja. 

~~~
$ bash middle.sh pentane.pdb
~~~
{: .bash}

~~~
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
~~~
{: .output}

Modifikasi yang kita lakukan, yakni mengganti `octave.pdb` dengan `"$1"` artinya 
mengambil nama file (atau disebut sebagai parameter) yang  akan dimasukkan pada skrip shell dimana 
`"$1"` berada.

> ## Double-Quotes Around Arguments
>
> For the same reason that we put the loop variable inside double-quotes,
> in case the filename happens to contain any spaces,
> we surround `$1` with double-quotes.
{: .callout}

Permasalahan selanjutnya, kita masih perlu mengedit `middle.sh` setiap kali 
kita mengatur banyak baris yang ingin kita tampilkan: 3, 5, atau 7 baris misalnya. 
Mari kita tambahkan variable baru yakni `$2` dan `$3` untuk jumlah baris pada perintah 
`head` dan `tail`.

~~~
$ nano middle.sh
~~~
{: .bash}

~~~
head -n "$2" "$1" | tail -n "$3"
~~~
{: .output}

Sekarang bisa kita jalankan:

~~~
$ bash middle.sh pentane.pdb 15 5
~~~
{: .bash}

~~~
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
~~~
{: .output}

Dengan mengubah argumen dari skripp shell, kita bisa mengubah perangai 
dari skrip shell yang kita buat. Lebih fleksibel dari skrip awal.

~~~
$ bash middle.sh pentane.pdb 20 5
~~~
{: .bash}

~~~
ATOM     14  H           1      -1.259   1.420   0.112  1.00  0.00
ATOM     15  H           1      -2.608  -0.407   1.130  1.00  0.00
ATOM     16  H           1      -2.540  -1.303  -0.404  1.00  0.00
ATOM     17  H           1      -3.393   0.254  -0.321  1.00  0.00
TER      18              1
~~~
{: .output}


Lagi, skrip shell ini mungkin sangat berguna bagi kita, 
namun jika ada orang lain yang melihatnya akan menjadi bingung. Kita tambahkan 
**komen** pada baris paling atas.

~~~
$ nano middle.sh
~~~
{: .bash}

~~~
# Select lines from the middle of a file.
# Usage: bash middle.sh filename end_line num_lines
head -n "$2" "$1" | tail -n "$3"
~~~
{: .output}

Sebuah komen(tar) dimulai dengan tanda hash/kres, `#`. Tanda ini memberitahu 
`bash` untuk tidak meproses teks pada baris tersebut. Komen sangat 
berguna bagi orang lain yang membaca skrip kita. Mungkin saja 
mereka bisa memperbaiki atau meningkatkan keefektifan skrip yang telah kita buta.

Bagaimana jika kita ingin memproses banyak file dalam satu pipeline?
Sebagai contoh, kita ingin mengurutkan file `.pdb` berdasarkan panjangnya.

~~~
$ wc -l *.pdb | sort -n
~~~
{: .bash}


Ingat `wc -l` adalah untuk mendaftar file berdasarkan jumlah barisnya. 
Bisakah kita menggunakan teknik seperti sebelumnya, yakni dengan 
menambahkan `$1`, `$2`, `$3`...?
Tidak bisa, karena kita tidak tahu berapa jumlah filenya.
Sebaliknya kita bisa menggunakan variabel `$@` yang artinya "Semua parameter command-line 
pada skrip shell".

Jika namafilenya ada spasinya, kita perlu menggunakan tanda double quote untuk `$@` tersebut.

Kita buat skrip shell baru sebagai berikut.

~~~
$ nano sorted.sh
~~~
{: .bash}

~~~
# Sort filenames by their length.
# Usage: bash sorted.sh one_or_more_filenames
wc -l "$@" | sort -n
~~~
{: .output}

~~~
$ bash sorted.sh *.pdb ../creatures/*.dat
~~~
{: .bash}

~~~
9 methane.pdb
12 ethane.pdb
15 propane.pdb
20 cubane.pdb
21 pentane.pdb
30 octane.pdb
163 ../creatures/basilisk.dat
163 ../creatures/unicorn.dat
~~~
{: .output}

> ## Why Isn't It Doing Anything?
>
> Apa yang terjadi ketika sebuah skrip diharapkan untuk memproses
> sejumlah file namun kita tida memberinya nama file. Contoh,
>
> ~~~
> $ bash sorted.sh
> ~~~
> {: .bash}
>
> namun tidak ada argumen setelahnya, baik itu `*.dat` ataupun yang lainnya.
> Pada kasus ini, `$@` tidak akan mencari file apapun. Sehingga, sama 
> saja yang dijalankan hanyalah apa yang ada dalam file `sorted.sh`.
>
> ~~~
> $ wc -l | sort -n
> ~~~
> {: .bash}
>
> Karena tidak ada nama file sebagai input, maka `wc` sebenarnya akan menunggu
> inputnya. Namun jika kita beri input, misal `octane.pdb`, outputnya juga 
> tidak ada. Hal ini karena skrip tersebu tidak melakukan apapun. Jika kita tekan
> `Ctrl-D` (untuk memandai End of File, EOF), maka akan menghasilkan output `1` 
> karena hanya satu baris (hasil dari sort). Jadi pada kasus diatas (`bash sorted.sh`),
> skrip tidak melakukan apapun.
{: .callout}


Menyimpan history

Sering kita menemukan beberapa perintah yang sangat bermanfaat kemudian kita ingin 
menyimpannya. Misalnya setelah beberapa kali mem-plot.
Alih-alih mengulangi perintah tersebut, kita bisa langsung menyimpannya dengan perintah berikut:

~~~
$ history | tail -n 5 > redo-figure-3.sh
~~~
{: .bash}

Perintah tersebut akan menghasilkan file baru `redo-figure-3.sh` yang berisi:

~~~
297 bash goostats -r NENE01729B.txt stats-NENE01729B.txt
298 bash goodiff stats-NENE01729B.txt /data/validated/01729.txt > 01729-differences.txt
299 cut -d ',' -f 2-3 01729-differences.txt > 01729-time-series.txt
300 ygraph --format scatter --color bw --borders none 01729-time-series.txt figure-3.png
301 history | tail -n 5 > redo-figure-3.sh
~~~
{: .source}

Masih perlu edit manual lagi untuk menghapus nomor baris perintah dan 
juga perintah di baris terakhir (bisa otomatis juga lewat command line jika anda sudah jago). 
Sekarang kita punya data yang cukup untuk menghasilkan plot yang dibuat tadi (misalnya).

## Nelle's Pipeline: Creating a Script

Pembimbing Nelle menyarankan bahwa dia dapat membuat parameter tambahan untuk 
program `goostats` ketika dia memproses datanya.
Jika ini dikerjakan dengan tangan, akan butuh banyak waktu. Namun dengan loop `for`, 
dia cukup membutuhkan bebera jam saja.

Berdasarkan pengalamannya, dia belajar bahwa jika ada yang perlu dilakukan dua kali,
maka kemungkinan akan ada yang ketiga dan empat kalinya.
Dia menjalankan text editor dan menuliskan baris berikut.

~~~
# Calculate reduced stats for data files at J = 100 c/bp.
for datafile in "$@"
do
    echo $datafile
    bash goostats -J 100 -r $datafile stats-$datafile
done
~~~
{: .bash}

Parameter `-J 100` and `-r` merupakan dua parameter tambahan dari pembimbingnya.
Dia menyimpan file baru tersebut dengan nama `do-stats.sh` 
Jadi, sekarang dia menjalankan ulang perhitungannya sebagai berikut.

~~~
$ bash do-stats.sh *[AB].txt
~~~
{: .bash}

Dan juga melakukan hal berikut,

~~~
$ bash do-stats.sh *[AB].txt | wc -l
~~~
{: .bash}

Jadi outputnya adalah jumlah file yang diproses, bukan nama file yang diproses.
Tambahan penting yang dilakukan Nelle adalah dia memberikan 
fleksibilitas file mana yang akan diproses. Maka dia menuliskannya 
sebagai berikut:

~~~
# Calculate reduced stats for Site A and Site B data files at J = 100 c/bp.
for datafile in *[AB].txt
do
    echo $datafile
    bash goostats -J 100 -r $datafile stats-$datafile
done
~~~
{: .bash}

Keuntungan dari skrip yang baru saja dibuatnya adalah dia dapat 
memilih file yang diinginkan. Tidak perlu mengingat untuk mengecualikan file 
dengan akhiran 'Z'. 
Kelemahan dari skripnya tersebut adalah bahwa dia hanya bisa memproses 
file yang dipilih saja --- tidak bisa menjalankan semua file, 
termasuk yang berakhiran 'Z', 'G', atau 'H' sebagaimana file-file 
yang dihasilkan oleh teman-temannya yang bekerja di Antartika.
Jika dia ingin memproses file tersebut (berakhiran 'Z, 'G, 'H') 
maka dia harus memodifikasi skripnya untuk bisa mengecek parameter/argumen 
command-line yang diberikan, dan menggunakan `*[AB].txt` sebagai default 
jika tidak opsi yang diberikan. Hal ini merupakan pilihan antaran 
fleksibilitas dan kompleksitas dari sebuah skrip Shell. 
Seseorang yang telah berpengalaman dengan skrip Shell akan bisa menyelesaikan 
permasalahan tersebut,dan anda juga akan bisa bila terus belajar dan menggunakannya.

## LATIHAN
> ## Variables in Shell Scripts
>
> In the `molecules` directory, imagine you have a shell script called `script.sh` containing the
> following commands:
>
> ~~~
> head -n $2 $1
> tail -n $3 $1
> ~~~
> {: .bash}
>
> While you are in the `molecules` directory, you type the following command:
>
> ~~~
> bash script.sh '*.pdb' 1 1
> ~~~
> {: .bash}
>
> Which of the following outputs would you expect to see?
>
> 1. All of the lines between the first and the last lines of each file ending in `.pdb`
>    in the `molecules` directory
> 2. The first and the last line of each file ending in `.pdb` in the `molecules` directory
> 3. The first and the last line of each file in the `molecules` directory
> 4. An error because of the quotes around `*.pdb`
>
> > ## Solution
> > The correct answer is 2. 
> >
> > The special variables $1, $2 and $3 represent the command line arguments given to the
> > script, such that the commands run are:
> >
> > ```
> > $ head -n 1 cubane.pdb ethane.pdb octane.pdb pentane.pdb propane.pdb
> > $ tail -n 1 cubane.pdb ethane.pdb octane.pdb pentane.pdb propane.pdb
> > ```
> > {: .bash}
> > The shell does not expand `'*.pdb'` because it is enclosed by quote marks.
> > As such, the first argument to the script is `'*.pdb'` which gets expanded within the
> > script by `head` and `tail`.
> {: .solution}
{: .challenge}

> ## List Unique Species
>
> Leah has several hundred data files, each of which is formatted like this:
>
> ~~~
> 2013-11-05,deer,5
> 2013-11-05,rabbit,22
> 2013-11-05,raccoon,7
> 2013-11-06,rabbit,19
> 2013-11-06,deer,2
> 2013-11-06,fox,1
> 2013-11-07,rabbit,18
> 2013-11-07,bear,1
> ~~~
> {: .source}
>
> An example of this type of file is given in `data-shell/data/animals.txt`.
> 
> Write a shell script called `species.sh` that takes any number of
> filenames as command-line parameters, and uses `cut`, `sort`, and
> `uniq` to print a list of the unique species appearing in each of
> those files separately.
>
> > ## Solution
> >
> > ```
> > # Script to find unique species in csv files where species is the second data field
> > # This script accepts any number of file names as command line arguments
> >
> > # Loop over all files
> > for file in $@ 
> > do
> > 	echo "Unique species in $file:"
> > 	# Extract species names
> > 	cut -d , -f 2 $file | sort | uniq
> > done
> > ```
> > {: .source}
> {: .solution}
{: .challenge}

> ## Find the Longest File With a Given Extension
>
> Write a shell script called `longest.sh` that takes the name of a
> directory and a filename extension as its parameters, and prints
> out the name of the file with the most lines in that directory
> with that extension. For example:
>
> ~~~
> $ bash longest.sh /tmp/data pdb
> ~~~
> {: .bash}
>
> would print the name of the `.pdb` file in `/tmp/data` that has
> the most lines.
>
> > ## Solution
> >
> > ```
> > # Shell script which takes two arguments: 
> > #    1. a directory name
> > #    2. a file extension
> > # and prints the name of the file in that directory
> > # with the most lines which matches the file extension.
> > 
> > wc -l $1/*.$2 | sort -n | tail -n 2 | head -n 1
> > ```
> > {: .source}
> {: .solution}
{: .challenge}

> ## Why Record Commands in the History Before Running Them?
>
> If you run the command:
>
> ~~~
> $ history | tail -n 5 > recent.sh
> ~~~
> {: .bash}
>
> the last command in the file is the `history` command itself, i.e.,
> the shell has added `history` to the command log before actually
> running it. In fact, the shell *always* adds commands to the log
> before running them. Why do you think it does this?
>
> > ## Solution
> > If a command causes something to crash or hang, it might be useful
> > to know what that command was, in order to investigate the problem.
> > Were the command only be recorded after running it, we would not
> > have a record of the last command run in the event of a crash.
> {: .solution}
{: .challenge}

> ## Script Reading Comprehension
>
> For this question, consider the `data-shell/molecules` directory once again.
> This contains a number of `.pdb` files in addition to any other files you
> may have created.
> Explain what a script called `example.sh` would do when run as
> `bash example.sh *.pdb` if it contained the following lines:
>
> ~~~
> # Script 1
> echo *.*
> ~~~
> {: .bash}
>
> ~~~
> # Script 2
> for filename in $1 $2 $3
> do
>     cat $filename
> done
> ~~~
> {: .bash}
>
> ~~~
> # Script 3
> echo $@.pdb
> ~~~
> {: .bash}
>
> > ## Solutions
> > Script 1 would print out a list of all files containing a dot in their name.
> >
> > Script 2 would print the contents of the first 3 files matching the file extension.
> > The shell expands the wildcard before passing the arguments to the `example.sh` script.
> > 
> > Script 3 would print all the arguments to the script (i.e. all the `.pdb` files),
> > followed by `.pdb`.
> > cubane.pdb ethane.pdb methane.pdb octane.pdb pentane.pdb propane.pdb.pdb
> {: .solution}
{: .challenge}

> ## Debugging Scripts
>
> Suppose you have saved the following script in a file called `do-errors.sh`
> in Nelle's `north-pacific-gyre/2012-07-03` directory:
>
> ~~~
> # Calculate reduced stats for data files at J = 100 c/bp.
> for datafile in "$@"
> do
>     echo $datfile
>     bash goostats -J 100 -r $datafile stats-$datafile
> done
> ~~~
> {: .bash}
>
> When you run it:
>
> ~~~
> $ bash do-errors.sh *[AB].txt
> ~~~
> {: .bash}
>
> the output is blank.
> To figure out why, re-run the script using the `-x` option:
>
> ~~~
> bash -x do-errors.sh *[AB].txt
> ~~~
> {: .bash}
>
> What is the output showing you?
> Which line is responsible for the error?
>
> > ## Solution
> > The `-x` flag causes `bash` to run in debug mode.
> > This prints out each command as it is run, which will help you to locate errors.
> > In this example, we can see that `echo` isn't printing anything. We have made a typo
> > in the loop variable name, and the variable `datfile` doesn't exist, hence returning
> > an empty string.
> {: .solution}
{: .challenge}

Bacaan:
- [Review buku Linux: the art of problem determination](http://www.bagustris.blogspot.com/2014/02/the-art-of-problem-determination.html)
- [Keluar dari neraka dependensi](http://www.bagustris.blogspot.com/2014/02/keluar-dari-neraka-dependensi_28.html)
- [Shebang portable dalam shell bash](https://www.cyberciti.biz/tips/finding-bash-perl-python-portably-using-env.html)
