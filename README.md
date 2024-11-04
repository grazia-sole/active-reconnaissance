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

Esistono anche numerosi componenti aggiuntivi per Firefox e Chrome utili nei test di penetrazione. Ecco alcuni esempi:

- **FoxyProxy**: consente di cambiare rapidamente il server proxy utilizzato per accedere al sito target. È comodo quando si utilizza uno strumento come Burp Suite o se è necessario cambiare regolarmente server proxy. Puoi ottenere FoxyProxy per Firefox da [qui](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/).
  
- **User-Agent Switcher and Manager**: permette di fingere di accedere alla pagina web da un sistema operativo o un browser web diverso. Ad esempio, puoi fingere di navigare su un sito usando un iPhone mentre in realtà stai accedendo da Mozilla Firefox. Puoi scaricare User-Agent Switcher and Manager per Firefox da [qui](https://addons.mozilla.org/en-US/firefox/addon/user-agent-string-switcher/).
  
- **Wappalyzer**: fornisce informazioni sulle tecnologie utilizzate nei siti web visitati. Questa estensione è utile, soprattutto quando si raccoglie informazioni mentre si naviga nel sito come un normale utente. Puoi trovare Wappalyzer per Firefox [qui](https://addons.mozilla.org/en-US/firefox/addon/wappalyzer/).

Con il tempo, potresti trovare alcune estensioni che si adattano perfettamente al tuo flusso di lavoro.

--- 


