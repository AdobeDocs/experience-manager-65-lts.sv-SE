---
title: Söka efter formulär och resurser
description: Du kan söka efter formulär och resurser i din AEM-instans med AEM Search. Med grundläggande och avancerad sökning kan du snabbt hitta dina resurser.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-manager
docset: aem65
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
exl-id: 1e3c4724-9dbd-4e39-a0fc-efe7fd8906cd
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 0%

---

# Söka efter formulär och resurser{#searching-for-forms-and-assets}

Du kan söka efter formulär eller formulärresurser med hjälp av en textsträng eller textsträng tillsammans med jokertecken. Du kan även begränsa sökningen med hjälp av de villkor som finns i olika kategorier på sökpanelen.

När du markerar ett eller flera villkor och även anger en textsträng, returneras skärningspunkten för texten och villkoren som sökresultat. Sökresultaten är lika bra som de metadata för formulär och resurser som finns.

Klicka på ![aem6forms_search](assets/aem6forms_search.png) om du vill visa eller dölja sökpanelen.

## Grundläggande sökning {#basic-search}

En grundläggande sökning är standardsökningen, som körs utan att du behöver ange några filter. En textsökning av metadataegenskaper utförs av AEM Forms.

Om du vill utföra en grundläggande sökning anger du sökfrågan i textfältet och trycker på Retur. Du kan också ange jokertecknet (&#42;) så att det matchar ett valfritt antal tecken.

Adobe Experience Manager söker efter den angivna texten i metadataegenskaperna och returnerar motsvarande resultat. Om du skriver mer än ett ord matchar sökningen hela texten.

Observera följande om grundsökningen:

* Sökningen utförs med hjälp av egenskaperna för metadata för formulär och resurser.
* Om du skriver mer än ett ord matchar sökningen hela texten.
* Sökningen är inte skiftlägeskänslig. När du till exempel skriver `geometrixx` visas resurser med rubrikerna `Geometrixx`, `GEOMETRIXX` och `GeoMetRixx` i sökresultaten.

* Partiella matchningar av ett ord stöds inte. Använd jokertecknet &#42; om du vill söka med partiella strängar. Om sökfrågan däremot matchar ett fullständigt ord visas motsvarande formulär eller resurs.
* Extra blanksteg bevaras och trimmas inte under sökningen. `My form` är till exempel inte samma sökfråga som `My form`.

* Om data- och visningsvärdena för fälten i metadataegenskaperna är olika kan du inte använda visningsvärden som sökparametrar. Du kan till exempel inte söka baserat på en status, till exempel Ändrad eller Publicerad, eftersom dessa egenskaper lagras i ett annat format.

## Avancerad sökning {#advanced-search}

Förutom frågan kan du i sökvillkoren ange vissa sökparametrar för att göra den grundläggande sökningen effektivare och mer fokuserad.

![Sökfält och parametrar eller filter för AEM formulär- och resurssökning](assets/search_forms_assets.png)

Sökfält och parametrar eller filter för AEM formulär- och resurssökning

### Resurssökväg {#asset-path}

Genom att använda filtret för resurssökväg kan du begränsa sökresultaten till den aktuella katalogen. Om alternativet Sök i aktuell katalog inte är markerat innehåller sökresultaten resurser från baskatalogen. Om den aktuella sidan inte är en katalog och alternativet Sök i den aktuella katalogen är markerat, returnerar sökningen de resurser som finns i den överordnade katalogen.

### Ändrad tillgång {#asset-modification}

Välj något av följande alternativ om du vill söka bland alla resurser som har ändrats inom en viss tidsperiod.

| **Alternativ** | **Beskrivning** |
|---|---|
| För två timmar sedan | Sök bland alla resurser som har ändrats under de senaste två timmarna. |
| För en vecka sedan | Sök bland alla resurser som har ändrats under den senaste veckan. |
| För en månad sedan | Sök bland alla resurser som har ändrats under den senaste månaden. |
| För ett år sedan | Sök bland alla resurser som har ändrats under det senaste året. |

### Resursstatus {#asset-status}

Du kan söka efter resurser med hjälp av någon av följande statusar:

* **Publicerad**: Sök i alla resurser som har publicerats och inte ändrats efter publiceringen.

* **Opublicerad**: Sök i alla resurser som aldrig publiceras.

* **Ändrad**: Sök i alla resurser som har ändrats eller inte publicerats efter publiceringen.

### Resurstyp {#asset-type}

Du kan välja valfritt antal resurstyper. Sökningen returnerar en union av alla valda resurstyper.

<table>
 <tbody>
  <tr>
   <th>Alternativ</th> 
   <th>Beskrivning</th> 
  </tr>
  <tr>
   <td>Formulärmall <br /> </td> 
   <td>Sök i alla formulärmallar.<br /> </td> 
  </tr>
  <tr>
   <td>PDF Form</td> 
   <td>Sök i alla PDF-dokument.</td> 
  </tr>
  <tr>
   <td>Dokument</td> 
   <td>Sök i alla dokument.</td> 
  </tr>
  <tr>
   <td>Anpassat formulär <br /> </td> 
   <td>Sök i alla anpassade formulär.</td> 
  </tr>
  <tr>
   <td>Resurs</td> 
   <td>Sök bland alla resurser.<br /> </td> 
  </tr>
 </tbody>
</table>

### Taggar {#tags}

Taggar är etiketter som är kopplade till resurser för identifiering. Vid sökning väljer du ett valfritt antal taggar i listrutan eller lägger till egna taggar, om det behövs. Ett sökresultat innehåller skärningspunkten för de markerade taggarna.
