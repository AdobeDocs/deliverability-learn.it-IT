---
title: E-mail non consegnate
description: Scopri i diversi tipi di e-mail non consegnate.
topics: Deliverability
kt: 7047
thumbnail: kt7047.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 6338eb67-3efd-476e-8b26-97bbb6a1d35f
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: ht
source-wordcount: '477'
ht-degree: 100%

---

# E-mail non consegnate

Le e-mail non consegnate sono il risultato di un tentativo e di un errore di consegna in cui l’ISP (fornitore di servizi Internet) genera messaggi di errore. La gestione delle e-mail non consegnate è una parte fondamentale dell’igiene degli elenchi. Quando una data e-mail non viene consegnate diverse volte di fila, questo processo la contrassegna per l’eliminazione. Il numero e il tipo di e-mail non consegnate necessari per attivare l’eliminazione variano da sistema a sistema. Questo processo impedisce ai sistemi di continuare a inviare messaggi a indirizzi e-mail non validi. Le e-mail non consegnate sono uno dei dati chiave utilizzati dagli ISP per determinare la reputazione dell’IP. È molto importante tenere d’occhio questa metrica. Le classificazioni “Consegnato” e “Non consegnato” sono probabilmente il modo più comune per misurare la consegna dei messaggi di marketing: più alta è la percentuale di messaggi consegnati, meglio è.

Scopri di seguito due diversi tipi di e-mail non consegnate.

## Mancati recapiti permanenti

I mancati recapiti permanenti sono errori permanenti generati dopo che un ISP determina che un tentativo di invio di e-mail all’indirizzo di un abbonato è impossibile da consegnare. In Adobe Campaign, i mancati recapiti permanenti classificati come impossibili da recapitare vengono aggiunti alla quarantena, il che significa che la consegna non sarà ritentata. Ci sono alcuni casi in cui un mancato recapito permanente verrebbe ignorato se la causa dell’errore fosse sconosciuta.
Di seguito sono riportati alcuni esempi comuni di mancati recapiti permanenti:

* Indirizzo inesistente
* Account disattivato
* Sintassi errata
* Dominio errato

## Mancato recapito non permanente

I mancati recapiti non permanenti sono errori temporanei generati dagli ISP in caso di problemi nella consegna di un’e-mail. I tentativi di consegna in caso di errori per mancati recapiti non permanenti saranno ripetuti più volte (con varianza in base all’utilizzo di impostazioni di consegna personalizzate o predefinite) per cercare di effettuare una consegna corretta. Gli indirizzi che generano mancanti recapiti non permanenti in modo continuo non saranno aggiunti alla quarantena fino a quando non sarà raggiunto il numero massimo di tentativi (che, anche in questo caso, varia in base alle impostazioni). Di seguito alcune cause comuni di mancati recapiti non permanenti:

* Casella in entrata piena
* Server di posta elettronica di destinazione non accessibile
* Problemi di reputazione del mittente

![tipi di mancato recapito](../assets/bounce-types.png)

>[!NOTE]
>
>I mancati recapiti sono indicatori chiave di un problema di reputazione, in quanto possono evidenziare un’origine dati errata (mancati recapiti permanenti) o un problema di reputazione con un ISP (mancati recapiti non permanenti).
>
>I mancati recapiti non permanenti si verificano spesso durante l’invio di e-mail e dovrebbero poter essere risolti durante l’elaborazione dei nuovi tentativi prima di essere definiti veri e propri problemi di consegna. Se il tasso di mancati recapiti non permanenti è superiore al 30% per un singolo ISP e non si risolve entro 24 ore, è consigliabile segnalare il problema al consulente del team Deliverability di Adobe Campaign.

## Risorse specifiche per i prodotti

**Adobe Campaign Classic**

* [Tipi e motivi di errori di consegna](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html?lang=it#delivery-failure-types-and-reasons)
* [Gestione delle mail non recapitate](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html?lang=it#bounce-mail-management)
* [Rapporti sui messaggi non recapitati](https://experienceleague.adobe.com/docs/campaign-classic/using/reporting/reports-on-deliveries/global-reports.html?lang=it#non-deliverables-and-bounces)

**Adobe Campaign Standard**

* [Tipi e motivi di errori di consegna](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-delivery-failures.html?lang=it#delivery-failure-types-and-reasons)
* [Qualificazione di mail non recapitate](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-delivery-failures.html?lang=it#bounce-mail-qualification)
* [Rapporto di riepilogo sulle mail non recapitate](https://experienceleague.adobe.com/docs/campaign-standard/using/reporting/list-of-reports/bounce-summary.html?lang=it#reporting)
