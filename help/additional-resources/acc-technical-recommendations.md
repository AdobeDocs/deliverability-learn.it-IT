---
title: 'Campaign Classic: consigli tecnici'
description: Scopri le tecniche, le configurazioni e gli strumenti che puoi utilizzare per migliorare il tasso di consegna con Adobe Campaign Classic.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 39ed3773-18bf-4653-93b6-ffc64546406b
source-git-commit: b163628adde1e4d7225a1c2c54d29b24e2b2a352
workflow-type: tm+mt
source-wordcount: '2064'
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
>Puoi usare [questo strumento esterno](https://mxtoolbox.com/SuperTool.aspx) per verificare la configurazione di un dominio.

### Regole MX {#mx-rules}

Le regole MX (Mail eXchanger) sono le regole che gestiscono la comunicazione tra un server di invio e un server ricevente.

Più precisamente, vengono utilizzati per controllare la velocità con cui l’MTA di Adobe Campaign (Message Transfer Agent) invia e-mail a ogni singolo dominio e-mail o ISP (ad esempio, hotmail.com, comcast.net). Queste regole si basano in genere sui limiti pubblicati dagli ISP (ad esempio, non includono più di 20 messaggi per ogni connessione SMTP).

>[!NOTE]
>
>Per ulteriori informazioni sulla gestione MX in Adobe Campaign Classic, consulta [questa sezione](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/additional-configurations/email-deliverability.html?lang=it#mx-configuration).

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

definisce i due indirizzi IP, 12.34.56.78 e 12.34.56.79, come autorizzati a inviare e-mail per il dominio. **~all** significa che qualsiasi altro indirizzo deve essere interpretato come SoftFail.

Recommendations per definire un record SPF:

* Aggiungere **~all** (SoftFail) o **-all** (Fail) alla fine per rifiutare tutti i server diversi da quelli definiti. In caso contrario, i server saranno in grado di forgiare questo dominio (con una valutazione neutra).
* Non aggiungere **ptr** (openspf.org consiglia di non aggiungerlo perché è costoso e inaffidabile).

>[!NOTE]
>
>Ulteriori informazioni su SPF in [questa sezione](/help/additional-resources/authentication.md#spf).

## Autenticazione

>[!NOTE]
>
>Ulteriori informazioni sulle diverse forme di autenticazione delle e-mail in [questa sezione](/help/additional-resources/authentication.md).

### DKIM {#dkim-acc}

>[!NOTE]
>
>Per le installazioni in hosting o ibride, se hai eseguito l&#39;aggiornamento a [MTA avanzato](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/sending-emails/sending-an-email/sending-with-enhanced-mta.html?lang=it#sending-messages), la firma di autenticazione e-mail DKIM viene eseguita dall&#39;MTA avanzato per tutti i messaggi di tutti i domini.

L&#39;utilizzo di [DKIM](/help/additional-resources/authentication.md#dkim) con Adobe Campaign Classic richiede il seguente prerequisito:

**Dichiarazione opzione Adobe Campaign**: in Adobe Campaign, la chiave privata DKIM si basa su un selettore DKIM e un dominio. Attualmente non è possibile creare più chiavi private per lo stesso dominio/sottodominio con selettori diversi. Non è possibile definire quale dominio/sottodominio selettore deve essere utilizzato per l’autenticazione né nella piattaforma né nell’e-mail. In alternativa, la piattaforma selezionerà una delle chiavi private, il che significa che l’autenticazione ha un’alta probabilità di non riuscire.

* Se hai configurato DomainKeys per l&#39;istanza di Adobe Campaign, devi solo selezionare **dkim** nelle [Regole di gestione del dominio](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html?lang=it#email-management-rules). In caso contrario, segui gli stessi passaggi di configurazione (chiave privata/pubblica) di DomainKeys (che ha sostituito DKIM).
* Non è necessario abilitare sia DomainKeys che DKIM per lo stesso dominio in quanto DKIM è una versione migliorata di DomainKeys.
* I seguenti domini attualmente convalidano DKIM: AOL, Gmail.

## Ciclo di feedback {#feedback-loop-acc}

Un ciclo di feedback funziona dichiarando a livello di ISP un determinato indirizzo e-mail per un intervallo di indirizzi IP utilizzati per l’invio dei messaggi. L’ISP invierà a questa casella di posta, in modo simile a quanto avviene per i messaggi non recapitati, i messaggi segnalati dai destinatari come spam. La piattaforma deve essere configurata per bloccare le consegne future agli utenti che hanno sporto reclamo. È importante non contattarli più anche se non hanno utilizzato il collegamento di rinuncia appropriato. È sulla base di questi reclami che un ISP aggiungerà un indirizzo IP al suo inserisco nell&#39;elenco Bloccati di. A seconda dell’ISP, un tasso di reclami dell’1% circa determinerà il blocco di un indirizzo IP.

È in corso l&#39;elaborazione di uno standard per definire il formato dei messaggi del ciclo di feedback: [ARF (Abuse Feedback Reporting Format)](https://tools.ietf.org/html/rfc6650).

L’implementazione di un ciclo di feedback per un’istanza richiede:

* Una cassetta postale dedicata all’istanza, che può essere la cassetta postale di mancato recapito
* Indirizzi di invio IP dedicati all’istanza

L’implementazione di un semplice ciclo di feedback in Adobe Campaign utilizza la funzionalità per messaggi non recapitati. La cassetta postale del ciclo di feedback viene utilizzata come cassetta postale di mancato recapito e viene definita una regola per rilevare questi messaggi. Gli indirizzi e-mail dei destinatari che hanno segnalato il messaggio come spam verranno aggiunti all’elenco di quarantena.

* Crea o modifica una regola di posta non recapitata, **Feedback_loop**, in **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Mail rule sets]** con il motivo **Rifiutato** e il tipo **Rigido**.
* Se è stata definita una cassetta postale specifica per il ciclo di feedback, definire i parametri per accedervi creando un nuovo account di posta non recapitata esterno in **[!UICONTROL Administration > Platform > External accounts]**.

Il meccanismo è immediatamente operativo per elaborare le notifiche di reclamo. Per assicurarti che questa regola funzioni correttamente, puoi disattivare temporaneamente gli account in modo che non raccolgano questi messaggi, quindi controllare manualmente il contenuto della cassetta postale del ciclo di feedback. Sul server, eseguire i seguenti comandi:

```
nlserver stop inMail@instance,
nlserver inMail -instance:instance -verbose.
```

Se si è costretti a utilizzare un unico indirizzo di feedback per più istanze, è necessario:

* Replica i messaggi ricevuti su tutte le cassette postali esistenti.
* Ciascuna cassetta postale deve essere selezionata da una singola istanza,
* Configura le istanze in modo che elaborino solo i messaggi che le riguardano: le informazioni sull’istanza sono incluse nell’intestazione Message-ID dei messaggi inviati da Adobe Campaign e si trovano quindi anche nei messaggi del ciclo di feedback. È sufficiente specificare il parametro **checkInstanceName** nel file di configurazione dell&#39;istanza (per impostazione predefinita, l&#39;istanza non viene verificata e questo può causare la messa in quarantena errata di un determinato indirizzo):

  ```
  <serverConf>
    <inMail checkInstanceName="true"/>
  </serverConf>
  ```

Il servizio di recapito messaggi di Adobe Campaign gestisce l’abbonamento ai servizi del ciclo di feedback per i seguenti ISP: AOL, BlueTime, Comcast, Cox, EarthLink, FastMail, Gmail, Hotmail, HostedEmail, Libero, Mail.ru, MailTrust, OpenSRS, QQ, RoadRunner, Synacor, Telenor, Terra, UnitedOnline, USA, XS4ALL, Yahoo, Yandex, Zoho.

## Annullamento iscrizione mailing list {#list-unsubscribe}

Per garantire una gestione ottimale del recapito messaggi, è necessario aggiungere un&#39;intestazione SMTP denominata **Annulla sottoscrizione elenco**.

Questa intestazione può essere utilizzata in alternativa all’icona &quot;Segnala come SPAM&quot;. Viene visualizzato come collegamento per annullare l’abbonamento nelle interfacce e-mail degli ISP.

L’utilizzo di questa funzionalità riduce la percentuale di reclami e aiuta a proteggere la reputazione. Il feedback verrà eseguito come annullamento dell’abbonamento.

Gmail, Outlook.com, Yahoo! e Microsoft Outlook supportano questo metodo. Nell’interfaccia è disponibile un collegamento che consente di annullare l’abbonamento. Ad esempio:

![immagine](../assets/List-Unsubscribe-example-Gmail.png)

>[!NOTE]
>
>Il collegamento &quot;Annulla iscrizione&quot; potrebbe non essere sempre visualizzato. In effetti, può dipendere dai criteri e dalle politiche specifici di ciascun ISP. Assicurati pertanto che i messaggi siano inviati da un mittente:
>
>* Di buona reputazione
>* Al di sotto della soglia di reclamo spam degli ISP
>* Completamente autenticato

Esistono due versioni della funzionalità di intestazione Annulla sottoscrizione elenco:

* **&quot;mailto&quot; - Annullamento iscrizione a mailing list** - Con questo metodo, facendo clic sul collegamento **Annulla iscrizione**, viene inviato un messaggio e-mail precompilato all&#39;indirizzo di annullamento dell&#39;iscrizione specificato nell&#39;intestazione dell&#39;e-mail. [Ulteriori informazioni](#mailto-list-unsubscribe)

* **&quot;One-Click&quot; List-Unsubscribe** - Con questo metodo, facendo clic sul collegamento **Unsubscribe**, l&#39;utente annulla direttamente l&#39;abbonamento. [Ulteriori informazioni](#one-click-list-unsubscribe)

>[!NOTE]
>
>A partire dal 1° giugno 2024, gli ISP principali richiederanno ai mittenti di conformarsi a **Unsubscribe-List-Unsubscribe con un clic**.

### Annullamento iscrizione mailing-to {#mailto-list-unsubscribe}

Con questo metodo, facendo clic sul collegamento **Annulla iscrizione**, viene inviata un&#39;e-mail precompilata all&#39;indirizzo di annullamento dell&#39;iscrizione specificato nell&#39;intestazione dell&#39;e-mail.

Per utilizzare &quot;mailto&quot; - Annulla iscrizione a mailing list, è necessario immettere una riga di comando in cui specificare un indirizzo di posta elettronica, ad esempio: `List-Unsubscribe: <mailto:client@newsletter.example.com?subject=unsubscribe?body=unsubscribe>`

>[!CAUTION]
>
>L’esempio precedente si basa sulla tabella dei destinatari. Se l&#39;implementazione del database viene eseguita da un&#39;altra tabella, assicurarsi di ridigitare le informazioni corrette nella riga di comando.

È inoltre possibile creare una sottoscrizione dinamica di tipo &quot;mailto&quot; List-Unsubscribe utilizzando una riga di comando quale: `List-Unsubscribe: <mailto:<%=errorAddress%>?subject=unsubscribe%=message.mimeMessageId%>`

Per implementare **&quot;mailto&quot; List-Unsubscribe** in Campaign, puoi:

* Aggiungi direttamente la riga di comando nel modello di consegna o consegna - [Scopri come](#adding-a-command-line-in-a-delivery-template)

* Crea una regola di tipologia - [Scopri come](#creating-a-typology-rule)

#### Aggiunta di una riga di comando in una consegna o in un modello {#adding-a-command-line-in-a-delivery-template}

La riga di comando deve essere aggiunta alla sezione **[!UICONTROL Additional SMTP headers]** dell&#39;intestazione SMTP dell&#39;e-mail.

Questa aggiunta può essere eseguita in ogni e-mail o nei modelli di consegna esistenti. Puoi anche creare un nuovo modello di consegna che include questa funzionalità.

Immettere ad esempio il seguente script nel campo **[!UICONTROL Additional SMTP headers]**: `List-Unsubscribe: mailto:unsubscribe@domain.com`. Facendo clic sul collegamento **annulla iscrizione**, viene inviata un&#39;e-mail all&#39;indirizzo unsubscribe@domain.com.

È inoltre possibile utilizzare un indirizzo dinamico. Ad esempio, per inviare un messaggio e-mail all&#39;indirizzo di errore definito per la piattaforma, è possibile utilizzare lo script seguente: `List-Unsubscribe: <mailto:<%=errorAddress%>?subject=unsubscribe%=message.mimeMessageId%>`

![immagine](../assets/List-Unsubscribe-template-SMTP.png)

#### Creazione di una regola di tipologia {#creating-a-typology-rule}

La regola deve contenere lo script che genera la riga di comando e deve essere inclusa nell’intestazione e-mail.

Scopri come creare regole di tipologia in Adobe Campaign v7/v8 in [questa sezione](https://experienceleague.adobe.com/docs/campaign-classic/using/orchestrating-campaigns/campaign-optimization/about-campaign-typologies.html?lang=it#typology-rules).

>[!NOTE]
>
>È consigliabile creare una regola di tipologia: la funzionalità Annulla iscrizione a mailing list verrà aggiunta automaticamente in ogni e-mail utilizzando questa regola di tipologia.

### Annulla iscrizione mailing list con un solo clic {#one-click-list-unsubscribe}

Con questo metodo, facendo clic sul collegamento **Annulla sottoscrizione** l&#39;utente annulla direttamente l&#39;abbonamento e richiede una sola azione per annullare l&#39;abbonamento.

A partire dal 1° giugno 2024, gli ISP principali richiederanno ai mittenti di conformarsi a **Unsubscribe-List-Unsubscribe con un clic**.

Per soddisfare tale requisito, i mittenti devono:

* Aggiungere la seguente riga di comando: `List-Unsubscribe-Post: List-Unsubscribe=One-Click`.
* Includi un collegamento per annullare l’iscrizione URI.
* Supporta la ricezione della risposta HTTP POST dal ricevitore, supportata da Adobe Campaign. Puoi anche utilizzare un servizio esterno.

Per supportare la risposta One-Click List-Unsubscribe POST direttamente in Adobe Campaign v7/v8, devi aggiungere nell’applicazione web &quot;Unsubscribe recipients no-click&quot; (Annulla l’abbonamento dei destinatari senza clic). Per eseguire questa operazione:

1. Vai a **[!UICONTROL Resources]** > **[!UICONTROL Online]** > **[!UICONTROL Web applications]**.

1. Carica il file [XML](/help/assets/WebAppUnsubNoClick.xml.zip) per annullare l&#39;iscrizione dei destinatari senza fare clic.

Per configurare **l&#39;annullamento dell&#39;iscrizione a un solo clic** in Campaign, puoi effettuare le seguenti operazioni:

* Aggiungi la riga di comando nel modello di consegna - [Scopri come](#one-click-delivery-template)
* Crea una regola di tipologia - [Scopri come](#one-click-typology-rule)

#### Configurazione dell’annullamento dell’iscrizione all’elenco con un solo clic nella consegna o nel modello {#one-click-delivery-template}

Per configurare l’annullamento dell’iscrizione a un elenco con un solo clic nel modello di consegna o consegna, segui i passaggi indicati di seguito.

1. Vai alla sezione **[!UICONTROL SMTP]** delle proprietà di consegna.

1. In **[!UICONTROL Additional SMTP Headers]** immettere le righe di comando come nell&#39;esempio seguente. Ogni intestazione deve essere su una riga separata.

Ad esempio:

```
List-Unsubscribe-Post: List-Unsubscribe=One-Click
List-Unsubscribe: <https://domain.com/webApp/unsubNoClick?id=<%= recipient.cryptedId %> >, < mailto:<%@ include option='NmsEmail_DefaultErrorAddr' %>?subject=unsubscribe<%=escape(message.mimeMessageId) %> >
```

![immagine](../assets/List-Unsubscribe-1-click-template-SMTP.png)

L’esempio precedente abiliterà l’annullamento dell’iscrizione all’elenco con un solo clic per gli ISP che supportano One-Click, garantendo al contempo che i destinatari che non supportano &quot;mailto&quot; possano comunque richiedere l’annullamento dell’iscrizione tramite e-mail.

#### Creazione di una regola di tipologia per supportare l’annullamento dell’abbonamento a un clic {#one-click-typology-rule}

Per configurare l’annullamento dell’iscrizione a un elenco con un solo clic utilizzando una regola di tipologia, effettua le seguenti operazioni.

1. Dalla struttura di navigazione, passare a **[!UICONTROL Typolgy rules]** e fare clic su **[!UICONTROL New]**.

   ![immagine](../assets/CreatingTypologyRules1.png)


1. Configura la nuova regola di tipologia, ad esempio:

   * **[!UICONTROL Rule type]**: **[!UICONTROL Control]**
   * **[!UICONTROL Phase]**: **[!UICONTROL At the start of targeting]**
   * **[!UICONTROL Channel]**: **[!UICONTROL Email]**
   * **[!UICONTROL Level]**: scelta
   * **[!UICONTROL Active]**


   ![immagine](../assets/CreatingTypologyRules2.png)

1. Crea un codice JavaScript per la regola di tipologia, come nell’esempio seguente.

   >[!NOTE]
   >
   >Il codice descritto di seguito deve essere utilizzato solo come esempio.

   In questo esempio viene descritto come:
   * Configurare un elenco &quot;mailto&quot; per annullare l’iscrizione. Aggiunge le intestazioni o aggiunge i parametri &quot;mailto:&quot; esistenti e li sostituisce con: &lt;mailto.https://...
   * Aggiungi nell’intestazione One-Click List-Unsubscribe. Utilizza `var headerUnsubUrl = "https://campmomentumv7-mkt-prod3.campaign.adobe.com/webApp/unsubNoClick?id=<%= recipient.cryptedId %>"÷`

   >[!NOTE]
   >
   >È possibile aggiungere altri parametri (ad esempio &amp;service =...).

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
   var headerUnsubUrl = "https://campmomentumv7-mkt-prod3.campaign.adobe.com/webApp/unsubNoClick?id=<%= recipient.cryptedId %>"; 
     
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


   ![immagine](../assets/CreatingTypologyRules3.png)

1. Aggiungi la nuova regola a una tipologia applicabile alle e-mail.

   >[!NOTE]
   >
   >Puoi aggiungerla alla tipologia predefinita.

   ![immagine](../assets/CreatingTypologyRules4.png)

1. Prepara una nuova consegna.

   >[!CAUTION]
   >
   >Verifica che il campo **[!UICONTROL Additional SMTP headers]** nelle proprietà di consegna sia vuoto.

   ![immagine](../assets/CreatingTypologyRules5.png)

1. Controlla durante la preparazione della consegna che la nuova regola di tipologia sia applicata.

   ![immagine](../assets/CreatingTypologyRules6.png)

1. Verifica che sia presente il collegamento per annullare l’iscrizione.

   ![immagine](../assets/CreatingTypologyRules7.png)

## Ottimizzazione delle e-mail {#email-optimization}

### SMTP {#smtp}

SMTP (Simple Mail Transfer Protocol) è uno standard Internet per la trasmissione di e-mail.

Gli errori SMTP non controllati da una regola sono elencati nella cartella **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Delivery log qualification]**. Per impostazione predefinita, questi messaggi di errore vengono interpretati come errori soft non raggiungibili.

Per qualificare correttamente il feedback dai server SMTP, è necessario identificare gli errori più comuni e aggiungere una regola corrispondente in **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Mail rule sets]**. In caso contrario, la piattaforma eseguirà nuovi tentativi non necessari (nel caso di utenti sconosciuti) o metterà erroneamente in quarantena alcuni destinatari dopo un determinato numero di test.

### IP dedicati {#dedicated-ips}

Adobe fornisce una strategia IP dedicata per ogni cliente con un IP incrementale al fine di creare una reputazione e ottimizzare le prestazioni di consegna.
