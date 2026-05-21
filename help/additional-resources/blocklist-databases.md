---
title: Elenchi Blackhole in tempo reale
description: Scopri le organizzazioni che gestiscono elenchi di indirizzi IP e domini che potrebbero essere utilizzati da spammer.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 4155b89f-a636-404c-8951-563c1b4d0289
TQID: https://experienceleague.adobe.com/dsyoiAT3fYro3L8HkRT6OuXGfBqaVOJNy-IoFXQK37I
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87id: dfc56824-e8b9-499e-85d4-21aedb507314
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: f8a45b24-4be7-4f1b-909b-60d06b483a20
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 75df8537199680e5f1fc4b98cefdb05220fee7bf
workflow-type: tm+mt
source-wordcount: 423
ht-degree: 9%

---

# Elenchi Blackhole in tempo reale

Diverse organizzazioni gestiscono database di indirizzi IP e domini che si ritiene siano utilizzati da spammer. Consultare questi siti può essere utile per capire perché alcuni messaggi sono stati rifiutati come spam. È generalmente possibile chiedere la rimozione di un indirizzo erroneamente aggiunto a tali elenchi.

Questi database sono chiamati RBL (Real-time Blackhole Lists) e vengono consultati tramite un meccanismo DNS. Esistono tre tipi di RBL:

* Per indirizzo IP: elenca gli indirizzi IP che inviano spam o che probabilmente inoltrano spam.
* Per dominio mittente: elenca i domini del mittente (dominio completo dell’indirizzo e-mail non recapitate) che inviano spam o che sono configurati in modo errato.
* Per dominio web: elenca i domini (domini di alto livello registrati con i registrar) presenti negli URL dei collegamenti e delle immagini contenuti nel contenuto spam. Nelle soluzioni Adobe, il dominio da considerare è generalmente l’indirizzo utilizzato per il tracciamento.

Di seguito è riportato un elenco degli URL più utilizzati. Per un elenco più completo, puoi fare riferimento a [https://www.dnsstuff.com/](https://tools.dnsstuff.com/).

* **Spahaus**

  Fai riferimento a [https://www.spamhaus.org/](https://www.spamhaus.org/)

  Il database è più importante. Essere classificati in questo elenco è generalmente una situazione grave. In questo caso, devi agire IMMEDIATAMENTE e avvisare i servizi commerciali, il recapito messaggi e l&#39;[Assistenza clienti Adobe](https://helpx.adobe.com/it/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html).

* **SpamCop**

  Fai riferimento a [https://www.spamcop.net/](https://www.spamcop.net/)

  Si tratta di uno dei database più rinomati. Se uno dei tuoi indirizzi IP viene inserito in questo elenco, ciò significa in genere che gli utenti SpamCop hanno dichiarato che i tuoi messaggi sono spam o che hai inviato messaggi a un honeypot SpamCop.

* **URIBL**

  Fai riferimento a [https://www.uribl.com/](https://www.uribl.com/)

  Questo elenco identifica i domini che compaiono regolarmente nei messaggi dichiarati come spam. Se il dominio viene visualizzato in questo elenco, può influire in modo significativo sul recapito messaggi. Devi informare immediatamente i servizi di recapito messaggi e [l&#39;Assistenza clienti Adobe](https://helpx.adobe.com/it/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html).

* **SURBL**

  Fai riferimento a [https://surbl.org/](https://surbl.org/)

  SURBL identifica i siti web che compaiono regolarmente nello spam. Se il dominio viene visualizzato in questo elenco, può influire in modo significativo sul recapito messaggi. Devi informare immediatamente i servizi di recapito messaggi e [l&#39;Assistenza clienti Adobe](https://helpx.adobe.com/it/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html).

* **iX Manitu**

  Questo è un elenco di IP ed è ampiamente utilizzato in Germania. Fai riferimento a [https://www.heise.de/ix/nixspam/](https://www.heise.de/ix/nixspam/)

<!--
* SORBS

  [https://www.nl.sorbs.net](https://www.nl.sorbs.net) compiles a list of IP addresses that are reputed to be dynamic IP address (i.e. attributed temporarily to ISP subscribers) or "open relay" addresses. Certain domains check whether the IP address of a sender is not listed on this site before accepting email. Checking the IP addresses on this site can prove useful.
-->
