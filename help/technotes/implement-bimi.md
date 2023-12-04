---
title: Implementare gli indicatori del marchio Gmail per l’identificazione dei messaggi (BIMI)
description: Scopri come implementare BIMI
topics: Deliverability
role: Admin
level: Beginner
jira: KT-14079
exl-id: 6b911bcc-a531-466a-8bd3-7fa469b96cc7
source-git-commit: b96539608acd51ce76ef5bdaf5afd07b5a4208b7
workflow-type: tm+mt
source-wordcount: '1125'
ht-degree: 0%

---

# Implementare [!DNL Brand Indicators for Message Identification] (BIMI)

[!DNL Brand Indicators for Message Identification] (BIMI) è uno standard di settore che consente la visualizzazione di un logo approvato accanto all’e-mail di un mittente nelle piattaforme partecipanti.

Con questo standard, un marchio può determinare un logo da visualizzare nelle caselle in entrata dei provider di caselle postali. Una volta pubblicato in un cosiddetto record DNS (Domain Name System) BIMI, un provider di caselle postali potrebbe raccogliere questo logo e visualizzarlo nella casella in entrata se vengono soddisfatti determinati criteri.

Diversi provider eseguono implementazioni diverse, ma i vantaggi sono chiari: spiccare nella casella in entrata, creare fiducia e avere il controllo di ciò che viene mostrato.

BIMI non migliora direttamente il recapito messaggi o la tua reputazione. Può aiutare a creare maggiore fiducia nei destinatari e, quindi, stimolare più coinvolgimento.

## Che aspetto ha?

