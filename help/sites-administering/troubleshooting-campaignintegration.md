---
title: Felsökning av Adobe Campaign Classic-integrering
description: Lär dig hur du felsöker problem med Adobe Campaign Classic-integreringen.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: integration
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Integration
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '802'
ht-degree: 0%

---


# Felsökning av Adobe Campaign Classic-integrering{#troubleshooting-your-adobe-campaign-classic-integration}

Lär dig hur du felsöker problem med integreringen av Adobe Campaign Classic (ACC).

Följande felsökningstips hjälper dig att lösa de vanligaste problemen du kan stöta på när du integrerar AEM med ACC.

## Allmänna felsökningstips {#general-troubleshooting-tips}

Kontrollera om HTTP-samtal skickas och tas emot av båda lösningarna (AEM > Adobe Campaign Classic, Adobe Campaign Classic > AEM). Detta tips hjälper dig att undvika brandväggs-/SSL-problem.

* För AEM-funktioner ser du att JSON-anrop begärs från AEM författargränssnitt
   * Dessa anrop bör inte resultera i ett HTTP-500-fel.
   * Om du ser HTTP-500-fel bör du kontrollera `error.log` för mer information.
* Genom att höja felsökningsnivån för kampanjklasser i AEM kan du även felsöka problem.

## Om anslutningen misslyckas {#when-the-connection-fails}

Kontrollera att du har konfigurerat operatorn **`aemserver`** i Adobe Campaign Classic.

## Om bilder inte visas i Adobe Campaign Classic Console {#if-images-do-not-appear-in-the-adobe-campaign-console}

Kontrollera HTML-källan och bekräfta att du kan öppna URL:en från klientdatorn. Om URL:en innehåller `localhost:4503` ändrar du konfigurationen för Day CQ Link Externalizer på din AEM-författarinstans. Låt den peka på en publiceringsinstans som kan nås från Adobe Campaign Classic konsoldator.

