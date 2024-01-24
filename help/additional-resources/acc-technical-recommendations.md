---
title: 'Campaign Classic: consigli tecnici'
description: Scopri le tecniche, le configurazioni e gli strumenti che puoi utilizzare per migliorare il tasso di consegna con Adobe Campaign Classic.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 39ed3773-18bf-4653-93b6-ffc64546406b
source-git-commit: f7c2dcbf1bb86d7018c31b1ae2ef29903fb758aa
workflow-type: tm+mt
source-wordcount: '1871'
ht-degree: 1%

---

# Campaign Classic: consigli tecnici {#technical-recommendations}

Di seguito sono elencate diverse tecniche, configurazioni e strumenti che puoi utilizzare per migliorare il tasso di consegna dei messaggi quando utilizzi Adobe Campaign Classic.

## Configurazione {#configuration}

### DNS inverso {#reverse-dns}

Adobe Campaign controlla se viene fornito un DNS inverso per un indirizzo IP e che questo punti correttamente all’IP.

Un punto importante nella configurazione della rete è assicurarsi che per ogni indirizzo IP dei messaggi in uscita sia definito un DNS inverso corretto. Ciò significa che per un determinato indirizzo IP esiste un record DNS inverso (record PTR) con un DNS (record A) corrispondente che esegue il ciclo all’indirizzo IP iniziale.

