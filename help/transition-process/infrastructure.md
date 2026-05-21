---
title: Infrastruttura
description: Scopri cosa è necessario per creare correttamente un’infrastruttura e-mail.
topics: Deliverability
jira: KT-7052
thumbnail: kt7052.jpg
doc-type: article
activity: understand
role: Admin, Leader
level: Beginner
team: ACS
exl-id: 4025d95c-cc77-4e0c-9904-aaf60019b18c
TQID: https://experienceleague.adobe.com/FWlVtNGACEM6dKsnYQJU-z04mP902M5EXZmxxsKDyqU
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
  - id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87
  - id: dfc56824-e8b9-499e-85d4-21aedb507314
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 75df8537199680e5f1fc4b98cefdb05220fee7bf
workflow-type: tm+mt
source-wordcount: 923
ht-degree: 2%

---

# Infrastruttura

Il successo della consegna dei messaggi dipende da una solida base. L’infrastruttura e-mail è un elemento fondamentale. Un’infrastruttura e-mail costruita correttamente include più componenti, ovvero domini e indirizzi IP. Questi componenti sono come il meccanismo alla base delle e-mail che invii e spesso sono l’ancora della reputazione di invio. I consulenti di recapito messaggi garantiscono che questi elementi siano configurati correttamente durante l’implementazione, ma a causa dell’elemento reputazione è importante che tu disponga di queste conoscenze di base.

## Configurazione e strategia del dominio {#domain-setup-and-strategy}

I tempi sono cambiati e alcuni ISP (come Gmail e Yahoo) ora incorporano la reputazione del dominio come punto aggiuntivo quando si tratta di allegare la reputazione e-mail a un mittente. La reputazione del dominio si basa sul dominio di invio anziché sull’indirizzo IP. Ciò significa che il tuo marchio ha la precedenza nelle decisioni di filtraggio degli ISP.

Parte del processo di onboarding per i nuovi mittenti sulle piattaforme Adobe include la configurazione dei domini di invio e la verifica che l’infrastruttura sia stabilita correttamente. Devi collaborare con un esperto per individuare i domini che intendi utilizzare nel lungo periodo. Di seguito sono riportati alcuni suggerimenti per definire una buona strategia di dominio:

* Sii il più chiaro e riflessivo possibile sul marchio con il dominio scelto in modo che gli utenti non identifichino erroneamente la posta come spam. Alcuni esempi sono newsletter.foo.com, receipts.foo.com e così via.
* Non utilizzare il dominio principale o aziendale in quanto potrebbe influire sulla consegna della posta dall’organizzazione agli ISP.
* Prendi in considerazione l’utilizzo di un sottodominio del dominio principale per legittimare il dominio di invio.
* Separa i sottodomini per le categorie dei messaggi transazionali e di marketing. Questo aiuterà il tuo flusso di traffico e-mail su una base più affidabile, in quanto gli ISP cercano questo metodo di invio, che è una best practice per le e-mail nota ed è altamente consigliato.

## Strategia IP {#ip-strategy}

È importante creare una strategia di IP ben strutturata per contribuire a stabilire una reputazione positiva. Il numero di IP e la configurazione variano a seconda del modello di business e degli obiettivi di marketing. Collabora con un esperto per sviluppare una strategia chiara per iniziare subito. Considera questi aspetti importanti da notare:

* **Troppi IP** possono causare problemi di reputazione in quanto si tratta di una tattica comune degli spammer per **snowshoe**, una tattica utilizzata dagli spammer in cui il traffico viene distribuito su molti IP per massimizzare la consegna di posta indesiderata. Anche se non sei uno spammer, potresti sembrarne uno se utilizzi troppi IP, soprattutto se quegli IP non hanno avuto traffico in precedenza.
* **Un numero insufficiente di IP** può causare problemi di velocità effettiva e potenzialmente causare problemi di reputazione. Il throughput varia a seconda dell&#39;ISP. La quantità e la rapidità con cui un ISP è disposto ad accettare dipendono in genere dall’infrastruttura e dalle soglie di reputazione di invio.
* La separazione del traffico per i tipi di messaggistica è fondamentale. È importante, come minimo, separare la posta di marketing e quella transazionale su pool IP separati.
* A seconda della strategia di posta, se la tua reputazione è drasticamente diversa, potrebbe essere consigliabile separare prodotti o flussi di marketing diversi su pool IP diversi. Alcuni esperti di marketing segmentano anche per area geografica. Separare l’IP per il traffico con una reputazione inferiore non corregge il problema di reputazione, ma evita i problemi con le consegne e-mail di reputazione &quot;buona&quot;. Dopo tutto, non si vuole sacrificare il proprio buon pubblico per uno più rischioso.

