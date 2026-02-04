---
title: Söker efter processinstanser
description: Använd sidan Processsökning om du vill ange sökvillkor för att hitta en processinstans.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_forms_workflow
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: e358ee51-c23f-4737-9dcf-3193ed541bbb
source-git-commit: 51342861dd01e659999c19fbe0274e8d3cbcf8c4
workflow-type: tm+mt
source-wordcount: '458'
ht-degree: 0%

---

# Söker efter processinstanser{#searching-for-process-instances}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Använd sidan Processsökning om du vill ange sökvillkor för att hitta en processinstans. Du kommer åt sidan Processsökning på Forms Workflow-sidan. Du kan också klicka på **Sök** på processinstanssidan.

Du kan ange grundläggande villkor för att utföra en allmän sökning, särskilda attribut för att utföra en detaljerad sökning eller en kombination av grundläggande villkor och specifika attribut för att utföra en kombinerad sökning.

## Utför en allmän sökning {#perform-a-general-search}

En allmän sökning efter en process är lämpligast om du känner till process-ID:t för processinstansen. Eller om du söker efter en grupp med relaterade processinstanser eller om bara ett fåtal processinstanser körs.

Ange de grundläggande villkoren för att utföra en allmän sökning. Om du anger flera villkor utförs sökningen med ett underförstått AND-villkor.

1. I administrationskonsolen klickar du på Tjänster > Forms Workflow > Processsökning.
1. Ange följande villkor under Allmän sökning på sidan Processsökning:

   * **Process-ID:** Det positiva heltal som identifierar varje unik processinstans.
   * **Processstatus:** Välj en status i listan.
   * **Program:** Välj ett program i listan. Endast distribuerade program visas.
   * **Processnamn - Version:** Välj ett processnamn på menyn. Endast distribuerade processer visas.

1. Klicka på **Sök**. Sidan Processinstans visas med en lista över de instanser som hittats.

## Utför en detaljerad sökning efter en process {#perform-a-detailed-search-for-a-process}

Du kan ange specifika attribut för att göra en detaljerad sökning. En detaljerad sökning passar bäst om du har många processinstanser igång och du måste begränsa antalet möjliga sökningar efter vissa villkor.

1. I administrationskonsolen klickar du på Tjänster > Forms Workflow > Processsökning.
1. Ange din första villkorsuppsättning under Detaljerad sökning på sidan Processsökning:

   * Välj ett attribut i attributlistan.
   * Välj en operator i filterlistan.
   * I rutan Värde anger du ett värde som passar det attribut du har valt.

1. Om du vill lägga till ytterligare en rad väljer du Fler filter. En annan uppsättning med attribut-, filter- och värdeslistor visas och en villkorslista.
1. Välj OCH eller ELLER under Villkor. Upprepa steg 1-3 om det behövs för att begränsa sökningen ytterligare.
1. Om du vill lägga till eller ta bort rader klickar du på Fler filter eller Färre filter. Du kan ha mellan en och fyra rader.
1. Klicka på **Sök**. Sidan Processinstans visas med en lista över de instanser som hittats.

Se även [Om processinstansstatus](/help/forms/using/admin-help/processes.md#about-process-instance-statuses).

## Utför en kombinerad sökning efter en process {#perform-a-combined-search-for-a-process}

Om du vill skapa en sökning som använder både allmänna och detaljerade villkor anger du värden i båda områdena på sidan Processsökning. Systemet tillämpar en implicit `AND` mellan de två områdena.

Om sökningen är för smal går det inte att hitta några instanser.
