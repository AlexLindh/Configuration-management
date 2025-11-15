# h4 - pkg-file-service

## Lue ja tiivistä

- pkg-file-service on yleinen tapa hallita konfiguraatioita keskitetyssä palvelinten hallinnassa. (Karvinen 2018)

- pkg-file-servicessä asennetaan paketti, muokataan tiedostoa ja käynnistetään se uudelleen automaattisesti. (Karvinen 2018)

## SSHouto. Lisää uusi portti, jossa SSHd kuuntelee.

### Koneiden ja Saltin asentaminen

Aloitin asentamalla ja käynnistämällä vagrantin koneellani, jonka ympäristössä testaisin tämän tehtävän. Asennuksen hoidin aikaisemmin luodun raportin mukaan: https://github.com/AlexLindh/Configuration-management/blob/main/h3-soitto-kotiin.md. 

Käynnistettyä vagrantilla kaksi virtuaalikonetta asensin Salt-masterin ja salt-minionin ohjeiden mukaisesti: https://github.com/AlexLindh/Configuration-management/blob/main/h1-viisikko.md. Laitoin minion koneelle myös masterin ip-osoitteen ja hyväksyin sen masterilla. testasin pingausta komennolla:
    
    '$ sudo salt '*' test.ping'

Pingaus oli positiivinen ja jatkoin testailuja!

Koneet toiminnassa ja saltit asennettua testasin ssh yhteyttä porteista 22 ja 1234 toisiinsa:

<img width="1004" height="140" alt="kuva" src="https://github.com/user-attachments/assets/caa4d3a9-7e68-4407-b14f-37e372670b49" />

Yhteyttä ei tullut aluksi, koska ssh paketteja ei ollut ladattu.

### Manuaalinen testaaminen

Aloin testaamaan ensin manuaalisesti ssh:n asentamista ja käyttöönottoa ennen automatisointia. 

Aloitin asentamalla paketin komennolla: 

    '$ sudo apt-get install ssh'

Seuraavaksi muokkasin ssh:n konfiguraatio tiedostoa komennolla:

    '$ sudoedit /etc/ssh/sshd_config'

Poistin kaikista kohdista '#', jotta koodissa ei olisi kommentteja ja lisäsin myös uuden portin 1234 tiedostoon avattavaksi. Koodista tuli seuraavanlainen:

    Port 1234
    
    Port 22
    
    AddressFamily any
    
    ListenAddress 0.0.0.0
    
    ListenAddress ::
    
    HostKey /etc/ssh/ssh_host_rsa_key
    
    HostKey /etc/ssh/ssh_host_ecdsa_key
    
    HostKey /etc/ssh/ssh_host_ed25519_key
    
    RekeyLimit default none
    
    SyslogFacility AUTH
    
    LogLevel INFO
    
    LoginGraceTime 2m
    
    PermitRootLogin prohibit-password
    
    StrictModes yes
    
    MaxAuthTries 6
    
    MaxSessions 10
    
    PubkeyAuthentication yes
    
    AuthorizedKeysFile      .ssh/authorized_keys .ssh/authorized_keys2
    
    AuthorizedPrincipalsFile none
    
    AuthorizedKeysCommand none
    
    AuthorizedKeysCommandUser nobody
    
    HostbasedAuthentication no
    
    IgnoreUserKnownHosts no
    
    IgnoreRhosts yes
    
    PasswordAuthentication no
    
    PermitEmptyPasswords no

    ChallengeResponseAuthentication no

    KerberosAuthentication no
    
    KerberosOrLocalPasswd yes

    KerberosTicketCleanup yes

    #KerberosGetAFSToken no

    GSSAPIAuthentication no

    GSSAPICleanupCredentials yes

    GSSAPIStrictAcceptorCheck yes

    GSSAPIKeyExchange no

    UsePAM yes

    AllowAgentForwarding yes

    AllowTcpForwarding yes

    GatewayPorts no

    X11Forwarding yes

    X11DisplayOffset 10

    X11UseLocalhost yes

    PermitTTY yes
    
    PrintMotd no

    PrintLastLog yes

    TCPKeepAlive yes

    PermitUserEnvironment no

    Compression delayed

    ClientAliveInterval 0

    ClientAliveCountMax 3

    UseDNS no

    PidFile /var/run/sshd.pid

    MaxStartups 10:30:100

    PermitTunnel no

    ChrootDirectory none

    VersionAddendum none

    Banner none

    AcceptEnv LANG LC_*

    Subsystem       sftp    /usr/lib/openssh/sftp-server

    Match User anoncvs
        
        X11Forwarding no
        
        AllowTcpForwarding no
        
        PermitTTY no
        
        ForceCommand cvs server

    ClientAliveInterval 120

    #UseDNS no

Kopioin myös tämän tiedoston talteen jatkoa varten itselleni ylös.

