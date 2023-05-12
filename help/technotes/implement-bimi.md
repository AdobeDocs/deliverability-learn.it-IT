---
title: Implementare gli indicatori di marchio Gmail per l’identificazione dei messaggi (BIMI)
description: Scopri come implementare BIMI
topics: Deliverability
exl-id: 6b911bcc-a531-466a-8bd3-7fa469b96cc7
source-git-commit: 05f6cd331f4e610e2442d43405333823644d349e
workflow-type: tm+mt
source-wordcount: '1063'
ht-degree: 0%

---

# Implementare [!DNL Brand Indicators for Message Identification] (BIMI)

[!DNL Brand Indicators for Message Identification] (BIMI) è uno standard di settore che consente a un logo approvato di apparire accanto all’e-mail del mittente nelle piattaforme partecipanti.

Con questo standard, un marchio può determinare un logo che deve essere visualizzato nelle caselle in entrata dei provider di cassette postali. Una volta pubblicato in un record DNS (Domain Name System) BIMI, un provider di cassette postali potrebbe scegliere questo logo e visualizzarlo nella casella in entrata se vengono soddisfatti determinati criteri.

Diversi provider implementano in modo diverso, ma i vantaggi sono chiari: stare nella casella in entrata, creare fiducia e controllare ciò che viene mostrato.

BIMI non migliora direttamente la consegna o la tua reputazione. Può essere utile per creare maggiore fiducia con i destinatari e quindi incrementare il coinvolgimento.

## Com&#39;è?