La scelta del dominio per un DNS inverso ha un impatto sulla gestione di determinati ISP. AOL, in particolare, accetta solo cicli di feedback con un indirizzo nello stesso dominio del DNS inverso (vedi [Ciclo di feedback](#feedback-loop)).

>[!NOTE]
>
>È possibile utilizzare [questo strumento esterno](https://mxtoolbox.com/SuperTool.aspx) per verificare la configurazione di un dominio.

### Regole MX {#mx-rules}

Le regole MX (Mail eXchanger) sono le regole che gestiscono la comunicazione tra un server di invio e un server ricevente.

Più precisamente, vengono utilizzati per controllare la velocità con cui l’MTA di Adobe Campaign (Message Transfer Agent) invia e-mail a ogni singolo dominio e-mail o ISP (ad esempio, hotmail.com, comcast.net). Queste regole si basano in genere sui limiti pubblicati dagli ISP (ad esempio, non includono più di 20 messaggi per ogni connessione SMTP).

>[!NOTE]
>
>Per ulteriori informazioni sulla gestione MX in Adobe Campaign Classic, consulta [questa sezione](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/additional-configurations/email-deliverability.html#mx-configuration).

### TLS {#tls}

TLS (Transport Layer Security) è un protocollo di crittografia che può essere utilizzato per proteggere la connessione tra due server e-mail e proteggere il contenuto di un’e-mail dall’essere letto da chiunque non sia il destinatario desiderato.

### Dominio del mittente {#sender-domain}

Per definire il dominio utilizzato per il comando HELO, modificare il file di configurazione dell&#39;istanza (conf/config-instance.xml) e definire un attributo &quot;localDomain&quot; come segue:

```
<serverConf>
  <shared>
    <dnsConfig localDomain="mydomain.net"/>
  </shared>
</serverConf>
```

Il dominio MAIL FROM è il dominio utilizzato nei messaggi tecnici di mancato recapito. Questo indirizzo è definito nella procedura guidata di distribuzione o tramite l&#39;opzione NmsEmail_DefaultErrorAddr.

### Record SPF {#dns-configuration}

Attualmente un record SPF può essere definito su un server DNS come record di tipo TXT (codice 16) o record di tipo SPF (codice 99). Un record SPF assume la forma di una stringa di caratteri. Ad esempio:

```
v=spf1 ip4:12.34.56.78/32 ip4:12.34.56.79/32 ~all
```

definisce i due indirizzi IP, 12.34.56.78 e 12.34.56.79, come autorizzati a inviare e-mail per il dominio. **~tutti** significa che qualsiasi altro indirizzo deve essere interpretato come SoftFail.

Recommendations per definire un record SPF:

* Aggiungi **~tutti** (SoftFail) o **-all** (Non riuscito) alla fine per rifiutare tutti i server diversi da quelli definiti. In caso contrario, i server saranno in grado di forgiare questo dominio (con una valutazione neutra).
* Non aggiungere **ptr** (openspf.org consiglia di non farlo come costoso e inaffidabile).

>[!NOTE]
>
>Ulteriori informazioni su SPF in [questa sezione](/help/additional-resources/authentication.md#spf).

## Autenticazione

>[!NOTE]
>
>Ulteriori informazioni sui diversi tipi di autenticazione delle e-mail disponibili in [questa sezione](/help/additional-resources/authentication.md).

### DKIM {#dkim-acc}

>[!NOTE]
>
>Per le installazioni in hosting o ibride, se hai effettuato l’aggiornamento al [MTA avanzato](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/sending-emails/sending-an-email/sending-with-enhanced-mta.html#sending-messages), la firma di autenticazione dell’e-mail DKIM viene eseguita dall’MTA avanzato per tutti i messaggi di tutti i domini.

Utilizzo di [DKIM](/help/additional-resources/authentication.md#dkim) con Adobe Campaign Classic richiede i seguenti prerequisiti:

**Dichiarazione di opzione Adobe Campaign**: in Adobe Campaign, la chiave privata DKIM si basa su un selettore DKIM e un dominio. Attualmente non è possibile creare più chiavi private per lo stesso dominio/sottodominio con selettori diversi. Non è possibile definire quale dominio/sottodominio selettore deve essere utilizzato per l’autenticazione né nella piattaforma né nell’e-mail. In alternativa, la piattaforma selezionerà una delle chiavi private, il che significa che l’autenticazione ha un’alta probabilità di non riuscire.

* Se hai configurato DomainKeys per la tua istanza di Adobe Campaign, devi solo selezionare **dkim** nel [Regole di gestione dei domini](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html#email-management-rules). In caso contrario, segui gli stessi passaggi di configurazione (chiave privata/pubblica) di DomainKeys (che ha sostituito DKIM).
* Non è necessario abilitare sia DomainKeys che DKIM per lo stesso dominio in quanto DKIM è una versione migliorata di DomainKeys.
* I seguenti domini attualmente convalidano DKIM: AOL, Gmail.

## Ciclo di feedback {#feedback-loop-acc}

Un ciclo di feedback funziona dichiarando a livello di ISP un determinato indirizzo e-mail per un intervallo di indirizzi IP utilizzati per l’invio dei messaggi. L’ISP invierà a questa casella di posta, in modo simile a quanto avviene per i messaggi non recapitati, i messaggi segnalati dai destinatari come spam. La piattaforma deve essere configurata per bloccare le consegne future agli utenti che hanno sporto reclamo. È importante non contattarli più anche se non hanno utilizzato il collegamento di rinuncia appropriato. È sulla base di questi reclami che un ISP aggiungerà un indirizzo IP al suo inserisco nell&#39;elenco Bloccati di. A seconda dell’ISP, un tasso di reclami dell’1% circa determinerà il blocco di un indirizzo IP.

Attualmente è in fase di elaborazione uno standard per definire il formato dei messaggi del ciclo di feedback: [Formato di segnalazione dei feedback sugli abusi (ARF)](https://tools.ietf.org/html/rfc6650).

L’implementazione di un ciclo di feedback per un’istanza richiede:

* Una cassetta postale dedicata all’istanza, che può essere la cassetta postale di mancato recapito
* Indirizzi di invio IP dedicati all’istanza

L’implementazione di un semplice ciclo di feedback in Adobe Campaign utilizza la funzionalità per messaggi non recapitati. La cassetta postale del ciclo di feedback viene utilizzata come cassetta postale di mancato recapito e viene definita una regola per rilevare questi messaggi. Gli indirizzi e-mail dei destinatari che hanno segnalato il messaggio come spam verranno aggiunti all’elenco di quarantena.

* Creare o modificare una regola di mail non recapitate, **Feedback_loop**, in **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Mail rule sets]** con il motivo **Rifiutato** e il tipo **Rigido**.
* Se una cassetta postale è stata definita appositamente per il ciclo di feedback, definisci i parametri per accedervi creando un nuovo account e-mail non recapitate esterno in **[!UICONTROL Administration > Platform > External accounts]**.

Il meccanismo è immediatamente operativo per elaborare le notifiche di reclamo. Per assicurarti che questa regola funzioni correttamente, puoi disattivare temporaneamente gli account in modo che non raccolgano questi messaggi, quindi controllare manualmente il contenuto della cassetta postale del ciclo di feedback. Sul server, eseguire i seguenti comandi:

```
nlserver stop inMail@instance,
nlserver inMail -instance:instance -verbose.
```

Se si è costretti a utilizzare un unico indirizzo di feedback per più istanze, è necessario:

* Replica i messaggi ricevuti su tutte le cassette postali esistenti.
* Ciascuna cassetta postale deve essere selezionata da una singola istanza,
* Configura le istanze in modo che elaborino solo i messaggi che le riguardano: le informazioni sull’istanza sono incluse nell’intestazione Message-ID dei messaggi inviati da Adobe Campaign e si trovano quindi anche nei messaggi del ciclo di feedback. È sufficiente specificare **checkInstanceName** parametro nel file di configurazione dell’istanza (per impostazione predefinita, l’istanza non viene verificata e questo potrebbe causare una quarantena errata per alcuni indirizzi):

  ```
  <serverConf>
    <inMail checkInstanceName="true"/>
  </serverConf>
  ```

Il servizio di recapito messaggi di Adobe Campaign gestisce l’abbonamento ai servizi del ciclo di feedback per i seguenti ISP: AOL, BlueTime, Comcast, Cox, EarthLink, FastMail, Gmail, Hotmail, HostedEmail, Libero, Mail.ru, MailTrust, OpenSRS, QQ, RoadRunner, Synacor, Telenor, Terra, UnitedOnline, USA, XS4ALL, Yahoo, Yandex, Zoho.

## Annullamento iscrizione mailing list {#list-unsubscribe}

### Informazioni sull’annullamento dell’iscrizione a un elenco {#about-list-unsubscribe}

Aggiunta di un’intestazione SMTP denominata **Annullamento iscrizione mailing list** è obbligatorio per garantire una gestione ottimale del recapito messaggi.A partire dal 1° giugno 2024, Yahoo e Gmail richiederanno ai mittenti di conformarsi all’annullamento dell’iscrizione all’elenco con un clic. Per informazioni su come configurare l’annullamento dell’abbonamento a un clic con l’elenco, consulta di seguito.


Questa intestazione può essere utilizzata in alternativa all’icona &quot;Segnala come SPAM&quot;. Viene visualizzato come collegamento per annullare l’abbonamento nell’interfaccia e-mail.

L’utilizzo di questa funzionalità aiuta a proteggere la tua reputazione e il feedback verrà eseguito come un annullamento dell’abbonamento.

Per utilizzare Annulla sottoscrizione elenco, è necessario immettere una riga di comando simile alla seguente:

```
List-Unsubscribe: <mailto: client@newsletter.example.com?subject=unsubscribe?body=unsubscribe>
```

>[!CAUTION]
>
>L’esempio precedente si basa sulla tabella dei destinatari. Se l&#39;implementazione del database viene eseguita da un&#39;altra tabella, assicurarsi di ridigitare le informazioni corrette nella riga di comando.

Per creare una dinamica è possibile utilizzare la riga di comando seguente **Annullamento iscrizione mailing list**:

```
List-Unsubscribe: <mailto: %=errorAddress%?subject=unsubscribe%=message.mimeMessageId%>
```

Gmail, Outlook.com e Microsoft Outlook supportano questo metodo e un pulsante per annullare l’abbonamento è disponibile direttamente nell’interfaccia. Questa tecnica riduce la percentuale di reclami.

Puoi implementare **Annullamento iscrizione mailing list** mediante:

* Direttamente [aggiunta della riga di comando nel modello di consegna](#adding-a-command-line-in-a-delivery-template)
* [Creazione di una regola di tipologia](#creating-a-typology-rule)

### Aggiunta di una riga di comando in un modello di consegna {#adding-a-command-line-in-a-delivery-template}

La riga di comando deve essere aggiunta nella sezione aggiuntiva dell’intestazione SMTP dell’e-mail.

Questa aggiunta può essere eseguita in ogni e-mail o nei modelli di consegna esistenti. Puoi anche creare un nuovo modello di consegna che include questa funzionalità.

* Annullamento iscrizione a mailing list: <mailto:unsubscribe@domain.com>
Facendo clic sul collegamento per annullare l’abbonamento si apre il client e-mail predefinito dell’utente. Questa regola di tipologia deve essere aggiunta in una tipologia utilizzata per la creazione di e-mail.

* Annullamento iscrizione a mailing list: <https://domain.com/unsubscribe.jsp>
Facendo clic sul collegamento per annullare l’abbonamento, l’utente viene reindirizzato al modulo per annullare l’abbonamento.
  ![immagine](https://git.corp.adobe.com/storage/user/38257/files/3b46450f-2502-48ed-87b9-f537e1850963)


### Creazione di una regola di tipologia {#creating-a-typology-rule}

La regola deve contenere lo script che genera la riga di comando e deve essere inclusa nell’intestazione e-mail.

>[!NOTE]
>
>È consigliabile creare una regola di tipologia: la funzionalità Annulla iscrizione a mailing list verrà aggiunta automaticamente in ogni e-mail.

>[!NOTE]
>
>Scopri come creare regole di tipologia in Adobe Campaign Classic in [questa sezione](https://experienceleague.adobe.com/docs/campaign-classic/using/orchestrating-campaigns/campaign-optimization/about-campaign-typologies.html#typology-rules).

### Annullamento iscrizione a elenco con un solo clic

A partire dal 1° giugno 2024, Yahoo e Gmail richiederanno ai mittenti di conformarsi all’annullamento dell’iscrizione all’elenco con un solo clic. Per rispettare il requisito relativo all’annullamento dell’iscrizione all’elenco con un solo clic, i mittenti devono:

* Aggiungi in un &quot;List-Unsubscribe-Post: List-Unsubscribe=One-Click&quot;
* Includi un collegamento per l’annullamento dell’iscrizione URI
* Supporta la ricezione della risposta HTTP POST dal ricevitore, supportata da Adobe Campaign.

Per configurare direttamente l’annullamento dell’iscrizione con un solo clic:

* Aggiungi nella seguente applicazione web &quot;Annulla iscrizione destinatari senza clic&quot; 
* Vai a Risorse -> Online -> Applicazioni Web
* Carica l’XML &quot;Unsubscribe recipients no-click&quot; (Annulla sottoscrizione destinatari senza clic)
* Configurare i post di annullamento iscrizione e annullamento iscrizione a mailing list
* Vai alla sezione SMTP delle proprietà di consegna.
* In Intestazioni SMTP aggiuntive, immetti nelle righe di comando (ogni intestazione deve trovarsi su una riga separata):

```
List-Unsubscribe-Post: List-Unsubscribe=One-Click
List-Unsubscribe: <https//domain.com/webApp/unsubNoClick?id=<%= recipient.cryptidcamp %>>, <mailto: %=errorAddress%?
subject=unsubscribe%=message.mimeMessageId%>
```

L’esempio precedente abiliterà l’annullamento dell’iscrizione all’elenco con un solo clic per gli ISP che supportano questo servizio, garantendo al contempo che i destinatari che non supportano tale annullamento possano comunque richiedere l’annullamento dell’iscrizione tramite e-mail.


### Creazione della regola di tipologia per supportare l’annullamento dell’iscrizione con un solo clic:

* Creare la nuova regola di tipologia
* Dalla struttura di navigazione, fai clic su Nuovo per creare una nuova tipologia
  ![immagine](https://git.corp.adobe.com/AdobeDocs/deliverability-learn.en/blob/main/help/assets/CreatingTypologyRules1.png)
* Procedi con la configurazione della regola di tipologia
* Tipo di regola: controllo
* Canale: e-mail
* Fase : All’inizio della personalizzazione
* Livello: scelta
* Attivo
  ![immagine](https://git.corp.adobe.com/AdobeDocs/deliverability-learn.en/blob/main/help/assets/CreatingTypologyRules2.png)

* Crea un codice JavaScript per la regola di tipologia.

>[!NOTE]
>
>Il codice descritto di seguito deve essere utilizzato solo come esempio.
>

In questo esempio viene descritto come:
* Configura un URL con il comando Annulla sottoscrizione elenco e aggiungi le intestazioni o aggiungi i parametri mailto: esistenti e sostituiscili con: &lt;mailto..>, <http://…>
* Aggiungi nell’intestazione List-Unsubscribe-Post

L’esempio di URL post utilizza var headerUnsubUrl = &quot;http;//campmomentumv7-mkt-prod3.campaign.adobe.com/webApp/unsubNoClick?id=&lt;%= recipient.cryptedId %>&quot;;

È possibile aggiungere altri parametri (come &amp;service = ...)

```
// Function to add or replace a header in the provided headers 
function addHeader(headers, header, value)  { 
    
  // Create the new header line 
  var headerLine = header + ": " + value; 
    
  // Create a regular expression to find the specified header 
  var regExp = new RegExp(header + ":(.*)$", "i") 
    
  // Split the headers into individual lines 
  var headerLines = headers.split("\n"); 
    
  // Loop through each line 
  for (var i=0; i < headerLines.length; i++) { 
      
    // Check if the specified header exists 
    var match = headerLines[i].match(regExp) 
      
    // If it exists 
    if ( match != null ) { 
        
      // Replace the existing header line 
      headerLines[i] = headerLine; 
        
      // Return the modified headers 
      return headerLines.join("\n"); 
    } 
  } 
    
  // If the header does not exist, add the new header line 
  headerLines.push(headerLine); 
    
  // Return the modified headers 
  return headerLines.join("\n"); 
} 
  
// Function to get the value of a specified header from the provided headers 
function getHeader(headers, header) { 
    
  // Create a regular expression to find the specified header 
  var regExp = new RegExp(header + ":(.*)$", "i") 
    
  // Split the headers into individual lines 
  var headerLines = headers.split("\n"); 
    
  // Loop each line 
  for each (line in headerLines) { 
      
    // Check if the specified header exists 
    var match = line.match(regExp); 
      
    // If it exists 
    if ( match != null ) { 
        
      // Return the header value, removing leading whitespace 
      return match[1].replace(/^\s*/, ""); 
    } 
  } 
    
  // If the header does not exist, return an empty string 
  return ""; 
} 
  
  
// Define the unsubscribe URL 
var headerUnsubUrl = "http://campmomentumv7-mkt-prod3.campaign.adobe.com/webApp/unsubNoClick?id=<%= recipient.cryptedId %>"; 
  
// Get the value of the List-Unsubscribe header 
var headerUnsub = getHeader(delivery.mailParameters.headers, "List-Unsubscribe"); 
  
// If the List-Unsubscribe header does not exist 
if ( headerUnsub === "" ) { 
  // Add the List-Unsubscribe header 
  delivery.mailParameters.headers = addHeader(delivery.mailParameters.headers, "List-Unsubscribe", "<"+headerUnsubUrl+">"); 
} 
// If the List-Unsubscribe header exists and contains 'mailto' 
else if(headerUnsub.search('mailto')){ 
  // Replace the existing List-Unsubscribe header 
  delivery.mailParameters.headers = addHeader(delivery.mailParameters.headers, "List-Unsubscribe", "<"+headerUnsubUrl+">"); 
} 
  
// Get the value of the List-Unsubscribe-Post header 
var headerUnsubPost = getHeader(delivery.mailParameters.headers, "List-Unsubscribe-Post"); 
  
// If the List-Unsubscribe-Post header does not exist 
if ( headerUnsubPost === "" ) { 
  // Add the List-Unsubscribe-Post header 
  delivery.mailParameters.headers = addHeader(delivery.mailParameters.headers, "List-Unsubscribe-Post", "List-Unsubscribe=One-Click"); 
} 
  
// Return true to indicate success 
return true; 
```
![immagine](https://git.corp.adobe.com/AdobeDocs/deliverability-learn.en/blob/main/help/assets/CreatingTypologyRules3.png)

* Aggiungi la nuova regola a una tipologia in un messaggio e-mail (la tipologia predefinita è ok).
  ![immagine](https://git.corp.adobe.com/AdobeDocs/deliverability-learn.en/blob/main/help/assets/CreatingTypologyRules4.png)

* Prepara una nuova consegna (verifica che le intestazioni SMTP aggiuntive nella proprietà di consegna siano vuote).
  ![immagine](https://git.corp.adobe.com/AdobeDocs/deliverability-learn.en/blob/main/help/assets/CreatingTypologyRules5.png)

* Verifica durante la preparazione della consegna che la nuova Regola di tipologia sia applicata.
  ![immagine](https://git.corp.adobe.com/AdobeDocs/deliverability-learn.en/blob/main/help/assets/CreatingTypologyRules6.png)

* Verifica che sia presente l’opzione Annulla sottoscrizione elenco
  ![immagine](https://git.corp.adobe.com/AdobeDocs/deliverability-learn.en/blob/main/help/assets/CreatingTypologyRules6.png)




## Ottimizzazione delle e-mail {#email-optimization}

### SMTP {#smtp}

SMTP (Simple Mail Transfer Protocol) è uno standard Internet per la trasmissione di e-mail.

Gli errori SMTP che non sono controllati da una regola sono elencati nella **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Delivery log qualification]** cartella. Per impostazione predefinita, questi messaggi di errore vengono interpretati come errori soft non raggiungibili.

È necessario identificare gli errori più comuni e aggiungere una regola corrispondente in **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Mail rule sets]** se desideri qualificare correttamente il feedback dai server SMTP. In caso contrario, la piattaforma eseguirà nuovi tentativi non necessari (nel caso di utenti sconosciuti) o metterà erroneamente in quarantena alcuni destinatari dopo un determinato numero di test.

### IP dedicati {#dedicated-ips}

Adobe fornisce una strategia IP dedicata per ogni cliente con un IP incrementale al fine di creare una reputazione e ottimizzare le prestazioni di consegna.
