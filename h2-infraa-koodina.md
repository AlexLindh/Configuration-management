# h2 - Infraa koodina

## x) Lue ja tiivistä

- Luodaan /srv/salt kansio, mikä on jaettu kaikille orjakoneille.
- Luodaan uusia .sls tiedostoja käyttämällä "$ sudoedit" -komentoa.
- Paikallisen salt -komennon voi ajaa koodilla "$ sudo salt-call --local state.apply [kansion nimi, missä .sls tiedosto sijaitsee]".
- Idempotenssin voi testata ajamalla komennon monta kertaa ja tarkistamalla, että komento tekee muutoksen vain ensimmäisellä kerralla tai kun tehdään muutoksia ajaman komennon lopputulokseen.
- YAML -renderöintitapa on oletus Saltissa.
- YAML:in tehtävä on muuttaa YAML datastruktuuri Python datastrukruutiksi Saltille.
- YAML koostuu kolmesta perustyypistä: Scalars, Lists & Dictionaries.
- Top file on tiedosto, joka sisältää asetettavat tilat ja määritykset verkon yli koneille ja ryhmille.
- Top tiedostojen nimi on oletetusti aina top.sls
- Top tiedostossa on kolme komponenttia: Environment, Target & State files.

## a) Hei infrakoodi! Kokeile paikallisesti (esim 'sudo salt-call --local') infraa koodina. Kirjota sls-tiedosto, joka tekee esimerkkitiedoston /tmp/ -kansioon.

- Aloitin luomalla kansiot saltille, minne luon .sls tiedostot.

<img width="750" height="120" alt="kuva" src="https://github.com/user-attachments/assets/307d375e-6bb7-46df-b8db-5b8c7d3d6e13" />

- Seuraavaksi loin init.sls tiedoston komennolla '$ sudoedit init.sls', jonka sisällä olevan koodi luo /tmp/ kansioon tiedoston nimellä hello-infra.

<img width="246" height="92" alt="kuva" src="https://github.com/user-attachments/assets/54a83679-2bcb-43ac-bff3-f68c382193f9" />

- Seuraavaksi testasin ajamalla luomani kansion nimeltä helloalex, joka sisältää init.sls tiedoston testatakseni sen toiminnon.

<img width="1732" height="646" alt="kuva" src="https://github.com/user-attachments/assets/58828fcd-c2d3-4e67-82b8-3a59050edf45" />

- Ajoin koodin vielä uudestaan testaakseni sen idempotenssin. Komennon uudelleen ajaminen ei tehnyt muutoksia toisella kerralla todentaakseen sen idempotenssin.

<img width="1132" height="464" alt="kuva" src="https://github.com/user-attachments/assets/8d93a69c-f51a-4074-87d4-2848fabaf1d5" />

- Kävin vielä tarkistamassa, että komento oikeasti loi tiedoston.

<img width="314" height="136" alt="kuva" src="https://github.com/user-attachments/assets/d81411e3-112f-4c44-9bfd-b31a56135012" />

## b) Toppping. Tee top-file, niin että kaikki omat tilasi ajetaan kerralla komennolla 'sudo salt-call --local state.apply'.

- Aloitin luomalla top-filen olemalla /srv/salt -kansiossa, missä loin top.sls -tiedoston komennolla '$ sudoedit top.sls'.

<img width="298" height="136" alt="kuva" src="https://github.com/user-attachments/assets/7874b959-ce45-4288-a1cd-3777c0bc7302" />

- top.sls -tiedoston sisältö ajaa tällä komennolla helloalex -kansiossa olevan init.sls -tiedoston.

<img width="1724" height="614" alt="kuva" src="https://github.com/user-attachments/assets/93d8f840-7de3-45a8-b4c4-7dc07ae1cdb5" />

- Top-filen ajaminen ei tehnyt muutoksia tällä hetkellä, koska /tmp/hello-infra -tiedosto on jo olemassa edellisen tehtävän puolesta.
  
## c) Viisikko tiedostossa. Tee erilliset esimerkit kustakin viidestä tärkeimmästä tilafunktiosta pkg, file, service, user, cmd. Kirjoita esimerkit omiksi tiloikseen /srv/salt/ alle, esim /srv/salt/hellopkg/init.sls.

- Aloitin luomalla jokaiselle erilliselle esimerkille oman kansion '$ sudo mkdir hellopkg/helloalex/hellosrv/hellouser/hellocmd'

  <img width="808" height="60" alt="kuva" src="https://github.com/user-attachments/assets/f87bf712-bd89-4a91-81b6-7d38cb0df0f6" />

### Package:

  Aloitin luomalla /hellopkg -kansioon init.sls tiedoston '$ sudoedit' -komennolla.

  Loin init.sls -tiedostoon koodin, joka asentaa "tree" -palvelun ajettuaan.

  <img width="246" height="110" alt="kuva" src="https://github.com/user-attachments/assets/f89e878c-bbca-4215-bcd0-28b858ab0fb1" />

  Testasin vielä ajamalla /hellopkg:n init.sls -tiedoston testatakseen sen toimivuuden

  <img width="1000" height="488" alt="kuva" src="https://github.com/user-attachments/assets/f117a66b-8855-4c7b-a0cf-3e6c8f0ed1b7" />

### File:

  Tiedoston luonti onnistui aikaisemmassa tehtävässä /helloalex -kansiossa olevassa init.sls -tiedostosta, joka loi /tmp -kansioon hello-infra nimisen tekstitiedoston.

  Kävin muokkaamassa kyseistä init.sls -tiedostoa siten, että luomaan tekstitiedostoon tulee tekstiä.

  <img width="268" height="120" alt="kuva" src="https://github.com/user-attachments/assets/185ab766-3ecf-479c-b1d6-136f501abbe8" />

  Ajettuani helloalex -kansion uudestaan teki se muutoksen ja lisäsi tekstiä jo luomaan tiedostoon.

  <img width="1026" height="654" alt="kuva" src="https://github.com/user-attachments/assets/98628af7-4dc4-42e6-8435-cc2c6a76ac2a" />

  asd
  
### Service:

  asd

### User:

  acd

### CMD:

  asd

  loppu

## d) Tee sls-tiedosto, joka käyttää vähintään kahta eri tilafunktiota näistä: package, file, service, user. Tarkista eri ohjelmalla, että lopputulos on oikea. Osoita useammalla ajolla, että sls-tiedostosi on idempotentti.

## Lähteet:

https://terokarvinen.com/2024/hello-salt-infra-as-code/

https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml

https://docs.saltproject.io/en/latest/ref/states/top.html

