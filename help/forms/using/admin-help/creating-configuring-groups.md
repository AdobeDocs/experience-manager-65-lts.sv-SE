---
title: Skapa och konfigurera grupper
description: Lär dig skapa grupper manuellt eller dynamiskt, redigera en grupp, visa information om en grupp eller ta bort en grupp.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/setting_up_and_organizing_users
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
role: User, Developer
feature: Adaptive Forms
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '1585'
ht-degree: 0%

---

# Skapa och konfigurera grupper{#creating-and-configuring-groups}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

När du skapar grupper med användare kan du tilldela roller till gruppen i stället för till enskilda användare.

Det finns två olika typer av grupper. Du kan skapa en grupp manuellt och lägga till användare och andra grupper i den. Du kan också skapa dynamiska grupper som automatiskt inkluderar alla användare som uppfyller en viss uppsättning regler.

Användare kan få en långsammare svarstid om de tillhör många grupper (till exempel 500 eller fler) eller om grupperna är djupt inkapslade (till exempel 30 nivåer). Om du får det här problemet kan du konfigurera AEM-formulär så att information från vissa domäner hämtas i förväg. (Se [Konfigurera AEM-formulär för att hämta domäninformation i förväg](/help/forms/using/admin-help/configure-aem-forms-prefetch-domain.md#configure-aem-forms-to-prefetch-domain-information).)

## Skapa en grupp manuellt {#create-a-group-manually}

När du skapar en grupp manuellt kan du lägga till användare och andra grupper i den och tilldela roller till gruppen. Du kan även associera gruppen med en överordnad grupp.

Om du använder innehållstjänster (borttagen) kan du markera alternativet för att överföra användare och grupper till registrerade externa huvudlagringsleverantörer på sidan Domänhantering för att skicka informationen till nya användare eller grupper som du skapar i innehållstjänster (borttagen).

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Användare och grupper och sedan på Ny grupp.
1. Fyll i avsnittet Allmänna inställningar och klicka på Nästa. Kanoniskt namn och gruppnamn är obligatoriska attribut.

   Det kanoniska namnet är en unik identifierare för gruppen. Varje grupp och användare i en domän måste ha ett unikt kanoniskt namn. Markera kryssrutan Systemgenererad om du vill att användarhantering ska tilldela ett unikt värde eller avmarkera kryssrutan och ange ett anpassat värde för det kanoniska namnet.

   Undvik att använda understreck (_) i kanoniska namn, till exempel `sample_group`. När du söker efter grupper baserat på deras kanoniska namn, returneras inte grupper som innehåller understreck.

1. Om du vill lägga till användare och grupper i den nya gruppen klickar du på Sök efter användare/grupper och gör följande:

   * Ange sökvillkoren i rutan Sök.
   * I listan In väljer du Användare, Grupper eller Användare och grupper.
   * Välj Namn, E-post eller Användar-ID i listan Använder.
   * Markera domänen, välj det antal objekt som ska visas och klicka på Sök.
   * I sökresultaten markerar du kryssrutorna för de användare och grupper som ska läggas till i den nya gruppen och klickar på OK.

1. Klicka på Nästa.
1. Om du vill lägga till den nya gruppen i andra befintliga grupper klickar du på Sök grupper och gör följande:

   * Ange sökvillkoren i rutan Sök.
   * Markera domänen, markera antalet objekt som ska visas och klicka på Sök.
   * Markera kryssrutorna för de grupper som den nya gruppen tillhör i sökresultaten och klicka på OK.

1. Klicka på Nästa.
1. Om du vill tilldela roller till gruppen klickar du på Sök roller, markerar kryssrutorna för varje roll som ska tilldelas gruppen och klickar på OK. Användare i gruppen ärver roller som är tilldelade på gruppnivå.
1. Klicka på Slutför.

## Skapa en dynamisk grupp {#create-a-dynamic-group}

I en dynamisk grupp väljer du inte de användare som tillhör gruppen separat. I stället anger du en uppsättning regler så läggs alla användare som uppfyller de reglerna automatiskt till i den dynamiska gruppen.

Använd något av följande två sätt för att skapa dynamiska grupper:

* Gör det möjligt att automatiskt skapa dynamiska grupper baserat på e-postdomäner, som @adobe.com. När du aktiverar den här funktionen skapar Hantering av användare en dynamisk grupp för varje unik e-postdomän i AEM formulärdatabas. Använd ett cron-uttryck för att ange hur ofta användarhantering söker i AEM formulärdatabas efter nya e-postdomäner. Dessa dynamiska grupper läggs till i den lokala domänen DefaultDom och får namnet&quot;Alla användare med ett *`[email domain]`* post-ID&quot;.
* Skapa en dynamisk grupp baserat på angivna villkor, inklusive användarens e-postdomän, beskrivning, kanoniskt namn och domännamn. För att kunna tillhöra den dynamiska gruppen måste användaren uppfylla alla angivna villkor. Om du vill ställa in ett &quot;eller&quot;-villkor skapar du två separata dynamiska grupper och lägger till båda i en lokal grupp. Använd till exempel den metoden för att skapa en grupp användare som tillhör e-postdomänen @adobe.com eller vars kanoniska namn innehåller ou=adobe.com. Användarna behöver dock inte nödvändigtvis uppfylla båda villkoren.

En dynamisk grupp innehåller bara användare. Den får inte innehålla andra grupper. En dynamisk grupp kan dock tillhöra en överordnad grupp.

### Skapa dynamiska grupper automatiskt baserat på e-postdomäner {#automatically-create-dynamic-groups-based-on-email-domains}

1. Klicka på Inställningar > Användarhantering > Konfiguration > Konfigurera avancerade systemattribut.
1. Markera kryssrutan under Automatiskt skapande av dynamisk grupp.
1. Ange när användarhanteraren ska söka efter nya e-postdomäner. Den här tiden bör vara efter domänsynkroniseringstiden eftersom skapandet av dynamiska grupper bara är logiskt om domänsynkroniseringen är slutförd.

   * Om du vill aktivera automatisk synkronisering dagligen anger du tiden i 24-timmarsformat i rutan Inträffar varje dag. När du sparar inställningarna konverteras det här värdet till ett cron-uttryck, som visas i rutan nedan.
   * Om du vill schemalägga synkronisering på en viss dag i veckan eller månaden, eller under en viss månad, väljer du lämpligt cron-uttryck i rutan. Standardvärdet är `0 00 4 ? * *` (vilket betyder check klockan 4.00 varje dag).

     Användningen av cron-uttryck baseras på Quartz-systemet för jobbplanering med öppen källkod, version 1.4.0.

1. Klicka på Spara.

### Skapa en dynamisk grupp baserat på angivna villkor {#create-a-dynamic-group-based-on-specified-criteria}

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Användare och grupper.
1. Klicka på Ny dynamisk grupp.
1. Fyll i avsnittet Allmänna inställningar. Gruppnamn är ett obligatoriskt attribut. Du kan tilldela gruppen till valfri konfigurerad domän.
1. Under Dynamiska gruppvillkor anger du ett eller flera attribut som används för att fylla den dynamiska gruppen.

   >[!NOTE]
   >
   >Attributen för e-post, beskrivning och kanoniskt namn är skiftlägeskänsliga när operatorn Lika används. De är inte skiftlägeskänsliga med operatorerna Börjar med, Slutar med eller Innehåller.

   **E-post:** Användarens e-postdomän, till exempel `@adobe.com`.

   **Beskrivning:** Användarbeskrivning, t.ex. &quot;Computer Scientist&quot;

   **Kanoniskt namn:** Användarens kanoniska namn, till exempel `ou=adobe.com`

   **Domännamn:** Namnet på den domän som användaren tillhör, till exempel `DefaultDom`. Attributet Domännamn är skiftlägeskänsligt när du använder operatorn Innehåller. Den är inte skiftlägeskänslig med operatorerna Börjar med, Slutar med eller Likhetstecken.

1. Klicka på Testa. På en testsida visas de första 200 användarna som uppfyller de definierade villkoren. Klicka på Stäng.
1. Om testet returnerade det förväntade resultatet klickar du på Nästa. I annat fall redigerar du de dynamiska gruppvillkoren och testar igen.
1. Om du vill lägga till den dynamiska gruppen i en överordnad grupp klickar du på Sök grupper och gör följande:

   * Ange sökvillkoren i rutan Sök.
   * Markera domänen, markera antalet objekt som ska visas och klicka på Sök.
   * Markera kryssrutorna för de grupper som den dynamiska gruppen tillhör i sökresultatet och klicka på OK.

1. Klicka på Nästa.
1. Om du vill tilldela roller till den dynamiska gruppen klickar du på Sök roller, markerar kryssrutorna för varje roll som ska tilldelas gruppen och klickar sedan på OK. Användare i gruppen ärver roller som är tilldelade på gruppnivå.
1. Klicka på Slutför.

## Visa information om en grupp {#view-details-about-a-group}

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Användare och grupper.
1. I listan In väljer du Gruppera och klickar sedan på Sök. Resultatet av sökningen visas längst ned på sidan. Du kan sortera listan genom att klicka på någon av kolumnrubrikerna.
1. Klicka på namnet på gruppen om du vill visa information om den. Sidan Gruppinformation visas.
1. Om du vill visa direktmedlemmar i gruppen klickar du på Underordnade huvudkonton.

## Redigera en grupp {#edit-a-group}

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Användare och grupper.
1. Gör följande för att hitta gruppen som ska redigeras:

   * Ange sökvillkoren i rutan Sök.
   * Välj Namn eller E-post i listan Använda.
   * I listan In väljer du Grupper.
   * Markera domänen, markera antalet objekt som ska visas och klicka på Sök.
   * Klicka på namnet på gruppen som du vill redigera i sökresultaten.

1. Redigera de allmänna inställningarna på fliken Information och klicka på Spara.
1. Om du vill redigera de associerade grupperna klickar du på fliken Överordnade grupper och gör följande:

   * Om du vill söka efter grupper att lägga till i associationen klickar du på Sök grupper och fyller i sökinformationen.
   * Om du vill lägga till grupper markerar du kryssrutan för de grupper som ska läggas till, klickar på OK och sedan på Spara.
   * Om du vill ta bort en associerad grupp markerar du kryssrutan för gruppen som ska tas bort, klickar på Ta bort, klickar på OK och sedan på Spara.

1. Om du vill redigera användare och grupper i gruppen klickar du på fliken Underordnade huvudkonton och gör följande:

   * Om du vill söka efter användare och grupper att lägga till klickar du på Sök efter användare/grupper och fyller i sökinformationen.
   * Om du vill lägga till en användare eller grupp markerar du kryssrutan för användaren eller gruppen, klickar på OK och sedan på Spara.
   * Om du vill ta bort en användare eller grupp markerar du kryssrutan för användaren eller gruppen, klickar på Ta bort, klickar på OK och sedan på Spara.

1. Om du vill redigera rolltilldelningar klickar du på fliken Rolltilldelningar och gör följande:

   * Om du vill söka efter roller som ska tilldelas gruppen klickar du på Sök roller.
   * Om du vill lägga till en roll markerar du kryssrutan för rollen, klickar på OK och sedan på Spara.
   * Om du vill ta bort tilldelningen för en roll markerar du kryssrutan för rollen, klickar på Ta bort tilldelning och sedan på Spara.

## Ta bort en grupp {#delete-a-group}

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Användare och grupper.
1. Välj Grupper i listan Sök och klicka sedan på Sök.
1. Markera kryssrutan för gruppen som ska tas bort, klicka på Ta bort och klicka sedan på OK.
