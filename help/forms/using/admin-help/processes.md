---
title: Hantera processer
description: På sidan Processlista visas de processer som en användare har initierat eller som startades automatiskt. Läs mer om hur du hanterar processerna.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_forms_workflow
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 6459abd5-6341-4c28-a747-bde9a91e3a88
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1643'
ht-degree: 0%

---

# Hantera processer {#managing-processes}

På sidan Processlista visas de processer som en användare har initierat eller som startades automatiskt.

1. I administrationskonsolen klickar du på Tjänster > Forms-arbetsflöde > Forms-arbetsflöde. I processlistan visas följande information:

   **Processnamn - Version:** Processens namn, enligt definition i Workbench.

   **Program:** Programmet som processen tillhör, enligt definition i Workbench.

   **Status:** Aktiv innebär att processen är den som aktiveras för processversionen. Inaktiv innebär att processen är en gammal version som fortfarande har processinstanser.

   **Skapad:** Datum och tid då processen distribuerades.

1. Klicka på ett processnamn för att visa dess processinstanser på sidan Processinstans.

## Arbeta med processinstanser {#working-with-process-instances}

Om du öppnar sidan Processinstans från sidan Processlista visas alla processinstanser för den process du har valt. Om du öppnar sidan Processinstans efter att ha gjort en sökning visas bara de processinstanser som hittats.

För varje processinstans visas följande information i listan:

**Process-ID:** Den identifierare som tilldelas i formulärarbetsflödet när processen initieras (det vill säga när en användare eller ett automatiskt steg initierar en process). Du kan använda den här identifieraren för att spåra processinstansen genom dess livscykel.

**Processnamn - Version:** Processens namn, enligt definition i Workbench.

**Status:** Anger om processinstansen körs normalt, om tillståndet ändras eller om den har stoppats. (Se Om processinstansstatus.)

**Skapad:** Datum och tid då processinstansen skapades.

**Uppdateringsdatum:** Datum och tid då processinstansens status senast ändrades.

Du kan göra följande på sidan Processinstans:

* Välj en processinstans om du vill visa information om den, t.ex. dess åtgärder och underprocesser. När du väljer en processinstans visas sidan Processinstansinformation.
* Pausa, avbryta uppehåll eller avsluta processinstanser.
* Sök efter en processinstans. Klicka på Sök för att påbörja en sökning.

### Om processinstansstatus {#about-process-instance-statuses}

En processinstans, inklusive underprocesser, kan ha följande status:

**COMPLETE:** Alla förgreningar och åtgärder i processinstansen har slutförts. COMPLETE är den slutliga statusen för en processinstans.

**SLUTFÖRD:** Processinstansens status ändras till COMPLETE.

**INITIERAD:** Processinstansen har skapats men körs inte än. INITIATED är den första statusen för en processinstans.

**KÖRS:** Processinstansen körs normalt. Ett automatiskt steg kan vara på gång, eller så kan processinstansen ta emot användarindata eller vänta på användarinteraktion.

**UPPHÄVD:** Processinstansen har inaktiverats av en administratör eller av ett steg i processen. Inga fler åtgärder utförs förrän statusen har ändrats.

**UPPHÄVANDE:** Statusen ändras nu till UPPHÄVD. Om en åtgärd har utformats för att ignorera pausbegäranden och ännu inte har slutförts, måste den åtgärden slutföras innan processinstansen pausas.

**AVSLUTAD:** Processinstansen har avslutats av en administratör.

**AVSLUTAR:** Statusen ändras till AVSLUTAD. Om en åtgärd har utformats för att ignorera avslutningsbegäranden och ännu inte har slutförts, måste åtgärden slutföras innan processinstansen avslutas.

**UPPHÄVANDE:** Status ändras till KÖRNING efter att ha AVBRUTITS.

>[!NOTE]
>
>När en begäran görs om att ändra status för en processinstans (till exempel att göra uppehåll eller avsluta), kommer begäran in i kommandokö för arbetsflöde för formulär. Beroende på storleken på kön och den totala bearbetningshastigheten kan det hända att den visade statusen inte ändras förrän sidan har lästs in en eller flera gånger.

### Pausa eller avbryta uppehåll i processinstanser {#suspend-or-unsuspend-process-instances}

Om du behöver felsöka ett problem eller om du vet att ett problem kommer att uppstå i en processinstans i ett senare steg på grund av ett externt tillstånd, kan du göra uppehåll i processinstansen tillfälligt.

Du kan göra uppehåll i processinstanser som har statusen KÖRNING.

När du har gjort uppehåll i en processinstans ändras dess status till SUSPENDING, SUSPENDED och processen pausas vid den aktuella åtgärden. Processinstansen behåller den här statusen tills statusen ändras till OVÄNTAD.

Det är bara processinstanser som har statusen SUSPENDED som kan ändras till UNSUSPENDED.

När du avbryter uppehållet för en processinstans ändras dess status till RUNNING, och den fortsätter med åtgärden där den pausats.

När du gör uppehåll i en processinstans som har anropat andra processer (underordnade processer) med sin invoke-åtgärd, pausas även de underordnade processerna.

1. I administrationskonsolen klickar du på Tjänster > Forms-arbetsflöde > Forms-arbetsflöde.
1. Markera processen på sidan Processinstans och klicka på Gör uppehåll eller Gör uppehåll.

### Avsluta en processinstans {#terminate-a-process-instances}

Om en åtgärd för en processinstans har stannat eller påträffat något annat feltillstånd, eller om du behöver tvinga en processinstans att sluta köra, kan du avsluta processinstansen.

