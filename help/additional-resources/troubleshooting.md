---
title: Risoluzione dei problemi di recapito
description: Scopri come identificare e risolvere i problemi di consegna dei messaggi.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 4cc85124-e7e4-4cd5-99a9-23d2d8cf08fe
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '677'
ht-degree: 0%

---

# Risoluzione dei problemi di recapito {#troubleshooting}

Di seguito sono riportate alcune best practice che possono essere utili per identificare e risolvere i problemi di recapito messaggi.

## Identificare un problema di recapito messaggi {#identify-deliverability-issue}

Per identificare un possibile problema, gli elementi elencati in [quella pagina](/help/ongoing-monitoring.md) potrebbero richiamare l&#39;attenzione.

<!--
Mailing or campaign metrics: unsubscribe, abuse complaint and/or bounce rates are higher than usual.
Subscriber activity: opens, clicks and/or transactions are lower than usual.
Seed accounts show filtered or non-delivered mailings.
-->

## Ipotizzare le possibili cause {#potential-causes}

Per identificare le possibili cause del problema di recapito messaggi, fai le seguenti domande:

* Si è verificato un cambiamento recente nella segmentazione degli elenchi?
* Sono state acquisite nuove origini dati?
* Ho inviato accidentalmente un file di quarantena?
* Il problema potrebbe essere dovuto al contenuto del messaggio?
* Invio messaggi con una frequenza sufficiente per mantenere gli IP caldi?
* Sto segmentando le mie e-mail per attività/coinvolgimento o inviando file completi?
* Qual è il segmento &quot;sicuro&quot; nel mio file in termini di aggiornamento recente?
* Sono attive strategie di riattivazione e riconferma per segmenti non definiti sicuri?

## Risolvere il problema {#address-issue}

### Reclami

[Reclami](/help/metrics/complaints.md) sono definiti dagli abbonati che segnalano le e-mail come spam premendo il pulsante corrispondente dalla loro casella in entrata.

Se il problema di consegna è stato causato da reclami:
* Devi cercare di determinare il motivo per cui i destinatari si lamentano.
* Puoi anche valutare la possibilità di spostare il collegamento per annullare l’abbonamento nella parte superiore dell’e-mail. Questo incoraggerà gli abbonati ad annullare l’abbonamento invece di lamentarsi con il pulsante di posta indesiderata.

I mittenti possono ottenere numerose informazioni dai loro [reclami del ciclo di feedback](/help/transition-process/infrastructure.md#feedback-loops):
* È importante archiviare i dati e cercare modelli in elementi come l’origine del consenso, per quanto tempo l’indirizzo è stato iscritto o persino determinati dati demografici comportamentali.
* I reclami possono spesso identificare un’origine dati o un segmento rischioso all’interno del file. Il rischio è definito come la più probabile causa di reclamo, che può danneggiare la reputazione e, a sua volta, le percentuali di posta in arrivo.

Le lamentele vengono anche dagli abbonati che semplicemente non vogliono più ricevere e-mail:
* Questo può essere spesso dovuto a messaggi eccessivi, alla percezione che gli abbonati hanno del messaggio, al fatto che non si aspettavano il messaggio o al fatto che non ricordano di aver acconsentito.
* È inoltre importante eseguire un controllo di audit per garantire che tutti i punti di raccolta siano chiari e che non vi siano caselle preselezionate nei punti di acquisizione.
* Dovresti anche inviare un’e-mail di benvenuto quando gli abbonati acconsentono a impostare il tono e spiegare con quale frequenza possono aspettarsi di ricevere e-mail da te.

### Validità dei dati

**Mancati recapiti permanenti** si verificano quando si invia a un **indirizzo non recapitato** presso un ISP. Un indirizzo può non essere recapitato per molti motivi, ad esempio:
* Indirizzo errato. Questo problema può essere risolto con un servizio di convalida dei dati in tempo reale, o richiedendo un consenso confermato prima di inviare e-mail di marketing a tale indirizzo.
* Elenco o origine dati non valida. Se proviene da una nuova origine, controlla come sono stati raccolti gli indirizzi e assicurati che sia stata concessa l’autorizzazione.
* Invio a un indirizzo che era precedentemente attivo, ma che è stato chiuso o terminato dopo un periodo di inattività.

### Coinvolgimento

Oltre ai reclami e alla validità dei dati, gli ISP si stanno concentrando più che mai su **coinvolgimento positivo** per prendere decisioni sulla consegna. Cercano di vedere se i tuoi abbonati stanno aprendo le tue e-mail o eliminandole senza leggerle. Poiché non condividono questi dati con i mittenti, dobbiamo utilizzare le informazioni disponibili e tradurre come coinvolgimento aperture, clic e transazioni.

Come parte della manutenzione della reputazione in corso, è importante comprendere in che modo gli abbonati coinvolti sono nel tuo elenco e sviluppare una **gerarchia dei rischi di aggiornamento** per gli abbonati su ciascun file. L’attualità è definita come ultima data di apertura, clic, transazione o iscrizione. Questo arco temporale può differire in senso verticale. Per eseguire questa operazione:

1. Determinare i segmenti attivi (&quot;sicuri&quot;) per ciascun segmento verticale. Si tratta in genere di abbonati che sono stati attivi negli ultimi 3-6 mesi.
1. Riduci la frequenza a inattiva.
1. Crea una serie [re-engagement](/help/additional-resources/re-engagement.md) per le inattività a rischio moderato. In genere sono 6-9 mesi senza coinvolgimento.
1. Sviluppa una campagna di riconferma per le inattività a rischio più elevato. Si tratta in genere di abbonati che non si sono impegnati con un’e-mail in 9-12 mesi.
1. Infine, imposta una regola di abbandono e rimuovi gli abbonati che non hanno aperto le e-mail in &quot;x&quot; mesi. In genere consigliamo più di 12 mesi, ma questo può variare in base al ciclo di vendita e acquisto.
