---
title: Processo di richiesta del certificato SSL
description: Scopri come installare i certificati SSL nei sottodomini delegati ad Adobe.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 8a78abd3-afba-49a7-a2ae-8b2c75326749
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '2265'
ht-degree: 2%

---

# Processo di richiesta del certificato SSL

Dopo aver delegato un dominio ad Adobe per l’invio di e-mail (consulta [Impostazione del nome di dominio](/help/additional-resources/ac-domain-name-setup.md)), Adobe creerà e utilizzerà alcuni sottodomini per funzioni specifiche.

Ad esempio, se hai delegato *email.example.com* ad Adobe per l’invio di e-mail, Adobe creerà sottodomini come ad esempio:
* *t.email.example.com*  - per collegamenti di tracciamento
* *m.email.example.com*  - per pagine mirror
* *res.email.example.com*  - per risorse in hosting (come immagini)

È consigliabile **proteggere questi domini tramite SSL (HTTPS)**. In effetti, i collegamenti non sicuri (HTTP) sono vulnerabili all&#39;intercettazione e contrassegneranno gli avvisi sui browser moderni.

Per installare i certificati SSL in questi sottodomini, il processo comporta la richiesta di un file CSR e successivamente l’acquisto di certificati SSL per Adobe da installare o rinnovare.

