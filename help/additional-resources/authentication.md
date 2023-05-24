---
title: Autenticazione
description: Ulteriori informazioni sui metodi di autenticazione SPF, DKIM e DMARC.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 03609139-b39b-4051-bcde-9ac7c5358b87
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# Autenticazione

## SPF {#spf}

SPF (Sender Policy Framework) è uno standard di autenticazione e-mail che consente al proprietario di un dominio di specificare quali server e-mail possono inviare e-mail per conto di quel dominio. Questo standard utilizza il dominio nell’intestazione &quot;Return-Path&quot; dell’e-mail (noto anche come indirizzo &quot;Envelope From&quot;).

>[!NOTE]
>
>È possibile utilizzare [questo strumento esterno](https://www.kitterman.com/spf/validate.html) per verificare un record SPF.

L’SPF è una tecnica che, in una certa misura, ti consente di verificare che il nome di dominio utilizzato in un’e-mail non sia contraffatto. Quando un messaggio viene ricevuto da un dominio, viene eseguita una query sul server DNS del dominio. La risposta è un breve record (il record SPF) che indica quali server sono autorizzati a inviare e-mail da questo dominio. Supponendo che solo il proprietario del dominio abbia i mezzi per modificare questo record, possiamo considerare che questa tecnica non consente di falsificare l’indirizzo del mittente, almeno non la parte dalla destra di &quot;@&quot;.

Nella versione finale [Specifiche RFC 4408](https://www.rfc-editor.org/info/rfc4408), due elementi del messaggio vengono utilizzati per determinare il dominio considerato come mittente: il dominio specificato dal comando SMTP &quot;HELO&quot; (o &quot;EHLO&quot;) e il dominio specificato dall’indirizzo dell’intestazione &quot;Return-Path&quot; (o &quot;MAIL FROM&quot;), che è anche l’indirizzo non recapitato. Considerazioni diverse consentono di prendere in considerazione solo uno di questi valori; si consiglia di assicurarsi che entrambe le origini specifichino lo stesso dominio.

La verifica dell&#39;SPF fornisce una valutazione della validità del dominio del mittente:

* **Nessuno**: impossibile eseguire la valutazione.
* **Neutro**: il dominio su cui viene eseguita la query non abilita la valutazione.
* **Superato**: il dominio è considerato autentico.
* **Non riuscito**: il dominio è stato contraffatto e il messaggio deve essere rifiutato.
* **SoftFail**: il dominio è probabilmente contraffatto, ma il messaggio non deve essere rifiutato unicamente in base a questo risultato.
* **TempError**: un errore temporaneo ha interrotto la valutazione. Il messaggio può essere rifiutato.
* **PermError**: i record SPF del dominio non sono validi.

È opportuno notare che i record creati a livello dei server DNS possono richiedere fino a 48 ore per essere presi in considerazione. Questo ritardo dipende dalla frequenza con cui vengono aggiornate le cache DNS dei server riceventi.

## DKIM {#dkim}

L&#39;autenticazione DKIM (DomainKeys Identified Mail) è un successore di SPF. Utilizza la crittografia a chiave pubblica che consente al server e-mail ricevente di verificare che un messaggio sia stato effettivamente inviato dalla persona o dall’entità da cui afferma di essere stato inviato e se il contenuto del messaggio sia stato modificato tra il momento in cui è stato inviato originariamente (e DKIM &quot;firmato&quot;) e il momento in cui è stato ricevuto. Questo standard utilizza in genere il dominio nell’intestazione &quot;Da&quot; o &quot;Mittente&quot;.

DKIM proviene da una combinazione di DomainKeys, Yahoo! e Cisco hanno identificato i principi di autenticazione di Internet Mail e vengono utilizzati per verificare l’autenticità del dominio del mittente e garantire l’integrità del messaggio.

DKIM sostituito **DomainKeys** autenticazione.

L&#39;utilizzo di DKIM richiede alcuni prerequisiti:

* **Sicurezza**: la crittografia è un elemento chiave di DKIM. Per garantire il livello di sicurezza del DKIM, 1024b è la dimensione di crittografia consigliata come best practice. Le chiavi DKIM inferiori non sono considerate valide dalla maggior parte dei provider di accesso.
* **Reputazione**: la reputazione si basa sull’IP e/o sul dominio, ma anche il selettore DKIM meno trasparente è un elemento chiave da considerare. La scelta del selettore è importante: evita di mantenere quello &quot;predefinito&quot; che potrebbe essere utilizzato da chiunque e che quindi ha una reputazione debole. È necessario implementare un selettore diverso per **conservazione e comunicazioni di acquisizione** e per l’autenticazione.

Ulteriori informazioni sui prerequisiti DKIM quando si utilizza Campaign Classic in [questa sezione](/help/additional-resources/acc-technical-recommendations.md#dkim-acc).

## DMARC {#dmarc}

Il DMARC (Domain-based Message Authentication, Reporting and Conformance) è la forma più recente di autenticazione e-mail e si basa sull&#39;autenticazione SPF e DKIM per determinare se un messaggio e-mail viene inviato o meno. DMARC è unico e potente in due modi:

* Conformità: consente al mittente di indicare agli ISP come comportarsi con eventuali messaggi che non vengono autenticati (ad esempio: non accettarli).
* Generazione rapporti: fornisce al mittente un rapporto dettagliato che mostra tutti i messaggi che non hanno superato l’autenticazione DMARC, insieme al dominio &quot;Da&quot; e all’indirizzo IP utilizzati per ciascuno di essi. Questo consente a un’azienda di identificare le e-mail legittime che non riescono a eseguire l’autenticazione e richiedono un qualche tipo di &quot;correzione&quot; (ad esempio, l’aggiunta di indirizzi IP al record SPF), nonché le origini e la prevalenza dei tentativi di phishing sui propri domini e-mail.

>[!NOTE]
>
>DMARC può sfruttare i rapporti generati da [250 ok](https://250ok.com/).
