---
title: Reclami
description: 'Scopri i reclami registrati quando un utente indica che un’e-mail è indesiderata o inaspettata. '
feature: Metriche
topics: Deliverability
kt: 7048
thumbnail: kt7048.jpg
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: d42a8c3b06308fca0cf3e9db8d634a767fc0cdc6
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 2%

---


# Reclami

I reclami vengono registrati quando un utente indica che un’e-mail è indesiderata o inaspettata. Questa azione dell’utente iscritto viene generalmente registrata tramite il client e-mail dell’utente che si abbona quando preme il pulsante spam o tramite un sistema di segnalazione di spam di terze parti.

## reclamo dell&#39;ISP

La maggior parte degli ISP di livello 1 e alcuni di livello 2 forniscono un metodo di segnalazione di spam ai loro utenti in quanto i processi di rinuncia e annullamento dell’abbonamento sono stati utilizzati in modo dannoso in passato per convalidare un indirizzo e-mail. Adobe Campaign riceve questi reclami tramite ISP FBL. Questo viene stabilito durante il processo di configurazione per tutti gli ISP che forniscono FBL e consentono ad Adobe Campaign di aggiungere automaticamente gli indirizzi e-mail che si sono lamentati alla tabella di quarantena per la soppressione. I picchi nei reclami dell&#39;ISP possono essere un indicatore di scarsa qualità dell&#39;elenco, metodi di raccolta di elenchi meno che ottimali, o criteri di coinvolgimento deboli. Vengono anche spesso annotati quando il contenuto non è rilevante.

## Denunce di terzi

Ci sono diversi gruppi anti-spam che consentono la segnalazione di spam a un livello più ampio. Le metriche dei reclami utilizzate da queste terze parti vengono utilizzate per assegnare tag al contenuto delle e-mail per identificare le e-mail di spam. Questo processo è noto anche come impronte digitali. Gli utenti di questi metodi di reclamo di terze parti sono generalmente più astuti riguardo l&#39;e-mail, quindi possono avere un impatto maggiore di altri reclami può avere se lasciato senza risposta.

>[!NOTE]
>
>Gli ISP raccolgono reclami e li utilizzano per determinare la reputazione complessiva di un mittente. Tutte le denunce dovrebbero essere soppresse e non più contattate il più rapidamente possibile e in conformità delle leggi e dei regolamenti locali.

## Risorse specifiche per i prodotti

**Adobe Campaign Standard**

* [Reclami](https://experienceleague.adobe.com/docs/campaign-standard/using/reporting/list-of-reports/complaints.html#reporting)