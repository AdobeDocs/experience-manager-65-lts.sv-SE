---
title: Utvecklingsverktyg
description: Det finns flera verktygsuppsättningar för att utveckla dina JCR-, Apache Sling- eller Adobe Experience Manager-program.
contentOwner: msm-service
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: development-tools
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing,Developer Tools
role: Developer
exl-id: 46db0690-03e9-4b31-aa44-200f224f3707
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# Utvecklingsverktyg{#development-tools}

Följande verktygsuppsättningar är tillgängliga för att utveckla dina JCR-, Apache Sling- eller Adobe Experience Manager-program (AEM):

* en uppsättning bestående av [CRXDE Lite](/help/sites-developing/developing-with-crxde-lite.md) och WebDAV. CRXDE Lite är inbäddat i CRX/AEM och gör att du kan utföra standardutvecklingsuppgifter i webbläsaren. Med CRXDE Lite kan du skapa och redigera filer (som .jsp och .java), mappar, mallar, komponenter, dialogrutor, noder, egenskaper och paket när du loggar och integrerar med SVN.

  CRXDE Lite rekommenderas när du inte har direktåtkomst till CRX/AEM-servern, när du utvecklar ett program genom att utöka eller ändra körklara komponenter och Java™-paket eller när du inte behöver en dedikerad felsökare, kodkomplettering och syntaxmarkering.

* en uppsättning bestående av följande:
   * En integrerad utvecklingsmiljö. Exempel: [Eclipse](/help/sites-developing/howto-projects-eclipse.md) eller [IntelliJ](/help/sites-developing/ht-intellij.md).
   * Ett byggverktyg. Exempel: [Apache Maven](/help/sites-developing/ht-projects-maven.md).
   * FileVault som har utvecklats av Adobe för att mappa en databas till ett filsystem, ett versionskontrollsystem. Exempel: Subversion.
   * Ett felsökningssystem. Till exempel Jira.
   * Ett centralt system för beroendehantering. Exempel: Apache Archiva.
   * Och ett system för automatisering av bygge. Exempel: Apache Continuum.

  Med den här installationen kan du helt integrera programmet (innehåll, kod, konfiguration) i alla utvecklingsmiljöer och processer. Länken mellan de olika elementen är databasens filsystemrepresentation via FileVault, eftersom alla tidigare nämnda utvecklingsverktyg kan användas med filer.

## Tillägg för integrerade utvecklingsmiljöer {#extensions-for-integrated-development-environments}

Adobe har släppt följande tillägg:

* [AEM Eclipse-tillägg](/help/sites-developing/aem-eclipse.md)
* [AEM Brackets Extension](/help/sites-developing/aem-brackets.md)

### Andra verktyg {#other-tools}

AEM levereras med andra verktyg som underlättar utvecklingen:

* [Dialogruteredigeraren](/help/sites-developing/dialog-editor.md)
* [Använda översättare för att hantera ordlistor](/help/sites-developing/i18n-translator.md)
* [Hantera paket med Maven](/help/sites-developing/vlt-mavenplugin.md)
* [Utveckla AEM-projekt med Eclipse](/help/sites-developing/howto-projects-eclipse.md)
* [Skapa AEM-projekt med Apache Maven](/help/sites-developing/ht-projects-maven.md)
* [Utveckla AEM-projekt med IntelliJ IDEA](/help/sites-developing/ht-intellij.md)
* [Så här använder du VLT-verktyget](/help/sites-developing/ht-vlttool.md)
* [Så här använder du proxyserververktyget](/help/sites-developing/ht-proxy-server.md)
* [AEM Modernization Tools](/help/sites-developing/modernization-tools.md)
* [AEM Repo Tool](/help/sites-developing/aem-repo-tool.md)

Verktyg som underlättar skapandet av nya projekt:

* [AEM Project Archetype](https://github.com/adobe/aem-project-archetype)
* [AEM Lazybone Templates](https://github.com/Adobe-Consulting-Services/lazybones-aem-templates)

>[!NOTE]
>
>Följande självstudiekurser kan vara intressanta när du startar ett nytt AEM-projekt:
>[Komma igång med AEM Sites del 1 - Projektinställningar](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/part1.html)
