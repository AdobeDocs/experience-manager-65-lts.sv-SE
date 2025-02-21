---
title: Förstå AEM Forms processer
description: Läs om hur AEM Forms processer innefattar blankettgenerering, inlämning, datahantering, validering, integrering, automatisering av arbetsflöden och hantering av utdata.
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: development-tools, coding
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '793'
ht-degree: 0%

---

# Förstå AEM Forms processer {#understanding-aem-forms-processes}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

Ett vanligt användningsexempel är en uppsättning AEM Forms-tjänster som kan användas på ett och samma dokument. Du kan skicka en begäran till tjänstbehållaren genom att skapa en process med Workbench. En process representerar en affärsprocess som du automatiserar. Mer information om hur du skapar processer finns i [Använda Workbench](https://www.adobe.com/go/learn_aemforms_workbench_63).

När en process har aktiverats blir den en tjänst och kan anropas som andra tjänster. En skillnad mellan en standardtjänst, som krypteringstjänsten och en tjänst som kommer från en process, är att den senare har en åtgärd som utför många åtgärder. En standardtjänst har däremot många åtgärder. Varje åtgärd utför vanligtvis en åtgärd, till exempel att tillämpa en profil på ett dokument eller kryptera ett dokument.

Processerna kan vara kortvariga eller långvariga. En kortlivad process är en åtgärd som utförs synkront och på samma körningstråd som den anropades från. Kortlivade åtgärder är jämförbara med standardbeteendet som finns i de flesta programmeringsspråk, där ett klientprogram anropar en metod och väntar på ett returvärde.

Det finns dock situationer där en process inte kan slutföras synkront på grund av faktorer som:

* En process kan omfatta en hel del tid.
* En process kan omfatta flera organisatoriska gränser.
* En process behöver externa indata för att den ska kunna slutföras. Tänk dig till exempel en situation där ett formulär skickas till en chef som inte är på kontoret. I det här fallet är processen inte slutförd förrän hanteraren returnerar och fyller i formuläret.

  Dessa typer av processer kallas långvariga processer. En långvarig process utförs asynkront, vilket gör att systemen kan interagera när resurserna tillåter det, och som gör det möjligt att spåra och övervaka operationen. När en långvarig process anropas skapar AEM Forms ett anrops-ID-värde som en del av en post som spårar den långvariga processens status. Posten lagras i AEM Forms-databasen. Du kan rensa långvariga processposter när de inte längre behövs.

>[!NOTE]
>
>AEM Forms skapar inte en post när en kort process anropas.

Med hjälp av anropsidentifierarvärdet kan du spåra den långvariga processens status. Du kan till exempel använda processens identifierarvärde för anrop för att utföra processhanteraråtgärder som att avsluta en pågående processinstans.

**Exempel på en kortlivad process**

Följande bild är ett exempel på en kortlivad process som heter *MyApplication/EncryptDocument*.

>[!NOTE]
>
>Processen bygger inte på någon befintlig AEM Forms-process. Om du vill följa med i kodexemplen som beskriver hur du anropar den här processen skapar du en process med namnet `MyApplication/EncryptDocument` med Workbench. (Se [Använda Workbench](https://www.adobe.com/go/learn_aemforms_workbench_63).)

När den här kortvariga processen anropas utför den följande åtgärder:

1. Hämtar det oskyddade PDF-dokumentet som skickas till processen som ett indatavärde.
1. Krypterar PDF-dokumentet med ett lösenord. Namnet på indataparametern för den här processen är `inDoc` och datatypen är dokument.
1. Sparar det lösenordskrypterade PDF-dokumentet som en PDF-fil i det lokala filsystemet. Den här processen returnerar det krypterade PDF-dokumentet som ett utdatavärde. Namnet på utdataparametern för den här processen är `outDoc` och datatypen är dokument.

   Den här processen slutförs synkront på samma körningstråd som den anropades från. Namnet på den här kortlivade processen är `MyApplication/EncryptDocument` och dess åtgärd är `invoke`.

   >[!NOTE]
   >
   >Vanligtvis består en kort process av mer än tre åtgärder. Du skapar en process med Workbench. (Se [Använda Workbench](https://www.adobe.com/go/learn_aemforms_workbench_63).)

   *Programmering med AEM-formulär* beskriver följande sätt på vilka du programmässigt kan anropa den här korta processen:

   * [Anropar en kort process genom att skicka ett osäkert dokument med AEM Forms Remoting](/help/forms/developing/invoking-aem-forms-using-remoting.md#invoking-a-short-lived-process-by-passing-an-unsecure-document-using-remoting) (Använda ett Flex-program)
   * [Anropar en kort process med anrops-API:t ](/help/forms/developing/invoking-aem-forms-using-java.md#invoking-a-short-lived-process-using-the-invocation-api) (Java™ Anvocation API)
   * [Anropar AEM Forms med Base64-kodning](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-base64-encoding) (webbtjänstexempel)
   * [Anropar AEM Forms med MTOM](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-mtom) (webbtjänstexempel)
   * [Anropar AEM Forms med SwaRef](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-swaref) (webbtjänstexempel)
   * [Anropar AEM Forms med BLOB-data över HTTP](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-blob-data-over-http) (webbtjänstexempel)
   * [Anropar AEM Forms med DIME](/help/forms/developing/invoking-aem-forms-using-web.md#invoking-aem-forms-using-dime) (webbtjänstexempel)
   * [Anropa processen MyApplication/EncryptDocument med REST](/help/forms/developing/invoking-aem-forms-using-rest.md)

**Exempel på långvarig process**

Följande bild är ett exempel på en långvarig process.

Denna process anropas när en sökande lämnar in en låneblankett. Processen är inte slutförd förrän en lånehandläggare godkänner eller avvisar låneansökan. Namnet på den här långvariga processen är *FirstAppSolution/PreLoanProcess* och åtgärden är `invoke_Async`. Den här processen måste anropas asynkront. Mer information om att programmatiskt anropa den här långvariga processen finns i [Anropa humancentrerade, långlivade processer](/help/forms/developing/invoking-human-centric-long-lived.md#invoking-human-centric-long-lived-processes).

>[!NOTE]
>
>Du kan skapa den här processen genom att följa den självstudiekurs som anges i [Skapa ditt första AEM Forms-program](https://www.adobe.com/go/learn_aemforms_firstapp_ds_63).
