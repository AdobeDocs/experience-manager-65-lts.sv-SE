---
title: Alltid användaretablering
description: Använd tidsbestämd etablering för att lägga till användare i användarhantering efter lyckad autentisering och tilldela dynamiskt relevanta roller och grupper till den nya användaren.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/setting_up_and_organizing_users
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 0%

---

# Alltid användaretablering {#just-in-time-user-provisioning}

AEM-formulär har stöd för etablering i realtid av användare som ännu inte finns i användarhantering. Med just-in-time-etablering läggs användare automatiskt till i användarhanteringen efter att inloggningsuppgifterna har autentiserats. Dessutom tilldelas relevanta roller och grupper dynamiskt till den nya användaren.

## Behovet av användarprovisionering i precis tid {#need-for-just-in-time-user-provisioning}

Så här fungerar traditionell autentisering:

1. När en användare försöker logga in på AEM-formulär skickar användarhanteringen användarens inloggningsuppgifter sekventiellt till alla tillgängliga autentiseringsleverantörer. (Inloggningsuppgifterna innehåller en kombination av användarnamn/lösenord, Kerberos-biljett, PKCS7-signatur och så vidare.)
1. Autentiseringsprovidern validerar inloggningsuppgifterna.
1. Autentiseringsprovidern kontrollerar sedan om användaren finns i databasen för användarhantering. Följande resultat är möjliga:

   **Finns:** Om användaren är aktuell och olåst returnerar Hantering av användare autentiseringen. Om användaren inte är aktuell eller låst returneras ett autentiseringsfel.

   **Finns inte:** Användarhantering returnerar ett autentiseringsfel.

   **Ogiltig:** Användarhantering returnerar ett autentiseringsfel.

1. Resultatet som returneras av autentiseringsprovidern utvärderas. Om autentiseringsprovidern returnerade autentiseringen kan användaren logga in. Annars kontrolleras användarhanteringen med nästa autentiseringsprovider (steg 2-3).
1. Autentiseringsfel returneras om ingen tillgänglig autentiseringsprovider validerar inloggningsuppgifterna.

När etablering bara är i tid implementeras skapas en ny användare dynamiskt i användarhantering om en av autentiseringsleverantörerna validerar användarens inloggningsuppgifter. (Efter steg 3 i den traditionella autentiseringsproceduren ovan.)

## Implementera etablering av användare som bara är i tid {#implement-just-in-time-user-provisioning}

### API:er för etablering i precis tid {#apis-for-just-in-time-provisioning}

AEM-formulär innehåller följande API:er för etablering i precis tid:

```java
package com.adobe.idp.um.spi.authentication  ;
publ ic interface IdentityCreator {
/**
* Tries  to create a user with the  in formation  provided in the <code>UserProvisioningBO</code> object.
* If the user is successfully created, a valid AuthResponse is returned along with the information using which the user was created.
* It is the responsibility of the IdentityCreator to set the User obje ct  in the cre dential map with th e  ke y  <code>UMA u thenticationUtil.authenticatedUserKey</code>
* The credentials are available in the <code>UserProvisioningBO</code> object in the 'credentials' property.
* If the IdentityCreator is unable to create a user due to any reason, it returns <code>null</code>
* @param userBO An object of <code>com.adobe. i dp.um . spi.authenti c ationUserProvisioningBO</code>
* @return */public AuthResponse create(UserProvisioningBO userBO);
/**
* Returns the name of the IdentityCreator which will be registered in preferences.
* This name is used to associate the IdentityProvider with the Auth Provider Configuration in the domain.
* @return The name of the Identity Creator which is recognized in Configuration.
*/
public String getName();
}
package com.adobe.idp.um.spi.authentication;
import com.adobe.idp.um.api.infomodel.User;
public interface AssignmentProvider {
/**
* Tries to assign roles or permissions or group memberships to users created via Just-in-time provisioning.
* @param user The User created via the Just-in-time provisioning process.
* @return a Boolean flag indicating whether the assignment was successful or not.
*/
public Boolean assign(User user);
/**
* Returns the name of the AssignmentProvider through which it is registered under preferences.
* This name is used to associate the AssignmentProvider with the Auth Provider Configuration in the domain.
* @return The name of the AssignmentProvider which is recognized in Configuration.
*/public String getName();
}
```

### Att tänka på när du skapar en domän som bara är aktiverad vid en viss tidpunkt {#considerations-while-creating-a-just-in-time-enabled-domain}

* När du skapar en anpassad `IdentityCreator` för en hybriddomän måste du se till att ett dummy-lösenord anges för den lokala användaren. Lämna inte det här lösenordsfältet tomt.
* Rekommendation: Använd `DomainSpecificAuthentication` för att validera användarautentiseringsuppgifter mot en specifik domän.

### Skapa en domän som är aktiverad just-in-time {#create-a-just-in-time-enabled-domain}

1. Skriv en DSC som implementerar API:erna i avsnittet&quot;API:er för etablering i precis tid&quot;.
1. Distribuera DSC till Forms Server.
1. Skapa en domän som bara är aktiverad vid en viss tidpunkt:

   * I administrationskonsolen klickar du på Inställningar > Användarhantering > Domänhantering > Ny företagsdomän.
   * Konfigurera domänen och välj Aktivera etablering i realtid. <!--Fix broken link (See Setting up and managing domains).-->
   * Lägg till autentiseringsproviders. När du lägger till autentiseringsproviders väljer du en registrerad identitetsskapare och tilldelningsprovider på skärmen Ny autentisering.

1. Spara den nya domänen.

## Bakom scenen {#behind-the-scenes}

Anta att en användare försöker logga in på AEM-formulär och att en autentiseringsleverantör accepterar sina användaruppgifter. Om användaren inte finns i databasen för användarhantering än misslyckas identitetskontrollen för användaren. AEM-formulär utför nu följande åtgärder:

1. Skapa ett `UserProvisioningBO`-objekt med autentiseringsdata och placera det i en autentiseringsuppgiftskarta.
1. Baserat på domäninformation som returnerats av `UserProvisioningBO` hämtar och anropar du den registrerade `IdentityCreator` och `AssignmentProvider` för domänen.
1. Anropa `IdentityCreator`. Extrahera `UserInfo` från autentiseringsuppgiftskartan om det returnerar en `AuthResponse` som lyckades. Skicka det till `AssignmentProvider` för grupp-/rolltilldelning och annan efterbearbetning när användaren har skapats.
1. Om användaren har skapats utan fel returnerar du användarens inloggningsförsök.
1. För hybriddomäner hämtar du användarinformation från autentiseringsdata som tillhandahålls till autentiseringsprovidern. Om den här informationen har hämtats kan du skapa användaren direkt.

>[!NOTE]
>
>Etableringsfunktionen för just-in-time levereras med en standardimplementering av `IdentityCreator` som du kan använda för att dynamiskt skapa användare. Användare skapas med den information som är associerad med katalogerna i domänen.
