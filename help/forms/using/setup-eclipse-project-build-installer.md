---
title: Bygg appen AEM Forms Android
description: Steg för att konfigurera Android Studio-projektet och skapa APK-filen för AEM Forms-appen för Android
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-app
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 168cb023768ff3139937ab7f437ab7d00185bca0
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 0%

---

# Bygg appen AEM Forms Android {#build-the-aem-forms-android-app}

Utför följande steg i den rekommenderade sekvensen för att skapa Android-appen för AEM Forms.

1. [Ladda ned kodpaketet för AEM Forms App Source](#download-android-zip)
1. [Ange miljövariabler](#set-environment-variable-android)
1. [Bygg en standardapp för AEM Forms](#set-up-the-xcode-project)

## Ladda ned kodpaketet för AEM Forms App Source {#download-android-zip}

AEM Forms App Source Code Package refererar till arkivet `adobe-lc-mobileworkspace-src-<version>.zip`. Detta arkiv innehåller den källkod som krävs för att skapa en anpassad AEM Forms-app. Arkivet ingår i det `adobe-aemfd-forms-app-src-pkg-<version>.zip`paket som är tillgängligt för programvarudistribution.

Så här hämtar du filen `adobe-aemfd-forms-app-src-pkg-<version>.zip`:

1. Öppna [Programvarudistribution](https://experience.adobe.com/downloads). Du behöver en Adobe ID för att logga in på Software Distribution.
1. Välj **[!UICONTROL Adobe Experience Manager]** som finns på rubrikmenyn.
1. I avsnittet **[!UICONTROL Filters]**:
   1. Välj **[!UICONTROL Forms]** i listrutan **[!UICONTROL Solution]**.
   2. Välj version och typ för paketet. Du kan också använda alternativet **[!UICONTROL Search Downloads]** för att filtrera resultaten.
1. Välj det paketnamn som gäller för ditt operativsystem, välj **[!UICONTROL Accept EULA Terms]** och välj **[!UICONTROL Download]**.
1. Öppna [Pakethanteraren](https://experienceleague.adobe.com/docs/experience-manager-65-lts/administering/contentmanagement/package-manager.html) och klicka på **[!UICONTROL Upload Package]** för att överföra paketet.
1. Markera paketet och klicka på **[!UICONTROL Install]**.
1. Om du vill hämta källkodsarkivet öppnar du **https://&lt;server>:&lt;port>/crx/de/content/forms/mobileapps/src/adobe-lc-mobileworkspace-src-&lt;version>.zip** i webbläsaren. ZIP-filen för Android-appen hämtas till din enhet.
1. Extrahera innehållet i ZIP-filen till en mapp i det lokala filsystemet. Exempel: *C:\&lt;Mappstruktur>\adobe-lc-mobileworkspace-src-2.4.20*

I följande bild visas strukturen för mappen `adobe-lc-mobileworkspace-src-<version>.zip\android`.

![zip_android_folder_structure](assets/zip_android_folder_structure.png)

## Ange miljövariabler {#set-environment-variable-android}

Ange följande miljövariabler innan du startar byggprocessen för AEM Forms-programmet:

* Ställ in miljövariabeln JAVA_HOME på JDK-programvarans plats i det lokala filsystemet. Exempel: C:\Program Files\Java\jdk1.8.0_181
* Ställ in systemmiljövariabeln `ANDROID_SDK_ROOT` på SDK-platsen för Android. Till exempel C:\Users\&amp;lt;användarnamn>\AppData\Local\Android\Sdk
* Ange systemmiljövariabeln `Path` så att den omfattar mapplatserna för plattformsverktyg och verktyg för Android. Till exempel C:\Users\&amp;lt;användarnamn>\AppData\Local\Android\Sdk\platform-tools och C:\Users\&amp;lt;användarnamn>\AppData\Local\Android\Sdk\tools.

## Bygg en standardapp för AEM Forms {#set-up-the-xcode-project}

När du har sparat filen adobe-lc-mobileworkspace-src-&lt;version>.zip i det lokala filsystemet och angett systemvariabler skapar du en AEM Forms Android-standardapp med något av följande alternativ:

* [Bygg AEM Forms-app med Android Studio](#using-android-studio)
* [Generera APK-fil med Android Studio](#generate-apk-android-studio)

### Bygg AEM Forms-app med Android Studio {#using-android-studio}

Så här skapar du en AEM Forms-app med Android Studio:

1. Starta Android Studio på datorn.
1. Klicka på **Öppna ett befintligt Android Studio-projekt**. Om dialogrutan för att öppna ett befintligt projekt inte visas automatiskt väljer du **Arkiv** > **Öppna**.
1. Navigera till *adobe-lc-mobileworkspace-src-&lt;version>.zip/android* i det lokala filsystemet och klicka på **OK**.

   Alternativet **android** visas i den vänstra rutan.

   ![android_folder_studio](assets/android_folder_studio.png)

1. Välj **android** i den vänstra rutan och klicka på **Kör** > **Kör android**.
1. Markera Android-enheten under Anslutna enheter i dialogrutan Välj distributionsmål och klicka på OK.

   När du har skapat utvecklingsmiljön kan du nu använda anpassningar i appen. Använd följande artiklar för att anpassa appen:

   * [Anpassning av varumärkesprofilering](/help/forms/using/branding-customization.md)
   * [Temaanpassning](/help/forms/using/theme-customization.md)
   * [Gestanpassning](/help/forms/using/gesture-customization.md)

   När du har anpassat programmet kan du generera APK-filen för distribution.

### Generera APK-fil med Android Studio {#generate-apk-android-studio}

Så här skapar du .apk-filen med Android Studio:

1. Starta Android Studio på datorn.
1. Välj **Öppna ett befintligt Android Studio-projekt**. Om dialogrutan för att öppna ett befintligt projekt inte visas automatiskt väljer du **Arkiv** > **Öppna**.
1. Navigera till *adobe-lc-mobileworkspace-src-&lt;version>.zip/android* i det lokala filsystemet och klicka på **OK**.

   Android-alternativet visas i den vänstra rutan.

1. Om du vill generera APK-filen väljer du **Build** > **Build APK**.

   Du kan också välja **Skapa** > **Generera signerad APK** för att generera en [signerad version](https://developer.android.com/studio/publish/app-signing) av .apk-filen.

## Använda Android Debug Bridge {#build-android-debug-bridge}

När APK-filen har skapats kör du följande kommando för att installera programmet på en Android-enhet med [Android Debug Bridge](https://developer.android.com/tools/adb).

**Windows-användare:** `adb install %HOMEPATH%\Projects\[your-project]\adobe-lc-mobileworkspace-src-[version]\android\build\outputs\apk\android-debug.apk`

**Mac-användare:** `adb install [User_Home]/Projects/[your-project]/adobe-lc-mobileworkspace-src-[version]/android/build/outputs/apk/android-debug.apk`
