---
title: Best practice per coinvolgere di nuovo
description: Scopri come migliorare il recapito messaggi tramite strategie di ricoinvolgimento.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 30118706-d4c0-4bd8-8c9b-50c26b8374ef
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 1%

---

# Best practice per coinvolgere di nuovo {#re-engagement}

Durante l’implementazione del recapito messaggi, alcune delle best practice consistono nel cercare di mantenere una base di abbonati sana e migliorare il recapito messaggi tramite strategie di ricoinvolgimento (o riconquista).

* Mantenere una base di abbonati sana è uno degli aspetti principali per garantire una consegna buona e coerente. Molti problemi di recapito dei messaggi derivano da procedure di gestione e manutenzione dei dati non corrette.
* Uno dei problemi più comuni che gli esperti di marketing devono affrontare oggi è l’attività degli abbonati inattiva (nota anche come bassa o non partecipazione) che può influenzare negativamente la consegna delle e-mail e il basso ROI.

>[!NOTE]
>
>Per ulteriori informazioni sulle strategie per campagne di ricoinvolgimento e sui servizi di recapito messaggi dell’Adobe, contatta il tuo consulente di recapito messaggi o contatta il tuo agente di vendita Adobe.

## In che modo gli ISP visualizzano le attività di mancato coinvolgimento? {#how-do-isps-view-non-engagement-activity-}

Per anni, gli ISP hanno utilizzato le metriche di feedback del coinvolgimento dei loro utenti per decidere dove inserire i messaggi o se consegnarli. Utente [coinvolgimento](/help/engagement.md) consiste in feedback sia positivi che negativi e gli ISP monitorano entrambi su base costante. L&#39;assenza di coinvolgimento è forse uno dei principali fattori che contribuiscono all&#39;impegno negativo. Dal punto di vista del recapito messaggi, l’invio coerente di campagne agli utenti che non mostrano alcun coinvolgimento può anche ridurre la reputazione complessiva del tuo indirizzo IP e dei tuoi domini.

ISP come Gmail, Microsoft® e OATH visualizzano il mancato coinvolgimento come e-mail indesiderata e iniziano a reindirizzare i messaggi alla cartella di posta indesiderata. Inoltre, questi abbonati potrebbero non essere più proprietari dell’account e-mail e questo può essere utilizzato come trappola spam &quot;riciclata&quot;. Ciò significa che l’indirizzo non era valido per un certo periodo di tempo e che tutti i messaggi vengono rifiutati. Se il sistema di gestione degli abbonati non rimuove gli indirizzi &quot;hard bounce&quot;, è probabile che l’invio di trappole spam possa causare problemi di consegna significativi.

## Come affrontare l’inattività? {#how-should-you-approach-inactivity-}

I clienti che utilizzano la piattaforma Adobe possono visualizzare l’inattività all’interno dell’istanza rivedendo i dati aperti e facendo clic su di essi in base al segmento. Poiché il mancato coinvolgimento può ostacolare la consegna, la prima cosa da considerare è rimuovere gli abbonati dal database. Tuttavia, a volte questa potrebbe rivelarsi un&#39;opzione sbagliata. Pertanto, una strategia di ricoinvolgimento (nota anche come strategia di riconquista) è il miglior consiglio per mantenere gli abbonati che sono interessati a ricevere la posta, e gradualmente eliminare coloro che non mostrano più attività.

## Le campagne di ricoinvolgimento funzionano davvero? {#do-re-engagement-campaigns-really-work-}

Secondo uno studio sul percorso di ritorno, le campagne di ricoinvolgimento hanno avuto come risultato un tasso di apertura del 12%, rispetto a una media del 14% per le campagne normali. Anche se solo il 24% degli abbonati aveva letto la campagna di ricoinvolgimento, circa il 45% di loro ha letto i messaggi successivi.

![](../../help/assets/deliverability_implementation_1.png)

## Come si crea una campagna di coinvolgimento? {#how-do-you-create-a-re-engagement-campaign-}

### Fase 1 {#phase-1}

