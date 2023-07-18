---
title: Processo di richiesta del certificato SSL
description: Scopri come installare i certificati SSL nei sottodomini a cui hai delegato Adobe.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 8a78abd3-afba-49a7-a2ae-8b2c75326749
source-git-commit: 57016f89df54d5c74755a6a108a92db45153ec18
workflow-type: tm+mt
source-wordcount: '2252'
ht-degree: 3%

---

# Processo di richiesta del certificato SSL

Dopo aver delegato un dominio ad Adobe per l’invio di e-mail (consulta [Impostazione del nome di dominio](/help/additional-resources/ac-domain-name-setup.md)), Adobe creerà e utilizzerà alcuni sottodomini per funzioni specifiche.

Ad esempio, se hai delegato *email.example.com* ad Adobe, per l’invio di e-mail, Adobe creerà sottodomini come i seguenti:
* *t.email.example.com* - per il tracciamento dei collegamenti
* *m.email.example.com* - per pagine mirror
* *res.email.example.com* - per risorse in hosting (come immagini)

Si consiglia di: **proteggere questi domini tramite SSL (HTTPS)**. Infatti, i collegamenti non protetti (HTTP) sono vulnerabili alle intercettazioni e segnaleranno gli avvisi sui browser moderni.

Per installare i certificati SSL su questi sottodomini, la procedura comporta la richiesta di un file CSR e successivamente l’acquisto di certificati SSL, ad Adobe per l’installazione o il rinnovo.

