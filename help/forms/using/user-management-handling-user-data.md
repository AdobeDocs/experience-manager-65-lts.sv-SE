---
title: Forms användarhantering | Hantera användardata
description: Läs om hur du i AEM Forms JEE User Management-komponenten kan skapa, auktorisera och hantera användare som behöver åtkomst till AEM Forms.
topic-tags: grdp
products: SG_EXPERIENCEMANAGER/6.5/FORMS
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 0%

---

# Forms användarhantering | Hantera användardata {#forms-user-management-handling-user-data}

Användarhantering är en AEM Forms JEE-komponent som gör det möjligt att skapa, hantera och ge AEM Forms-användare åtkomst till AEM Forms. I användarhantering används domäner som kataloger för att hämta användarinformation. Följande domäntyper stöds:

**Lokala domäner**: Den här typen av domän är inte ansluten till ett tredjepartslagringssystem. I stället skapas användare och grupper lokalt och finns i databasen för användarhantering. Lösenord lagras lokalt och autentisering görs med en lokal databas.

**Hybrid-domäner**: Den här typen av domän är inte ansluten till ett tredjeparts lagringssystem. I stället skapas användare och grupper lokalt och finns i databasen för användarhantering. Till skillnad från lokala domäner använder hybriddomäner en extern autentiseringsprovider, som kan vara LDAP, Kerberos, SAML eller en anpassad autentiseringsprovider.

**Företagsdomäner**: Består av användare och grupper som finns i ett tredjepartslagringssystem, till exempel en LDAP-katalog. Användarhantering skriver inte till lagringssystemet från tredje part. I stället synkroniserar Hantering av användare användar- och gruppinformationen med databasen för användarhantering. Enterprise-domäner använder också en extern autentiseringsprovider, som kan vara LDAP, Kerberos, SAML eller en anpassad autentiseringsprovider.

<!-- Fix broken links For more information about how user management works and configured, see AEM Forms JEE administration help. -->

## Användardata och datalager {#user-data-and-data-stores}

Användarhantering lagrar användardata i en databas, t.ex. My SQL, Oracle, MS® SQL Server och IBM® DB2®. Dessutom skapas användare som har loggat in minst en gång i Forms-program på AEM-författaren `https://'[server]:[port]'lc` i AEM-databasen. Därför lagras användarhantering i följande datalager:

* Databas
* AEM-databas
* Tredjepartslagring som LDAP-katalog

>[!NOTE]
>
>Data som lagras i tredjepartslager ligger utanför dokumentets omfång. Kontakta tredjepartsleverantören direkt för att hantera användardata i sådana lager.

### Databas {#database}

Användarhantering lagrar användardata i följande databastabeller:

<table>
 <tbody>
  <tr>
   <td>Databas</td>
   <td>Beskrivning</td>
  </tr>
  <tr>
   <td><code>EdcPrincipalEntity</code></td>
   <td><p>Lagrar information om huvudenheter. Ett huvudkonto kan vara en användare, en grupp eller en roll.</p> <p> </p> </td>
  </tr>
  <tr>
   <td><code>EdcPrincipalUserEntity</code></td>
   <td>Lagrar användares personligt identifierbara information. Den innehåller en post för varje användare från lokala domäner, företagsdomäner och hybriddomäner.</td>
  </tr>
  <tr>
   <td><p><code>EdcPrincipalLocalAccountEntity</code></p> <p><code class="code">EdcPrincipalLocalAccount
       </code>(Oracle- och MS® SQL-databaser)</p> </td>
   <td>Lagrar data endast för lokala användare.</td>
  </tr>
  <tr>
   <td><p><code>EdcPrincipalEmailAliasEntity</code></p> <p><code class="code">EdcPrincipalEmailAliasEn
       </code>(Oracle- och MS® SQL-databaser)</p> </td>
   <td>Innehåller poster för alla användare från lokala domäner, företagsdomäner och hybriddomäner. Den innehåller användarens e-post-ID.</td>
  </tr>
  <tr>
   <td><p><code>EdcPrincipalGrpCtmntEntity</code></p> <p><code>EdcPrincipalGrpCtmntEnti</code><br /> (Oracle- och MS® SQL-databaser)</p> </td>
   <td>Lagrar mappningen mellan användare och grupper.</td>
  </tr>
  <tr>
   <td><code>EdcPrincipalRoleEntity</code></td>
   <td>Lagrar mappningen mellan roller och principer för både användare och grupper.</td>
  </tr>
  <tr>
   <td><code>EdcPriResPrmEntity</code></td>
   <td>Lagrar mappningen mellan huvudnamn och behörigheter för både användare och grupper.</td>
  </tr>
  <tr>
   <td><p><code>EdcPrincipalMappingEntity</code></p> <p><code>EdcPrincipalMappingEntit</code><br /> (Oracle- och MS® SQL-databaser)</p> </td>
   <td>Lagrar gamla och nya attributvärden som motsvarar ett huvudkonto.<br /> </td>
  </tr>
 </tbody>
</table>

### AEM-databas {#aem-repository}

Användarhanteringsdata för användare som minst en gång har använt Forms-programmen under `https://'[server]:[port]'lc` lagras också i AEM-databasen.

## Få åtkomst till och ta bort användardata {#access-and-delete-user-data}

Du kan komma åt och exportera användarhanteringsdata för användare i användarhanteringsdatabaserna och AEM-databasen, och vid behov ta bort dem permanent.

### Databas {#database-1}

Om du vill exportera eller ta bort användardata från användarhanteringsdatabasen måste du ansluta till databasen med en databasklient och ta reda på det huvud-ID som baseras på användarens PII-kod. Om du till exempel vill hämta användarens huvud-ID med ett inloggnings-ID kör du följande `select`-kommando i databasen.