* Il primo passaggio consiste nell’identificare gli abbonati che hanno un’attività di apertura o clic minima o nulla e, di conseguenza, segmentare questo gruppo in base a un intervallo di tempo impostato. La regola empirica è quella di rivedere gli abbonati che non hanno aperto o fatto clic su un’e-mail negli ultimi 90 giorni. Tuttavia, questo varia a seconda della natura dell’attività (ad esempio, invio stagionale).
* Un altro aspetto da tenere presente quando si definiscono gli intervalli di tempo è che gli ISP e le aziende di elenco Bloccati di considerano il coinvolgimento come un periodo compreso tra 1,5 e 1,8 anni. Inoltre, attività comportamentali come acquisti e attività sul sito web o altri punti di contatto, come le preferenze durante la fase di abbonamento o il primo punto di contatto.

### Fase 2 {#phase-2}

* Una volta definito un segmento, il passaggio successivo consiste nel creare una campagna di ricoinvolgimento che soddisfi l’abbonato in base alle metriche identificate. La creazione di un oggetto consente di aumentare l’interesse dell’abbonato. Secondo uno studio sul percorso di ritorno, le righe dell’oggetto e i contenuti con lo stato &quot;Ci manchi&quot; generano tassi di risposta più elevati rispetto a &quot;Ti rivogliamo&quot;.
* È inoltre possibile offrire un incentivo per coinvolgere di nuovo l’e-mail. Quando si considerano le offerte con sconti, è meglio utilizzare gli importi in dollari rispetto alle percentuali. Return Path suggerisce anche di farlo in quanto si verificano tassi di risposta più elevati. Infine, anche l’esecuzione di test suddivisi A/B per esaminare i tassi di risposta e di successo è un’opzione utile.

### Fase 3 {#phase-3}

Il passaggio successivo consiste nel determinare la frequenza della campagna di ricoinvolgimento. A differenza dei messaggi di riconferma, le campagne di ricoinvolgimento hanno lo scopo di riconquistare l’abbonato con una serie di e-mail nel tempo. L’esempio seguente fornisce un esempio della frequenza.

![](../../help/assets/deliverability_implementation_2.png)

Gli abbonati che si impegnano con la campagna seguendo l’attività di apertura o clic vengono aggiunti nuovamente all’elenco degli abbonati coinvolti.

### Fase 4 {#phase-4}

* La fase successiva consiste nell’identificare gli abbonati che non mostrano continuamente alcuna attività e ridurre gradualmente l’invio di e-mail a loro in un determinato periodo di tempo. Se non è presente alcuna attività nell’ultimo anno, è consigliabile sospendere l’abbonamento e-mail degli abbonati. Anche se non hanno mostrato alcun interesse per il contenuto dell’e-mail, esiste sempre l’ultima possibilità di farli riattivare l’abbonamento inviando una campagna di riconferma una tantum.
* Le campagne di riconferma sono un buon modo per chiedere agli abbonati che sono inattivi da molto tempo se desiderano rimanere nell’elenco degli abbonamenti. Durante la creazione della campagna, è preferibile aggiungere un collegamento &quot;fai clic qui&quot; in modo che possano confermare l’azione e verificare il proprio indirizzo. In questo modo, l’azione può essere registrata nel database. Di seguito è riportato un esempio di e-mail di riconferma:

   ![](../../help/assets/deliverability_implementation_3.png)

   Una volta che l’abbonato ha intrapreso un’azione, è possibile offrire una pagina di destinazione con la conferma del suo riabbonamento. Di seguito è riportato un esempio della pagina di destinazione:

   ![](../../help/assets/deliverability_implementation_4.png)

## Risorse specifiche per il prodotto

**Adobe Campaign**

* [Registri di tracciamento in Campaign Classic](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/delivery-dashboard.html#tracking-logs)
* [Registri di tracciamento in Campaign Standard](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/sending-and-tracking-messages/tracking-messages.html#tracking-logs)

**Adobe di gestione dei Percorsi di clienti**

* [Tracciamento dei messaggi](https://experienceleague.adobe.com/docs/journey-optimizer/using/reporting/message-tracking.html?lang=it)
