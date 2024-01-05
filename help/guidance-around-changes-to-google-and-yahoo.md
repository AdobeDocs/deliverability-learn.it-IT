---
title: 'Linee guida sulle modifiche annunciate: [!DNL Google] e [!DNL Yahoo]'
description: 'Linee guida sulle modifiche annunciate: [!DNL Google] e [!DNL Yahoo]'
role: Admin
level: Experienced
doc-type: Article
last-substantial-update: 2023-11-06T00:00:00Z
jira: KT-14320
thumbnail: KT-14320.jpeg
exl-id: 879e9124-3cfe-4d85-a7d1-64ceb914a460
source-git-commit: 1f2a6c7b53a5f5110250c8aecac349c5b72feb6b
workflow-type: tm+mt
source-wordcount: '1759'
ht-degree: 0%

---

# Linee guida sulle modifiche annunciate: [!DNL Google] e [!DNL Yahoo]

Il 3 ottobre entrambi [!DNL Google] e [!DNL Yahoo] hanno annunciato congiuntamente i cambiamenti pianificati tramite i loro blog. Queste modifiche sono progettate per mantenere gli utenti più sicuri e fornire una migliore esperienza e-mail applicando alcune best practice comuni del settore come nuovi requisiti a partire dal 1° febbraio 2024.

