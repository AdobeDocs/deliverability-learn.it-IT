---
source-git-commit: 0332be5688f9d0375d1dba97c39a87d0e8d28c52
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 7%

---
# Creazione della regola di tipologia per supportare l’annullamento dell’iscrizione con un solo clic:

**1. Crea la nuova regola di tipologia:**
* Dalla struttura di navigazione, fai clic su &quot;Nuovo&quot; per creare una nuova tipologia

![immagine](/help/assets/CreatingTypologyRules1.png)

**2. Procedi con la configurazione della regola di tipologia:**
* Tipo di regola: controllo
* Fase: all’inizio del targeting
* Canale: e-mail
* Livello: scelta
* Attivo


![immagine](/help/assets/CreatingTypologyRules2.png)


**Crea un codice JavaScript per la regola di tipologia:**


>[!NOTE]
>
>Il codice descritto di seguito deve essere utilizzato solo come esempio.
>In questo esempio viene descritto come:
>* Configura un URL con il comando Annulla sottoscrizione elenco e aggiungi le intestazioni o aggiungi i parametri mailto: esistenti e sostituiscili con: &lt;mailto..>>, https://.
>* Aggiungi nell’intestazione List-Unsubscribe-Post
>L’esempio di URL post utilizza var headerUnsubUrl = &quot;https://campmomentumv7-mkt-prod3.campaign.adobe.com/webApp/unsubNoClick?id=&lt;%= recipient.cryptedId %>&quot;÷
>* È possibile aggiungere altri parametri (come &amp;service = ...)
>


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


![immagine](/help/assets/CreatingTypologyRules3.png)

**3. Aggiungi la nuova regola a una tipologia in un messaggio e-mail (la tipologia predefinita è ok):**

![immagine](/help/assets/CreatingTypologyRules4.png)

**4. Prepara una nuova consegna (verifica che le intestazioni SMTP aggiuntive nella proprietà di consegna siano vuote)**

![immagine](/help/assets/CreatingTypologyRules5.png)

**5. Verifica durante la preparazione della consegna che la nuova Regola di tipologia sia applicata.**

![immagine](/help/assets/CreatingTypologyRules6.png)



**6. Verifica che sia presente l’opzione Annulla sottoscrizione elenco.**

![immagine](/help/assets/CreatingTypologyRules7.png)
