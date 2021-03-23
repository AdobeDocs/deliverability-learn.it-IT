---
title: Avvio di una nuova piattaforma
description: Ulteriori informazioni sulla gestione del recapito messaggi all’avvio di una nuova piattaforma con Adobe Campaign.
feature: Attuazione
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 1e539b5df54250a5927701009e7a9c84e5d73fae
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 3%

---


# Avvio di una nuova piattaforma {#starting-new-platform}

Mantenere la reputazione del dominio e dell’indirizzo IP è fondamentale quando si configura una nuova piattaforma da utilizzare con Adobe Campaign.

## Passaggio sensibile

Devi prestare molta attenzione quando inizi a inviare e-mail su una nuova piattaforma, perché la piattaforma non dispone di alcuna cronologia di utilizzo e non gode di alcuna reputazione quando gli IP di invio non sono mai stati utilizzati a questo scopo.

Gli ISP sono naturalmente sospettosi degli indirizzi IP che non sono mai stati utilizzati per inviare e-mail e che improvvisamente iniziano a inviare grandi volumi di traffico e-mail. In effetti, gli spammer generalmente utilizzano indirizzi IP &quot;sconosciuti&quot; (indirizzi che non sono mai stati in elenco Bloccati) per inviare il maggior numero possibile di messaggi prima del rilevamento.

Non ci si può aspettare di raggiungere la velocità operativa in termini di output all&#39;inizio della fase di produzione. Inoltre, non devi tentare di inviare messaggi a questo ritmo in quanto potrebbe indurre gli ISP a bloccare gli indirizzi di invio e a compromettere gravemente il resto della fase di avvio.

## Principi fondamentali

Di seguito sono elencati i principi principali da seguire all’avvio di una nuova piattaforma.

* Configura un sottodominio dedicato specifico per le campagne e-mail inviate da Adobe.

* Se disponi di queste informazioni, **importa indirizzi non validi nella tabella di quarantena**.
L’avvio di una piattaforma spesso avviene quando si utilizza per la prima volta un elenco di indirizzi che potrebbero non essere completamente qualificati. Se invii indirizzi non validi o a indirizzi non validi, ciò contribuirà a ridurre la reputazione della piattaforma.

   * Se disponi di un elenco di indirizzi non validi, è nell’interesse dell’utente importarlo nella tabella di quarantena prima di inviarlo per la prima volta. La tabella di quarantena è disponibile tramite i menu **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Non deliverables and addresses]** (Campaign Classic) e **[!UICONTROL Administration > Channels > Quarantines > Addresses]** (Campaign Standard).

   * Se, comunque, si desidera riqualificare gli indirizzi non validi, è di gran lunga preferibile farlo una volta che la reputazione della piattaforma è stabilita e bit per pezzo al fine di &quot;diluire&quot; l&#39;uso di indirizzi non validi nel tempo.

* **Limita il** tasso di throughput limitando il numero di cache. Per ulteriori informazioni sulla regolazione di tale impostazione tecnica, contatta il tuo amministratore Adobe Campaign.

* **Aumentare progressivamente i volumi** inviati per evitare di essere contrassegnati come spam. Non eseguire il targeting dell&#39;intero database fin dall&#39;inizio, ma aggiungere una frazione extra dell&#39;elenco ogni volta che si invia. Questo dovrebbe consentire di aumentare il volume a ogni passaggio riducendo al contempo il tasso complessivo di indirizzi non validi. Per garantire uno sviluppo uniforme della fase di avvio, è possibile utilizzare le onde.

* **Invia regolarmente**. In un certo senso, è meglio inviare regolarmente piccole riprese piuttosto che sporadicamente campagne di grandi dimensioni.
* **Presta particolare attenzione ai rapporti** di consegna. Indicatori di errore elevati possono indicare che un&#39;impostazione tecnica non è configurata correttamente.

## Risorse aggiuntive

Per ulteriori informazioni sui principi elencati sopra e sulla loro implementazione con Adobe Campaign, consulta le sezioni seguenti:

* [Aumenta la reputazione dell&#39;e-mail con il riscaldamento dell&#39;IP](../../help/additional-resources/increase-reputation-with-ip-warming.md)
* [Tutto su spam trappole](../../help/additional-resources/all-about-spam-traps.md)

**Adobe Campaign Classic**

* [Ottimizzazione della consegna tramite quarantena](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [Identificare gli indirizzi messi in quarantena per l’intera piattaforma](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#identifying-quarantined-addresses-for-the-entire-platform)
* [Invio tramite più ondate](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html#sending-using-multiple-waves)
* [Monitoraggio della consegna](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/about-delivery-monitoring.html#sending-messages)

**Adobe Campaign Standard**

* [Ottimizzazione della consegna tramite quarantena](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [Identificare gli indirizzi messi in quarantena per l’intera piattaforma](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html)
* [Monitoraggio di una consegna](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/monitoring-a-delivery.html)
