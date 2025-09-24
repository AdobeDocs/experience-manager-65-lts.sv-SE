---
title: AEM Forms on OSGi Groups and Privileges
description: Tilldela användare till grupper för att hantera Adobe Experience Manager (AEM) Forms på OSGi
contentOwner: anujkapo
products: SG_EXPERIENCEMANAGER/6.5/FORMS
content-type: reference
topic-tags: Configuration
docset: aem65
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
exl-id: 1681e92b-2d88-4b10-a700-a516aa5a02c8
source-git-commit: 30ec8835be1af46e497457f639d90c1ee8b9dd6e
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# AEM Forms on OSGi Groups and Privileges{#aem-forms-on-osgi-groups-and-privileges}

## Gäller för {#applies-to}

Den här dokumentationen gäller **AEM 6.5 LTS Forms**.

Mer information om AEM as a Cloud Service finns i [AEM Forms på Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/setup-configure-migrate/forms-groups-privileges-tasks.html).

Du kan [skapa grupper](/help/sites-administering/user-group-ac-admin.md#group-administration) och tilldela profiler och [användare](/help/sites-administering/user-group-ac-admin.md#user-administration) till grupper i Adobe Experience Manager (AEM). Dessa profiler styr behörigheterna för de användare som är en del av gruppen.

När du har installerat [AEM Forms-tilläggspaketet](../../forms/using/installing-configuring-aem-forms-osgi.md) blir de grupper som omnämns i den här artikeln automatiskt tillgängliga för tilldelning, till exempel formuläranvändare och användare med formulärbehörighet. I följande tabell visas de uppgifter som en användare kan utföra för AEM Forms på OSGi baserat på grupptilldelningarna:

<table>
 <tbody>
  <tr>
   <td>Grupp</td> 
   <td>Uppgifter</td> 
  </tr>
  <tr>
   <td>forms-users <sup>[1]</sup></td> 
   <td>
    <ul> 
     <li>Skapa, förhandsgranska, publicera och skicka adaptiva formulär</li> 
     <li>Skapa, förhandsgranska och publicera interaktiv kommunikation och dokumentfragment</li> 
     <li>Överföra resurser till en AEM-instans</li> 
     <li>Skapa teman</li> 
    </ul> </td> 
  </tr>
  <tr>
   <td>formgivare</td> 
   <td>
    <ul> 
     <li>Skapa, förhandsgranska, publicera och skicka adaptiva formulär</li> 
     <li>Skapa, förhandsgranska och publicera interaktiv kommunikation och dokumentfragment</li> 
     <li>Skapa skript för adaptiva formulär med en kodredigerare</li> 
     <li>Överför resurser inklusive skript</li> 
     <li>Skapa teman</li> 
     <li>Importera paket som innehåller XDP</li> 
    </ul> </td> 
  </tr>
  <tr>
   <td>formulär-inskickning-granskare</td> 
   <td>
    <ul> 
     <li>Granska inskickat material</li> 
     <li>Godkänn eller avslå inskickade bidrag</li> 
    </ul> </td> 
  </tr>
  <tr>
   <td>template-authors <sup>[2]</sup></td> 
   <td>
    <ul> 
     <li>Skapa och förhandsgranska anpassningsbara formulär eller mallar för interaktiv kommunikation</li> 
    </ul> </td> 
  </tr>
  <tr>
   <td><p>fdm-authors</p> </td> 
   <td>
    <ul> 
     <li>Skapa och ändra en formulärdatamodell</li> 
    </ul> </td> 
  </tr>
  <tr>
   <td>cm-agent-users</td> 
   <td>
    <ul> 
     <li>Få åtkomst till brev från Correspondence Management eller interaktiv kommunikation med hjälp av agentgränssnittet</li> 
    </ul> </td> 
  </tr>
  <tr>
   <td><p>arbetsflödesredigerare</p> </td> 
   <td>
    <ul> 
     <li>Skapa ett inkorgsprogram</li> 
     <li>Skapa en arbetsflödesmodell</li> 
    </ul> </td> 
  </tr>
  <tr>
   <td>arbetsflöde-användare</td> 
   <td>
    <ul> 
     <li>Använd AEM Inbox-program<br /> <strong>Obs! </strong>Du måste ha grupptilldelningar för cm-agent-users och workflow-users för att få åtkomst till gränssnittet för Interactive Communications Agent i AEM Inbox.</li> 
     <li>Hantera arbetsflödesinstanser</li> 
    </ul> </td> 
  </tr>
  <tr>
   <td>fd-administratörer</td> 
   <td>
    <ul> 
     <li>Konfigurera PDF Generator</li> 
     <li>Konfigurera den bevakade mappen</li> 
     <li>Hantera arbetsflödesprogram</li> 
    </ul> </td> 
  </tr>
 </tbody>
</table>

1. Användaren med gruppbehörighet för formuläranvändare kan inte skriva skript för adaptiva formulär.
1. Användaren med gruppbehörighet för mallförfattare kan inte skriva skript för mallar.
