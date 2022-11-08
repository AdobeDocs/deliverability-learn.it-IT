---
title: Implementare gli indicatori di marchio Gmail per l’identificazione dei messaggi (BIMI)
description: Scopri come implementare BIMI
topics: Deliverability
exl-id: 6b911bcc-a531-466a-8bd3-7fa469b96cc7
source-git-commit: 683ffd3c87a4849aa9fa48fbf50db9ade97991af
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 0%

---

# Implementare Gmail [!DNL Brand Indicators for Message Identification] (BIMI)

Gmail ha recentemente annunciato che [lancio del sostegno generale del BIMI](https://cloud.google.com/blog/products/identity-security/bringing-bimi-to-gmail-in-google-workspace){target=&quot;_blank&quot;}. Ci sono un certo numero di elementi che si dovrà trattare prima di poter sfruttare questo anche se: Certificati Mark verificati, Logo registrati, Logo formattati correttamente, configurazione DMARC e infine pubblicazione di un record BIMI nel DNS. Riesamineremo tutti questi passaggi in questo articolo.

[!DNL Brand Indicators for Message Identification] (BIMI) è uno standard di settore che consente a un logo approvato di apparire accanto all’e-mail del mittente nelle piattaforme partecipanti. Non solo questo accattivante forse aumentando l&#39;impegno, aiuta anche a confermare l&#39;autenticità del mittente riducendo il rischio di phishing e altre tattiche spammy.

## Certificato a marchio verificato

Uno degli elementi chiave del programma BIMI di Gmail è il requisito che i mittenti dispongano di un certificato a marchio verificato (VMC) rilasciato da un’autorità di certificazione valida. Attualmente questi VMC sono disponibili solo da Entrust e DigiCert, ma è probabile che l’elenco dei provider cresca a seguito dell’annuncio di Gmail.

I VMC saranno simili ai certificati SSL in alcuni modi. Avrai bisogno di un VMC per ogni logo che desideri visualizzare, quindi se hai molti marchi dovresti pianificare per avere bisogno di più VMC. Ogni VMC può essere valido su più domini anche se si ottiene un VMC multi-SAN. Quindi, se desideri che un logo venga visualizzato in più domini di invio, è necessario un solo VMC.

## Marchio del logo

Prima di ottenere il VMC, c&#39;è un altro passaggio chiave che deve essere completato. Per ottenere un VMC il logo che si desidera visualizzare deve essere registrato con uno degli 8 uffici di marchi e brevetti globali approvati.

* Ufficio brevetti e marchi degli Stati Uniti (USPTO)
* Ufficio canadese per la proprietà intellettuale
* Ufficio dell&#39;Unione europea per la proprietà intellettuale
* Ufficio britannico per la proprietà intellettuale
* Deutsches Patent- und Markenamt
* Ufficio marchi del Giappone
* Ufficio spagnolo dei brevetti e dei marchi
* IP Australia

Se il logo che si desidera visualizzare non è registrato o non è registrato in una di queste 8 organizzazioni, sarà necessario lavorare con il team legale per risolvere il problema prima di richiedere il VMC.

## Formato immagine logo

Questo sarebbe anche un buon momento per assicurarsi che il vostro logo sarà conforme ai requisiti del logo BIMI per il formato.

Deve essere in formato SVG e rispettare il profilo SVG Portable/Secure (SVG-P/S). Per informazioni su come farlo, consulta [Gruppo di lavoro BIMI](https://bimigroup.org/svg-conversion-tools-released){target=&quot;_blank&quot;}.

## DMARC

Una volta formattato correttamente il Logo e il certificato a marchio verificato, dovrai anche accertarti che il DMARC sia completamente configurato su qualsiasi dominio di invio per il quale desideri che BIMI funzioni.

Ciò include assicurarsi che P= sia impostato su Quarantena o Rifiuta. Se il DMARC utilizza P=None, non si qualifica per BIMI. L&#39;impostazione P=None è altamente raccomandata per assicurarsi di sapere quale posta sta uscendo da un dominio e che nulla sarebbe accidentalmente bloccato se si cambia in &quot;Quarantena&quot; o &quot;Rifiuta&quot;, pensarlo come la fase di test e raccolta di informazioni. Sarà necessario andare oltre questo per l&#39;applicazione, ma prima che BIMI diventi disponibile per voi.

## Voce DNS

Con tutto il resto finalmente allineato e pronto per andare, è ora di aggiornare la voce DNS con il tuo BIMI.

Questa è una voce semplice che dovrebbe avere un aspetto simile al seguente:

```
default._bimi.[domain] IN TXT “v=BIMI1; l=[SVG URL] 
```

Puoi ottenere i dettagli su quella voce e anche utilizzare un BIMI checker gratuito al [Sito del gruppo di lavoro BIMI](https://bimigroup.org/implementation-guide){target=&quot;_blank&quot;}.


## Aree principali

Se sei un [!DNL Adobe Campaign], l’Adobe può aiutarti a creare l’aggiornamento DNS BIMI: contatta l’Assistenza clienti Adobe per richiederne una. L&#39;Adobe può anche essere utile per la risoluzione dei problemi se BIMI non funziona correttamente.

Se sei un client Marketo, consulta [questo post di blog](https://nation.marketo.com/t5/support-blogs/how-to-bimi/ba-p/296966){target=&quot;_blank&quot;} per informazioni sulla creazione del record BIMI.

Per informazioni sui marchi o i certificati con marchio verificato, rivolgiti al team legale e a un fornitore VMC autorizzato.

Ottenere la configurazione BIMI per Gmail potrebbe non essere un processo rapido, ma è uno che può avere vantaggi significativi sia dal punto di vista del marketing che della sicurezza.
