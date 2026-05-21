---
title: Verizon Media Group (Yahoo, AOL, Verizon, ecc.)
description: '[!DNL Verizon Media Group] è in genere uno dei primi tre domini per la maggior parte degli elenchi B2C. Si comportano in modo univoco, in quanto generalmente si limitano a limitare o a inviare posta in massa se sorgono problemi di reputazione.'
topics: Deliverability
jira: KT-5320
doc-type: article
activity: understand
role: Admin, Leader, User
level: Beginner
team: TM
exl-id: 43e6d3cb-23c3-4076-8026-a1a08e76bd1b
TQID: https://experienceleague.adobe.com/ycELLXdqC1E3EIxywp-I1HWnSyWayRHcwkNOzmzSBvY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
  - id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87
  - id: dfc56824-e8b9-499e-85d4-21aedb507314
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 75df8537199680e5f1fc4b98cefdb05220fee7bf
workflow-type: tm+mt
source-wordcount: 289
ht-degree: 2%

---

# [!DNL Verizon Media Group] (Yahoo, AOL, Verizon, ecc.)

[!DNL Verizon Media Group] è in genere uno dei primi tre domini per la maggior parte degli elenchi B2C. Si comportano in modo univoco, in quanto generalmente si limitano a limitare o a inviare posta in massa se sorgono problemi di reputazione.

Ecco alcuni punti salienti:

## Quali dati sono importanti

[!DNL Verizon Media Group] (VMG) ha creato e mantenuto i propri filtri spam proprietari, utilizzando una combinazione di contenuti e filtri URL e reclami di spam. Insieme a Gmail, sono uno dei primi ISP che filtrano le e-mail per dominio e per indirizzo IP.

## Quali dati rendono disponibili

VMG ha un FBL utilizzato per trasmettere le informazioni sui reclami ai mittenti. Inoltre, stanno analizzando la possibilità di aggiungere ulteriori dati in futuro.

## Reputazione mittente

La reputazione di un mittente è costituita da una combinazione di indirizzo IP, dominio e indirizzo di origine. La reputazione viene calcolata utilizzando i componenti tradizionali, tra cui reclami, trappole spam, indirizzi inattivi o non corretti e coinvolgimento. VMG utilizza la limitazione della velocità (nota anche come limitazione) insieme alla cartella in massa per difendersi dallo spam. Completa i sistemi di filtraggio interni con alcune liste nere di [!DNL Spamhaus], tra cui PBL, SBL e XBL per proteggere i propri utenti.

## Approfondimenti

VMG dispone di periodi di manutenzione regolari per gli indirizzi e-mail vecchi, inattivi di recente. Ciò significa che è comune osservare un aumento significativo di mancati recapiti non validi, che può influire sulla frequenza di consegna per un breve periodo di tempo. Sono inoltre sensibili a tassi elevati di mancati recapiti di indirizzi non validi da parte di un mittente, il che indica la necessità di restringere i criteri di acquisizione o coinvolgimento. I mittenti possono spesso riscontrare un impatto negativo a circa l’1% degli indirizzi non validi.
