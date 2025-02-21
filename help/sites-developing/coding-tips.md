---
title: Kodningstips
description: Lär dig några tips om hur du kodar de effektivaste strategierna i Adobe Experience Manager.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: best-practices
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '857'
ht-degree: 0%

---

# Kodningstips{#coding-tips}

## Använd taglibs eller HTL så mycket som möjligt {#use-taglibs-or-htl-as-much-as-possible}

Om du inkluderar skript i JSP:er blir det svårt att felsöka problem i koden. Genom att inkludera skriptlets i JSP:er är det dessutom svårt att skilja affärslogik från visningslagret, vilket är ett brott mot principen om ett enda ansvar och designmönstret för MVC.

### Skriv läsbar kod {#write-readable-code}

Koden skrivs en gång, men läses många gånger. Om du lägger lite tid på att städa koden som är skriven lönar du dig utdelningar längs vägen när du och andra utvecklare läser den senare.

### Välj namn som ska avslöjas {#choose-intention-revealing-names}

Helst behöver inte en annan programmerare öppna en modul för att förstå vad den gör. På samma sätt bör de kunna avgöra vad en metod gör utan att läsa den. Ju bättre du kan prenumerera på dessa idéer, desto enklare är det att läsa koden och desto snabbare kan du skriva och ändra koden.

Följande konventioner används i kodbasen i AEM:


* En enskild implementering av ett gränssnitt heter `<Interface>Impl`, det vill säga `ReaderImpl`.
* Flera implementeringar av ett gränssnitt har namnet `<Variant><Interface>`, det vill säga `JcrReader` och `FileSystemReader`.
* Abstrakta basklasser har namnet `Abstract<Interface>` eller `Abstract<Variant><Interface>`.
* Paket har namnet `com.adobe.product.module`. Varje Maven-artefakt eller OSGi-paket måste ha ett eget paket.
* Java™-implementeringar placeras i ett impl-paket under deras API.


Dessa konventioner gäller inte nödvändigtvis för kundimplementeringar, men det är viktigt att konventionerna definieras och följs så att koden kan bevaras.

Helst borde namn visa sin avsikt. Ett vanligt kodtest för när namn inte är så tydliga som de ska vara är att det finns kommentarer som förklarar vad variabeln eller metoden är till för:

<table>
 <tbody>
  <tr>
   <td><p><strong>Oskarp</strong></p> </td>
   <td><p><strong>Rensa</strong></p> </td>
  </tr>
  <tr>
   <td><p>int d; //förfluten tid i dagar</p> </td>
   <td><p>int elapsedTimeInDays;</p> </td>
  </tr>
  <tr>
   <td><p>//get tagged images<br /> public List getItems() {}</p> </td>
   <td><p>public List getTaggedImages() {}</p> </td>
  </tr>
 </tbody>
</table>

### Upprepa inte  {#don-t-repeat-yourself}

DRY anger att samma koduppsättning aldrig ska dupliceras. Detta gäller även för exempelvis stränglitteraler. Kodduplicering öppnar dörren för defekter när något måste ändras och bör sökas ut och elimineras.

### Undvik nakna CSS-regler {#avoid-naked-css-rules}

CSS-reglerna ska vara specifika för målelementet i programmets sammanhang. En CSS-regel som tillämpas på *.content.center* skulle till exempel vara alltför bred och skulle kunna påverka mycket innehåll i hela systemet, vilket kräver att andra åsidosätter den här stilen i framtiden. *.myapp-centertext* skulle vara en mer specifik regel eftersom den anger centrerad *text* i programmets sammanhang.

### Eliminera användning av inaktuella API:er {#eliminate-usage-of-deprecated-apis}

När ett API är inaktuellt är det alltid bättre att hitta det nya rekommenderade sättet i stället för att förlita sig på det inaktuella API:t. Detta ger smidigare uppgraderingar i framtiden.

### Skriv lokaliserbar kod {#write-localizable-code}

Alla strängar som inte tillhandahålls av en författare bör kapslas in i ett anrop till AEM i18n-ordlista via *I18n.get()* i JSP/Java och *CQ.I18n.get()* i JavaScript. Den här implementeringen returnerar strängen som skickades till den om ingen implementering hittas, vilket ger flexibiliteten att implementera lokalisering efter implementering av funktionerna på det primära språket.

### Escape-resurssökvägar för säkerhet {#escape-resource-paths-for-safety}

Även om sökvägar i JCR inte får innehålla blanksteg, bör koden inte brytas om de finns. Jackrabbit tillhandahåller en textverktygsklass med metoderna *escape()* och *escapePath()* . För JSP:er visar Granite-gränssnittet en *granite:encodeURIPath() EL* -funktion.

### Använd XSS API och/eller HTML för att skydda mot serveröverskridande skriptattacker (cross-site scripting) {#use-the-xss-api-and-or-htl-to-protect-against-cross-site-scripting-attacks}

AEM har ett XSS-API för att enkelt rensa parametrar och säkerställa säkerheten vid serveröverskridande skriptattacker (cross-site scripting). HTML har dessutom dessa skydd inbyggda direkt i mallspråket. Ett API-kalkylblad finns tillgängligt för hämtning på [Development - Guidelines and Best Practices](/help/sites-developing/dev-guidelines-bestpractices.md).

### Implementera lämplig loggning {#implement-appropriate-logging}

För Java™-kod har AEM stöd för slf4j som standard-API för loggningsmeddelanden och bör användas med de konfigurationer som görs tillgängliga via OSGi-konsolen för att ge en konsekvent administration. Slf4j visar fem olika loggningsnivåer. Adobe rekommenderar att du använder följande riktlinjer när du väljer vilken nivå ett meddelande ska loggas på:

* FEL: När något har brutits i koden och bearbetningen inte kan fortsätta. Detta beror ofta på ett oväntat undantag. Det är praktiskt att ta med stackspår i dessa scenarier.
* VARNING: När något inte har fungerat som det ska, men bearbetningen kan fortsätta. Detta beror ofta på ett undantag som vi förväntade oss, till exempel ett *PathNotFoundException*.
* INFORMATION: Information som kan vara användbar när du övervakar ett system. Tänk på att detta är standardinställningen och att de flesta kunder låter detta vara på plats i sina miljöer. Använd den därför inte för mycket.
* FELSÖK: Information om bearbetning på lägre nivå. Användbart vid felsökning av supportproblem.
* TRACE: Information på lägsta nivå, till exempel genom att ange/avsluta metoder. Detta används vanligtvis bara av utvecklare.

Om det finns JavaScript bör *console.log* endast användas under utvecklingen och alla loggsatser bör tas bort före lanseringen.

### Undvik lasthanteringsprogrammering {#avoid-cargo-cult-programming}

Undvik att kopiera kod utan att förstå vad den gör. Om du är osäker är det alltid bäst att fråga någon som har mer erfarenhet av modulen eller API:t som du inte är säker på.
