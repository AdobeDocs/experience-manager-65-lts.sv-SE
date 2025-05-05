---
title: Anpassning av varumärkesprofilering
description: Anpassa programikonen, programnamnet, startbilderna och inloggningssidan för att ge AEM Forms-appen ett distinkt utseende och känsla.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-app
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: e2d31db9-bb47-4260-8ebb-000a7b776f53
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 0%

---

# Anpassning av varumärkesprofilering {#branding-customization}

Du kan anpassa programikonen, programnamnet, startbilderna och inloggningssidan för att ge ett tydligt organisationsspecifikt utseende för AEM Forms-programmet. Du kan till exempel ändra bilderna så att de använder logotyper från din organisation. AEM Forms-appen stöder följande anpassningar:

* Anpassa programikoner och starta bilder
* Anpassa appnamn
* Anpassa bilder på inloggningssidan
* Anpassa logotypen på appmenyn

## Anpassa ikoner och starta bilder {#customizing-icon-and-launch-images}

Följ de här stegen för att anpassa standardprogramikonen och startbilden för AEM Forms-programmet:

>[!NOTE]
>
>Använd icke-sammanflätade PNG-format för alla ikoner och bilder.

### Anpassa ikoner och starta bilder {#to-customize-icon-and-launch-images}

#### För iOS {#for-ios}

