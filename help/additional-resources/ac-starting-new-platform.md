---
title: Avvio di una nuova piattaforma
description: Scopri come gestire il recapito messaggi quando inizi una nuova piattaforma con Adobe Campaign.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 6c9ade01-3052-4311-af80-888294820024
TQID: https://experienceleague.adobe.com/cQa5nOTSJwxDGX-QkXGez5dpm5N-8I7QZ-LsEW0FRLo
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
  - id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87
  - id: dfc56824-e8b9-499e-85d4-21aedb507314
feature_v2:
  - id: a075b2c1-7748-4328-b7f6-343aa314616a
  - id: c5474392-5419-4296-9e41-f6f4ce4f6e9b
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
  - id: fdbb8fc9-ffa3-4b86-88fe-aa4c5a3e1bc6
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 75df8537199680e5f1fc4b98cefdb05220fee7bf
workflow-type: tm+mt
source-wordcount: 666
ht-degree: 9%

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

* [Ottimizzare la consegna tramite quarantena](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [Identificare gli indirizzi messi in quarantena per l’intera piattaforma](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#identifying-quarantined-addresses-for-the-entire-platform)
* [Invio in più ondate](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html#sending-using-multiple-waves)
* [Monitoraggio della consegna](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/about-delivery-monitoring.html?lang=it#sending-messages)

**Adobe Campaign Standard**

* [Ottimizzare la consegna tramite quarantena](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [Identificare gli indirizzi messi in quarantena per l’intera piattaforma](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html)
* [Monitoraggio di una consegna](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/monitoring-a-delivery.html?lang=it)