Puoi trovare alcuni esempi di implementazioni di diversi provider e ulteriori dettagli sui provider in cui viene visualizzato il logo nella [Pagina del gruppo BIMI](https://bimigroup.org/where-is-my-bimi-logo-displayed/){target="_blank"}.

## Chi è il gruppo BIMI?

Il gruppo BIMI è un gruppo di lavoro che sviluppa BIMI in quanto non solo copre il logo, ma anche i requisiti tecnici, giuridici e di conformità.

Il gruppo BIMI è composto da diverse parti interessate provenienti da diversi settori del settore: Google, Yahoo, Fastmail, Proofpoint, Mailchimp, Sendgrid, Valimail e Validity.

## Chi supporta BIMI?

L&#39;elenco dei provider di caselle di posta elettronica che supportano BIMI è in continua crescita. È possibile trovare un elenco aggiornato [qui](https://bimigroup.org/bimi-infographic/){target="_blank"} sia per i prestatori di servizi di supporto che per i prestatori che prendono in considerazione il BIMI.

A partire da aprile 2023, l’elenco include Gmail, Yahoo, La Poste, Fastmail, Onet.pl e Zone, Proofpoint come dispositivo anti-spam e Apple Mail (da iOS 16 in poi).

I nomi più importanti su quell&#39;elenco sono ovviamente Yahoo, Gmail e un recente utente: Apple con iOS 16. Apple ha un ruolo speciale nel mix in quanto non sono un provider di caselle postali, ma hanno aggiunto il supporto BIMI alla loro app di posta nativa. Le e-mail conformi a BIMI verranno visualizzate come &quot;e-mail con certificazione digitale&quot;, il che aumenta la fiducia nel marchio.

## Implementazione

L’implementazione di BIMI prevede diverse fasi:

1. Implementazione di DMARC (Domain Based Message Authentication, Reporting and Conformance) a livello di applicazione sia per il dominio di invio che per il relativo dominio organizzativo - [Ulteriori informazioni](#dmarc)

1. Creazione del logo del tuo marchio nel formato SVG TinyPS - [Ulteriori informazioni](#create-brand-logo)

1. Registrazione per un certificato contrassegno verificato (necessario solo per alcuni provider) - [Ulteriori informazioni](#vmc)

1. Pubblicare un record DNS BIMI con il logo e il certificato - [Ulteriori informazioni](#publish-bimi-record)

1. Buona reputazione - [Ulteriori informazioni](#good-reputation)

>[!NOTE]
>
>Tutti i passaggi devono essere disattivati.


### DMARC {#dmarc}

DMARC è uno standard che consente al brand di decidere cosa deve fare un provider di caselle postali con un’e-mail che ha esito negativo [autenticazione](../additional-resources/authentication.md). I cosiddetti criteri variano da &quot;nessuno&quot; su &quot;quarantena&quot; (posizionamento cartella spam) a &quot;rifiuto&quot; (blocco diretto della posta). Solo queste ultime due politiche sono denominate &quot;applicazione&quot; e sono ammissibili al BIMI. La posta inviata dall’Adobe passa l’autenticazione, in quanto SPF (Sender Policy Framework) e DKIM (Domain Keys Identified Mail) sono impostati per impostazione predefinita. L’Adobe sta configurando il DMARC sul dominio di invio su richiesta.

Oltre a DMARC nel dominio di invio, DMARC deve essere utilizzato anche a livello di applicazione per il dominio organizzativo (se il dominio di invio è news.example.com, example.com è il dominio organizzativo).

### Creazione del logo del brand {#create-brand-logo}

La creazione del logo deve rispettare i requisiti al 100%. Fai sempre riferimento al [Linee guida del gruppo BIMI](https://bimigroup.org/creating-bimi-svg-logo-files/){target="_blank"}.

Il logo deve essere archiviato in un percorso sicuro (HTTPS), nel caso in cui venga utilizzata una rete CDN (Content Delivery Network), è necessario disabilitare qualsiasi protezione che impedisca ai provider di cassette postali di ottenere il logo (ad esempio, Protezione bot).

Oltre ai requisiti tecnici, ci sono alcune raccomandazioni pratiche come avere un logo quadrato, avere un colore a tinta unita come sfondo e altri. Questi consigli sono utili per ottenere una visualizzazione ottimale. Alcuni fornitori hanno requisiti propri che si aggiungono a quelli del gruppo di lavoro BIMI. [Gmail](https://support.google.com/a/answer/10911027?sjid=903725605955621707-EU){target="_blank"} ad esempio, richiede che il logo sia di almeno 96 x 96 pixel.
La mancata conformità può comportare la mancata visualizzazione del logo.

### Certificato contrassegno verificato (VMC) {#vmc}

Un VMC (Verified Mark Certificate) è necessario solo per alcuni provider di cassette postali come Gmail e Apple ed è quindi facoltativo. Consigliamo di ottenere un VMC anche se per sfruttare davvero BIMI.

Un certificato di marchio verificato è una convalida legale che consente al brand di utilizzare il logo. Un&#39;autorità di certificazione controllerà questo aspetto attraverso l&#39;ufficio dei marchi presso il quale è registrato il logo del marchio. Questo processo richiede diverse convalide e controlli legali e può richiedere del tempo. Attualmente due CA (autorità di certificazione) rilasciano VMC: Digicert e Entrust. La prima serie di uffici dei marchi è costituita da Stati Uniti, Canada, UE, Regno Unito, Germania, Giappone, Australia e Spagna.

Come regola generale, è necessario un VMC per logo. Disporre di una VMC per il dominio dell’organizzazione coprirà i sottodomini e, con una funzione aggiuntiva, anche i domini diversi. Se si dispone di loghi diversi, è necessario più di un VMC. L&#39;autorità di certificazione o il partner con cui si sceglie di lavorare sarà di aiuto nella configurazione.

>[!NOTE]
>
>Si noti che i VMC hanno una tariffa annuale.

### Pubblicazione del record BIMI {#publish-bimi-record}

Una volta completati gli altri passaggi, è possibile pubblicare il record DNS BIMI. Il record si presenta così:

```
default._bimi.[domain] IN TXT "v=BIMI1; l=[SVG URL]; a=[PEM URL]
```

&quot;URL PEM&quot; è la posizione del file del certificato contrassegno verificato.

Per il dominio di invio, questa operazione deve essere eseguita per Adobe.

### Buona reputazione {#good-reputation}

La fiducia è fondamentale per BIMI. L’utente si fida del proprio provider di caselle postali per mostrare solo il logo per i mittenti legittimi, pertanto il provider di caselle postali deve fidarsi del marchio, e questo viene fatto dalla reputazione del mittente. Se hai una reputazione elevata, tutto è buono, ma in caso contrario, il logo non verrà visualizzato. Sfortunatamente, non ci sono informazioni o metriche che possiamo esaminare per capire se la reputazione è abbastanza alta.

Anche considerando gli sforzi e le spese per un VMC, questa parte non viene rimossa. Se il provider della cassetta postale non considera attendibile il marchio, il logo non verrà visualizzato.

## Suggerimenti

* Il gruppo BIMI offre un utile strumento di convalida per BIMI. Se si desidera verificare se tutto è configurato e pronto o solo verificare se il logo è conforme, visitare il sito [questo collegamento](https://bimigroup.org/bimi-generator/){target="_blank"}. Per quest’ultimo, fai clic su **[!UICONTROL Generate BIMI]** e inserisci un dominio segnaposto ma l’URL del logo corretto. L&#39;ispettore ti comunicherà se il logo è conforme.

* Puoi iniziare senza VMC, non vi è alcun danno sulla tua reputazione se il tuo record BIMI non include un URL VMC, ma il logo può già essere visualizzato in Yahoo.

* L&#39;attuazione del DMARC a livello organizzativo è una grande impresa. Alcune aziende sono specializzate nell&#39;aiutare i brand a raggiungere una piena adozione del DMARC.

* Viene pubblicato un ampio elenco di domande frequenti [qui](https://bimigroup.org/faqs-for-senders-esps/){target="_blank"}.
