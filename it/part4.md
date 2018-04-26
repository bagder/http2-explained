# 4. Aggiornare HTTP

Non sarebbe carino creare un protocollo più potente ? Si, si ..

1. Meno sensibile alla latenza
2. Miglior pipelining e basta bloccaggi "head of line"
3. Eliminare il bisogno di incrementare il numero di connessioni ad ogni host
4. Mantenere tutte le interfacce esistenti, i contenuti, i formati URI e gli schemi
5. Che sia sviluppato assieme al gruppo di lavoro IETF HTTPbis

## 4.1. Lo IETF e il gruppo di lavoro HTTPbis

La Internet Engineering Task Force (IETF) è una organizzazione che sviluppa e promuove gli standard dell'internet, soprattutto a livello di definizione di protocolli. Sono da sempre consosciuti per le serie di memo RFC su argomenti quali TCP, DNS e FTP, best practices, HTTP e numerose varianti che non si sono mai trasformate in niente di più.

All'interno di IETF, gruppi di lavoro dedicati sono istituiti al fine di lavorare verso un obiettivo comune, con un dominio limitato. Stabiliscono un "charter" ed alcune linee-guida (e limiti) rispetto alle aspettative. Chiunque è benvenuto ad aggiungersi, a partecipare alla discussione e allo sviluppo. Chiunque partecipi e dica qualcosa di concreto ha lo stesso peso nell'influenzare l'esito del lavoro. Tutti i membri sono considerati allo stesso livello, senza troppa importanza rispetto al datore di lavoro.

Il gruppo di lavoro HTTPbis (vedi più avanti per la spiegazione del nome) è 
stato formato durante l'estate 2007 e incaricato di creare un aggiornamento 
delle specifiche HTTP 1.1. All'interno del gruppo, la discusssione a proposito
di una futura versione è iniziata in tardo 2012. Il lavoro di aggiornamento su
HTTP 1.1 è stato completato nel 2014 ed è risultato nella serie di RFC 
[RFC 7230](https://tools.ietf.org/html/rfc7230)

Il meeting finale per il GdL HTTPbis inter-op si è tenuto a New York City inizio Giugno 2014. Le restanti discussioni e le procedure IETF svolte per pubblicare definitivamente la RFC si sono tenute fino all'anno successivo.

Alcuni dei più grandi attori in campo di HTTP sono spariti dalle discussioni e dagli incontri del gruppo di lavoro. Non voglio citare nessuna compagnia in particolare o alcun nome di prodotto, ma è chiaro come alcuni protagonisti dell'Internet di oggi sembrino essere convinti che la IETF se la caverà da sola, senza bisogno che tali grosse compagnie siano coinvolte...

### 4.1.1. La parte "bis" nel nome

Il gruppo si chiama HTTPbis dove "bis" deriva dall'avverbio latino significante "secondo" [Latin adverb for two](https://en.wiktionary.org/wiki/bis#Latin). Bis è comunemente usato in contesto IETF come suffisso o parte del nome per un aggiornamento o una review di una specifica; in tal caso, l'aggiornamento di HTTP 1.1

## 4.2. http2 ha preso vita da SPDY

[SPDY](https://en.wikipedia.org/wiki/SPDY) è un protocollo sviluppato e diffuso da Google. Lo hanno sicuramente sviluppato in maniera aperta, invitando chiunque a partecipare; è anche ovvio che Google ne avrebbe tratto beneficio, mantenendo il controllo su una popolare implementazione di browser e su una significativa popolazione di server con servizi ben noti.

Al momento in cui HTTPbis ha deciso che fosse tempo di iniziare a lavorare su http2, SPDY aveva gia ampiamente provato la sua validità concettuale. SPDY ha mostrato come fosse possibile distribuirlo su internent, c'erano statistiche e numeri a riprova delle ottime performances. Il lavoro su http2 è cominciato con la draft di SPDY/3 che di fatto è stata trasformata nella draft-00 di http2, con un colpetto di sed/replace.