Se [Konfigurera externalisering.](/help/sites-administering/campaignstandard.md#configuring-the-externalizer)

## Om du inte kan ansluta från AEM till Adobe Campaign Classic  {#if-you-cannot-connect-from-aem-to-adobe-campaign}

Sök efter följande felmeddelande i Adobe Campaign Classic.

* `No datasource defined in the instance 'default'.`

* `Make sure the DNS alias used to access the server is correct (for example, avoid hard-coded IP addresses). (iRc=16384)`

Åtgärda problemet genom att ändra följande i `$CAMPAIGN_HOME/conf/config-<instance-name>.xml`:

* `<dataStore hosts="*" lang="en_GB">`

## Om inga data visas i Adobe Campaign Classic-dialogrutan {#if-no-data-displays-in-the-adobe-campaign-dialog}

Kontrollera att det inte finns något avslutande snedstreck (`/`) efter portnumret i Adobe Campaign Classic.

![Adobe Campaign Classic - se till att inget avslutande snedstreck visas efter portnummer](assets/chlimage_1-149.png)

## Om du får en varning om språkområdet {#if-you-get-a-warning-about-your-setlocale}

När du startar Apache HTTPD-tjänsten för Adobe Campaign Classic kan felet `Warning: setlocale: LC_CTYPE cannot change locale` visas

Kontrollera att du har `en_CA.ISO-8859-15 locale` installerat på din Adobe Campaign Classic-server.

* Du kan kontrollera om det är installerat med `local -a`.
* Om det inte är installerat kan du korrigera skriptet `/usr/local/neolane/nl6/env.sh` och ändra språkinställningen till en installerad.

## Om du får ett fel när skriptet &#39;get_nms_amcGetSeedMetaData_jssp&#39; kompileras {#if-you-get-an-error-while-compiling-script-get-nms-amcgetseedmetadata-jssp}

Om följande felmeddelande visas i AEM loggfil:

`com.day.cq.mcm.campaign.impl.CampaignConnectorImpl Internal Adobe Campaign error: response body is Error while compiling script 'get_nms_amcGetSeedMetaData_jssp' line 45: String.prototype.toJSON called on incompatible XML.`

Använd följande lösning på Adobe Campaign Classic-servern.

1. Öppna filen `$CAMPAIGN_HOME/datakit/nms/fra/js/amcIntegration.js`
1. Ändra rad 467 i metod `amcGetSeedMetaData`
1. Ändra `label : [inclView.@label](mailto:inclView.@label)` till `label : String([inclView.@label](mailto:inclView.@label))`
1. Spara.
1. Starta om servern.

## Om Adobe Campaign Classic visar ett fel när du klickar på Synkronisera {#if-adobe-campaign-displays-an-error-when-clicking-the-synchronize-button}

När du klickar på knappen **Synkronisera** i Adobe Campaign Classic kan följande fel uppstå.

* `Error while executing the method 'aemListContent' of service [nms:delivery](https://nmsdelivery/)`

Kontrollera att det går att nå AEM-anslutnings-URL:en som konfigurerats i **externa konton** i Adobe Campaign Classic från datorn.

Ett byte från `localhost` till en IP-adress för URL-adressen kan ofta lösa problemet.

## Om du får ett &#39;Cannot parse XTK Date+Time &#39;undefined&#39;-fel {#if-you-get-a-cannot-parse-xtk-date-time-undefined-error}

När du har klickat på **Synkronisera** i AEM kan du få ett felmeddelande om att ett skript har inträffat på sidorna.

* `Cannot parse XTK Date+Time 'undefined': not a valid XTK value.`

Det här felet inträffar om det finns inaktuell Adobe Campaign Classic-information på AEM-instansen. Du kan lösa det här problemet genom att göra följande:

1. Ta bort alla integrationskonfigurationer för Adobe Campaign Classic som finns i AEM.
1. Återskapa integreringen.
1. Skapa en mall.

## Om en anslutning till SSL visar ett fel när Cloud Service konfigureras {#if-a-connection-to-ssl-displays-an-error-when-setting-up-the-cloud-service}

Skicka in en anmälan till Adobe Campaign supportteam om följande visas i `error.log` i AEM.

```text
javax.net.ssl.SSLProtocolException: handshake alert:  unrecognized_name
at sun.security.ssl.ClientHandshaker.handshakeAlert(Unknown Source)
at sun.security.ssl.SSLSocketImpl.recvAlert(Unknown Source)
at sun.security.ssl.SSLSocketImpl.readRecord(Unknown Source)
at sun.security.ssl.SSLSocketImpl.performInitialHandshake(Unknown Source)
at sun.security.ssl.SSLSocketImpl.writeRecord(Unknown Source)
at sun.security.ssl.AppOutputStream.write(Unknown Source)
```

## Om du ser HTTP i stället för förväntade HTTPS-länkar i synkroniseringsdialogrutan {#if-you-see-http-instead-of-an-expected-https-links-in-the-synchronization-dialog}

När AEM försöker synkronisera innehåll i Adobe Campaign Classic-leverans returneras en lista med nyhetsbrev. URL:erna till nyhetsbreven i listan kan dock vara HTTP-adresser i stället för HTTPS. När du väljer ett av objekten i listan inträffar ett fel. Det här felet kan inträffa med följande inställningar.

* Adobe Campaign som värd använder https för kommunikation med AEM Author
* Återför proxy som avslutar SSL
* Lokal instans av AEM Author

Så här löser du problemet:

* AEM Dispatcher eller omvänd proxy måste vara konfigurerade att skicka det ursprungliga protokollet som en rubrik.
* **Apache Felix Http Service SSL-filtret** i OSGi-konfigurationen för AEM måste konfigureras med de nödvändiga huvudinställningarna.
   * `https://<host>:<port>/system/console/configMgr`
   * Se [https://github.com/apache/felix-dev/tree/master/http#using-the-ssl-filter](https://github.com/apache/felix-dev/tree/master/http#using-the-ssl-filter)

## Det går inte att välja en anpassad mall i Sidegenskaper {#if-the-custom-template-i-created-cannot-be-selected-in-page-properties}

När du skapar en e-postmall i AEM för Adobe Campaign Classic måste du inkludera egenskapen `acMapping` med värdet `mapRecipient` i noden `jcr:content` i mallen. Om du inte gör det kan inte välja Adobe Campaign Classic-mallen i **Sidegenskaper** i AEM. Fältet verkar vara inaktiverat.

## Om felet com.day.cq.mcm.campaign.servlets.util.ParameterMapper visas i AEM-loggarna {#if-you-get-the-error-com-day-cq-mcm-campaign-servlets-util-parametermapper-in-your-logs}

Felet `com.day.cq.mcm.campaign.servlets.util.ParameterMapper` kan visas i AEM-loggarna när du använder en anpassad mall.

Det här felet inträffar om egenskapen `acMapping` har ett annat värde än `recipient.firstName`, så skapas ett tomt värde i Adobe Campaign Manager.

Om det här felet inträffar installerar du funktionspaket 6576 för AEM från [paketresurs](/help/sites-administering/package-manager.md#package-share).