>[!CAUTION]
>
>Prima di installare un certificato SSL, accertati di conoscere i prerequisiti elencati in [questa pagina](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html#installing-ssl-certificate).
>
>Adobe supporta solo fino a 2048 certificati a 8 bit. I certificati a 4096 bit non sono ancora supportati.

## Glossario

| Termine | Descrizione |
|--- |--- |
| CA (autorità di certificazione) | Provider di certificati SSL che rilascia certificati digitali a organizzazioni o singoli utenti dopo la verifica della loro identità, ad esempio DigiCert, Symantec, ecc.<ul><li>Una CA attendibile viene generalmente considerata come una CA di terze parti che rilascia un certificato radice.</li><li>Se il certificato è firmato dalla stessa organizzazione/società che utilizza il certificato, viene classificato come CA non attendibile anche se si tratta di certificati SSL, ad esempio certificati autofirmati.</li></ul> |
| Certificato a catena | Un certificato che include un certificato radice e uno o più certificati intermedi è denominato certificato a catena (o concatenato). |
| CSR (richiesta di firma del certificato) | Un blocco di testo codificato fornito a un’autorità di certificazione quando si richiede un certificato SSL. In genere viene generato sul server in cui è installato il certificato. |
| DER (regole di codifica distinte) | Un tipo di estensione del certificato. L&#39;estensione .der viene utilizzata per i certificati codificati DER binari. Questi file possono anche supportare l&#39;estensione .cer o .crt. |
| Certificato EV (convalida estesa) | Un certificato EV è un nuovo tipo di certificato progettato per prevenire attacchi di phishing. Richiede una convalida estesa della tua azienda e della persona che ordina il certificato. |
| Certificato di garanzia elevata | I certificati di alta garanzia sono rilasciati dalla CA dopo aver verificato la proprietà del nome di dominio e la registrazione commerciale valida. |
| CA intermedia | Autorità di certificazione dei certificati intermedi inclusi in un certificato a catena. |
| Certificato intermedio | Un’autorità di certificazione rilascia i certificati sotto forma di struttura ad albero. Il certificato principale è il certificato più alto della struttura. Qualsiasi certificato tra il certificato e il certificato principale è denominato certificato a catena o intermedio. |
| Certificato di garanzia insufficiente | Un certificato di garanzia basso, denominato anche certificato convalidato per il dominio, include solo il nome di dominio nel certificato (e non il nome società/organizzazione). |
| PEM (Privacy Enhanced Mail) | Certificato con estensione .pem contenente dati ASCII (Base64). Tali certificati iniziano con una linea &quot; - - - - - - - - - - -&quot; CERTIFICATO INIZIALE. |
| Certificato radice | Un’autorità di certificazione rilascia i certificati sotto forma di struttura ad albero. Il certificato principale è il certificato più alto della struttura. |
| SAN (nome alternativo soggetto) | I nomi alternativi oggetto sono nomi host aggiuntivi (siti, indirizzi IP, nomi comuni, ecc.) deve essere firmato come parte di un singolo certificato SSL. |
| Certificato autofirmato | Un certificato firmato dalla persona che lo ha creato anziché da un’autorità di certificazione attendibile. I certificati autofirmati possono abilitare lo stesso livello di crittografia di un certificato firmato da una CA, ma si verificano due principali svantaggi:<ul><li>La connessione di un visitatore può essere dirottata consentendo a un autore dell’attacco di visualizzare tutti i dati inviati (vanificando così lo scopo di crittografare la connessione)</li><li> Impossibile revocare il certificato come un certificato attendibile.</li></ul> |
| SSL (Secure Sockets Layer) | Tecnologia di sicurezza standard per stabilire un collegamento crittografato tra un server web e un browser. |
| Certificato jolly | Un certificato con caratteri jolly può proteggere un numero illimitato di sottodomini di primo livello in un nome di dominio singolo, ad esempio *.adobe.com. |

## Passaggi principali

1. Richiedi un file CSR (Certificate Signing Request) e fornisci le informazioni richieste (paese, stato, città, nome dell’organizzazione, nome dell’unità organizzativa, ecc.) all&#39;Adobe.
1. Convalida il file CSR generato da Adobe e verifica che tutte le informazioni fornite siano corrette.
1. Utilizza i dettagli CSR per generare un certificato firmato da un&#39;autorità di certificazione affidabile<!--taking care of asking for using the subjectAltName SSL extension (SAN) if it is for several domain names, and get/purchase the resulting certificate (ideally) in PEM format for Apache server-->.
1. Convalida il certificato SSL e verifica che corrisponda alla CSR.
1. Fornisci ad Adobe il certificato SSL, che lo installerà.
1. Verifica che il certificato SSL sia installato correttamente per ciascun sottodominio protetto.
1. Monitora il periodo di validità del certificato SSL.
1. Aggiorna qualsiasi configurazione specifica in Adobe Campaign.

## Procedura dettagliata

### Prerequisiti

Devi identificare i nomi di dominio e le funzioni (tracciamento, pagine mirror, applicazioni web, ecc.) per proteggere.
>[!NOTE]
>
>Un Adobe può essere utile nella definizione dei nomi di dominio e delle funzioni da coinvolgere. Per ulteriori informazioni, contatta il tuo Adobe Customer Success Manager.

### Passaggio 1 - Ottenere un file CSR

Per ottenere un file CSR (Certificate Signing Request), segui i passaggi riportati di seguito.

* Se hai accesso al [Pannello di controllo Campaign](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=it), segui le istruzioni su [questa pagina](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html#subdomains-and-certificates) per generare e scaricare un file CSR dal Pannello di controllo Campaign.

* In caso contrario, crea un ticket di supporto tramite https://adminconsole.adobe.com/ per ottenere un file CSR dall’Assistenza clienti di Adobe per i sottodomini richiesti.

Seguono alcune best practice:

* Solleva una richiesta per sottodominio delegato.
* È possibile combinare più sottodomini in un’unica richiesta CSR, ma solo all’interno dello stesso ambiente. Ad esempio, in Campaign Classic, il server di marketing, il [server di mid-sourcing](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/install-campaign-on-prem/mid-sourcing-server.html) e l’ [istanza di esecuzione](https://experienceleague.adobe.com/docs/campaign-classic/using/transactional-messaging/configure-transactional-messaging/configuring-instances.html#execution-instance) sono tre ambienti separati.
* È necessario ottenere una nuova CSR prima di qualsiasi rinnovo del certificato SSL. Non utilizzare un vecchio file CSR di un anno fa o più.

Dovrai fornire le seguenti informazioni.

>[!CAUTION]
>
>Devono essere compilati tutti i campi indicati nelle tabelle seguenti. In caso contrario, la richiesta CSR non può essere elaborata.

**Informazioni da fornire con l&#39;assistenza del team Adobe:**

| Informazioni da fornire | Valore di esempio | Nota |
|--- |--- |--- |
| Nome client | La mia azienda Inc. | Nome dell’organizzazione. Questo campo viene utilizzato dall’Adobe per monitorare la richiesta (non farà parte del certificato CSR/SSL). |
| URL ambiente Adobe Campaign | https://client-mid-prod1.campaign.adobe.com | URL dell’istanza di Adobe Campaign. |
| Nome comune [CN] | t.subdomain.customer.com | Può trattarsi di uno qualsiasi dei domini rilevanti, ma solitamente del dominio di tracciamento. |
| Oggetto Nome alternativo [SAN] | t.subdomain.customer.com | Assicurati di includere il sottodominio di tracciamento come SAN. |
| Oggetto Nome alternativo [SAN] | m.subdomain.customer.com |
| Oggetto Nome alternativo [SAN] | res.subdomain.customer.com |

**Informazioni da fornire dal team interno IT/SSL:**

| Informazioni da fornire | Valore di esempio | Nota |
|--- |--- |--- |
| Paese [C] | US | Deve essere un codice a due lettere. Accedi all&#39;elenco completo [qui](https://www.ssl.com/csrs/country_codes/).</br>*Nota: Per il Regno Unito, utilizzare GB (non UK).* |
| Stato (o nome della provincia) [ST] | Illinois | Se applicabile. Il valore deve essere un nome completo, non abbreviato. |
| Nome città/località [L] | Chicago |
| Nome organizzazione [O] | ACME |
| Nome unità organizzativa [OU] | IT |

>[!NOTE]
>
>Sostituisci &quot;subdomain.customer.com&quot; con il sottodominio delegato e gli altri valori di esempio con i valori appropriati.

### Passaggio 2: convalidare il file CSR

Dopo aver inviato la richiesta con le informazioni pertinenti, Adobe genera e fornisce un file CSR (Certificate Signing Request).

Il testo nel file CSR risultante deve iniziare con **&quot;—BEGIN CERTIFICATE REQUEST—&quot;**.

Una volta ricevuto il file CSR dall&#39;Adobe, segui i passaggi seguenti:

1. Copia e incolla il testo del file CSR in un decodificatore online come https://www.sslshopper.com/csr-decoder.html, <!--https://www.certlogik.com/decoder/,--> o https://www.entrust.net/ssl-technical/csr-viewer.cfm.
In alternativa, è possibile utilizzare il comando *OpenSSL* localmente su un computer Linux. Per ulteriori informazioni, consulta [questa pagina esterna](https://www.question-defense.com/2009/09/22/use-openssl-to-verify-the-contents-of-a-csr-before-submitting-for-a-ssl-certificate).
1. Verifica che tutti i controlli abbiano esito positivo.
1. Verifica che siano inclusi i parametri e i nomi di dominio corretti.
1. Verifica che tutti gli altri dati corrispondano ai dettagli forniti al momento dell’invio della richiesta.

### Passaggio 3: generare il certificato SSL

Una volta fornito il file CSR, devi acquistare e generare un certificato SSL per i domini appropriati utilizzando il file CSR.

* Certificato SSL:
   * deve essere in formato Apache PEM;
   * non deve essere più lungo di 2048 bit;
   * deve essere firmato da un&#39;autorità di certificazione valida;
   * devono includere tutte le SAN (Subject Alternative Names) come indicato nel file CSR.
* Se sono presenti uno o più certificati intermedi, è necessario fornire ad Adobe il certificato principale e tutti i certificati intermedi.
* Puoi impostare qualsiasi periodo di validità del certificato, ma l’Adobe consiglia di sceglierlo sufficientemente lungo (ad esempio, due anni).

>[!NOTE]
>
>Se utilizzi i tuoi strumenti interni o un portale fornito da una CA per richiedere il certificato, assicurati di utilizzare gli stessi dettagli forniti nella richiesta CSR per evitare ritardi o discrepanze nel processo di generazione del certificato.

### Passaggio 4: convalidare il certificato SSL

Una volta generato il certificato SSL, devi convalidarlo prima di inviarlo ad Adobe. A questo scopo, segui i passaggi riportati qui sotto:

1. Assicurati che il certificato abbia l&#39;estensione pem. In caso contrario, convertirlo in formato PEM. Puoi effettuare la conversione utilizzando *OpenSSL*.
1. Conferma che il certificato inizi con **&quot;—BEGIN CERTIFICATE—&quot;**.
1. Copia il testo del certificato in un decodificatore online, ad esempio https://www.sslshopper.com/certificate-decoder.html o https://www.entrust.net/ssl-technical/csr-viewer.cfm.
In alternativa, è possibile utilizzare il comando *OpenSSL* localmente su un computer Linux. Per ulteriori informazioni, consulta [questa pagina esterna](https://www.shellhacks.com/decode-ssl-certificate/).
1. Assicurati che il certificato venga risolto correttamente, compresi il Nome comune, la SAN, l&#39;emittente e il periodo di validità.
1. Se la verifica del certificato SSL ha esito positivo, verifica che il certificato corrisponda alla CSR utilizzando [questo sito web](https://www.sslshopper.com/certificate-key-matcher.html): seleziona **Controlla se una CSR e un certificato corrispondono**, quindi inserisci il certificato e la CSR nei campi corrispondenti. Dovrebbero corrispondere.

### Passaggio 5: richiesta dell’installazione del certificato SSL

* Se hai accesso al [Pannello di controllo Campaign](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html), segui le istruzioni su [questa pagina](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html#installing-ssl-certificate) per caricare il certificato sul Pannello di controllo Campaign.

* In caso contrario, crea un altro ticket di supporto tramite https://adminconsole.adobe.com/ per richiedere l’Adobe di installare il certificato sui server di Adobe.

Dovrai fornire:

* Il file del certificato, il certificato radice ed eventuali certificati intermedi (allegati al ticket), preferibilmente in formato Apache PEM.
* Numero del ticket di supporto precedente raccolto per la CSR.
* Gli stessi dati forniti per il ticket CSR (tra cui Nome comune, URL istanza, Stato, Città/Località, Nome organizzazione, Nome unità organizzazione, ecc.).

### Passaggio 6: verifica l’installazione del certificato SSL

Una volta installato e confermato il certificato SSL dall’Assistenza clienti Adobe, assicurati che sia stato installato correttamente per tutti gli URL.

Esegui i test seguenti prima di chiudere il ticket di installazione SSL. Assicurati anche di aggiornare qualsiasi configurazione specifica come indicato in [questa sezione](#update-configuration).

Passa ai seguenti URL nel browser (sostituisci &quot;subdomain.customer.com&quot; con il tuo sottodominio):

* https://subdomain.customer.com/r/test (solo per i sottodomini [applicazioni web](https://experienceleague.adobe.com/docs/campaign-classic/using/designing-content/web-applications/about-web-applications.html) - non si applica ai sottodomini di posta elettronica)
* https://t.subdomain.customer.com/r/test
* https://m.subdomain.customer.com/r/test
* https://res.subdomain.customer.com/r/test

Un risultato positivo fornisce informazioni sull’ambiente e la barra degli indirizzi nell’URL indica che la connessione è protetta. Ad esempio, puoi visualizzare il seguente messaggio in Google Chrome:

![](../../help/assets/ssl-google-successful-result.png)

Se il certificato SSL non è installato correttamente, viene visualizzato il seguente avviso:

![](../../help/assets/ssl-google-unsuccessful-result.png)

### Passaggio 7: controllare il periodo di validità del certificato

Puoi controllare il periodo di validità del certificato nel browser. Ad esempio, in Google Chrome, fai clic su **Protetto** > **Certificato**.

È tua responsabilità controllare il periodo di validità. L’Adobe consiglia di implementare un processo per monitorare la scadenza dei certificati. Ulteriori informazioni su cosa succede quando il certificato SSL scade in [questo articolo](https://www.thesslstore.com/blog/what-happens-when-your-ssl-certificate-expires/).

* Crea un ticket di supporto per richiedere un certificato aggiornato almeno due settimane prima della data di scadenza del certificato. Non è necessario richiedere un’ulteriore CSR, a meno che i dettagli della CSR non siano cambiati.

* Se disponi dell&#39;accesso al [Pannello di controllo Campaign](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html) e se l&#39;ambiente è ospitato da Adobe in un ambiente AWS, puoi utilizzare il Pannello di controllo Campaign per rinnovare il certificato prima della scadenza. [Ulteriori informazioni](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-ssl-certificates.html#monitoring-certificates).

### Passaggio 8: aggiornare qualsiasi configurazione specifica {#update-configuration}

Una volta certi che i certificati SSL richiesti sono installati correttamente, è possibile aggiornare tutti i riferimenti in Adobe Campaign da HTTP a HTTPS.

>[!NOTE]
>
>Per Campaign Classic, gli URL da aggiornare si trovano principalmente nella [procedura guidata di distribuzione](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/initial-configuration/deploying-an-instance.html#deployment-wizard) e negli [account esterni](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/accessing-external-database/external-accounts.html) (monitoraggio, pagina speculare e domini di risorse pubbliche). Per Campaign Standard, consulta [Configurazione del branding](https://experienceleague.adobe.com/docs/campaign-standard/using/administrating/application-settings/branding.html#about-brand-identity).

Una volta aggiornate le configurazioni, le nuove e-mail verranno inviate con URL HTTPS anziché HTTP. Per verificare che gli URL siano ora protetti, puoi eseguire rapidamente i seguenti test:

* Carica un’immagine da Adobe Campaign. Una volta caricata l’immagine, l’URL restituito deve essere HTTPS.
* Crea un’e-mail di prova con un collegamento a una pagina speculare, alcune immagini, del testo e un collegamento di annullamento all’abbonamento. Invia l’e-mail a un ID e-mail esterno (ad esempio l’indirizzo Gmail). Una volta ricevuta, apri l’e-mail e assicurati che tutti i collegamenti presenti nell’e-mail si aprano correttamente nel modulo HTTPS (non HTTP), senza avvisi o errori di certificato SSL.

## Risorse specifiche per i prodotti

**Campaign Classic**

* [Pannello di controllo Campaign: Aggiunta di certificati SSL (esercitazione)](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html)  - Scopri come aggiungere certificati SSL per proteggere i sottodomini.

**Campaign Standard**

* [Pannello di controllo Campaign: Aggiunta di certificati SSL (esercitazione)](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html)  - Scopri come aggiungere certificati SSL per proteggere i sottodomini.
