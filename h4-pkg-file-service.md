# h4 - pkg-file-service

## Lue ja tiivistä

## SSHouto. Lisää uusi portti, jossa SSHd kuuntelee.

Aloitin asentamalla ja käynnistämällä vagrantin koneellani, jonka ympäristössä testaisin tämän tehtävän. Asennuksen hoidin aikaisemmin luodun raportin mukaan: https://github.com/AlexLindh/Configuration-management/blob/main/h3-soitto-kotiin.md. 

Käynnistettyä vagrantilla kaksi virtuaalikonetta asensin Salt-masterin ja salt-minionin ohjeiden mukaisesti: https://github.com/AlexLindh/Configuration-management/blob/main/h1-viisikko.md. Laitoin minion koneelle myös masterin ip-osoitteen ja hyväksyin sen masterilla. testasin pingausta komennolla:
    
    '$ sudo salt '*' test.ping'

Pingaus oli positiivinen ja jatkoin testailuja!

Koneet toiminnassa ja saltit asennettua testasin ssh yhteyttä porteista 22 ja 1234 toisiinsa:

<img width="1004" height="140" alt="kuva" src="https://github.com/user-attachments/assets/caa4d3a9-7e68-4407-b14f-37e372670b49" />

Yhteyttä ei tullut aluksi, koska ssh paketteja ei ollut ladattu.

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

