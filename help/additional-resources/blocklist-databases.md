---
title: Elenchi Blackhole in tempo reale
description: Scopri le organizzazioni che gestiscono elenchi di indirizzi IP e domini che probabilmente verranno utilizzati dagli spammer.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 4155b89f-a636-404c-8951-563c1b4d0289
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 6%

---

# Elenchi Blackhole in tempo reale

Diverse organizzazioni mantengono database di indirizzi IP e domini ritenuti utilizzati dagli spammer. Consultare questi siti può essere utile per capire perché alcuni messaggi sono stati rifiutati come spam. È generalmente possibile richiedere la rimozione di un indirizzo erroneamente aggiunto a questi elenchi.

Questi database sono chiamati RBL (Real-time Blackhole List) e sono consultati tramite un meccanismo DNS. Esistono tre tipi di RBL:

* Per indirizzo IP: elenca gli indirizzi IP che inviano spam o che potrebbero inviare spam.
* Per dominio del mittente: elenca i domini del mittente (dominio completo dell’indirizzo e-mail non recapitato) che inviano spam o configurati in modo errato.
* Per dominio Web: elenca i domini (domini di alto livello registrati con i registrar) che si trovano negli URL dei collegamenti e delle immagini contenuti nel contenuto spam. Nelle soluzioni di Adobe, il dominio da considerare è in genere l’indirizzo utilizzato per il tracciamento.

Di seguito è riportato un elenco degli URL più utilizzati. Per un elenco più completo, puoi fare riferimento a [https://www.dnsstuff.com/](https://tools.dnsstuff.com/).

* **Spamhaus**

   Fare riferimento a [https://www.spamhaus.org/](https://www.spamhaus.org/)

   Il database è più importante. Essere classificato in questo elenco è generalmente una situazione grave. In questo caso, devi agire immediatamente e avvisare i servizi commerciali, il recapito messaggi e [Adobe Customer Care](https://helpx.adobe.com/it/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html).

* **SpamCop**

   Fare riferimento a [https://www.spamcop.net/](https://www.spamcop.net/)

   È uno dei database più rinomati. Se uno dei tuoi indirizzi IP è inserito in questo elenco, ciò significa in genere che gli utenti di SpamCop hanno dichiarato i tuoi messaggi come Spam o che hai inviato messaggi a un nido d’ape SpamCop.

* **URIBL**

   Fare riferimento a [https://www.uribl.com/](https://www.uribl.com/)

   Questo elenco identifica i domini che compaiono regolarmente nei messaggi dichiarati come spam. Se il dominio viene visualizzato in questo elenco, può influenzare in modo significativo il recapito messaggi. Devi informare immediatamente i servizi di recapito messaggi e l’ [Assistenza clienti Adobe](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) .

* **SURBO**

   Fare riferimento a [http://www.surbl.org/](http://www.surbl.org/)

   SURBL identifica i siti web che compaiono regolarmente nello spam. Se il dominio viene visualizzato in questo elenco, può influenzare in modo significativo il recapito messaggi. Devi informare immediatamente i servizi di recapito messaggi e l’ [Assistenza clienti Adobe](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) .

* **Manitu iX**

   Questo è un elenco di IP ed è ampiamente utilizzato in Germania. Fare riferimento a [https://www.heise.de/ix/nixspam/](https://www.heise.de/ix/nixspam/)

<!--* SORBS

  [https://www.nl.sorbs.net](https://www.nl.sorbs.net) compiles a list of IP addresses that are reputed to be dynamic IP address (i.e. attributed temporarily to ISP subscribers) or "open relay" addresses. Certain domains check whether the IP address of a sender is not listed on this site before accepting email. Checking the IP addresses on this site can prove useful.-->