1. Öppna projektet `Capture.xcodeproj` i Xcode.
1. (***Navigera till &#x200B;** [!UICONTROL Capture > Capture > Supporting Files > Capture-info.plist]&#x200B;**om du vill anpassa ikonen***) i navigeringsvyn i Capture. Klicka på listrutan bredvid ikonfilerna. Ange namnet på ikonfilen (.png) och överför filen på **[!UICONTROL Capture > Capture > Resources > icons]**. De dimensioner som stöds för närvarande är: 29 × 29, 50 × 50, 58 × 58, 72 × 72, 100 × 100 och 144 × 144.
1. (***För anpassning av startbilder***) Kontrollera att filnamnen för dina bilder är:

   * För stående: `Default-Portrait~ipad.png` och `Default-Portrait@2x~ipad.png`
   * För liggande: `Default-Landscape~ipad.png` och `Default-Landscape@2x~ipad.png`

   Överför dem till Capture-projektet för att ersätta befintliga filer i projektet.

   >[!NOTE]
   >
   >Kontrollera att namnet och upplösningen på bilden matchar den bild du ersätter i projektet.

1. Bygg och kör AEM Forms på iOS-enheter eller iOS-simulator.

#### För Android {#for-android}

1. Namnge programikonfilerna som:

   `ic_launcher.png`

1. Placera motsvarande ikonfiler i följande kataloger:

   * `[User_Home]/Projects/[your-project]/src/android/res/drawable-hdpi`
   * `[User_Home]/Projects/[your-project]/src/android/res/drawable-mdpi`
   * `[User_Home]/Projects/[your-project]/src/android/res/drawable-xhdpi`
   * `[User_Home]/Projects/[your-project]/src/android/res/drawable-xxhdpi`
   * `[User_Home]/Projects/[your-project]/src/android/res/drawable-xxxhdpi`

   >[!NOTE]
   >
   >Kontrollera att namnet och upplösningen på bilden matchar den bild du ersätter i projektet.

1. Återskapa AEM Forms-appen.

### För Windows {#for-windows}

1. Ersätt ikonerna i sökvägen:

   `%HOMEPATH%\adobe-lc-mobileworkspace-src-<version>\src\windows\MWSWindows\res\icons\windows`

1. Ersätt startbilden i sökvägen:

   `%HOMEPATH%\adobe-lc-mobileworkspace-src-<version>\src\windows\MWSWindows\res\screens\windows`

   >[!NOTE]
   >
   >Kontrollera att namnet och upplösningen på bilden matchar den bild du ersätter i projektet.

1. Återskapa AEM Forms-appen.

## Anpassa appnamnet {#customize-the-app-name}

### För iOS {#for-ios-1}

1. Öppna projektet `Capture.xcodeproj` i Xcode.
1. Navigera till **[!UICONTROL Capture > Capture > Supporting Files > InfoPlist.strings]** i navigeringsvyn i Capture.

   Uppdatera värdet för attributet `CFBundleDisplayName` till ett namn som du vill visa för programmet.

1. Bygg och kör AEM Forms på iOS-enheter eller iOS-simulator.

   Mer information om hur du skapar appen för iOS finns i [Konfigurera Xcode-projektet och skapa iOS-appen](/help/forms/using/setup-xcode-project-build-installer.md).

### För Android {#for-android-1}

1. Öppna följande XML i en text- eller XML-redigerare:

   `[User_Home]/Projects/[your-project]/src/android/res/values/strings.xml and android/res/values-en/strings.xml`

1. Uppdatera värdet för nyckeln `app_name`.
1. Återskapa AEM Forms-appen.

   Mer information om hur du skapar appen för Android finns i [Konfigurera Eclipse-projektet och skapa Android-appen](/help/forms/using/setup-eclipse-project-build-installer.md).

### För Windows {#for-windows-1}

1. Öppna följande XML i en textredigerare:

   `%HOMEPATH%\adobe-lc-mobileworkspace-src-<version>\src\windows\MWSWindows\config.xml`

1. Uppdatera värdet i taggen `<name>...</name>`.
1. Återskapa AEM Forms-appen.

   Mer information om hur du skapar appen för Windows finns i [Konfigurera Visual Studio-projektet och skapa Windows-appen](/help/forms/using/setup-visual-studio-project-build-installer.md).

## Anpassa bilder på inloggningssidan {#customizing-images-on-the-login-page}

Inloggningssidan för AEM Forms-programmet innehåller logotyper och bakgrundsbilder. Logotypen finns ovanför inloggningsdialogrutan och bakgrundsbilden finns under inloggningsdialogrutan. Följ de här stegen för att anpassa standardbilden på inloggningssidan:

**Innan du börjar**

Kontrollera att du har följande bilder:

<table>
 <tbody>
  <tr>
   <th><p>Beskrivning</p> </th>
   <th><p>Storlek</p> </th>
   <th><p>Filnamn</p> </th>
  </tr>
  <tr>
   <td><p>Logotyp</p> </td>
   <td><p>72 x 72 pixlar</p> </td>
   <td><p>LC-logo.png</p> </td>
  </tr>
  <tr>
   <td><p>Bakgrundsbild (stående)</p> </td>
   <td><p>1 280 x 989 pixlar</p> </td>
   <td><p>Landing_bg.jpeg</p> </td>
  </tr>
 </tbody>
</table>

**Anpassa bilder på inloggningssidan med Xcode**

1. Öppna projektet `Capture.xcodeproj` i Xcode.

1. Navigera till mappen `www/wsmobile/images`.
1. Om du vill ändra logotyp ersätter du standardfilen `LC-logo.png` med den anpassade filen `LC-logo.png`.
1. Om du vill ändra bakgrunden ersätter du standardfilen `Landing_bg.jpeg` med den anpassade `Landing_bg.jpeg`-filen.
1. Bygg och kör AEM Forms på iOS-enheter eller iOS-simulator.

### Anpassa bilder på inloggningssidorna med Eclipse {#to-customize-images-on-the-login-pages-using-eclipse}

1. Öppna Android i Eclipse.

1. Navigera till mappen `assets/www/wsmobile/images`.
1. Om du vill ändra logotyp ersätter du standardfilen `LC-logo.png` med den anpassade filen `LC-logo.png`.
1. Om du vill ändra bakgrunden ersätter du standardfilen `Landing_bg.jpeg` med den anpassade `Landing_bg.jpeg`-filen.
1. Bygg och kör AEM Forms på Android-enheter.

### Anpassa bilder på inloggningssidorna med Visual Studio {#to-customize-images-on-the-login-pages-using-visual-studio}

1. Öppna projektet `MWSWindows.sln` i Visual Studio.

1. Navigera till mappen `MWSWindows\www\wsmobile\images`.
1. Om du vill ändra logotyp ersätter du standardfilen `LC-logo.png` med den anpassade filen `LC-logo.png`.
1. Om du vill ändra bakgrunden ersätter du standardfilen `Landing_bg.jpeg` med den anpassade `Landing_bg.jpeg`-filen.
1. Bygg och kör AEM Forms på Windows-enheter.

## Anpassa logotypen på appmenyn {#customizing_images_on_the_login_page-1}

När du har loggat in på AEM Forms-appen och valt menyknappen visas logotypen ovanför menyn. Följ de här stegen för att anpassa standardlogotypen:

**Innan du börjar**

Kontrollera att du har följande bild:

<table>
 <tbody>
  <tr>
   <th><p>Beskrivning</p> </th>
   <th><p>Storlek</p> </th>
   <th><p>Filnamn</p> </th>
  </tr>
  <tr>
   <td><p>Logotyp</p> </td>
   <td><p>72 x 72 pixlar</p> </td>
   <td><p>aem_icon.png</p> </td>
  </tr>
 </tbody>
</table>

**Anpassa bilder på inloggningssidan med Xcode**

1. Öppna projektet `Capture.xcodeproj` i Xcode.

1. Navigera till mappen `www/wsmobile/images`.
1. Om du vill ändra logotypen ersätter du standardfilen `aem_icon.png` med den anpassade filen `aem_icon.png`.
1. Bygg och kör AEM Forms på iOS-enheter eller iOS-simulator.

### Anpassa bilder på inloggningssidorna med Eclipse {#to-customize-images-on-the-login-pages-using-eclipse-1}

1. Öppna Android i Eclipse.

1. Navigera till mappen `assets/www/wsmobile/images`.
1. Om du vill ändra logotypen ersätter du standardfilen `aem_icon.png` med den anpassade filen `aem_icon.png`.
1. Bygg och kör AEM Forms på Android-enheter.

### Anpassa bilder på inloggningssidorna med Visual Studio {#to-customize-images-on-the-login-pages-using-visual-studio-1}

1. Öppna projektet `MWSWindows.sln` i Visual Studio.

1. Navigera till mappen `MWSWindows\www\wsmobile\images`.
1. Om du vill ändra logotypen ersätter du standardfilen `aem_icon.png` med den anpassade filen `aem_icon.png`.
1. Bygg och kör AEM Forms på Windows-enheter.
