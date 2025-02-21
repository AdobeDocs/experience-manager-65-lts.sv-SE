---
title: Anpassa datatjänster för utkast och överföring
description: AEM Forms lagrar som standard utkast och skickade adaptiva formulär i en standardnod på Publish-instansen. Du kan dock konfigurera AEM Forms tjänster för utkast och inskickning av data för att anpassa lagringen av utkast och inskickade adaptiva formulär.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: customization
solution: Experience Manager, Experience Manager Forms
role: User, Developer
feature: Adaptive Forms,Foundation Components
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Anpassa datatjänster för utkast och överföring {#customizing-draft-and-submission-data-services}

## Ökning {#overview}

Med AEM Forms kan användare spara ett anpassat formulär som ett utkast. Utkastfunktionen ger användarna möjlighet att behålla ett pågående formulär. Användaren kan sedan fylla i och skicka formuläret när som helst från vilken enhet som helst.

Som standard lagrar AEM Forms användardata som är kopplade till utkastet och överföringen på Publish-instansen i noden `/content/forms/fp`.

AEM Forms Portal-komponenterna tillhandahåller dock datatjänster som gör att du kan anpassa implementeringen av lagring av användardata för utkast och inskickningar. Du kan till exempel lagra data i ett datalager som är implementerat i din organisation.

Om du vill anpassa lagringen av användardata måste du implementera tjänsterna [Utkastdata](/help/forms/using/custom-draft-submission-data-services.md#p-draft-data-service-p) och [Skicka data](/help/forms/using/custom-draft-submission-data-services.md#p-submission-data-service-p).

## Förutsättningar {#prerequisites}

* Aktivera [Forms Portal-komponenter](/help/forms/using/enabling-forms-portal-components.md)
* Skapa en [Forms Portal-sida](/help/forms/using/creating-form-portal-page.md)
* Aktivera [adaptiva formulär för Forms Portal](/help/forms/using/draft-submission-component.md)
* Lär dig [implementeringsinformation för anpassad lagring](/help/forms/using/draft-submission-component.md#customizing-the-storage)

## Utkastdatatjänst {#draft-data-service}

Om du vill anpassa lagringen av användarutkastdata måste du tillhandahålla implementering för alla metoder i gränssnittet `DraftAFDataService`.

En beskrivning av metoderna och deras argument finns i följande kodexempel i gränssnittet:

```java
public interface DraftAFDataService {

 /**
  * Deletes the user data stored against the ID passed as the argument
  *
  * @param draftDataID
  * @return status for the just occurred delete draft UserData operation
  * @throws FormsPortalException
  */
 public Boolean deleteAFDraftUserData (String draftDataID) throws FormsPortalException;

 /**
  * Saves user data provided in the argument map
  *
  * @param draftUserDataMap contains Form Data (key - "guideState"), Adaptive Form Name (Key - "guideName"), and Draft DataID (Key - "userDataID") if there is update
  * @return userData ID would be returned which needs to be saved in metadata node
  * @throws FormsPortalException
  */
 public String saveAFUserData (Map<String, Object> draftUserDataMap) throws FormsPortalException;

 /**
  * Gets the user data stored against the ID passed as the argument
  *
  * @param Draft DataID
  * @return guideState (which would then be populated in adaptive form to reload the draft) which is stored against draftDataID
  * @throws FormsPortalException
  */
 public byte[] getAFDraftUserData(String draftDataID) throws FormsPortalException;

 /**
  * Saves the attachments for current adaptive form instance
  *
  * @param attachmentsBytes would expect byte array of the attachment to be saved
  * @return id for the attachment just saved (so that it could be retrieved later)
  * @throws FormsPortalException
  */
 public String saveAttachments(byte[] attachmentBytes) throws FormsPortalException;
}
```

## Datatjänst för överföring {#submission-data-service}

Om du vill anpassa lagringen av användarinskickade data måste du tillhandahålla implementering för alla metoder i gränssnittet `SubmittedAFDataService`.

En beskrivning av metoderna och deras argument finns i följande kodexempel i gränssnittet:

```java
public interface SubmittedAFDataService {

 /**
  * Submits the user data passed in argument map
  *
  * @param submittedAFUserdataMap contains Form Data (key - "guideState"), Adaptive Form Name (Key - "guideName"), and Draft DataID (Key - "userDataID")
  * @return userData ID is returned that needs to be saved in the metadata node
  * @throws FormsPortalException
  */
 public String submitAFUserData (Map<String, Object> submittedAFUserdataMap) throws FormsPortalException;

 /**
  * Gets the user data stored against the ID passed as argument
  *
  * @param submitDataID
  * @return guideState which would be used to open DOR
  * @throws FormsPortalException
  */
 public byte[] getSubmittedAFUSerData(String submitDataID) throws FormsPortalException;

 /**
  * Deletes user data stored against the ID passed as argument
  *
  * @param Submit DataID
  * @return status of the delete operation on Submitted User data
  * @throws FormsPortalException
  */

 public Boolean deleteSubmittedAFUserData(String submitDataID) throws FormsPortalException;

 /**
  * Submits the attachment bytes passed as argument
  *
  * @param attachmentsBytes would expect byte array of the attachment to be saved
  * @return id for the attachment just saved (so that it could be retrieved later)
  * @throws FormsPortalException
  */
 public String submitAttachments(Object attachmentBytes) throws FormsPortalException;

}
```
