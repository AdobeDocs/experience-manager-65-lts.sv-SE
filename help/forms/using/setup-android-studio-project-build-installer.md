---
title: Konfigurera Android&trade; studio project och bygg Android&trade; app
description: Steg för att konfigurera Android&trade; Studio-projekt och bygga installationsprogrammet för Adobe Experience Manager (AEM) Forms-appen
content-type: reference
products: SG_EXPERIENCEMANAGER/6.3/FORMS
topic-tags: forms-app
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 887dc1d6d7e11672b62ef5ca5463ea6181ff0320
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 0%

---

# Konfigurera Android™ studio-projektet och bygg appen Android™ {#set-up-the-android-studio-project-and-build-the-android-app}

Den här artikeln används för att skapa AEM Forms App 6.3.1.1 och senare versioner. Information om hur du skapar en app från källkod i AEM Forms App 6.3 finns i [Konfigurera Eclipse-projektet och skapa Android™-appen](/help/forms/using/setup-eclipse-project-build-installer.md).

AEM Forms tillhandahåller den fullständiga källkoden för AEM Forms-appen. Källan innehåller alla komponenter som behövs för att skapa en anpassad AEM Forms-app. Källkodsarkivet, `adobe-lc-mobileworkspace-src-<version>.zip`, är en del av paketet `adobe-aemfd-forms-app-src-pkg-<version>.zip` för programvarudistribution.

Så här hämtar du programkällan för AEM Forms:

1. Öppna [Programvarudistribution](https://experience.adobe.com/downloads). Du behöver en Adobe ID för att logga in på Software Distribution.
1. Välj **[!UICONTROL Adobe Experience Manager]** som finns på rubrikmenyn.
1. I avsnittet **[!UICONTROL Filters]**:
   1. Välj **[!UICONTROL Forms]** i listrutan **[!UICONTROL Solution]**.
   2. Välj version och typ för paketet. Du kan också använda alternativet **[!UICONTROL Search Downloads]** för att filtrera resultaten.
1. Välj det paketnamn som gäller för ditt operativsystem, välj **[!UICONTROL Accept EULA Terms]** och välj **[!UICONTROL Download]**.
1. Öppna [Pakethanteraren](https://experienceleague.adobe.com/docs/experience-manager-65-2025/administering/contentmanagement/package-manager.html) och klicka på **[!UICONTROL Upload Package]** för att överföra paketet.
1. Markera paketet och klicka på **[!UICONTROL Install]**.

Följande bild visar det extraherade innehållet i `adobe-lc-mobileworkspace-src-<version>.zip`.

![Extraherat innehåll i den zippade Android™-källan](assets/mws-content-1.png)

Följande bild visar katalogstrukturen för mappen `android` i mappen `src`.

![Katalogstrukturen för android-mappen i src](assets/android-folder.png)

## Bygg AEM Forms standardapp {#set-up-the-xcode-project}

1. Utför följande steg för att konfigurera ett projekt i Android™ Studio och ange en signeringsidentitet:

   Logga in på en dator där Android™ Studio är installerat och konfigurerat.

1. Kopiera det hämtade `adobe-lc-mobileworkspace-src-<version>.zip`-arkivet till:

   **För Mac-användare**: `[User_Home]/Projects`

   **För Windows®-användare**: `%HOMEPATH%\Projects`

   >[!NOTE]
   >
   >För Windows® rekommenderar vi att du behåller Android™-projektet i systemenheten.

1. Extrahera arkivet i följande katalog:

   **För Mac-användare**: `[User_Home]/Projects/[your-project]`

   **För Windows®-användare**: `%HOMEPATH%\Projects\[your-project]`

   >[!NOTE]
   >
   >Vi rekommenderar att du behåller det extraherade Android-projektet i systemenheten innan du importerar projektet till Android™ Studio.

1. Starta Android™ Studio.

   **För Mac-användare**: Uppdatera filen `local.properties` som finns i mappen `[User_Home]/Projects/[your-project]/android` och peka variabeln `sdk.dir` till `SDK` på skrivbordet.

   **För Windows®-användare**: Uppdatera filen `local.properties` som finns i mappen `%HOMEPATH%\Projects\[your-project]\android` och peka variabeln `sdk.dir` till `SDK` på skrivbordet.

1. Klicka på **[!UICONTROL Finish]** för att skapa projektet.

   Projektet är tillgängligt i ADT Project Explorer.

   ![Förmörka projekt efter att appen har skapats](assets/eclipsebuildmws.png)

1. I Android™ Studio väljer du **[!UICONTROL Import Project (Eclipse ADT, Gradle, Etc.)]**.
1. I projektutforskaren väljer du rotkatalogen för projektet som du vill skapa i textrutan **Rotkatalog**:

   **För Mac-användare:** [User_Home]/Projects/MobileWorkspace/src/android

   **För Windows®-användare:** %HOMEPATH%\Projects\MobileWorkspace\src\android

1. När projektet har importerats visas ett popup-fönster med alternativet att uppdatera plugin-programmet för Android™. Klicka på lämplig knapp beroende på dina behov.

   ![dontremindmeagainforisproject](assets/dontremindmeagainforthisproject.png)

1. När du har skapat en övertoning visas följande skärm. Anslut lämplig enhet eller emulator till systemet och klicka på **[!UICONTROL Run Android™]**.

   ![graderskonsol](assets/gradleconsole.png)

1. Android™ Studio visar anslutna enheter och tillgängliga emulatorer. Välj den enhet som du vill köra programmet på och klicka sedan på **OK**.

   ![ansluten enhet](assets/connecteddevice.png)

När du har skapat projektet kan du välja att installera programmet med Android™ Debug Bridge eller Android™ Studio.

### Använda Android™ Debug Bridge {#andriod-debug-bridge}

Du kan installera programmet på en Android™-enhet med hjälp av [Android™ Debug Bridge](https://developer.android.com/tools/adb) med följande kommando:

**För Mac-användare**: `adb install [User_Home]/Projects/[your-project]/adobe-lc-mobileworkspace-src-[version]/android/build/outputs/apk/android-debug.apk`

**För Windows®-användare**: `adb install %HOMEPATH%\Projects\[your-project]\adobe-lc-mobileworkspace-src-[version]\android\build\outputs\apk\android-debug.apk`
