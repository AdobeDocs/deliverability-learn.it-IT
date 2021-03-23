---
title: Impostazione del nome di dominio
description: Scopri come delegare un sottodominio ad Adobe Campaign.
feature: Attuazione
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 1e539b5df54250a5927701009e7a9c84e5d73fae
workflow-type: tm+mt
source-wordcount: '2032'
ht-degree: 2%

---


# Impostazione del nome di dominio

Questo documento descrive i requisiti aziendali e tecnici per la configurazione e la delega dei nomi di dominio. Per ospitare i componenti web (pagine di destinazione, pagina di rinuncia) per la piattaforma di Adobe in uso, dovrai selezionare un sottodominio che invia e-mail e, facoltativamente, un sottodominio rivolto all’esterno.

>[!NOTE]
>
>Puoi anche impostare nuovi sottodomini utilizzando il Pannello di controllo Campaign (disponibile come versione beta). Ulteriori informazioni in [questa sezione](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/setting-up-new-subdomain.html#must-read).

## Sottodomini

Ad Adobe, il marketing digitale può diventare il motore contestuale che alimenta il programma di marketing di coinvolgimento dei clienti del tuo marchio.  L’e-mail rimane la base dei programmi di marketing digitale. Tuttavia, raggiungere la casella in entrata è diventato più difficile che mai.

La creazione di un sottodominio per le campagne e-mail consente ai brand di isolare diversi tipi di traffico (marketing e aziende, ad esempio) in pool IP specifici e con domini specifici, velocizzando il [processo di riscaldamento degli IP](../../help/additional-resources/increase-reputation-with-ip-warming.md) e migliorando il recapito messaggi in generale. Se condividi un dominio e questo viene bloccato o aggiunto all&#39;elenco Bloccati, potrebbe influire sulla consegna della posta aziendale. Tuttavia, problemi o blocchi di reputazione su un dominio specifico per le tue comunicazioni di marketing e-mail avranno un impatto solo su quel flusso di e-mail.  L’utilizzo del dominio principale come mittente o indirizzo &quot;Da&quot; per più flussi di posta potrebbe anche interrompere l’autenticazione delle e-mail, bloccando o inserendo i messaggi nella cartella spam.

### Delega

La delega del nome di dominio è un metodo che consente al proprietario di un nome di dominio (tecnicamente: una zona DNS) per delegare una sua suddivisione (tecnicamente: una zona DNS sotto di essa, che può essere chiamata sottozona) a un’altra entità. In sostanza, se un cliente gestisce la zona &quot;example.com&quot;, può delegare la sottozona &quot;marketing.example.com&quot; ad Adobe Campaign.

Ciò significa che i server DNS di Adobe Campaign avranno piena autorità solo su quella zona e non sul dominio di primo livello. I server DNS di Adobe Campaign forniranno risposte autorevoli alle query sui nomi di dominio in quella zona, come &quot;t.marketing.example.com&quot; ma non &quot;www.example.com&quot;.

Delegando un sottodominio da utilizzare con Adobe Campaign, i client possono fare affidamento su un Adobe per mantenere l’infrastruttura DNS necessaria per soddisfare i requisiti di recapito dei messaggi standard del settore per i domini di invio di e-mail marketing, mantenendo e controllando il DNS per i propri domini e-mail interni.  La delega dei sottodomini consente:

I client mantengono l’immagine del proprio marchio utilizzando un alias DNS con i relativi nomi di dominio
Adobe di implementare autonomamente tutte le best practice tecniche per ottimizzare completamente il recapito messaggi durante l’invio di e-mail

## Opzioni di configurazione DNS

Per fornire un servizio gestito basato su cloud, Adobe incoraggia vivamente i clienti a utilizzare la delega dei sottodomini durante la distribuzione di Adobe Campaign.  Tuttavia, Adobe offre ai client un’opzione alternativa - Configurazione CNAME - per la configurazione del DNS.

| Opzione | Descrizione | Responsabilità Adobi | Responsabilità dei clienti |
|--- |------- |--- |--- |
| Delega dei sottodomini ad Adobe Campaign | Il client delega un sottodominio (email.example.com) ad Adobe. In questo scenario, Adobe è in grado di distribuire la Campaign come servizio gestito controllando e mantenendo tutti gli aspetti del DNS necessari per la consegna, il rendering e il tracciamento delle campagne e-mail. | Gestione completa del sottodominio e di tutti i record DNS richiesti per Adobe Campaign. | Delega corretta del sottodominio all’Adobe |
| Utilizzo dei CNAME | Il client crea un sottodominio e utilizza i CNAME per puntare a record specifici per l’Adobe.  Utilizzando questa configurazione, sia Adobe che il cliente condividono la responsabilità della manutenzione del DNS. | Gestione dei record DNS richiesti per Adobe Campaign. | Creazione e controllo del sottodominio e creazione/gestione dei record CNAME richiesti per Adobe Campaign. |

## Record DNS richiesti

| Tipo di record | Finalità | Esempi Record/Contenuto |
|--- |--- |--- |
| MX | Specificare server di posta per i messaggi in arrivo | <i>email.example.com</i></br><i>10 inbound.email.example.com</i> |
| SPF (TXT) | Framework dei criteri di invio | <i>email.example.com</i></br>&quot;v=spf1 redirect=__spf.campaign.adobe.com&quot; |
| DKIM (TXT) | Posta identificata DomainKeys | <i>client._domainkey.email.example.com</i></br>&quot;v=DKIM1; k=rsa;&quot; &quot;DKIMPUBLICKEY QUI&quot; |
| DMARC (TXT) | Autenticazione dei messaggi basata su dominio | Reporting e conformità | _dmarc.email.example.com</br>&quot;v=DMARC1; p=none; rua=mailto:mailauth-reports@myemail.com&quot; |
| Host Record (A) | Visualizzare in mirroring le pagine, l&#39;hosting delle immagini e i collegamenti di tracciamento, tutti i domini di invio | m.email.example.com IN A 123.111.100.99</br>t.email.example.com IN A 123.111.100.98</br>email.example.com IN A 123.111.100.97 |
| DNS inversi (PTR) | Mappa gli indirizzi IP del client su un nome host con marchio del cliente | 18.101.100.192.in-addr.arpa puntatore del nome di dominio r18.email.example.com |
| CNAME | Fornisce un alias a un altro nome di dominio | t1.email.example.com è un alias per | t1.email.example.campaign.adobe.com |

## Requisiti di configurazione

### Delega del sottodominio

Questo richiede che il client crei un sottodominio nei propri server DNS e definisca i server dei nomi per questo sottodominio come quelli gestiti da Adobe.  Ad esempio, un client il cui nome di dominio principale è &quot;example.com&quot; e che desidera delegare la gestione di &quot;marketing.example.com&quot; ad Adobe per le consegne di e-mail deve materializzare questa delega per aggiungere i seguenti record di tipo al suo DNS:

```
marketing.example.com. NS a.ns.campaign.adobe.com.
marketing.example.com. NS b.ns.campaign.adobe.com.
marketing.example.com. NS c.ns.campaign.adobe.com.
marketing.example.com. NS d.ns.campaign.adobe.com.
```

La delega di un nome di dominio implica che questo dominio sarà dedicato alla consegna di e-mail tramite la piattaforma Adobe Campaign e quindi non può essere utilizzato per altri mezzi (ad esempio, l’invio di e-mail da un’altra infrastruttura e-mail).

Durante il processo di configurazione, Adobe garantirà che il dominio sia allegato all’infrastruttura e-mail in ingresso Adobe per gestire ed elaborare le e-mail rimbalzate che ritornano a questi domini (configurazione record DNS di tipo MX).

### Utilizzo dei CNAME

Se il client sceglie di utilizzare i CNAME anziché delegare un sottodominio ad Adobe, durante la fase di configurazione, Adobe fornirà i record da inserire nei server DNS client e configurerà i valori corrispondenti nei server DNS di Adobe Campaign.

## Requisiti generali per la distribuzione

Quando implementi una nuova soluzione di marketing aziendale, sono necessari componenti rivolti all’esterno.  ad esempio l’hosting di pagine di destinazione e moduli web, l’impostazione di collegamenti e pagine web da tracciare, la visualizzazione di pagine mirror e la configurazione di una pagina di rinuncia.

Questi requisiti vengono gestiti tramite componenti ospitati sia da Adobe che dal cliente, ma includono URL che possono essere visualizzati dai destinatari delle e-mail.  Per evitare di disporre di URL che indicano la soluzione tecnica sottostante o il provider di hosting, è possibile impostare sottodomini per renderlo trasparente ai destinatari delle e-mail.  Ad esempio, quando si cerca un URL come http://www.customer.com/, il dominio sarà &quot;www.customer.com&quot;.  Il sottodominio di questo sarà &quot;www&quot;.

### Requisiti del sottodominio

Determina i sottodomini da utilizzare per gli URL con brand (pagine mirror e URL di tracciamento) dall’applicazione Adobe Campaign.  Inoltre, decidi quale sarà l’indirizzo &quot;Da&quot;, &quot;Da nome&quot; e &quot;Indirizzo di risposta&quot; per ciascun sottodominio sulle consegne e-mail.

Compila la tabella seguente, la prima riga è solo un esempio.

| Subdomain | Indirizzo mittente | Nome mittente | Indirizzo di risposta |
|--- |--- |--- |--- |
| emails.customer.com | news@emails.customer.com | Customer | customercare@customer.com |
| </br> | </br> | </br> | </br> |

>[!NOTE]
>
>* Lo scopo del campo &quot;Indirizzo di risposta&quot; è quando si desidera che il destinatario risponda a un indirizzo diverso da &quot;Indirizzo di origine&quot;.  Sebbene non sia un campo obbligatorio, l&#39;Adobe consiglia vivamente che l&#39;&quot;Indirizzo di risposta&quot; sia valido e collegato a una cassetta postale monitorata.  Questa cassetta postale deve essere ospitata dal cliente.  Potrebbe trattarsi di una cassetta postale di supporto, ad esempio customercare@customer.com, in cui le e-mail vengono lette e a cui vengono fornite risposte.
>* Se il cliente non sceglie &quot;Indirizzo di risposta&quot;, l&#39;indirizzo predefinito è sempre `<tenant>-<type>-<env>@<subdomain>`.
>* Quando il &quot;Indirizzo di risposta&quot; è impostato in questo modo, le risposte verranno inviate a una casella di posta non monitorata.
>* Quando si inviano e-mail da Adobe Campaign, la cassetta postale &quot;Da indirizzo&quot; non viene monitorata e gli utenti del marketing non possono accedere a questa cassetta postale. Adobe Campaign inoltre non offre la possibilità di rispondere automaticamente o inoltrare automaticamente le e-mail ricevute in questa cassetta postale.
>* L’indirizzo della campagna da/mittente e l’indirizzo di errore non possono essere &quot;abusivi&quot; o &quot;postmaster&quot;.


## Delega dei sottodomini

I sottodomini scelti per la piattaforma Adobe Campaign devono essere delegati creando quattro record del server dei nomi (NS).  Questo consente di delegare correttamente ad Adobe il sottodominio.  Di seguito è riportato un esempio di delega di sottodominio e le relative istruzioni DNS.  Sostituisci &quot;email.customer.com&quot; con il sottodominio che desideri delegare.  Tieni presente che il sottodominio deve essere univoco e non può essere già utilizzato da un’altra parte (ad esempio, un ESP o MSP esistente).

| Sottodominio delegato | Istruzioni DNS |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.cacampaign.adobe.com.  </br> `<subdomain>` NS b.ns.cacampaign.adobe.com.  </br> `<subdomain>` NS c.ns.cacampaign.adobe.com.  </br> `<subdomain>` NS d.ns.cacampaign.adobe.com. |

## Tracciamento, pagine mirror, risorse

Una volta che il sottodominio o i sottodomini di invio dell’e-mail vengono delegati correttamente ad Adobe Campaign, il team TechOps di Adobe creerà due o più domini di livello inferiore per gestire il tracciamento e il mirroring delle pagine in modo indipendente.

| Tipo | Domain |
|--- |--- |
| Pagine specchiate | m.`<subdomain>` |
| Tracking | t.`<subdomain>` |
| Risorse | res`<subdomain>` |

## Distribuzione cloud (opzionale)

Questo si applica solo se Adobe Campaign Classic è interamente ospitato nel cloud per Adobe.  Questa è una configurazione facoltativa.

Tutti i sondaggi, i moduli web e le pagine di destinazione da sviluppare sono gestiti tramite Adobe Campaign interamente ospitato nel cloud.  Se necessario, un sottodominio aggiuntivo può essere delegato ad Adobe (ad esempio, web.customer.com) da utilizzare per qualsiasi componente web all’interno dello strumento.  Tieni presente che il sottodominio deve essere univoco e non può essere utilizzato da un’altra parte (ad esempio, un ESP o MSP esistente).

| Sottodominio delegato | Istruzioni DNS |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.cacampaign.adobe.com.</br>`<subdomain>` NS b.ns.cacampaign.adobe.com.</br>`<subdomain>` NS c.ns.cacampaign.adobe.com.</br>`<subdomain>` NS d.ns.cacampaign.adobe.com. |

>[!NOTE]
>
>Per impostazione predefinita, qualsiasi componente Web nello strumento utilizza il sottodominio iniziale delegato per l’e-mail.

## Distribuzione di messaggi cloud (facoltativo)

Nel caso in cui l’istanza di marketing Adobe Campaign Classic sia ospitata on-premise presso il cliente, il cliente dovrà effettuare configurazioni tecniche aggiuntive.

Tutti i sondaggi, i moduli web e le pagine di destinazione da sviluppare vengono gestiti tramite l’istanza di marketing Adobe Campaign, dove sono presenti i record dei destinatari.

È necessaria una configurazione CNAME DNS aggiuntiva per distribuire componenti web rivolti all’esterno ospitati dall’istanza di marketing Adobe Campaign.  Questo consentirà ai componenti web (ad esempio, web.customer.com) di essere accessibili al pubblico su Internet e contrassegnati con il dominio del cliente.

Sarà inoltre necessario configurare i firewall per consentire l’accesso all’istanza di marketing Adobe Campaign che ospita questi componenti web (sulle porte 80 o 443).

**Best practice Recommendations:**

Il sottodominio che ospita i componenti web sarà visibile ai clienti, quindi assicurati di renderlo correttamente contrassegnato e semplice da ricordare in quanto potrebbe essere necessario digitarlo manualmente, ad esempio: https://web.customer.com.
Se è necessario ospitare moduli su pagine protette (HTTPS), sarà necessaria un’ulteriore configurazione tecnica, come descritto di seguito.

| Sottodominio delegato | Istruzioni DNS |
|--- |--- |
| `<subdomain>` | `<subdomain>` CNAME  `<internal customer server>` |

## Servizi resi

A seguito di queste delegazioni, l’infrastruttura istituita dall’Adobe garantisce che i seguenti servizi siano eseguiti per ciascun dominio di invio delegato o con alias CNAME:

* Creazione di caselle in entrata postmaster@ e abusive@
* Impostazione dei loop di feedback per il dominio delegato
* Su richiesta, Adobe configurerà anche un record DMARC come specificato. Il tuo consulente di recapito può assistere nella progettazione di una politica DMARC a lungo termine e pianificare i tuoi domini di invio.
I parametri stabiliti dall&#39;Adobe sono validi solo dal momento in cui la delega è stata completata e poi verificata per Adobe e rimangono funzionanti.  Tutte le offerte di Adobe Campaign Cloud includono la delega di nomi di dominio come standard.

## Condizioni di fatturazione e di attuazione

* In base al contratto iniziale e al tipo di pacchetto selezionato, possono essere incluse altre delegazioni oltre a quelle incluse come standard oltre a questa delega iniziale,
* Oltre a queste delegazioni incluse, verranno fatturate ulteriori delegazioni,
* Il metodo di fatturazione per queste ulteriori delegazioni viene a un costo mensile supplementare, come specificato nel contratto iniziale.

Queste delegazioni saranno accettate purché il CLIENT scelga i nomi di dominio associati dedicati alle consegne tramite lo strumento Adobe Campaign e che i prerequisiti di delega descritti nel documento pertinente siano correttamente implementati.

## Interruzione dei servizi

In qualsiasi momento, il CLIENT sarà in grado di fare una richiesta scritta per non beneficiare più dei servizi di delega e di assumere le configurazioni DNS necessarie.

In questo caso, l’Adobe fornisce al CLIENT una stima che specifica il numero di giorni di servizio necessari per tornare alla modalità di delega non di dominio.

L&#39;Adobe sarà esonerato da qualsiasi responsabilità per l&#39;impegno del tasso di consegna di cui sopra se il Cliente non rispetta gli impegni sopra indicati.

La cessazione del servizio di Marketing Cloud porterà automaticamente alla fine delle delegazioni di dominio e alla manutenzione DNS per tali domini per Adobe.

## Monitoraggio dei sottodomini tramite il Pannello di controllo Campaign

Una volta configurati i sottodomini per l’istanza, puoi monitorarli utilizzando il Pannello di controllo Campaign .

Questo ti consente di visualizzare tutti i sottodomini delegati ad Adobe Campaign e di richiedere il rinnovo dei loro certificati SSL.

Per ulteriori informazioni, consulta la [documentazione dedicata](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-subdomains.html#subdomains-and-certificates).

>[!NOTE]
>
>[I ](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html) pannelli di controllo sono disponibili solo per i clienti che utilizzano Adobe Managed Services.