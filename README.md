# Active Reconnaissance

Ecco un riassunto tradotto per i tuoi appunti da inserire su GitHub:

---

### Modulo di Sicurezza della Rete - Seconda Stanza: Ricognizione Attiva

Ricognizione attiva e sugli strumenti essenziali ad essa correlati:  **ping, traceroute, telnet e nc** per raccogliere informazioni sulla rete, sul sistema e sui servizi.
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

La ricognizione attiva richiede di stabilire un contatto con il target. Questo contatto può essere una telefonata o una visita all'azienda target sotto qualche pretesto per raccogliere ulteriori informazioni, solitamente come parte di un'operazione di social engineering. In alternativa, può essere una connessione diretta al sistema target, visitando il loro sito web o controllando se il firewall ha una porta SSH aperta. Pensate a questo come a un'ispezione attenta di finestre e serrature. È essenziale ricordare di non intraprendere attività di ricognizione attiva senza aver ricevuto un'autorizzazione legale scritta dal cliente. 

Qualsiasi connessione di questo tipo potrebbe lasciare informazioni nei log, come l'indirizzo IP del cliente, l'orario e la durata della connessione. Tuttavia, non tutte le connessioni sono sospette; è possibile far apparire la ricognizione attiva come attività normale di un cliente. Considerate la navigazione web; nessuno sospetterebbe un browser connesso a un server web target tra centinaia di altri utenti legittimi. Potete usare tali tecniche a vostro favore quando lavorate come parte del red team (attaccanti) e non volete allarmare il blue team (difensori).


Iniziamo con il browser web e i suoi strumenti di sviluppo integrati; inoltre, mostriamo come un browser possa essere "armato" per diventare un'efficace framework di ricognizione. Successivamente, discutiamo altri strumenti benigni come ping, traceroute e telnet. Tutti questi programmi richiedono una connessione al target, e quindi le nostre attività rientrano nella ricognizione attiva.


