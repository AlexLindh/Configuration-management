# h3 - Soitto kotiin

## x) Lue ja tiivistä

## a) Hello Vagrant ja VirtualBox!
- Aloitin vagrantin asentamisen (Karvinen vuosi) ja (salminen vuosi) ohjeiden mukaan isäntäjärjestelmään, koska vinkeissä oli kerrottu, että vagrant toimii huonosti virtuaalikoneen sisällä. Vagrantin asennusohjelman löysin vagrantin omilta verkkosivuilta: https://developer.hashicorp.com/vagrant/downloads.
- Vagrantin ladattua ja velhon asennettua käynnistin koneen uudelleen ja testasin, että CMD:llä, että vagrant oikeasti asentui.

  <img width="1004" height="526" alt="kuva" src="https://github.com/user-attachments/assets/a3fa6d0c-6d2e-4597-aa6f-1840a0d66ccf" />

- VirtualBox oli jo asennettuna, eikä asennuksessa ollut mitään ongelmia!

  <img width="1004" height="669" alt="kuva" src="https://github.com/user-attachments/assets/3acbd5d8-f8e3-4f5e-afc6-cfaf04b297cc" />

## b & c) Uusien virtuaalikoneiden luonti vagrantilla ja verkon testaaminen
- Uusien virtuaalikoneiden luomisen aloitin ensin luomalla hakemiston ~/twohost vagrantille minne luon Vagrantfilen komennolla '$ vagrant init'
- Seuraavaksi aloin muokkaamaan juuri luomaa Vagrantfile tiedostoa komennolla: '$ notepad Vagrantfile' ja kopioin (karvinen vuosi) ohjeista tiedoston sisällön ja muokkasin ip-osoitteita niin, että vaihdoin 192.168.88.101/102 -> 192.168.56.101/102, koska alkuperäinen ei ollut sallittu alue, joka tuli vastaan ekalla kerralla, kun yritin käynnistää vagranttia.

  <img width="1004" height="726" alt="kuva" src="https://github.com/user-attachments/assets/22f80560-fcb8-49c2-bc23-2fcb45787fe5" />

- Käynnistin vagrantin komennolla '$ vagrant up' -hakemistossa /twohost missä juuri luoma Vagrantfile on.
- Muutaman minuutin jälkeen vagrant loi ja käynnisti 2 uutta linux virtuaalikonetta nimillä t001 ja t002, mitkä oli vagrantfile tiedostossa määritetty.

<img width="1004" height="231" alt="kuva" src="https://github.com/user-attachments/assets/837e2374-474c-491a-8483-082c125425a6" />

- Otin koneisiin yhteyden komennoilla '$ vagrant ssh t001/t002'. Tässä esimerkissä t001 toimii master -koneena ja t002 orja -koneena.
- Heti kun yhdistin koneisiin testasin verkkoa pingaamalla koneilla toisiaan.

  <img width="1004" height="277" alt="kuva" src="https://github.com/user-attachments/assets/07670f60-142c-4cb2-bffd-f9c5a1c2a0e5" />

- Pingaus toimi moitteettomasti!

## d) Herra-orja arkkitehtuuri verkon yli
- Aloitin asentamalla molemmille koneille saltin (karvinen vuosi) ohjeiden mukaisesti. Saltin asennuksessa ei ollut ongelmia. Asensin t001 salt-masterin ja t002 salt-minion.
- Seuraavaksi orja koneella komennolla '$ sudoedit /etc/salt/minion' loin tiedoston mihin lisässin master koneen ip-osoitteen jotta se voisi ottaa yhteyttä verkon yli masteriin.

  <img width="1004" height="233" alt="kuva" src="https://github.com/user-attachments/assets/1b9bfa20-e24d-4806-a838-8400a73c54a3" />

- Tämän jälkeen käynnistin salt-minion palvelun uudestaan komennolla '$ sudo systemctl restart salt-minion' ja saltin käynnistettyä uudelleen tarkistin vielä, että se on oikeasti päällä komennolla '$ sudo systemctl status salt-minion'

  <img width="1004" height="277" alt="kuva" src="https://github.com/user-attachments/assets/feb13aac-54b9-4a8a-b73e-e9ef7065c178" />

- Seuraavaksi master -koneella tarkistin, että onko orja oikeasti ottanut yhteyttä tähän.

  <img width="1004" height="367" alt="kuva" src="https://github.com/user-attachments/assets/fb1e947a-c350-406f-8d9f-8bbde3caa6c8" />

- Orja on ottanut yhteyttä ja hyväksyin sen komennolla '$ sudo salt-key -a t002'

  <img width="1004" height="305" alt="kuva" src="https://github.com/user-attachments/assets/7dcf7894-d662-4087-86d4-491adf66f575" />

- Seuraavaksi testasin vielä verkon yli komentaa orja konetta masterilla ja samalla testasin idempotenssia.

  <img width="1004" height="494" alt="kuva" src="https://github.com/user-attachments/assets/20943b49-6747-4895-9aa2-51982db12a79" />

- Komennus toimi moitteettomasti verkon yli ja idempotenssi tuli kanssa ilmi!

## e) Kahden eri tilan testaaminen verkon yli

## Lähteet:

https://terokarvinen.com/install-salt-on-debian-13-trixie/
https://oispadotka.wordpress.com/2020/05/12/h6/
https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/
