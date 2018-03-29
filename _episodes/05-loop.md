---
title: "Loops"
teaching: 40
exercises: 15
questions:
- "Bagaimana mengimplementasikan perintah-perintah yang sama pada file yang berbeda?"
objectives:
- "Membuat loop yang mengimplementasikan satu perintah atau lebih pada tiap file file dalam satu set."
- "Menelusuri nilai yang diambil dari sebuah variabel loop selama eksekusi dari loop tersebut."
- "Menjelaskan perbedaan dari tiap nama variabel dan nilainya."
- "Menjelaskan mengapa spasi dan beberapa karakter seharusnya tidak digunakan untuk menamai file."
- "Mendemonstrasikan bagaimana melihat perintah yang baru saja dieksekusi."
- "Menjalankan lagi perintah yang baru saja dieksekusi tanpa mengetikkan ulang."
keypoints:
- "Sebuah loop `for` melakukan perintah yang sama untuk semua file dalam sebuah list."
- "Semua loop `for` membutuhkan variabel untuk mengeksekusi operasi yang dilakukan terhadap file tersebut."
- "Gunakan `$name` atau untuk mengexpand variabel, yakni untuk mendapatkan nilainya."
- "Jangan menggunakan spasi, quotes, atau karakter wildcard seperti '*' or '?' dalam nama file."
- "Gunakan nama file yang konsisten agar mudah untuk menggunakan pola wildcard dalam membuat looping."
- "Gunakan panah ke atas untuk melihat perintah yang digunakan sebelumnya."
- "Gunakan `Ctrl-R` untuk mencari perintah yang pernah digunakan."
- "Gunakan perintah `history` untuk melihat perintah-perintah terakhir digunakan dan gunakan `!number` untuk mengulang perintah sesuai nomor yang ditampilkan."
---

**Loops** adalah kunci produktivitas melalui teknik otomasi karena dengan loop kita bisa mengukang operasi yang sama untuk file yang berbebeda-beda tanpa mengulangi pengetikkan perintah pada file lainnya. Sama halnya dengan penggunaan wildcard maupun tab completion, penggunaan loop meningkatkan efisiensi dan menghindari salah ketik.
Misal kita punya file yang dinamai dengan `basilisk.dat`, `unicorn.dat`, dan seterusnya.
Pada contoh ini kita akan menggunakan direktori `creatures` yang hanya berisi dua contoh file,
namun pada prinsipnya kita bisa mengaplikasikannya pada banyak file, ratusan, ribuat atau jutaan pada sekali eksekusi dengan
teknik looping. Kasusnya adalah kita ingin meng-copy semua file dalam direktori `creatures` tersebut dengan menambahkan
awalan `original-namafile.dat`, sehingga menjadi seperti 
`original-basilisk.dat` and `original-unicorn.dat`.

*Instead*, setelah belajar wildcard, kita ingin menggunakannya, tapi nyatanya tidak bisa:

~~~
$ cp *.dat original-*.dat
~~~
{: .bash}

karena perintah tersebut akan diterjemahkan menjadi:

~~~
$ cp basilisk.dat unicorn.dat original-*.dat
~~~
{: .bash}

Sehingga akan terjadi error dan file malah tidak ter-back-up:

~~~
cp: target `original-*.dat` is not a directory
~~~
{: .error}

Masalah ini tejadi karena `cp` menerina lebih dari dua input/argumen. Argumen terakhir
diartikan oleh `cp` sebagai direktori output. Karena tidak ada direktori yang namanya `original-*.dat`
maka terjadilah error.

Solusinya, kita menggunakan **loop** untuk melakukan operasi berulang dengan sekali eksekusi.
Berikut contohj sederhananya,

~~~
$ for filename in basilisk.dat unicorn.dat
> do
>    head -n 3 $filename
> done
~~~
{: .bash}

~~~
COMMON NAME: basilisk
CLASSIFICATION: basiliscus vulgaris
UPDATED: 1745-05-02
COMMON NAME: unicorn
CLASSIFICATION: equus monoceros
UPDATED: 1738-11-24
~~~
{: .output}

