---
title: XFA-stöd i XDP-baserade adaptiva formulär
description: Visar XFA-händelser, egenskaper, skript och validering som stöds i adaptiva formulär.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop
docset: aem65
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 4efa0c3f-f628-4168-88c9-ba5d0ce075ff
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '732'
ht-degree: 0%

---

# XFA-stöd i XDP-baserade adaptiva formulär{#xfa-support-in-xdp-based-adaptive-forms}

## Introduktion {#introduction}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

Adaptiva formulär har stöd för olika XFA-händelser, egenskaper, skript och valideringar som definieras i en XDP-fil, inklusive:

* Körning av skript som definierats för händelser i XDP-filen.
* Hämta standardvärden och beteendeegenskaper för fält i XDP-filen.
* Körning av valideringsskript som definierats i XDP-filen.

När ett anpassat formulär skapas baserat på en XDP-fil fylls egenskaperna, händelserna och valideringarna i automatiskt i användargränssnittet för formulärutveckling. Formulärförfattare kan dock åsidosätta några av dessa element för att skapa en alternativ upplevelse.

I den här artikeln listas XFA-händelser, egenskaper och valideringar som stöds i anpassningsbara formulär och hur du åsidosätter dem i anpassningsbara formulär.

## XFA-element som stöds och deras mappning i adaptiva formulär {#supported-xfa-elements-and-their-mapping-in-adaptive-forms-br}

### Fält {#fields}

När ett adaptivt formulär skapas med en XDP-fil kan du dra och släppa ett XFA-fält på det adaptiva formuläret. I följande tabell visas hur XFA-fält mappas till adaptiva formulärfält.

<table>
 <tbody>
  <tr>
   <td><p><strong>XFA-fält eller -behållare</strong></p> </td>
   <td><p><strong>Motsvarande adaptiv formulärkomponent</strong></p> </td>
  </tr>
  <tr>
   <td><p>Knapp </p> </td>
   <td><p>Knapp</p> </td>
  </tr>
  <tr>
   <td><p>Kryssruta </p> </td>
   <td><p>Kryssruta</p> </td>
  </tr>
  <tr>
   <td><p>Listruta </p> </td>
   <td><p>Listruta</p> </td>
  </tr>
  <tr>
   <td><p>Datum-/tidsfält </p> </td>
   <td><p>Datumväljaren</p> </td>
  </tr>
  <tr>
   <td><p>Signature Scribble</p> </td>
   <td><p>Klottersignatur</p> </td>
  </tr>
  <tr>
   <td><p>Numeriskt fält </p> </td>
   <td><p>Numerisk ruta</p> </td>
  </tr>
  <tr>
   <td><p>Decimalfält</p> </td>
   <td><p>Numerisk ruta</p> </td>
  </tr>
  <tr>
   <td><p>Textfält </p> </td>
   <td><p>Textruta</p> </td>
  </tr>
  <tr>
   <td><p>Lösenordsfält </p> </td>
   <td><p>Lösenordsruta</p> </td>
  </tr>
  <tr>
   <td><p>Bild</p> </td>
   <td><p>Bild</p> </td>
  </tr>
  <tr>
   <td><p>Text</p> </td>
   <td><p>Text</p> </td>
  </tr>
  <tr>
   <td><p>Delformulär </p> </td>
   <td><p>Panel</p> </td>
  </tr>
  <tr>
   <td><p>Område (grupp)</p> </td>
   <td><p>Panel</p> </td>
  </tr>
  <tr>
   <td><p>Delformulärsuppsättning </p> </td>
   <td><p>Panel</p> </td>
  </tr>
 </tbody>
</table>

### Egenskaper {#properties}

Följande tabell visar hur olika XFA-skript som definieras i XDP-filerna fungerar i anpassningsbara formulär.

