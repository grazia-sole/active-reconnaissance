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

