---
title: 'Campaign Classic: consigli tecnici'
description: Scopri tecniche, configurazioni e strumenti che puoi utilizzare per migliorare il tasso di recapito messaggi con Adobe Campaign Classic.
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
exl-id: 39ed3773-18bf-4653-93b6-ffc64546406b
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '1575'
ht-degree: 1%

---

# Campaign Classic: consigli tecnici {#technical-recommendations}

Di seguito sono elencate diverse tecniche, configurazioni e strumenti che è possibile utilizzare per migliorare il tasso di consegna quando si utilizza Adobe Campaign Classic.

## Configurazione {#configuration}

### DNS inversi {#reverse-dns}

Adobe Campaign controlla se viene fornito un DNS inverso per un indirizzo IP e che questo punti correttamente all’IP.

Un punto importante nella configurazione della rete è quello di assicurarsi che sia definito un DNS inverso corretto per ciascuno degli indirizzi IP per i messaggi in uscita. Ciò significa che per un dato indirizzo IP, esiste un record DNS inverso (record PTR) con un record DNS corrispondente (record A) che torna all’indirizzo IP iniziale.

La scelta del dominio per un DNS inverso ha un impatto quando si tratta di alcuni ISP. AOL, in particolare, accetta solo cicli di feedback con un indirizzo nello stesso dominio del DNS inverso (vedere [Feedback loop](#feedback-loop)).

>[!NOTE]
>
>Puoi utilizzare [questo strumento esterno](https://mxtoolbox.com/SuperTool.aspx) per verificare la configurazione di un dominio.

### Regole MX {#mx-rules}

Le regole MX (Mail eXchanger) sono regole che gestiscono la comunicazione tra un server di invio e un server di ricezione.

Più precisamente, vengono utilizzati per controllare la velocità alla quale l’MTA di Adobe Campaign (Message Transfer Agent) invia e-mail a ogni singolo dominio e-mail o ISP (ad esempio, hotmail.com, comcast.net). Queste regole si basano in genere sui limiti pubblicati dagli ISP (ad esempio, non includono più di 20 messaggi per ogni connessione SMTP).

>[!NOTE]
>
>Per ulteriori informazioni sulla gestione MX in Adobe Campaign Classic, consulta [questa sezione](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/additional-configurations/email-deliverability.html#mx-configuration).

### TLS {#tls}

TLS (Transport Layer Security) è un protocollo di cifratura che può essere utilizzato per proteggere la connessione tra due server e-mail e proteggere il contenuto di un’e-mail da essere letto da qualsiasi utente diverso dai destinatari desiderati.

### Dominio del mittente {#sender-domain}

Per definire il dominio utilizzato per il comando HELO, modifica il file di configurazione dell’istanza (conf/config-instance.xml) e definisci un attributo &quot;localDomain&quot; come segue:

```
<serverConf>
  <shared>
    <dnsConfig localDomain="mydomain.net"/>
  </shared>
</serverConf>
```

Il dominio MAIL FROM è il dominio utilizzato nei messaggi non recapitati tecnici. Questo indirizzo viene definito nella procedura guidata di distribuzione o tramite l’opzione NmsEmail_DefaultErrorAddr.

### Record SPF {#dns-configuration}

Un record SPF può attualmente essere definito su un server DNS come record di tipo TXT (codice 16) o come record di tipo SPF (codice 99). Un record SPF assume la forma di una stringa di caratteri. Esempio:

```
v=spf1 ip4:12.34.56.78/32 ip4:12.34.56.79/32 ~all
```

definisce i due indirizzi IP 12.34.56.78 e 12.34.56.79 come autorizzati a inviare e-mail per il dominio. **~** significa anche che qualsiasi altro indirizzo deve essere interpretato come un SoftFail.

Recommendations per la definizione di un record SPF:

* Aggiungi **~all** (SoftFail) o **-all** (Fail) alla fine per rifiutare tutti i server diversi da quelli definiti. In caso contrario, i server saranno in grado di creare questo dominio (con una valutazione neutra).
* Non aggiungere **ptr** (openspf.org consiglia di non considerarlo costoso e inaffidabile).

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
>Per le installazioni in hosting o ibride, se hai effettuato l’aggiornamento all’ [MTA avanzato](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/sending-emails/sending-an-email/sending-with-enhanced-mta.html#sending-messages), la firma di autenticazione dell’e-mail DKIM viene eseguita dall’MTA avanzato per tutti i messaggi con tutti i domini.

L&#39;utilizzo di [DKIM](/help/additional-resources/authentication.md#dkim) con Adobe Campaign Classic richiede il seguente prerequisito:

**Dichiarazione** dell’opzione Adobe Campaign: in Adobe Campaign, la chiave privata DKIM è basata su un selettore DKIM e su un dominio. Al momento non è possibile creare più chiavi private per lo stesso dominio/sottodominio con diversi selettori. Non è possibile definire quale dominio/sottodominio del selettore deve essere utilizzato per l’autenticazione né nella piattaforma né nell’e-mail. In alternativa, la piattaforma selezionerà una delle chiavi private, il che significa che l’autenticazione ha un’alta probabilità di errore.

* Se hai configurato DomainKeys per la tua istanza Adobe Campaign, devi solo selezionare **dkim** nelle [regole di gestione del dominio](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html#email-management-rules). In caso contrario, segui gli stessi passaggi di configurazione (chiave privata/pubblica) di DomainKeys (che ha sostituito DKIM).
* Non è necessario abilitare sia DomainKeys che DKIM per lo stesso dominio di DKIM è una versione migliorata di DomainKeys.
* I seguenti domini attualmente convalidano DKIM: AOL, Gmail.

## Ciclo di feedback {#feedback-loop-acc}

Un ciclo di feedback funziona dichiarando a livello di ISP un determinato indirizzo e-mail per una serie di indirizzi IP utilizzati per l’invio di messaggi. L&#39;ISP invierà a questa cassetta postale, in modo simile a quello che viene fatto per i messaggi non recapitati, quei messaggi che i destinatari segnalano come spam. La piattaforma deve essere configurata per bloccare le consegne future agli utenti che hanno presentato reclamo. È importante non contattarli più anche se non hanno utilizzato il collegamento di rinuncia appropriato. Si basa su queste lamentele che un ISP aggiungerà un indirizzo IP al suo elenco Bloccati. A seconda dell&#39;ISP, un tasso di reclamo di circa l&#39;1% comporterà il blocco di un indirizzo IP.

È attualmente in fase di elaborazione uno standard per definire il formato dei messaggi del ciclo di feedback: il [formato ARF (Abuse Feedback Reporting Format)](https://tools.ietf.org/html/rfc6650).

L&#39;implementazione di un ciclo di feedback per un&#39;istanza richiede:

* Una cassetta postale dedicata all&#39;istanza, che può essere la cassetta postale di rimbalzo
* Indirizzi IP di invio dedicati all’istanza

L’implementazione di un semplice ciclo di feedback in Adobe Campaign utilizza la funzionalità messaggio non recapitato. La cassetta postale del ciclo di feedback viene utilizzata come cassetta postale non recapitata e viene definita una regola per rilevare questi messaggi. Gli indirizzi e-mail dei destinatari che hanno segnalato il messaggio come spam verranno aggiunti all’elenco di quarantena.

* Crea o modifica una regola relativa alla posta non recapitata, **Feedback_loop**, in **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Mail rule sets]** con il motivo **Rifiutato** e il tipo **Rigido**.
* Se è stata definita una cassetta postale appositamente per il ciclo di feedback, definisci i parametri per accedervi creando un nuovo account Bounce Mails esterno in **[!UICONTROL Administration > Platform > External accounts]**.

Il meccanismo è immediatamente operativo per il trattamento delle notifiche di reclamo. Per assicurarsi che questa regola funzioni correttamente, è possibile disattivare temporaneamente gli account in modo che non raccolgano questi messaggi, quindi controllare manualmente il contenuto della cassetta postale del ciclo di feedback. Sul server, esegui i seguenti comandi:

```
nlserver stop inMail@instance,
nlserver inMail -instance:instance -verbose.
```

Se sei costretto a utilizzare un unico indirizzo del ciclo di feedback per più istanze, devi:

* Replicare i messaggi ricevuti su un numero di cassette postali pari a quello delle istanze,
* far sì che ogni cassetta postale venga prelevata da un&#39;unica istanza,
* Configura le istanze in modo che elaborino solo i messaggi che le riguardano: le informazioni sull’istanza sono incluse nell’intestazione Message-ID dei messaggi inviati da Adobe Campaign e si trovano quindi anche nei messaggi del ciclo di feedback. È sufficiente specificare il parametro **checkInstanceName** nel file di configurazione dell&#39;istanza (per impostazione predefinita, l&#39;istanza non viene verificata e questo può comportare una quarantena errata di un determinato indirizzo):

   ```
   <serverConf>
     <inMail checkInstanceName="true"/>
   </serverConf>
   ```

Il servizio Adobe Campaign Deliverability gestisce l’abbonamento a servizi di loop di feedback per i seguenti ISP: AOL, BlueTie, Comcast, Cox, EarthLink, FastMail, Gmail, Hotmail, HostedEmail, Libero, Mail.ru, MailTrust, OpenSRS, QQ, RoadRunner, Synacor, Telenor, Terra, UnitedOnline, USA, XS4ALL, Yahoo, Yandex, Zoho.

## Annulla sottoscrizione elenco {#list-unsubscribe}

### Informazioni sull’annullamento della sottoscrizione a un elenco {#about-list-unsubscribe}

L’aggiunta di un’intestazione SMTP denominata **List-Unsubscription** è obbligatoria per garantire una gestione ottimale del recapito messaggi.

Questa intestazione può essere utilizzata come alternativa all’icona &quot;Report as SPAM&quot;. Verrà visualizzato come collegamento di annullamento all’abbonamento nell’interfaccia e-mail.

L’utilizzo di questa funzionalità consente di proteggere la reputazione e il feedback verrà eseguito come annullamento dell’abbonamento.

Per utilizzare Annulla sottoscrizione a elenco, è necessario immettere una riga di comando simile alla seguente:

```
List-Unsubscribe: mailto: client@newsletter.example.com?subject=unsubscribe?body=unsubscribe
```

>[!CAUTION]
>
>L’esempio precedente è basato sulla tabella dei destinatari. Se l’implementazione del database viene eseguita da un’altra tabella, assicurati di ripetere la riga di comando con le informazioni corrette.

La seguente riga di comando può essere utilizzata per creare una riga di comando dinamica **List-Unsubscription**:

```
List-Unsubscribe: mailto: %=errorAddress%?subject=unsubscribe%=message.mimeMessageId%
```

Gmail, Outlook.com e Microsoft Outlook supportano questo metodo e un pulsante di annullamento della sottoscrizione è disponibile direttamente nella loro interfaccia. Questa tecnica riduce i tassi di reclamo.

Puoi implementare **List-Unsubscription** eseguendo una delle seguenti operazioni:

* Direttamente [aggiunta della riga di comando nel modello di consegna](#adding-a-command-line-in-a-delivery-template)
* [Creazione di una regola di tipologia](#creating-a-typology-rule)

### Aggiunta di una riga di comando in un modello di consegna {#adding-a-command-line-in-a-delivery-template}

La riga di comando deve essere aggiunta nella sezione aggiuntiva dell’intestazione SMTP dell’e-mail.

Questa aggiunta può essere eseguita in ogni e-mail o nei modelli di consegna esistenti. Puoi anche creare un nuovo modello di consegna che includa questa funzionalità.

### Creazione di una regola di tipologia {#creating-a-typology-rule}

La regola deve contenere lo script che genera la riga di comando e deve essere inclusa nell’intestazione dell’e-mail.

>[!NOTE]
>
>È consigliabile creare una regola di tipologia: la funzionalità Annulla iscrizione verrà aggiunta automaticamente in ogni e-mail.

1. Annulla sottoscrizione elenco: &lt;mailto:unsubscribe@domain.com>

   Facendo clic sul collegamento **Annulla sottoscrizione** si apre il client e-mail predefinito dell&#39;utente. Questa regola di tipologia deve essere aggiunta in una tipologia utilizzata per creare le e-mail.

1. Annulla sottoscrizione elenco: `<https://domain.com/unsubscribe.jsp>`

   Facendo clic sul collegamento **Annulla sottoscrizione** l&#39;utente viene reindirizzato al modulo di annullamento dell&#39;abbonamento.

   Esempio:

   ![](../assets/s_tn_del_unsubscribe_param.png)

>[!NOTE]
>
>Scopri come creare regole di tipologia in Adobe Campaign Classic in [questa sezione](https://experienceleague.adobe.com/docs/campaign-classic/using/orchestrating-campaigns/campaign-optimization/about-campaign-typologies.html#typology-rules).

## Ottimizzazione e-mail {#email-optimization}

### SMTP {#smtp}

SMTP (protocollo di trasferimento semplice della posta) è uno standard Internet per la trasmissione delle e-mail.

Gli errori SMTP non controllati da una regola sono elencati nella cartella **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Delivery log qualification]** . Questi messaggi di errore vengono interpretati per impostazione predefinita come errori soft irraggiungibili.

Gli errori più comuni devono essere identificati e deve essere aggiunta una regola corrispondente in **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Mail rule sets]** se si desidera qualificare correttamente il feedback dai server SMTP. In caso contrario, la piattaforma eseguirà tentativi non necessari (in caso di utenti sconosciuti) o metterà erroneamente in quarantena alcuni destinatari dopo un determinato numero di test.

### IP dedicati {#dedicated-ips}

Adobe fornisce una strategia IP dedicata per ogni cliente con un IP di espansione per costruire una reputazione e ottimizzare le prestazioni di consegna.
