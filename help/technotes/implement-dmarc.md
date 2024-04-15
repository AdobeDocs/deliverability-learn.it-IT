---
title: Implementare gli indicatori del marchio Gmail per l’identificazione dei messaggi (BIMI)
description: Scopri come implementare BIMI
topics: Deliverability
role: Admin
level: Beginner
exl-id: f1c14b10-6191-4202-9825-23f948714f1e
source-git-commit: 2a78db97a46150237629eef32086919cacf4998c
workflow-type: tm+mt
source-wordcount: '1284'
ht-degree: 5%

---

# Implementare [!DNL Domain-based Message Authentication, Reporting and Conformance] (DMARC)

Lo scopo di questo documento è quello di fornire al lettore ulteriori informazioni sul metodo di autenticazione e-mail, DMARC. Spiegando come funziona DMARC e le sue varie opzioni strategiche, i lettori comprenderanno meglio l’impatto di DMARC sul recapito e-mail.

## Che cos&#39;è DMARC? {#about}

Domain-based Message Authentication, Reporting and Conformance, è un metodo di autenticazione e-mail che consente ai proprietari del dominio di proteggere il proprio dominio da utilizzi non autorizzati. DMARC fornisce inoltre un feedback sullo stato di autenticazione delle e-mail e consente ai mittenti di controllare cosa accade alle e-mail che non superano l’autenticazione. Ciò include opzioni per monitorare, mettere in quarantena o rifiutare la posta a seconda del criterio DMARC implementato.

DMARC dispone di tre opzioni:

* **Monitor (p=none):** Indica al provider di cassette postali/ISP di eseguire le operazioni normalmente eseguite sul messaggio.
* **Quarantena (p=quarantena):** Indica al provider della cassetta postale o all&#39;ISP di recapitare messaggi che non passano DMARC alla cartella di posta indesiderata o indesiderata del destinatario.
* **Rifiuta (p=rifiuta):** Indica al provider di cassette postali o all&#39;ISP di bloccare i messaggi che non passano DMARC causando un mancato recapito.

## Come funziona DMARC? {#how}

SPF e DKIM vengono entrambi utilizzati per associare un’e-mail a un dominio e collaborare per l’autenticazione dell’e-mail. DMARC compie un ulteriore passo avanti e aiuta a prevenire lo spoofing corrispondendo al dominio controllato da DKIM e SPF. Per passare il DMARC, un messaggio deve passare SPF o DKIM. Se entrambe queste operazioni non riescono, l’autenticazione DMARC avrà esito negativo e l’e-mail verrà recapitata in base ai criteri DMARC selezionati.

>[!NOTE]
>
>Il DMARC richiede l&#39;allineamento tra l&#39;indirizzo ‘From’ e ‘Return-Path’.

## Perché implementare DMARC? {#why}

Il DMARC è facoltativo e, sebbene non sia obbligatorio, è gratuito e consente ai destinatari delle e-mail di identificare facilmente l’autenticazione delle e-mail, il che potrebbe potenzialmente migliorare la consegna. Uno dei vantaggi principali di DMARC è la possibilità di creare rapporti sui messaggi che non vanno a buon fine in SPF e/o DKIM. Inoltre, offre ai mittenti un livello di controllo su ciò che accade con la posta che non passa nessuno di questi metodi di autenticazione. Tramite il reporting DMARC, i mittenti possono ottenere visibilità sui messaggi che non superano il DMARC, consentendo di adottare le misure necessarie per mitigare ulteriori errori.

>[!NOTE]
>
>Se desideri implementare BIMI, è necessario specificare p=quarantena o p=rifiuto del criterio DMARC.

## Best practice per l’implementazione di DMARC {#best-practice}

Poiché DMARC è facoltativo, non sarà configurato per impostazione predefinita su alcuna piattaforma ESP. È necessario creare un record DMARC nel DNS per il dominio affinché funzioni. È inoltre necessario un indirizzo e-mail a scelta per indicare la destinazione dei rapporti DMARC all&#39;interno dell&#39;organizzazione. Come best practice, si consiglia di implementare lentamente l’implementazione di DMARC aumentando il livello dei criteri DMARC da p=none a p=quarantena, fino a p=rifiuta man mano che acquisisci una comprensione DMARC del potenziale impatto di DMARC.

