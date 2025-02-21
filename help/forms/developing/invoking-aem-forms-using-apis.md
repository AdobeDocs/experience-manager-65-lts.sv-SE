---
title: Hur anropar jag AEM Forms med API:er?
description: Lär dig hur du anropar AEM Forms-tjänster med en Java&handel; API, webbtjänster, Remoting och REST.
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: coding, development-tools
role: Developer
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 0%

---

# Anropa AEM Forms med API:er {#invoking-aem-forms-using-apis}

**Exempel och exempel i det här dokumentet gäller endast för AEM Forms i JEE-miljö.**

Adobe Experience Manager Forms är en J2EE-baserad företagsprogramvara som består av tjänster som fungerar i en delad infrastruktur. Serviceåtgärder brukar förbruka eller producera dokument. Med AEM Forms kan man kombinera Forms Workflow med elektroniska blanketter, dokumentsäkerhet och dokumentgenerering i en integrerad och sammanhängande uppsättning tjänster. Dessa tjänster kan nås inifrån och utanför brandväggen.

Klientapplikationer kan programmatiskt anropa AEM Forms-tjänster med Java™ API, webbtjänster, Remoting och REST. Med administrationskonsolen kan du konfigurera en tjänst så att en slutpunkt visas som gör att AEM Forms-tjänster kan anropas via programmering. Som standard är de flesta tjänster förkonfigurerade för att visa slutpunkter för Remoting, Java™ och webbtjänster.

Dina affärskrav avgör vilken anropsmetod som ska användas. Med Java™ API kan du t.ex. integrera AEM Forms-funktioner i Java™-företagsapplikationer som Java™ Entity och Message Beans. På samma sätt kan du integrera AEM Forms-funktionalitet i .NET-projekt (eller andra projekt som utvecklats med utvecklingsmiljöer som stöder webbtjänststandarder) med hjälp av webbtjänster.

Tjänsterna kräver en tjänstbehållare för att köras, ungefär som Enterprise JavaBeans™ (EJB) kräver en J2EE-behållare. AEM Forms innehåller endast en implementering av en tjänstbehållare. Tjänstbehållaren ansvarar för att hantera tjänstens livstid, inklusive distribuering, och ser till att alla begäranden skickas till rätt tjänst. Den hanterar också dokument som en tjänst förbrukar eller producerar.

>[!NOTE]
>
>Programmering med AEM-formulär innehåller ingen information om hur du anropar AEM Forms med Bevakade mappar eller e-post.
