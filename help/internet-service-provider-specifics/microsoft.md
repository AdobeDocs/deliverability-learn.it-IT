---
title: Microsoft (Hotmail, Outlook, Windows Live, ecc.)
description: Microsoft è generalmente il secondo o il terzo fornitore più grande a seconda della composizione dell’elenco e gestisce il traffico in modo leggermente diverso rispetto agli altri ISP.
topics: Deliverability
jira: KT-5319
doc-type: article
activity: understand
role: Admin, Leader, User
level: Beginner
team: TM
exl-id: d706cb90-828a-4ab3-8f93-c9bd71553d63
source-git-commit: 6b312cdbba496818337c97ec4f42962aea757901
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 1%

---

# [!DNL Microsoft] ([!DNL Hotmail], [!DNL Outlook], [!DNL Windows Live], ecc.)

[!DNL Microsoft] è in genere il secondo o il terzo provider più grande a seconda della composizione dell&#39;elenco e gestisce il traffico in modo leggermente diverso rispetto agli altri ISP.

Ecco alcuni punti salienti:

## Quali dati sono importanti

[!DNL Microsoft] si concentra sulla reputazione del mittente, sui reclami, sul coinvolgimento dell&#39;utente e sul proprio gruppo di utenti attendibili (noti anche come dati di reputazione del mittente o SRD) per i quali viene eseguito il polling per ottenere un feedback.

## Quali dati rendono disponibili

Lo strumento proprietario per la generazione di rapporti sui mittenti [!DNL Smart Network Data Services] (SNDS) di [!DNL Microsoft] consente di visualizzare le metriche relative alla quantità di posta inviata e accettata, nonché ai reclami e alle trappole spam. Tieni presente che i dati condivisi sono di esempio e non riflettono numeri esatti, ma è meglio rappresentare il modo in cui [!DNL Microsoft] ti vede come mittente. [!DNL Microsoft] non fornisce pubblicamente informazioni sul gruppo di utenti trusted, ma tali dati sono disponibili tramite il programma [!DNL Return Path Certification] a un costo aggiuntivo.

## Reputazione mittente

[!DNL Microsoft] si è tradizionalmente concentrato sull&#39;invio di IP nelle valutazioni della reputazione e nelle decisioni di filtro. Inoltre, stanno lavorando attivamente all’espansione delle funzionalità del dominio di invio. Entrambi sono in gran parte guidati dai tradizionali influencer della reputazione, come reclami e trappole spam. Il recapito messaggi può anche essere fortemente influenzato dal programma di certificazione del percorso di ritorno, che ha specifici requisiti di programma quantitativi e qualitativi.

## Approfondimenti

[!DNL Microsoft] combina tutti i domini di ricezione per stabilire e tenere traccia della reputazione di invio. Sono inclusi [!DNL Hotmail], [!DNL Outlook], MSN, [!DNL Windows Live] e così via, nonché tutte le e-mail aziendali ospitate in Office 365. [!DNL Microsoft] può essere particolarmente sensibile alle fluttuazioni del volume, pertanto è consigliabile applicare strategie specifiche per aumentare e diminuire gli invii di grandi dimensioni, anziché consentire modifiche improvvise basate sul volume.

[!DNL Microsoft] è inoltre particolarmente rigido durante i primi giorni di riscaldamento dell&#39;IP, il che in genere significa che la maggior parte dei messaggi viene filtrata inizialmente. La maggior parte degli ISP considera i mittenti innocenti fino a quando non ne viene provata la colpevolezza. [!DNL Microsoft] è il contrario e ti considera colpevole fino a quando non ti dimostri innocente.
