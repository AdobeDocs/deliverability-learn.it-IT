---
title: Aggiornamento qualificazione rimbalzo dopo l'interruzione online di Italia
description: Scopri come aggiornare la qualifica di rimbalzo dopo Italia Online outage
feature: Deliverability
exl-id: a11e88cf-bf37-42cc-9c09-1d58360459b7
hide: true
hidefromtoc: true
source-git-commit: 016d7f9da67193d893e762fbe6e191cf87d5b030
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 1%

---

# Aggiorna non corretti rimbalzi hard dopo l&#39;interruzione online di Italia {#update-bounce-italia}

## Contesto{#outage-context}

A partire dal 22 gennaio (ora locale), Italia Online ha subito un&#39;interruzione che ha causato diversi ritardi e ha rifiutato le e-mail. Il servizio ha iniziato a riprendere con capacità limitata il 26 gennaio.

I domini interessati sono: **libero.it**, **virgilio.it**, **inwind.it**, **iol.it** e **blu.it**.

Questo problema si è verificato dal 22/01/2023 al 26/1/2023, ma la maggior parte delle quarantene erroneamente è avvenuta il 26 gennaio.

Ulteriori informazioni nella comunicazione ufficiale [qui](https://tecnologia.libero.it/avviato-il-ritorno-online-di-libero-mail-e-virgilio-mail-66832){_blank}.


## Impatto{#outage-impact}

Come nella maggior parte dei casi in cui si verifica un’interruzione di un ISP, alcune e-mail inviate tramite Campaign sono state erroneamente contrassegnate come mancate consegne. Questo non ha avuto solo un impatto sull&#39;Adobe, ma tutti quelli che cercano di ottenere e-mail consegnata a Italia Online durante la durata dell&#39;interruzione.

I sintomi erano:

* **Rimbalzi differiti** con il messaggio `452 requested action aborted: try again later` - sono stati automaticamente riprovati e non sono necessarie azioni.

* **Rimbalzi netti** con il messaggio `550 <email address> recipient rejected` sono stati restituiti dal provider di servizi Internet il 26 gennaio, tra le 8.00 e le 2.00 ora locale, per impedire ai mittenti di continuare a sovraccaricare i propri server. Come confermato dal postmaster online Italia, questi non sono veri e propri rimbalzi duri, quindi si consiglia di non mettere in quarantena tutti gli indirizzi e-mail che sono stati esclusi il 26 gennaio 2023 a causa di quel messaggio.

## Processo di aggiornamento{#outage-update}

### Adobe Campaign{#ac-update}

Per logica standard di gestione dei messaggi non recapitati, Adobe Campaign ha aggiunto automaticamente questi destinatari all’elenco di quarantena con un **[!UICONTROL Status]** definizione **[!UICONTROL Quarantine]**. Per correggere questo problema, devi aggiornare la tabella di quarantena in Campaign trovando e rimuovendo questi destinatari o modificando i relativi **[!UICONTROL Status]** a **[!UICONTROL Valid]** in modo che il flusso di lavoro di pulizia notturna li rimuova.

Per trovare i destinatari interessati da questo problema, o nel caso in cui si verifichi di nuovo con un altro ISP, consulta le istruzioni riportate di seguito:

* Per Campaign Classic v7 e Campaign v8, consulta [questa pagina](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}.
* Per Campaign Standard, consulta [questa pagina](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}.

### Adobe Journey Optimizer{#ajo-update}

In base alla logica standard di gestione dei messaggi non recapitati, Adobe Journey Optimizer ha aggiunto automaticamente questi indirizzi e-mail all’elenco di eliminazione con un **[!UICONTROL Reason]** definizione **[!UICONTROL Invalid Recipient]**. Per correggere questo problema, è necessario aggiornare l’elenco di soppressione individuando e rimuovendo questi indirizzi e-mail.

Una volta identificati, questi indirizzi possono essere rimossi manualmente dall’elenco di soppressione utilizzando **[!UICONTROL Delete]** pulsante . Questi indirizzi possono quindi essere inclusi nelle campagne e-mail future.

Ulteriori informazioni in [questa sezione](https://experienceleague.adobe.com/docs/journey-optimizer/using/configuration/monitor-reputation/manage-suppression-list.html#remove-from-suppression-list){_blank}.

