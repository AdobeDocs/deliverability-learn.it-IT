---
title: Infrastruttura
description: 'Scopri cosa è necessario per creare correttamente un’infrastruttura e-mail. '
topics: Deliverability
kt: 7052
thumbnail: kt7052.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 4025d95c-cc77-4e0c-9904-aaf60019b18c
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 2%

---

# Infrastruttura

Il successo del recapito di messaggi dipende da una base solida. L’infrastruttura e-mail è un elemento fondamentale. Un’infrastruttura e-mail correttamente costruita include più componenti, ovvero domini e indirizzi IP. Questi componenti sono come il macchinario dietro le e-mail che invii e sono spesso l&#39;ancoraggio di inviare reputazione. I consulenti di recapito assicurano che questi elementi siano impostati correttamente durante l’implementazione, ma a causa dell’elemento reputazione, è importante che tu disponga di questa comprensione di base.

## Configurazione e strategia del dominio {#domain-setup-and-strategy}

I tempi sono cambiati e alcuni ISP (come Gmail e Yahoo) ora incorporano la reputazione del dominio come punto aggiuntivo quando si tratta di allegare la reputazione delle e-mail a un mittente. La reputazione del dominio si basa sul dominio di invio anziché sull’indirizzo IP. Ciò significa che il tuo marchio ha la precedenza quando si tratta di decisioni di filtro ISP.

Parte del processo di onboarding per i nuovi mittenti sulle piattaforme Adobe include la configurazione dei domini di invio e la garanzia che l’infrastruttura sia stabilita correttamente. Devi collaborare con un esperto su quali domini intendi utilizzare a lungo termine. Di seguito sono riportati alcuni suggerimenti che definiscono una buona strategia per il dominio:

* Con il dominio scelto devi essere il più chiaro e riflessivo possibile sul marchio in modo che gli utenti non identifichino erroneamente la posta come spam. Alcuni esempi sono newsletter.foo.com, Receips.foo.com e così via.
* Non devi utilizzare il dominio padre o aziendale in quanto potrebbe influire sulla consegna della posta dalla tua organizzazione agli ISP.
* Prendi in considerazione l’utilizzo di un sottodominio del dominio principale per legittimare il dominio di invio.
* Separa i sottodomini per le categorie di messaggi transazionali e di marketing. Questo aiuterà il tuo flusso di traffico e-mail su una base più affidabile, in quanto gli ISP cercano questo metodo di invio, che è una best practice e-mail nota ed è altamente consigliato.

## Strategia IP {#ip-strategy}

È importante creare una strategia IP ben strutturata per contribuire a creare una reputazione positiva. Il numero di IP e la configurazione varia a seconda del modello aziendale e degli obiettivi di marketing. Collabora con un esperto per sviluppare una strategia chiara per iniziare bene. Considera questi aspetti importanti da notare:

* **Troppi IP** può innescare problemi di reputazione in quanto è una tattica comune di spammer a **scarpa da neve**, che è una tattica utilizzata dagli spammer in cui il traffico viene distribuito su molti IP per massimizzare la consegna della posta indesiderata. Anche se non sei uno spammer, potresti assomigliare a uno se utilizzi troppi IP, specialmente se questi IP non hanno avuto traffico precedente.
* **IP insufficienti** può causare problemi di throughput e potenzialmente causare problemi di reputazione. Il throughput varia a seconda dell&#39;ISP. Quanto e quanto rapidamente un ISP è disposto ad accettare si basa tipicamente sulla sua infrastruttura e l&#39;invio di soglie di reputazione.
* La chiave è la separazione del traffico per i tipi di messaggi. È importante, come minimo, separare le attività di marketing e la posta transazionale su pool IP separati.
* A seconda della strategia della posta, può essere anche consigliabile separare diversi prodotti o flussi di marketing su diversi pool IP se la reputazione è drasticamente diversa. Anche alcuni addetti al marketing si segmentano per regione. Separare l&#39;IP per il traffico con una reputazione inferiore non risolverà il problema della reputazione, ma impedirà problemi con le consegne e-mail di &quot;buona&quot; reputazione. Dopo tutto, non vuoi sacrificare il tuo buon pubblico per uno più rischioso.

## Ciclo di feedback {#feedback-loops}

Dietro le quinte, le piattaforme di Adobe elaborano dati su mancati recapiti, reclami, annullamenti di abbonamenti e altro ancora. La configurazione di questi cicli di feedback è un aspetto importante per il recapito messaggi. I reclami possono danneggiare la reputazione, pertanto devi inviare un messaggio e-mail agli indirizzi che registrano i reclami da parte del pubblico di destinazione. È importante notare che Gmail non fornisce questi dati. Le intestazioni di annullamento dell’abbonamento e il filtro di coinvolgimento sono particolarmente importanti per gli abbonati Gmail, che ora costituiscono la maggior parte dei database di abbonati.

## Autenticazione {#authentication}

L&#39;autenticazione è il processo utilizzato dagli ISP per convalidare l&#39;identità di un mittente. I due protocolli di autenticazione più comuni sono [!DNL Sender Policy Framework] (SPF) e [!DNL DomainKeys Identified Mail] (DKIM). Questi non sono visibili all’utente finale, ma aiutano gli ISP a filtrare le e-mail da mittenti verificati. [!DNL Domain-based Message Authentication Reporting and Conformance] (DMARC) sta guadagnando popolarità, anche se le sue politiche non sono ancora state incorporate da tutti gli ISP nei loro sistemi di reputazione.

### SPF

[!DNL Sender Policy Framework] (SPF) è un metodo di autenticazione che consente al proprietario di un dominio di specificare quali server di posta utilizzano per inviare posta da quel dominio.

### DKIM

[!DNL Domain Keys Identified Mail] (DKIM) è un metodo di autenticazione utilizzato per rilevare gli indirizzi del mittente falsificati (comunemente denominato spoofing). Se DKIM è abilitato, consente al ricevitore di confermare se il mittente è autorizzato a inviare posta da quel dominio.

### DMARC

[!DNL Domain-based Message Authentication, Reporting and Conformance] (DMARC) è un metodo di autenticazione che consente ai proprietari del dominio di proteggere il proprio dominio da un uso non autorizzato. DMARC utilizza SPF o DKIM o entrambi per consentire al proprietario di un dominio di controllare cosa succede alla posta che non riesce l&#39;autenticazione: consegnati, messi in quarantena o rifiutati.

## Risorse specifiche per i prodotti

**Campaign**

* Scopri come delegare completamente un sottodominio a Adobe Campaign Classic o Standard in [questa sezione](/help/additional-resources/ac-domain-name-setup.md).
* [Pannello di controllo Campaign: Delega completa dei sottodomini (esercitazione)](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *Scopri come delegare completamente un sottodominio a Adobe Campaign Classic.*
* [Pannello di controllo Campaign: Delega completa dei sottodomini (esercitazione)](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *Scopri come delegare completamente un sottodominio ad Adobe Campaign Standard.*
* Ulteriori informazioni sull’implementazione di un ciclo di feedback per un’istanza di Campaign Classic in [questa sezione](/help/additional-resources/acc-technical-recommendations.md#feedback-loop-acc).

## Risorse aggiuntive

* Ulteriori informazioni sui metodi di autenticazione SPF, DKIM e DMARC in [questa sezione](/help/additional-resources/authentication.md).
* Ulteriori informazioni sull&#39;aumento della reputazione delle e-mail con il riscaldamento dell&#39;IP in [questa sezione](/help/additional-resources/increase-reputation-with-ip-warming.md).
