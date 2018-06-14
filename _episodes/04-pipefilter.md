---
title: "Pipes dan Filters"
teaching: 30
exercises: 20
questions:
- "Bagaimana mengkombinasikan perintah-perintah yang ada untuk menghasilkan output baru yang diinginkan?"
objectives:
- "Me-redirect output dari sebuah perintah ke dalam file."
- "Memproses file menggunakan redirection."
- "Membangun pipelines perintah dengan dua tingkat atau lebih."
- "Menjelaskan apa yang erjadi jika sebuah program atau pipeline tidak memberikan input ke proses."
- "Menjelaskan potongan kecil Unix yang membentuk filosofi besar."
keypoints:
- "`cat` menampilkan isi dari input/argumennya."
- "`head` menampilkan beberapa baris pertama dari inputnya."
- "`tail` menampilkan beberapa baris terakhir dari inputnya."
- "`sort` mensortir input."
- "`wc` menghitung baris, kata dan karakter dari input."
- "`*` mencocokkan nol atau lebih karakter dalam nama file, misal `*.txt` akan mencocokkan semua file yang berakhiran dengan ekstensi `.txt`."
- "`?` mencocokkan satu karakter apapun dalam nama file, misal `?.txt` akan mencocokkan dengan `a.txt` tetapi tidak cocok dengan`any.txt`."
- "`command > file` menyalurkan output perintah `command` ke dalam `file`."
- "`first | second` adalah sebuah pipeline: output dari first digunakan sebagai input dari second."
- "Cara terbaik menggunakan shell adalah dengan menggunakan pipes untuk mengkombinasikan program single-purpose sederhana (filters)."
---

Sekarang kita mengetahui beberapa perintah dasar dari shell.
Kemampuan shell akan maksimal jika kita mampu mengkombinasikan beberapa perintah
untuk mendapatkan ouput yang kita inginkan.

Kita akan memulai dengan sebuah directory yang disebut `molecules`.
Di dalamnya, terdapat enam file yang menggambarkan beberapa molekul organik sederhana.
Ekstensi `.pdb` mengindikasikan bahwa file disimpan dalam format **P**rotein **D**ata **B**ank,
sebuah format teks sederhana yang menyatakan tipe dan posisi tiap atom pada molekul.

~~~
$ ls molecules
~~~
{: .bash}

~~~
cubane.pdb    ethane.pdb    methane.pdb
octane.pdb    pentane.pdb   propane.pdb
~~~
{: .output}

Kemudian masuklah pada direktori `molecules` tersebut dengan perintah `cd` dan jalankan perintah `wc *.pdb`.
`wc` merupakan kepanjangan dari "word count":
perintah tersebut akan menghitung jumlah baris, kata dan karakter pada sebuah file.
Tanda bintang, `*` pada `*.pdb` mencocokkan nol atau lebih karakter,
jadi shell akan menghasilkan output dari `*.pdb` sebuah lish dari semua file `.pdb` dalam direktori ini.

~~~
$ cd molecules
$ wc *.pdb
~~~
{: .bash}

~~~
  20  156 1158 cubane.pdb
  12   84  622 ethane.pdb
   9   57  422 methane.pdb
  30  246 1828 octane.pdb
  21  165 1226 pentane.pdb
  15  111  825 propane.pdb
 107  819 6081 total
~~~
{: .output}

