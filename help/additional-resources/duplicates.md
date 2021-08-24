---
title: Duplicati
description: Scopri come identificare e limitare i duplicati per migliorare il recapito messaggi.
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
exl-id: f89dbb38-a8d4-4294-b017-6fed72591593
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 6%

---

# Duplicati {#duplicates}

Avere indirizzi e-mail duplicati può avere più conseguenze:

* Lo stesso messaggio viene inviato più di una volta. Anche se Adobe esegue una procedura di deduplicazione per impostazione predefinita prima dell’invio, non c’è nulla che impedisca l’invio dello stesso messaggio da parte di azioni diverse con lo stesso contenuto quando una destinazione viene divisa.
* Richieste di annullamento dell’abbonamento non rispettate. Se un destinatario annulla l’abbonamento dopo aver ricevuto un messaggio, il suo profilo duplicato sarà comunque idoneo per i messaggi futuri.

Oltre a questa fase secondaria delle procedure di consenso, questa situazione porterà probabilmente gli utenti a considerare i messaggi come spam e ad attivare una procedura di elenco Bloccati presso l&#39;ISP.

È necessario essere particolarmente prudenti quando si eseguono operazioni sul database:

* Le importazioni devono essere configurate meticolosamente, in particolare quando si sceglie la chiave di riconciliazione.
* Gli indirizzi e-mail modificati possono anche essere una fonte di duplicati. In particolare, due indirizzi con domini diversi possono essere indirizzati alla stessa cassetta postale, ad esempio se un&#39;azienda ha cambiato nome e ha mantenuto il dominio precedente per un certo periodo di tempo: joe.doe@amce-co.com e joe.doe@acme-rebranded.com.
* Le importazioni automatiche, sia di elenchi che di altri database, sono elementi da considerare nella gestione dei profili. Cosa succede quando si elimina o si sposta un profilo in un&#39;altra partizione? Può essere ricreato nella partizione iniziale tramite un&#39;importazione automatica, ad esempio, quando viene inserito un ordine di acquisto.
* L’archiviazione dei profili in cartelle diverse può essere implementata utilizzando viste anziché partizioni. In questo modo, sei sicuro che i profili si trovano nella stessa partizione fisica pur consentendo comunque la visualizzazione e la gestione dei diritti adeguati.

Ci sono, comunque, casi in cui i duplicati tra le diverse partizioni sono normali. Ad esempio, quando invii a terzi o a entità aziendali diverse, è logico che la stessa persona sia un destinatario per motivi diversi. Tuttavia, raramente è normale trovare duplicati all’interno della stessa partizione.

## Risorse specifiche per i prodotti

La deduplicazione degli indirizzi protegge la reputazione dell’invio e garantisce una buona gestione della quarantena. Ulteriori informazioni sono disponibili nelle seguenti sezioni della documentazione del prodotto:

**Adobe Campaign Classic**

* [Attività di deduplicazione](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/targeting-activities/deduplication.html)
* [Utilizzo della funzionalità di unione dell’attività Deduplication](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/deduplication-merge.html?lang=it)

**Adobe Campaign Standard**

* [Deduplicazione di dati da un file importato](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/workflow-use-case/data-management/deduplicating-data-imported-file.html)
