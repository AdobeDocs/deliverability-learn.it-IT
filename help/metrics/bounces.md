---
title: Rimbalzi
description: Scopri i diversi tipi di messaggi non recapitati
feature: Metriche
topics: Deliverability
kt: 7047
thumbnail: kt7047.jpg
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: d42a8c3b06308fca0cf3e9db8d634a767fc0cdc6
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 2%

---


# Rimbalzi

I rimbalzi sono il risultato di un tentativo di consegna e di un errore in cui l&#39;ISP fornisce avvisi di errore di back. La lavorazione dei rifiuti è una parte fondamentale dell&#39;igiene degli elenchi. Dopo che una data e-mail è stata rimbalzata diverse volte di fila, questo processo la contrassegna per la soppressione. Il numero e il tipo di messaggi non recapitati necessari per attivare la soppressione variano da sistema a sistema. Questo processo impedisce ai sistemi di continuare a inviare indirizzi e-mail non validi. I rimbalzi sono uno dei dati chiave utilizzati dagli ISP per determinare la reputazione IP. È molto importante tenere d&#39;occhio questa metrica. &quot;Consegnato&quot; rispetto a &quot;rimbalzato&quot; è probabilmente il modo più comune per misurare la consegna dei messaggi di marketing: più alta è la percentuale consegnata, meglio è.

Ci immergeremo in due diversi tipi di rimbalzi.

## Rimbalzi netti

I mancati recapiti rigidi sono errori permanenti generati dopo che un ISP determina un tentativo di invio di posta a un indirizzo utente iscritto come non recapitato. In Adobe Campaign, i messaggi non recapitati rigidi classificati come non recapitati vengono aggiunti alla quarantena, il che significa che non verranno tentati nuovamente. Ci sono alcuni casi in cui un rimbalzo duro verrebbe ignorato se la causa del fallimento è sconosciuta.
Di seguito sono riportati alcuni esempi comuni di rimbalzi duri:

* L&#39;indirizzo non esiste
* Account disabilitato
* Sintassi errata
* Dominio non valido

## Rimbalzi morbidi

I messaggi non recapitati morbidi sono errori temporanei generati dagli ISP in caso di difficoltà nella consegna della posta. Gli errori software ripeteranno più volte (con varianza a seconda dell’utilizzo delle impostazioni di consegna personalizzate o predefinite) per tentare di eseguire una consegna corretta. Gli indirizzi che rimbalzano continuamente morbido non verranno aggiunti alla quarantena fino a quando non viene tentato il numero massimo di tentativi (che variano di nuovo a seconda delle impostazioni). Alcune cause comuni di non recapiti morbidi includono:

* Cassetta postale piena
* Ricezione del server e-mail in corso
* Problemi di reputazione del mittente

![tipi di rimbalzo](../assets/bounce-types.png)

>[!NOTE]
>
>I rimbalzi sono un indicatore chiave di un problema di reputazione perché possono evidenziare una sorgente dati errata (rimbalzo difficile) o un problema di reputazione con un ISP (rimbalzo morbido).
>
>I mancati recapiti morbidi si verificano spesso come parte dell’e-mail di invio e dovrebbero essere autorizzati a risolvere durante l’elaborazione dei nuovi tentativi prima di caratterizzarsi come un problema di consegna effettiva. Se il tasso di mancato recapito è superiore al 30% per un singolo ISP e non si risolve entro 24 ore, è consigliabile sollevare un problema con il tuo consulente di recapito messaggi Adobe Campaign.

## Risorse specifiche per i prodotti

**Adobe Campaign Standard**

* [Elenco dei report - Riepilogo messaggi non recapitati (documentazione del prodotto)](https://experienceleague.adobe.com/docs/campaign-standard/using/reporting/list-of-reports/bounce-summary.html?lang=en#reporting)
* [Informazioni sugli errori di consegna (documentazione del prodotto)](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-delivery-failures.html?lang=en#about-delivery-failures)

**Adobe Campaign Classic**

* [Informazioni sugli errori di consegna (documentazione del prodotto)](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html?lang=en#sending-messages)