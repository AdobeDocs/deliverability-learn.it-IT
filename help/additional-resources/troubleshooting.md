---
title: Risoluzione dei problemi di recapito messaggi
description: Scopri come identificare e risolvere i problemi di recapito messaggi.
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
source-wordcount: '672'
ht-degree: 1%

---


# Risoluzione dei problemi di recapito {#troubleshooting}

Di seguito sono riportate alcune best practice che possono aiutare a identificare e risolvere i problemi di recapito messaggi.

## Identificare un problema di recapito messaggi {#identify-deliverability-issue}

Per identificare un possibile problema, gli elementi elencati in [tale pagina](/help/ongoing-monitoring.md) possono attirare la tua attenzione.

<!--
Mailing or campaign metrics: unsubscribe, abuse complaint and/or bounce rates are higher than usual.
Subscriber activity: opens, clicks and/or transactions are lower than usual.
Seed accounts show filtered or non-delivered mailings.
-->

## Le possibili cause dell&#39;ipotetdimensioni {#potential-causes}

Fai le seguenti domande per identificare le possibili cause del problema di recapito messaggi:

* C&#39;è stato un recente cambiamento nella segmentazione dell&#39;elenco?
* Ho acquisito nuove fonti di dati?
* Ho inviato accidentalmente un file in quarantena?
* Il problema potrebbe essere dovuto al mio contenuto del messaggio?
* Invia frequentemente abbastanza per mantenere gli IP caldi?
* Sto segmentando le mie invii per attività/impegno, o inviando file completi?
* Qual è il segmento &quot;sicuro&quot; del file in termini di aggiornamento?
* Sono state implementate strategie di riattivazione e riconferma per i segmenti che non sono definiti sicuri?

## Risolvere il problema {#address-issue}

### Reclami

[](/help/metrics/complaints.md) I reclami sono definiti dagli abbonati che segnalano e-mail come spam premendo il pulsante corrispondente dalla loro casella in entrata.

Se il problema di consegna è stato causato da reclami:
* Devi cercare di determinare perché i destinatari si lamentano.
* Puoi anche considerare di spostare il collegamento per l’annullamento all’abbonamento nella parte superiore dell’e-mail. Questo incoraggerà gli abbonati a annullare l’iscrizione invece di lamentarsi con il pulsante spam.

I mittenti possono raccogliere una grande quantità di informazioni dai loro [cicli di feedback](/help/transition-process/infrastructure.md#feedback-loops) reclami:
* È importante mettere da parte i dati e cercare pattern in elementi come la sorgente di consenso, per quanto tempo l’indirizzo è stato sottoscritto o anche in determinate caratteristiche demografiche comportamentali.
* I reclami possono spesso identificare un’origine dati o un segmento rischiosi all’interno del file. Risky è definito come più probabile lamentarsi, che può danneggiare la reputazione, e a sua volta, tariffe casella in entrata.

I reclami provengono anche dagli abbonati che semplicemente non desiderano più ricevere e-mail:
* Questo può spesso essere dovuto a messaggi in eccesso, la percezione del messaggio da parte degli abbonati, che non si aspettavano il messaggio o che non si ricordavano di aver acconsentito.
* È inoltre importante eseguire un controllo di audit per garantire che tutti i punti di raccolta siano chiari e che nei punti di acquisizione non siano presenti caselle di controllo preselezionate.
* È inoltre necessario inviare un messaggio e-mail di benvenuto quando gli abbonati decidono di impostare il tono e spiegare quanto spesso possono aspettarsi di ricevere e-mail da te.

### Validità dei dati

**I** messaggi non recapitati rigidi si verificano quando si invia a un  **indirizzo non recapitato** a un ISP. Un indirizzo può non essere recapitato per molti motivi, ad esempio:
* Indirizzo errato. Questo problema può essere risolto con un servizio di convalida dei dati in tempo reale o richiedendo un consenso confermato prima di inviare e-mail di marketing a tale indirizzo.
* Elenco o origine dati non validi. Se proviene da una nuova origine, controlla in che modo gli indirizzi sono stati raccolti e assicurati che sia presente l’autorizzazione.
* Invio a un indirizzo che era in una volta attivo, ma è stato chiuso o terminato dopo un periodo di inattività.

### Coinvolgimento

Oltre ai reclami e alla validità dei dati, gli ISP si stanno concentrando più che mai sul **coinvolgimento positivo** per prendere decisioni sulla consegna. Stanno cercando di vedere se i tuoi abbonati stanno aprendo le tue e-mail, o cancellarle senza leggerle. Poiché non condividono questi dati con i mittenti, dobbiamo utilizzare le informazioni disponibili e tradurre aperture/clic/transazioni come coinvolgimento.

Come parte della manutenzione continua della reputazione, è importante comprendere in che modo gli abbonati sono inseriti nel tuo elenco e sviluppare una **gerarchia dei rischi di recency** per gli abbonati su ciascun file. La recency è definita come data dell’ultima apertura/clic/transazione o registrazione. Questo intervallo temporale può differire in verticale. Per eseguire questa operazione:

1. Determinare i segmenti attivi (&quot;sicuri&quot;) per ogni verticale. Si tratta in genere di abbonati che sono stati attivi negli ultimi 3-6 mesi.
1. Riduci la frequenza agli inattivi.
1. Crea una serie [re-engagement](/help/additional-resources/re-engagement.md) per inattività dei rischi moderati. Questo è tipicamente 6-9 mesi senza impegno.
1. Sviluppa una campagna di riconferma per attività a rischio più elevato. Si tratta in genere di abbonati che non hanno mai effettuato un’e-mail in 9-12 mesi.
1. Infine, imposta una regola di drop-off e rimuovi gli abbonati che non hanno aperto le tue e-mail in &#39;x&#39; mesi. In genere consigliamo più di 12 mesi, ma questo può differire in base al ciclo di vendita e di acquisto.