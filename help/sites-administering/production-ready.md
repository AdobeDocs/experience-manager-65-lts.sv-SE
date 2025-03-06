---
title: Köra AEM i produktionsklart läge
description: Lär dig hur du kör AEM i Production Ready Mode.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: Security
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Deploying
role: Admin
exl-id: 99724bd6-41b4-4491-9958-1f5d9e1f5050
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 1%

---

# Köra AEM i produktionsklart läge{#running-aem-in-production-ready-mode}

Med AEM 6.1 introducerar Adobe det nya körningsläget `"nosamplecontent"` som syftar till att automatisera de steg som krävs för att förbereda en AEM-instans för distribution i en produktionsmiljö.

Det nya körningsläget konfigurerar inte bara instansen automatiskt så att den följer de säkerhetsstandarder som beskrivs i checklistan, utan tar även bort alla Geometrixx-exempelprogram och -konfigurationer i processen.

>[!NOTE]
>
>Eftersom AEM Production Ready Mode av praktiska skäl endast omfattar de flesta uppgifter som krävs för att skydda en instans rekommenderar vi att du läser [checklistan](/help/sites-administering/security-checklist.md) innan du publicerar i produktionsmiljön.
>
>Observera också att om du kör AEM i Production Ready Mode inaktiveras åtkomsten till CRXDE Lite. Om du behöver det för felsökning läser du [Aktivera CRXDE Lite i AEM](/help/sites-administering/enabling-crxde-lite.md).

![chlimage_1-83](assets/chlimage_1-83a.png)

Om du vill köra AEM i produktionsklar läge behöver du bara lägga till `nosamplecontent` via körlägesväxeln `-r` i dina befintliga startargument:

```shell
java -jar aem-quickstart.jar -r nosamplecontent
```

Du kan t.ex. använda den produktion som är klar för att starta en författarinstans med MongoDB-beständighet så här:

```shell
java -jar aem-quickstart.jar -r author,crx3,crx3mongo,nosamplecontent -Doak.mongo.uri=mongodb://remoteserver:27017 -Doak.mongo.db=aem-author
```

## Ändrar en del av produktionsklart läge {#changes-part-of-the-production-ready-mode}

Mer specifikt utförs följande konfigurationsändringar när AEM körs i produktionsklart läge:

1. Supportpaketet **CRXDE** ( `com.adobe.granite.crxde-support`) är inaktiverat som standard i produktionsklart läge. Den kan installeras när som helst från Adobe publika Maven-databas. Version 3.0.0 krävs för AEM 6.1.

1. Paketet **Apache Sling Simple WebDAV Access till databaser** ( `org.apache.sling.jcr.webdav`) är bara tillgängligt för **author**-instanser.

1. Nyligen skapade användare måste ändra lösenordet vid den första inloggningen. Detta gäller inte administratörsanvändaren.
1. **Generera felsökningsinformation** har inaktiverats för **Apache Sling JavaScript Handler**.

1. **Mappat innehåll** och **Generera felsökningsinformation** är inaktiverade för **Apache Sling JSP Script Handler**.

1. CQ WCM-filtret **för** dag är inställt på `edit` för **författare** och `disabled` för **publiceringsinstanser**.

1. **Adobe Granite HTML Library Manager** har konfigurerats med följande inställningar:

   1. **Minify:** `enabled`
   1. **Felsök:** `disabled`
   1. **GZIP:** `enabled`
   1. **Timing:** `disabled`

1. **Apache Sling GET Servlet** är inställt på att som standard stödja säkra konfigurationer enligt följande:

| **Konfiguration** | **Författare** | **Publicera** |
|---|---|---|
| TXT-rendering | inaktiverad | inaktiverad |
| HTML rendering | inaktiverad | inaktiverad |
| JSON-rendering | aktiverad | aktiverad |
| XML-rendering | inaktiverad | inaktiverad |
| json.maximumresults | 1000 | 100 |
| Automatiskt index | inaktiverad | inaktiverad |
