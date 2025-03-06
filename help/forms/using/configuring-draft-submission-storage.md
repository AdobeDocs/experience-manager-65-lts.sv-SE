---
title: Konfigurera lagringstjänster för utkast och överföringar
description: Lär dig hur du konfigurerar lagring för utkast och överföringar
topic-tags: publish
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Forms Portal
role: Admin, User, Developer
exl-id: 33769e4f-2213-442b-bd1c-1728cd917460
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 0%

---

# Konfigurera lagringstjänster för utkast och överföringar {#configuring-storage-services-for-drafts-and-submissions}

## Ökning {#overview}

Med AEM Forms kan du lagra:

* **Utkast**: Ett pågående formulär som slutanvändarna fyller i och sparar för senare och skickar därefter.

* **Sändningar**: Skickade formulär som innehåller data från användare.

AEM Forms portaldata och metadatatjänster har stöd för utkast och inlämning. Som standard lagras data i publiceringsinstansen, som sedan återreplikeras till den konfigurerade författarinstansen så att den blir tillgänglig för körning till andra publiceringsinstanser.

Problemet med den befintliga användningsklara metoden är att den lagrar alla data i publiceringsinstansen, inklusive data som kan vara personligt identifierbar information (PII).

Utöver det ovan nämnda standardtillvägagångssättet finns det även en alternativ implementering för att direkt överföra formulärdata till bearbetning i stället för att spara dem lokalt. Kunder som oroar sig för lagring av potentiellt känsliga data på en publiceringsinstans kan välja den alternativa implementering som data skickas till en bearbetningsserver. Eftersom bearbetningen sker på författarinstansen ligger den vanligtvis kvar i en säker zon.

>[!NOTE]
>
>När du använder Forms Portal-åtgärden eller aktiverar alternativet Lagra data i formulärportalen i anpassat format, lagras formulärdata i AEM-databasen. I en produktionsmiljö bör du inte lagra utkast eller inskickade formulärdata i AEM-databasen. Istället måste ni integrera utkasten och skicka-komponenten med en säker lagringsplats, t.ex. en företagsdatabas, för att lagra utkast och skickade formulärdata.
>
>Mer information finns i [Exempel på hur du integrerar komponenter för utkast och överföringar med databas](/help/forms/using/integrate-draft-submission-database.md).

## Konfigurera Forms Portal-utkast och inskickningstjänster {#configuring-forms-portal-drafts-and-submissions-services}

Öppna **Forms Portal Draft and Submission Configuration** i redigeringsläge genom att klicka på `https://[host]:'port'/system/console/configMgr` i AEM Web Console Configuration.

Ange värden för egenskaper baserat på dina krav enligt beskrivningen nedan:

### Körklara tjänster för lagring av data i publiceringsinstansen {#out-of-the-box-services-to-store-data-on-publish-instance}

Data replikeras omvänt till den konfigurerade författarinstansen.

<table>
 <tbody>
  <tr>
   <th>Egenskap</th>
   <th>Värde</th>
  </tr>
  <tr>
   <td>Forms Portal Draft Data Service (identifierare för Project Data Service (<strong>draft.data.service</strong>))</td>
   <td>com.adobe.fd.fp.service.impl.DraftDataServiceImpl<br /> </td>
  </tr>
  <tr>
   <td>Forms Portal Draft Metadata Service (identifierare för utkast till metadatatjänst (<strong>draft.metadata.service</strong>))</td>
   <td>com.adobe.fd.fp.service.impl.DraftMetadataServiceImpl<br /> </td>
  </tr>
  <tr>
   <td>Forms Portal Submit Data Service (identifierare för skicka data-tjänst (<strong>submit.data.service</strong>))</td>
   <td>com.adobe.fd.fp.service.impl.SubmitDataServiceImpl<br /> </td>
  </tr>
  <tr>
   <td>Forms Portal Submit Metadata Service (identifierare för tjänsten Skicka metadata (<strong>submit.metadata.service</strong>))</td>
   <td>com.adobe.fd.fp.service.impl.SubmitMetadataServiceImpl<br /> </td>
  </tr>
 </tbody>
</table>

### Utgångstjänster för lagring av data på fjärrbearbetningsinstansen {#out-of-the-box-services-to-store-data-on-remote-processing-instance}

Data skickas direkt till konfigurerad fjärrinstans

<table>
 <tbody>
  <tr>
   <th>Egenskap</th>
   <th>Värde</th>
  </tr>
  <tr>
   <td>Forms Portal Draft Data Service (identifierare för Project Data Service (<strong>draft.data.service</strong>))</td>
   <td>com.adobe.fd.fp.service.impl.DraftDataServiceRemoteImpl<br /> </td>
  </tr>
  <tr>
   <td>Forms Portal Draft Metadata Service (identifierare för utkast till metadatatjänst (<strong>draft.metadata.service</strong>))</td>
   <td>com.adobe.fd.fp.service.impl.DraftMetadataServiceRemoteImpl<br /> </td>
  </tr>
  <tr>
   <td>Forms Portal Submit Data Service (identifierare för skicka data-tjänst (<strong>submit.data.service</strong>))</td>
   <td>com.adobe.fd.fp.service.impl.SubmitDataServiceRemoteImpl<br /> </td>
  </tr>
  <tr>
   <td>Forms Portal Submit Metadata Service (identifierare för tjänsten Skicka metadata (<strong>submit.metadata.service</strong>))</td>
   <td>com.adobe.fd.fp.service.impl.SubmitMetadataServiceRemoteImpl<br /> </td>
  </tr>
 </tbody>
</table>

Förutom konfigurationen ovan anger du information om den konfigurerade fjärrbearbetningsinstansen.

Öppna **AEM DS-inställningstjänsten** i redigeringsläge genom att klicka på `https://[host]:'port'/system/console/configMgr` i AEM Web Console-konfigurationen. I dialogrutan AEM DS-inställningstjänst anger du information om hur server-URL bearbetas, användarnamn och lösenord för servern.

>[!NOTE]
>
>Ett exempel på implementering finns också för lagring av användardata i en databas. Mer information om hur du konfigurerar data- och metadatatjänster för att lagra användardata i en extern databas finns i [Exempel på hur du integrerar komponenter för utkast och överföringar med databas](/help/forms/using/integrate-draft-submission-database.md).
