---
title: Aggiornare la qualifica di mancato recapito dopo l’interruzione di Italia Online
description: Scopri come aggiornare la qualifica di mancato recapito dopo l’interruzione di Italia Online
feature: Deliverability
exl-id: a11e88cf-bf37-42cc-9c09-1d58360459b7
hide: true
hidefromtoc: true
source-git-commit: aca77fb9326e34455a6fec7ffc9a7ad8e1750467
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 3%

---

# Aggiorna i mancati recapiti permanenti errati dopo l&#39;interruzione di Italiaonline {#update-bounce-italia}

## Contesto{#outage-context}

A partire dal 22 gennaio (ora locale), Italia Online ha subito un&#39;interruzione che ha causato diversi ritardi e il rifiuto delle e-mail. Il servizio ha iniziato a riprendere con capacità limitata il 26 gennaio.

I domini interessati sono: **libero.it**, **virgilio.it**, **inwind.it**, **iol.it**, e **blu.it**.

Questo problema si è verificato dal 1/22/2023 al 1/26/2023, ma la maggior parte delle quarantene erroneamente si sono verificate il 26 gennaio.

Ulteriori informazioni nella comunicazione ufficiale [qui](https://tecnologia.libero.it/avviato-il-ritorno-online-di-libero-mail-e-virgilio-mail-66832){_blank}.


## Impatto{#outage-impact}

Come nella maggior parte dei casi in cui si verifica un’interruzione del servizio di un provider di servizi Internet (ISP), alcune e-mail inviate tramite Campaign o Journey Optimizer sono state erroneamente contrassegnate come mancate consegne. Questo non ha avuto un impatto solo sugli Adobi, ma su tutti coloro che hanno cercato di ricevere e-mail inviate a Italia Online durante il periodo di interruzione.

I sintomi erano:

* **Mancati recapiti non permanenti** con il messaggio `452 requested action aborted: try again later` - questi sono stati ritentati automaticamente e non è necessaria alcuna azione.

* **Mancato recapito permanente** con il messaggio `550 <email address> recipient rejected` sono stati restituiti dall’ISP il 26 gennaio, tra le 8.00 e le 14.00 ora locale, per evitare che i mittenti continuino a sovraccaricare i loro server. Come confermato dal Postmaster di Italia Online, non si tratta di veri e propri hard bounce, per cui consigliamo di non mettere in quarantena tutti gli indirizzi e-mail che sono stati esclusi il 26 gennaio 2023 a causa di quel messaggio.

## Processo di aggiornamento{#outage-update}

### Adobe Campaign{#ac-update}

In base alla logica standard di gestione dei mancati recapiti, Adobe Campaign ha aggiunto automaticamente questi destinatari all’elenco di quarantena con una **[!UICONTROL Status]** impostazione di **[!UICONTROL Quarantine]**. Per risolvere questo problema, aggiorna la tabella di quarantena in Campaign trovando e rimuovendo questi destinatari o modificando i **[!UICONTROL Status]** a **[!UICONTROL Valid]** in modo che il flusso di lavoro di pulizia notturna li rimuova.

Per trovare i destinatari interessati da questo problema o nel caso in cui questo si verifichi nuovamente con un altro ISP, consulta le istruzioni di seguito:

* Per Campaign Classic v7 e Campaign v8, consulta [questa pagina](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}.
* Per Campaign Standard, consulta [questa pagina](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}.

### Adobe Journey Optimizer{#ajo-update}

In base alla logica standard di gestione dei mancati recapiti, Adobe Journey Optimizer ha aggiunto automaticamente questi indirizzi e-mail all’elenco di soppressione con una **[!UICONTROL Reason]** impostazione di **[!UICONTROL Invalid Recipient]**. Per risolvere questo problema, devi aggiornare l’elenco di soppressione trovando e rimuovendo questi indirizzi e-mail.

Una volta identificati, questi indirizzi possono essere rimossi manualmente dall’elenco di soppressione utilizzando **[!UICONTROL Delete]** pulsante. Questi indirizzi possono quindi essere inclusi nelle campagne e-mail future.

Ulteriori informazioni in [questa sezione](https://experienceleague.adobe.com/docs/journey-optimizer/using/configuration/monitor-reputation/manage-suppression-list.html#remove-from-suppression-list){_blank}.