Du kan avsluta processinstanser som har vilken status som helst.

När du avslutar en processinstans ändras dess status till TERMINATING, sedan TERMINATED, och processen stoppas vid den aktuella åtgärden. Inga fler åtgärder körs och alla associerade åtgärder och uppgifter avslutas.

1. I administrationskonsolen klickar du på Tjänster > Forms-arbetsflöde > Forms-arbetsflöde.
1. Markera processen på processinstanssidan och klicka på Avsluta.

## Arbeta med processinstansinformation {#working-with-process-instance-details}

På sidan Processinstansinformation visas historiken för en processinstans.

Under Sammanfattning visas grundläggande information om processinstansen.

På fliken Åtgärder visas varje åtgärd för processinstansen i den ordning som de slutförs från första till sista med följande information:

**Åtgärdsnamn:** Åtgärdens namn, enligt definition i Workbench.

**Status:** Anger om åtgärden körs normalt eller har stoppats. (Se Om processinstansstatus.)

**Grennamn:** Namnet på grenen, enligt definition i Workbench.

**Startdatum:** Datum och tid då åtgärden startade.

**Slutförd:** Datum och tid då åtgärden slutfördes.

En underprocess är en processinstans som startas av en annan process och som körs oberoende av den andra processen. Underprocesser visas bara om de har designats som en del av processen i Workbench. På fliken Underprocesser visas varje underprocess med följande information:

**Process-ID:** Detta positiva heltal som formulärarbetsflödet tilldelar när processen initieras (det vill säga när en användare eller ett automatiskt steg initierar processen). Du kan använda den här identifieraren för att spåra processinstansen genom dess livscykel.

**Processnamn - Version:** Processens namn, enligt definition i Designer.

**Status:** Anger om processinstansen körs normalt, om tillståndet ändras eller om den har stoppats. (Se Om processinstansstatus.)

**Skapad:** Datum och tid då underprocessen skapades.

**Uppdateringsdatum:** Datum och tid då status för underprocessen senast ändrades.

Du kan göra följande på sidan Processinstansinformation:

* Välj en åtgärd om du vill visa information om den. När du väljer en åtgärd visas sidan Åtgärdsinformation.
* Välj en underprocess för att visa information om den. När du väljer en underprocess visas sidan Processinstansinformation.
* Avsluta eller försök igen, beroende på status.

### Om åtgärdsstatus {#about-operation-statuses}

En åtgärd (ett steg i en process) kan ha följande status:

**SLUTFÖRD:** Åtgärden har slutförts.

**KÖRS:** Åtgärden körs normalt. Den kan ta emot användarindata eller vänta på användarinteraktion, eller så kan ett automatiskt steg vara på gång.

**STALLED:** Ett problem uppstod när åtgärden bearbetades. Leta efter felet eller undantaget på sidan Installerade åtgärder.

**AVSLUTAD:** Åtgärden avbröts av en administratör.

### Avsluta operationer eller underprocesser {#terminate-operations-or-subprocesses}

Om en åtgärd eller en underprocess har avstannat eller påträffat något annat feltillstånd, eller om du behöver tvinga en åtgärd eller en underprocess att sluta köras, kan du avsluta den.

Du kan avsluta en åtgärd som är RUNNING.

När du avslutar en åtgärd ändras dess status till AVSLUTAD. Åtgärden slutförs inte och processinstansen avbryter körningen.

Du kan avsluta en underprocess som har vilken status som helst.

När du avslutar en underprocess ändras dess status till TERMINATING, sedan TERMINATED, och processinstansen stoppas vid den aktuella operationen. Inga fler åtgärder körs i underprocessen, men den överordnade processinstansen fortsätter att köras.

Du kan inte avsluta processer som har gatewayelement i processdiagrammet. Om du försöker avsluta den här typen av processer påverkas inte åtgärderna i gateway-elementen. Om du vill avsluta åtgärder som finns i ett gateway-element måste du avsluta åtgärderna direkt.

1. Klicka på fliken Åtgärder eller fliken Underprocesser på sidan Processinstansinformation.
1. Markera åtgärden eller underprocessen och klicka på Avsluta.

### Försök igen {#retry-an-operation}

Du kan försöka utföra en åtgärd som har statusen STALLED igen.

När du försöker utföra en åtgärd på nytt skickas en begäran om att starta om åtgärden från Forms. Om begäran lyckas ändras statusen till RUNNING. Om åtgärden inte kan startas om, förblir den STALLED och du kan behöva avsluta den.

1. Klicka på fliken Åtgärder på sidan Processinstansinformation.
1. Markera åtgärden och klicka på Försök igen.

## Arbeta med åtgärder {#working-with-operations}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

På sidan Åtgärdsinformation visas en sammanfattning av en åtgärd i en process och dess aktuella användartilldelningar.

1. I administrationskonsolen klickar du på Tjänster > Forms-arbetsflöde > Forms-arbetsflöde.
1. Klicka på ett processnamn för att visa dess processinstanser. Klicka på en processinstans för att visa sidan Processinstansinformation och välj sedan en åtgärd för att visa sidan Åtgärdsinformation.

   För varje uppgift visas följande information i listan:

   **Processnamn - Version:** Processens namn, enligt definition i Workbench.

   **Program:** Programmet som processen tillhör, enligt definition i Workbench.

   **Status:** Aktiv innebär att processen är den som aktiveras för processversionen. Inaktiv innebär att processen är en gammal version som fortfarande har processinstanser.

   **Skapad:** Datum och tid då processen distribuerades.
