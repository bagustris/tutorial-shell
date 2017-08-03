---
title: "Pipes dan Filters"
teaching: 20
exercises: 20
questions:
- "Bagaimana mengkombinasikan perintah-perintah yang ada untuk menghasilkan output baru yang diinginkan?"
objectives:
- "Redirect a command's output to a file."
- "Process a file instead of keyboard input using redirection."
- "Construct command pipelines with two or more stages."
- "Explain what usually happens if a program or pipeline isn't given any input to process."
- "Explain Unix's 'small pieces, loosely joined' philosophy."
keypoints:
- "`cat` menampilkan isi dari input/argumennya."
- "`head` menampilkan beberapa baris pertama dari inputnya."
- "`tail` menampilkan beberapa baris terakhir dari inputnya"
- "`sort` mensortir input."
- "`wc` menghitung baris, kata dan karakter dari input."
- "`*` mencocokkan nol atau lebih karakter dalam nama file, misal `*.txt` akan mencocokkan semua file yang berakhiran dengan ekstensi `.txt`."
- "`?` mencocokkan satu karakter apapun dalam nama file, misal `?.txt` akan mencocokkan dengan `a.txt` tetapi tidak cocok dengan`any.txt`."
- "`command > file` menyalurkan output perintah "command" ke dalam "file"."
- "`first | second` adalah sebuah pipeline: output dari first digunakan sebagai input dari second."
- "Cara terbaik menggunakan shell adalah dengan menggunakan pipes untuk mengcombinasika program single-purpose sederhana (filters)."
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
> `*` is a **wildcard**. It matches zero or more
> characters, so `*.pdb` matches `ethane.pdb`, `propane.pdb`, and every
> file that ends with '.pdb'. On the other hand, `p*.pdb` only matches
> `pentane.pdb` and `propane.pdb`, because the 'p' at the front only
> matches filenames that begin with the letter 'p'.
>
> `?` is also a wildcard, but it only matches a single character. This
> means that `p?.pdb` would match `pi.pdb` or `p5.pdb` (if we had these two
> files in the `molecules` directory), but not `propane.pdb`.
> We can use any number of wildcards at a time: for example, `p*.p?*`
> matches anything that starts with a 'p' and ends with '.', 'p', and at
> least one more character (since the `?` has to match one character, and
> the final `*` can match any number of characters). Thus, `p*.p?*` would
> match `preferred.practice`, and even `p.pi` (since the first `*` can
> match no characters at all), but not `quality.practice` (doesn't start
> with 'p') or `preferred.p` (there isn't at least one character after the
> '.p').
>
> When the shell sees a wildcard, it expands the wildcard to create a
> list of matching filenames *before* running the command that was
> asked for. As an exception, if a wildcard expression does not match
> any file, Bash will pass the expression as a parameter to the command
> as it is. For example typing `ls *.pdf` in the `molecules` directory
> (which contains only files with names ending with `.pdb`) results in
> an error message that there is no file called `*.pdf`.
> However, generally commands like `wc` and `ls` see the lists of
> file names matching these expressions, but not the wildcards
> themselves. It is the shell, not the other programs, that deals with
> expanding wildcards, and this is another example of orthogonal design.
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

> ## Output Halaman demi Halaman
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
> It's a very bad idea to try redirecting
> the output of a command that operates on a file
> to the same file. For example:
>
> ~~~
> $ sort -n lengths.txt > lengths.txt
> ~~~
> {: .bash}
>
> Doing something like this may give you
> incorrect results and/or delete
> the contents of `lengths.txt`.
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
The shell is actually just another program.
Under normal circumstances,
whatever we type on the keyboard is sent to the shell on its standard input,
and whatever it produces on standard output is displayed on our screen.
When we tell the shell to run a program,
it creates a new process
and temporarily sends whatever we type on our keyboard to that process's standard input,
and whatever the process sends to standard output to the screen.

Here's what happens when we run `wc -l *.pdb > lengths.txt`.
The shell starts by telling the computer to create a new process to run the `wc` program.
Since we've provided some filenames as parameters,
`wc` reads from them instead of from standard input.
And since we've used `>` to redirect output to a file,
the shell connects the process's standard output to that file.

