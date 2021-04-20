---
title: Best practice per ripetere il coinvolgimento
description: Scopri come migliorare il recapito messaggi tramite strategie di riimpegno.
feature: Additional resources
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 96ed84da391faaabd3001ddd6a411ddc1f46b033
workflow-type: tm+mt
source-wordcount: '928'
ht-degree: 1%

---


# Best practice per ripetere il coinvolgimento {#re-engagement}

Durante l’implementazione del recapito messaggi, alcune delle best practice consistono nel cercare di mantenere una base di abbonati sana e migliorare il recapito messaggi tramite strategie di riimpegno (o win-back).

* Mantenere una base di abbonati sana è uno degli aspetti principali per garantire una consegna corretta e coerente. Molti problemi di recapito dei dati derivano da procedure e manutenzione scadenti dei dati.
* Uno dei problemi più comuni che gli addetti al marketing devono affrontare oggi è l’attività degli abbonati inattivi (anche detta bassa o non coinvolgimento) che può influenzare negativamente la consegna delle e-mail e il basso ROI.

>[!NOTE]
>
>Per ulteriori informazioni sulle strategie per il riimpegno delle campagne e sui servizi di recapito messaggi di Adobe, contatta il tuo consulente di recapito o parla con il tuo agente di vendita Adobe.

## In che modo gli ISP visualizzano l’attività di non coinvolgimento? {#how-do-isps-view-non-engagement-activity-}

Per anni, gli ISP hanno utilizzato le metriche di feedback sul coinvolgimento dei loro utenti per decidere dove collocare i messaggi, o se devono comunque consegnarli. L&#39;utente [engagement](/help/engagement.md) è costituito da feedback sia positivi che negativi e gli ISP monitorano sia su base costante. Non avere impegno è forse uno dei principali fattori che contribuiscono all&#39;impegno negativo. Dal punto di vista del recapito messaggi, l’invio coerente di campagne agli utenti che non mostrano alcun coinvolgimento può anche ridurre la reputazione complessiva del tuo indirizzo IP e dei tuoi domini.

Gli ISP come Gmail, Microsoft e OATH visualizzano il non coinvolgimento come e-mail indesiderata e iniziano a reindirizzare i messaggi alla cartella spam. Inoltre, questi abbonati non possono più possedere l&#39;account e-mail, e questo può essere utilizzato come una trappola di spam &quot;riciclata&quot;. Questo significa che l&#39;indirizzo non è valido da qualche tempo e che tutti i messaggi vengono rifiutati. Se il sistema di gestione degli abbonati non rimuove gli indirizzi &quot;rimbalzati rigidi&quot;, è molto probabile che spedire a spam trappole che possono portare a significativi problemi di consegna.

## Come si dovrebbe affrontare l&#39;inattività? {#how-should-you-approach-inactivity-}

I clienti che utilizzano la piattaforma Adobe possono visualizzare l’inattività all’interno della propria istanza esaminando i dati di apertura e facendo clic su in base al segmento. Poiché il mancato coinvolgimento può ostacolare la consegna, il primo pensiero può essere quello di rimuovere gli abbonati dal database. Tuttavia, a volte questa potrebbe rivelarsi un&#39;opzione sbagliata. Pertanto, una strategia di riimpegno (nota anche come riacquisto) è la migliore raccomandazione per mantenere gli abbonati interessati a ricevere la posta, e gradualmente eliminare quelli che non mostrano più attività.

## Le campagne di rilancio funzionano davvero? {#do-re-engagement-campaigns-really-work-}

Secondo uno studio Return Path, le campagne di riimpegno sono state lanciate con un tasso di apertura del 12% rispetto a una media del 14% per le campagne normali. Anche se solo il 24% degli abbonati aveva letto la campagna di rilancio, circa il 45% di loro ha letto i messaggi successivi.

![](../../help/assets/deliverability_implementation_1.png)

## Come si crea una campagna di coinvolgimento? {#how-do-you-create-a-re-engagement-campaign-}

### Fase 1 {#phase-1}

