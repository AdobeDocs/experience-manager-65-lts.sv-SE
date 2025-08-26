---
title: Konfigurera AI-assistenten i AEM
description: Lär dig hur du konfigurerar och konfigurerar AI-assistenten med Admin Console i Adobe Experience Manager.
solution: Experience Manager
feature: Cloud Manager, Developing
role: Admin, Architect, Developer
source-git-commit: 3cf3d5c83a91fbc582840c2a3b11b3b705f86712
workflow-type: tm+mt
source-wordcount: '1092'
ht-degree: 0%

---

# Konfigurera AI-assistenten i AEM {#aem-ai-asst-admin-setup}

<!-- An Administrator must configure access, permissions, and settings before users in their organization can use the features in AI Assistant in AEM. -->

<!-- badge: label="Beta" type="Positive" -->

Om du vill använda AI Assistant i AEM (Adobe Experience Manager) måste du anmäla dig på Admin Console-nivå. En produktadministratör skapar (eller väljer) en användargrupp och ger den den nya behörigheten AI Assistant. Alla som läggs till i den gruppen får omedelbart tillgång till AI-assistenten i AEM. Om målet är företagsomfattande tillgänglighet tilldelar administratören helt enkelt alla användare till den gruppen.

Ur personalens perspektiv är processen enkel: identifiera produktadministratören för Adobe Experience Manager i din organisation och begär att få läggas till i den AI-aktiverade användargruppen. När du visas i den gruppen visas ikonen Assistant automatiskt nästa gång du loggar in.

Administratörer bör tänka på normal Cloud Manager-styrning. Håll produktadministratörsbehörighet i Admin Console för att skapa profiler, hantera användargrupper eller redigera behörigheter. Om användarna också behöver assistentens inbyggda **Skapa supportbiljett** -funktion lägger du till standardrollen **Support Admin** (Admin Console standardroll) för samma personer eller grupp.

Konfigurationsprocessen för AI-assistenten i AEM består av följande steg:

