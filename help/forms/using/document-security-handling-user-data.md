---
title: Dokumentsäkerhet | Hantera användardata
description: Läs om hur du med AEM Forms Document Security kan hantera användardata och datalager samt få tillgång till, ta bort och exportera användardata.
topic-tags: grdp
products: SG_EXPERIENCEMANAGER/6.5/FORMS
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Document Security,Adaptive Forms
exl-id: c619a3b8-cd06-4f5d-af20-67f3a4bfcdce
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '908'
ht-degree: 0%

---

# Dokumentsäkerhet | Hantera användardata {#document-security-handling-user-data}

Med AEM Forms dokumentsäkerhet kan du skapa, lagra och använda fördefinierade säkerhetsinställningar i dina dokument. Det säkerställer att endast behöriga användare kan använda dokumenten. Du kan skydda dokument med hjälp av profiler. En profil är en samling information som innehåller säkerhetsinställningar och en lista över behöriga användare. Du kan tillämpa en profil på ett eller flera dokument och auktorisera användare som läggs till i AEM Forms JEE-användarhantering.

<!-- Fix broken link For more information about how document security works, see AEM Forms JEE administration help. -->

## Användardata och datalager {#user-data-and-data-stores}

Dokumentsäkerhet lagrar profiler och data som rör skyddade dokument, inklusive användardata i en databas, t.ex. My SQL, Oracle, MS® SQL Server och IBM® DB2®. Dessutom lagras data för behöriga användare i en princip i användarhanteringen. Mer information om data som lagras i användarhantering finns i [Forms användarhantering: Hantera användardata](/help/forms/using/user-management-handling-user-data.md).

Följande tabell visar hur dokumentsäkerhet organiserar data i databastabeller.

<table>
 <tbody>
  <tr>
   <td>Databas</td>
   <td>Beskrivning</td>
  </tr>
  <tr>
   <td><code>EdcPrincipalKeyEntity</code></td>
   <td>Lagrar information om huvudnycklar för användarna. Nycklarna används i offline-arbetsflöden för dokumentsäkerhet.</td>
  </tr>
  <tr>
   <td><code>EdcAuditEntity</code></td>
   <td>Lagrar information om granskningshändelser som användarhändelser, dokumenthändelser och principhändelser.</td>
  </tr>
  <tr>
   <td><p><code>EdcLicenseEntity</code></p> </td>
   <td>Lagrar post för ett skyddat dokument. Den lagrar licensinformation för alla skyddade dokument.</td>
  </tr>
  <tr>
   <td><p><code>EdcDocumentEntity</code></p> </td>
   <td>Lagrar dokumentnamn för alla licenser som skapas i systemet.</td>
  </tr>
  <tr>
   <td><p><code>EdcRevokationEntity</code></p> </td>
   <td>Lagrar information om återkallande och återinförande av skyddade dokument.</td>
  </tr>
  <tr>
   <td><code>EdcMyPolicyListEntity</code></td>
   <td>Lagrar information om användare som kan skapa personliga profiler som visas på fliken Mina profiler på sidan Profiler. </td>
  </tr>
  <tr>
   <td><code>EdcPolicyEntity</code></td>
   <td>Lagrar information om profiler. Varje princip motsvarar en rad i den här tabellen.</td>
  </tr>
  <tr>
   <td><code>EdcPolicyXmlEntity</code></td>
   <td>Lagrar XML-filer för aktiva profiler. En princip-XML <sup> </sup> innehåller referenser till huvud-ID för användare som är associerade med principen. Princip-XML lagras som ett Blob-objekt.</td>
  </tr>
  <tr>
   <td><code>EdcPolicyArchiveEntity</code></td>
   <td>Lagrar information om arkiverade profiler. En arkiverad princip innehåller XML-principfilen som lagras som ett Blob-objekt.</td>
  </tr>
  <tr>
   <td><p><code>EdcPolicySetPrincipalEntity</code></p> <p><code>EdcPolicySetPrincipalEnt</code><br /> (Oracle- och MS® SQL-databaser)</p> </td>
   <td>Lagrar mappningen mellan principuppsättningar och användare.</td>
  </tr>
  <tr>
   <td><code>EdcInvitedUserEntity</code></td>
   <td>Lagrar information om inbjuden användare.</td>
  </tr>
 </tbody>
</table>

## Få åtkomst till och ta bort användardata {#access-and-delete-user-data}

