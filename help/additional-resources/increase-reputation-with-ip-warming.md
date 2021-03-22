---
title: Aumenta la reputazione dell'e-mail con il riscaldamento dell'IP
description: Scopri perché è importante migliorare la reputazione delle e-mail con il riscaldamento dell’IP e come procedere per un recapito messaggi ottimale.
feature: Risorse aggiuntive
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 96ed84da391faaabd3001ddd6a411ddc1f46b033
workflow-type: tm+mt
source-wordcount: '1602'
ht-degree: 0%

---


# Aumenta la reputazione dell&#39;e-mail con il riscaldamento dell&#39;IP

<!--Increase your email reputation with IP warming

## IP Warming overview

In the Adobe Deliverability Consulting and Deliverability Operations teams, we have a vested interest in helping new Campaign customers be as successful as possible as they embark on the route of an IP warming process. If you’ve never been a part of such a project, you may have a lot of questions about it. Let’s get down to the details!-->

## Introduzione

Ad Adobe, i clienti devono condividere la propria configurazione per aiutare il team di Adobe Deliverability a comprendere il tuo programma univoco. Le domande che poniamo sono progettate per aiutare il team di Adobe Deliverability a ottenere un senso della tua reputazione di invio e del volume delle e-mail. Senza una comprensione concreta del tuo modello di business, degli obiettivi di marketing e delle metriche sulla reputazione, non saremo in grado di personalizzare la strategia e ci saranno problemi di recapito messaggi.

All’inizio, ti verranno assegnati i tuoi indirizzi IP (Internet Protocol) dedicati. Nel contesto dell’invio dell’e-mail, un indirizzo IP è il percorso utilizzato per inviare i messaggi e-mail ai clienti. Gli indirizzi IP e i domini vengono utilizzati per identificare i mittenti su una rete agli ISP riceventi. L’Adobe assegna il numero appropriato di indirizzi IP dedicati per l’invio di e-mail in base al volume di invio, ai programmi e-mail, alle pratiche di segmentazione dei dati e al contratto.