1. [Skapa en ny produktprofil i Adobe Admin Console](#create-profile).
1. [Aktivera AI Assistant-produktkunskapsbehörighet](#enable-permission).
1. [Skapa en ny användargrupp (eller använd en befintlig användargrupp)](#create-user-group).
1. [Lägg till användare i användargruppen](#add-users).
1. [Tilldela produktprofilen till användargruppen](#assign-product-profile).

**Förutsättningar**

Innan du börjar bör du kontrollera att du uppfyller följande krav:

* Du måste ha minst administratörsbehörighet för produkten i Adobe Admin Console.
* Du har en förståelse för organisationens användarhanteringsstruktur.

**Konfigurationsaspekter**

* Bearbetningstid: Resurser som skapats i Cloud Manager kan ta upp till två minuter att visa i Admin Console för behörighetskonfiguration.
* Flera profiler: Användare kan ingå i flera profiler, och behörigheter kombineras från alla tilldelade profiler.
* Organisationsomfång: Vissa behörigheter kan gälla på organisationsnivå för alla program.
* Fördefinierade profiler: Ta inte bort fördefinierade behörighetsprofiler från Admin Console.


## 1 - Skapa en ny produktprofil i Adobe Admin Console{#create-profile}

1. Följ de detaljerade instruktionerna i [Skapa en ny produktprofil i Adobe Admin Console](https://experienceleague.adobe.com/en/docs/experience-platform/access-control/ui/create-profile) som finns i Experience Platform-dokumentationen.

1. När du skapar den nya produktprofilen kan du använda följande föreslagna värden för AI-assistenten.

   | Textfält | Föreslaget värde |
   | --- | --- |
   | Produktprofilnamn | `AI Assistant in AEM` (eller ditt beskrivande namn) |
   | Visningsnamn (valfritt) | `AI Assistant` |
   | Beskrivning (valfritt) | `Product profile for managing AI Assistant in AEM access` |
   | Meddelande | Konfigurera baserat på organisationens inställningar |


## 2 - Aktivera AI Assistant Product Knowledge-behörighet{#enable-permission}

Processen för att tilldela anpassade behörigheter till produktprofiler följer standardarbetsflödet för anpassade behörigheter i Adobe Cloud Manager.

Referensartikel: [Tilldela anpassade behörigheter till den nya produktprofilen](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-manager/content/requirements/custom-permissions#assign-permissions)

1. I Admin Console klickar du på namnet på den nya produktprofilen (`AI Assistant in AEM`)

   ![Skärmbild](/help/assets/assets-ai/ai-assistant-console.png)

1. Klicka på fliken **Behörigheter** om du vill visa listan över redigerbara behörigheter.

1. Leta reda på behörigheten `AI Assistant Product Knowledge` i tabelllistan.

   ![Behörigheter för AI-assistenten på fliken Admin Console](/help/assets/assets-ai/ai-assistant-permission.png)

1. Klicka på ikonen ![Penna eller Redigera](https://spectrum.adobe.com/static/icons/workflow_18/Smock_Edit_18_N.svg) till höger om behörighetens namn.

1. På sidan **Redigera behörigheter för AI Assistant** aktiverar du alternativet **AI Assistant Product Knowledge**.

   ![Sidan Redigera behörigheter för växlingsalternativet AI Assistant Product Knowledge ](/help/assets/assets-ai/ai-assistant-prod-knowledge.png)

1. Klicka på **Spara** i sidans nedre högra hörn.

   Din produktprofil har nu AI Assistant-produktkunskapsbehörighet aktiverad.


## 3 - Skapa en ny användargrupp (eller använd en befintlig användargrupp){#create-user-group}

1. Gör något av följande:

>[!BEGINTABS]

>[!TAB Skapa en ny användargrupp]

1. I Admin Console klickar du på **Användare** > **Användargrupper**.

   ![Användargrupper](/help/assets/assets-ai/ai-assistant-user-groups.png)

1. Klicka på **Ny användargrupp** på sidan **Användargrupper**.

   ![Knappen Ny användargrupp på sidan Användargrupper](/help/assets/assets-ai/ai-assistant-new-user-group.png)

1. Ange följande information på sidan **Skapa en ny användargrupp**:

   | Alternativ | Föreslaget värde |
   | --- | --- |
   | Användargruppsnamn | `AI Assistant in AEM` (eller ditt namn) |
   | Beskrivning (valfritt) | `User group for managing AI Assistant in AEM access` |

   ![Skapa en ny användargruppsida](/help/assets/assets-ai/ai-assistant-create-new-user-group.png)

1. Klicka på **Spara** i det nedre högra hörnet på sidan.

>[!TAB Använd en befintlig användargrupp]

Du kan använda en befintlig AEM-användargrupp om den uppfyller åtkomstkraven för AI Assistant i stället för att skapa en ny grupp.

>[!ENDTABS]

## 4 - Lägg till användare i användargruppen{#add-users}

1. Gör något av följande:

>[!BEGINTABS]

>[!TAB Lägg till enskilda användare]

1. På sidan **Användargrupper** klickar du i tabellen **Gruppnamn** på det användargruppnamn som du skapade nyligen eller på ett befintligt användargruppnamn.

   ![Sidan Användargrupper visar AI Assistant i AEM-användargruppnamnet i tabellen](/help/assets/assets-ai/ai-assistant-user-group-name-in-table.png)

1. På sidan **Användargrupper** för **AI-assistenten i AEM** klickar du på fliken **Användare** och sedan på **Lägg till användare**.

   ![AI-assistenten på sidan för användargrupper i AEM som visar fliken Användare och knappen Lägg till användare](/help/assets/assets-ai/ai-assistant-add-users.png)

1. På sidan **`Add users to this user group`** söker du efter och väljer användare som behöver åtkomst till AI Assistant i AEM.

   ![Lägg till användare på den här användargruppsidan](/help/assets/assets-ai/ai-assistant-add-users-to-this-group.png)

1. Klicka på **Spara** i sidans nedre högra hörn.
1. [Tilldela produktprofilen till användargruppen](#assign-product-profile).

>[!TAB Lägg till användare i grupp]

Du kan använda massöverföring i Admin Console.

1. Förbered en CSV-fil med användarinformation.
1. Använd alternativet **`Add users by CSV`** för effektiv massutökning.
1. [Tilldela produktprofilen till användargruppen](#assign-product-profile).

>[!ENDTABS]


## 5 - Tilldela produktprofilen till användargruppen{#assign-product-profile}

Det här steget följer Adobe Admin Console standardarbetsflöde för att tilldela produktprofiler till användargrupper.

Referensartikel: [Hantera produktprofiler för företagsanvändare](https://helpx.adobe.com/enterprise/using/manage-product-profiles.html)

1. Klicka på fliken [Tilldelade produktprofiler](#add-users) när du fortfarande är i AI-assistenten i AEM-användargruppen från **4 - Lägg till användare i användargruppen**.
1. Klicka på **Tilldela profil**.

   ![AI-assistenten på AEM användargruppsida med fliken Tilldelade produktprofiler vald](/help/assets/assets-ai/ai-assistant-assign-profile.png)

1. På sidan **Tilldela produkter och profiler** söker du efter och väljer din **AI Assistant**-produktprofil i dialogrutan **Välj produktprofiler**.

   ![Sidan&quot;Tilldela produkter och profiler&quot; med dialogrutan&quot;Välj produktprofiler&quot; och produktprofilen&quot;AI Assistant&quot; vald](/help/assets/assets-ai/ai-assistant-select-product-profile.png)

1. Klicka på **Använd** i dialogrutans nedre högra hörn.
1. Klicka på **Spara** i det nedre högra hörnet på sidan **Tilldela produkter och profiler**.

   ![Den AI Assistant-produktprofil som visas för AI-assistenten i AEM-användargrupp](/help/assets/assets-ai/ai-assistant-profile-assigned-to-user-group.png)


## Verifiera konfigurationen

* Kontrollera att produktprofilen visar rätt antal tilldelade användargrupper.
* Kontrollera att användargruppen visar rätt antal användare.
* Bekräfta att AI Assistant-produktkunskapsbehörigheten är aktiverad och korrekt konfigurerad.


## Testa konfigurationen

Låt en användare från den tilldelade gruppen göra följande:

1. Logga in på AEM.
2. Kontrollera att AI Assistant-funktionerna är tillgängliga.
3. Testa AI-assistentens funktionalitet för att säkerställa korrekt aktivering.

## Se även

* [AI Assistant i AEM](/help/ai-assistant-in-aem.md)
* [Adobe Experience Platform Access Control](https://experienceleague.adobe.com/en/docs/experience-platform/access-control/ui/overview)
<!-- * [Cloud Manager Custom Permissions](/help/implementing/cloud-manager/custom-permissions.md) -->
