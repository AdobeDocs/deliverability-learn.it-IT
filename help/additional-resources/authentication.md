---
title: Autenticazione
description: Ulteriori informazioni sui metodi di autenticazione SPF, DKIM e DMARC.
feature: Risorse aggiuntive
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 1e539b5df54250a5927701009e7a9c84e5d73fae
workflow-type: tm+mt
source-wordcount: '764'
ht-degree: 0%

---


# Autenticazione

## SPF {#spf}

SPF (Sender Policy Framework) è uno standard di autenticazione e-mail che consente al proprietario di un dominio di specificare quali server e-mail possono inviare e-mail per conto di tale dominio. Questo standard utilizza il dominio nell’intestazione &quot;Return-Path&quot; dell’e-mail (noto anche come indirizzo &quot;Envelope From&quot;).

>[!NOTE]
>
>È possibile utilizzare [questo strumento esterno](https://www.kitterman.com/spf/validate.html) per verificare un record SPF.

L’SPF è una tecnica che, in un certo senso, ti consente di assicurarti che il nome di dominio utilizzato in un’e-mail non sia forgiato. Quando un messaggio viene ricevuto da un dominio, viene eseguita una query sul server DNS del dominio. La risposta è un breve record (il record SPF) che specifica quali server sono autorizzati a inviare e-mail da questo dominio. Supponendo che solo il proprietario del dominio abbia i mezzi per modificare questo record, possiamo considerare che questa tecnica non consente di forgiare l&#39;indirizzo del mittente, almeno non la parte a destra del &quot;@&quot;.

Nella specifica finale [RFC 4408](https://www.rfc-editor.org/info/rfc4408), vengono utilizzati due elementi del messaggio per determinare il dominio considerato come mittente: il dominio specificato dal comando SMTP &quot;HELO&quot; (o &quot;EHLO&quot;) e il dominio specificato dall&#39;indirizzo dell&#39;intestazione &quot;Return-Path&quot; (o &quot;MAIL FROM&quot;), che è anche l&#39;indirizzo non recapitato. Considerazioni diverse permettono di tenere conto soltanto di uno di questi valori; è consigliabile assicurarsi che entrambe le origini specifichino lo stesso dominio.

Il controllo dell&#39;SPF fornisce una valutazione della validità del dominio del mittente:

* **Nessuno**: Impossibile eseguire alcuna valutazione.
* **Neutro**: Il dominio interrogato non abilita la valutazione.
* **Passa**: Il dominio è considerato autentico.
* **Non riuscito**: Il dominio viene forgiato e il messaggio deve essere rifiutato.
* **SoftFail**: Il dominio è probabilmente forgiato, ma il messaggio non deve essere rifiutato solo in base a questo risultato.
* **TempError**: Un errore temporaneo ha interrotto la valutazione. Il messaggio può essere rifiutato.
* **PermError**: I record SPF del dominio non sono validi.

Vale la pena notare che i record effettuati a livello dei server DNS possono richiedere fino a 48 ore per essere presi in considerazione. Questo ritardo dipende dalla frequenza con cui vengono aggiornate le cache DNS dei server riceventi.

## DKIM {#dkim}

L&#39;autenticazione DKIM (DomainKeys Identified Mail) è un successore di SPF. Utilizza la crittografia a chiave pubblica che consente al server e-mail ricevente di verificare che un messaggio sia stato effettivamente inviato dalla persona o dall’entità da cui sostiene di essere stato inviato e se il contenuto del messaggio è stato modificato tra il momento in cui è stato inviato originariamente (e DKIM &quot;signed&quot;) e il momento in cui è stato ricevuto. In genere, questo standard utilizza il dominio nell’intestazione &quot;Da&quot; o &quot;Mittente&quot;.

DKIM proviene da una combinazione di DomainKeys, Yahoo! e Cisco Identificato i principi di autenticazione della posta Internet e viene utilizzato per controllare l&#39;autenticità del dominio del mittente e garantire l&#39;integrità del messaggio.

L&#39;autenticazione DKIM ha sostituito **DomainKeys**.

L’utilizzo di DKIM richiede alcuni prerequisiti:

* **Sicurezza**: La crittografia è un elemento chiave del DKIM. Per garantire il livello di sicurezza del DKIM, la dimensione di crittografia 1024b è la best practice consigliata. Le chiavi DKIM inferiori non sono considerate valide dalla maggior parte dei provider di accesso.
* **Reputazione**: La reputazione si basa sull’IP e/o sul dominio, ma anche il selettore DKIM meno trasparente è un elemento chiave da prendere in considerazione. La scelta del selettore è importante: evitare di mantenere quello &quot;di default&quot; che potrebbe essere utilizzato da chiunque e quindi ha una reputazione debole. È necessario implementare un selettore diverso per le comunicazioni **Retention vs acquisition** e per l&#39;autenticazione.

Ulteriori informazioni sul prerequisito DKIM quando si utilizza Campaign Classic in [questa sezione](/help/additional-resources/acc-technical-recommendations.md#dkim-acc).

## DMARC {#dmarc}

DMARC (Domain-based Message Authentication, Reporting and Conformance) è la forma più recente di autenticazione tramite e-mail e si basa sia sull’autenticazione SPF che su quella DKIM per determinare se un’e-mail viene passata o meno. Il DMARC è unico e potente in due modi importanti:

* Conformità : consente al mittente di istruire gli ISP su cosa fare con qualsiasi messaggio che non riesce ad autenticarsi (ad esempio: non accettarlo).
* Reporting : fornisce al mittente un rapporto dettagliato che mostra tutti i messaggi che non hanno eseguito l’autenticazione DMARC, insieme al dominio &quot;Da&quot; e all’indirizzo IP utilizzati per ciascuno di essi. Questo consente a un&#39;azienda di identificare e-mail legittime che non stanno eseguendo l&#39;autenticazione e che necessitano di un certo tipo di &quot;fix&quot; (ad esempio, l&#39;aggiunta di indirizzi IP al proprio record SPF), nonché le fonti e la prevalenza di tentativi di phishing nei loro domini e-mail.

>[!NOTE]
>
>DMARC può sfruttare i rapporti generati da [250ok](https://250ok.com/).
