# Active Reconnaissance

Ricognizione attiva e strumenti essenziali ad essa correlati:  **ping, traceroute, telnet e nc** per raccogliere informazioni sulla rete, sul sistema e sui servizi.
```
ping
```
```
traceroute
```
```
telnet
```
```
nc
```

La **ricognizione attiva** richiede di stabilire un contatto con il target. Questo contatto può essere una telefonata o una visita all'azienda target sotto qualche pretesto per raccogliere ulteriori informazioni, solitamente come parte di un'operazione di social engineering. In alternativa, può essere una connessione diretta al sistema target, visitando il loro sito web o controllando se il firewall ha una porta SSH aperta. Pensate a questo come a un'ispezione attenta di finestre e serrature. È essenziale ricordare di non intraprendere attività di ricognizione attiva senza aver ricevuto un'autorizzazione legale scritta dal cliente. 

Qualsiasi connessione di questo tipo potrebbe lasciare informazioni nei log, come l'indirizzo IP del cliente, l'orario e la durata della connessione. Tuttavia, non tutte le connessioni sono sospette; è possibile far apparire la ricognizione attiva come attività normale di un cliente. Considerate la navigazione web; nessuno sospetterebbe un browser connesso a un server web target tra centinaia di altri utenti legittimi. Potete usare tali tecniche a vostro favore quando lavorate come parte del red team (attaccanti) e non volete allarmare il blue team (difensori).


Iniziamo con il browser web e i suoi strumenti di sviluppo integrati; inoltre, mostriamo come un browser possa essere "armato" per diventare un'efficace framework di ricognizione. Successivamente, discutiamo altri strumenti benigni come ping, traceroute e telnet. Tutti questi programmi richiedono una connessione al target, e quindi le nostre attività rientrano nella ricognizione attiva.

Ecco un riassunto tradotto per i tuoi appunti da inserire su GitHub:

---

## Utilizzo del Browser Web per la Ricognizione

Il browser web è uno strumento comodo e disponibile su tutti i sistemi, che può essere utilizzato in diversi modi per raccogliere informazioni su un target.

A livello di trasporto, il browser si connette a:

- **TCP porta 80** per HTTP
- **TCP porta 443** per HTTPS

Poiché 80 e 443 sono porte predefinite per HTTP e HTTPS, il browser non le mostra nella barra degli indirizzi. Tuttavia, è possibile utilizzare porte personalizzate per accedere a un servizio. Ad esempio, `https://127.0.0.1:8834/` si connetterà a 127.0.0.1 (localhost) sulla porta 8834 tramite il protocollo HTTPS, e se un server HTTPS è in ascolto su quella porta, riceveremo una pagina web.

