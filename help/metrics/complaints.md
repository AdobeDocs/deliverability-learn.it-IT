---
title: Reclami
description: Scopri di più sui reclami registrati quando un utente indica che un’e-mail è indesiderata o inaspettata.
topics: Deliverability
jira: KT-7048
thumbnail: kt7048.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 0343820d-f5af-4b8a-bcab-dbb47ae7aecb
source-git-commit: 9444f8601f2f349398ee5deb9d5f4d4f7abb44f5
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 100%

---

# Reclami

I reclami vengono registrati quando un utente indica che un’e-mail è indesiderata o inaspettata. Questa azione da parte degli abbonati viene generalmente registrata tramite il client e-mail dell’utente quando questo preme il pulsante di posta indesiderata o tramite un sistema di segnalazione di posta indesiderata di terze parti.

## Reclami di ISP (fornitori di servizi Internet)

La maggior parte degli ISP di livello 1 e alcuni di livello 2 forniscono un metodo di segnalazione di posta indesiderata ai loro utenti poiché, in passato, i processi di rinuncia e annullamento dell’abbonamento sono stati utilizzati in modo dannoso per convalidare un indirizzo e-mail. Adobe Campaign riceve questi reclami tramite i FBL (cicli di feedback) dell’ISP. Questo viene stabilito durante il processo di configurazione di tutti gli ISP che forniscono FBL e consentono ad Adobe Campaign di aggiungere automaticamente gli indirizzi e-mail che hanno sporto un reclamo e hanno richiesto l’eliminazione dalla tabella di quarantena. I picchi nei reclami degli ISP possono indicare scarsa una qualità dell’elenco, metodi di raccolta di elenchi non ottimali o criteri di coinvolgimento inefficaci. Si verificano spesso anche quando il contenuto non è rilevante.

## Reclami di terze parti

Ci sono diversi gruppi anti-spam che consentono la segnalazione di posta indesiderata a un livello più ampio. Le metriche dei reclami utilizzate da queste terze parti vengono utilizzate per assegnare tag al contenuto delle e-mail e identificare la posta indesiderata. Questo processo è noto anche come fingerprinting. Gli utenti di questi metodi di reclamo di terze parti sono generalmente più esperti riguardo le e-mail e, di conseguenza, se i loro reclami vengono lasciati senza risposa, possono avere un impatto maggiore rispetto ad altri reclami.

>[!NOTE]
>
>Gli ISP raccolgono reclami e li utilizzano per determinare la reputazione complessiva di un mittente. Tutti i reclami dovrebbero essere eliminati e non più contattati il più rapidamente possibile e in conformità alle leggi e ai regolamenti locali.

## Risorse specifiche per i prodotti

**Adobe Campaign Classic**

* [Indicatori di tracciamento](https://experienceleague.adobe.com/docs/campaign-classic/using/reporting/reports-on-deliveries/delivery-reports.html?lang=it#tracking-indicators)

**Adobe Campaign Standard**

* [Rapporto reclami](https://experienceleague.adobe.com/docs/campaign-standard/using/reporting/list-of-reports/complaints.html?lang=it#reporting)