Kyseisten tiedostojen muokkausten jälkeen käynnistin palvelun uudestaan komennoilla:

    '$ sudo systemctl stop ssh'
    
    '$ sudo systemctl start ssh'

    '$ sudo systemctl restart ssh'

Uusien konfiguraatioiden otettua käyttöön testasin ssh yhteyttä koneideni välillä:

<img width="1004" height="183" alt="kuva" src="https://github.com/user-attachments/assets/5ca91695-47e7-4cb6-a15a-9f013f327134" />
<img width="1004" height="201" alt="kuva" src="https://github.com/user-attachments/assets/5c491e70-1503-4d70-a0e3-93d7f0c9aa55" />

Yhteydet porteista '22' ja '1234' ovat ylhäällä vaikka kuvanmukaista julkista avainta ei löytynyt ja portista '8888' yhteys ei ole mahdollista, koska sitä porttia ei ole määritetty avattavaksi tiedostossa.

Seuraavaksi kun sain tämän manuaalisesti toimintaan, poistin kaikki paketit koskien ssh:ta aloittaakseni sen asennuksen uudelleen automaattisesti saltin avulla. Poistin ssh:n näillä komennoilla:

    '$ sudo apt-get purge ssh'

    '$ sudo apt-get purge openssh-server'

Testasin vielä poistamisen jälkeen yhteyksiä varmistaakseni, että yhteydet eivät toimi:

<img width="1004" height="164" alt="kuva" src="https://github.com/user-attachments/assets/9e9398d6-7788-4630-8459-d1b3200c3c71" />

### Automatisoinnin aloittaminen saltilla

Aloitin luomalla uudelle projektilleni oman kansion /srv/salt -hakemistoon.

    '$ sudo mkdir /srv/salt/sshd'

Seuraavaksi loin kyseiseen hakemistoon 'init.sls' -tiedoston, johon lisäsin infraa koodina, että se asentaisi kaikille minioneille ssh paketin.

<img width="1004" height="222" alt="kuva" src="https://github.com/user-attachments/assets/d1ec090b-07d8-4758-b073-afb989a7ca5a" />

Testasin tämän jälkeen masterilla, asentaako tiedosto oikeasti paketin minionilleni.

    '$ sudo salt '*' state.apply sshd'

<img width="1004" height="878" alt="kuva" src="https://github.com/user-attachments/assets/bca9a9ae-6aef-47ab-85cb-69f6f6e9a98b" />

Komento asentaa paketin openssh-server, joten jatkoin 'init.sls' -tiedoston muokkaamista.

Lisäsin koodin, joka muokkaa paketin asentaman /etc/ssh/sshd_config -hakemistossa olevan tiedoston aikaisemmin kopioimani tiedoston asetuksilla. Tämän sain toteutettua luomalla samaan /srv/salt/sshd -hakemistoon tiedoston nimeltä sshd_config ja kopioimalla aikaisemmin luomani tiedoston tänne ja lisäämällä 'init.sls' -tiedostoon seuraavan koodinpätkän:

<img width="1004" height="389" alt="kuva" src="https://github.com/user-attachments/assets/0fbcbfed-2b1b-48e4-8849-0754bf5f8786" />

Testasin koodin toimivuutta ajamalla koodin ja tarkastamalla minion koneella tiedoston /etc/ssh/sshd_config seuraavalla komennolla:

    'cat /etc/ssh/sshd_config'

<img width="1004" height="588" alt="kuva" src="https://github.com/user-attachments/assets/204b03a2-5a83-4ffd-a895-d13c695ebe10" />

Kaikki toimii vielä!

Seuraavaksi lisäsin koodin 'init.sls' -tiedostoon, joka tarkistaa, että palvelu on päällä ja tekee muokkauksia, jos masterin 'sshd_config' tiedosto muuttuu:

<img width="1004" height="505" alt="kuva" src="https://github.com/user-attachments/assets/c5f7c9cc-7b71-4ec1-a8a1-a2bdef23d238" />

Seuraavaksi poistin minion koneelta ssh paketin, muokkasin /etc/ssh/sshd_config tiedostoa ja ajoin masterilla koodin uudestaan.

<img width="1004" height="580" alt="kuva" src="https://github.com/user-attachments/assets/cd048a60-03c2-4d7c-83a7-0cfa8a7ec52c" />

Succeeded:  3 (Changed=3)

Tilan ajettua se asensi paketin uudestaan ja korjasi virheet /etc/ssh/sshd_config tiedostoon ja antoi tilaksi 'true', että palvelu on päällä

Lopuksi testasin vielä ssh ja netcat yhteyksiä toimivuuden varmistamiseksi.

<img width="1004" height="482" alt="kuva" src="https://github.com/user-attachments/assets/ce2a47a0-0ac9-438b-bd43-8ef558d9ab16" />
Yhteydet pelaa!

Uusi avattu portti '1234' toimii ja avaamaton portti '8888' pysyy kiinni!


## Lähteet:

Karvinen 2018. Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port. Luettavissa: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/. Luettu 14.11.2025





