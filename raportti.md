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

Tämä siis näyttää eri "patcheja" mitä meillä on ollut, eli mitä ollaan committattu. Hyvä
muutosten seuraamiseen.

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

Tiedostossa lukee lähinnä background värien hex koodeja jotka kuvaa siis mitä väriä ne ovat. 
Voimme siis näitä muokkaamalla vaihtaa kyseistä väriä. Meillä on profiili [default] ja siinä
on background colorin väri kerrottu. 

Vaihoin terminaalin background väriä siniseksi, ja se toimi. Olemme siis pystyneet nyt todentamaaan,
että asetusten muokkaaminen config filessä myös oikeasti toimii. Meidän viimeinen ongelma on siinä,
että config filet terminaattoriin sijaitsevat käyttäjän home directoryssä. Ajattelin hieman
ohittaa tämän vaiheen tekemällä niin, että vaihamme etäorjan asetuksia manuaalisesti (etäorja on yksi
orjistani tässä ympäristössä, sen home directory /home/papu) ja laitamme asetukset myös /etc/skel
uusia käyttäjiä varten. 

Meidän pitää kuitenkin vielä kopioida nämä asetukset. Vaihdan ihan vain testimielessä terminatorin
backcgroundia uudestaan vaikka tällä kertaa vihreäksi, ja käytän limenvihreää eli värikoodia
9CFF33. Tämän jälkeen voimme kopioida config filen /srv/salt/terminator ja käyttää sitä sieltä.
Tämä onnistuu komennolla

	sudo cp config /srv/salt/terminator

juostuna config filen kansiosta, eli ./.config/terminator/

<br>


Joudumme vielä muokkaamaan init.sls tiedostoa /srv/salt/terminator jotta voimme varmistaa että
tämä uusi tiedosto tulee orjillemme sinne minne haluamme sen. Käytämme tähän file.managed. Tältä 
näyttää uusi init.sls: 

<br>

	terminator:
	  pkg.installed
	terminator_config:
	  file.managed:
	    - names:
	      - /etc/skel/.config/terminator/config 
	      - /home/papu/.config/terminator/config 
	    - source: salt://terminator/config

<br>

Varmistetaan että toimii ensin. Vaihdan /srv/salt/terminator/config vielä niin, että background
color on oikeasti violetti. Käytän värikoodia A233FF tätä varten. Sitten pistetään vain 

	sudo salt '*' state.apply 

ja katsotaan mitä tapahtuu. 

<br>

Muutokset eivät onnistuneet, ja tajusin kyllä heti miksi: eihän meidän koneella ole 
/home/papu directoryä! Tämän takiahan se ei toimi koska ei se löydä kyseistä tiedostoa. 
Hieman hämmentyneempi olen siitä että minkä takia se valittaa että /etc/skel parent directory 
ei ole olemassa. Tämänhän oli tarkoitus luoda sinne uudet kansiot! Menin hieman salt dokumentaatio
läpi, ja tajusin että eihän file.managed sitä tee, vaan tarvitsemme uuden kohdan tiedostoon 
file.directory:

<br>

Tehdäänpä uudet muutokset /srv/salt/terminator/init.sls ja katsotaan mitä tapahtuu: 

	terminator:
	  pkg.installed

	/etc/skel/.config/terminator/:
	  file.directory:
	    - makedirs: True

	terminator_config:
	  file.managed:
	    - name: 
	      - /etc/skel/.config/terminator/config
	    - source: salt://terminator/config


<br>

Tässä siis poistin ensinnäkin /home/papu/ init.sls ja siirrän sen kohta jonnekkin muualle, 
ja kerroin samalla saltille että se loisi uuden directoryn /etc/skel tuolla fire.directory toiminnon
avulla. Kokeillaan nyt toimiiko 

	sudo salt '*' state.apply

avulla. 

<br>

Melkein onnistui. Jos katsomme init.sls, huomaamme että siellä on pieni typo name:
kohdalla. Näiden pitäisi olla samalla rivillä. Muokataan tätä riviä näin:

	- name: /etc/skel/.config/terminator/config

ja katsotaan toimiiko se nyt. State apply uudestaan.

<br>

 Tällä kertaa salt ei kohdannut ongelmia,
joten tarkastetaan vielä tulokset. Ensinnäkin, /etc/skel/.config/terminator/config on nyt olemassa,
ja siellä on oikeat asetukset eli ne mitkä säädin. Seuraavaksi meidän pitää vain tehdä uusi
init.sls ihan vain etäorjaamme varten, jossa siis myös spesifioimme tuon home directory configin.

<br>

Minun on kyllä pakko myöntää, että en tykkää siitä että config filet sijaitsevat ihmisten home directoryissä.
Niitä on huomattavasti vaikeampi muokata niin että kaikki orjat saavat oikeat conf tiedostot. 
/etc/skel ratkaisee ongelman osittain, mutta se ei muokkaa ennestään olevien käyttäjien config.
tiedostoja. Luin kuitenkin myös jotain sellaisesta kuin $XDG_CONFIG_HOME/terminator pathista
terminatorin dokumentaatiossa, joten koitan vielä käyttää sitä file pathina ihan vain testinä. Mikäli
tämä ei toimi teen sen niinkuin ajattelin alussa. Lisäsin seuraavan tiedostooni, ihan vain testiksi:

	testi_config:
	  file.managed:
	    - name: $XDG_CONFIG_HOME/terminator/config
	    - source: salt://terminator/config
 
<br>

Katsotaan mitä tapahtuu. Mikäli tämä toimii, ratkaisee se ongelmani. Ikävä kyllä, salt sanoo
että tämä ei ole absoluuttinen polku, joten se ei käy. Tätä odotinkin. Poistetaan tämä ja tehdään niinkuin ajattelin normaalisti. 

<br>

Muokkasin vain init.sls hieman, eli lisäsin /home/papu sinne uudestaan. Muokkaan nyt sen vain top filestä
niin, että tämä init.sls pätee ainoastaan tähän oikeaan koneeseen. Joku IF statement olisi tässä 
varmaan ihan OK myös. Luin jotain dokumentaatio siitä ja vaikutti vähän turhan monimutkaiselta tässä
vaiheessa. Aika kuitenkin kokeilla toimiiko tämä meidän toisella orjalla, jolle ei olla vielä
edes asennettu terminaattoria. Muokkasin top filestä niin että tälle orjalle annetaan terminator
state. Nyt vain state.apply ja katsotaan mitä tapahtuu: 

<br>

Mahdollinen uusi ongelma löydetty? Terminator ei luonut asennuksen yhteydessä orjalle .config/terminator/config
kansiota. Sinänsä sillä ei ole väliä, sillä voimme vain käyttää aiemmin opimaamme file.directory. 
Muokataan init.sls tiedostoa näin:

	/home/papu/.config/terminator/:
	  file.directory: 
	    - makedirs: True

Kokeillaan uudestaan state.apply. 

<br>

Kaikki onnistui, ja salt myös loi config tiedoston meille. Saimme ihanan vihreän limen terminaalimme
backgroundiksi (en suosittele tätä värikoodia). Tässä olemme siis nyt pystyneet muokkaamaaan
terminator ohjelmaa niin, että saamme jokaiselle käyttäjällä limenvihreän terminator backgroundin
tahtoessamme. 

Caius Juvonen 2020

