---
title: "Mencari File"
teaching: 25
exercises: 15
questions:
- "Bagaimana cara menemukan file?"
- "Bagaimana mencari kata kunci pada file?"
- "Bagaimana saya mengetahui lokasi dari file/direktori/perintah pada komputer saya?"
objectives:
- "Gunakan `grep` untuk mencari baris yang polanya cocok dengan yang dicari."
- "Gunakan `find` untuk mencari file yang polanya sesuai dengan yang diinginkan."
- "Gunakan `locate` atau `whereis` untuk menemukan lokasi dari sebuah file atau direktori pada komputer."
- "Gunakan output dari suatu perintah sebagai argumen dari perintah lainnya."
- "Jelaskan apa maksud dari 'text' dan 'binary' files dan kenapa kebanyakan tools tidak bisa menangani 'binary' files dengan sangat baik."
keypoints:
- "`find` untuk mencari file sesuai kata kunci/pola"
- "`grep` untuk memilih baris yang cocok dengan pencarian."
- "`--help` digunakan untuk menampilkan informasi yang terkandun pada perintah (di depannya)."
- "`man command` digunakan untuk menampilkan manual (`manpages`) dari suatu perintah."
- "`$(command)` memasukkan output dari perintah"
---

Dengan cara yang sama dimana sebagaian besar dari kita 
perngah "meng-google" untuk mencari tahu sesuatu, maka Unix/Linux 
shell pun juga memiliki kemampuan serupa (Jauh sebelum Google ada). 

Ada beberapa cara untuk **mencari** pada shell Linux: `grep`, 
`find`, `locate` dan `whereris`. Mungkin masih ada yang lainnya, 
namun setidaknya itu yang sering kami gunakan.

`grep` merupakan tools yang ampuh untuk mencari kata atau frasa. 
Perintah tersebut merupakan kepanjangan dari "global/regular expression/print",
sebuah operasi sekuensial dalam text editor Unix tahap awal yang 
juga menjadi perintah yang sangat berguna pada command-line.

`grep` menemukan dan mencetak baris dari file yang memiliki pola berkesesuaian.  
Sebagai contoh, kita agak menggunakan file yang berisi haiku (puisi singkat) yang 
diambil dari majalah *Salon*. Untuk contoh kasus ini kita akan berada pada 
subdirektori `writing`.

~~~
$ cd
$ cd writing
$ cat haiku.txt
~~~
{: .bash}

~~~
The Tao that is seen
Is not the true Tao, until
You bring fresh toner.

With searching comes loss
and the presence of absence:
"My Thesis" not found.

Yesterday it worked
Today it is not working
Software is like that.
~~~
{: .output}

