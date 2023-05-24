---
title: Tutto sulle trappole anti-spam
description: Scopri come comprendere, identificare ed evitare le trappole anti-spam durante la gestione del recapito messaggi.
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

A [trappola spam](/help/metrics/spam-traps.md) è un indirizzo valido, senza messaggio di errore quando le e-mail vengono inviate a. Una trappola spam ha una missione principale: identificare spammer o mittenti senza processo di igiene dei dati.

## Chi gestisce questi indirizzi di trappole spam?

Il primo tipo di indirizzi di trappole spam è l’azienda di inserisce nell&#39;elenco Bloccati IP e Domain come SpamHaus, Sorbs, SpamCop. Queste aziende hanno una vasta rete di indirizzi che vengono inviati su varie pagine Internet come siti web, blog, forum in modo che i loro indirizzi sono raccolti da spammer.

Il secondo tipo di trappola spam si basa sui vecchi indirizzi ISP attivi. Questi ISP dispongono di una propria rete di trappole spam basata su indirizzi inattivi riconvertiti in trappola e ogni hit che influisce sull’IP del mittente e sulla reputazione del dominio.

## Come funziona?

**Un indirizzo e-mail senza utente finale**: questi indirizzi non hanno e non avranno mai un utente finale che potrebbe registrarsi alle newsletter o a qualsiasi altro tipo di comunicazione.

**Un indirizzo e-mail abbandonato da un utente**: dopo un periodo di inattività, gli indirizzi vengono disattivati dagli ISP. I messaggi non recapitati vengono inviati ai mittenti per informarli di questo nuovo stato. I mittenti devono mettere in quarantena questi indirizzi o rimuoverli da comunicazioni future. Gli ISP utilizzano questi indirizzi trasformati in &quot;trappola spam&quot; per monitorare i mittenti con pratiche scorrette.

## Come riconoscere o identificare una trappola spam?

È un lavoro difficile identificare le trappole spam, questi indirizzi devono rimanere anonimi in quanto vengono utilizzati per identificare i mittenti malintenzionati. La maggior parte degli ISP non dispone di un sistema aperto e di clic per monitorare gli hit dei mittenti non validi. In base alle precedenti definizioni, è possibile determinare un pod di indirizzi sospetti e testare l’efficienza della selezione del pod.

## Perché il database è infetto da trappole spam?

Il database degli indirizzi e-mail contiene una trappola spam, come potrebbe essere possibile? I due principali motivi sono la mancanza di un processo di igiene del database o la raccolta di disfunzioni.

Questi pochi punti consentono di controllare i processi:

* Disfunzione di raccolta:
   * Da dove provengono gli indirizzi e-mail? Quante origini vengono utilizzate per raccogliere questi indirizzi? È in grado di identificarli? Interno/registrazione?
   * Il sistema di consenso funziona correttamente?
   * Hai controllato i domini e l’alias dei tuoi indirizzi? Fallo con il tavolo qui sotto!
* Processo di igiene del database:
   * Qual è il processo relativo agli indirizzi inattivi negli ultimi 12 mesi?
   * Stai elaborando una quarantena sui mancati recapiti non permanenti come &quot;utente inattivo&quot;?
   * Quando è stata l&#39;ultima volta che si è occupato del database e si è tentato di ripulirlo? Fallo regolarmente.

## Alias e domini da evitare

**Alias**

![](../../help/assets/aliases.png)

**Domini**

![](../../help/assets/domains.png)
