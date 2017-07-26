---
layout: page
title: Setup
permalink: /setup/
---


Anda membutuhkan komputer berbasis Linux untuk mempraktekkan tutorial ini. Sangat direkomendasikan memakai [Ubuntu](https://ubuntu.com) ataupun varian Unix yang lain seperti: MacOS, BSD, dan distro Linux lainnya: Fedora, Open SUSE, Arch, Gentoo. Menggunakan Git for Windows mungkin berjalan untuk beberapa perintah, namun pada beberapa kasus seperti `man` tidak bisa dijalankan.

Anda membutuhkan file berikut untuk tutorial ini:

1. Download [shell-tutorial-data.zip]({{ page.root }}/data/shell-tutorial-data.zip) dan pindahkan ke /home direktori anda.
2. Ekstrak file tersebut, bisa gunakan klik kanan >> extract. Atau jika anda cukup berani gunakan perintah berikut, `unzip shell-tutorial-data.zip` di direktori tempat anda berada (current directory).
3. Buka terminal dan ketikkan:

~~~
$ cd
~~~
{: .bash}

Pada tutorial ini anda akan belajar cara mengakses file dan folder.

Cobalah beberapa perintah berikut untuk belajar:
~~~
$ help cd
$ cd .
$ cd ..
$ cd ~
$ cd - 
~~~
{: .bash}
