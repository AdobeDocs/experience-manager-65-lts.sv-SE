---
title: Kreativt projekt- och PIM-integrering
description: Creative Project effektiviserar hela arbetsflödet för fotografering, inklusive generering av en fotoplåtningsförfrågan, överföring av en fotoplåtning, samarbete i en fotoplåtning och paketering av godkända mediefiler
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: projects
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '2888'
ht-degree: 0%

---


# Kreativt projekt- och PIM-integrering {#creative-project-and-pim-integration}

Om du är marknadsförare eller kreatör kan du använda verktygen i Adobe Experience Manager (AEM) för att hantera e-handelsrelaterade produktfotografier och tillhörande kreativa processer inom organisationen.

Du kan använda Creative Project för att effektivisera följande uppgifter i arbetsflödet för fotografering:

* Generera en begäran om fotofotografering
* Överföra en fototagning
* Samarbeta i en fototagning
* Paketera godkända resurser

>[!NOTE]
>
>Se [Projektanvändarroller för information](/help/sites-authoring/projects.md#user-roles-in-a-project) om hur du tilldelar användarroller och arbetsflöden till vissa typer av användare.

## Fotofotograferingsarbetsflöden  {#exploring-product-photo-shoot-workflows}

Creative Project innehåller olika projektmallar som uppfyller olika projektkrav. Mallen **Produktfotoprojekt** är tillgänglig direkt. Den här mallen innehåller arbetsflöden för fotoplåtning där du kan initiera och hantera begäranden om produktfotografering. Det innehåller även en rad uppgifter som gör att du kan få digitala bilder för produkter genom lämpliga gransknings- och godkännandeprocesser.

## Skapa ett produktfotoprojekt {#create-a-product-photo-shoot-project}

1. Klicka på **Skapa** i konsolen **Projekt** och välj sedan **Skapa projekt** i listan.

   ![Skapa projektknapp](assets/chlimage_1-132a.png)

1. På sidan **Skapa projekt** väljer du mallen **Projekt för produktfototagning** och klickar på **Nästa**.

   ![Projektguiden](assets/chlimage_1-133a.png)

1. Ange projektinformation, inklusive titel, beskrivning och förfallodatum. Lägg till användare och tilldela dem olika roller. Du kan också lägga till en miniatyrbild för projektet.

   ![Projektinformation](assets/chlimage_1-134a.png)

1. Klicka på **Skapa**. Ett bekräftelsemeddelande meddelar att projektet har skapats.
1. Klicka på **Klar** för att återgå till konsolen **Projekt**. Du kan också klicka på **Öppna** för att visa resurserna i projektet.

## Starta arbete i ett produktfotoprojekt {#starting-work-in-a-product-photo-shoot-project}

Starta ett arbetsflöde genom att klicka på ett projekt och sedan på **Lägg till arbete** på sidan med projektinformation om du vill starta en fotoplåtning.

![Lägg till arbete](assets/chlimage_1-135a.png)

Ett **produktfotoprojekt** innehåller följande färdiga arbetsflöden:

* **Arbetsflöde för produktfototagning (Commerce Integration)**: Det här arbetsflödet använder handelsintegrering med PIM-systemet (Product Information Management) för att automatiskt generera en tagningslista för de valda produkterna (hierarki). Du kan visa produktdata som en del av resursmetadata när arbetsflödet är klart.
* **Arbetsflöde för produktfoto**: Med det här arbetsflödet kan du skapa en lista i stället för att använda den beroende på e-handelsintegrering. Den mappar de överförda bilderna till en CSV-fil i projektresursmappen.

Använd arbetsflödet **Produktfototagning (Commerce-integrering)** för att mappa bildresurser till produkterna i AEM. Det här arbetsflödet använder handelsintegrering för att länka de godkända bilderna till befintliga produktdata på platsen `/etc/commerce`.

Arbetsflödet för **produktfototagning (Commerce-integrering)** innehåller följande uppgifter:

* Skapa lista över bilder
* Överför fototagning
* Retuschera fototagning
* Granska och godkänn
* Flytta till produktionsuppgift

Om produktinformation inte är tillgänglig i AEM kan du använda arbetsflödet **Produktfototagning** för att mappa bildresurser med produkterna baserat på den information du överför i en CSV-fil. CSV-filen måste innehålla grundläggande produktinformation, t.ex. produkt-ID, kategori och beskrivning. Arbetsflödet hämtar godkända resurser för produkterna.

Det här arbetsflödet innehåller följande uppgifter:

* Överför bildlista
* Överför fototagning
* Retuschera fototagning
* Granska och godkänn
* Flytta till produktionsuppgift

Du kan anpassa det här arbetsflödet med hjälp av alternativet för arbetsflödeskonfigurationer.

Båda arbetsflödena innehåller steg för att länka produkter till deras godkända resurser. Varje arbetsflöde innehåller följande steg:

* Arbetsflödeskonfiguration: Beskriver alternativen för att anpassa arbetsflödet
* Starta ett projektarbetsflöde: Beskriver hur du startar en produktfotografering
* Information om arbetsflödesuppgifter: Tillhandahåller information om tillgängliga uppgifter i arbetsflödet

## Spåra projektförlopp {#tracking-project-progress}

Du kan följa förloppet för ett projekt genom att övervaka de aktiva/slutförda uppgifterna i ett projekt.

Använd följande för att övervaka förloppet för ett projekt:

* Uppgiftskort
* Uppgiftslista

Uppgiftskortet visar projektets övergripande förlopp. Det visas bara på projektinformationssidan om projektet har några relaterade uppgifter. Uppgiftskortet visar projektets aktuella slutförandestatus baserat på antalet slutförda uppgifter. Det omfattar inte framtida uppgifter.

Uppgiftskortet innehåller följande information:

* Procent av aktiva uppgifter
* Procent slutförda uppgifter

![Aktivitetskort](assets/chlimage_1-136a.png)

Uppgiftslistan innehåller detaljerad information om den aktuella arbetsflödesaktiviteten för projektet. Klicka på aktivitetskortet om du vill visa listan. Uppgiftslistan innehåller även metadata som startdatum, förfallodatum, tilldelad, prioritet och status för uppgiften.

![Uppgiftslista](assets/chlimage_1-137a.png)

## Arbetsflödeskonfiguration {#workflow-configuration}

Den här uppgiften innebär att tilldela användare arbetsflödessteg baserat på deras roller.

Så här konfigurerar du arbetsflödet för **produktfototagning**:

1. Navigera till **Verktyg** > **Arbetsflöden** och markera sedan rutan **Modeller** för att öppna sidan **Arbetsflödesmodeller**.
1. Välj arbetsflödet för **produktfototagning** och välj ikonen **Redigera** i verktygsfältet för att öppna den i redigeringsläge.

   ![Modell för produktfoto](assets/chlimage_1-138a.png)

1. Öppna en projektaktivitet på sidan **Produkt - fotoarbetsflöde**. Öppna t.ex. aktiviteten **Överför fotolista**.

   ![Redigera modell](assets/project-photo-shoot-workflow-model.png)

1. Klicka på fliken **Aktivitet** för att konfigurera följande:

   * Namn på uppgiften
   * Standardanvändare (roll) som tar emot uppgiften
   * Uppgiftens standardprioritet, som visas i användarens uppgiftslista
   * Uppgiftsbeskrivning som ska visas när den som tilldelas öppnar uppgiften
   * Förfallodatum för en aktivitet, som beräknas baserat på den tid som aktiviteten startades

1. Klicka på **OK** om du vill spara konfigurationsinställningarna.

Du kan konfigurera ytterligare uppgifter för arbetsflödet **Produktfototagning** på ett liknande sätt.

Utför samma steg för att konfigurera åtgärderna i arbetsflödet för **produktfototagning (Commerce Integration)**.

## Starta ett projektarbetsflöde {#starting-a-project-workflow}

I det här avsnittet beskrivs hur du integrerar produktinformationshantering med ditt kreativa projekt.

1. Navigera till ett produktfotoprojekt och klicka på ikonen **Lägg till arbete** på kortet **Arbetsflöden** .
1. Välj arbetsflödeskortet för **produktfototagning (Commerce-integrering)** om du vill starta arbetsflödet för **produktfototagning (Commerce-integrering)**. Om produktinformationen inte är tillgänglig under `/etc/commerce` väljer du arbetsflödet **Produktfototagning** och startar arbetsflödet **Produktfototagning**.

   ![Arbetsflödesguiden](assets/chlimage_1-140a.png)

1. Klicka på **Nästa** för att initiera arbetsflödet i projektet.
1. Ange arbetsflödesinformation på nästa sida.

   ![Arbetsflödesinformation](assets/chlimage_1-141a.png)

1. Klicka på **Skicka** för att starta arbetsflödet för fototagning. Sidan med projektinformation för fotoprojektet visas.

   ![Projektsida med nytt arbetsflöde](assets/chlimage_1-142a.png)

### Information om arbetsflödesuppgifter {#workflow-tasks-details}

Arbetsflödet för fotografering omfattar flera uppgifter. Varje uppgift tilldelas till en användargrupp baserat på den konfiguration som har definierats för uppgiften.

#### Skapa aktivitet för lista över bilder {#create-shot-list-task}

Aktiviteten **Skapa lista över scenbilder** gör det möjligt för projektägaren att välja produkter för vilka bilder krävs. Baserat på det alternativ som användaren valt skapas en CSV-fil som innehåller grundläggande produktinformation.

1. Klicka på ellipsknappen längst ned till höger på [aktivitetskortet](#tracking-project-progress) i projektmappen för att visa uppgiftsobjektet i arbetsflödet.

   ![Aktivitetskort](assets/chlimage_1-143a.png)

1. Välj aktiviteten **Skapa lista med bilder** och klicka sedan på ikonen **Öppna** i verktygsfältet.

   ![Öppnar tagningslistaktivitet](assets/chlimage_1-144a.png)

1. Granska aktivitetsinformationen och klicka sedan på knappen **Skapa lista för fotografering** .

   ![Information om aktivitetslistan för tagning](assets/chlimage_1-145a.png)

1. Välj produkter för vilka det finns produktdata utan kopplade bilder.

   ![Väljer produkter](assets/chlimage_1-146a.png)

1. Klicka på knappen **Lägg till i lista över bilder** om du vill skapa en CSV-fil som innehåller en lista över alla sådana produkter. Ett meddelande bekräftar att tagningslistan har skapats för de valda produkterna. Klicka på **Stäng** för att slutföra arbetsflödet.

1. När du har skapat en tagningslista visas länken **Visa lista**. Om du vill lägga till fler produkter i tagningslistan klickar du på **Lägg till i tagningslistan**. I det här fallet läggs data till i den ursprungligen skapade tagningslistan.

   ![Lägg till i tagningslistan](assets/chlimage_1-147a.png)

1. Klicka på **Visa lista över tagningar** för att visa den nya listan.

   ![Visa tagningslista](assets/chlimage_1-148a.png)

   Om du vill redigera befintliga data eller lägga till nya data klickar du på **Redigera** i verktygsfältet. Endast fälten **Product **och **Description** kan redigeras.

   ![Redigera tagningslista](assets/chlimage_1-149a.png)

   När du har uppdaterat filen klickar du på **Spara** i verktygsfältet för att spara filen.

1. När du har lagt till produkterna klickar du på ikonen **Slutför** på sidan med aktivitetsinformation för **Skapa lista för foto** för att markera uppgiften som slutförd. Du kan lägga till en valfri kommentar.

När aktiviteten har slutförts introduceras följande ändringar i projektet:

* Assets som motsvarar produkthierarkin skapas i en mapp med samma namn som arbetsflödets rubrik.
* Metadata för resurserna kan redigeras med Assets-konsolen, även innan fotografen visar bilderna.
* En mapp för fotografering skapas som lagrar bilderna som fotografen tillhandahåller. Fotomappen innehåller undermappar för varje produktpost i tagningslistan.

### Uppgift för överföring av lista över bilder {#upload-shot-list-task}

Den här uppgiften ingår i arbetsflödet för produktfotografering. Du utför den här uppgiften om produktinformation inte är tillgänglig i AEM. I det här fallet överför du en lista över produkter i en CSV-fil för vilka bildresurser krävs. Baserat på informationen i CSV-filen kan du mappa bildobjekt till produkterna. Filen måste vara en CSV-fil med namnet `shotlist.csv`.

Använd länken **Visa lista över foton** under projektkortet i föregående procedur för att hämta en CSV-exempelfil. Granska exempelfilen för att ta reda på det vanliga innehållet i en CSV-fil.

Produktlistan eller CSV-filen kan innehålla fält, till exempel **Kategori, Produkt, ID, Beskrivning** och **Sökväg**. Fältet **Id** är obligatoriskt och innehåller produkt-ID:t. De andra fälten är valfria.

En produkt kan tillhöra en viss kategori. Produktkategorin kan listas i CSV-filen under kolumnen **Kategori**. Fältet **Produkt** innehåller namnet på produkten. I fältet **Beskrivning** anger du produktbeskrivningen eller instruktionerna för fotografen.

1. Klicka på ellipsknappen längst ned till höger på [aktivitetskortet](#tracking-project-progress) i projektmappen för att visa listan över uppgifter i arbetsflödet.
1. Välj aktiviteten **Överför lista över bilder** och klicka sedan på ikonen **Öppna** i verktygsfältet.

   ![Överför fotolista](assets/chlimage_1-150a.png)

1. Granska uppgiftsinformationen och klicka sedan på knappen **Överför lista över bilder** .

   ![Överför fotolista](assets/chlimage_1-151a.png)

1. Klicka på knappen **Ladda upp lista** för att ladda upp CSV-filen. Arbetsflödet identifierar den här filen som en källa som kan användas för att extrahera produktdata för nästa uppgift.
1. Överför en CSV-fil som innehåller produktinformation i lämpligt format. Länken **Visa överförda Assets** visas under kortet när CSV-filen har överförts.

   ![Överför produktinformation](assets/chlimage_1-152a.png)

   Klicka på ikonen **Slutför** för att slutföra uppgiften.

1. Klicka på ikonen **Slutför** för att slutföra uppgiften.

### Ladda upp fototagningsaktivitet {#upload-photo-shoot-task}

Om du är redigerare kan du överföra tagningar för de produkter som listas i filen **short.csv** som skapades eller överfördes i föregående uppgift.

Namnet på de bilder som ska överföras måste börja med `<ProductId_>` där `ProductId` refereras från fältet **Id** i filen `shotlist.csv`. För en produkt i tagningslistan med till exempel **Id** `397122` laddar du upp filer med namnen `397122_highcontrast.jpg`, `397122_lowlight.png` osv.

Du kan antingen överföra bilderna direkt eller överföra en ZIP-fil som innehåller bilderna. Baserat på deras namn placeras bilderna i respektive produktmapp i fotograferingsmappen.

1. Klicka på ellipsknappen längst ned till höger på [aktivitetskortet](#tracking-project-progress) under projektmappen för att visa uppgiftsobjektet i arbetsflödet.
1. Välj aktiviteten **Överför fototagning** och klicka sedan på ikonen **Öppna** i verktygsfältet.

   ![Överför fotoplåtning](assets/chlimage_1-153a.png)

1. Klicka på **Överför fototagning** och överför fotofotografierna.
1. Klicka på ikonen **Slutför** i verktygsfältet för att slutföra åtgärden.

### Retuschera fototagningsaktivitet {#retouch-photo-shoot-task}

Om du har redigeringsbehörighet utför du uppgiften **Retuschera fototagning** för att redigera bilderna som överförts till fotofotograferingsmappen.

1. Klicka på ellipsknappen längst ned till höger på [aktivitetskortet](#tracking-project-progress) under projektmappen för att visa uppgiftsobjektet i arbetsflödet.
1. Välj åtgärden **Retuschera fototagning** och klicka sedan på ikonen **Öppna** i verktygsfältet.

   ![Retuschera fotoplåtning](assets/chlimage_1-154a.png)

1. Klicka på länken **Visa överförda Assets** på sidan **Retuschera fototagning** för att bläddra bland de överförda bilderna.

   ![Visa överförda resurser](assets/chlimage_1-155a.png)

   Om det behövs kan du redigera bilderna i ett Adobe Creative Cloud-program.

   ![Redigera resurs](assets/chlimage_1-156a.png)

1. Klicka på ikonen **Slutför** i verktygsfältet för att slutföra åtgärden.

### Granska och godkänn uppgift {#review-and-approve-task}

I det här fallet granskar du fotot som överförts av en fotograf och markerar bilderna som godkända för användning.

1. Klicka på ellipsknappen längst ned till höger på [aktivitetskortet](#tracking-project-progress) under projektmappen för att visa uppgiftsobjektet i arbetsflödet.
1. Välj åtgärden **Granska och godkänn** och klicka sedan på ikonen **Öppna** i verktygsfältet.

   ![Granska och godkänn](assets/chlimage_1-157a.png)

1. På sidan **Granska och godkänn** tilldelar du granskningsaktiviteten till en roll och klickar sedan på **Granska** för att börja granska de överförda produktbilderna.

   ![Börja granska resurser](assets/chlimage_1-158a.png)

1. Välj en produktbild och klicka på ikonen **Godkänn** i verktygsfältet för att markera den som godkänd. När du har godkänt en bild visas en godkänd banderoll över den.

   ![Godkänner en bild](assets/chlimage_1-159a.png)

1. Klicka på **Slutför**. De godkända bilderna länkas till de tomma resurserna som skapades.

Du kan utelämna vissa produkter utan någon bild. Senare kan du göra om uppgiften och markera den som slutförd när den är klar.

Du kan navigera till projektresurser med Assets-gränssnittet och verifiera godkända bilder.

Klicka på nästa nivå om du vill visa produkter enligt din produktdatahierarki.

Creative Project associerar godkända resurser med den refererade produkten. Metadata för resursen uppdateras med produktreferensen och grundläggande information på fliken **Produktdata** under resursegenskaper som visas i avsnittet Metadata för AEM-resurser.

>[!NOTE]
>
>I arbetsflödet för **produktfototagning** (utan e-handelsintegrering) har de godkända bilderna ingen koppling till produkter.

### Flytta till produktionsuppgift {#move-to-production-task}

Den här aktiviteten flyttar de godkända resurserna till den produktionsklara mappen så att de blir tillgängliga för användning.

1. Klicka på ellipsknappen längst ned till höger på [aktivitetskortet](#tracking-project-progress) under projektmappen för att visa uppgiftsobjektet i arbetsflödet.
1. Välj aktiviteten **Flytta till produktion** och klicka sedan på ikonen **Öppna** i verktygsfältet.

   ![Flytta till produktion](assets/chlimage_1-160a.png)

1. Om du vill visa godkända resurser för fototagningen innan du flyttar dem till produktionsklar mapp klickar du på länken **Visa godkänd Assets** under miniatyrbilden av projektet på aktivitetssidan **Flytta till produktion** .

   ![Flytta till sidan för produktionsuppgift](assets/chlimage_1-161a.png)

1. Ange sökvägen till den produktionsklara mappen i fältet **Flytta till**.

   ![Flytta till bana](assets/chlimage_1-162a.png)

1. Klicka på **Flytta till produktion**. Stäng bekräftelsemeddelandet. Resurserna flyttas till den angivna sökvägen och en snurruppsättning skapas automatiskt för de godkända resurserna för varje produkt baserat på mapphierarkin.

1. Klicka på ikonen **Fullständig** i verktygsfältet. Arbetsflödet slutförs när det sista steget markeras som slutfört.

## Visa DAM-resursmetadata {#viewing-dam-asset-metadata}

När du har godkänt mediefilerna länkas de till motsvarande produkter. På sidan [Egenskaper](/help/assets/manage-assets.md#editing-properties) för de godkända resurserna finns nu ytterligare en **produktdatafliken** (länkad produktinformation). På den här fliken visas produktinformation, SKU-nummer och annan produktrelaterad information som länkar resursen. Klicka på ikonen **Redigera** för att uppdatera en resursegenskap. Produktrelaterad information förblir skrivskyddad.

Klicka på länken som visas för att navigera till respektive produktinformationssida i produktkonsolen som resursen är associerad med.

## Anpassa arbetsflödena för projektfototagning {#customizing-the-project-photo-shoot-workflows}

Du kan anpassa arbetsflödena för **Project Photo Shoot** utifrån dina behov. Detta är en valfri rollbaserad uppgift som du utför för att ange värdet för en variabel i projektet. Senare kan du använda det konfigurerade värdet för att komma fram till ett beslut.

1. Klicka på AEM logotyp och gå sedan till **Verktyg** > **Arbetsflöde** > **Modeller** för att öppna sidan **Arbetsflödesmodeller**.
1. Välj arbetsflödet för **produktfototagning (Commerce-integrering)** eller arbetsflödet för **produktfototagning** och klicka på **Redigera** i verktygsfältet för att öppna arbetsflödet i redigeringsläge.
1. Öppna sidopanelen och leta upp steget **Skapa rollbaserad projektuppgift** och dra det till arbetsflödet.

   ![Skapa rollbaserad projektuppgift](assets/project-model-role-based.png)

1. Öppna steget **Rollbaserad aktivitet**.
1. Ange ett namn för uppgiften som ska visas i uppgiftslistan på fliken **Aktivitet**. Du kan också tilldela en roll uppgiften, ange standardprioritet, ange en beskrivning och ange en tidpunkt när uppgiften förfaller.

   ![Konfigurera arbetsflödessteg](assets/project-task-step.png)

1. Ange aktivitetens åtgärder på fliken **Routning**. Om du vill lägga till flera åtgärder klickar du på länken **Lägg till objekt** .

   ![Fliken Routning](assets/project-task-step-routing.png)

1. När du har lagt till alternativen klickar du på **OK** för att lägga till ändringarna i steget.

1. I fönstret **Arbetsflödesmodell** klickar du på **Synkronisera** för att spara ändringarna i hela arbetsflödet. Om du trycker eller klickar på **OK** för steget sparas inte ändringarna i arbetsflödet. Om du vill spara ändringarna i arbetsflödet klickar du på **Synkronisera**.

1. Öppna sidopanelen och leta upp arbetsflödet **Gå till steg** och dra det till arbetsflödet.

1. Öppna **Gå till**-aktiviteten och klicka på fliken **Process**.

1. Välj **målsteget** att gå till och ange att **routningsuttrycket** är ECMA-skript. Ange sedan följande kod i fältet **Script**:

   ```javascript
   function check() {
   
   if (workflowData.getMetaDataMap().get("lastTaskAction","") == "Reject All") {
   
   return true
   
   }
   
   // set copywriter user in metadata
   
   var previousId = workflowData.getMetaDataMap().get("lastTaskCompletedBy", "");
   
   workflowData.getMetaDataMap().put("copywriter", previousId);
   
   return false;
   
   }
   ```

   >[!TIP]
   >
   >Mer information om skript i arbetsflödessteg finns i [Definiera en regel för en OR-delning](/help/sites-developing/workflows-models.md).

   ![Gå till skript](assets/project-workflow-goto.png)

1. Klicka på **OK**.

1. Klicka på **Synkronisera** för att spara arbetsflödet.

En ny aktivitet visas nu när [Flytta till produktion](#move-to-production-task) har slutförts och tilldelats ägaren.

Användaren i rollen **Ägare** kan slutföra uppgiften och välja en åtgärd (från listan med åtgärder som lagts till i arbetsflödesstegskonfigurationerna) i listan i kommentarspopup-fönstret.

>[!NOTE]
>
>När du startar en server cachelagrar uppgiftslistservern mappningarna mellan uppgiftstyper och URL:er som definieras under `/libs/cq/core/content/projects/tasktypes`. Du kan sedan utföra den vanliga övertäckningen och lägga till anpassade uppgiftstyper genom att placera dem under `/apps/cq/core/content/projects/tasktypes`.