## Cicli di feedback {#feedback-loops}

Dietro le quinte, le piattaforme Adobe elaborano i dati relativi a mancati recapiti, reclami, annullamenti di abbonamenti e altro ancora. La configurazione di questi cicli di feedback è un aspetto importante per il recapito dei messaggi. I reclami possono danneggiare la reputazione, pertanto devi inviare un’e-mail agli indirizzi che registrano i reclami del pubblico di destinazione. È importante notare che Gmail non restituisce questi dati. Le intestazioni per l’annullamento dell’iscrizione agli elenchi e i filtri di coinvolgimento sono particolarmente importanti per gli abbonati Gmail, che ora costituiscono la maggior parte dei database di abbonati.

## Autenticazione {#authentication}

L’autenticazione è il processo utilizzato dagli ISP per convalidare l’identità di un mittente. I due protocolli di autenticazione più comuni sono [!DNL Sender Policy Framework] (SPF) e [!DNL DomainKeys Identified Mail] (DKIM). Questi messaggi non sono visibili all’utente finale, ma aiutano gli ISP a filtrare i messaggi e-mail dai mittenti verificati. [!DNL Domain-based Message Authentication Reporting and Conformance] (DMARC) sta guadagnando popolarità, anche se i suoi criteri non sono ancora incorporati da tutti gli ISP nei loro sistemi di reputazione.

### SPF

[!DNL Sender Policy Framework] (SPF) è un metodo di autenticazione che consente al proprietario di un dominio di specificare i server di posta utilizzati per inviare posta da tale dominio.

### DKIM

[!DNL Domain Keys Identified Mail] (DKIM) è un metodo di autenticazione utilizzato per rilevare indirizzi di mittente contraffatti (comunemente denominato spoofing). Se DKIM è abilitato, il destinatario potrà confermare se il mittente è autorizzato a inviare messaggi da tale dominio.

### DMARC

[!DNL Domain-based Message Authentication, Reporting and Conformance] (DMARC) è un metodo di autenticazione che consente ai proprietari del dominio di proteggere il proprio dominio da utilizzi non autorizzati. DMARC utilizza SPF o DKIM o entrambi per consentire al proprietario di un dominio di controllare ciò che accade ai messaggi che non riescono a eseguire l’autenticazione: recapitati, messi in quarantena o rifiutati.

## Risorse specifiche per i prodotti

**Campaign**

* Scopri come delegare completamente un sottodominio a Adobe Campaign Classic o Standard in [questa sezione](/help/additional-resources/ac-domain-name-setup.md).
* [Pannello di controllo Campaign: delega completa dei sottodomini (tutorial)](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html?lang=it) - *Scopri come delegare completamente un sottodominio a Adobe Campaign Classic.*
* [Pannello di controllo Campaign: delega completa dei sottodomini (tutorial)](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html?lang=it) - *Scopri come delegare completamente un sottodominio ad Adobe Campaign Standard.*
* Ulteriori informazioni sull&#39;implementazione di un ciclo di feedback per un&#39;istanza di Campaign Classic in [questa sezione](/help/additional-resources/acc-technical-recommendations.md#feedback-loop-acc).

## Risorse aggiuntive

* Ulteriori informazioni sui metodi di autenticazione di SPF, DKIM e DMARC sono disponibili in [questa sezione](/help/additional-resources/authentication.md).
* Ulteriori informazioni su come aumentare la reputazione e-mail con la preparazione degli indirizzi IP in [questa sezione](/help/additional-resources/increase-reputation-with-ip-warming.md).