>[!CAUTION]
>
>Prima di installare un certificato SSL, accertati di conoscere i prerequisiti elencati in [questa pagina](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html?lang=it#installing-ssl-certificate).
>
>Adobe supporta solo certificati fino a 2048 bit. I certificati a 4096 bit non sono ancora supportati.

## Glossario

| Termine | Descrizione |
|--- |--- |
| CA (autorità di certificazione) | Provider di certificati SSL che rilascia certificati digitali a organizzazioni o singoli utenti dopo averne verificato l&#39;identità, ad esempio DigiCert, Symantec e così via.<ul><li>Una CA attendibile viene in genere considerata una CA di terze parti che emette un certificato radice.</li><li>Se il certificato è firmato dalla stessa organizzazione/società che lo utilizza, viene classificato come CA non attendibile anche quando si tratta di certificati SSL, ad esempio certificati autofirmati.</li></ul> |
| Certificato catena | Un certificato che include un certificato radice e uno o più certificati intermedi è detto certificato concatenato. |
| CSR (richiesta di firma del certificato) | Blocco di testo codificato assegnato a un’autorità di certificazione quando si richiede un certificato SSL. In genere viene generato nel server in cui è installato il certificato. |
| DER (Regole di codifica distinte) | Un tipo di estensione del certificato. L&#39;estensione .der viene utilizzata per i certificati con codifica DER binaria. Questi file possono anche supportare l&#39;estensione cer o crt. |
| Certificato EV (convalida estesa) | Un certificato EV è un nuovo tipo di certificato progettato per prevenire gli attacchi di phishing. Richiede una convalida estesa della tua attività e della persona che ordina il certificato. |
| Certificato di affidabilità elevata | I certificati ad alta affidabilità vengono rilasciati dalla CA dopo aver verificato la proprietà del nome di dominio e la registrazione aziendale valida. |
| CA intermedia | Un’autorità di certificazione dei certificati intermedi inclusi in un certificato della catena. |
| Certificato intermedio | Un’autorità di certificazione rilascia i certificati sotto forma di struttura ad albero. Il certificato radice è il certificato più in alto della struttura. Qualsiasi certificato tra il certificato e il certificato radice è denominato certificato a catena o intermedio. |
| Certificato di garanzia bassa | Un certificato a bassa affidabilità, denominato anche certificato convalidato dal dominio, include solo il nome di dominio nel certificato (e non il nome dell’azienda/organizzazione). |
| PEM (Privacy Enhanced Mail) | Certificato con estensione .pem contenente dati ASCII (Base64). Tali certificati iniziano con una riga &quot; - - - - - BEGIN CERTIFICATE - - - -&quot;. |
| Certificato radice | Un’autorità di certificazione rilascia i certificati sotto forma di struttura ad albero. Il certificato radice è il certificato più in alto della struttura. |
| SAN (nome alternativo soggetto) | I nomi alternativi dei soggetti sono nomi host aggiuntivi (siti, indirizzi IP, nomi comuni, ecc.) che devono essere firmate come parte di un singolo certificato SSL. |
| Certificato autofirmato | Certificato firmato dalla persona che lo ha creato anziché da un&#39;autorità di certificazione attendibile. I certificati autofirmati possono abilitare lo stesso livello di crittografia di un certificato firmato da una CA, ma vi sono due principali inconvenienti:<ul><li>La connessione di un visitatore potrebbe essere bloccata consentendo a un utente malintenzionato di visualizzare tutti i dati inviati (vanificando in tal modo lo scopo della crittografia della connessione)</li><li> Impossibile revocare il certificato come un certificato attendibile.</li></ul> |
| SSL (Secure Sockets Layer) | La tecnologia di sicurezza standard per stabilire un collegamento crittografato tra un server web e un browser. |
| Certificato con caratteri jolly | Un certificato con caratteri jolly può proteggere un numero illimitato di sottodomini di primo livello su un singolo nome di dominio, ad esempio *.adobe.com. |

## Passaggi principali

1. Richiedi un file CSR (Certificate Signing Request) e fornisci le informazioni richieste (paese, stato, città, nome organizzazione, nome unità organizzativa, ecc.) all&#39;Adobe.
1. Convalida il file CSR generato da Adobe e verifica che tutte le informazioni fornite siano corrette.
1. Utilizzare i dettagli CSR per generare un certificato firmato da un&#39;autorità di certificazione attendibile<!--taking care of asking for using the subjectAltName SSL extension (SAN) if it is for several domain names, and get/purchase the resulting certificate (ideally) in PEM format for Apache server-->.
1. Convalida il certificato SSL e verifica che corrisponda alla CSR.
1. Fornisci il certificato SSL all’Adobe, che lo installerà.
1. Verifica che il certificato SSL sia stato installato correttamente per ogni sottodominio protetto.
1. Monitora il periodo di validità del certificato SSL.
1. Aggiorna qualsiasi configurazione specifica in Adobe Campaign.

## Processo dettagliato

### Prerequisiti

Devi identificare i nomi di dominio e le funzioni (tracciamento, pagine mirror, applicazioni web, ecc.) per proteggere.
>[!NOTE]
>
>Adobe può essere utile per definire i nomi di dominio e le funzioni da coinvolgere. Per ulteriori informazioni, contatta il team del tuo account Adobe.

### Passaggio 1: ottenere un file CSR

Per ottenere un file CSR (Certificate Signing Request), segui i passaggi indicati di seguito.

* Se hai accesso a [Pannello di controllo Campaign](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=it), seguire le istruzioni su [questa pagina](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html?lang=it#subdomains-and-certificates) per generare e scaricare un file CSR dal Pannello di controllo Campaign.

* In caso contrario, crea un ticket di supporto tramite https://adminconsole.adobe.com/ per ottenere un file CSR da Adobe Customer Care per i sottodomini richiesti.

Ecco alcune best practice da seguire:

* Genera una richiesta per sottodominio delegato.
* È possibile combinare più sottodomini in un’unica richiesta CSR, ma solo all’interno dello stesso ambiente. In Campaign Classic, ad esempio, il server di marketing, [server di mid-sourcing](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/install-campaign-on-prem/mid-sourcing-server.html)e [istanza di esecuzione](https://experienceleague.adobe.com/docs/campaign-classic/using/transactional-messaging/configure-transactional-messaging/configuring-instances.html#execution-instance) sono tre ambienti separati.
* Devi ottenere una nuova CSR prima di qualsiasi rinnovo del certificato SSL. Non utilizzare un vecchio file CSR di un anno fa o più.

Dovrai fornire le seguenti informazioni.

>[!CAUTION]
>
>Compilare tutti i campi indicati nelle tabelle seguenti. In caso contrario, non è possibile elaborare la richiesta CSR.

**Informazioni da fornire con l’assistenza dell’équipe di Adobi:**

| Informazioni da fornire | Esempio di valore | Nota |
|--- |--- |--- |
| Nome client | La mia azienda Inc. | Nome dell’organizzazione. Questo campo viene utilizzato da Adobe per tenere traccia della richiesta (non farà parte del certificato CSR/SSL). |
| URL ambiente Adobe Campaign | https://client-mid-prod1.campaign.adobe.com | URL dell’istanza Adobe Campaign. |
| Nome comune [CN] | t.subdomain.customer.com | Può trattarsi di uno qualsiasi dei domini rilevanti, ma in genere del dominio di tracciamento. |
| Nome alternativo soggetto [SAN] | t.subdomain.customer.com | Assicurati di includere il sottodominio di tracciamento come SAN. |
| Nome alternativo soggetto [SAN] | m.subdomain.customer.com |
| Nome alternativo soggetto [SAN] | res.subdomain.customer.com |

**Informazioni fornite dal team interno IT/SSL:**

| Informazioni da fornire | Esempio di valore | Nota |
|--- |--- |--- |
| Paese [C] | STATI UNITI | Deve essere un codice di due lettere. Accedere all’elenco completo dei paesi [qui](https://www.ssl.com/csrs/country_codes/).</br>*Nota: per il Regno Unito, utilizzare GB (non UK).* |
| Stato (o nome della provincia) [ST] | Illinois | Se applicabile. Il valore deve essere un nome completo, non abbreviato. |
| Nome città/località [L] | Chicago |
| Nome organizzazione [O] | ACME |
| Nome unità organizzativa [OU] | IT |

>[!NOTE]
>
>Sostituisci &quot;subdomain.customer.com&quot; con il tuo sottodominio delegato e gli altri valori di esempio con i valori appropriati.

### Passaggio 2: convalidare il file CSR

Dopo aver inviato la richiesta con le informazioni pertinenti, Adobe genera e fornisce un file CSR (Certificate Signing Request).

Il testo nel file CSR risultante deve iniziare con **&quot;-----INIZIA RICHIESTA CERTIFICATO-----&quot;**.

Una volta ricevuto il file CSR da Adobe, segui i passaggi seguenti:

1. Copiare e incollare il testo del file CSR in un decodificatore online come https://www.sslshopper.com/csr-decoder.html, <!--https://www.certlogik.com/decoder/,--> o https://www.entrust.net/ssl-technical/csr-viewer.cfm.
In alternativa, è possibile utilizzare *OpenSSL* su un computer Linux.
1. Verifica che tutti i controlli abbiano esito positivo.
1. Verifica che siano inclusi i parametri e i nomi di dominio corretti.
1. Verificate che tutti gli altri dati corrispondano ai dettagli forniti al momento dell&#39;invio della richiesta.

### Passaggio 3: generare il certificato SSL

Una volta fornito il file CSR, devi acquistare e generare un certificato SSL per i domini appropriati utilizzando il file CSR.

* Il certificato SSL:
   * deve essere in formato Apache PEM;
   * non deve essere superiore a 2048 bit;
   * deve essere firmata da un’autorità di certificazione (CA) valida;
   * deve includere tutte le SAN (Subject Alternative Names) come indicato nel file CSR.
* Se sono presenti uno o più certificati intermedi, è necessario fornire il certificato radice e tutti i certificati intermedi per l&#39;Adobe.
* È possibile impostare qualsiasi periodo di validità del certificato, ma l&#39;Adobe consiglia di sceglierlo sufficientemente lungo (ad esempio, due anni).

>[!NOTE]
>
>Se utilizzi strumenti interni o un portale fornito da una CA per richiedere il certificato, assicurati di utilizzare gli stessi dettagli forniti nella richiesta CSR per evitare ritardi o discrepanze nel processo di generazione del certificato.

### Passaggio 4: convalidare il certificato SSL

Una volta generato il certificato SSL, devi convalidarlo prima di inviarlo a Adobe. A questo scopo, segui i passaggi riportati qui sotto:

1. Assicurati che il certificato abbia l’estensione .pem. In caso contrario, convertirlo nel formato PEM. Puoi effettuare la conversione utilizzando *OpenSSL*.
1. Conferma che il certificato inizi con **&quot;-----INIZIA CERTIFICATO-----&quot;**.
1. Copia il testo del certificato in un decodificatore online, ad esempio https://www.sslshopper.com/certificate-decoder.html o https://www.entrust.net/ssl-technical/csr-viewer.cfm.
In alternativa, è possibile utilizzare *OpenSSL* su un computer Linux. Per ulteriori informazioni, consulta [questa pagina esterna](https://www.shellhacks.com/decode-ssl-certificate/).
1. Assicurati che il certificato sia risolto correttamente, inclusi Nome comune, SAN, Autorità emittente e Periodo di validità.
1. Se la verifica del certificato SSL ha esito positivo, verifica che il certificato corrisponda alla CSR utilizzando [questo sito web](https://www.sslshopper.com/certificate-key-matcher.html): seleziona **Verifica se una CSR e un certificato corrispondono** e inserisci il certificato e la CSR nei campi corrispondenti. Dovrebbero corrispondere.

### Passaggio 5: richiesta dell’installazione del certificato SSL

* Se hai accesso a [Pannello di controllo Campaign](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=it), seguire le istruzioni su [questa pagina](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html?lang=it#installing-ssl-certificate) per caricare il certificato nel Pannello di controllo Campaign.

* In caso contrario, crea un altro ticket di supporto tramite https://adminconsole.adobe.com/ per richiedere all’Adobe di installare il certificato sui server di Adobe.

Dovrai fornire:

* Il file del certificato, il certificato radice ed eventuali certificati intermedi (allegati al ticket), preferibilmente in formato Apache PEM.
* Il numero del precedente ticket di supporto generato per la CSR.
* Gli stessi dati forniti per il ticket CSR (inclusi Nome comune, URL istanza, Stato, Città/Località, Nome organizzazione, Nome unità organizzativa, ecc.).

### Passaggio 6: verifica dell’installazione del certificato SSL

Una volta che il certificato SSL è installato e confermato dall’Assistenza clienti Adobe, accertati che sia stato installato correttamente per tutti gli URL.

Esegui i test seguenti prima di chiudere il ticket di installazione SSL. Inoltre, assicurati di aggiornare qualsiasi configurazione specifica come indicato in [questa sezione](#update-configuration).

Passa ai seguenti URL nel browser (sostituisci &quot;subdomain.customer.com&quot; con il tuo sottodominio):

* https://subdomain.customer.com/r/test (per [applicazioni web](https://experienceleague.adobe.com/docs/campaign-classic/using/designing-content/web-applications/about-web-applications.html) solo sottodomini: non si applica ai sottodomini e-mail)
* https://t.subdomain.customer.com/r/test
* https://m.subdomain.customer.com/r/test
* https://res.subdomain.customer.com/r/test

In caso di esito positivo, vengono fornite informazioni sull’ambiente e la barra degli indirizzi nell’URL indica che la connessione è sicura. Ad esempio, puoi visualizzare il seguente messaggio in Google Chrome:

![](../../help/assets/ssl-google-successful-result.png)

Se il certificato SSL non è installato correttamente, viene visualizzato il seguente avviso:

![](../../help/assets/ssl-google-unsuccessful-result.png)

### Passaggio 7: verifica del periodo di validità del certificato

Puoi controllare il periodo di validità del certificato nel browser. Ad esempio, in Google Chrome, fai clic su **Protetto** > **Certificato**.

È tua responsabilità controllare il periodo di validità. L’Adobe consiglia di implementare una procedura per monitorare la scadenza dei certificati. Ulteriori informazioni su cosa accade alla scadenza del certificato SSL in [questo articolo](https://www.thesslstore.com/blog/what-happens-when-your-ssl-certificate-expires/).

* Crea un ticket di supporto per richiedere un certificato aggiornato almeno due settimane prima della data di scadenza del certificato. Non è necessario richiedere un’ulteriore CSR, a meno che i dettagli della CSR non siano cambiati.

* Se hai accesso a [Pannello di controllo Campaign](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=it), e se il tuo ambiente è ospitato da Adobe in un ambiente AWS, puoi utilizzare il Pannello di controllo Campaign per rinnovare il certificato prima della scadenza. Ulteriori informazioni in [questa sezione](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-ssl-certificates.html#monitoring-certificates).

### Passaggio 8: aggiornare eventuali configurazioni specifiche {#update-configuration}

Una volta che hai la certezza che i certificati SSL richiesti siano installati correttamente, puoi aggiornare tutti i riferimenti in Adobe Campaign da HTTP a HTTPS.

>[!NOTE]
>
>Per Campaign Classic, gli URL da aggiornare si trovano principalmente in [Distribuzione guidata](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/initial-configuration/deploying-an-instance.html#deployment-wizard) e nella [Account esterni](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/accessing-external-database/external-accounts.html) (domini di tracciamento, pagina mirror e risorse pubbliche). Per Campaign Standard, consulta [Configurazione del branding](https://experienceleague.adobe.com/docs/campaign-standard/using/administrating/application-settings/branding.html#about-brand-identity).

Una volta aggiornate le configurazioni, le nuove e-mail verranno inviate con URL HTTPS anziché HTTP. Per verificare che gli URL siano ora protetti, puoi eseguire rapidamente i seguenti test:

* Carica un’immagine da Adobe Campaign. Una volta caricata l’immagine, l’URL restituito deve essere HTTPS.
* Crea una consegna e-mail di prova che includa un collegamento alla pagina speculare, alcune immagini, del testo e un collegamento per annullare l’abbonamento. Invia l’e-mail a un ID e-mail esterno (come il tuo indirizzo Gmail). Una volta ricevuto, apri l’e-mail e assicurati che tutti i collegamenti all’interno dell’e-mail si aprano correttamente nel modulo HTTPS (non HTTP), senza avvertenze o errori relativi al certificato SSL.

## Risorse specifiche per i prodotti

**Campaign Classic**

* [Pannello di controllo Campaign: aggiunta di certificati SSL (tutorial)](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html) : scopri come aggiungere certificati SSL per proteggere i sottodomini.

**Campaign Standard**

* [Pannello di controllo Campaign: aggiunta di certificati SSL (tutorial)](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html?lang=it) : scopri come aggiungere certificati SSL per proteggere i sottodomini.
