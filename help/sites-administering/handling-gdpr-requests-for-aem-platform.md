---
title: Hantera GDPR-begäranden för Adobe Experience Manager Foundation
description: Hantera GDPR-begäranden för Adobe Experience Manager Foundation
contentOwner: sarchiz
solution: Experience Manager, Experience Manager Sites
feature: Administering
role: Admin
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# Hantera GDPR-begäranden för Adobe Experience Manager (AEM Foundation){#handling-gdpr-requests-for-the-aem-foundation}

>[!IMPORTANT]
>
>GDPR används som exempel i avsnitten nedan, men de ingående detaljerna gäller alla dataskydds- och sekretessbestämmelser, som GDPR, CCPA och så vidare.

## Stöd för AEM Foundation GDPR {#aem-foundation-gdpr-support}

På AEM Foundation-nivå är de personuppgifter som lagras användarprofilen. Därför handlar informationen i den här artikeln främst om hur du får åtkomst till och tar bort användarprofiler, om GDPR Access och Delete-begäranden.

## Åtkomst till en användarprofil {#accessing-a-user-profile}

### Manuella steg {#manual-steps}

1. Öppna konsolen för användaradministration genom att bläddra till **[!UICONTROL Settings - Security - Users]** eller genom att bläddra direkt till `https://<serveraddress>:<serverport>/libs/granite/security/content/useradmin.html`

   ![useradmin2](assets/useradmin2.png)

1. Sök sedan efter användaren genom att skriva namnet i sökfältet högst upp på sidan:

   ![användarsökning](assets/usersearch.png)

1. Öppna sedan användarprofilen genom att klicka på den och kontrollera den på fliken **[!UICONTROL Details]**.

   ![userprofile_small](assets/userprofile_small.png)

### HTTP-API {#http-api}

Som vi nämnt tillhandahåller Adobe API:er för åtkomst av användardata, för att underlätta automatisering. Det finns flera typer av API:er som du kan använda:

**API för användaregenskaper**

```shell
curl -u user:password http://localhost:4502/libs/granite/security/search/profile.userproperties.json\?authId\=cavery
```

**Sling API**

*Identifierar användarens startsida:*

```xml
curl -g -u user:password 'http://localhost:4502/libs/granite/security/search/authorizables.json?query={"condition":[{"named":"cavery"}]}'
     {"authorizables":[{"type":"user","authorizableId_xss":"cavery","authorizableId":"cavery","name_xss":"Carlene Avery","name":"Carlene Avery","home":"/home/users/we-retail/DSCP-athB1NYLBXvdTuN"}],"total":1}
```

*Hämtar användardata*

Använda nodsökvägen från egenskapen home för JSON-nyttolasten som returneras från ovanstående kommando:

```shell
curl -u user:password  'http://localhost:4502/home/users/we-retail/DSCP-athB1NYLBXvdTuN/profile.-1.json'
```

```shell
curl -u user:password  'http://localhost:4502/home/users/we-retail/DSCP-athB1NYLBXvdTuN/profiles.-1.json'
```

## Inaktivera en användare och ta bort associerade profiler {#disabling-a-user-and-deleting-the-associated-profiles}

### Inaktivera användare {#disable-user}

1. Öppna konsolen för användaradministration och sök efter användaren i fråga enligt beskrivningen ovan.
1. Håll pekaren över användaren och klicka på markeringsikonen. Profilen blir grå vilket anger att den är markerad.

1. Tryck på knappen Inaktivera i den övre menyn för att inaktivera användaren:

   ![userdisable](assets/userdisable.png)

1. Bekräfta slutligen åtgärden:

   ![image2018-2-6_1-40-58](assets/image2018-2-6_1-40-58.png)

   Användargränssnittet indikerar att användaren inaktiveras genom att gråta ut och lägga till ett lås till profilkortet:

   ![inaktiverad användare](assets/disableduser.png)

### Ta bort användarprofilinformation {#delete-user-profile-information}

1. Logga in på CRXDE Lite och sök efter `[!UICONTROL userId]`:

   ![image2018-2-6_1-57-11](assets/image2018-2-6_1-57-11.png)

1. Öppna användarnoden som finns under `[!UICONTROL /home/users]` som standard:

   ![image2018-2-6_1-58-25](assets/image2018-2-6_1-58-25.png)

1. Ta bort profilnoder och alla underordnade noder. Profilnoderna har två format, beroende på vilken version av AEM det är:

   1. Den privata standardprofilen under `[!UICONTROL /profile]`
   1. `[!UICONTROL /profiles]`, för nya profiler som skapats med AEM 6.5.

   ![image2018-2-6_2-0-4](assets/image2018-2-6_2-0-4.png)

### HTTP-API {#http-api-1}

Följande procedurer använder kommandoradsverktyget `curl` för att illustrera hur du inaktiverar användaren med **[!UICONTROL cavery]** `userId` och tar bort profiler för `cavery` som är tillgängliga på standardplatsen.

* *Identifierar användarens startsida*

```shell
curl -g -u user:password 'http://localhost:4502/libs/granite/security/search/authorizables.json?query={"condition":[{"named":"cavery"}]}'
     {"authorizables":[{"type":"user","authorizableId_xss":"cavery","authorizableId":"cavery","name_xss":"Carlene Avery","name":"Carlene Avery","home":"/home/users/we-retail/DSCP-athB1NYLBXvdTuN"}],"total":1}
```

* *Inaktiverar användaren*

Använda nodsökvägen från egenskapen home för JSON-nyttolasten som returneras från ovanstående kommando:

```shell
curl -X POST -u user:password -FdisableUser="describe the reasons for disabling this user (GDPR in this case)" 'http://localhost:4502/home/users/we-retail/DSCP-athB1NYLBXvdTuN.rw.userprops.html'
```

* *Tar bort användarprofiler*

Använd nodsökvägen från egenskapen home för JSON-nyttolasten som returneras från kontoidentifieringskommandot och den kända out-of-box-profilnodplatsen:

```shell
curl -X POST -u user:password -H "Accept: application/json,**/**;q=0.9" -d ':operation=delete' 'http://localhost:4502/home/users/we-retail/DSCP-athB1NYLBXvdTuN/profile'
```

```shell
curl -X POST -u user:password -H "Accept: application/json,**/**;q=0.9" -d ':operation=delete' 'http://localhost:4502/home/users/we-retail/DSCP-athB1NYLBXvdTuN/profile'
```