<table>
 <tbody>
  <tr>
   <td><p><strong>XFA-komponentegenskaper</strong></p> </td>
   <td><p><strong>Motsvarande beteende i anpassningsbara formulär</strong></p> </td>
  </tr>
  <tr>
   <td><p>somExpression </p> </td>
   <td><p>Mappad till egenskapen Bind reference (bindRef) i adaptiv form.</p> </td>
  </tr>
  <tr>
   <td><p>närvaro </p> </td>
   <td><p>Mappas till egenskapen visible i adaptiv form. Du kan åsidosätta den med synlighetsuttrycket.</p> </td>
  </tr>
  <tr>
   <td><p>åtkomst </p> </td>
   <td><p>Mappas till egenskapen enabled i adaptiv form. Du kan åsidosätta den med Access-uttrycket.</p> </td>
  </tr>
  <tr>
   <td><p>Tillgänglighet: roll </p> </td>
   <td><p>Mappad till rollegenskapen i adaptiv form.</p> </td>
  </tr>
  <tr>
   <td><p>Tillgänglighet: TalkPriority </p> </td>
   <td><p>Mappad till egenskapen talkPriority i adaptiv form.</p> </td>
  </tr>
  <tr>
   <td><p>Hjälpmedel: talkText</p> </td>
   <td><p>Mappas till den anpassade hjälpmedelstexten i anpassad form.</p> </td>
  </tr>
  <tr>
   <td><p>Tillgänglighet: toolTip </p> </td>
   <td><p>Mappad till egenskapen short description i adaptiv form.</p> </td>
  </tr>
  <tr>
   <td><p>bildtext <em> (alla fälttyper)</em></p> </td>
   <td><p>Mappad till egenskapen Title i anpassad form.</p> </td>
  </tr>
  <tr>
   <td><p>displayFormat<em> (alla fälttyper)</em></p> </td>
   <td><p>Mappas till visningsmönstret i anpassad form.</p> </td>
  </tr>
  <tr>
   <td><p>rawValue<em> (alla fälttyper)</em></p> </td>
   <td><p>Mappad till värdeegenskap i adaptiv form.</p> </td>
  </tr>
  <tr>
   <td><p>objekt<em> (listruta, kryssruta)</em></p> </td>
   <td><p>Mappad till alternativegenskap i adaptiv form. Du kan åsidosätta den med hjälp av uttrycket Alternativ.</p> </td>
  </tr>
  <tr>
   <td><p>maxChar<em> (textfält)</em></p> </td>
   <td><p>Mappas till egenskapen Maximum för tillåtna tecken i adaptiv form.</p> </td>
  </tr>
  <tr>
   <td><p>multiline<em> (textfält)</em></p> </td>
   <td><p>Mappad till egenskapen Tillåt flera rader i adaptiv form.</p> </td>
  </tr>
  <tr>
   <td><p>fracDigit<em> (numeriskt fält, decimalfält)</em></p> </td>
   <td><p>Mappas till egenskapen Frac digits i adaptiv form.</p> </td>
  </tr>
  <tr>
   <td><p>leadDigit<em> (numeriskt fält, decimalfält)</em></p> </td>
   <td><p>Mappas till egenskapen Leadsiffror i adaptiv form.</p> </td>
  </tr>
  <tr>
   <td><p>multiSelect<em> (listruta)</em></p> </td>
   <td><p>Mappad till Tillåter flera markeringsegenskaper i adaptiv form.</p> </td>
  </tr>
 </tbody>
</table>

### Skript {#scripts}

Följande tabell visar hur olika XFA-skript som definieras i XDP-filen fungerar i anpassningsbara formulär.

<table>
 <tbody>
  <tr>
   <td><p><strong>XFA-skripthändelser</strong></p> </td>
   <td><p><strong>Motsvarande beteende i anpassningsbara formulär</strong></p> </td>
  </tr>
  <tr>
   <td><p>initialize </p> </td>
   <td><p>Det här skriptet körs under körning och kan inte åsidosättas i adaptiv form.</p> </td>
  </tr>
  <tr>
   <td><p>beräkna</p> </td>
   <td><p>Mappad till beräkningsuttrycket i adaptiv form.</p> </td>
  </tr>
  <tr>
   <td><p>validera </p> </td>
   <td><p>Mappas till valideringsuttrycket i adaptiv form.</p> </td>
  </tr>
  <tr>
   <td><p>validationState </p> </td>
   <td><p>Det här skriptet körs under körning och kan inte åsidosättas i adaptiv form.<br /> </p> </td>
  </tr>
  <tr>
   <td><p>avsluta </p> </td>
   <td><p>Det här skriptet körs under körning och kan inte åsidosättas i adaptiv form.</p> </td>
  </tr>
  <tr>
   <td><p>klicka (knappfält)</p> </td>
   <td><p>Mappas till knappens Click-uttryck.</p> </td>
  </tr>
  <tr>
   <td><p>Stöd för serverskript</p> </td>
   <td><p>Det här skriptet körs under körning och kan inte åsidosättas i adaptiv form.</p> </td>
  </tr>
  <tr>
   <td><p>Stöd för webbtjänster</p> </td>
   <td><p>Det här skriptet körs under körning och kan inte åsidosättas i adaptiv form.</p> </td>
  </tr>
  <tr>
   <td><p>Ändra (klotterfält, alternativknapp, kryssruta)</p> </td>
   <td><p>Det här skriptet körs under körning och kan inte åsidosättas i adaptiv form.</p> </td>
  </tr>
 </tbody>
</table>

### Valideringar {#validations}

Följande tabell visar hur XFA-valideringar mappas till valideringar i adaptiva formulär.

<table>
 <tbody>
  <tr>
   <td><p><strong>XFA-validering</strong></p> </td>
   <td><p><strong>Motsvarande validering i adaptiv form</strong></p> </td>
  </tr>
  <tr>
   <td><p>Valideringsmönster (formatTest)</p> </td>
   <td><p>validatePictureClause</p> </td>
  </tr>
  <tr>
   <td><p>Meddelande för valideringsmönster (formatTestMessage)</p> </td>
   <td><p>validatePictureMessage</p> </td>
  </tr>
  <tr>
   <td><p>Obligatoriskt (nullTest)</p> </td>
   <td><p>obligatoriskt </p> </td>
  </tr>
  <tr>
   <td><p>Tomt meddelande (nullTestMessage) </p> </td>
   <td><p>mandatoryMessage</p> </td>
  </tr>
  <tr>
   <td><p>Validera skript (scriptTest)</p> </td>
   <td><p>validateExp</p> </td>
  </tr>
  <tr>
   <td><p>Meddelande för valideringsskript (scriptTestMessage)</p> </td>
   <td><p>validateMessage</p> </td>
  </tr>
 </tbody>
</table>

>[!NOTE]
>
>Du kan inte åsidosätta den obligatoriska egenskapen för alternativknappar för adaptiva formulär och kryssrutegrupper som är bundna till XFA-kontrollknappar.
