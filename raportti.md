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

Se näytti meille kaikki muutokset mitä olin tehnyt raportti.md, kuka ne teki, ja milloin. 
Vaikuttaa ihan hyödylliseltä tiedostojen muokkaajien selvittämiseksi. Myös komennon nimen
mukaisesti tällä voi syyttää jotain toista henkilö mikäli he tekevät huonoja muutoksia jotka
rikkoo softan :)

<br>
<br>

#### Tässä kohtaa pitää tehdä git reset --hard tyhmien muutosten jälkeen. 

Turhaa spammia tähän:

<br>

No, en ollut ihan varma miten tulokset pitäisi demota mutta laitoin tuohon turhaa spammia alle 
kaikenmaailman sotkua, tein jopa git add . ja sitten kirjoitin git reset --hard, ja kaikki spam 
katosi vaikka olin ehtinyt kirjoittaa git add . Mikään ennen tätä ei kuitenkaan kadonnut. 
Tällä siis saa kaiken mikä on tehty ennen committia katoamaan. 

<br>
<br>

#### d) Uusi salt-moduuli. Tässä kohtaa pitää asentaa taas uusi ohjelma ja tehdä asetuksia sille. 

<br>

Päätin asentaa terminatorin ja vaihtaa sen default backgroundinvärin joksikin muuksi. 
Teen /srv/salt/ uuden kansion nimeltä terminator ja teen sen sisälle init.sls tiedoston. Sen sisälle kirjoitan:

	terminator:
	  pkg.installed

<br>

Seuraavaksi liikutaan top.sls /srv/salt kansiossa ja lisätään sinne vielä 
toiselle orjalle tämä moduuli. Top.sls näyttää about tältä tässä kohtaa:

	base:
	  'papunorja':
	    - terminator

<br>

Sitten vedetään vain sudo salt '*' state.apply ja pääsemme asentamaan terminatorin.
Tämän jälkeen meidän pitäisi keksiä miten terminatorin terminaalin backgroundin värin voi muuttaa. 

Terminaatorista voi muokkaa terminaalin background color right clikkaamalla ja sitten
preferences -> sieltä voi löytää asetukset terminaalin background värille. Muokataan niitä,
ja katotaan mitä tiedostoja muuttuu:

	find -printf '%T+ %p\n' | sort
 
<br>

Komento siis näyttää viimeiseksi muokattuja tiedostoja. Näemme että olemme muokanneet tiedostoa
./.config/terminator/config . Kuulostaa mielenkiintoiselta. Käydään katsomassa mitä siellä tapahtuu. 

<br>

Tiedostossa lukee lähinnä background värien rbg koodeja jotka kuvaa siis mitä väriä ne ovat. 
Voimme siis näitä muokkaamalla vaihtaa kyseistä väriä. Meillä on profiili [default] ja siinä
on background colorin väri kerrottu. 
