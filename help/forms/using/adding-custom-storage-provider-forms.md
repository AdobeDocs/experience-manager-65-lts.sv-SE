---
title: Anpassad lagring för utkast och inskickningskomponenter
description: Se hur du kan anpassa lagring av användardata för utkast och inskickade data.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: Configuration
feature: Forms Portal
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 2f7caa43-213e-4cd2-bb02-6b18c3efb81c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 0%

---

# Anpassad lagring för utkast och inskickningskomponenter {#custom-storage-for-drafts-and-submissions-component}

## Ökning {#overview}

Med AEM Forms kan du spara ett formulär som ett utkast. Med utkastsfunktionen kan du underhålla ett pågående formulär som du kan fylla i och skicka senare från vilken enhet som helst.

Som standard lagrar AEM Forms användardata som är kopplade till utkastet och överföringen av ett formulär i noden `/content/forms/fp` på publiceringsinstansen. Dessutom innehåller komponenterna i AEM Forms Portal datatjänster som du kan använda för att anpassa implementeringen av lagring av användardata för utkast och inskickade data. Du kan till exempel lagra användardata i ett datalager.

## Förutsättningar  {#prerequisites}

* Aktivera [Forms Portal-komponenter](/help/forms/using/enabling-forms-portal-components.md)
* Skapa en [Forms Portal-sida](/help/forms/using/creating-form-portal-page.md)
* Aktivera [adaptiva formulär för Forms Portal](/help/forms/using/draft-submission-component.md)
* Lär dig [implementeringsinformation för anpassad lagring](/help/forms/using/draft-submission-component.md#customizing-the-storage)

## Utkastdatatjänst {#draft-data-service}

Om du vill anpassa lagring av användardata för utkast måste du implementera alla metoder i gränssnittet `DraftDataService`. I följande exempelkod beskrivs metoderna och argumenten.

```java
/**
 * DraftDataService service will get/delete/save user data (attachments and form data) filled with a draft instance of Form
 */

public interface DraftDataService {

    /**
     * To save/modify user data for this userDataID, it will be null if there is creation
     * @param draftDataID: unique identifier associated with the form data
     * @param formName: name of the form whose draft is being saved
     * @param formData: user data associated with this draft
     * @return userdataID corresponding to which user data has been stored and which can be used later to retrieve this user data
     * @throws FormsPortalException
     */
    public String saveData (String draftDataID, String formName, String formData) throws FormsPortalException;

    /**
     * Returns the user data stored against the ID passed as the argument
     * @param userDataID: unique data id for user data associated with a draft
     * @return user data associated with this data ID
     * @throws FormsPortalException
     */

    public byte[] getData (String userDataID) throws FormsPortalException;

    /**
     * To delete data associated with this draft
     * @param userDataID: unique data id for data associated with a draft
     * @return status of delete operation on data associated with this draft
     * @throws FormsPortalException
     */

    public boolean deleteData (String userDataID) throws FormsPortalException;

    /**
     * Saves the attachment for current form instance
     * @param attachmentsBytes: byte array of the attachment to be saved
     * @return unique id (attachmentID) for the attachment just saved (so that it could be retrieved later)
     * @throws FormsPortalException
     */
    public String saveAttachment (byte[] attachmentBytes) throws FormsPortalException;

    /**
     * To delete an attachment
     * @param attachmentID: unique id for this attachment
     * @return status of delete operation performed on attachment corresponding to this attachment ID
     * @throws FormsPortalException
     */
    public boolean deleteAttachment (String attachmentID) throws FormsPortalException;

    /**
     * To get attachment bytes
     * @param attachmentID: unique id for this attachment
     * @return data corresponding to this attachmentID
     * @throws FormsPortalException
     */
    public byte[] getAttachment (String attachmentID) throws FormsPortalException;
}
```

>[!NOTE]
>
>Minsta tillåtna värde för längden på utkastets ID-fält är 26 tecken. Adobe rekommenderar att du anger ett utkast-ID med 26 eller fler tecken.

## Datatjänst för överföring {#submission-data-service}

Om du vill anpassa lagring av användardata för överföringar måste du implementera alla metoder i gränssnittet `SubmitDataService`. I följande exempelkod beskrivs metoderna och argumenten.

```java
/**
 * SubmitDataService service will get/delete/submit user data (attachments and form data) filled with a submission of Form
 */
public interface SubmitDataService {

    /**
     * Submits the user data passed in argument map
     * @param userDataID, unique identifier associated with this user data
     * @param formName, name of the form whose draft is being submitted
     * @param formData, user data associated with this submission
     * @return userdataID, corresponding to which the user data has been stored and which can be used later to retrieve this data
     * @throws FormsPortalException
     */
    public String saveData (String userDataID, String formName, String formData) throws FormsPortalException;

    /**
     * Submits the user data provided as byte array
     * @param id
     * @param data
     * @return id corresponding to saved data
     * @throws FormsPortalException
     */
    public String saveData (String id, byte[] data) throws FormsPortalException;

    /** Submits the user data provided as byte array asynchronously for the user name provided in the options map
     * @param data data to be saved in bytes
     * @param options map containing options that affect this save
     * @return id of the saved data instance
     */
    public String saveDataAsynchronusly(byte[] data, Map<String, Object> options) throws FormsPortalException;

    /**
     * Gets the user data stored against the ID passed as argument
     * @param userDataID: unique id associated with this user data for this submission
     * @return user data associated with this submission
     * @throws FormsPortalException
     */
    public byte[] getData(String userDataID) throws FormsPortalException;

    /**
     * Deletes user data stored against the userDataID
     * @param userDataID: unique id associated with this user data for this submission
     * @return status of the delete operation on this submission
     * @throws FormsPortalException
     */

    public boolean deleteData(String userDataID) throws FormsPortalException;

    /**
     * Submits the attachment bytes passed as argument
     * @param attachmentsBytes: would expect byte array of the attachment for this submission
     * @return attachmentID for the attachment just saved (so that it could be retrieved later)
     * @throws FormsPortalException
     */
    public String saveAttachment(byte[] attachmentBytes) throws FormsPortalException;

    /** Submits the attachment bytes passed as argument asynchronously for the user id provided in options map.
     * @param attachmentBytes would expect byte array of the attachment for this submission
     * @param options map containing options that affect this save
     * @return attachmentID for the attachment just saved (so that it could be retrieved later)
     * @throws FormsPortalException
     */
    public String saveAttachmentAsynchronously(byte[] attachmentBytes, Map<String, Object> options) throws FormsPortalException;

    /**
     * To delete an attachment
     * @param attachmentID: Unique id for this attachment
     * @return status of delete operation performed on attachment corresponding to this attachment ID
     * @throws FormsPortalException
     */
    public boolean deleteAttachment (String attachmentID) throws FormsPortalException;

    /**
     * To get attachment bytes
     * @param attachmentID: unique id for this attachment
     * @return data corresponding to this attachmentID
     * @throws FormsPortalException
     */
    public byte[] getAttachment (String attachmentID) throws FormsPortalException;
}
```

Forms Portal använder UUID (Universally Unique IDentifier) för att generera ett unikt ID för varje utkast och skickat formulär. Du kan också generera ett unikt ID. Du kan implementera gränssnittet FPKeyGeneratorService, åsidosätta dess metoder och utveckla en anpassad logik för att generera ett anpassat unikt ID för varje utkast och skickat formulär. Ange även en tjänstrankning för implementering av anpassad ID-generering som är högre än 0. Det ser till att den anpassade implementeringen används i stället för standardimplementeringen.

```java
public interface FPKeyGeneratorService {
    /**
     * returns a unique id for draft and submission
     *
     * @param none
     * @return unique id in string format as per the implementation
     * @throws FormsPortalException
     */
    public String getUniqueId() throws FormsPortalException;
}
```

Du kan använda anteckningen nedan för att öka rankningen för anpassade ID:n som genereras med ovanstående kod:

`@Properties(value = { @Property(name = "service.ranking", intValue = 15) } )`

Om du vill använda ovanstående anteckning importerar du följande till ditt projekt:

```java
import org.apache.felix.scr.annotations.Properties;
import org.apache.felix.scr.annotations.Property;
```