Du kan komma åt och exportera dokumentsäkerhetsdata för användare i databaserna och vid behov ta bort dem permanent.

Om du vill exportera eller ta bort användardata från en databas måste du ansluta till databasen med en databasklient och ta reda på det huvud-ID som baseras på viss personligt identifierbar information om användaren. Om du till exempel vill hämta användarens huvud-ID med ett inloggnings-ID kör du följande `select`-kommando i databasen.

Ersätt `<user_login_id>` med inloggnings-ID:t för den användare vars huvud-ID du vill hämta från databastabellen `EdcPrincipalUserEntity` i kommandot `select`.

```sql
select refprincipalid from EdcPrincipalUserEntity where uidstring = <user_login_id>
```

När du känner till ditt huvuds-ID kan du exportera eller ta bort användardata.

### Exportera användardata {#export-user-data}

Kör följande databaskommandon så att du kan exportera användardata för ett huvuds-ID från databastabeller. Ersätt `<principal_id>` i kommandot `select` med det huvud-ID för användaren vars data du vill exportera.

>[!NOTE]
>
>Följande kommandon använder databastabellnamn i My SQL- och IBM® DB2®-databaser. När du kör dessa kommandon på Oracle- och MS® SQL-databaser ersätter du `EdcPolicySetPrincipalEntity` med `EdcPolicySetPrincipalEnt` i kommandona.

```sql
Select * from EdcPrincipalKeyEntity where principalid = '<principal_id>';

Select * from EdcLicenseEntity where publisherId = '<principal_id>';

Select * from EdcDocumentEntity where id in (Select documentid from EdcLicenseEntity where publisherId = '<principal_id>');

Select * from EdcRevokationEntity where licenseid in (Select id from EdcLicenseEntity where publisherId = '<principal_id>');

Select * from EdcMyPolicyListEntity where principalId = '<principal_id>';

Select * from edcpolicyentity where policyownerId = '<principal_id>';

Select * from edcpolicyxmlentity where policyidref in (Select id from edcpolicyentity where policyownerId = '<principal_id>');

Select * from edcpolicyarchiveentity where policyownerId = '<principal_id>';

Select * from edcpolicysetprincipalentity where principalId = '<principal_id>';

Select * from edcinviteduserentity where principalId = '<principal_id>';
```

>[!NOTE]
>
>Om du vill exportera data från tabellen `EdcAuditEntity` använder du API:t [ EventManager.exportEvents](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/programlc/javadoc/index.html?com/adobe/livecycle/rightsmanagement/client/EventManager.html) som använder [ EventSearchFilter](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/programlc/javadoc/com/adobe/livecycle/rightsmanagement/client/infomodel/EventSearchFilter.html) som parameter för att exportera granskningsdata baserat på `principalId`, `policyId` eller `licenseId`.

Om du vill få fram fullständiga data om en användare i systemet måste du få åtkomst till och exportera data från databasen för användarhantering. Mer information finns i [Forms användarhantering: Hantera användardata](/help/forms/using/user-management-handling-user-data.md).

### Ta bort användardata {#delete-user-data}

Gör följande för att ta bort dokumentsäkerhetsdata för ett säkerhetsobjekt-ID från databastabeller.

1. Stäng av AEM Forms Server.
1. Kör följande databaskommandon så att du kan ta bort data för det primära ID:t från databastabeller för dokumentsäkerhet. Ersätt `<principal_id>` i kommandot `Delete` med det huvud-ID för användaren vars data du vill ta bort.

   ```sql
   Delete from EdcPrincipalKeyEntity where principalid = '<principal_id>';
   
   Delete from EdcMyPolicyListEntity where principalId = '<principal_id>';
   
   Delete from edcpolicyarchiveentity where policyownerId = '<principal_id>';
   
   Delete from edcpolicysetprincipalentity where principalId = '<principal_id>';
   
   Delete from edcinviteduserentity where principalId = '<principal_id>';
   ```

   >[!NOTE]
   >
   >Om du vill ta bort data från tabellen `EdcAuditEntity` använder du API:t [ EventManager.deleteEvents](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/programlc/javadoc/index.html?com/adobe/livecycle/rightsmanagement/client/EventManager.html) som tar [ EventSearchFilter](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/programlc/javadoc/com/adobe/livecycle/rightsmanagement/client/infomodel/EventSearchFilter.html) som parameter för att ta bort granskningsdata som baseras på `principalId`, `policyId` eller `licenseId`.