Ketika shell menemukan kata kunci `for`, 
dia akan mengetahui kalau perintah tersebut digunakan untuk mengulang sebuah perintah 
atau kumpulan perintah untuk tiap sesuatu `pada` sebuah list.
Untuk tiap iterasi (satu loop),
tiap nama sesuatu secara sekuensial di-assign pada **variabel** 
dan perintah di dalam loop dieksekusi sebelum berpindah pada sesuatu yang lain 
pada list tersebut.
Di dalam sebuah loop,
kita dapat memanggil nilai dari variabel dengan meletakkan tanda `$` di depannya.
Tanda `$` memerintahkan shell interpreter untuk memberkalukan **variabel** sebagai
nama variabel dan menggantinya dengan nilai yang sesuai menurut letaknya.
Bukan dieksekusi sebagai text atau perintah lainnya.

Pada contoh ini, daftar filenya ada dua: `basilisk.dat` and `unicorn.dat`.
Setiap kali loop iterasi, variabel `filename` akan diiisi dengan nama file dan 
menjalankan perintah `head` commmand.

Pada loop pertama, `$filename` adalah `basilisk.dat`. 
Interpreter menjalankan perintah `head` pada file `basilisk.dat`, 
dan mencetak tiga baris teratas dari `basilisk.dat`.
Pada iterasi kedua, `$filename` menjadi 
`unicorn.dat`. Kali ini, shell menjalankan `head` pada `unicorn.dat`
dan mencetak tiga baris teratas dari `unicorn.dat`. 
Karena hanya ada loop, maka shell berakhir setelah loop kedua.

Ketika menggunakan variabel, dimungkinan untuk meletakkan nama file pada 
kurung kurawal untuk membatasi nama variabelnya. 
Sehingga`$filename` ekivalen dengan `${filename}`, tapi berbeda dengan 
`${file}name`. Ini banyak dijumpai pada skrip shell.

