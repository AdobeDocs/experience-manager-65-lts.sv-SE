---
title: Hämtar aktivitetsvariabler i sammanställnings-URL
description: Hur du återanvänder informationen om en uppgift och skapar en sammanfattande URL för att sammanfatta eller beskriva en uppgift.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---

# Hämtar aktivitetsvariabler i sammanställnings-URL {#getting-task-variables-in-summary-url}

Sammanfattningssidan visar uppgiftsrelaterad information. I den här artikeln beskrivs hur du kan återanvända uppgiftsrelaterad information på sammanfattningssidan.

I den här exempelstrukturen skickar en medarbetare ett ledighetsansökningsformulär. Ansökningsformuläret skickas sedan till den anställdes chef för godkännande.

1. Skapa ett exempel på en HTML-renderare (html.esp) för ResursType **Employees/PtoApplication**.

   Återgivning förutsätter att följande egenskaper ställs in på noden:

   * namn
   * empid
   * orsak
   * varaktighet

   >[!NOTE]
   >
   >Den här återgivaren är mallen för sammanfattningssidor.

   Följande exempelkod för den här renderaren finns i:

   `apps/Employees/PtoApplication/html.esp`

   ```html
   <html>
     <body>
       <table>
       <tbody>
       <tr>
           <td>
               <h3>Employee Name: <%= currentNode.ename %></h3>
               <h3>Employee ID: <%= currentNode.eid %></h3>
               <h3>Leave duration: <%= currentNode.duration %> days</h3>
               <h3>Reason: <%= currentNode.reason %></h3>
           </td>
       </tr>
       </tbody>
       </table>
     </body>
   </html>
   ```

1. Ändra orkestreringen för att extrahera de fyra egenskaperna från skickade formulärdata. Sedan skapas en nod i CRX av typen **Employees/PtoApplication** med egenskaperna ifyllda.

   1. Skapa en process **skapa en PTO-sammanfattning** och använd den som en underprocess innan åtgärden **Tilldela uppgift** i din organisation.
   1. Definiera **employeeName**, **employeeID**, **ptoReason**, **totalDays** och **nodeName** som indatavariabler i den nya processen. Dessa variabler skickas som skickade formulärdata.

      Definiera också en utdatavariabel, **ptoNodePath**, som används när sammanfattnings-URL anges.

   1. I **skapa PTO-sammanfattningsprocessen** använder du komponenten **set value** för att ange indatainformation i en **nodeProperty**(**nodeProps**)-mappning.

      Tangenterna på kartan ska vara samma som tangenterna som definierades i HTML renderare i föregående steg.

      Lägg även till en **sling:resourceType**-nyckel med värdet **Employees/PtoApplication** i kartan.

   1. Använd underprocessen **storeContent** från tjänsten **ContentRepositoryConnector** i processen **skapa PTO-sammanfattning**. Den här delprocessen skapar en CRX-nod.

      Den har tre indatavariabler:

      * **Mappsökväg**: Sökvägen till den nya CRX-noden. Ange sökvägen som **/content**.
      * **Nodnamn**: Tilldela indatavariabeln nodeName till det här fältet. Detta är en unik nodnamnssträng.
      * **Nodtyp**: Definiera typen som **inte:ostrukturerad**. Utdata för den här processen är nodePath. nodePath är CRX-sökvägen för den nya noden. ndoePath skulle vara det slutliga resultatet av sammanfattningsprocessen **skapa PTO**.

   1. Skicka skickade formulärdata (**employeeName**, **employeeID**, **ptoReason** och **totalDays**) som indata till den nya processen **skapa PTO-sammanfattning**. Ta utdata som **ptoSummaryNodePath**.

1. Definiera sammanfattnings-URL:en som ett XPath-uttryck som innehåller serverinformationen tillsammans med **ptoSummaryNodePath**.

   XPath: `concat('https://[*server*]:[*port*]/lc',/process_data/@ptoSummaryNodePath,'.html')`.

När du öppnar en uppgift i AEM Forms-arbetsytan öppnas CRX-noden i sammanfattnings-URL:en, och HTML-återgivaren visar sammanfattningen.

Sammanfattningslayouten kan ändras utan att processen ändras. Sammanfattningen visas korrekt i HTML renderare.
