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


- Seuraavaksi loin init.sls tiedoston komennolla "$ sudoedit init.sls", jonka sisällä olevan koodi luo /tmp/ kansioon tiedoston nimellä hello-infra.


<img width="246" height="92" alt="kuva" src="https://github.com/user-attachments/assets/54a83679-2bcb-43ac-bff3-f68c382193f9" />


- Seuraavaksi testasin ajamalla luomani kansion nimeltä helloalex, joka sisältää init.sls tiedoston testatakseni sen toiminnon.


<img width="1732" height="646" alt="kuva" src="https://github.com/user-attachments/assets/58828fcd-c2d3-4e67-82b8-3a59050edf45" />


- Ajoin koodin vielä uudestaan testaakseni sen idempotenssin. Komennon uudelleen ajaminen ei tehnyt muutoksia toisella kerralla todentaakseen sen idempotenssin.


<img width="1132" height="464" alt="kuva" src="https://github.com/user-attachments/assets/8d93a69c-f51a-4074-87d4-2848fabaf1d5" />


- Kävin vielä tarkistamassa, että komento oikeasti loi tiedoston.


<img width="314" height="136" alt="kuva" src="https://github.com/user-attachments/assets/d81411e3-112f-4c44-9bfd-b31a56135012" />


## b) Top filen tekeminen

## c) Viisikko tiedostot

## d) kahden tilafunktion .sls tiedosto

# Lähteet:

https://terokarvinen.com/2024/hello-salt-infra-as-code/
https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml
https://docs.saltproject.io/en/latest/ref/states/top.html

