---
title: Avvio di una nuova piattaforma
description: Scopri come gestire il recapito messaggi quando inizi una nuova piattaforma con Adobe Campaign.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 6c9ade01-3052-4311-af80-888294820024
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 6%

---

# Avvio di una nuova piattaforma {#starting-new-platform}

Mantenere la reputazione del dominio e dell’indirizzo IP è essenziale durante la configurazione di una nuova piattaforma da utilizzare con Adobe Campaign.

## Un passaggio sensibile

Devi prestare molta attenzione quando inizi a inviare e-mail su una nuova piattaforma, perché la piattaforma non ha alcuna cronologia d’uso e non ha alcuna reputazione quando gli IP di invio non sono mai stati utilizzati a questo scopo.

Gli ISP sono naturalmente sospettosi degli indirizzi IP che non sono mai stati utilizzati per inviare e-mail e che improvvisamente iniziano a inviare grandi quantità di traffico e-mail. Infatti, gli spammer generalmente utilizzano indirizzi IP &quot;sconosciuti&quot; (indirizzi che non sono mai stati su inserisce nell&#39;elenco Bloccati di) per inviare il maggior numero possibile di messaggi prima del rilevamento.

Non ci si può aspettare di raggiungere la velocità operativa in termini di output all&#39;inizio della fase di produzione. Inoltre, non devi tentare di inviare messaggi a questa velocità in quanto potrebbe portare gli ISP a bloccare gli indirizzi di invio e a compromettere gravemente il resto della fase di avvio.

## Principi fondamentali

Di seguito sono elencati i principi principali da seguire per l’avvio di una nuova piattaforma.

* Configura un sottodominio dedicato specifico per le campagne e-mail inviate da Adobe.

* Se si dispone di queste informazioni, **importare indirizzi non validi nella tabella di quarantena**.
L’avvio di una piattaforma si verifica spesso quando si utilizza per la prima volta un elenco di indirizzi che potrebbero non essere completamente qualificati. Se invii a indirizzi non validi o a indirizzi honeypot, questo contribuirà a ridurre la reputazione della piattaforma.

   * Se disponi di un elenco di indirizzi non validi, è nel tuo interesse importarlo nella tabella di quarantena prima di inviarlo per la prima volta. La tabella di quarantena è disponibile tramite i menu **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Non deliverables and addresses]** (Campaign Classic) e **[!UICONTROL Administration > Channels > Quarantines > Addresses]** (Campaign Standard).

   * Se, tuttavia, si desidera riqualificare gli indirizzi non validi, è di gran lunga preferibile farlo una volta che la reputazione della piattaforma è stabilita e bit per bit al fine di &quot;diluire&quot; l&#39;uso di indirizzi non validi nel tempo.

* **Limitare la velocità effettiva** limitando il numero di mtachild. Per ulteriori informazioni sulla regolazione di tali impostazioni tecniche, contatta il tuo amministratore Adobe Campaign.

* **Aumentare progressivamente i volumi inviati** per evitare di essere contrassegnati come spam. Non eseguire il targeting dell’intero database dall’inizio, ma aggiungere una frazione aggiuntiva dell’elenco ogni volta che si invia. Questo dovrebbe consentirti di aumentare il volume in ogni passaggio riducendo la percentuale complessiva di indirizzi non validi. Per garantire un regolare sviluppo della fase di avvio, è possibile utilizzare le onde.

* **Invia regolarmente**. In una certa misura, è meglio inviare piccole riprese regolarmente piuttosto che campagne sporadiche di grandi dimensioni.
* **Presta particolare attenzione ai report di consegna**. Indicatori di errore elevati possono indicare che un’impostazione tecnica non è configurata correttamente.

## Risorse aggiuntive

Per ulteriori informazioni sui principi elencati sopra e sulla loro implementazione con Adobe Campaign, consulta le sezioni seguenti:

* [Aumentare la reputazione e-mail con la preparazione degli indirizzi IP](../../help/additional-resources/increase-reputation-with-ip-warming.md)
* [Tutto sulle trappole anti-spam](../../help/additional-resources/all-about-spam-traps.md)

**Adobe Campaign Classic**

* [Ottimizza la consegna tramite quarantena](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [Identificare gli indirizzi messi in quarantena per l&#39;intera piattaforma](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#identifying-quarantined-addresses-for-the-entire-platform)
* [Invio tramite più scaglioni](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html#sending-using-multiple-waves)
* [Monitoraggio della consegna](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/about-delivery-monitoring.html?lang=it#sending-messages)

**Adobe Campaign Standard**

* [Ottimizza la consegna tramite quarantena](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [Identificare gli indirizzi messi in quarantena per l&#39;intera piattaforma](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html)
* [Monitoraggio di una consegna](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/monitoring-a-delivery.html?lang=it)
