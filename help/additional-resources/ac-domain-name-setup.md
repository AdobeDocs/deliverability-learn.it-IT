---
title: Impostazione del nome di dominio
description: Scopri come delegare un sottodominio ad Adobe Campaign.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 4d52d197-d20e-450c-bfcf-e4541c474be4
source-git-commit: 82f7254a9027f79d2af59aece81f032105c192d5
workflow-type: tm+mt
source-wordcount: '2061'
ht-degree: 2%

---

# Impostazione del nome di dominio

Questo documento descrive i requisiti tecnici e aziendali per la configurazione e la delega dei nomi di dominio. Dovrai selezionare un sottodominio di invio di e-mail e, facoltativamente, un sottodominio rivolto verso l’esterno per ospitare i componenti web (pagine di destinazione, pagina di rinuncia) per la piattaforma di Adobe in uso.

>[!NOTE]
>
>Puoi anche impostare nuovi sottodomini utilizzando il Pannello di controllo Campaign (disponibile come versione beta). Ulteriori informazioni in [questa sezione](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/setting-up-new-subdomain.html#must-read).

## Sottodomini

Ad Adobe, il marketing digitale può davvero diventare il motore contestuale che alimenta il programma di marketing del coinvolgimento dei clienti del tuo marchio.  L’e-mail rimane la base dei programmi di marketing digitale. Tuttavia, è diventato più difficile che mai raggiungere la casella in entrata.

La creazione di un sottodominio per le campagne e-mail consente ai brand di isolare diversi tipi di traffico (ad esempio marketing e aziendale) in pool IP specifici e con domini specifici, velocizzando il [Processo di riscaldamento IP](../../help/additional-resources/increase-reputation-with-ip-warming.md) e migliorare complessivamente la consegna dei messaggi. Se condividi un dominio e questo viene bloccato o aggiunto all’elenco Bloccati, ciò potrebbe influire sulla consegna della posta aziendale. Tuttavia, problemi o blocchi di reputazione su un dominio specifico per le comunicazioni di e-mail marketing avranno un impatto solo su tale flusso di e-mail.  L’utilizzo del dominio principale come mittente o indirizzo &quot;Da&quot; per più flussi di posta potrebbe inoltre interrompere l’autenticazione e-mail, causando il blocco o l’inserimento dei messaggi nella cartella di posta indesiderata.

### Delega

La delega del nome di dominio è un metodo che consente al proprietario di un nome di dominio (tecnicamente: una zona DNS) di delegare una sua suddivisione (tecnicamente: una zona DNS al di sotto di essa, che può essere definita sottozona) a un’altra entità. Fondamentalmente, se un cliente gestisce la zona &quot;example.com&quot;, può delegare la sottozona &quot;marketing.example.com&quot; ad Adobe Campaign.

Ciò significa che i server DNS di Adobe Campaign avranno autorità completa solo su tale zona e non sul dominio di primo livello. I server DNS di Adobe Campaign forniranno risposte autorevoli alle query sui nomi di dominio in tale zona, ad esempio &quot;t.marketing.example.com&quot; stesso ma non &quot;www.example.com&quot;.

Delegando un sottodominio da utilizzare con Adobe Campaign, i client possono contare su Adobe per mantenere l’infrastruttura DNS necessaria per soddisfare i requisiti di recapito dei messaggi standard di settore per i domini di invio del marketing e-mail, continuando al contempo a mantenere e controllare il DNS per i domini e-mail interni.  La delega dei sottodomini consente:

I clienti devono mantenere la propria immagine del marchio utilizzando un alias DNS con i relativi nomi di dominio, ad Adobe per implementare in modo autonomo tutte le best practice tecniche al fine di ottimizzare il recapito messaggi durante l’invio di e-mail

## Opzioni di configurazione DNS

Al fine di fornire un servizio gestito basato su cloud, Adobe incoraggia vivamente i clienti a utilizzare la delega dei sottodomini durante la distribuzione di Adobe Campaign.  Tuttavia, Adobe offre ai client un’opzione alternativa, ovvero la configurazione CNAME, per la configurazione del DNS.

| Opzione | Descrizione | Responsabilità Adobe | Responsabilità del cliente |
|--- |------- |--- |--- |
| Delega dei sottodomini ad Adobe Campaign | Il client delega un sottodominio (email.example.com) all’Adobe. In questo scenario, Adobe è in grado di fornire Campaign come servizio gestito controllando e mantenendo tutti gli aspetti del DNS necessari per la distribuzione, il rendering e il tracciamento delle campagne e-mail. | Gestione completa del sottodominio e di tutti i record DNS richiesti per Adobe Campaign. | Delega corretta del sottodominio ad Adobe |
| Utilizzo dei CNAME | Il client crea un sottodominio e utilizza i CNAME per puntare a record specifici dell’Adobe.  Utilizzando questa configurazione, sia Adobe che il cliente condividono la responsabilità della manutenzione del DNS. | Gestione dei record DNS richiesti per Adobe Campaign. | Creazione e controllo del sottodominio e creazione/gestione dei record CNAME richiesti per Adobe Campaign. |

## Record DNS richiesti

| Tipo di record | Finalità | Esempi di record/contenuti |
|--- |--- |--- |
| MX | Specificare i server di posta per i messaggi in arrivo | <i>email.example.com</i></br><i>10 inbound.email.example.com</i> |
| SPF (TXT) | Framework criteri mittente | <i>email.example.com</i></br>&quot;v=spf1 redirect=__spf.campaign.adobe.com&quot; |
| DKIM (TXT) | Posta identificata DomainKeys | <i>client._domainkey.email.example.com</i></br>&quot;v=DKIM1; k=rsa;&quot; &quot;DKIMPUBLICKEY HERE&quot; |
| Ospita i record (A) | Pagine mirror, hosting di immagini e collegamenti di tracciamento, tutti i domini di invio | m.email.example.com IN A 123.111.100.99</br>t.email.example.com IN A 123.111.100.98</br>email.example.com IN A 123.111.100.97 |
| DNS inverso (PTR) | Mappa gli indirizzi IP del client su un nome host con marchio client | 18.101.100.192.in-addr.arpa puntatore al nome di dominio r18.email.example.com |
| CNAME | Fornisce un alias a un altro nome di dominio | t1.email.example.com è un alias per t1.email.example.campaign.adobe.com |


Si consiglia di utilizzare DMARC (Domain-based Message Authentication, Reporting, and Conformance) per autenticare i mittenti di posta e assicurarsi che i sistemi di posta elettronica di destinazione considerino attendibili i messaggi inviati dal dominio.

Esempio di record TXT DMARC:

```
_dmarc.email.example.com

“v=DMARC1; p=none; rua=mailto:mailauth-reports@myemail.com” 
```

Puoi implementare DMARC manualmente o contattare un Adobe per aiutarti a configurare DMARC per il tuo marchio.

## Requisiti di configurazione

### Delega dei sottodomini

Questo richiede che il client crei un sottodominio nei propri server DNS e definisca i server dei nomi per questo sottodominio come quelli gestiti da Adobe.  Ad esempio, un client il cui nome di dominio principale è &quot;example.com&quot; e che desidera delegare la gestione di &quot;marketing.example.com&quot; ad Adobe per le consegne e-mail dovrà materializzare questa delega per aggiungere i seguenti record di tipo al proprio DNS:

```
marketing.example.com. NS a.ns.campaign.adobe.com.
marketing.example.com. NS b.ns.campaign.adobe.com.
marketing.example.com. NS c.ns.campaign.adobe.com.
marketing.example.com. NS d.ns.campaign.adobe.com.
```

La delega di un nome di dominio implica che questo dominio sarà dedicato alla consegna dei messaggi e-mail tramite la piattaforma Adobe Campaign e non può quindi essere utilizzato per altri mezzi (ad esempio, l’invio di messaggi e-mail da un’altra infrastruttura e-mail).

Durante il processo di configurazione, Adobe garantirà che il dominio sia associato all’infrastruttura e-mail in arrivo di Adobe per gestire ed elaborare le e-mail di rimbalzo che tornano a questi domini (configurazione del record DNS di tipo MX).

### Utilizzo dei CNAME

Se il client sceglie di utilizzare i CNAME anziché delegare un sottodominio ad Adobe, durante la fase di configurazione Adobe fornirà i record da inserire nei server DNS client e configurerà i valori corrispondenti nei server DNS di Adobe Campaign.

## Requisiti generali per la distribuzione

Quando si implementa una nuova soluzione di marketing aziendale, sono necessari componenti rivolti verso l’esterno.  Questi includono l’hosting di pagine di destinazione e moduli web, la configurazione di collegamenti e pagine web da tracciare, la visualizzazione di pagine mirror e la configurazione di una pagina di rinuncia.

Questi requisiti vengono gestiti tramite componenti in hosting sia dall’Adobe che dal cliente, ma includono URL che possono essere visualizzati dai destinatari delle e-mail.  Per evitare di avere URL che indicano la soluzione tecnica o il provider di hosting sottostante, è possibile configurare i sottodomini per rendere trasparente l’operazione ai destinatari delle e-mail.  Ad esempio, quando si cerca un URL come, http://www.customer.com/, il dominio sarà &quot;www.customer.com&quot;.  Il sottodominio di questo sarebbe &quot;www&quot;.

### Requisiti del sottodominio

Determina i sottodomini da utilizzare per gli URL con marchio (pagine mirror e URL di tracciamento) dall’applicazione Adobe Campaign.  Inoltre, decidi quale sarà &quot;Indirizzo mittente&quot;, &quot;Nome mittente&quot; e &quot;Indirizzo destinatario risposta&quot; per ciascun sottodominio nelle consegne e-mail.

Completa la tabella seguente; la prima riga è solo un esempio.

| Subdomain | Indirizzo mittente | Nome mittente | Indirizzo di risposta |
|--- |--- |--- |--- |
| emails.customer.com | news@emails.customer.com | Customer | customercare@customer.com |
| </br> | </br> | </br> | </br> |

>[!NOTE]
>
>* Il campo &quot;Indirizzo di risposta&quot; ha lo scopo di indicare quando si desidera che il destinatario risponda a un indirizzo diverso da quello del campo &quot;Indirizzo mittente&quot;.  Anche se non è un campo obbligatorio, Adobe consiglia vivamente che &quot;Reply-To Address&quot; (Indirizzo di risposta) sia valido e collegato a una cassetta postale monitorata.  Questa cassetta postale deve essere ospitata dal cliente.  Potrebbe trattarsi di una casella di posta di supporto, ad esempio customercare@customer.com, in cui le e-mail vengono lette e a cui viene inviata una risposta.
>* Se il cliente non ha scelto alcun &quot;Indirizzo di risposta&quot;, l’indirizzo predefinito è sempre `<tenant>-<type>-<env>@<subdomain>`.
>* Quando l&#39;indirizzo di risposta è impostato in questo modo, le risposte verranno inviate a una cassetta postale non monitorata.
>* Quando si inviano e-mail da Adobe Campaign, la cassetta postale &quot;Indirizzo mittente&quot; non viene monitorata e gli utenti marketing non possono accedere a questa cassetta postale. Inoltre, Adobe Campaign non offre la possibilità di rispondere automaticamente o inoltrare automaticamente le e-mail ricevute in questa casella di posta.
>* L’indirizzo del mittente/mittente della campagna e l’indirizzo di errore non possono essere &quot;abusivi&quot; o &quot;postmaster&quot;.

## Delega dei sottodomini

I sottodomini scelti per essere utilizzati per la piattaforma Adobe Campaign devono essere delegati creando quattro record del server dei nomi (NS).  Questo consente di delegare correttamente il sottodominio ad Adobe.  Di seguito è riportato un esempio di delega di sottodominio e delle relative istruzioni DNS.  Sostituisci ‘emails.customer.com’ con il sottodominio che desideri delegare.  Il sottodominio deve essere univoco e non può essere già utilizzato da un’altra parte (ad esempio, un ESP o MSP esistente).

| Sottodominio delegato | Istruzioni DNS |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com. </br> `<subdomain>` NS b.ns.campaign.adobe.com. </br> `<subdomain>` NS c.ns.campaign.adobe.com. </br> `<subdomain>` NS d.ns.campaign.adobe.com. |

## Tracciamento, pagine mirror, risorse

Una volta che i sottodomini di invio delle e-mail sono stati delegati correttamente ad Adobe Campaign, il team TechOps di Adobe creerà due o più domini di livello inferiore per gestire il tracciamento e il mirroring delle pagine in modo indipendente.

| Tipo | Domain |
|--- |--- |
| Pagine mirror | m.`<subdomain>` |
| Tracciamento | t.`<subdomain>` |
| Risorse | ris.`<subdomain>` |

## Distribuzione cloud (opzionale)

Questo si applica solo se Adobe Campaign Classic è completamente ospitato nel cloud da Adobe.  Questa è una configurazione facoltativa.

Tutti i sondaggi, i moduli web e le pagine di destinazione da sviluppare vengono gestiti tramite Adobe Campaign completamente ospitati nel cloud.  Se necessario, è possibile delegare un ulteriore sottodominio ad Adobe (ad esempio, web.customer.com) da utilizzare per qualsiasi componente web all’interno dello strumento.  Tieni presente che il sottodominio deve essere univoco e non può essere utilizzato da un’altra parte (ad esempio, un ESP o MSP esistente).

| Sottodominio delegato | Istruzioni DNS |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com.</br>`<subdomain>` NS b.ns.campaign.adobe.com.</br>`<subdomain>` NS c.ns.campaign.adobe.com.</br>`<subdomain>` NS d.ns.campaign.adobe.com. |

>[!NOTE]
>
>Per impostazione predefinita, qualsiasi componente web nello strumento utilizzerà il sottodominio iniziale delegato da utilizzare per l’e-mail.

## Distribuzione dei messaggi cloud (opzionale)

Nel caso in cui l’istanza di marketing Adobe Campaign Classic sia ospitata on-premise presso il cliente, quest’ultimo dovrà effettuare configurazioni tecniche aggiuntive.

Eventuali sondaggi, moduli web e pagine di destinazione da sviluppare vengono gestiti tramite l’istanza di marketing Adobe Campaign, in cui sono presenti i record dei destinatari.

È necessaria una configurazione DNS CNAME aggiuntiva per distribuire componenti web rivolti verso l’esterno e ospitati dall’istanza marketing di Adobe Campaign.  In questo modo i componenti web (ad esempio, web.customer.com) saranno accessibili al pubblico su Internet e contrassegnati con il dominio del cliente.

I firewall devono essere configurati anche per consentire l’accesso all’istanza marketing di Adobe Campaign che ospita questi componenti web (sulla porta 80 o 443).

**Best Practice Recommendations:**

Il sottodominio con cui ospitare i componenti web sarà visibile ai clienti, quindi accertati di personalizzarlo correttamente e di ricordarlo facilmente, in quanto potrebbe essere necessario digitarlo manualmente, ad esempio: https://web.customer.com.
Se è necessario che i moduli siano ospitati su pagine sicure (HTTPS), sarà necessaria un’ulteriore configurazione tecnica, descritta di seguito.

| Sottodominio delegato | Istruzioni DNS |
|--- |--- |
| `<subdomain>` | `<subdomain>` CNAME `<internal customer server>` |

## Servizi sottoposti a rendering

A seguito di tali deleghe, l’infrastruttura istituita da Adobe garantisce l’esecuzione dei seguenti servizi per ciascun dominio di invio delegato o con alias CNAME:

* Creazione di caselle in entrata postmaster@ e abusive@
* Configurazione dei cicli di feedback per il dominio delegato
* Su richiesta, Adobe configurerà anche un record DMARC come specificato. Il tuo consulente di recapito messaggi è in grado di assistere nella progettazione di una policy DMARC a lungo termine e nella pianificazione dei domini di invio.
I parametri stabiliti da Adobe sono validi solo dal momento in cui la delega è stata completata e poi verificata da Adobe e rimangono funzionali.  Tutte le offerte di Adobe Campaign Cloud includono la delega dei nomi di dominio come standard.

## Fatturazione e condizioni di implementazione

* In base al contratto iniziale e al tipo di pacchetto selezionato, possono essere incluse altre deleghe oltre a quelle standard oltre a questa delega iniziale,
* Oltre alle delegazioni incluse, saranno fatturate altre delegazioni,
* Il metodo di fatturazione per queste deleghe supplementari prevede un costo mensile supplementare, come specificato nel contratto iniziale.

Queste deleghe saranno accettate a condizione che il CLIENTE scelga i nomi di dominio associati dedicati alle consegne tramite lo strumento Adobe Campaign e che i prerequisiti per la delega descritti nel relativo documento siano correttamente implementati.

## Interruzione del servizio

In qualsiasi momento, il CLIENT sarà in grado di effettuare una richiesta scritta per non beneficiare più dei servizi di delega e di assumere autonomamente le necessarie configurazioni DNS.

In questo caso, Adobe fornirà al CLIENT una stima che descrive il numero di giorni di servizio necessari per tornare alla modalità senza delega del dominio.

L&#39;Adobe sarà sollevato da qualsiasi responsabilità per l&#39;impegno del Tasso di Consegna di cui sopra se il Cliente non rispetta gli impegni di cui sopra.

La cessazione del servizio di Marketing Cloud comporterà automaticamente la fine delle deleghe di dominio e la manutenzione DNS per tali domini per Adobe.

## Monitoraggio dei sottodomini tramite il Pannello di controllo Campaign

Una volta configurati i sottodomini per l’istanza, puoi monitorarli utilizzando il Pannello di controllo Campaign.

Questo ti consente di visualizzare tutti i sottodomini delegati ad Adobe Campaign, nonché di richiedere il rinnovo dei loro certificati SSL.

Per ulteriori informazioni, consulta la [documentazione dedicata](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-subdomains.html#subdomains-and-certificates).

>[!NOTE]
>
>[Pannello di controllo Campaign](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=it) è disponibile solo per i clienti che utilizzano Adobe Managed Services.
