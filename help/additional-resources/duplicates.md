---
title: Duplicati
description: Scopri come identificare e limitare i duplicati per migliorare il recapito messaggi.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: f89dbb38-a8d4-4294-b017-6fed72591593
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 6%

---

# Duplicati {#duplicates}

La presenza di indirizzi e-mail duplicati può avere diverse conseguenze:

* Lo stesso messaggio viene inviato più volte. Anche se Adobe esegue una procedura di deduplicazione per impostazione predefinita prima dell’invio, nulla impedisce che lo stesso messaggio venga inviato da azioni diverse con lo stesso contenuto quando un target viene suddiviso.
* Richieste di annullamento dell’abbonamento non soddisfatte. Se un destinatario annulla l’iscrizione dopo aver ricevuto un messaggio, il suo profilo duplicato sarà comunque idoneo per i messaggi futuri.

Oltre a queste procedure di opt-in, questa situazione porterà probabilmente gli utenti a considerare i messaggi come spam e a attivare una procedura di inserisce nell&#39;elenco Bloccati di presso l&#39;ISP.

È necessario essere particolarmente prudenti quando si eseguono operazioni sul database:

* Le importazioni devono essere configurate meticolosamente, in particolare quando si sceglie la chiave di riconciliazione.
* Gli indirizzi e-mail modificati possono anche essere fonte di duplicati. In particolare, due indirizzi con domini diversi possono essere indirizzati alla stessa cassetta postale, ad esempio se un’azienda ha cambiato nome e ha mantenuto il dominio precedente per un certo periodo di tempo: joe.doe@amce-co.com e joe.doe@acme-rebranded.com.
* Le importazioni automatiche, sia di elenchi che da altri database, sono elementi da considerare durante la gestione dei profili. Cosa succede quando si elimina o si sposta un profilo in un&#39;altra partizione? Può essere ricreato nella partizione iniziale tramite un’importazione automatica, ad esempio quando viene inserito un ordine di acquisto.
* L’archiviazione di profili in cartelle diverse può essere implementata utilizzando viste anziché partizioni. In questo modo, sei sicuro che i profili si trovino nella stessa partizione fisica pur continuando a consentire la visualizzazione e la gestione dei diritti adeguati.

Esistono, tuttavia, casi in cui i duplicati tra le diverse partizioni sono normali. Ad esempio, quando invii per terze parti o entità aziendali diverse, è logico che la stessa persona sia un destinatario per motivi diversi. Tuttavia, raramente è normale trovare duplicati all’interno della stessa partizione.

## Risorse specifiche per i prodotti

La deduplicazione degli indirizzi protegge la reputazione del mittente e garantisce una buona gestione della quarantena. Per ulteriori informazioni, consulta le sezioni seguenti della documentazione del prodotto:

**Adobe Campaign Classic**

* [Attività di deduplicazione](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/targeting-activities/deduplication.html)
* [Utilizzo della funzionalità di unione dell’attività Deduplicazione](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/deduplication-merge.html?lang=it)

**Adobe Campaign Standard**

* [Deduplicazione di dati da un file importato](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/workflow-use-case/data-management/deduplicating-data-imported-file.html)