1. Aktiva och arkiverade princip-XML-filer lagras i databastabellerna `EdcPolicyXmlEntity` respektive `EdcPolicyArchiveEntity`. Så här tar du bort data för en användare från de här tabellerna:

   1. Öppna XML-blobben för varje rad i tabellen `EdcPolicyXMLEntity` eller `EdcPolicyArchiveEntity` och extrahera XML-filen. XML-filen liknar den som visas nedan.
   1. Redigera XML-filen så att du kan ta bort blobben för huvuds-ID:t.
   1. Upprepa steg 1 och 2 för den andra filen.

   >[!NOTE]
   >
   >Ta bort hela blobben i taggen `Principal` för ett princip-ID eller så kan princip-XML vara skadat eller oanvändbart.

   ```xml
   <ns2:Principal PrincipalNameType="USER">
       <ns2:PrincipalDomain>OID</ns2:PrincipalDomain>
       <ns2:PrincipalName>56F33FEB-098A-1036-A651-00000A2A2656</ns2:PrincipalName>
   </ns2:Principal>
   </ns2:PolicyEntry>
       <ns2:Property PropertyName="isCertified">
           <ns2:PropertyValue xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:xs="https://www.w3.org/2001/XMLSchema" xsi:type="xs:string">false</ns2:PropertyValue>
       </ns2:Property>
       <ns2:Property PropertyName="encryptionAlgorithm">
           <ns2:PropertyValue xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:xs="https://www.w3.org/2001/XMLSchema" xsi:type="xs:string">AES128</ns2:PropertyValue>
       </ns2:Property>
       <ns2:Property PropertyName="AccessDeniedErrorMessage">
           <ns2:PropertyValue xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:xs="https://www.w3.org/2001/XMLSchema" xsi:type="xs:string"></ns2:PropertyValue>
       </ns2:Property>
   <ns2:PolicyEntry>
   <ns2:Permission PermissionName="ns3:com.adobe.aps.onlineOpen" Access="ALLOW"/>
   <ns2:Permission PermissionName="ns3:com.adobe.aps.pdf.copy" Access="ALLOW"/>
   <ns2:Permission PermissionName="ns3:com.adobe.aps.offlineOpen" Access="ALLOW"/>
   <ns2:Permission PermissionName="ns3:com.adobe.aps.pdf.accessible" Access="ALLOW"/>
   <ns2:Permission PermissionName="ns3:com.adobe.aps.pdf.editNotes" Access="ALLOW"/>
   <ns2:Permission PermissionName="ns3:com.adobe.aps.pdf.edit" Access="ALLOW"/>
   <ns2:Permission PermissionName="ns3:com.adobe.aps.pdf.fillAndSign" Access="ALLOW"/>
   <ns2:Permission PermissionName="ns3:com.adobe.aps.pdf.printHigh" Access="ALLOW"/>
   <ns2:Permission PermissionName="ns3:com.adobe.aps.pdf.printLow" Access="ALLOW"/>
   ```

   Förutom att ta bort data direkt från tabellen `EdcPolicyXmlEntity` finns det ytterligare två sätt att uppnå detta:

   **Använder administrationskonsol**

   1. Som administratör loggar du in på administrationskonsolen för Forms JEE på https://[*server*]:[*port*]/adminui.
   1. Navigera till **[!UICONTROL Services > Document Security > Policy Sets]**.
   1. Öppna en principuppsättning och ta bort användaren från profilen.

   **Använder webbsidan för dokumentsäkerhet**

   Dokumentsäkerhetsanvändare som har behörighet att skapa personliga profiler kan ta bort användardata från sina profiler. Så här gör du:

   1. Användare med egna profiler loggar in på sin dokumentsäkerhetswebbsida på https://[*server*]:[*port*]/edc.
   1. Navigera till **[!UICONTROL Services > Document Security > My Policies]**.
   1. Öppna en profil och ta bort användaren från profilen.

   >[!NOTE]
   >
   >Administratörer kan söka efter, komma åt och ta bort användardata från andra användares personliga profiler i **[!UICONTROL Services > Document Security > My Policies]** med hjälp av administrationskonsolen.

1. Ta bort data för huvuds-ID från användarhanteringsdatabasen. Mer information finns i [Forms användarhantering | Hantera användardata ](/help/forms/using/user-management-handling-user-data.md).
1. Starta AEM Forms Server.