Mentre si naviga in una pagina web, è possibile premere **Ctrl + Shift + I** su un PC o **Option + Command + I** (⌥ + ⌘ + I) su un Mac per aprire gli **Strumenti per Sviluppatori** in Firefox. Scorciatoie simili funzionano anche su Google Chrome o Chromium. Gli Strumenti per Sviluppatori permettono di ispezionare molte cose che il browser ha ricevuto e scambiato con il server remoto, come modificare i file JavaScript (JS), ispezionare i cookie impostati sul sistema e scoprire la struttura delle cartelle dei contenuti del sito.
![image](https://github.com/user-attachments/assets/95902e76-7c00-491e-876a-753888059b19)
Questo è un esempio dei Developer Tools su Firefox.

Esistono anche numerosi componenti aggiuntivi per Firefox e Chrome utili nei test di penetrazione. Ecco alcuni esempi:

- **FoxyProxy**: consente di cambiare rapidamente il server proxy utilizzato per accedere al sito target. È comodo quando si utilizza uno strumento come Burp Suite o se è necessario cambiare regolarmente server proxy. Puoi ottenere FoxyProxy per Firefox da [qui](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/).

  > **Nota:** Un server proxy è un intermediario tra il tuo dispositivo (come un computer o uno smartphone) e Internet. Quando utilizzi un proxy, le tue richieste di connessione a siti web o servizi online vengono prima inviate al server proxy, che poi inoltra queste richieste al sito desiderato. Quando il server proxy riceve la risposta dal sito, la inoltra a te..

  
- **User-Agent Switcher and Manager**: permette di fingere di accedere alla pagina web da un sistema operativo o un browser web diverso. Ad esempio, puoi fingere di navigare su un sito usando un iPhone mentre in realtà stai accedendo da Mozilla Firefox. Puoi scaricare User-Agent Switcher and Manager per Firefox da [qui](https://addons.mozilla.org/en-US/firefox/addon/user-agent-string-switcher/).
  
- **Wappalyzer**: fornisce informazioni sulle tecnologie utilizzate nei siti web visitati. Questa estensione è utile, soprattutto quando si raccoglie informazioni mentre si naviga nel sito come un normale utente. Puoi trovare Wappalyzer per Firefox [qui](https://addons.mozilla.org/en-US/firefox/addon/wappalyzer/).


--- 

## Ping


#### **Ping**
Il comando `ping` è utilizzato per verificare la connettività di rete tra due sistemi. Invia un pacchetto ICMP Echo a un sistema remoto e attende una risposta. Se il sistema remoto è attivo e il pacchetto è stato correttamente instradato, il sistema dovrebbe restituire un pacchetto ICMP Echo Reply. Questo permette di concludere che il sistema remoto è online e raggiungibile.

- **Utilizzo**: 
  ```
  ping MACHINE_IP
  ```
  oppure 
  ```
  ping HOSTNAME
  ```

- **Controllo pacchetti**: Puoi limitare il numero di pacchetti inviati su Linux con 
  ```
  ping -c 10 MACHINE_IP
  ``` 
  e su Windows con 
  ```
  ping -n 10 MACHINE_IP
  ```

- **Output**: Mostra il numero di pacchetti inviati, ricevuti e il tempo medio di risposta.
- **Nota**: per bloccare il ping `CTRL C`

### Esempi:
![image](https://github.com/user-attachments/assets/7e1e8604-b8e5-4b8b-b500-2a74fea452ce)
Da questo esempio si nota una risosta al ping, dunque è online e raggiungibile.

![image](https://github.com/user-attachments/assets/6531a373-9b90-4656-a81c-3bb850c18d29)
Da questo esempio si nota che non vi sono risposte al ping, e che l'host non è raggiungibile.



Quando non ricevi una risposta, ciò può indicare che:
- Il computer di destinazione non è attivo o non risponde.
- È scollegato dalla rete o c'è un dispositivo di rete guasto.
- Un firewall sta bloccando i pacchetti.

---

## Traceroute: Tracciare il Percorso dei Pacchetti

Il comando `traceroute` ha lo scopo di tracciare il percorso dei pacchetti dal tuo sistema a un altro host, rivelando gli indirizzi IP dei router (o salti) attraversati lungo il tragitto. Questo comando è utile per determinare il numero di router tra i due sistemi e il percorso che i pacchetti seguono, il quale può variare poiché molti router utilizzano protocolli di routing dinamico che si adattano ai cambiamenti nella rete.

### Comandi

- Su Linux e macOS: 
  ```
  traceroute 10.10.216.106
  ```
- Su Windows: 
  ```
  tracert 10.10.216.106
  ```

`traceroute` utilizza ICMP per "ingannare" i router affinché rivelino i loro indirizzi IP, sfruttando un valore di Time To Live (TTL) impostato in modo strategico. Anche se il "T" in TTL sta per tempo, questo valore indica il numero massimo di router che un pacchetto può attraversare prima di essere scartato. Ogni router che riceve un pacchetto decrementa il TTL di uno. Se il TTL raggiunge 0, il pacchetto viene scartato e viene inviato un messaggio ICMP "Time-to-Live exceeded" al mittente originale.
![image](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/e82c42dcfae78ac592a8d7843465d2d6.png)

Quando il sistema invia un pacchetto con TTL=1, il primo router lo scarta, inviando un messaggio ICMP al mittente, rivelando così il suo indirizzo. Il processo continua aumentando il TTL, consentendo a `traceroute` di scoprire gli indirizzi IP di ogni router lungo il percorso.

### Output di Traceroute

Ecco un esempio di utilizzo del comando:

```
traceroute tryhackme.com
```

Nell'output di `traceroute`, ogni riga rappresenta un router/salto. Il sistema invia tre pacchetti per ogni valore di TTL, e si può ricevere risposta da diversi router. L'output include i tempi di risposta, ad esempio:

```
12 99.83.69.207 (99.83.69.207) 17.603 ms 15.827 ms 17.351 ms
```

In alcuni casi, potrebbero non arrivare tutte le risposte attese, come indicato da asterischi in corrispondenza dei pacchetti non ricevuti.

### Considerazioni Finali

- Il numero di salti/router varia in base al momento in cui si esegue `traceroute`; non vi è alcuna garanzia che i pacchetti seguano sempre lo stesso percorso.
- Alcuni router potrebbero restituire indirizzi IP pubblici, fornendo spunti per eventuali test di penetrazione.
- Alcuni router potrebbero non rispondere affatto.

In sintesi, `traceroute` è uno strumento potente per diagnosticare il percorso e la latenza tra il tuo sistema e un altro host, ma il percorso può cambiare nel tempo a causa della natura dinamica delle reti.

---


## TELNET: Protocollo di Comunicazione Remota

Il protocollo TELNET (Teletype Network) è stato sviluppato nel 1969 per consentire la comunicazione con un sistema remoto tramite un'interfaccia a riga di comando (CLI). Il comando `telnet` utilizza questo protocollo per l'amministrazione remota, impiegando la porta predefinita 23. 

### Sicurezza

Da un punto di vista della sicurezza, TELNET trasmette tutti i dati, inclusi username e password, in chiaro, rendendo facile per chiunque abbia accesso al canale di comunicazione rubare le credenziali di accesso. Per un'alternativa più sicura, è consigliato utilizzare SSH (Secure SHell), che crittografa i dati trasmessi.

### Utilizzo del Cliente Telnet

Nonostante le sue limitazioni in termini di sicurezza, il client TELNET può essere impiegato anche per altri scopi, poiché si basa sul protocollo TCP. È possibile utilizzarlo per connettersi a qualsiasi servizio TCP e raccogliere informazioni, come ad esempio il banner del servizio.

#### Esempio di Connessione a un Servizio Web

Per scoprire ulteriori informazioni su un server web in ascolto sulla porta 80, puoi connetterti al server e comunicare utilizzando il protocollo HTTP. Ecco un esempio di come farlo:

```
telnet 10.10.216.106 80
```

Dopo esserti connesso, puoi inviare una richiesta HTTP. Ad esempio, per ottenere la pagina principale, digita:

```
GET / HTTP/1.1
host: example
```

Dovrai premere invio due volte per ricevere una risposta valida. Questo comando richiede al server la pagina principale e, se tutto è impostato correttamente, otterrai una risposta come:

```
HTTP/1.1 200 OK
Server: nginx/1.6.2
...
```

### Considerazioni Finali

![image](https://github.com/user-attachments/assets/0389fd28-8bda-4600-8de1-153cbd8a5ba7)

- **Informazioni Utili**: Nell'output, l'intestazione `Server: nginx/1.6.2` è particolarmente interessante, poiché indica il tipo e la versione del server web in uso.
- **Protocollo Specifico**: Se ci si connette a un server di posta, è necessario utilizzare i comandi appropriati per il protocollo in uso, come SMTP o POP3.

In sintesi, mentre TELNET è utile per l'amministrazione remota, la sua trasmissione di dati in chiaro lo rende inadatto per l'uso in contesti sensibili, e si consiglia di preferire protocolli più sicuri come SSH. Tuttavia, la sua semplicità lo rende uno strumento pratico per esplorare e interagire con vari servizi TCP.