1. Analizza il feedback ricevuto e utilizza (p=none), che indica al destinatario di non eseguire azioni contro i messaggi che non superano l’autenticazione, ma inviano comunque i rapporti e-mail al mittente. Inoltre, se l’autenticazione dei messaggi legittimi non riesce, esamina e risolvi i problemi relativi a SPF/DKIM.
1. Determina se SPF e DKIM sono allineati e trasmettono l’autenticazione per tutte le e-mail legittime, quindi sposta il criterio in (p=quarantena), che indica al server e-mail ricevente di mettere in quarantena le e-mail che non riescono a eseguire l’autenticazione (in genere significa inserire tali messaggi nella cartella di posta indesiderata).
1. Imposta criterio su (p=rifiuta). Il criterio c= rifiuta indica al destinatario di rifiutare completamente (non recapitare) qualsiasi e-mail per il dominio che non supera l’autenticazione. Con questo criterio abilitato, solo i messaggi e-mail verificati come autenticati al 100% dal dominio avranno anche la possibilità di inserire la casella in entrata.

   >[!NOTE]
   >
   >Utilizza questo criterio con cautela e stabilisci se è appropriato per la tua organizzazione.

## Reporting DMARC {#reporting}

DMARC offre la possibilità di ricevere rapporti relativi alle e-mail che non superano SPF/DKIM. Esistono due diversi rapporti generati dai server ISP come parte del processo di autenticazione che i mittenti possono ricevere tramite i tag RUA/RUF nei propri criteri DMARC:

* **Rapporti aggregati (RUA):** Non contiene dati PII (personalmente identificabili) che potrebbero essere sensibili ai requisiti RGPD.
* **Rapporti forensi (RUF):** Contiene indirizzi e-mail sensibili al RGPD. Prima di utilizzare, è consigliabile verificare internamente come gestire le informazioni che devono essere conformi ai requisiti RGPD.

L’utilizzo principale di questi rapporti consiste nel ricevere una panoramica delle e-mail che vengono tentate di spoofing. Si tratta di rapporti altamente tecnici che è meglio digerire tramite uno strumento di terze parti. Alcune aziende specializzate nel monitoraggio DMARC sono:

