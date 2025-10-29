# h2 - Infraa koodina

## x) Lue ja tiivistä

- Luodaan /srv/salt kansio, mikä on jaettu kaikille orjakoneille.
- Luodaan uusia .sls tiedostoja käyttämällä "$ sudoedit" -komentoa.
- Paikallisen salt -komennon voi ajaa koodilla "$ sudo salt-call --local state.apply [kansion nimi, missä .sls tiedosto sijaitsee]".
- Idempotenssin voi testata ajamalla komennon monta kertaa ja tarkistamalla, että komento tekee muutoksen vain ensimmäisellä kerralla tai kun tehdään muutoksia ajaman komennon lopputulokseen.
- YAML -renderöintitapa on oletus Saltissa.
- YAML:in tehtävä on muuttaa YAML datastruktuuri Python datastrukruutiksi Saltille.
- YAML koostuu kolmesta perustyypistä: Scalars, Lists & Dictionaries.

## a) Hei infrakoodi!

## b) Top filen tekeminen

## c) Viisikko tiedostot

## d) kahden tilafunktion .sls tiedosto

# Lähteet:

https://terokarvinen.com/2024/hello-salt-infra-as-code/
https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml

