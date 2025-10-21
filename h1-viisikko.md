# h1 - Viisikko

## x) Lue ja tiivistä

- Salt asennettaan uutena arkistona, johon ladataan 2 tiedostoa (Karvinen 2025)

- Uusi asennus linuxin ulkopuolelta vaatii täysin luoton, sillä binääritiedoston asennettua annetaan root oikeudet sovellukselle (Karvinen 2025)

- Paikalliset salt komennot ovat hyödyllisiä, sillä vastaukset saadaan välittömästi, joten testaukset ovat nopeapia ja tehokkaampia (Karvinen 2023)

## b) Saltin asennus Linuxille (Karvinen 2025) ohjeiden mukaan.

Aloitin Saltin asentamisen linuxille asentamalla wget tiedostot:

- $ sudo apt-get update
- $ sudo apt-get install wget

Seuraavaksi loin uuden hakemiston nimellä saltrepo/ ja latasin sinne 2 tiedostoa verkosta:

- $ wget https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public
- $ wget https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources

sen jälkeen vertasin less komennolla lataamiani tiedostoja ohjeiden kuviin tarkistaakseni, että kaikki meni oikein.

Seuraavaksi asensin saltin arkiston sekä itse saltin:

- $ sudo cp public /etc/apt/keyrings/salt-archive-keyring.pgp
- $ sudo cp salt.sources /etc/apt/sources.list.d/

- $ sudo apt-get update
- $ sudo apt-get install salt-minion salt-master

Lopuksi testasin Saltin, jotta se toimisi hakemalla sen version ja luomalla tiedoston:

- $ salt --version
- $ sudo salt-call --local state.single file.managed /tmp/hellotero

Versioksi sain salt 3007.8 (Chlorine) ja tiedoston luonti onnistui myös luomalla tiedoston ID:llä /tmp/helloalex

## c) Viisi tärkeintä Saltin tilafunktiota

pkg
- Pakettien hallinnan työkalu
- $ sudo salt-call --local -l info state.single pkg.installed tree komennolla sain asennettua paketin nimellä tree. Lyötyäni saman komennon uudestaan, ei se tehnyt mitään, koska paketti oli jo asennettu.
- $ sudo salt-call --local -l info state.single pkg.removed tree komennolla sain poistettua luotuni tree nimisen paketin. Saman koodin laitettuani ei tapahtunut muutoksia, sillä kyseinen paketti oli jo poistettu.

file
- Tiedostojen hallinnan työkalu
- Huomasin toimintaperiaatten olevan samanlainen kuin ylemmässä kohdassa.
- $ sudo salt-call --local -l info state.single file.managed /tmp/helloalex komennolla sain luotua tiedoston nimellä /tmp/helloalex.
- Sain lisättyä myös tekstiä tiedostoon lisäämällä komennon contents="<tekstiä>" aiemman komennon loppuun.
- Sain poistettua tiedostot komennolla sudo salt-call --local -l info state.single file.absent /tmp/helloalex.

service
user
cmd

## d) Idempotentti

## Lähteet

Karvinen 2025. Install Salt on Debian 13 Trixie https://terokarvinen.com/install-salt-on-debian-13-trixie/

Karvinen 2023. Run Salt Command Locally https://terokarvinen.com/2021/salt-run-command-locally/

Karvinen 2018. Salt Quickstart https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/

Karvinen 2006. Raportin kirjoittaminen https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/
