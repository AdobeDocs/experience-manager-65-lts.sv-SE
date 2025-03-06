---
title: Spåra processer
description: Spåra era processer genom att söka efter dem och visa deras information.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: Admin, User, Developer
exl-id: 4c456045-dbd1-491a-a136-3995ae51e629
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# Spåra processer {#tracking-processes}

På spårningssidan kan du söka efter aktiva eller slutförda processer som du har påbörjat eller deltagit i och visa processinformationen. Processinformationen visar uppgifter, uppdrag och formulär som ingår i processen. Du kan också starta nya processer med formulärdata från en process som du tidigare har initierat.

## Sök efter processer och uppgifter {#search-for-processes-and-tasks}

Du kan söka efter processinstanser och associerade uppgifter baserat på processnamn eller med hjälp av sökmallar som angetts av AEM Forms-arbetsyteadministratör.

Du kan ange vilka kolumner som ska visas i sökresultaten.

>[!NOTE]
>
>Sökresultaten inkluderar inte uppgifter som visas i en grupp eller delad lista som du har tillgång till, såvida du inte faktiskt har deltagit i uppgifterna. Resultatet innehåller inte slutförda processinstanser som administratören tömde.

### Sök efter processnamn {#search-by-process-name}

1. Välj ett processnamn i den vänstra rutan på spårningssidan. Alla instanser av den processen som du initierade eller slutförde en åtgärd för visas i huvudrutan.
1. Klicka på en processinstans om du vill visa mer information om den.

### Söka efter en uppgift med en sökmall {#search-for-a-task-using-a-search-template}

1. Välj **Sökmallar** i listan till vänster på spårningssidan och välj en sökmall.
1. Om mallen stöder sökparametrar kan du begränsa sökparametrarna genom att fylla i mallfälten och sedan klicka på **Sök**. Visar en lista med alla aktiviteter som du har deltagit i, som matchar sökvillkoren.

## Visa processinformation {#view-process-details}

På spårningssidan kan du välja en process och visa information om den. Du kan söka i processerna baserat på olika parametrar för att visa uppgiftsinformationen. Du kan även visa fliken Status för processer där flera användare tar emot uppgifter parallellt där verktygen för granskning av dokument är aktiverade.

**Status:** Statusen för uppgifter i en process visas i kolumnen Markerad åtgärd när du klickar på en aktivitet. Processens status är dock inte tillgänglig.

1. Välj processinstansen i sökresultatlistan om du vill visa information om de uppgifter som är en del av processinstansen.
1. Om du vill visa mer information om en uppgift utför du en eller flera av dessa åtgärder:

   * Om du vill visa anteckningar och bilagor för en uppgift klickar du på fliken Bifogade filer.
   * Om du vill visa information om uppgiftstilldelningen klickar du på fliken Uppdrag.
   * Om du vill visa det associerade formuläret klickar du på formulärknappen.