* [ValiMail](https://www.valimail.com/products/#automated-delivery)
* [Agari](https://www.agari.com/)
* [Dmarciano](https://dmarcian.com/)
* [Proofpoint](https://www.proofpoint.com/us)

>[!CAUTION]
>
>Se gli indirizzi e-mail che si stanno aggiungendo per ricevere i rapporti si trovano all&#39;esterno del dominio per il quale viene creato il record DMARC, è necessario autorizzare il dominio esterno a specificare il DNS di cui si è proprietari. A questo scopo, segui i passaggi descritti in [Documentazione di dmarc.org](https://dmarc.org/2015/08/receiving-dmarc-reports-outside-your-domain)

### Esempio di record DMARC {#example}

```
v=DMARC1; p=reject; fo=1; rua=mailto:dmarc_rua@emaildefense.proofpoint.com;ruf=mailto:dmarc_ruf@emaildefense.proofpoint.co
```

## Tag DMARC e operazioni {#tags}

I record DMARC hanno più componenti denominati tag DMARC. Ogni tag ha un valore che specifica un determinato aspetto di DMARC.

| Nome tag | Obbligatorio/facoltativo | Funzione | Esempio | Valore predefinito |
|  ---  |  ---  |  ---  |  ---  |  ---  |
| v | Obbligatorio | Questo tag DMARC specifica la versione. Al momento è disponibile una sola versione, quindi il valore fisso sarà v=DMARC1 | V=DMARC1 DMARC1 | DMARC1 |
| p | Obbligatorio | Mostra il criterio DMARC selezionato e indica al destinatario di segnalare, mettere in quarantena o rifiutare i messaggi che non superano i controlli di autenticazione. | p=none, quarantena o rifiuto | - |
| fo | Facoltativo | Consente al proprietario del dominio di specificare le opzioni di reporting. | 0: genera il rapporto se tutto non riesce<br/>1: generare un rapporto in caso di errori<br/>d: Genera report in caso di errore DKIM<br/>s: Genera report se SPF non riesce | 1 (consigliato per i rapporti DMARC) |
| pct | Facoltativo | Indica la percentuale di messaggi soggetti a filtro. | pct=20 | 100 |
| rua | Facoltativo (consigliato) | Identifica dove verranno consegnati i rapporti aggregati. | `rua=mailto:aggrep@example.com` | - |
| ruf | Facoltativo (consigliato) | Identifica dove verranno consegnati i rapporti forensi. | `ruf=mailto:authfail@example.com` | - |
| sp | Facoltativo | Specifica il criterio DMARC per i sottodomini del dominio padre. | sp=rifiuta | - |
| adkim | Facoltativo | Può essere Strict (s) o Relaxed (r). L’allineamento rilassato indica che il dominio utilizzato nella firma DKIM può essere un sottodominio dell’indirizzo &quot;Da&quot;. L&#39;allineamento rigido indica che il dominio utilizzato nella firma DKIM deve corrispondere esattamente al dominio utilizzato nell&#39;indirizzo del mittente. | adkim=r | r |
| aspf | Facoltativo | Può essere Strict (s) o Relaxed (r). L&#39;allineamento semplificato indica che il dominio ReturnPath può essere un sottodominio dell&#39;indirizzo From. L&#39;allineamento rigido indica che il dominio del percorso di ritorno deve corrispondere esattamente all&#39;indirizzo Da. | aspf=r | r |

## DMARC e ADOBE CAMPAIGN {#campaign}

>[!NOTE]
>
>Se l’istanza Campaign è ospitata su AWS, puoi implementare DMARC per i sottodomini con il Pannello di controllo Campaign. [Scopri come implementare i record DMARC utilizzando Pannello di controllo Campaign](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/txt-records/dmarc.html).

Un motivo comune per gli errori DMARC è il disallineamento tra l&#39;indirizzo &quot;Da&quot; e &quot;Errori-A&quot; o &quot;Percorso di ritorno&quot;. Per evitare questo problema, durante la configurazione di DMARC, si consiglia di controllare due volte le impostazioni dell’indirizzo &quot;Da&quot; ed &quot;Errori-A&quot; nei modelli di consegna.

1. All’interno del Modello di consegna, controlla quale indirizzo è attualmente impostato come indirizzo &quot;Da&quot;.

   ![](../assets/dmarc1.png)

1. Da qui, seleziona &quot;Proprietà&quot; che ti consentirà di modificare ulteriormente il modello di consegna. In questa finestra, selezionare SMTP e deselezionare &quot;Usa l&#39;indirizzo di errore predefinito definito per la piattaforma&quot; se selezionato. Modelli di consegna in Adobe Campaign seleziona questa casella di controllo per impostazione predefinita. L’indirizzo di errore predefinito potrebbe non essere l’indirizzo associato all’indirizzo mittente in questo modello di consegna.

   ![](../assets/dmarc2.png)

1. Se questa casella non è selezionata, viene visualizzato un campo di testo che consente di immettere un indirizzo di errore univoco che utilizza lo stesso dominio impostato in Indirizzo mittente.

   ![](../assets/dmarc3.png)

Una volta salvate queste modifiche, potrai procedere con l’implementazione DMARC con il corretto allineamento del dominio.

## Collegamenti utili {#links}

* [DMARC.org](https://dmarc.org/){target="_blank"}
* [Autenticazione e-mail M3AAWG](https://www.m3aawg.org/sites/default/files/document/M3AAWG_Email_Authentication_Update-2015.pdf){target="_blank"}
