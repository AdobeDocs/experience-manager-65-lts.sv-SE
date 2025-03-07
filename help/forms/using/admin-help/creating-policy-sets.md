---
title: Skapa och hantera principuppsättningar
description: Policyuppsättningar används för att gruppera principer som har ett gemensamt affärssyfte. Du kan skapa, redigera och ta bort profiler i en principuppsättning.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/working_with_document_security
products: SG_EXPERIENCEMANAGER/6.5/FORMS
feature: Document Security
solution: Experience Manager, Experience Manager Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 7d975fe3-9444-4337-ba32-98a8cc2e03f3
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '1309'
ht-degree: 0%

---

# Skapa och hantera principuppsättningar {#creating-and-managing-policy-sets}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Policyuppsättningar används för att gruppera principer som har ett gemensamt affärssyfte. Policyuppsättningar kan göras tillgängliga för en delmängd av användarna i systemet.

Varje principuppsättning har minst en associerad principuppsättningskoordinator. *Principuppsättningskoordinatorn* är en administratör eller en användare som har ytterligare behörigheter. Policyuppsättningens koordinator är vanligtvis en specialist i organisationen som bäst kan skapa policyer i en viss uppsättning.

Koordinatorer för principuppsättningar kan utföra följande uppgifter:

* Skapa nya profiler
* Redigera och ta bort profiler i principuppsättningen
* Redigera inställningar för principuppsättning
* Lägg till och ta bort koordinatorer för principuppsättningen
* Visa policy- och dokumenthändelser för alla profiler eller dokument i principuppsättningen
* Återkalla åtkomst till dokument
* Byt profil för dokumentet

Policyuppsättningar skapas och tas bort i dokumentsäkerhetsadministratörens gränssnitt av superanvändare och koordinatorer för principuppsättningar som har behörighet att göra det.

När du tar bort en principuppsättning kan profiler som ingår i uppsättningen inte tillämpas på nya dokument. Du kan dock visa principinformationen både i administrationskonsolen och på slutanvändarens webbsidor för profiler som fortfarande används. Du kan visa principinformationen från dokumentinformationssidan för alla dokument som skyddas av profilen. Profiler som fortfarande används kan redigeras.

Den överordnade användaren eller principuppsättningskoordinatorn lägger till domäner som skapas i användarhantering till den synliga användaren och gruppen för varje principuppsättning. Den här listan är synlig för principuppsättningens koordinator och används för att ange gränser för vilka domäner som principuppsättningens koordinator kan bläddra i när användaren väljer att lägga till i profiler.

När du skapar principuppsättningar tilldelar du användare rollen som dokumentutgivare. *dokumentutgivaren* är den användare som skyddar dokumentet med en profil. Den här användaren ingår som standard alltid i en princip med fullständig åtkomstbehörighet, inklusive funktioner för återkallande och policyväxling. Administratörer kan dock ändra dokumentutgivarens åtkomsträttigheter för delade profiler. Administratören kan t.ex. inaktivera dokumentutgivarens rätt att återkalla dokumentåtkomst eller ändra profilen. Om en administratör byter profil för dokumentet uppdateras utgivarens namn till namnet på ägaren till profilen som senast användes för dokumentet.

Vid installation av dokumentsäkerhet skapas en standardprincipuppsättning som heter *Global principuppsättning*. Den här principinställningen hanteras av administratören som installerade programvaran eller av principuppsättningens koordinator som är utsedd för den här principinställningen.

## Skapa en principuppsättning {#create-a-policy-set}

Global principuppsättning är den enda standardprincipuppsättning som skapas vid installationen. Du kan skapa ytterligare principuppsättningar och lägga till profiler, användare, samordnare för principuppsättningar och dokumentutgivare. När du har skapat en principuppsättning kan du skapa profiler i uppsättningen.

När du skapar en principuppsättning kan du när som helst använda knappen Bakåt för att gå tillbaka till föregående skärm och knappen Spara för att spara din profiluppsättning.

1. Klicka på Profiler på dokumentsäkerhetssidan, klicka på fliken Profiluppsättningar och sedan på Ny.
1. I rutan Namn skriver du ett namn för principuppsättningen, skriver en beskrivning och klickar sedan på Nästa. Namnet får inte innehålla kolon **:**.

   >[!NOTE]
   >
   >Du kan skapa ett namn på en principuppsättning som innehåller utökade tecken, men när en jämförelse görs mellan två strängar anses både tecken med eller utan accent som &quot;e&quot; och &quot;é&quot; vara desamma. När någon skapar en principuppsättning görs en jämförelse för att kontrollera om det redan finns en principuppsättning med samma namn. Jämförelsen kan inte skilja mellan namn som är samma förutom för tecken med accent. Det antas att principuppsättningen redan har lagts till i databasen och att den nya inte läggs till.