Ersätt `<user_login_id>` med inloggnings-ID:t för den användare vars huvud-ID du vill hämta i kommandot `select`.

```sql
select refprincipalid from EdcPrincipalUserEntity where uidstring = <user_login_id>
```

När du känner till ditt huvuds-ID kan du exportera eller ta bort användardata.

#### Exportera användardata {#export-user-data}

Kör följande databaskommandon så att du kan exportera användarhanteringsdata för ett huvuds-ID från databastabeller. Ersätt `<principal_id>` i kommandot `select` med det huvud-ID för användaren vars data du vill exportera.

>[!NOTE]
>
>Följande kommandon använder databastabellnamn i My SQL- och IBM® DB2®-databaser. När du kör dessa kommandon i Oracle- och MS® SQL-databaser ersätter du följande tabellnamn i kommandona:
>
>* Ersätt `EdcPrincipalLocalAccountEntity` med `EdcPrincipalLocalAccount`
>
>* Ersätt `EdcPrincipalEmailAliasEntity` med `EdcPrincipalEmailAliasEn`
>
>* Ersätt `EdcPrincipalMappingEntity` med `EdcPrincipalMappingEntit`
>
>* Ersätt `EdcPrincipalGrpCtmntEntity` med `EdcPrincipalGrpCtmntEnti`
>

```sql
Select * from EdcPrincipalLocalAccountEntity where refuserprincipalid in (Select id from EdcPrincipalUserEntity where refprincipalid in (Select id from EDCPRINCIPALENTITY where id='<principal_id>'));

Select * from EdcPrincipalEmailAliasEntity where refprincipalid in (Select id from EdcPrincipalEntity where id='<principal_id>');

Select * from EdcPrincipalRoleEntity where refprincipalid in (Select id from EdcPrincipalEntity where id='<principal_id>');

Select * from EdcPriResPrmEntity where refprinid in (Select id from EdcPrincipalEntity where id='<principal_id>');

Select * from EdcPrincipalUserEntity where refprincipalid in (Select id from EdcPrincipalEntity where id='<principal_id>');

Select * from EdcPrincipalMappingEntity where refprincipalid in (Select id from EdcPrincipalEntity where id='<principal_id>');

Select * from EdcPrincipalGrpCtmntEntity where refchildprincipalid in (Select id from EdcPrincipalEntity where id='<principal_id>');

Select * from EdcPrincipalEntity where id='<principal_id>';
```

#### Ta bort användardata {#delete-user-data}

Så här tar du bort användarhanteringsdata för ett huvuds-ID från databastabeller.

1. Ta bort användardata från AEM-databasen, om tillämpligt, enligt beskrivningen i [Ta bort användardata](/help/forms/using/user-management-handling-user-data.md#delete-aem).
1. Stäng av AEM Forms Server.
1. Kör följande databaskommandon så att du kan ta bort användarhanteringsdata för ett huvuds-ID från databastabeller. Ersätt `<principal_id>` i kommandot `Delete` med det huvud-ID för användaren vars data du vill ta bort.

   ```sql
   Delete from EdcPrincipalLocalAccountEntity where refuserprincipalid in (Select id from EdcPrincipalUserEntity where refprincipalid in (select id from EdcPrincipalEntity where id='<principal_id>'));
   
   Delete from EdcPrincipalEmailAliasEntity where refprincipalid in (Select id from EdcPrincipalEntity where id='<principal_id>');
   
   Delete from EdcPrincipalRoleEntity where refprincipalid in (Select id from EdcPrincipalEntity where id='<principal_id>');
   
   Delete from EdcPriResPrmEntity where refprinid in (Select id from EdcPrincipalEntity where id='<principal_id>');
   
   Delete from EdcPrincipalUserEntity where refprincipalid in (Select id from EdcPrincipalEntity where id='<principal_id>');
   
   Delete from EdcPrincipalMappingEntity where refprincipalid in (Select id from EdcPrincipalEntity where id='<principal_id>');
   
   Delete from EdcPrincipalGrpCtmntEntity where refchildprincipalid in (Select id from EdcPrincipalEntity where id='<principal_id>');
   
   Delete from EdcPrincipalEntity where id='<principal_id>';
   ```

1. Starta AEM Forms Server.

### AEM-databas {#aem-repository-1}

Forms JEE-användare har sina data i AEM-databasen om de har öppnat minst en av AEM Forms författarinstanser. Du kan komma åt och ta bort användardata från AEM-databasen.

#### Åtkomst till användardata {#access-user-data}

Om du vill visa användare som har skapats i AEM-databasen loggar du in på `https://'[server]:[port]'/lc/useradmin` med AEM administratörsuppgifter. Observera att `server` och `port` i URL:en är samma som i AEM-författarinstansen. Här kan du söka efter användare med deras användarnamn. Dubbelklicka på en användare så att du kan visa information om användarens egenskaper, behörigheter och grupper. Egenskapen `Path` för en användare anger sökvägen till användarnoden som har skapats i AEM-databasen.

#### Ta bort användardata {#delete-aem}

Ta bort en användare:

1. Gå till `https://'[server]:[port]'/lc/useradmin` med AEM administratörsautentiseringsuppgifter.
1. Sök efter en användare och dubbelklicka på användarnamnet för att öppna användaregenskaperna. Kopiera egenskapen `Path`.
1. Gå till AEM CRXDE Lite på `https://'[server]:[port]'/lc/crx/de/index.jsp` och navigera eller sök i användarsökvägen.
1. Ta bort sökvägen och klicka på **[!UICONTROL Save All]** för att permanent ta bort användaren från AEM-databasen.
