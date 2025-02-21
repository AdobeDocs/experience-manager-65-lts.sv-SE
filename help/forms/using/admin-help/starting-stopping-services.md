---
title: Starta och stoppa tjänster
description: Lär dig hur du startar och stoppar tjänster som är kopplade till AEM Forms-moduler samt programservern och databasen.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/managing_services
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# Starta och stoppa tjänster {#starting-and-stopping-services}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Det finns två typer av tjänster som ingår i AEM-formulären:

* Tjänster som styr AEM blankettserver och databas.
* Tjänster som styr AEM blankettmoduler

## Starta eller stoppa tjänster som är kopplade till AEM formulärmoduler {#start-or-stop-the-services-associated-with-aem-forms-modules}

AEM formulärmoduler (till exempel Forms, Rights Management, Output) fungerar som tjänster. Ibland kan du behöva stoppa eller starta tjänsterna för dessa AEM-formulärmoduler. Du måste till exempel stoppa och sedan starta om en AEM-formulärtjänst när du har ändrat en inställning för tjänsten.

>[!NOTE]
>
> Du bör använda kommandot Ctrl + C för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.

1. I administrationskonsolen klickar du på **Tjänster** > **Program och tjänster** > **Tjänsthantering**.
1. Markera kryssrutan bredvid tjänsten som ska stoppas eller startas på sidan Tjänsthantering och klicka på Stopp eller Start.

## Starta eller stoppa tjänster för programservern och databasen {#start-or-stop-services-for-the-application-server-and-database}

En komplett implementering av AEM-blanketter innefattar en programserver och databastjänster:

* *`[application server]`* för AEM-formulär
* *`[database]`* för AEM-formulär

I Windows är dessa tjänster tillgängliga via **Administrationsverktyg** > **Tjänstpanelen**. Om du t.ex. har installerat AEM-formulär på JBoss med körningsmetoden är följande tjänster tillgängliga på datorn:

* JBoss för Adobe Experience Manager-formulär
* MySQL för Adobe Experience Manager-formulär

Starta eller stoppa dessa tjänster genom att markera dem i listan på panelen Tjänster och sedan klicka på lämplig åtgärdsknapp på panelen.

I UNIX® eller Linux anger du följande text från en kommandorad, där *`[service name]`* är namnet på den tjänst som du verifierar:

```java
     ps -A | grep [service name]
```