1. (Valfritt) Om du vill ange vilka domäner som ska visas för Document Publishers när de lägger till användare i en profil klickar du på Lägg till domäner, markerar de domäner som ska vara sökbara, klickar på Lägg till och sedan på OK.
1. Klicka på Nästa på sidan Lägg till synliga användare och grupper.
1. (Valfritt) Om du vill lägga till en principuppsättningskoordinator klickar du på Lägg till användare och grupper på sidan Lägg till principuppsättningskoordinator(er) (steg 3 av 4) och utför följande uppgifter:

   * Skriv namnet eller e-postadressen i rutan Sök.
   * Välj lämpligt alternativ i listan Använda.
   * I listan Typ väljer du Användare och i listan In väljer du en domän att söka i.
   * I visningslistan väljer du antalet resultat som ska visas per sida och klickar sedan på Sök.
   * Markera kryssrutan för användaren eller gruppen som ska läggas till och klicka på Nästa.
   * Välj behörigheter för principuppsättningskoordinatorn och klicka på Lägg till. Följande behörigheter kan anges:

      * Visa händelser
      * Hantera dokument (återkalla och återställ åtkomst till dokument och växla profiler på dokument)
      * Hantera profiler (skapa, redigera och ta bort profiler)
      * Hantera dokumentutgivare (lägga till och ta bort dokumentutgivare)
      * Delegera (lägg till och ta bort principuppsättningskoordinatorer)

1. Upprepa steg 5 om du vill lägga till fler koordinatorer för principuppsättningar.
1. Granska inställningarna för koordinatorn för principuppsättningen och klicka på Nästa.
1. Klicka på Lägg till användare och grupper för att lägga till dokumentutgivare som kan använda profilerna i profiluppsättningen för att skydda dokument.
1. Utför följande åtgärder på sidan Lägg till dokumentutgivare:

   * Skriv namnet eller e-postadressen i rutan Sök.
   * Välj lämpligt alternativ i listan Använda.
   * I listan Typ väljer du Användare och i listan In väljer du en domän att söka i.
   * I visningslistan väljer du antalet resultat som ska visas per sida och klickar sedan på Sök.
   * Markera kryssrutorna för de användare och grupper som ska läggas till, klicka på Lägg till och klicka sedan på OK.

1. Klicka på Spara.

Nu kan du lägga till profiler i din profiluppsättning. (Se [Skapa och redigera profiler](/help/forms/using/admin-help/creating-policies.md#creating-and-editing-policies).)

## Redigera en principuppsättning {#edit-a-policy-set}

1. Klicka på Profiler på dokumentsäkerhetssidan, klicka på fliken Principuppsättningar och klicka på den profiluppsättning som du vill redigera.
1. Klicka på lämplig flik och redigera enligt behov:

   * **Detalj:** Redigera principuppsättningsnamnet och beskrivningen.
   * **Principer:** Skapa, aktivera, redigera och ta bort profiler i principuppsättningen.
   * **Synliga användare och grupper:** Lägg till och ta bort synliga användare och grupper som kan inkluderas i en profil.
   * **Koordinatorer för principuppsättning:** Lägg till, ta bort och ändra behörigheter för koordinatorer.
   * **Dokumentutgivare:** Lägg till och ta bort användare som kan publicera dokument med hjälp av profilerna i uppsättningen.

1. Om du vill ta bort en synlig användare eller grupp, Policy Set Coordinator eller Document Publisher klickar du på lämplig flik, markerar kryssrutan för posten, klickar på Ta bort och sedan på OK.
1. Om du vill lägga till synliga användare eller grupper, en principuppsättningskoordinator eller Dokumentutgivare klickar du på lämplig flik, klickar på Lägg till användare eller grupper, söker efter användaren eller gruppen som ska läggas till, markerar posten, klickar på Lägg till och klickar sedan på OK.
1. På fliken Profiler söker du efter profiler att lägga till i uppsättningen och skapar nya profiler:

   * Om du vill söka efter en profil väljer du princip-ID eller principnamn, skriver motsvarande värde, väljer antalet objekt som ska visas och klickar på Sök.
   * Mer information om hur du skapar en profil finns i [Skapa och redigera profiler](/help/forms/using/admin-help/creating-policies.md#creating-and-editing-policies).

## Ta bort en principuppsättning {#delete-a-policy-set}

När du tar bort en principuppsättning kan profiler som ingår i uppsättningen inte tillämpas på nya dokument. Du kan dock visa principinformationen både i administrationskonsolen och på slutanvändarens webbsidor för profiler som fortfarande används. Du kan visa principinformationen från dokumentinformationssidan för alla dokument som skyddas av profilen. Profiler som fortfarande används kan redigeras.

1. Klicka på Profiler och sedan på fliken Principuppsättningar.
1. Markera kryssrutan för den principuppsättning som ska tas bort.
1. Klicka på Ta bort och sedan på OK.
