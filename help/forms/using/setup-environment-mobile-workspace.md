---
title: Konfigurera miljö för AEM Forms-program
description: Maskinvara, programvara och licenser för att bygga och driftsätta AEM Forms-appen.
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-app
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: Admin, User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# Konfigurera miljö för AEM Forms-program{#set-up-environment-for-aem-forms-app}

Du behöver följande maskinvara, programvara och licenser för att skapa och distribuera AEM Forms-appen:

## För Windows-enheter {#for-windows-devices}

* Microsoft® Windows 10
* Microsoft® Visual Studio 2015
* Microsoft® Visual Studio Tools for Apache Cordova

## För iOS-enheter {#for-ios-devices}

* Intel-baserad Apple Mac med macOS X 10.9.5 eller senare
* iOS SDK 8.4 eller senare
* Xcode-version: Xcode 6.4 för OS X eller senare
* Medlemskap i iOS Developer Enterprise Program
* Enterprise-certifikat för distribution av interna iOS-appar
* Apple iPad med iOS 8.4 eller senare

## För Android™-enheter {#for-android-devices}

* Android™ Development Toolkit (ADT bundle) som kan hämtas från [https://developer.android.com/studio](https://developer.android.com/studio)
* Om miljön är konfigurerad på ett Mac-system bör ADT installeras i mappen Program.
* Om ADT har installerats på någon annan plats i Mac, eller om miljön har konfigurerats på ett Windows-system, måste ADT-sökvägen för SDK uppdateras i filen `local.properties`. Den här filen är tillgänglig i mappen `src\android` i det extraherade källarkivet `mobileworkspace-src.zip`. I den här filen pekar du variabeln `sdk.dir` till ADT SDK-platsen på skrivbordet.

>[!NOTE]
>
>Adobe-lc-mobileworkspace-src.zip innehåller PhoneGap SDK 5.0. Kontrollera att PhoneGap SDK inte är förinstallerat.
