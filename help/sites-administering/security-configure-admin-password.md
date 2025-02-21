---
title: Konfigurera administratörslösenordet vid installationen
description: Lär dig hur du ändrar administratörslösenordet vid installation av Adobe Experience Manager.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: Security
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Security
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Konfigurera administratörslösenordet vid installationen{#configure-the-admin-password-on-installation}

## Ökning {#overview}

Sedan version 6.3 tillåter Adobe Experience Manager (AEM) att administratörslösenordet anges med kommandoraden när en ny instans installeras.

I tidigare versioner av AEM behövde lösenordet för administratörskontot, tillsammans med lösenordet för olika andra konsoler, ändras efter installationen.

Den här funktionen lägger till möjligheten att ange ett nytt administratörslösenord för databasen och servermotorn under installationen av en AEM-instans, vilket eliminerar behovet av att göra det manuellt efteråt.

>[!CAUTION]
>
>Funktionen täcker inte Felix Console, för vilken lösenordet måste ändras manuellt. Mer information finns i avsnittet [Säkerhetschecklista](/help/sites-administering/security-checklist.md#change-default-passwords-for-the-aem-and-osgi-console-admin-accounts).

## Hur använder jag den? {#how-do-i-use-it}

Den här funktionen aktiveras automatiskt om du väljer att installera AEM via kommandoraden, i stället för att dubbelklicka på JAR från en filsystemutforskare.

Den allmänna syntaxen för att köra en AEM-instans från kommandoraden är:

```shell
java -jar aem6.3.jar
```

När du har kört instansen från kommandoraden får du alternativet att ändra administratörslösenordet under installationsprocessen:

![chlimage_1-116](assets/chlimage_1-116a.png)

>[!NOTE]
>
>Uppmaningen att ändra administratörslösenordet visas bara under installationen av en ny AEM-instans.

## Använda flaggan -nointeractive {#using-the-nointeractive-flag}

Du kan också välja att ange lösenordet från en egenskapsfil. Detta görs genom att använda flaggan `-nointeractive` i kombination med systemegenskapen `-Dadmin.password.file`.

Nedan visas ett exempel:

```shell
java -Dadmin.password.file =/path/to/passwordfile.properties -jar aem6.3.jar -nointeractive
```

Lösenordet i filen `passwordfile.properties` måste ha följande format:

```xml
admin.password = 12345678
```

>[!NOTE]
>
>Om du bara använder parametern `-nointeractive` utan systemegenskapen `-Dadmin.password.file` används standardadministratörslösenordet i AEM utan att du ombeds ändra det, vilket i huvudsak replikerar beteendet från tidigare versioner. Det här icke-interaktiva läget kan användas för automatiska installationer med kommandoraden i ett installationsskript.
