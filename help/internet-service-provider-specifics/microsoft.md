---
title: Microsoft (Hotmail, Outlook, Windows Live, ecc.)
description: Microsoft è generalmente il secondo o il terzo fornitore più grande a seconda della composizione dell’elenco e gestisce il traffico in modo leggermente diverso rispetto agli altri ISP.
topics: Deliverability
kt: 5319
doc-type: article
activity: understand
team: TM
exl-id: d706cb90-828a-4ab3-8f93-c9bd71553d63
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 2%

---

# [!DNL Microsoft] ([!DNL Hotmail], [!DNL Outlook], [!DNL Windows Live], ecc.)

[!DNL Microsoft] è generalmente il secondo o il terzo fornitore più grande a seconda della composizione dell’elenco e gestisce il traffico in modo leggermente diverso rispetto agli altri ISP.

Ecco alcuni punti salienti:

## Quali dati sono importanti

[!DNL Microsoft] si concentra sulla reputazione del mittente, sui reclami, sul coinvolgimento degli utenti e sul loro gruppo di utenti affidabili (noti anche come Sender Reputation Data o SRD) su cui effettuano il polling per ottenere un feedback.

## Quali dati rendono disponibili

[!DNL Microsoft]strumento proprietario di comunicazione al mittente, [!DNL Smart Network Data Services] (SNDS), consente di visualizzare le metriche relative alla quantità di posta che si sta inviando e alla quantità di posta accettata, nonché ai reclami e alle trappole spam. Tieni presente che i dati condivisi sono di esempio e non riflettono numeri esatti, ma è meglio rappresentare come [!DNL Microsoft] ti vede come mittente. [!DNL Microsoft] non fornisce pubblicamente informazioni sul gruppo di utenti trusted, ma tali dati sono disponibili tramite [!DNL Return Path Certification] per un costo aggiuntivo.

## Reputazione mittente

[!DNL Microsoft] tradizionalmente si è concentrato sull’invio di IP nelle valutazioni della reputazione e nelle decisioni di filtro. Inoltre, stanno lavorando attivamente per espandere le funzionalità del dominio di invio. Entrambi sono in gran parte guidati dai tradizionali influencer della reputazione, come reclami e trappole spam. Il recapito messaggi può anche essere fortemente influenzato dal programma di certificazione del percorso di ritorno, che ha specifici requisiti di programma quantitativi e qualitativi.

## Insights

[!DNL Microsoft] combina tutti i domini di ricezione per stabilire e tenere traccia della reputazione di invio. Ciò include [!DNL Hotmail], [!DNL Outlook], MSN, [!DNL Windows Live]e così via, nonché le e-mail aziendali ospitate da Office 365. [!DNL Microsoft] può essere particolarmente sensibile alle fluttuazioni del volume, pertanto puoi applicare strategie specifiche per incrementare o diminuire gli invii di grandi dimensioni, anziché consentire cambiamenti improvvisi basati sul volume.

[!DNL Microsoft] è anche particolarmente rigido durante i giorni iniziali di riscaldamento dell’IP, il che in genere significa che la maggior parte delle e-mail viene filtrata inizialmente. La maggior parte degli ISP considera i mittenti innocenti fino a quando non ne viene provata la colpevolezza. [!DNL Microsoft] è il contrario e ti considera colpevole fino a quando non ti dimostri innocente.