> ## Mengikuti Prompt
>
> Prompt shell berubah dari tanda dolar, `$`, menjadi tanda "lebih dari", `>`
> ketika kita mengetikkan loop. Tanda kedua, `>`, berbeda menunjukkan bahwa 
> kita belum selesai mengetikkan perintah. Tanda semikolon, `;`, dapat digunakan 
> untuk memisahkan dua perintah yang ditulis dalam satu baris. Jadi loop diatas 
> dapat ditulis dalam satu bari dengan menambahkan `;` seperi berikut:
> `for filename in *.dat; do head -n 3 $filename; done.
{: .callout}

> ## Same Symbols, Different Meanings
>
> Here we see `>` being used a shell prompt, whereas `>` is also
> used to redirect output.
> Similarly, `$` is used as a shell prompt, but, as we saw earler,
> it is also used to ask the shell to get the value of a variable.
>
> If the *shell* prints `>` or `$` then it expects you to type something,
> and the symbol is a prompt.
>
> If *you* type `>` or `$` yourself, it is an instruction from you that
> the shell to redirect output or get the value of a variable.
{: .callout}

Kita menggunakan nama variable `filename` agar lebih mudah dibaca oleh manusia. 
Variable lain seperti `i` , `temperature` atau `x` dapat digunakan agar lebih sederhana.

~~~
for x in basilisk.dat unicorn.dat
do
    head -n 3 $x
done
~~~
{: .bash}

atau:

~~~
for temperature in basilisk.dat unicorn.dat
do
    head -n 3 $temperature
done
~~~
{: .bash}

Namun, orang lain akan kesulitan memahaminya, apakah `i` atau 
`x`, ataukah itu memang "temperature"...? Sebaliknya, nama `filename` akan 
segera dengan mudah diartikan bahwa 
variabel tersebut akan diisi nama file. 
Jadi, gunakan nama variabel yang sesuai.

Berikut contoh yang lebih kompleks.

~~~
for filename in *.dat
do
    echo $filename
    head -n 100 $filename | tail -n 20
done
~~~
{: .bash}

Shell mulai berjalan dengan mengenali file berekstensi `.dat` untuk diproses.
**body loop** kemudian mengeksekusi dua perintah yakni `echo` dan baris bawahnya.
Perintah echo akan mencetak nama file seperti halnya perintah berikut.

~~~
$ echo hello there
~~~
{: .bash}

prints:

~~~
hello there
~~~
{: .output}

Pada kasus diatas, maka akan dicetak `basilisk.dat` pada baris pertama output, 
kemudian dicari 100 baris paling atas (dari file basilisk.dat), dari 100 baris tersebut, 
dicari 20 baris terakhir (In this case,81~100) dan ditampilkan di bawah hasil echo tadi.

> ## Spaces in Names
>
> Spasi (*whitespace*) digunakan untuk memisahkan elemen pada list
> yang akan diiterasi. Jika ada elemen yang memiliki spasi
> maka dibutuhkan tanda kutip ganda diantara elemen tersebut 
> agar dibaca oleh shell sebagai satu kesatuan.
> 
> Contohnya adalah nama file berikut:
>
> ~~~
> red dragon.dat
> purple unicorn.dat
> ~~~
> {: .source}
> 
> 
> Maka kita tambahkan tanda kutip ganda sbb:
> 
> ~~~
> for filename in "red dragon.dat" "purple unicorn.dat"
> do
>     head -n 100 "$filename" | tail -n 20
> done
> ~~~
> {: .bash}
>
> Maka, akan lebih mudah untuk tidak memakai spasi (atau karakter lainnya) dalam penamaan 
> file, cukup menggunakan huruf dan angka saja.
>
> Karena dua file di atas tidak ada (`red dragon.dat` dan `purple unicorn.dat`)
> maka tidak bisa dibuka oleh shell.
>
> ```
> head: cannot open ‘red dragon.dat’ for reading: No such file or directory
> head: cannot open ‘purple unicorn.dat’ for reading: No such file or directory
> ```
> {: .output}
>
> Jika kita coba untuk menghapus tanda kutip pada `$filename`　maka `red dragon.dat` 
> akan dibaca menjadi dua file: `red` dan `dragon` begitu pula nama file setelahnya.s
> ```
> head: cannot open ‘red’ for reading: No such file or directory
> head: cannot open ‘dragon.dat’ for reading: No such file or directory
> head: cannot open ‘purple’ for reading: No such file or directory
> CGGTACCGAA
> AAGGGTCGCG
> CAAGTGTTCC
> ...
> ```
> {: . output}
{: .callout}

Kembali ke masalah awal bab ini, yakni untuk mengcopy file asli dengan 
menambahkan kata `original` di depannya, maka dapat dijalankan perintah berikut.
~~~
for filename in *.dat
do
    cp $filename original-$filename
done
~~~
{: .bash}

Pada loop pertama perintah `cp` akan mencopy file 
`basilisk.dat` ke file `original-basilisk.dat` seperti 
perintah berikut.

~~~
cp basilisk.dat original-basilisk.dat
~~~
{: .bash}

Pada loop kedua, maka berlaku nama file berikutnya, `unicorn.dat`

~~~
cp unicorn.dat original-unicorn.dat
~~~
{: .bash}

Begitu seterusnya sampai loop selesai. Pada kasus diatas hanya dua loop saja.
Bayangkan jika ada 1000 file, maka loop diatas sangat efisien.

Karena cp tidak memiliki output, akan sulit mengecek apak shell benar-benar 
berjalan seperti yang kita harapkan. Dengan menambahkan `echo` maka kita akan bisa 
mengecek jika setiap perinah _telah_ dieksekusi. Diagram berikut menggambarkan bagaimana 
`echo` dapat digunakan untuk [mendebug](https://en.wikipedia.org/wiki/Debugging) 
perintah Unix/Linux dengan sangat baik.

![For Loop in Action](../fig/shell_script_for_loop_flow_chart.svg)

Skrip dan diagram shell diatas hanya akan meng-echo perintah-perintahnya saja. Agar perintah juga dijalankan sekaligus ditampilan maka dapat ditambahkan pipe seperti berikut.

`for filename in *.dat; do var=$(cp $filename original-$filename) | echo $var; done`

## Nelle's Pipeline: Processing Files

Sekarang Nelle sudah siap untuk memproses datanya.
Karena dia masih belajar dalam menggunakan shell, 
dia memutuskan untuk membangun perintah-perintah yang 
digunakan secara bertahap. 
Pada tahap pertama dia ingin memastikan bahwa dia dapat 
memilih file yang tepat --- ingat, file yang berakhiran 'A' atau 'B', 
bukan 'Z'. 
Dari direktori home-nya, dia mengetikkan:

~~~
$ cd north-pacific-gyre/2012-07-03
$ for datafile in *[AB].txt
> do
>     echo $datafile
> done
~~~
{: .bash}

~~~
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
NENE02043A.txt
NENE02043B.txt
~~~
{: .output}

Langkah selanjutnya adalah memutuskan file yang 
akan dibuat oleh program `goostats` dengan menambahkan prefix "stats" 
pada tiap file. Ini terlihat mudah dengan sedikit modifikasi pada loop sebelumnya.

~~~
$ for datafile in *[AB].txt
> do
>     echo $datafile stats-$datafile
> done
~~~
{: .bash}

~~~
NENE01729A.txt stats-NENE01729A.txt
NENE01729B.txt stats-NENE01729B.txt
NENE01736A.txt stats-NENE01736A.txt
...
NENE02043A.txt stats-NENE02043A.txt
NENE02043B.txt stats-NENE02043B.txt
~~~
{: .output}

Dia belum menjalankan program `goostat`, 
hanya mensimulasikan file yang akan dia dapat. 
Sekarang dia yakin dapat memilih file yang tepat dan 
menghasilkan nama file baru yang sesuai pula.

Mengetikkan perintah lagi dan lagi akan cukup melelahkan 
dan membuang waktu. Kita dapat menggunakan tombol panah keatas 
untuk mengulangi perintah sebelumnya. Untuk perintah loop yang 
berbaris-baris, akan ditampilkan oleh shell dalam satu baris saja 
dengan menambahkan semicolon yang diperlukan.

~~~
$ for datafile in *[AB].txt; do echo $datafile stats-$datafile; done
~~~
{: .bash}

Menggunakan panah kanan dan ke kiri, Nelle bisa mengedit perintah 
sebelumnya yang dicari dengan panah ke atas, kemudian mengganti 
`echo` dengan `bash goostats`.

~~~
$ for datafile in *[AB].txt; do bash goostats $datafile stats-$datafile; done
~~~
{: .bash}

Ketika dia menekan tombol Enter, dia tidak melihat output apapun.
Kemudian tak lama kemudian (bergantung pada CPU dan RAM) command prompt `$` muncul lagi 
menandakan proses telah selesai.

Dia ingin tahu proses yang berjalan pada tiap file. .
Karenanya dia menambahkan echo sebagai berikut (setelah menekan panah keatas dan 
kekiri untuk mengeditnya)

~~~
$ for datafile in *[AB].txt; do echo $datafile; bash goostats $datafile stats-$datafile; done
~~~
{: .bash}

> ## Awal dan Akhir Perintah
>
> Kita bisa berpindah ke Awal perintah (karakter pertama) dengan menekan
> tombol `Ctrl-A`, dan akhir perintah (karakter terakhir) dengan `Ctrl-E`.
{: .callout}

Ketika dia menjalankan loop tersebut lagi tampak 
nama file yang sedang diproses oleh loop. 
Dia mengestimasi tiap file membutuhkan waktu komputasi 
sekitar lima detik.

~~~
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
~~~
{: .output}

Jika dia punya 1518 data, dikalikan 5 detik, 
dibagi 60, maka dia skripnya membutuhkan waktu komputasi sekitar dua jam. 
Setelah megeceknya lagi, dia membuka terminal, 
beralih ke direktori `north-pacific-gyre/2012-07-03`,
dan menggunakan `cat stats-NENE01729B.txt` untuk melihat outputnya.

Hasilnya terlihat baik. Dia memutuskan untuk pergi minum kopi dan membaca paper 
sambil menunggu hasil program yang dijalankannya tadi.

> ## Those Who Know History Can Choose to Repeat It
>
> Selain menggunakan panah atas, kita juga bisa menggunakan perinah `history` 
> untuk menampilkan daftar perintah yang telah digunakan sebelumnya. 
> Untuk mengulangi perintah yang ada dalam daftar tersebut, 
> kita menggunakan perintah `!nomor` dimana `nomor` merupakan nomor yang ada 
> pada daftar history (sisi paling kiri).
> 
> ~~~
> $ history | tail -n 5
> ~~~
> {: .bash}
> ~~~
>   456  ls -l NENE0*.txt
>   457  rm stats-NENE01729B.txt.txt
>   458  bash goostats NENE01729B.txt stats-NENE01729B.txt
>   459  ls -l NENE0*.txt
>   460  history
> ~~~
> {: .output}
>
> Untuk menjalankan program `goostats` pada file `NENE01729B.txt` dia mengetikkan:
> `!458`.
{: .callout}

> ## Other History Commands
>
> Ada cara lain untuk mengakses history yang lebih cepat untuk mencari history 
> perintah yang kita jalankan, yakni dengan shortcut 'Ctrl-R'.
> Tekan `Ctrl-R` dan ketikkan apa yang anda cari. Jika apa yang anda cari lebih dari 
> sekali, tekan `Ctrl-R` lagi untuk mencarinya. Misalnya: `Ctrl-R` kemudian ketikkan `for` 
> kemudian jika output yang ditampilkan bukan "loop for" yang dicari, tekan `Ctrl-R` lagi.
> - `!!` juga menjalankan perintah sebelumnya, namun tidak senyaman panah atas.
> - `!$` akan menjalankan kata terakhir pada perintah sebelumnya.
> 
> Berikut contoh penggunaannya yang sangat bermanfaat. Jalankan perintah 
> `bash goostats NENE01729B.txt stats-NENE01729B.txt`, kemudian ketikkan
> `less !$` untuk melihat isi `stats-NENE01729B.txt`. Ini lebih cepat 
> daripada menekan panah atas dan mengeditnya.
{: .callout}

## KUIS

> ## Variables in Loops
>
> This exercise refers to the `data-shell/molecules` directory.
> `ls` gives the following output:
>
> ~~~
> cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> ~~~
> {: .output}
>
> What is the output of the following code?
>
> ~~~
> for datafile in *.pdb
> do
>     ls *.pdb
> done
> ~~~
> {: .bash}
>
> Now, what is the output of the following code?
>
> ~~~
> for datafile in *.pdb
> do
>	ls $datafile
> done
> ~~~
> {: .bash}
>
> Why do these two loops give different outputs?
>
> > ## Solution
> > The first code block gives the same output on each iteration through
> > the loop.
> > Bash expands the wildcard `*.pdb` within the loop body (as well as
> > before the loop starts) to match all files ending in `.pdb`
> > and then lists them using `ls`.
> > The expanded loop would look like this:
> > ```
> > for datafile in cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > do
> >	ls cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > done
> > ```
> > {: .bash}
> >
> > ```
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > ```
> > {: .output}
> >
> > The second code block lists a different file on each loop iteration.
> > The value of the `datafile` variable is evaluated using `$datafile`,
> > and then listed using `ls`.
> >
> > ```
> > cubane.pdb
> > ethane.pdb
> > methane.pdb
> > octane.pdb
> > pentane.pdb
> > propane.pdb
> > ```
> > {: .output}
> {: .solution}
{: .challenge}

> ## Saving to a File in a Loop - Part One
>
> In the same directory, what is the effect of this loop?
>
> ~~~
> for alkanes in *.pdb
> do
>     echo $alkanes
>     cat $alkanes > alkanes.pdb
> done
> ~~~
> {: .bash}
>
> 1.  Prints `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb` and `propane.pdb`,
>     and the text from `propane.pdb` will be saved to a file called `alkanes.pdb`.
> 2.  Prints `cubane.pdb`, `ethane.pdb`, and `methane.pdb`, and the text from all three files would be
>     concatenated and saved to a file called `alkanes.pdb`.
> 3.  Prints `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, and `pentane.pdb`, and the text
>     from `propane.pdb` will be saved to a file called `alkanes.pdb`.
> 4.  None of the above.
>
> > ## Solution
> > 1. The text from each file in turn gets written to the `alkanes.pdb` file.
> > However, the file gets overwritten on each loop interation, so the final content of `alkanes.pdb`
> > is the text from the `propane.pdb` file.
> {: .solution}
{: .challenge}