Puoi trovare alcuni esempi di implementazioni da diversi provider e ulteriori dettagli sui quali i provider visualizzano il logo su [Pagina del gruppo BIMI](https://bimigroup.org/where-is-my-bimi-logo-displayed/).

## Chi è il gruppo BIMI?

Il gruppo BIMI è un gruppo di lavoro che sviluppa il sistema IMI in quanto copre non solo il logo ma anche i requisiti tecnici, legali e di conformità.

Il gruppo BIMI è composto da diversi soggetti interessati provenienti da diversi settori del settore: Google, Yahoo, Fastmail, Proofpoint, Mailchimp, Sendgrid, Valimail e Validità.

## Chi sostiene BIMI?

L&#39;elenco dei fornitori di cassette postali che supportano BIMI sta crescendo costantemente. È possibile trovare un elenco aggiornato [qui](https://bimigroup.org/bimi-infographic/) sia per i fornitori di supporto che per i fornitori che considerano BIMI.

A partire da aprile 2023, la lista include Gmail, Yahoo, La Poste, Fastmail, Onet.pl e Zone, Proofpoint come accessorio anti-spam e Apple Mail (da iOS 16 in poi).

I nomi più importanti in quella lista sono ovviamente Yahoo, Gmail e un recente adottante: Apple con iOS 16. Apple svolge un ruolo speciale nel mix in quanto non sono un provider di cassette postali, ma hanno aggiunto il supporto BIMI alla loro app di posta nativa. La corrispondenza conforme con BIMI verrà visualizzata come &quot;e-mail certificata digitalmente&quot; che aumenta la fiducia nel marchio.

## Implementazione

L’implementazione del sistema BIMI avviene in diversi passaggi:

1. Implementazione del DMARC (Domain based Message Authentication, Reporting and Conformance) a livello di applicazione sia per il dominio di invio che per il relativo dominio organizzativo - [Ulteriori informazioni](#dmarc)

1. Creazione del logo del marchio nel formato SVG TinyPS - [Ulteriori informazioni](#create-brand-logo)

1. Iscriviti a un certificato a marchio verificato (necessario solo per alcuni provider) - [Ulteriori informazioni](#vmc)

1. Pubblica un record DNS BIMI con il logo e il certificato - [Ulteriori informazioni](#publish-bimi-record)

1. Avere una buona reputazione - [Ulteriori informazioni](#good-reputation)

>[!NOTE]
>
>Tieni presente che tutti i passaggi devono essere interrotti.


### DMARC {#dmarc}

DMARC è uno standard che consente al marchio di decidere cosa deve fare un provider di cassette postali con un&#39;e-mail che non riesce [autenticazione](../additional-resources/authentication.md). I cosiddetti criteri variano da &quot;nessuno&quot; a &quot;quarantena&quot; (posizionamento cartella spam) a &quot;rifiuto&quot; (blocco totale della posta). Solo le ultime due politiche sono chiamate &quot;applicazione&quot; e possono essere qualificate per BIMI. La posta inviata da Adobe sta passando l&#39;autenticazione, in quanto SPF (Sender Policy Framework) e DKIM (Domain Keys Identified Mail) sono impostati per impostazione predefinita. L&#39;Adobe sta configurando DMARC sul dominio di invio su richiesta.

Oltre al DMARC sul dominio di invio, il DMARC deve essere utilizzato anche a livello di applicazione per il dominio organizzativo (se il dominio di invio è news.example.com, example.com è il dominio organizzativo).

### Creazione del logo del marchio {#create-brand-logo}

La creazione del logo deve rispettare i requisiti del 100%. Fai sempre riferimento al [Linee guida del gruppo BIMI](https://bimigroup.org/creating-bimi-svg-logo-files/).

Oltre ai requisiti tecnici, vi sono alcune raccomandazioni pratiche come avere un logo quadrato, avere un colore solido come sfondo e altri. Queste raccomandazioni sono per la visualizzazione ottimale.
Nota che la non conformità può comportare la mancata visualizzazione del logo.

### Certificato a marchio verificato (VMC) {#vmc}

Un certificato con contrassegno verificato (VMC) è necessario solo per alcuni provider di cassette postali come Gmail e Apple ed è pertanto facoltativo. Si consiglia di ottenere un VMC anche se per sfruttare davvero BIMI.

Un certificato a marchio verificato è una convalida legale che il marchio può utilizzare. Un&#39;autorità di certificazione verificherà questo problema attraverso l&#39;ufficio dei marchi in cui è registrato il logo del marchio. Questo processo comporta diverse convalide e verifiche legali e può richiedere del tempo. Attualmente due CA (autorità di certificazione) rilasciano VMC: Digicert e Entrust. I primi uffici di marchi sono Stati Uniti, Canada, UE, Regno Unito, Germania, Giappone, Australia e Spagna.

Come regola del pollice, avrete bisogno di un VMC per logo. Disporre di un VMC per il dominio organizzativo copre i sottodomini e con una funzione aggiunta anche domini diversi. Se hai diversi loghi, sono necessari più di un VMC. L&#39;Autorità di certificazione o il partner con cui scegli di lavorare ti aiuterà a configurarlo.

>[!NOTE]
>
>Si noti che i VMC hanno una tariffa annuale.

### Pubblicazione del record BIMI {#publish-bimi-record}

Una volta completati gli altri passaggi, è possibile pubblicare il record DNS BIMI. Il record si presenta così:

```
default._bimi.[domain] IN TXT "v=BIMI1; l=[SVG URL]; a=[PEM URL]
```

&quot;PEM URL&quot; è il percorso del file del certificato con contrassegno verificato.

Per il dominio di invio, questa operazione deve essere eseguita per Adobe.

### Buona reputazione {#good-reputation}

La fiducia è la chiave per BIMI. L&#39;utente si fida del proprio provider di cassette postali per mostrare solo il logo per i mittenti legittimi, quindi il provider di cassette postali deve fidarsi del marchio, e questo viene fatto dalla reputazione del mittente. Se hai un&#39;alta reputazione, tutto è buono, ma se non lo sei, il logo non sarà visualizzato. Sfortunatamente, non c&#39;è alcuna informazione o metrica che possiamo vedere per capire se la reputazione è abbastanza alta.

Perfino superare gli sforzi e le spese per un VMC non toglie questa parte. Se il provider della cassetta postale non considera attendibile il marchio, il logo non verrà visualizzato.

## Suggerimenti

* Il gruppo BIMI offre un utile strumento di convalida per BIMI. Per verificare se tutto è pronto o se il logo è conforme, vai a [questo link](https://bimigroup.org/bimi-generator/). Per quest&#39;ultimo clicca **[!UICONTROL Generate BIMI]** e immetti un dominio segnaposto ma l’URL corretto del logo. L&#39;ispettore le dirà se il logo è conforme.

* È possibile iniziare senza un VMC in tutta sicurezza, non c&#39;è alcun danno sulla tua reputazione se il tuo record BIMI non include un URL VMC, ma il logo può già essere mostrato in Yahoo.

* L’attuazione del DMARC a livello organizzativo è un’impresa di grandi dimensioni. Alcune aziende sono specializzate per aiutare i marchi a raggiungere un&#39;adozione completa DMARC.

* Viene pubblicato un elenco completo delle domande frequenti [qui](https://bimigroup.org/faqs-for-senders-esps/).
