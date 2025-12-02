---
title: Åtgärda serialiseringsproblem i AEM
description: Lär dig hur du undviker serialiseringsproblem i AEM.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: Security
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Security
role: Admin
exl-id: eef69d02-2e88-4f44-98bb-d98fa297e3a2
source-git-commit: ffb467630ee061bce5a9cab4e8a311e70b2aeb38
workflow-type: tm+mt
source-wordcount: '914'
ht-degree: 0%

---

# Åtgärda serialiseringsproblem i AEM{#mitigating-serialization-issues-in-aem}

## Ökning {#overview}

AEM-teamet på Adobe samarbetade nära med öppen källkodsprojektet [NotSoSerial](https://github.com/kantega/notsoserial) för att hjälpa till att minska säkerhetsluckorna som beskrivs i **CVE-2015-7501**. NotSoSerial licensieras under [Apache 2-licensen](https://www.apache.org/licenses/LICENSE-2.0) och innehåller ASM-kod som licensieras under dess egen [BSD-liknande licens](https://asm.ow2.io/).

Agentbehållaren som ingår i paketet är Adobe modifierade distribution av NotSoSerial.

NotSoSerial är en Java™-nivålösning på ett Java™-nivåproblem och är inte AEM-specifikt. En preflight-kontroll läggs till i ett försök att avserialisera ett objekt. Den här kontrollen testar ett klassnamn mot en brandväggsliknande tillåtelselista, blockeringslista eller båda. På grund av det begränsade antalet klasser i standardvärdet blockeringslista är det osannolikt att det här testet påverkar ditt system eller din kod.

Som standard utför agenten en kontroll av blockeringslista mot aktuella kända sårbara klasser. Blockeringslista är avsett att skydda dig från den aktuella listan över explosioner som använder den här typen av sårbarhet.

Du kan konfigurera blockeringslista och tillåtelselista genom att följa instruktionerna i avsnittet [Konfigurera agenten](/help/sites-administering/mitigating-serialization-issues.md#configuring-the-agent) i den här artikeln.

Medlet är avsett att bidra till att minska de senaste kända sårbara klasserna. Om ditt projekt avserialiserar otillförlitliga data kan det fortfarande vara känsligt för denial of service-attacker, slut på minnesattacker och okända framtida avserialiseringsattacker.

Adobe stöder officiellt Java™ 6, 7 och 8. Adobe förståelse är dock att NotSoSerial även stöder Java™ 5.

>[!WARNING]
>
>NotSoSerial ska inte användas med Java™ 17 eller senare.

## Installera agenten {#installing-the-agent}

>[!NOTE]
>
>Om du tidigare har installerat serialiseringssnabbkorrigeringen för AEM 6.1 tar du bort agentens startkommandon från Java™-körningsraden.

1. Installera paketet **com.adobe.cq.cq-serialization-provare**.

1. Gå till webbkonsolen Bundle på `https://server:port/system/console/bundles`
1. Leta efter serialiseringspaketet och starta det. När du gör det laddas NotSoSerial-agenten automatiskt.

## Installera agenten på programservrar {#installing-the-agent-on-application-servers}

NotSoSerial-agenten ingår inte i standarddistributionen av AEM för programservrar. Du kan dock extrahera den från AEM jar-distributionen och använda den med programserverkonfigurationen:

1. Ladda först ned AEM snabbstartsfil och extrahera den:

   ```shell
   java -jar aem-quickstart-6.2.0.jar -unpack
   ```

1. Gå till platsen för den uppzippade snabbstarten för AEM och kopiera mappen `crx-quickstart/opt/notsoserial/` till mappen `crx-quickstart` i installationen av AEM-programservern.

1. Ändra ägarskapet för `/opt` till användaren som kör servern:

   ```shell
   chown -R opt <user running the server>
   ```

1. Konfigurera och kontrollera att agenten har aktiverats korrekt enligt följande avsnitt i den här artikeln.

## Konfigurera agenten {#configuring-the-agent}

Standardkonfigurationen räcker för de flesta installationer. Den här konfigurationen innehåller en blockeringslista med kända, fjärrkörda sårbara klasser och en tillåtelselista paket där avserialisering av tillförlitliga data är säkert.

Brandväggskonfigurationen är dynamisk och kan ändras när som helst genom att:

1. Gå till webbkonsolen på `https://server:port/system/console/configMgr`
1. Söker efter och klickar på **Konfiguration av brandvägg för deserialisering.**

   >[!NOTE]
   >
   >Du kan även nå konfigurationssidan direkt genom att gå till URL:en på:
   >
   >* `https://server:port/system/console/configMgr/com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`

Den här konfigurationen innehåller loggning av tillåtelselista, blockeringslista och deserialisering.

**Tillåt listning**

I avsnittet Tillåt är dessa listor klasser eller paketprefix som tillåts för deserialisering. Om du avserialiserar egna klasser lägger du till antingen klasserna eller paketen i den här tillåtelselista.

**Blocklista**

I avsnittet med blocklistor är klasser som aldrig tillåts för deserialisering. Den första uppsättningen av dessa klasser är begränsad till klasser som har befunnits sårbara för attacker på fjärrbasis. Blockeringslista används före alla poster i listan över tillåtna.

**Diagnostikloggning**

I avsnittet för diagnostikloggning kan du välja flera alternativ för loggning när deserialisering sker. De här alternativen är bara inloggade första gången och loggas inte igen vid efterföljande användningar.

Standardvärdet för **class-name-only** informerar dig om de klasser som avserialiseras.

Du kan också ange alternativet **full-stack** som loggar en Java™-stack om det första deserialiseringsförsöket för att informera dig om var deserialiseringen sker. Det här alternativet är användbart när du vill hitta och ta bort deserialisering från din användning.

## Verifiera agentens aktivering {#verifying-the-agent-s-activation}

Du kan verifiera deserialiseringsagentens konfiguration genom att gå till URL:en på:

* `https://server:port/system/console/healthcheck?tags=deserialization`

När du har öppnat URL-adressen visas en lista med hälsokontroller som är relaterade till agenten. Du kan avgöra om agenten har aktiverats på rätt sätt genom att verifiera att hälsokontrollerna har godkänts. Om de misslyckas måste du läsa in agenten manuellt.

Mer information om felsökning av problem med agenten finns i [Hantera fel med dynamisk agentinläsning](#handling-errors-with-dynamic-agent-loading) nedan.

>[!NOTE]
>
>Om du lägger till `org.apache.commons.collections.functors` i tillåtelselista misslyckas alltid hälsokontrollen.

## Hantera fel med dynamisk agentinläsning {#handling-errors-with-dynamic-agent-loading}

Om fel visas i loggen, eller om verifieringsstegen upptäcker ett problem med att läsa in agenten, läser du in agenten manuellt. Det här arbetsflödet rekommenderas också om du använder en JRE (Java™ Runtime Environment) i stället för en JDK (Java™ Development Toolkit), eftersom verktygen för dynamisk inläsning inte är tillgängliga.

Så här läser du in agenten manuellt:

1. Redigera JVM-startparametrarna för CQ-behållaren och lägg till följande alternativ:

   ```shell
   -javaagent:<aem-installation-folder>/crx-quickstart/opt/notsoserial/notsoserial.jar
   ```

   >[!NOTE]
   >
   >Kräver att du även använder alternativet -nofork CQ/AEM, tillsammans med lämpliga JVM-minnesinställningar, eftersom agenten inte är aktiverad på en forked JVM.

   >[!NOTE]
   >
   >Adobe-distributionen av NotSoSerial Agent-behållaren finns i mappen `crx-quickstart/opt/notsoserial/` i din AEM-installation.

1. Stoppa och starta om JVM,

1. Verifiera agentens aktivering igen genom att följa stegen som beskrivs ovan i [Verifiera agentens aktivering](/help/sites-administering/mitigating-serialization-issues.md#verifying-the-agent-s-activation).

## Andra överväganden {#other-considerations}

Om du kör på en IBM® JVM läser du dokumentationen om stöd för Java™ Attach API på [den här platsen](https://www.ibm.com/docs/en/sdk-java-technology/8?topic=documentation-java-attach-api).
