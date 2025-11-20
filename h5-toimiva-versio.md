# h5 toimiva versio

## x) Lue ja tiivistä

## a) Uuden varaston luonti githubiin

Aloitin tehtävän luomalla githubissa uuden varaston. Lisäsin luomisvaiheessa myös README.md -tiedoston ja GNU general public license 3.0:n.

<img width="1004" height="788" alt="kuva" src="https://github.com/user-attachments/assets/4ea322fd-bb54-4e98-9a77-9081244fac60" />

## b) Kloonaa varasto koneelle ja näytä, että komentokehotteessa tekemät muutokset menevät weppiin

Ensinksi olin luonut uuden avainparin komennolla:

    ssh-keygen

Seuraavaksi kopioin juuri luomani julkisen avaimen githubiin, jotta yhteydet voidaan laittaa pystyyn.

<img width="1878" height="694" alt="kuva" src="https://github.com/user-attachments/assets/e3a23024-428a-4275-82d6-ca92c23c1dc5" />

Ensin loin uuden hakemiston tälle projektille nimeltä 'github'.

Seuraavaksi hain githubista repositorioni SSH kloonauksen ja kloonasin sen komentoriville komennolla:

    git clone git@github.com:AlexLindh/JonSnowFanFic.git

Seuraavaksi muokkasin README.md -tiedostoa ja puskin päivitykset verkkoon

<img width="1004" height="629" alt="kuva" src="https://github.com/user-attachments/assets/4d272938-e76f-47ae-a23e-00202fd16fbf" />

<img width="1004" height="306" alt="kuva" src="https://github.com/user-attachments/assets/1d9db647-6cb9-45df-9bf8-65fc17b4b0d7" />
Ja muokkaukset onnistuivat!


## c) Huonon muutoksen korjaaminen git reset --hard -komennolla

Seuraavaksi testasin komennon 'git reset --hard' toimintoa. Muokkasin ensin luomaani README.md tiedostoa ja poistin lisenssi tiedoston hakemistosta kokonaan.

Seuraavaksi palauduin viimeisimpään committiin, jossa kaikki vielä toimi ja sain palautettua kaikki ahkerasti tekemäni tiedostot ennalleen.

<img width="1440" height="678" alt="kuva" src="https://github.com/user-attachments/assets/67d93abe-ad5c-45ad-abd8-e0aae7939b5f" />
Kaikki toimii taas!


## d) Tarkastele ja selitä varastosi lokia. Tarkista, että nimi ja sähköposti ovat oikeat

## e) Aja salt tiloja omasta varastostasi