> ## Wildcards
>
> Tanda `*` adalah **wildcard**. Dengan menggunakan tanda ini
> shell akan mencocokkan karakter-karakter dalam tanda tersebut
> dengan yang dicari. Misalnya `ethane.pdb`, `propane.pdb` 
> dan file lainnya yang berakhiran dengan ekstensi `.pdb`. 
> Jika kita gunakan `p*.pdb` maka shell akan menghasilkan
> `propane.pdb` dan `pentane.pdb` karena kedua file tersebut
> diawali oleh huruf p. 
>
> Tanda `?` juga merupakan wildcard, bedanya tanda tanya tersebut 
> hanya mencocokkan satu karakter saja. Misal contoh terakhir diatas 
> jika kita ganti menjadi `p?.pdb` maka akan mencocokkan nama file yang
> berawalan huruf p dan satu karakter lagi setelahnya, misal
> `p1.pdb`, `pi.pdb` dan sejenisnya. Kedua wildcara tersebut (tanda
> asterisk dan tanda tanya bisa kita gabungkan untuk mencari file.
>
> Ketika shell menemukan wildcard dalam pola pencarian, 
> maka shell akan membuat list yang sesuai sebelum 
> menampilkan output dari perintah yang dijalankan. Contohnya perintah
> `ls *.pdb`, maka shell akan melist semua file yang berkekstensi .pdb (jika ada)
> dan menampilkannya sebagai output perintah `ls`. Sebaliknya jika tidak
> ada file yang sesuai dengan pola wildcard, maka 
> shell akan menampilkan pesan kesalahan. Misal
> kita jalankan perintah `ls *.pdf` dalam direktory `molecules`, 
> maka akan muncul pesan `ls: cannot access '*.pdf': No such file or directory`.
> Perintah-perintah seperti `wc` dan `ls` akan melihat list yang cocok
> dengan pola pencarian, namun tidak wildcard itu sendiri.
> Shell, tidak seperti program lainnya, mengikutsertakan wildcard
> dalam pencarian (bukan mencari tanda bintang atau tanda tanya),
> ini merupakan contoh dari desain orthogonal.
{: .callout}

> ## Menggunakan Wildcards
>
> When run in the `molecules` directory, which `ls` command(s) will
> produce this output?
>
> `ethane.pdb   methane.pdb`
>
> 1. `ls *t*ane.pdb`
> 2. `ls *t?ne.*`
> 3. `ls *t??ne.pdb`
> 4. `ls ethane.*`
>
>> ## Solution
>>  The solution is `3.`
>>
>> `1.` shows all files whose names contain zero or more characters (`*`) followed by the letter `t`, then zero or more characters (`*`) followed by `ane.pdb`. This gives `ethane.pdb  methane.pdb  octane.pdb  pentane.pdb`. 
>>
>> `2.` shows all files whose names start with zero or more characters (`*`) followed by the letter `t`, then a single character (`?`), then `ne.` followed by zero or more characters (`*`). This will give us `octane.pdb` and `pentane.pdb` but doesn't match anything which ends in `thane.pdb`.
>>
>> `3.` fixes the problems of option 2 by matching two characters (`??`) between `t` and `ne`. This is the solution.
>>
>> `4.` only shows files starting with `ethane.`.
> {: .solution}
{: .challenge}

Jika kita menjalankan perintah `wc -l` daripada `wc`,
maka outputnya akan menampilkan jumlah baris saja:

~~~
$ wc -l *.pdb
~~~
{: .bash}

~~~
  20  cubane.pdb
  12  ethane.pdb
   9  methane.pdb
  30  octane.pdb
  21  pentane.pdb
  15  propane.pdb
 107  total
~~~
{: .output}

Kita juga bisa menggunakan argumen `-w` untuk menampilkan jumlah kata
dan argumen `-c` untuk menampilkan jumlah karakter.

Mana dari file-file tersebut yang paling sedikit jumlah barisnya?

Jika hanya 6 file pada contoh diatas, kita dapat dengan mudah menemukan file dengan
jumlah baris paling sedikit. Bagaimana jika jumlah filenya 1000 atau 10000?
Akan sulit dicari mana file paling pendek dengan cara manual seperti di atas.
Cobalah perintah berikut:

~~~
$ wc -l *.pdb > lengths.txt
~~~
{: .bash}

Jadi, output dari `wc` akan kita simpan dalam file bernama length.txt. 
Perhatikan tanda **redirect**, yakni **>**. Jika file telah ada maka akan di-overwrite.
File length ini kemudian akan kita sort.
Argumen yang kita gunakan adalah `-n` yang akan mengurutkan berdasarkan angkanya (**n**umeric).
Dengan demikian kita mengdapatkan hasil sortir file `*.pdb` yang urut dari kecil ke besar.

`ls lengths.txt` mengkonfirmasi adanya file:

~~~
$ ls lengths.txt
~~~
{: .bash}

~~~
lengths.txt
~~~
{: .output}

Gunakan perintah `cat` untuk melihat isi file `lengths.txt`.

~~~
$ cat lengths.txt
~~~
{: .bash}

~~~
  20  cubane.pdb
  12  ethane.pdb
   9  methane.pdb
  30  octane.pdb
  21  pentane.pdb
  15  propane.pdb
 107  total
~~~
{: .output}

> ## Output halaman demi halaman
>
> We'll continue to use `cat` in this lesson, for convenience and consistency,
> but it has the disadvantage that it always dumps the whole file onto your screen.
> More useful in practice is the command `less`,
> which you use with `$ less lengths.txt`.
> This displays a screenful of the file, and then stops.
> You can go forward one screenful by pressing the spacebar,
> or back one by pressing `b`.  Press `q` to quit.
{: .callout}

~~~
$ sort -n lengths.txt
~~~
{: .bash}

~~~
  9  methane.pdb
 12  ethane.pdb
 15  propane.pdb
 20  cubane.pdb
 21  pentane.pdb
 30  octane.pdb
107  total
~~~
{: .output}

Kita bisa menyimpan hasil sortir diatas, misal dengan nama file `sorted-lengths.txt`
dengan menggunakan `> sorted-lengths.txt` setelah perintah sebelumya, `> lengths.txt` 
yang menyimpan output dari `wc` pada file `lengths.txt`.
Terakhir kita bisa menggunakan perintah `head` untuk melihat isi `sorted-lengths.txt`:

~~~
$ sort -n lengths.txt > sorted-lengths.txt
$ head -n 1 sorted-lengths.txt
~~~
{: .bash}

~~~
  9  methane.pdb
~~~
{: .output}

Menggunakan parameter `-n 1` pada perintah `head` berarti kita
menyuruh `head` menampikan hanyaa 1 baris saja, ganti dengan `-n 5` untuk
menampilkan lima baris peratama.

Beberapa perintah diatas dapat digabung sebagai berikut:

~~~
$ wc -l *.pdb > length.txt | sort -n
~~~
{: .bash}

> ## Redirecting ke file yang sama
>
> Adalah ide yang buruk untuk me-redirect
> output dari perintah yang bekerja pada file 
> yang sama. Sebagai contoh:
>
> ~~~
> $ sort -n lengths.txt > lengths.txt
> ~~~
> {: .bash}
>
> Melakukan operasi seperti itu akan berbahaya
> untuk file-file penting karena akan menghapus
> isi dari file `lengths.txt`.
{: .callout}


Jika anda mulai bingung dengan perintah-perintah ini,
ini artinya kabar baik. Cobalah beberapa kali perintah `wc`, `sort`, dan `head` 
Coba gabungkan beberapa perintah sekaligus untuk melihat outputnya.

~~~
$ sort -n lengths.txt | head -n 1
~~~
{: .bash}

~~~
  9  methane.pdb
~~~
{: .output}


Batang vertikal, `|` diantara dua perinah disebut **pipe**.
Jadi artinya ouput perintah di sebelah kiri **pipe** menjadi
input perintah di sebelah kanan **pipe**.
Komputer akan membuat file temporary file jika dibutuhkan, atau meng-copy
dari program satu ke yang lainnya dalam memory;
kita tidak perlu mengetahui detail prosesnya.

Dengan pipe, kita menyalurkan ouput perintah `wc` pada perintah `sort`,
kemudian hasilnya kita **redirect** dan kita tulis dalam file baru,
atau output `sort` kita inputkan ke `head` seperti berikut ini.


~~~
$ wc -l *.pdb | sort -n
~~~
{: .bash}

~~~
   9 methane.pdb
  12 ethane.pdb
  15 propane.pdb
  20 cubane.pdb
  21 pentane.pdb
  30 octane.pdb
 107 total
~~~
{: .output}

Dan kemudian output dari sort kita kirim sebagai input dari `head` sehingga pipeline fullnya menjadi sebagai berikut:

~~~
$ wc -l *.pdb | sort -n | head -n 1
~~~
{: .bash}

~~~
   9  methane.pdb
~~~
{: .output}

Ini adalah cara yang sama dengan yang dilakukan matematikawan ketika menghitung *log(3x)*,
yakni tiga kali *x*, dan hasilnya di-log-kan.
Pada kasus kita di atas, "head dari sort dari line count dari file`*.pdb`".

## stdin dan stdout
Beginilah sebenarnya yang terjadi ketika kita membuat pipe.
Ketika shell menjalan perintah --- perintah apapun ---, akan membuat suatu **process**
dalam memory untuk menahan perangkat lunak program tersebut dan keadaan saat itu.
Setiap proses memiliki kanal input yang disebut **standard input** (stdin).
Setiap proses juga memiliki ouput yang disebut **standard output**. (stdout
Kanal output lainnya adalah **standard error** (stderr).

Kanal error ini biasanya digunakan untuk untuk mendiagnosa program, selain tetap also mengirimkan
outout dari perintah di sebelah kiri pipe ke perintah di sebelah kanan pipe.

## Unix philosophy: Do one thing and do it well.
Shell sebenarnya adalah sebuah program, 
dalam keadaan normal, apapun yang kita tik
di shell sesuai standar input akan menampilkan
apapun outputnya pada layar terminal.
Ketika kita menjalankan sebuah prorgram pada shell, 
shel akan menjalankan sebuah proses dan mengirimnya sementara 
apapun yang kita tik pada keyboard ke input standar proses,
dan apapun proses yang terkirim akan dikiriom ke standar output 
ke standar output.

Apa yang terjadi ketika kita menjalankan `wc -l *.pdb > lengths.txt`.
Shell mulai meminta komputer untuk membuat proses baru menjalankan
program `wc`. Karena kita memberikan input nama file sebagai parameter, 
`wc` akan membacanya sebagai input, dan kita menambahkan  `>` untuk
me-redirect output dari proses ke sebuah file. Shell
menghubungkan output dari proses standar ke file tersebut.

Jika kita menjalankan `wc -l *.pdb | sort -n`,
shell akan membuat dua proses (satu untuk tiap sisi *pipe*) 
sehingga `wc` dan `sort` berjalan secara simultan.
Luaran standar dari `wc` diteruskan ke input standar dari `sort`;
karena tidak ada redirection `>`, output dari `sort` akan langsung
menuju layar. Jika kita jalankan `wc -l *.pdb | sort -n | head -n 1`,
kita akan mendapatkan tiga proses dengan aliran data dari satu ke yang lain, 
dari `wc` ke `sort`, dari `sort` ke `head` kemudian ke layar.

![Redirects and Pipes](../fig/redirects-and-pipes.png)

Ide sederhana dari Unix ini membuat Unix (dan Linux) sangat sukses.
Apa ide sederhananya..? Tak lain adalah membatasi tiap satu perintah
melakukan satu pekerjaan -- **do one job well**, dan tiap perintah 
tersebut bekerja dengan baik satu sama lain.
Model pemrograman dengan mengkombinasikan beberapa perintah ini 
disebut "pipes dan filters". Kita telah menggunakan "pipes", kemudian 
bagaimana dengan filter...?
Sebuah **filter** adalah program seperti `wc` dan `sort` yang
mentransformasikan stream input ke stream output.
Hampir semua perangkat standar Unix dapat bekerja sepert ini, kecuali 
diperintahkan untuk bekerja sebaliknya. Unix shell membaca input standar,
mengerjakan apa yang diperintahkan, dan menuliskan hasilnya ke output standar.

Intinya, setiap program yang dapat membaca input dari tiap baris file, dan 
menuliskan output tiap barisnya pada standar output dapat dikombinasikan dengan 
program yang lain yang juga bekerja demikian. Anda juga bisa, **dan seharusnya**, 
membuat program dengan cara ini sehingga orang lain yang menggunakan program anda
dapat meletakkan program anda tersebut dalam pipes agar lebih powerful.

> ## Redirecting Input
>
> Seperti halnya menggunakan `>` untuk me-redirect output dari sebuah program
> kita juga bisa menggunakan `<` untuk me-redirect input dari sebuah program
> misalnya untuk membaca file, bukan standar inputnya. Jadi daripada
> menulis `wc ammonia.pdb`, kita bisa menulis `wc < ammonia.pdb`. Pada kasus pertama 
> `wc` menerima paramater command line untuk membuka file tersebut.
> Pada perintah kedua, `wc` tidak menerima parameter command line, sehingga
> akan membaca dari input standar, namun kita telah menyuruh shell untuk mengirim
> isi dari `ammonia.pdb` ke standar input dari `wc`.
> standard input.
{: .callout}

## Nelle's Pipeline: Checking Files

Nelle telah menjalankan sampelnya pada komputer uji
da menghasilkan 1520 files pada direcktori `north-pacific-gyre/2012-07-03` yang 
telah dijelaskan sebelumnya.
Untuk mengeceknya secara cepat, dari directory `home`, Nelle mengetikkan:

~~~
$ cd north-pacific-gyre/2012-07-03
$ wc -l *.txt
~~~
{: .bash}

Outputnya adalah 1520 baris seperti ini:

~~~
300 NENE01729A.txt
300 NENE01729B.txt
300 NENE01736A.txt
300 NENE01751A.txt
300 NENE01751B.txt
300 NENE01812A.txt
... ...
~~~
{: .output}

Sekarang dia mengetikkan:

~~~
$ wc -l *.txt | sort -n | head -n 5
~~~
{: .bash}

~~~
 240 NENE02018B.txt
 300 NENE01729A.txt
 300 NENE01729B.txt
 300 NENE01736A.txt
 300 NENE01751A.txt
~~~
{: .output}

Whoops: salah satu file lebih pendek 60 baris dari yang lainnya.
Ketika dia kembali dan mengeceknya, 
dia melihat bahwa dia melakukan ujicoba pada jam 8 pagi Senin pagi --- 
seseorang mungkin telah lupa untuk meresetnya.

Sebelum menjalankan ulang sampel tersebut, 
dia mengecek apakah file-file tersebut memiliki banyak data:
~~~
$ wc -l *.txt | sort -n | tail -n 5
~~~
{: .bash}

~~~
 300 NENE02040B.txt
 300 NENE02040Z.txt
 300 NENE02043A.txt
 300 NENE02043B.txt
5040 total
~~~
{: .output}

Hasilnya terlihat baik --- namun apa maksud kode `Z` pada baris kedua?
Semua sampel seharusnya memiliki kode akhir `A` atau `B`. 
Dengan konvensi, lab-nya menggunakan tanda `Z` untuk mengindasikan sampel 
yang kurang informasinya.

Untuk mengetahui sampel lain yang demikian (kurang informasi) dia mengetikkan 
perintah berikut:

~~~
$ ls *Z.txt
~~~
{: .bash}

~~~
NENE01971Z.txt    NENE02040Z.txt
~~~
{: .output}

Cukup yakin, ketia dia mengecek data log pada laptopnya, tidak ada 
keterangan untuk mendapatkan informasi tambahan pada data yang kurang informasinya. 
Jadi, dia harus membuang data-data yang kurang informasinya tersebut.
Dia bisa menghapusnya dengan perintah `rm`. 
Namun, dia bisa melalukan analisis untuk sampel-sampel yang kurang tersebut.
Jadi dia lebih baik menggunakan ekspresi wilcard `*[AB].txt` saja. 
Seperti sebelumnya dijelaskan, tanda bintang `*` akan mengeksekusi file dengan 
nama berisi karakter apapun; `[AB]` artinya hanya file yang berakhiran A atau B saja 
yang dieksekusi.

## LATIHAN
> ## Apa makna perintah `sort -n`?
>
> Jika kita menjalankan perintah `sort` pada file tersebut:
>
> ~~~
> 10
> 2
> 19
> 22
> 6
> ~~~
> {: .source}
>
> outputnya adalah:
>
> ~~~
> 10
> 19
> 2
> 22
> 6
> ~~~
> {: .output}
>
> Jika menjalankan `sort -n` pada input yang sama, maka kita mendapatkan:
>
> ~~~
> 2
> 6
> 10
> 19
> 22
> ~~~
> {: .output}
>
> Jelakan efek opsi `-n` pada kasus di atas.
>
> > ## Solution
> > The `-n` flag specifies a numeric sort, rather than alphabetical.
> {: .solution}
{: .challenge}

> ## Apa makna tanda `<`?
>
> Berpindahlah pada direktori `data-shell`. 
>
> Apa perbedaan antara:
>
> ~~~
> $ wc -l notes.txt
> ~~~
> {: .bash}
>
> dengan:
>
> ~~~
> $ wc -l < notes.txt
> ~~~
> {: .bash}
>
> > ## Solution
> > `<` is used to redirect input to a command. 
> >
> > In both examples, the shell returns the number of lines from the input to
> > the `wc` command.
> > In the first example, the input is the file `notes.txt` and the file name is
> > given in the output from the `wc` command.
> > In the second example, the contents of the file `notes.txt` are redirected to
> > standard input.
> > It is as if we have entered the contents of the file by typing at the prompt.
> > Hence the file name is not given in the output - just the number of lines.
> > Try this for yourself:
> >
> > ```
> > $ wc -l
> > this
> > is
> > a test
> > Ctrl-D # This lets the shell know you have finished typing the input
> > ```
> > {: .bash}
> >
> > ```
> > 3
> > ```
> > {: .output}
> {: .solution}
{: .challenge}

> ## Apa makna `>>` ?
>
> Apa perbedaan antara:
>
> ~~~
> $ echo hello > testfile01.txt
> ~~~
> {: .bash}
>
> dengan:
>
> ~~~
> $ echo hello >> testfile02.txt
> ~~~
> {: .bash}
>
> Hint: Try executing each command twice in a row and then examining the output files.
{: .challenge}

> ## More on Wildcards
>
> Sam has a directory containing calibration data, datasets, and descriptions of
> the datasets:
>
> ~~~
> 2015-10-23-calibration.txt
> 2015-10-23-dataset1.txt
> 2015-10-23-dataset2.txt
> 2015-10-23-dataset_overview.txt
> 2015-10-26-calibration.txt
> 2015-10-26-dataset1.txt
> 2015-10-26-dataset2.txt
> 2015-10-26-dataset_overview.txt
> 2015-11-23-calibration.txt
> 2015-11-23-dataset1.txt
> 2015-11-23-dataset2.txt
> 2015-11-23-dataset_overview.txt
> ~~~
> {: .bash}
>
> Before heading off to another field trip, she wants to back up her data and
> send some datasets to her colleague Bob. Sam uses the following commands
> to get the job done:
>
> ~~~
> $ cp *dataset* /backup/datasets
> $ cp ____calibration____ /backup/calibration
> $ cp 2015-____-____ ~/send_to_bob/all_november_files/
> $ cp ____ ~/send_to_bob/all_datasets_created_on_a_23rd/
> ~~~
> {: .bash}
>
> Help Sam by filling in the blanks.
>
> > ## Solution
> > ```
> > $ cp *calibration.txt /backup/calibration
> > $ cp 2015-11-* ~/send_to_bob/all_november_files/
> > $ cp *-23-dataset?.txt ~send_to_bob/all_datasets_created_on_a_23rd/
> > ```
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Piping Commands Together
>
> In our current directory, we want to find the 3 files which have the least number of
> lines. Which command listed below would work?
>
> 1. `wc -l * > sort -n > head -n 3`
> 2. `wc -l * | sort -n | head -n 1-3`
> 3. `wc -l * | head -n 3 | sort -n`
> 4. `wc -l *.* | sort -n | head -n 3`
>
> > ## Solution
> > Option 4 is the solution.
> > The pipe character `|` is used to feed the standard output from one process to
> > the standard input of another.
> > `>` is used to redirect standard output to a file.
> > Try it in the `data-shell/molecules` directory!
> {: .solution}
{: .challenge}

> ## Why Does `uniq` Only Remove Adjacent Duplicates?
>
> The command `uniq` removes adjacent duplicated lines from its input.
> For example, the file `data-shell/data/salmon.txt` contains:
>
> ~~~
> coho
> coho
> steelhead
> coho
> steelhead
> steelhead
> ~~~
> {: .source}
>
> Running the command `uniq salmon.txt` from the `data-shell/data` directory produces:
>
> ~~~
> coho
> steelhead
> coho
> steelhead
> ~~~
> {: .output}
>
> Why do you think `uniq` only removes *adjacent* duplicated lines?
> (Hint: think about very large data sets.) What other command could
> you combine with it in a pipe to remove all duplicated lines?
>
> > ## Solution
> > ```
> > $ sort salmon.txt | uniq
> > ```
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Pipe Reading Comprehension
>
> A file called `animals.txt` (in the `data-shell/data` folder) contains the following data:
>
> ~~~
> 2012-11-05,deer
> 2012-11-05,rabbit
> 2012-11-05,raccoon
> 2012-11-06,rabbit
> 2012-11-06,deer
> 2012-11-06,fox
> 2012-11-07,rabbit
> 2012-11-07,bear
> ~~~
> {: .source}
>
> What text passes through each of the pipes and the final redirect in the pipeline below?
>
> ~~~
> $ cat animals.txt | head -n 5 | tail -n 3 | sort -r > final.txt
> ~~~
> {: .bash}
> Hint: build the pipeline up one command at a time to test your understanding
{: .challenge}

> ## Pipe Construction
>
> For the file `animals.txt` from the previous exercise, the command:
>
> ~~~
> $ cut -d , -f 2 animals.txt
> ~~~
> {: .bash}
>
> produces the following output:
>
> ~~~
> deer
> rabbit
> raccoon
> rabbit
> deer
> fox
> rabbit
> bear
> ~~~
> {: .output}
>
> What other command(s) could be added to this in a pipeline to find
> out what animals the file contains (without any duplicates in their
> names)?
>
> > ## Solution
> > ```
> > $ cut -d , -f 2 animals.txt | sort | uniq
> > ```
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Removing Unneeded Files
>
> Suppose you want to delete your processed data files, and only keep
> your raw files and processing script to save storage.
> The raw files end in `.dat` and the processed files end in `.txt`.
> Which of the following would remove all the processed data files,
> and *only* the processed data files?
>
> 1. `rm ?.txt`
> 2. `rm *.txt`
> 3. `rm * .txt`
> 4. `rm *.*`
>
> > ## Solution
> > 1. This would remove `.txt` files with one-character names
> > 2. This is correct answer
> > 3. The shell would expand `*` to match everything in the current directory,
> > so the command would try to remove all matched files and an additional
> > file called `.txt`
> > 4. The shell would expand `*.*` to match all files with any extension,
> > so this command would delete all files
> {: .solution}
{: .challenge}

> ## Wildcard Expressions
>
> Wildcard expressions can be very complex, but you can sometimes write
> them in ways that only use simple syntax, at the expense of being a bit
> more verbose.  
> Consider the directory `data-shell/north-pacific-gyre/2012-07-03` :
> the wildcard expression `*[AB].txt`
> matches all files ending in `A.txt` or `B.txt`. Imagine you forgot about
> this.
>
> 1.  Can you match the same set of files with basic wildcard expressions
>     that do not use the `[]` syntax? *Hint*: You may need more than one
>     expression.
>
> 2.  The expression that you found and the expression from the lesson match the
>     same set of files in this example. What is the small difference between the
>     outputs?
>
> 3.  Under what circumstances would your new expression produce an error message
>     where the original one would not?
>
> > ## Solution
> > 1. 
> >
> > ```
> > $ ls *A.txt
> > $ ls *B.txt
> > ```
> > {: .bash}
> > 2. The output from the new commands is separated because there are two commands.
> > 3. When there are no files ending in `A.txt`, or there are no files ending in
> > `B.txt`.
> {: .solution}
{: .challenge}

> ## Which Pipe?
>
> The file `data-shell/data/animals.txt` contains 586 lines of data formatted as follows:
>
> ~~~
> 2012-11-05,deer
> 2012-11-05,rabbit
> 2012-11-05,raccoon
> 2012-11-06,rabbit
> ...
> ~~~
> {: .output}
>
> Assuming your current directory is `data-shell/data/`,
> what command would you use to produce a table that shows
> the total count of each type of animal in the file?
>
> 1.  `grep {deer, rabbit, raccoon, deer, fox, bear} animals.txt | wc -l`
> 2.  `sort animals.txt | uniq -c`
> 3.  `sort -t, -k2,2 animals.txt | uniq -c`
> 4.  `cut -d, -f 2 animals.txt | uniq -c`
> 5.  `cut -d, -f 2 animals.txt | sort | uniq -c`
> 6.  `cut -d, -f 2 animals.txt | sort | uniq -c | wc -l`
>
> > ## Solution
> > Option 5. is the correct answer.
> > If you have difficulty understanding why, try running the commands, or sub-sections of
> > the pipelines (make sure you are in the `data-shell/data` directory).
> {: .solution}
{: .challenge}

> ## Appending Data
>
> Consider the file `animals.txt`, used in previous exercise.
> After these commands, select the answer that
> corresponds to the file `animalsUpd.txt`:
>
> ~~~
> $ head -3 animals.txt > animalsUpd.txt
> $ tail -2 animals.txt >> animalsUpd.txt
> ~~~
> {: .bash}
>
> 1. The first three lines of `animals.txt`
> 2. The last two lines of `animals.txt`
> 3. The first three lines and the last two lines of `animals.txt`
> 4. The second and third lines of `animals.txt`
>
> > ## Solution
> > 3.
> {: .solution}
{: .challenge}