If we run `wc -l *.pdb | sort -n` instead,
the shell creates two processes
(one for each process in the pipe)
so that `wc` and `sort` run simultaneously.
The standard output of `wc` is fed directly to the standard input of `sort`;
since there's no redirection with `>`,
`sort`'s output goes to the screen.
And if we run `wc -l *.pdb | sort -n | head -n 1`,
we get three processes with data flowing from the files,
through `wc` to `sort`,
and from `sort` through `head` to the screen.

![Redirects and Pipes](../fig/redirects-and-pipes.png)

This simple idea is why Unix has been so successful.
Instead of creating enormous programs that try to do many different things,
Unix programmers focus on creating lots of simple tools that each **do one job well**,
and that work well with each other.
This programming model is called "pipes and filters".
We've already seen pipes;
a **filter** is a program like `wc` or `sort`
that transforms a stream of input into a stream of output.
Almost all of the standard Unix tools can work this way:
unless told to do otherwise,
they read from standard input,
do something with what they've read,
and write to standard output.

The key is that any program that reads lines of text from standard input
and writes lines of text to standard output
can be combined with every other program that behaves this way as well.
You can *and should* write your programs this way
so that you and other people can put those programs into pipes to multiply their power.

> ## Redirecting Input
>
> As well as using `>` to redirect a program's output, we can use `<` to
> redirect its input, i.e., to read from a file instead of from standard
> input. For example, instead of writing `wc ammonia.pdb`, we could write
> `wc < ammonia.pdb`. In the first case, `wc` gets a command line
> parameter telling it what file to open. In the second, `wc` doesn't have
> any command line parameters, so it reads from standard input, but we
> have told the shell to send the contents of `ammonia.pdb` to `wc`'s
> standard input.
{: .callout}

## Nelle's Pipeline: Checking Files

Nelle has run her samples through the assay machines
and created 1520 files in the `north-pacific-gyre/2012-07-03` directory described earlier.
As a quick sanity check, starting from her home directory, Nelle types:

~~~
$ cd north-pacific-gyre/2012-07-03
$ wc -l *.txt
~~~
{: .bash}

The output is 1520 lines that look like this:

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

Now she types this:

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

Whoops: one of the files is 60 lines shorter than the others.
When she goes back and checks it,
she sees that she did that assay at 8:00 on a Monday morning --- someone
was probably in using the machine on the weekend,
and she forgot to reset it.
Before re-running that sample,
she checks to see if any files have too much data:

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

Those numbers look good --- but what's that 'Z' doing there in the third-to-last line?
All of her samples should be marked 'A' or 'B';
by convention,
her lab uses 'Z' to indicate samples with missing information.
To find others like it, she does this:

~~~
$ ls *Z.txt
~~~
{: .bash}

~~~
NENE01971Z.txt    NENE02040Z.txt
~~~
{: .output}

Sure enough,
when she checks the log on her laptop,
there's no depth recorded for either of those samples.
Since it's too late to get the information any other way,
she must exclude those two files from her analysis.
She could just delete them using `rm`,
but there are actually some analyses she might do later where depth doesn't matter,
so instead, she'll just be careful later on to select files using the wildcard expression `*[AB].txt`.
As always,
the `*` matches any number of characters;
the expression `[AB]` matches either an 'A' or a 'B',
so this matches all the valid data files she has.

> ## What Does `sort -n` Do?
>
> If we run `sort` on this file:
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
> the output is:
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
> If we run `sort -n` on the same input, we get this instead:
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
> Explain why `-n` has this effect.
>
> > ## Solution
> > The `-n` flag specifies a numeric sort, rather than alphabetical.
> {: .solution}
{: .challenge}

> ## What Does `<` Mean?
>
> Change directory to `data-shell` (the top level of our downloaded example data).
>
> What is the difference between:
>
> ~~~
> $ wc -l notes.txt
> ~~~
> {: .bash}
>
> and:
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

> ## What Does `>>` Mean?
>
> What is the difference between:
>
> ~~~
> $ echo hello > testfile01.txt
> ~~~
> {: .bash}
>
> and:
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
