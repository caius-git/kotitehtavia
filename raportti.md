# Tämän viikon raportti läksyistä!

#### b) git log --patch antaa meille seuraavan:

<br>
<br>

git log --patch
commit 2becc898b2eec927fd76908265d2786dadf05e2c (HEAD -> master)
Author: Caius Juvonen <caiusoj@gmail.com>
Date:   Thu Apr 16 18:38:21 2020 +0300

Eka muutos

diff --git a/raportti.md b/raportti.md
new file mode 100644
index 0000000..165e16c
--- /dev/null
+++ b/raportti.md
@@ -0,0 +1,3 @@
+# Tämän viikon raportti läksyistä!
+
+b) git log --ph

commit fadeeee57638450425300d464ff090a700e9303b (origin/master, origin/HEAD)
Author: caius-git <63778361+caius-git@users.noreply.github.com>
Date:   Thu Apr 16 18:32:08 2020 +0300

Initial commit

diff --git a/LICENSE b/LICENSE
new file mode 100644
index 0000000..f288702
--- /dev/null
+++ b/LICENSE
@@ -0,0 +1,674 @@
:

<br>
<br>

Seuraavaksi mietitään vaikka mitä git diff tekee, ja mitä voimme tehdä sen avulla. 
Dokumentaation perusteella se on komento joka näyttää erot committejen välillä. 
Poistuin hetkellisesti muokkaamasta tätä dokumenttia ja käytin heti git diff:
se näytti meille ne muutokset mitä olin tehnyt juuri tähän tiedostoon joita en ollut vielä
lisännyt 

	git add . 

avulla. Saimme tälläisen outputin:

<br>
<br>

git diff
diff --git a/raportti.md b/raportti.md
index cf63d53..1d7a5e5 100644
--- a/raportti.md
+++ b/raportti.md
@@ -33,3 +33,6 @@ index 0000000..f288702
 @@ -0,0 +1,674 @@
 :
 
+Seuraavaksi mietitään vaikka mitä git diff tekee, ja mitä voimme tehdä sen avulla. 
+Dokumentaation perusteella se on komento joka näyttää erot committejen välillä. 
+
 
<br>
<br>

#### Ainakin komento ihan itsenään ilman parametrejä näyttää vain ne muutokset mitä ei olla vielä lisätty.

<br>

git blame on viimeinen kohta. Kokeillana ensin sitä suoraan ilman parametrejä: 

Ei onnistu. Komento vaatii parametrit. Tutkitaan sitten että mitä. 
Näköjään git blame ainakin vaatii tiedostonimen parametriksi. Kokeillaan siis

	git blame raportti.md

