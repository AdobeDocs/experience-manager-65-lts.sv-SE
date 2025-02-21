---
title: 'Självstudiekurs: Planera interaktiv kommunikation'
description: Planera anatomin för interaktiv kommunikation
contentOwner: anujkapo
products: SG_EXPERIENCEMANAGER/6.5/FORMS
feature: Interactive Communication
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '652'
ht-degree: 0%

---

# Självstudiekurs: Planera interaktiv kommunikation {#tutorial-plan-the-interactive-communication}

Planera anatomin för interaktiv kommunikation

![02-create-adaptive-form-main-image](assets/02-create-adaptive-form-main-image.png)

Den här självstudiekursen är ett steg i [Skapa din första interaktiva kommunikationsserie](/help/forms/using/create-your-first-interactive-communication.md). Vi rekommenderar att du följer serien i kronologisk ordning för att förstå, utföra och demonstrera det fullständiga självstudiekurserna.

Det första steget i planeringen av interaktiv kommunikation är att färdigställa innehållet i den interaktiva kommunikationen. Ämnesexperter från avdelningar som juridik, ekonomi, support eller marknadsföring kan hjälpa dig att slutföra innehållet. När innehållet är klart måste du analysera det för att identifiera de olika resurstyper som krävs för att skapa den interaktiva kommunikationen.

## Planeringsöverväganden {#planning-considerations}

En interaktiv kommunikation innehåller följande element:

* **Statisk text** innehåller de flesta delar av den interaktiva kommunikationen som är generiska till sin natur och ingår i kommunikationen med alla kunder. Exempel: sidhuvud, sidfot, hälsning eller friskrivning.
* **Data som har hämtats från ett serverdelssystem (formulärdatamodell)** är kundspecifika och sammanfogas dynamiskt med den interaktiva kommunikationen. Policynumret eller adressen kan till exempel hämtas med hjälp av en formulärdatamodell.
* **Layout eller mallar** för Print- och Web-versionen av Interactive Communication.
* **Ordning** där de olika textstyckena visas i den interaktiva kommunikationen.
* **Data som anges av en frontlinjeanställd (agentanvändargränssnittet)** som anpassar kommunikationen innan den skickas ut. Exempel: betalningsdatum.

* **Villkorliga data** fylls i baserat på fördefinierade villkor. Till exempel datumet då den interaktiva kommunikationen genereras.
* **Bilder som lagras i en databas**, till exempel logotyper och signaturbilder. Bilder som företagslogotyper visas i de flesta eller alla interaktiva kommunikationer.
* **Diagram och tabeller** krävs för att förenkla representationen av komplexa data i en interaktiv kommunikation

## Anatomi i interaktiv kommunikation {#anatomy-of-the-interactive-communication}