> ## Saving to a File in a Loop - Part Two
>
> In the same directory, what would be the output of the following loop?
>
> ~~~
> for datafile in *.pdb
> do
>     cat $datafile >> all.pdb
> done
> ~~~
> {: .bash}
>
> 1.  All of the text from `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, and
>     `pentane.pdb` would be concatenated and saved to a file called `all.pdb`.
> 2.  The text from `ethane.pdb` will be saved to a file called `all.pdb`.
> 3.  All of the text from `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb`
>     and `propane.pdb` would be concatenated and saved to a file called `all.pdb`.
> 4.  All of the text from `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb`
>     and `propane.pdb` would be printed to the screen and saved to a file called `all.pdb`.
>
> > ## Solution
> > 3 is the correct answer. `>>` appends to a file, rather than overwriting it with the redirected
> > output from a command.
> > Given the output from the `cat` command has been redirected, nothing is printed to the screen.
> {: .solution}
{: .challenge}

> ## Limiting Sets of Files
>
> In the same directory, what would be the output of the following loop?
>
> ~~~
> for filename in c*
> do
>     ls $filename 
> done
> ~~~
> {: .bash}
>
> 1.  No files are listed.
> 2.  All files are listed.
> 3.  Only `cubane.pdb`, `octane.pdb` and `pentane.pdb` are listed.
> 4.  Only `cubane.pdb` is listed.
>
> > ## Solution
> > 4 is the correct answer. `*` matches zero or more characters, so any file name starting with 
> > the letter c, followed by zero or more other characters will be matched.
> {: .solution}
>
> How would the output differ from using this command instead?
>
> ~~~
> for filename in *c*
> do
>     ls $filename 
> done
> ~~~
> {: .bash}
>
> 1.  The same files would be listed.
> 2.  All the files are listed this time.
> 3.  No files are listed this time.
> 4.  The files `cubane.pdb` and `octane.pdb` will be listed.
> 5.  Only the file `octane.pdb` will be listed.
>
> > ## Solution
> > 4 is the correct answer. `*` matches zero or more characters, so a file name with zero or more
> > characters before a letter c and zero or more characters after the letter c will be matched.
> {: .solution}
{: .challenge}

> ## Doing a Dry Run
>
> A loop is a way to do many things at once --- or to make many mistakes at
> once if it does the wrong thing. One way to check what a loop *would* do
> is to `echo` the commands it would run instead of actually running them.
> 
> Suppose we want to preview the commands the following loop will execute
> without actually running those commands:
>
> ~~~
> for file in *.pdb
> do
>   analyze $file > analyzed-$file
> done
> ~~~
> {: .bash}
>
> What is the difference between the two loops below, and which one would we
> want to run?
>
> ~~~
> # Version 1
> for file in *.pdb
> do
>   echo analyze $file > analyzed-$file
> done
> ~~~
> {: .bash}
>
> ~~~
> # Version 2
> for file in *.pdb
> do
>   echo "analyze $file > analyzed-$file"
> done
> ~~~
> {: .bash}
>
> > ## Solution
> > The second version is the one we want to run.
> > This prints to screen everything enclosed in the quote marks, expanding the
> > loop variable name because we have prefixed it with a dollar sign.
> >
> > The first version redirects the output from the command `echo analyze $file` to
> > a file, `analyzed-$file`. A series of files is generated: `cubane.pdb`,
> > `ethane.pdb` etc.
> > 
> > Try both versions for yourself to see the output! Be sure to open the 
> > `analyzed-*.pdb` files to view their contents.
> {: .solution}
{: .challenge}

> ## Nested Loops
>
> Suppose we want to set up up a directory structure to organize
> some experiments measuring reaction rate constants with different compounds
> *and* different temperatures.  What would be the
> result of the following code:
>
> ~~~
> for species in cubane ethane methane
> do
>     for temperature in 25 30 37 40
>     do
>         mkdir $species-$temperature
>     done
> done
> ~~~
> {: .bash}
>
> > ## Solution
> > We have a nested loop, i.e. contained within another loop, so for each species
> > in the outer loop, the inner loop (the nested loop) iterates over the list of
> > temperatures, and creates a new directory for each combination.
> >
> > Try running the code for yourself to see which directories are created!
> {: .solution}
{: .challenge}