> ## Forever, or Five Years
>
> Kita tidak menyebutkan sumber dari Haiku diatas karena link dari majalah *Salon*'s telah mati,
> Seperti  [dikatakan Jeff Rothenberg](http://www.clir.org/pubs/archives/ensuring.pdf),
> "Digital information lasts forever --- or five years, whichever comes first."
{: .callout}

Sekarang kita cari kata "not" dalam hakiku tersebut.

~~~
$ grep not haiku.txt
~~~
{: .bash}

~~~
Is not the true Tao, until
"My Thesis" not found
Today it is not working
~~~
{: .output}

Disini, `not` adalah pola (string) yang kita cari. Perintah 
grep akan mencari [string](https://en.wikipedia.org/wiki/String_(computer_science)) `not` dalam file `haiku.txt`. 
Hasilnya adalah ketiga baris yang ditampilkan mengandung kata `not` diatas.

Mari kita cari dengan pola lainnya: "The".

~~~
$ grep The haiku.txt
~~~
{: .bash}

~~~
The Tao that is seen
"My Thesis" not found.
~~~
{: .output}

Kali ini, dua baris yang mengandung kata "The" ditampilkan. Bagaimanapun, 
kata "Thesis" juga ditampilkan, padahal kita tidak mencarinya.

Untuk membatasi hanya kata "The" saja, kita bisa menambahkan opsi `-w` sebagai berikut.

~~~
$ grep -w The haiku.txt
~~~
{: .bash}

~~~
The Tao that is seen
~~~
{: .output}

Opsi `-w` merupakan kepanjangan dari "word boundary" yang akan mencari pola yang dibatasi 
oleh spasi. Grep juga bisa digunakan untuk mencari frasa atau kalimat, yakni 
dengan menambahkan tanda dobel quote sebelum dan sesudah pola yang kita cari.

~~~
$ grep -w "is not" haiku.txt
~~~
{: .bash}

~~~
Today it is not working
~~~
{: .output}


Untuk satu kata pencarian, kita tidak perlu menggunakan tanda dobel quote. 
Namun, akan sangat membantu jika kita menggunakannya untuk mencari beberapa kata, 
yakni dengan memasukkan frase atau kalimat diantara dobel quote.
Tanda kutip ganda (*double quote*) ini juga membantu untuk membedakan antara 
pola pencarian dan frasa yang dicari.

Jadi pakailah tanda kutip ganda ini pada pola pencarian dengan grep. Ospi lainnya 
yang sangat berguna adalah menambahkan `-n`. Ini akan menampilkan nomor baris 
pada hasil pencarian.

~~~
$ grep -n "it" haiku.txt
~~~
{: .bash}

~~~
5:With searching comes loss
9:Yesterday it worked
10:Today it is not working
~~~
{: .output}

Di atas ditampilkan baris 5, 9, dan 10 yang mengandung huruf "it".
Opsi-opsi tersebut dapat digabungkan seperti berikut.

~~~
$ grep -n -w "the" haiku.txt
~~~
{: .bash}

atau:

~~~
$ grep -n -w "the" haiku.txt
~~~
{: .bash}

Outputnya,
~~~
2:Is not the true Tao, until
6:and the presence of absence:
~~~
{: .output}

Gunakan opsi `-i` agar pola pencarian menjadi *case-insensitive*:

~~~
$ grep -n -w -i "the" haiku.txt
~~~
{: .bash}

~~~
1:The Tao that is seen
2:Is not the true Tao, until
6:and the presence of absence:
~~~
{: .output}

Untuk mencari dengan pola **tidak mengandung** maka dapat digunakan `-v`. 
Contohnya kita ingin mencari baris yang **tidak mengandung** kata "the".

~~~
$ grep -n -w -v "the" haiku.txt
~~~
{: .bash}

~~~
1:The Tao that is seen
3:You bring fresh toner.
4:
5:With searching comes loss
7:"My Thesis" not found.
8:
9:Yesterday it worked
10:Today it is not working
11:Software is like that.
~~~
{: .output}

Masih banyak opsi yang dimiliki oleh, silahkan dieksplore dengan bantuan `--help`.

~~~
$ grep --help
~~~
{: .bash}

~~~
Usage: grep [OPTION]... PATTERN [FILE]...
Search for PATTERN in each FILE or standard input.
PATTERN is, by default, a basic regular expression (BRE).
Example: grep -i 'hello world' menu.h main.c

Regexp selection and interpretation:
  -E, --extended-regexp     PATTERN is an extended regular expression (ERE)
  -F, --fixed-strings       PATTERN is a set of newline-separated fixed strings
  -G, --basic-regexp        PATTERN is a basic regular expression (BRE)
  -P, --perl-regexp         PATTERN is a Perl regular expression
  -e, --regexp=PATTERN      use PATTERN for matching
  -f, --file=FILE           obtain PATTERN from FILE
  -i, --ignore-case         ignore case distinctions
  -w, --word-regexp         force PATTERN to match only whole words
  -x, --line-regexp         force PATTERN to match only whole lines
  -z, --null-data           a data line ends in 0 byte, not newline

Miscellaneous:
...        ...        ...
~~~
{: .output}

> ## Wildcards
>
> `grep`'s real power doesn't come from its options, though; it comes from
> the fact that patterns can include wildcards. (The technical name for
> these is **regular expressions**, which
> is what the "re" in "grep" stands for.) Regular expressions are both complex
> and powerful; if you want to do complex searches, please look at the lesson
> on [our website](http://v4.software-carpentry.org/regexp/index.html). As a taster, we can
> find lines that have an 'o' in the second position like this:
>
> ~~~
> $ grep -E '^.o' haiku.txt
> ~~~
> {: .bash}
>
> ~~~
> You bring fresh toner.
> Today it is not working
> Software is like that.
> ~~~
> {: .output}
>
> We use the `-E` flag and put the pattern in quotes to prevent the shell
> from trying to interpret it. (If the pattern contained a `*`, for
> example, the shell would try to expand it before running `grep`.) The
> `^` in the pattern anchors the match to the start of the line. The `.`
> matches a single character (just like `?` in the shell), while the `o`
> matches an actual 'o'.
{: .callout}

## FIND
Jika `grep` digunakan untuk mencari baris dalam file, maka `find` 
digunakan untuk menemukan file itu sendiri. Seperti halnya `grep` 
`find` juga memiliki banyak opsi.

![File Tree for Find Example](../fig/find-file-tree.svg)


Direktori `writing` berisi satu file yakni `haiku.txt` dan empat subdirektori:
- `thesis` (berisi file kosong, `empty-draft.md`),
- `data` (berisi dua file `one.txt` dan `two.txt`),
- `tools` (berisi program `format` dan `stats`),
- `old`, (berisi sebuah file, `oldtool`).

Perintah pertama jalankan adalah sebagai berikut.

~~~
$ find .
~~~
{: .bash}

~~~
.
./old
./old/.gitkeep
./data
./data/one.txt
./data/two.txt
./tools
./tools/format
./tools/old
./tools/old/oldtool
./tools/stats
./haiku.txt
./thesis
./thesis/empty-draft.md
~~~
{: .output}

Seperti kita ketahui, tanda titik satu, `.`, merupakan 
simbol untuk direktori saat ini, jadi kita mencari apa saja 
pada direktori saat ini.

Saking banyaknya opsi yang dimiliki oleh `find`, kita tidak 
akan mengkover semuanya, namun beberapa yang penting dan praktis digunakan. 
Opsi pertama yang akan kita gunakan adalah `-type d` yang artinya mencari sesuatu 
(file atau direktori) yang direktori saja. Maka ls akan menampilkan hasil berikut, 
termasuk direktori saat ini, `./`.

~~~
$ find . -type d
~~~
{: .bash}

~~~
./
./old
./data
./thesis
./tools
./tools/old
~~~
{: .output}

Hasil pencarian di atas tidak urut (tentu saja bisa kita urutkan dengan pipe dan `sort`!). 
Jika kita ingin mencari file, maka tinggal ganti saja opsi `-d` dengan `-f` seperti berikut.

~~~
$ find . -type f
~~~
{: .bash}

~~~
./haiku.txt
./tools/stats
./tools/old/oldtool
./tools/format
./thesis/empty-draft.md
./data/one.txt
./data/two.txt
~~~
{: .output}

Sekarang kita gunakan opsi yang sangat berguna yakni **`-name`**.

~~~
$ find . -name *.txt
~~~
{: .bash}

~~~
./haiku.txt
~~~
{: .output}

Kita mengharapkan semua file yang berekstensi `.txt`, namun 
yang kita dapatkan hanya satu file saja yakni `haiku.txt`. 
Padahal ada file lainnya juga yang berekstensi `.txt` juga.
Mengapa hanya satu yang ditemukan? Permasalahannya adalah shell 
mencari karakter wildcard **SEBELUM** perintah dijalankan, bukan setelahnya. 
Jadi sebenarnya, perintah di atas mengeksekusi perintah seperti ini.

~~~
$ find . -name haiku.txt
~~~
{: .bash}

Untuk mencari semua file yang berekstensi `.txt`, maka tambahkan tanda 
single quote diantara pola yang dicari.

~~~
$ find . -name '*.txt'
~~~
{: .bash}

~~~
./data/one.txt
./data/two.txt
./haiku.txt
~~~
{: .output}

> ## Listing vs. Finding
>
> `ls` dan `find` bisa dipakai untuk menghasilkan output yang mirip,
> contohnya adalah `ls *.pdb` dan `find -name '*.pdb'`.
> Namun dalam keadaan normal (dan secara prinsip), `ls` digunakan untuk
> mendaftar file/direktori, sedangkan `find` digunakan untuk mencarinya 
> sesuai pola yang diberikan.
{: .callout}

Seperti yang dikatakan sebelumnya, 
kekuatan perintah shell adalah dengan mengkombinasikan antar tools,
seperti yang kita lakukan dengan pipes. Sekarang kita coba contoh lainnya.

Seperti yang telah kita lihat, `find . -name '*.txt'` 
meberikan sebuah daftar file yang berekstensi ".txt" pada direktori dibawahnya.
Bagaimana jika dikombinasikan dengan perintah `sc -l` untuk menunjukkan jumlah barisnya?

Begini contohnya, dengan meletakkan `find` dalam `$()`:

~~~
$ wc -l $(find . -name '*.txt')
~~~
{: .bash}

~~~
11 ./haiku.txt
300 ./data/two.txt
70 ./data/one.txt
381 total
~~~
{: .output}

Mudah bukan? hanya dengan menambahkan tanda dolar, 
maka text setelah tanda dolar itu, dalam kurung, 
akan dieksekusi sebagai perintah. Artinya, 
output perintah dalam kurung tersebut akan menjadi 
input untuk `wc -l` secara sekuensial (per file).
Sehingga perintah di atas ekivalen dengan perintah berikut.

~~~
$ wc -l ./data/one.txt ./data/two.txt ./haiku.txt
~~~
{: .bash}

Ini berbeda dengan perintah:
~~~
$ find . -name '*.txt' | wc -l
~~~
(: .bash)

dimana outputnya sebagai berikut.
~~~
2
~~~
(: .output)

Karena hasil dari `find` hanya dua baris, maka output `wc -l` adalah angka 2.


Perintah `grep` dan `find` dapat dikombinasikan untuk mencari file tertentu, 
kemudian mencari kata/frasa dalam file tersebut.
Sebagai contoh, kita ingin mencari string "FE" pada fila yang berekstensi `*.pdb`.

~~~
$ grep "FE" $(find .. -name '*.pdb')
~~~
{: .bash}

~~~
../data/pdb/heme.pdb:ATOM     25 FE           1      -0.924   0.535  -0.518
~~~
{: .output}

> ## Binary Files
>
> 
> Selama ini kita hanya fokus pada file text, mencari, menemukan pola, 
> jumlah baris dan hal lainnya. Bagaimana dengan type file lain seperti 
> gambar, musik, video dll? Grep tidak bisa mencari pola
> dalam tipe file lain selain teks.
> Opsi pertama, adalah membuat tools lain (juga dengan shell) 
> yang mampu membaca format selain teks tadi. Hal ini akan sulit
> karena akan sangat banyak format yang harus didukung. 
>
> Ospi kedua, kita bisa "mengekstrak" informasi teks dari format lain tersebut.
> Contoh untuk data suara/musik, kita bisa mengekstrak header file yang 
> berisi nama file, judul lagu, panjang data/waktu, dan informasi lainnya. 
> Untuk data gambar, kita bisa mengekstrak ukuran pixel, panjang dan lebar, 
> author serta informasi lainnya. Untuk format lain sperti spreadsheet,
> akan sulit untuk mengekstrak hasil dari formula matematika.
> 
> Opsi ketiga, yang paling banyak dipakai dan merupakan kekuatan shell, 
> adalah mengkombinasikan dengan bahasa pemrograman. Python bisa membaca 
> data apa saja. Begitu pula C/C++, Octave/Matlab dll. Disini perananan
> Shell sebagai perekat (glue) antar bahasa pemrograman sangat berguna sekali.
{: .callout}

Shell Unix kini berumur lebih tua dari siapapun yang menggunakannya. 
Kenapa bisa bertahan sampai sekarang? Produktifitas. Shell Unix membuat 
pemakainya menjadi produktif (dan kreatif) meskipun banyak tools lain 
yang lebih mudah dan kompleks. Fleksibilitas Shell Unix membuat penggunanya 
untuk dapat melakukan apa saja yang diinginkan, seperti halnya yang dicontohkan 
pada tutorial ini. Bayangkan jika anda ingin mencari frasa tertentu dalam 1000 file Word 
dengan GUI (graphical user interface). Sangat tidak efisien, dan anda membutuhkan tool 
lain (baru) agar lebih mudah mencarinya. Namun, dengan dalam Unix/Linux, 
semuanya tetap bisa dilakukan, dengan shell. Pengguna Shell Unix merupakan 
kunci dari keberlangsungan Shell, seperti yang dikatakan Alfred
North Whitehead (1911), "Civilization advances by extending the
number of important operations which we can perform without thinking
about them." Shell, khususnya Bash, juga terus dikembangkan dari tahun ke tahun.

### `locate` dan `which` 
Sebagai perbandingan, tidak hanya `find` dapat digunakan untuk menemukan file,
tapi kita juga bisa menggunakan `locate` dan `which`. Bagaimana
cara menggunakannya...? Berikut penjelasan singkatnya.

#### locate
Menurut `man page`, `locate` merupakan perintah untuk menemukan file
berdasarkan nama file. Contoh:
> ~~~
> $ locate octave-cli
> /usr/local/bin/octave-cli
> /usr/local/bin/octave-cli-4.2.1
> /usr/local/share/man/man1/octave-cli.1
> ~~~
> {: .bash}

#### which
`which`, berbeda dengan `find` dan `locate` berfungsin untuk
menemukan lokasi dari suatu perintah, bukan file.
Contohnya adalah sebagai berikut:
> ~~~
> $ which gcc
> /usr/bin/gcc
> ~~~
> {: .bash}

### KUIS 

> ## Using `grep`
>
> Referring to `haiku.txt`
> presented at the begin of this topic,
> which command would result in the following output:
>
> ~~~
> and the presence of absence:
> ~~~
> {: .output}
>
> 1. `grep "of" haiku.txt`
> 2. `grep -E "of" haiku.txt`
> 3. `grep -w "of" haiku.txt`
> 4. `grep -i "of" haiku.txt`
>
> > ## Solution
> > The correct answer is 3, because the `-w` flag looks only for whole-word matches.
> > The other options will all match "of" when part of another word.
> {: .solution}
{: .challenge}

> ## `find` Pipeline Reading Comprehension
>
> Write a short explanatory comment for the following shell script:
>
> ~~~
> wc -l $(find . -name '*.dat') | sort -n
> ~~~
> {: .bash}
>
> > ## Solution
> > 1. Find all files with a `.dat` extension in the current directory
> > 2. Count the number of lines each of these files contains
> > 3. Sort the output from step 2. numerically
> {: .solution}
{: .challenge}

> ## Matching and Subtracting
>
> The `-v` flag to `grep` inverts pattern matching, so that only lines
> which do *not* match the pattern are printed. Given that, which of
> the following commands will find all files in `/data` whose names
> end in `s.txt` (e.g., `animals.txt` or `planets.txt`), but do
> *not* contain the word `net`?
> Once you have thought about your answer, you can test the commands in the `data-shell`
> directory.
>
> 1.  `find data -name '*s.txt' | grep -v net`
> 2.  `find data -name *s.txt | grep -v net`
> 3.  `grep -v "temp" $(find data -name '*s.txt')`
> 4.  None of the above.
>
> > ## Solution
> > The correct answer is 1. Putting the match expression in quotes prevents the shell
> > expanding it, so it gets passed to the `find` command.
> >
> > Option 2 is incorrect because the shell expands `*s.txt` instead of passing the wildcard
> > expression to `find`.
> >
> > Option 3 is incorrect because it searches the contents of the files for lines which
> > do not match "temp", rather than searching the file names.
> {: .solution}
{: .challenge}

> ## Tracking a Species
> 
> Leah has several hundred 
> data files saved in one directory, each of which is formatted like this:
> 
> ~~~
> 2013-11-05,deer,5
> 2013-11-05,rabbit,22
> 2013-11-05,raccoon,7
> 2013-11-06,rabbit,19
> 2013-11-06,deer,2
> ~~~
> {: .source}
>
> She wants to write a shell script that takes a species as the first command-line argument 
> and a directory as the second argument. The script should return one file called `species.txt` 
> containing a list of dates and the number of that species seen on each date.
> For example using the data shown above, `rabbits.txt` would contain:
> 
> ~~~
> 2013-11-05,22
> 2013-11-06,19
> ~~~
> {: .source}
>
> Put these commands and pipes in the right order to achieve this:
> 
> ~~~
> cut -d : -f 2  
> >  
> |  
> grep -w $1 -r $2  
> |  
> $1.txt  
> cut -d , -f 1,3  
> ~~~
> {: .bash}
>
> Hint: use `man grep` to look for how to grep text recursively in a directory
> and `man cut` to select more than one field in a line.
>
> An example of such a file is provided in `data-shell/data/animal-counts/animals.txt`
>
> > ## Solution
> >
> > ```
> > grep -w $1 -r $2 | cut -d : -f 2 | cut -d , -f 1,3  > $1.txt
> > ```
> > {: .source}
> >
> > You would call the script above like this:
> >
> > ```
> > $ bash count-species.sh bear .
> > ```
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Little Women
>
> You and your friend, having just finished reading *Little Women* by
> Louisa May Alcott, are in an argument.  Of the four sisters in the
> book, Jo, Meg, Beth, and Amy, your friend thinks that Jo was the
> most mentioned.  You, however, are certain it was Amy.  Luckily, you
> have a file `LittleWomen.txt` containing the full text of the novel
> (`data-shell/writing/data/LittleWomen.txt`).
> Using a `for` loop, how would you tabulate the number of times each
> of the four sisters is mentioned?
>
> Hint: one solution might employ
> the commands `grep` and `wc` and a `|`, while another might utilize
> `grep` options.
> There is often more than one way to solve a programming task, so a
> particular solution is usually chosen based on a combination of
> yielding the correct result, elegance, readability, and speed.
>
> > ## Solutions
> > ```
> > for sis in Jo Meg Beth Amy
> > do
> > 	echo $sis:
> >	grep -ow $sis littlewomen.txt | wc -l
> > done
> > ```
> > {: .source}
> >
> > Alternative, slightly inferior solution:
> > ```
> > for sis in Jo Meg Beth Amy
> > do
> > 	echo $sis:
> >	grep -ocw $sis LittleWomen.txt
> > done
> > ```
> > {: .source}
> >
> > This solution is inferior because `grep -c` only reports the number of lines matched.
> > The total number of matches reported by this method will be lower if there is more
> > than one match per line.
> {: .solution}
{: .challenge}

> ## Finding Files With Different Properties
> 
> The `find` command can be given several other criteria known as "tests"
> to locate files with specific attributes, such as creation time, size,
> permissions, or ownership.  Use `man find` to explore these, and then
> write a single command to find all files in or below the current directory
> that were modified by the user `ahmed` in the last 24 hours.
>
> Hint 1: you will need to use three tests: `-type`, `-mtime`, and `-user`.
>
> Hint 2: The value for `-mtime` will need to be negative---why?
>
> > ## Solution
> > Assuming that Nelle’s home is our working directory we type:
> >
> > ~~~
> > $ find ./ -type f -mtime -1 -user ahmed
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}



Bacaan:
- [How To Find A File In Linux Using The Command Line](https://www.lifewire.com/uses-of-linux-command-find-2201100)
- [Howtoforge: How to use locate `command`](https://www.howtoforge.com/linux-locate-command/)