När du är klar med innehållet och de element som används för att skapa den interaktiva kommunikationen kan du skapa en beskrivning av den interaktiva kommunikationen. Anatomin måste innehålla de uppgifter som anges i avsnittet [Planeringsöverväganden](/help/forms/using/planning-interactive-communications.md#planning-considerations). Baserat på vårt användningsexempel är följande ett exempel på en anatomi av den månatliga faktura som en telekomoperatör skickar till sina kunder.

Anatomin innehåller data med följande indatalägen:

* Statisk text
* Formulärdatamodell
* Agentgränssnitt
* Villkorliga data
* Bilder

I varje avsnitt representerar fetstilt statisk text. Databasen innehåller kundräkningar och samtalstabeller. En formulärdatamodell kan ta emot data från alla dessa tabeller. Mer information finns i [Skapa formulärdatamodell](/help/forms/using/create-form-data-model0.md).

Följande tabell visar datakällan för varje fält i anatomin Interactive Communication:

<table>
 <tbody>
  <tr>
   <td>Avsnitt</td>
   <td>Statisk text</td>
   <td>FDM </td>
   <td>Agentgränssnitt</td>
   <td>Bilder</td>
  </tr>
  <tr>
   <td>Fakturainformation</td>
   <td><p>Fakturanummer</p> <p>Faktureringsdatum</p> <p>Faktureringsperiod</p> <p>Din plan</p> </td>
   <td><p>Värde för fältet <strong>Din plan </strong></p> <p>Tabell - kund</p> </td>
   <td><p>Värden för följande fält:</p>
    <ul>
     <li>Fakturanummer</li>
     <li>Faktureringsdatum</li>
     <li>Faktureringsperiod</li>
    </ul> <p> </p> </td>
   <td>—</td>
  </tr>
  <tr>
   <td>Kundinformation</td>
   <td><p>Leveransort</p> <p>Statuskod</p> <p>Mobilnummer</p> <p>Alternativt kontaktnummer</p> <p>Relationsnummer</p> <p>Antal anslutningar</p> </td>
   <td><p>Värden för följande fält:</p>
    <ul>
     <li>Namn</li>
     <li>Adress</li>
     <li>Mobilnummer</li>
     <li>Alternativt kontaktnummer</li>
     <li>Relationsnummer</li>
    </ul> <p>Tabell - kund</p> </td>
   <td><p>Värden för följande fält:</p>
    <ul>
     <li>Leveransort</li>
     <li>Statuskod</li>
     <li>Antal anslutningar</li>
    </ul> </td>
   <td>—</td>
  </tr>
  <tr>
   <td>Fakturasammanfattning</td>
   <td><p>Föregående saldo</p> <p>Betalningar</p> <p>Justeringar</p> <p>Aktuell faktureringsperiod för avgifter</p> <p>Belopp att betala</p> <p>Förfallodatum</p> </td>
   <td><p>Värde för fältet <strong>Aktuell faktureringsperiod </strong> för avgifter</p> <p>Tabell - växlar</p> </td>
   <td><p>Värden för följande fält:</p>
    <ul>
     <li>Föregående saldo</li>
     <li>Betalningar</li>
     <li>Justeringar</li>
     <li>Belopp att betala</li>
     <li>Förfallodatum</li>
    </ul> </td>
   <td>—</td>
  </tr>
  <tr>
   <td>Sammanfattning av avgifter</td>
   <td><p>samtalsavgifter</p> <p>Konferenssamtalskostnader</p> <p>SMS-avgifter </p> <p>Mobila internetavgifter</p> <p>National Roaming Charts</p> <p>Internationella roamingavgifter</p> <p>Avgifter för värdeökade tjänster</p> <p>Totala avgifter</p> <p>TOTALT BETALNINGSBART</p> <p>Villkor för fältet Värde för tillagda avgifter</p> </td>
   <td><p>Värden för följande fält:</p>
    <ul>
     <li>samtalsavgifter</li>
     <li>Konferenssamtalskostnader</li>
     <li>SMS-avgifter </li>
     <li>Mobila internetavgifter</li>
     <li>National Roaming Charts</li>
     <li>Internationella roamingavgifter</li>
     <li>Avgifter för värdeökade tjänster</li>
     <li>Totala avgifter (använd debiterade fält)</li>
     <li>TOTALT BETALNINGSBART (använd debiterat fält)</li>
    </ul> <p>Tabell - växlar</p> </td>
   <td>Inga fält</td>
   <td>—</td>
  </tr>
  <tr>
   <td>Specificerade samtal - utgående</td>
   <td><p>Kolumnnamn:</p>
    <ul>
     <li>Datum</li>
     <li>Tid</li>
     <li>Nummer</li>
     <li>Varaktighet</li>
     <li>Avgifter</li>
    </ul> </td>
   <td><p>Alla värden</p> <p>Tabell - anrop</p> </td>
   <td>Inga fält</td>
   <td>—</td>
  </tr>
  <tr>
   <td>Betala nu</td>
   <td>—</td>
   <td>—</td>
   <td>—</td>
   <td>PayNow</td>
  </tr>
  <tr>
   <td>Mervärdestjänster</td>
   <td>—</td>
   <td>—</td>
   <td>—</td>
   <td>ValueAddedServices</td>
  </tr>
 </tbody>
</table>