* Il primo passo è quello di identificare gli abbonati che hanno un’attività di apertura o clic molto limitata, e quindi segmentare questo gruppo in base a un intervallo di tempo impostato. La regola generale consiste nel rivedere gli abbonati che non hanno aperto o fatto clic su un’e-mail negli ultimi 90 giorni. Tuttavia, questo varia a seconda della natura dell&#39;attività (ad esempio, l&#39;invio stagionale).
* Un altro punto da tenere a mente durante la definizione dei tempi è che gli ISP e le aziende elenchi Bloccati considerano l&#39;impegno un posto da 1,5 a 1,8 anni. Inoltre, attività comportamentali come acquisti e attività sul sito web o altri punti di contatto, come le preferenze durante la fase di registrazione o il primo punto di contatto.

### Fase 2 {#phase-2}

* Una volta definito un segmento, il passaggio successivo consiste nel creare una campagna di nuovo coinvolgimento che si rivolga al sottoscrittore in base alle metriche identificate. La creazione di una riga dell’oggetto consente di aumentare l’interesse dell’utente iscritto. Secondo uno studio Return Path, le righe dell&#39;oggetto e il contenuto che dicono &quot;Ci manchi&quot; generano tassi di risposta più alti di &quot;Ti vogliamo indietro&quot;.
* È inoltre possibile offrire un incentivo per il riutilizzo dell’e-mail. Quando si considerano le offerte con sconti, è consigliabile utilizzare gli importi in dollari rispetto alle percentuali. Inoltre, Return Path suggerisce di eseguire questa operazione in quanto supporta tassi di risposta più elevati. Infine, è utile anche eseguire test suddivisi A/B per rivedere le percentuali di risposta e di successo.

### Fase 3 {#phase-3}

Il passaggio successivo consiste nel determinare la frequenza della campagna di ricoinvolgimento. A differenza dei messaggi di riconferma, le campagne di riimpegno hanno lo scopo di convincere l’utente a tornare con una serie di e-mail nel tempo. L&#39;esempio seguente fornisce un esempio di frequenza.

![](../../help/assets/deliverability_implementation_2.png)

Gli abbonati che partecipano alla campagna seguendo l’attività di apertura o clic vengono aggiunti nuovamente all’elenco degli abbonati coinvolti.

### Fase 4 {#phase-4}

* La fase successiva consiste nell’identificare gli abbonati che non mostrano mai più alcuna attività e ridurre gradualmente l’invio di e-mail a essi in un periodo di tempo. Se non c&#39;è attività nell&#39;ultimo anno, è bene mettere in pausa l&#39;abbonamento e-mail degli abbonati. Anche se non hanno mostrato alcun interesse per il contenuto dell’e-mail, esiste sempre un’ultima possibilità per riattivare l’abbonamento inviando una campagna di riconferma una tantum.
* Le campagne di riconferma sono un buon modo per chiedere agli abbonati che sono inattivi per un lungo periodo di tempo se desiderano rimanere nell’elenco di abbonamenti. Durante la creazione della campagna, è preferibile aggiungere un collegamento &quot;fai clic qui&quot; in modo che possano confermare l’azione e verificare il loro indirizzo. In questo modo, l&#39;azione può essere registrata nel database. Di seguito è riportato un esempio di e-mail di riconferma:

   ![](../../help/assets/deliverability_implementation_3.png)

   Una volta che l’utente iscritto ha agito, può essere offerta una pagina di destinazione con la conferma del suo nuovo abbonamento. Di seguito è riportato un esempio della pagina di destinazione:

   ![](../../help/assets/deliverability_implementation_4.png)

## Risorse specifiche per i prodotti

**Adobe Campaign**

* [Tracciamento dei registri in Campaign Classic](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/delivery-dashboard.html#tracking-logs)
* [Tracciamento dei registri in Campaign Standard](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/sending-and-tracking-messages/tracking-messages.html#tracking-logs)

**Gestione dei Percorsi cliente Adobe**

* [Tracciamento dei messaggi](https://experienceleague.adobe.com/docs/customer-journey-management/using/reporting/message-tracking.html)