**Argomenti correlati:**
* [Come effettuare una transizione senza problemi quando si passano da una piattaforma e-mail all’altra](../../help/transition-process/switching-email-platforms.md)
* [Strategia IP](../../help/transition-process/infrastructure.md#ip-strategy)
* [Considerazioni specifiche dell&#39;ISP durante il riscaldamento dell&#39;IP](../../help/transition-process/isp-specific-considerations-during-ip-warming.md)

## Riscaldamento IP: Perché è fatto? {#why-ip-warming}

I provider di servizi Internet (ISP) o i provider di cassette postali (MBP) prendono precauzioni quando rilevano un IP sconosciuto e inviano un dominio. Questa è una procedura standard associata a qualsiasi nuovo IP di invio, indipendentemente dal tipo di mittente. Gli ISP/MBP mettono l’IP e il dominio di invio sotto controllo per determinare se le e-mail inviate da questo IP e dal dominio sono spam o no.  Questa è una procedura standard associata a qualsiasi nuovo IP di invio, indipendentemente dal tipo di mittente.

Gli ISP esaminano con attenzione il volume di invio, la frequenza di invio, i reclami e i tassi di mancato recapito generati da questi invii. Questi sono tutti controllati da vicino perché sono indicatori di reputazione del mittente - che sia buono o cattivo.

Naturalmente, questo processo di esame di questi dati richiede tempo e non può essere realizzato in un giorno o due. La reputazione è costruita nel tempo. Questo processo è come lasciare uno sconosciuto a casa tua. Avreste delle riserve sull&#39;avere qualcuno che non avete mai incontrato per entrare in casa vostra?

Molto probabilmente la risposta è sì. Vorreste analizzare questa persona e le loro motivazioni. Loro intendono un male? Sono una minaccia? Gli ISP fanno lo stesso per proteggere la loro rete dal traffico dannoso o indesiderato. Le metriche positive sulla reputazione ti aiutano a fare molta strada in un processo di riscaldamento dell&#39;IP di successo. Ecco perché sottolineiamo l’importanza di iniziare con l’invio di piccoli volumi di e-mail e di iniziare a inviare per primi ai clienti altamente coinvolti. Per ulteriori informazioni, consulta [Criteri di targeting per l’invio di un nuovo traffico](/help/transition-process/targeting-criteria.md).

L’invio di grandi quantità di e-mail da un nuovo IP o IP all’esterno del gate è una procedura non corretta e probabilmente causerà alcune difficoltà di recapito. È importante notare che anche se si inizia a inviare volumi di piccole dimensioni e a incrementarli gradualmente come consigliato, è comunque necessario seguire le best practice relative alle e-mail.

![](../../help/assets/ip-warming-volume-trend.png)

## Autorizzazione alla posta (Opt-in esplicito)

Questo è il componente più importante per la gestione e la crescita di un elenco di e-mail per gli utenti abbonati. Man mano che le leggi anti-spam crescono e diventano più complete a livello internazionale, dovrebbe essere un obiettivo primario per gli addetti al marketing garantire che abbiano ricevuto il consenso esplicito (o esplicito) da ogni abbonato al loro elenco. In altre parole, ogni utente iscritto ha accettato attivamente di ricevere e-mail dal tuo marchio. Ciò si differenzia dal consenso implicito in cui una persona viene aggiunta a un elenco di e-mail dopo aver eseguito un’azione che non era esplicitamente registrata per un programma e-mail.

Ulteriori informazioni su [Criteri di utilizzo accettabili per Adobe](https://www.adobe.com/legal/terms/aup.html).

## Metriche di reputazione: Cosa stanno cercando gli ISP?

Gli ISP utilizzano tecnologie sofisticate per prendere decisioni informate sull&#39;opportunità o meno di inviare e-mail a loro stessi da reti esterne. A volte hanno algoritmi complicati e proprietari nel loro set di strumenti per aiutarli in questo processo.

Alcuni dei dati esaminati sono:

* Colpi di trappola per spam
* Inserire nell&#39;elenco Bloccati degli hit
* Rimbalzi e-mail
* Abbonamento

Gli ISP richiedono configurazioni tecniche specifiche in linea con i relativi criteri e best practice. Adobe configura gli IP e i sottodomini delegati per identificarti come mittente responsabile e affidabile. Questa funzione è denominata [autenticazione e-mail](/help/transition-process/infrastructure.md#authentication). L’autenticazione consente ai destinatari di verificare se un mittente dispone dei diritti da inviare da quell’IP o da quel dominio.

L’autenticazione consente agli ISP di verificare che l’azienda che invia da un dominio o da un IP abbia il diritto di farlo. È essenzialmente fatto per dimostrare la tua identità e per assicurarti di non fingere di essere qualcun altro e che qualcun altro non finge di essere te.

Ad Adobe, configureremo SPF e DKIM per impostazione predefinita e configureremo DMARC su richiesta. Gli ISP fanno riferimento a SPF e DKIM come forme primarie di autenticazione. Molti ISP stanno inoltre incorporando DMARC (Domain-based Message Authentication, Reporting &amp; Conformance) nelle loro decisioni di filtraggio. Le e-mail non autenticate non sono necessariamente bloccate, ma passano attraverso un filtro aggiuntivo.

## Riscaldamento IP: Cosa aspettarsi

### Posta limitata o bloccata

Gli spammer inviano sempre da nuovi IP - masterizzeranno attraverso un pool di IP fino a quando non vengono chiusi e ripetono il processo su un altro pool di IP. Di conseguenza, gli ISP gestiscono con attenzione il traffico inviato da nuovi IP. Impediscono agli IP di inviare una grande quantità di e-mail perché sospettano che si tratti di un’attività dannosa eseguita dagli spammer.

Di conseguenza, non è raro ricevere messaggi di differimento o di limitazione quando si inizia a inviare messaggi dai nuovi IP. Dopo alcuni tentativi, il messaggio viene generalmente accettato e consegnato.

Il raggiungimento di un normale flusso di traffico attraverso gli ISP che differiscono i nuovi mittenti potrebbe richiedere alcuni giorni. Tuttavia, non interrompere l&#39;invio di posta - continuare a concentrarsi solo sull&#39;invio ai tuoi abbonati e-mail più impegnati.

In rari casi, l&#39;ISP blocca il nuovo mittente. L&#39;Adobe sta monitorando il tuo account, e se si sospetta che un blocco di questo tipo, si rivolgerà all&#39;ISP per cercare di aiutare a risolvere la situazione nel miglior modo possibile.

Ricordate che la coerenza è fondamentale qui. I pattern di invio irregolari del volume e i pattern di invio non frequenti causeranno alcune difficoltà di recapito.

### Reclami

[](/help/metrics/complaints.md) Completa quando un utente iscritto etichetta un’e-mail come spam attraverso il proprio programma e-mail. Questo invia un avviso all&#39;ISP sull&#39;attività di reclamo. Se ci sono abbastanza di queste lamentele che entrano nell&#39;ISP, che l&#39;ISP agirà per proteggere i suoi clienti - possibilmente bloccare molte e-mail dall&#39;arrivare agli abbonati o dirigere una parte di e-mail alla cartella di massa rispetto alle caselle in entrata degli abbonati. Se il problema di consegna è causato da reclami, è importante determinare il motivo per cui i destinatari si lamentano.

Gli abbonati si lamentano per vari motivi. A volte un utente iscritto non desidera ricevere altre e-mail da te, forse perché ritiene di ricevere troppi messaggi sullo stesso argomento, non si aspettava il messaggio o non si ricorda di registrarsi per ricevere le tue e-mail.

### Validità dei dati

I rimbalzi rigidi si verificano quando si invia a un indirizzo non recapitato a un ISP. Un indirizzo può non essere recapitato per molti motivi, ad esempio un errore durante la digitazione dell’indirizzo o l’invio a un indirizzo precedentemente attivo ma chiuso o terminato dopo un periodo di inattività.

Se incontri un numero considerevole di rimbalzi rigidi, è importante capire il perché. Rivedi come sono stati raccolti gli indirizzi e conferma che l’autorizzazione è stata concessa. A volte le persone chiudono il proprio account e-mail e non notificano a chi ha tale indirizzo nel proprio elenco di marketing.

### Coinvolgimento

Gli ISP cercano volumi coerenti e buona qualità dei dati. Aumenterete lentamente e costantemente il traffico nelle prossime quattro-otto settimane. A volte rampa-up richiedono più o meno tempo in base al volume e obiettivi, ma in genere, è almeno un processo di 8 settimane.

Il traffico e-mail deve essere distribuito in modo lento e costante, aumentando ogni settimana fino a quando l’intero elenco non è stato inviato. Inoltre, ogni segmento seguirà la pianificazione fino al completamento. Inizia prima con gli abbonati più recenti e finisce con gli abbonati meno coinvolti ultimi. Tieni presente che alcuni ISP possono richiedere un approccio più personalizzato a causa del modo in cui gestiscono il nuovo traffico.

Ulteriori informazioni su [coinvolgimento](/help/engagement.md).

## Rimani il corso

Potresti essere tentato di accelerare il processo di riscaldamento dell&#39;IP inviando più volume di quello consigliato, trascurando di passare il tempo identificando i tuoi abbonati più impegnati e non inviando loro la posta prima nel tentativo di costruire una reputazione positiva. Resistete a questo impulso! Non vi aiuterà a lungo termine.

È molto importante iniziare a spedire il tuo altamente impegnato (con e-mail!) gli abbonati solo per le fasi iniziali del riscaldamento dell&#39;IP. Questi clienti sono i tuoi più preziosi e la loro propensione ad aprire le tue e-mail ti aiuterà a mostrare agli ISP che sei un addetto al marketing che invia e-mail interessanti e ricercate. Mostra anche gli ISP che stai giocando secondo le regole e seguendo le best practice.

## Conclusione

Ricorda: IP Warm è una maratona - non uno sprint!  Anche se il processo può sembrare oneroso e dispendioso in termini di tempo, sarebbe più lavoro cercare di riparare una reputazione danneggiata dal non seguire le best practice e-mail provate e vere.

Migliori sono le tue pratiche di invio e più alti sono i punteggi di reputazione con gli ISP, maggiore è la probabilità che le tue e-mail saranno consegnate. Il riscaldamento e l’aumento dell’IP, insieme alle best practice per la progettazione delle tue e-mail, aiuteranno a ottimizzare la consegna della posta in arrivo.

Il nostro team di recapito messaggi globale è il vostro partner in questo processo e vi aiuterà durante questa fase di riscaldamento dell&#39;IP per posizionarvi per il successo.