[https://blog.google/products/gmail/gmail-security-authentication-spam-protection/](https://blog.google/products/gmail/gmail-security-authentication-spam-protection/){target="_blank"}

![[!DNL Google] Notifica_](/help/assets/Gmail.png)

[https://blog.postmaster.yahooinc.com/post/730172167494483968/more-secure-less-spam](https://blog.postmaster.yahooinc.com/post/730172167494483968/more-secure-less-spam){target="_blank"}

![[!DNL Yahoo] Annuncio](/help/assets/Yahoo.png)

Gli esperti di recapito messaggi e-mail all’Adobe hanno letto tutti questi post di blog e tutta la documentazione collegata in modo da non doverlo fare. Ecco le principali soluzioni da adottare.

## Quindi, cosa sono esattamente [!DNL Google] e [!DNL Yahoo] cosa sta facendo?

Nel mondo dell’e-mail esistono requisiti legali, requisiti pratici e best practice generali. I requisiti legali variano notevolmente da luogo a luogo e non fanno parte di questo argomento. Invece, [!DNL Google] e [!DNL Yahoo] stanno adottando le best practice e le stanno trasformando in requisiti pratici.

Nessuno degli elementi [!DNL Google] e [!DNL Yahoo] cominceranno a richiedere in febbraio, sono nuove e spesso sono state le best practice raccomandazioni per anni, ma l&#39;adozione è stata lenta e irregolare nel settore. Questo è [!DNL Google] e [!DNL Yahoo]come contribuire al progresso del processo di adozione dicendo &quot;Se desideri distribuire le e-mail ai nostri utenti (ciò può rappresentare una parte significativa del tuo elenco e-mail, in alcuni casi fino al 70%, a seconda dell’area geografica e del settore) devi fare queste cose.&quot;

## Quali sono i dettagli?

Se sei un cliente Adobe, la maggior parte di ciò che richiede fa già parte della configurazione, ma esistono alcuni elementi chiave tecnicamente facoltativi e vorrai essere certo che la tua organizzazione abbia adottato e configurato correttamente questi elementi prima di febbraio 2024.

## DMARC:

[!DNL Google] e [!DNL Yahoo] richiederanno entrambi di disporre di un record DMARC per qualsiasi dominio utilizzato per inviare loro e-mail. Attualmente NON richiedono un’impostazione di p=rifiuto o p=quarantena, quindi un’impostazione di p=nessuno, comunemente denominata impostazione di &quot;monitoraggio&quot;, è perfettamente accettabile per il momento. Questo non cambierà la modalità di elaborazione delle e-mail, ma farà ciò che farebbe normalmente senza DMARC. Questa configurazione è il primo passo per proteggersi con DMARC e, oltre al nuovo vantaggio di aiutarvi a inviare e-mail a [!DNL Google] e [!DNL Yahoo] può anche aiutarti a verificare se ci sono problemi di autenticazione in qualsiasi punto del tuo ecosistema e-mail.

Le regole per DMARC non cambiano, il che significa che, a meno che non sia configurato per impedirlo, un record DMARC sul dominio principale (adobe.com, ad esempio) verrà ereditato e riguarderà qualsiasi sottodominio (come email.adobe.com). Non sono necessari record DMARC diversi per i sottodomini, a meno che non si desideri o non si desideri aggiungerli per diversi motivi aziendali.

DMARC è completamente supportato in Adobe, ma non è richiesto. Utilizza qualsiasi controllo DMARC gratuito per verificare se hai configurato il DMARC per i tuoi sottodomini; in caso contrario, rivolgiti al team di supporto per gli Adobi per capire come procedere al meglio per ottenere tale configurazione.

Ulteriori informazioni su DMARC e su come implementarlo [qui](https://experienceleague.adobe.com/docs/deliverability-learn/deliverability-best-practice-guide/additional-resources/technotes/implement-dmarc.html?lang=it){target="_blank"} for Adobe Campaign or [here](https://experienceleague.adobe.com/docs/marketo/using/getting-started-with-marketo/setup/configure-protocols-for-marketo.html){target="_blank"} per Marketo Engage.

## 1-clic (elenco) per annullare l’iscrizione:

Non farti prendere dal panico. [!DNL Google] e [!DNL Yahoo] non sta parlando dei collegamenti per annullare l’abbonamento nel corpo o nel piè di pagina dell’e-mail che potrebbero essere cliccati da un bot di sicurezza che fa solo il suo lavoro o per errore. Ciò che intendono è la funzionalità di intestazione Annullamento iscrizione a mailing list per le versioni &quot;mailto&quot; o &quot;http/URL&quot;. Questa è la funzione all&#39;interno del [!DNL Yahoo] e le interfacce utente di Gmail in cui gli utenti possono fare clic per annullare l’abbonamento. Gmail chiede anche agli utenti che fanno clic su &quot;Segnala spam&quot; di vedere se hanno intenzione di annullare l’abbonamento, il che può ridurre il numero di reclami che ricevi (i reclami danneggiano la tua reputazione) trasformandoli in annullamenti di abbonamento (non danneggia la tua reputazione).

È importante notare che [!DNL Google] e [!DNL Yahoo] fanno entrambi riferimento all’opzione &quot;http/URL&quot; con il nome &quot;1-clic&quot;, il che è intenzionale. Tecnicamente, l’opzione &quot;http/URL&quot; originale consentiva di reindirizzare i destinatari a un sito web. Questo non è il punto centrale di [!DNL Yahoo] e [!DNL Google], che fanno entrambi riferimento al file aggiornato [RFC8058](https://datatracker.ietf.org/doc/html/rfc8058){target="_blank"} che si concentra sull’elaborazione dell’annullamento dell’abbonamento tramite una richiesta POST HTTPS, anziché tramite un sito web, rendendolo &quot;1-Click&quot;.

Oggi, Gmail accetta l’opzione &quot;mailto&quot; per annullare l’iscrizione all’elenco. Gmail ha detto che &quot;mailto&quot; non soddisfa le loro aspettative in futuro, e i mittenti dovranno avere l&#39;opzione &quot;post&quot; list-unsubscribe abilitata. I mittenti che dispongono già dell’annullamento dell’iscrizione di un certo tipo avranno tempo fino al 1° giugno 2024 per annullare l’iscrizione con un solo clic.

[!DNL Yahoo] Ha detto che continueranno ad onorare l&#39;opzione &quot;mailto&quot;, per ora, ma che anche loro richiederanno il &quot;post&quot; in futuro.

L’Adobe consiglia di utilizzare le opzioni &quot;mailto&quot; e &quot;post/1-Click&quot; per annullare l’iscrizione tramite elenco. Adobe sta lavorando all’abilitazione del supporto &quot;post&quot; su tutte le piattaforme di invio e-mail per supportare i nostri utenti nel soddisfare questi requisiti, con ulteriori aggiornamenti a breve.

La necessità di intestazioni per l’annullamento dell’iscrizione a mailing list non si applica alle e-mail transazionali. Tieni presente che i messaggi attivati, come il carrello abbandonato e comunicazioni simili non generate dall’abbonato, vengono considerati marketing da parte dei provider di caselle postali come [!DNL Google] e [!DNL Yahoo] e per quelli potrebbe essere necessario annullare l’iscrizione alla lista.

[!DNL Google] e [!DNL Yahoo] sono entrambi consapevoli del fatto che in alcuni casi un destinatario annullerà l’abbonamento e lo ripresenterà in un secondo momento. Anche se non sono disposti a condividere la salsa segreta di come identificano queste situazioni, stanno lavorando su metodi per evitare di penalizzare i mittenti in modo errato in questi casi.

>[!INFO]
> Adobe sta lavorando all’abilitazione del supporto &quot;post&quot; su tutte le piattaforme di invio e-mail per supportare i nostri utenti nel soddisfare questi requisiti:
> * [!DNL Adobe Campaign Classic V7/V8]: oggi supporta completamente POST 1-Click. Gli aggiornamenti alla configurazione dettagliata verranno pubblicati [qui](https://experienceleague.adobe.com/docs/deliverability-learn/deliverability-best-practice-guide/additional-resources/campaign/acc-technical-recommendations.html?lang=en#list-unsubscribe){target="_blank"} entro metà gennaio
>* [!DNL Adobe Campaign Standard]: in fase di aggiornamento per supportare POST 1-Click. Controlla di nuovo la disponibilità di aggiornamenti a breve. Verranno fornite le istruzioni per la configurazione [qui](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-14778.html?lang=en){target="_blank"}
>* [!DNL Adobe Journey Optimizer]: oggi supporta completamente POST 1-Click. Gli aggiornamenti alla configurazione dettagliata verranno pubblicati [qui](https://experienceleague.adobe.com/docs/journey-optimizer/using/email/email-opt-out.html?lang=en){target="_blank"} entro metà gennaio
>
> Marketo: in fase di aggiornamento per supportare POST 1-Click. Quando è pronto, viene applicato automaticamente, se necessario.


## Elabora annullamenti iscrizione entro 2 giorni:

Questa è stata una best practice consigliata per un po’ di tempo, poiché ogni e-mail distribuita a un utente che ha annullato l’abbonamento in genere genera un reclamo spam, quindi prima smetti di inviargli e-mail, meglio è. Anche in questo caso, i requisiti legali potrebbero essere molto più lunghi in alcuni casi, ma [!DNL Google] e [!DNL Yahoo] saprà che il loro utente ha annullato l’abbonamento tramite Annulla iscrizione e che stai ancora inviando loro e-mail il giorno 3, e hanno dichiarato che non consentiranno ai mittenti che lo fanno di continuare a inviare e-mail a QUALSIASI dei loro utenti.

Questo requisito di 2 giorni è applicabile a qualsiasi annullamento dell’iscrizione tramite i vari metodi di annullamento dell’iscrizione a mailing list. In alcuni casi (come &quot;mailto&quot;) ciò significa che Adobe li elaborerà. Adobe elabora tutte le richieste di annullamento dell’abbonamento immediatamente dopo il ricevimento della richiesta, ben entro il limite di 2 giorni. In altri casi, è possibile che vengano elaborate. Se elabori queste richieste, potresti dover apportare delle modifiche alla fine per rispettare questa sequenza temporale di 2 giorni.

## Percentuali reclami:

Mantenere bassi i tassi di reclamo al di sotto dello 0,2% è da tempo una buona pratica. [!DNL Google] L’Austria ha fissato un valore più alto allo 0,3% per un periodo di tempo prolungato, ma ha chiaramente affermato che mantenere la percentuale al di sotto dello 0,1% comporta dei vantaggi. Ecco i dettagli condivisi:

* Mantieni la tua percentuale di posta indesiderata al di sotto dello 0,10%.
* Evita una percentuale di spam dello 0,30% o superiore, soprattutto per un periodo di tempo prolungato.
* Mantenere un basso tasso di posta indesiderata rende i mittenti più resilienti ai picchi occasionali di feedback degli utenti.
* Allo stesso modo, il mantenimento di un elevato tasso di spam porterà a una classificazione più elevata dello spam. Può essere necessario del tempo affinché i miglioramenti della frequenza di posta indesiderata si riflettano positivamente sulla classificazione della posta indesiderata.

[!DNL Yahoo] ha dichiarato che anche la soglia di reclamo sarà dello 0,30%.

[!DNL Google] e [!DNL Yahoo]L&#39;obiettivo non è quello di punire i mittenti per un singolo giorno cattivo o per un errore che causa un picco temporaneo nelle lamentele. Invece, si concentrano sui mittenti che hanno tassi elevati di reclami per un periodo di tempo prolungato o un modello di cattivo comportamento di invio.

Se hai bisogno di assistenza per monitorare le percentuali dei reclami, o desideri ricevere assistenza per elaborare strategie per ridurre i reclami, contatta il tuo Adobe di Consulente per il recapito dei messaggi, o parla con il team del tuo account dell’aggiunta di un Consulente per il recapito dei messaggi, se non ne hai già uno.

## Quali tempistiche stiamo esaminando?

Sono stati aggiornati i tempi rispetto all&#39;annuncio originale di ottobre. Le timeline più recenti sono simili al seguente:

[!DNL Gmail]

Febbraio 2024 - Inizieranno i mancati recapiti temporanei progettati per fornire un avviso di non conformità. Le e-mail continueranno a essere consegnate come di consueto dopo un breve ritardo se non sei ancora in regola con le normative. Se sei pienamente conforme, non ci saranno mancati recapiti temporanei e non noterai nulla.

Aprile 2024 - Inizieranno i blocchi per i mittenti che non sono conformi a tutto eccetto List-Unsubscribe 1-Click. Solo una parte delle e-mail non conformi verrà bloccata inizialmente, con % bloccato in aumento nel tempo.

1 giugno 2024 - Qualsiasi mittente non pienamente conforme, incluso List-Unsubscribe 1-Click, si troverà in stato di blocco.

[!DNL Yahoo]

Non ha fornito date esatte, ma ha detto che &quot;l&#39;introduzione dell&#39;applicazione inizierà a febbraio 2024. L&#39;applicazione sarà introdotta gradualmente&quot;.

## Che impatto ha su di me, in qualità di esperto di marketing?

Il mancato rispetto di questi nuovi requisiti da parte di Gmail e [!DNL Yahoo] dovrebbe causare l’invio di e-mail nella cartella di posta indesiderata o il blocco (ovvero, il recupero di un messaggio non recapitato dal MBP che indica che l’e-mail non è stata consegnata).

Di conseguenza, Adobe consiglia vivamente di esaminare le modifiche descritte in precedenza e assicurarsi di iniziare a conformarti con esse il prima possibile. Ora è anche il momento ideale per iniziare a valutare le tue prestazioni su [!DNL Yahoo] e [!DNL Google] per verificare se sono presenti modifiche materiali alle metriche, visita febbraio.

Siamo qui per aiutarti, quindi se hai domande o hai bisogno di supporto, parlane con il tuo Adobe di consulente per il recapito messaggi o parlane con il team del tuo account per aggiungere un consulente per il recapito messaggi, se non ne hai già uno.

## Ci sono modi per aggirarlo?

Anche se questa è sempre una domanda che viene fuori, la realtà è che questi cambiamenti hanno senso per gli utenti finali di [!DNL Google] e [!DNL Yahoo]piattaforme di. Sono motivati dalle aspettative di tali utenti di far rispettare queste regole. Si consiglia di non aggirare queste modifiche, ma di fare un passo indietro e pensare a come adattarle.

## Nota finale:

Al momento questo non si applica alle e-mail inviate a [!DNL Yahoo].JP o [!DNL Gmail] Tuttavia, si applica anche alle e-mail provenienti da tali posizioni.

## Risorse aggiuntive (non specifiche per queste modifiche):

[Linee guida per mittenti Google](https://support.google.com/mail/answer/81126){target="_blank"}

[Domande frequenti su Google](https://support.google.com/a/answer/14229414?sjid=2864589551334481470-NC){target="_blank"}

[Linee guida per i mittenti di Yahoo](https://senders.yahooinc.com/best-practices/){target="_blank"}

[Domande frequenti su Yahoo](https://senders.yahooinc.com/faqs/){target="_blank"}
