---
title: Krav för integrering med Adobe Target
description: Läs om förutsättningarna för integrering med Adobe Target.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: integration
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Integration
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# Krav för integrering med Adobe Target{#prerequisites-for-integrating-with-adobe-target}

Som en del av [integreringen av AEM och Adobe Target](/help/sites-administering/target.md) måste du registrera dig hos Adobe Target, konfigurera replikeringsagenten och säkra aktivitetsinställningar på publiceringsnoden.

## Registrering hos Adobe Target {#registering-with-adobe-target}

Om du vill integrera AEM med Adobe Target måste du ha ett giltigt Adobe Target-konto. Det här kontot måste ha minst behörighet på **godkännarnivå**. När du registrerar dig hos Adobe Target får du en klientkod. Du behöver klientkoden och inloggningsnamnet och lösenordet för Adobe Target för att ansluta AEM till Adobe Target.

Klientkoden identifierar Adobe Target-kundkontot när Adobe Target-servern anropas.

>[!NOTE]
>
>Ditt konto måste också aktiveras av Target-teamet för att integreringen ska kunna användas.
>
>Kontakta [Adobe kundtjänst](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html) om så inte är fallet.

## Aktivera målreplikeringsagenten {#enabling-the-target-replication-agent}

Test- och Target [replication agent](/help/sites-deploying/replication.md) måste vara aktiverat på författarinstansen. Observera att den här replikeringsagenten inte är aktiverad som standard om du använde körningsläget [nosamplsinnehåll](/help/sites-deploying/configure-runmodes.md#using-samplecontent-and-nosamplecontent) för att installera AEM. Mer information om hur du skyddar din produktionsmiljö finns i [checklistan för säkerhet](/help/sites-administering/security-checklist.md).

1. På AEM hemsida klickar du på **Verktyg** > **Distribution** > **Replikering**.
1. Klicka på **Agenter på författare**.
1. Klicka på replikeringsagenten **Test och Target (test och mål)** och klicka sedan på **Redigera**.
1. Välj alternativet Aktiverad och klicka sedan på **OK**.

   >[!NOTE]
   >
   >När du konfigurerar replikeringsagenten Test och Target, på fliken **Transport**, anges URI som standard till **tnt:///**. Ersätt inte denna URI med **https://admin.testandtarget.omniture.com**.
   >
   >Om du försöker testa anslutningen med **tnt:///** genereras ett fel. Detta är förväntat eftersom denna URI endast är avsedd för internt bruk. Använd inte med **Testa anslutning**.

## Skydda noden Aktivitetsinställningar {#securing-the-activity-settings-node}

Skydda aktivitetsinställningsnoden **cq:ActivitySettings** på publiceringsinstansen så att den inte är tillgänglig för vanliga användare. Noden för aktivitetsinställningar ska bara vara tillgänglig för tjänsten som hanterar aktivitetssynkroniseringen till Adobe Target.

Noden **cq:ActivitySettings** är tillgänglig i CRXDE lite under `/content/campaigns/*nameofbrand*`* *under aktivitetsnoden jcr:content;* *till exempel, `/content/campaign/we-retail/master/myactivity/jcr:content/cq:ActivitySettings`. Den här noden skapas bara efter att du har angett en komponent som mål.

Noden **cq:ActivitySettings** under aktivitetens jcr:content skyddas av följande åtkomstkontrollistor:

* Neka alla för alla
* Tillåt jcr:read,rep:write för&quot;target-activity-authors&quot; (författaren är medlem i den här gruppen utanför rutan)
* Tillåt jcr:read,rep:write för &quot;target service&quot;

Dessa inställningar ser till att normala användare inte har tillgång till nodegenskaperna. Använd samma åtkomstkontrollista vid författare och publicering. Mer information finns i [Användaradministration och -säkerhet](/help/sites-administering/security.md).

## Konfigurera AEM Link Externalizer {#configuring-the-aem-link-externalizer}

När du redigerar en aktivitet i Adobe Target pekar URL:en på **localhost** om du inte ändrar URL:en på AEM författarnod. Du kan konfigurera AEM Link Externalizer om du vill att det exporterade innehållet ska peka mot en specifik *publiceringsdomän* .

>[!NOTE]
>
>Se även [Lägg till molnkonfigurationen](/help/sites-administering/experience-fragments-target.md#add-the-cloud-configuration).

Så här konfigurerar du AEM externalizer:

>[!NOTE]
>
>Mer information finns i [Externalisera URL:er](/help/sites-developing/externalizer.md).

1. Gå till OSGi-webbkonsolen på **https://&lt;server>:&lt;port>/system/console/configMgr.**
1. Sök efter **Day CQ Link Externalizer** och ange domänen för författarnoden.

   ![Dagens CQ Link Externalizer](assets/aem-externalizer-01.png)
