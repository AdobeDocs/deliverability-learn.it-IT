---
title: Tutto sulle trappole anti-spam
description: Scopri come comprendere, identificare ed evitare le trappole con spam durante la gestione del recapito messaggi.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 45cdcda0-70e4-47f4-8713-a834500e7881
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '440'
ht-degree: 2%

---

# Tutto sulle trappole anti-spam

A [trappola per spam](/help/metrics/spam-traps.md) è un indirizzo valido, senza messaggio di errore quando le e-mail vengono inviate a . Una trappola per spam ha una missione principale: identificare spammer o mittenti senza processo di igiene dei dati.

## Chi gestisce questi indirizzi della trappola dello spam?

Il primo tipo di indirizzi di trappola per spam è l&#39;azienda di inserire nell&#39;elenco Bloccati IP e dominio come SpamHaus, Sorbs, SpamCop. Queste aziende hanno un&#39;enorme rete di indirizzi che vengono inviati su varie pagine internet come sito web, blog, forum in modo che i loro indirizzi siano raccolti da spammer.

Il secondo tipo di trappola per spam è basato su vecchi indirizzi ISP attivi. Questi ISP hanno la propria rete di trappola dello spam costruita su indirizzi inattivi riconvertiti in trappola e ogni hit che influisce sulla reputazione dell&#39;IP del mittente e del dominio.

## Come funziona?

**Un indirizzo e-mail senza utente finale**: Questi indirizzi non hanno e non avranno mai un utente finale che potrebbe registrarsi alle newsletter o a qualsiasi altro tipo di comunicazione.

**Un indirizzo e-mail abbandonato da un utente**: Dopo un periodo di inattività, gli indirizzi vengono disattivati dagli ISP. I messaggi di rimbalzo vengono inviati ai mittenti per informarli di questo nuovo stato. I mittenti devono mettere in quarantena questi indirizzi o rimuoverli dalle comunicazioni future. Gli ISP utilizzano questi indirizzi trasformati in &quot;trappola dello spam&quot; per monitorare i mittenti con le cattive pratiche.

## Come riconoscere o identificare una trappola di spam?

È un lavoro difficile identificare le trappole spam, questi indirizzi devono rimanere anonimi in quanto sono utilizzati per identificare i malintenzionati mittenti. La maggior parte degli ISP non dispone di un sistema aperto e clicca per monitorare gli hit di mittenti danneggiati. Secondo le definizioni precedenti, è possibile determinare un pod di indirizzi sospetti e verificare l&#39;efficienza della selezione del pod.

## Perché il tuo database è infettato da spam trap?

Il database degli indirizzi e-mail contiene la trappola dello spam, come potrebbe essere possibile? Le due cause principali sono una mancanza nel processo di igiene del database o raccogliere disfunzioni.

Questi pochi punti ti aiutano a controllare i tuoi processi:

* Disfunzione di raccolta:
   * Da dove vengono i tuoi indirizzi e-mail? Quante fonti vengono utilizzate per raccogliere questi indirizzi? Sei in grado di identificarli? Interno/Registrazione?
   * Il sistema di consenso funziona correttamente?
   * Hai controllato domini e alias dei tuoi indirizzi? Fatela con il tavolo qui sotto!
* Processo di igiene del database:
   * Qual è il processo relativo all’indirizzo inattivo negli ultimi 12 mesi?
   * Stai elaborando una quarantena sui messaggi non recapitati come &quot;utente inattivo&quot;?
   * Quando è stata l&#39;ultima volta che ti sei occupato del tuo database e hai provato a pulirlo? Lo faccia regolarmente.

## Alias e domini da evitare

**Alias**

![](../../help/assets/aliases.png)

**Domini**

![](../../help/assets/domains.png)
