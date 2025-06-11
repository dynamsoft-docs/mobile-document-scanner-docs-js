---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
title: Mobile Web Capture - Adding the dependency
keywords: Documentation, Mobile Web Capture, Adding the dependency
breadcrumbText: Adding the dependency
description: Mobile Web Capture Documentation Adding the dependency
permalink: /gettingstarted/add_dependency-v1.1.html
---

# Adding the dependency

To build the solution, we need to include five packages

1. `dynamsoft-document-viewer`: Required. It provides functions to create the viewers.
2. `dynamsoft-core`: Required. It encompasses common classes, interfaces, and enumerations that are shared across all SDKs in DCV architecture.
3. `dynamsoft-license`: Required. It introduces the `LicenseManager` class.
4. `dynamsoft-document-normalizer`: Required. It defines interfaces and enumerations specifically tailored to the document normalizer module.
5. `dynamsoft-capture-vision-router`: Required. It defines the class `CaptureVisionRouter`, which governs the entire image processing workflow.

## Use a CDN

The simplest way to include the SDK is to use either the [jsDelivr](https://jsdelivr.com/) or [UNPKG](https://unpkg.com/) CDN.

- jsDelivr

  ```html
  <script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-viewer@1.1.0/dist/ddv.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/dynamsoft-core@3.0.30/dist/core.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/dynamsoft-license@3.0.20/dist/license.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-normalizer@2.0.20/dist/ddn.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/dynamsoft-capture-vision-router@2.0.30/dist/cvr.js"></script>
  ```

- UNPKG

  ```html
  <script src="https://unpkg.com/dynamsoft-document-viewer@1.1.0/dist/ddv.js"></script>
  <script src="https://unpkg.com/dynamsoft-core@3.0.30/dist/core.js"></script>
  <script src="https://unpkg.com/dynamsoft-license@3.0.20/dist/license.js"></script>
  <script src="https://unpkg.com/dynamsoft-document-normalizer@2.0.20/dist/ddn.js"></script>
  <script src="https://unpkg.com/dynamsoft-capture-vision-router@2.0.30/dist/cvr.js"></script>
  ```

## Host yourself

Besides using the CDN, you can also download the SDKs and host related files on your own website/server before including it in your application. When using a CDN, resources related to `dynamsoft-image-processing` and `dynamsoft-capture-vision-std` are automatically loaded over the network; When using them locally, these two packages need to be configured manually.

**Step 1** Download the SDKs:
{% comment %}
- From the website

  [Download the JavaScript ZIP package](https://www.dynamsoft.com/mobile-web-capture/downloads/)

- yarn

  ```cmd
  yarn add dynamsoft-document-viewer@1.1.0
  yarn add dynamsoft-core@3.0.30
  yarn add dynamsoft-license@3.0.20
  yarn add dynamsoft-document-normalizer@2.0.20
  yarn add dynamsoft-capture-vision-router@2.0.30
  yarn add dynamsoft-capture-vision-std@1.0.0
  yarn add dynamsoft-image-processing@2.0.30
  ```
{% endcomment %}

- npm

  ```cmd
  npm install dynamsoft-document-viewer@1.1.0
  npm install dynamsoft-core@3.0.30
  npm install dynamsoft-license@3.0.20
  npm install dynamsoft-document-normalizer@2.0.20
  npm install dynamsoft-capture-vision-router@2.0.30
  npm install dynamsoft-capture-vision-std@1.0.0
  npm install dynamsoft-image-processing@2.0.30
  ```



**Step 2** Include the SDKs

Depending on where you put them, you can typically include them like this:
{% comment %}
  ```html
  <script src="./distributables/dynamsoft-document-viewer@1.1.0/dist/ddv.js"></script>
  <script src="./distributables/dynamsoft-core@3.0.30/dist/core.js"></script>
  <script src="./distributables/dynamsoft-license@3.0.20/dist/license.js"></script>
  <script src="./distributables/dynamsoft-document-normalizer@2.0.20/dist/ddn.js"></script>
  <script src="./distributables/dynamsoft-capture-vision-router@2.0.30/dist/cvr.js"></script>
  ```

or
{% endcomment %}
  ```html
  <script src="./node_modules/dynamsoft-document-viewer/dist/ddv.js"></script>
  <script src="./node_modules/dynamsoft-core/dist/core.js"></script>
  <script src="./node_modules/dynamsoft-license/dist/license.js"></script>
  <script src="./node_modules/dynamsoft-document-normalizer/dist/ddn.js"></script>
  <script src="./node_modules/dynamsoft-capture-vision-router/dist/cvr.js"></script>
  ```

**Step 3** Specify the location of the engine files(optinal)

If you would like to use the SDKs completely offline, please refer to [Use your own hosted engine files](#use-your-own-hosted-engine-files).

## Specify the location of the engine files

This is usually only required with frameworks like Angular or React, etc. where the referenced JavaScript files such as cvr.js, ddn.js are compiled into another file, or hosting the engine files and using the SDKs completely offline. The purpose is to tell the SDK where to find the engine files (*.worker.js, *.wasm.js and *.wasm, etc.).

### Use the jsDelivr CDN with frameworks like Angular or React, etc.
  ```typescript
  Dynamsoft.DDV.Core.engineResourcePath = "https://cdn.jsdelivr.net/npm/dynamsoft-document-viewer@1.1.0/dist/engine";
  Dynamsoft.Core.CoreModule.engineResourcePaths.core = "https://cdn.jsdelivr.net/npm/dynamsoft-core@3.0.30/dist/";
  Dynamsoft.Core.CoreModule.engineResourcePaths.license = "https://cdn.jsdelivr.net/npm/dynamsoft-license@3.0.20/dist/";
  Dynamsoft.Core.CoreModule.engineResourcePaths.ddn = "https://cdn.jsdelivr.net/npm/dynamsoft-document-normalizer@2.0.20/dist/";
  Dynamsoft.Core.CoreModule.engineResourcePaths.cvr = "https://cdn.jsdelivr.net/npm/dynamsoft-capture-vision-router@2.0.30/dist/";
  Dynamsoft.Core.CoreModule.engineResourcePaths.std = "https://cdn.jsdelivr.net/npm/dynamsoft-capture-vision-std@1.0.0/dist/";
  Dynamsoft.Core.CoreModule.engineResourcePaths.dip = "https://cdn.jsdelivr.net/npm/dynamsoft-image-processing@2.0.30/dist/";
  ```

### Use your own hosted engine files

  ```typescript
  //Feel free to change it to your own location of these files
  Dynamsoft.DDV.Core.engineResourcePath = "./node_modules/dynamsoft-document-viewer/dist/engine";
  Dynamsoft.Core.CoreModule.engineResourcePaths.core = "./node_modules/dynamsoft-core/dist/";
  Dynamsoft.Core.CoreModule.engineResourcePaths.license = "./node_modules/dynamsoft-license/dist/";
  Dynamsoft.Core.CoreModule.engineResourcePaths.ddn = "./node_modules/dynamsoft-document-normalizer/dist/";
  Dynamsoft.Core.CoreModule.engineResourcePaths.cvr = "./node_modules/dynamsoft-capture-vision-router/dist/";
  Dynamsoft.Core.CoreModule.engineResourcePaths.std = "./node_modules/dynamsoft-capture-vision-std/dist/";
  Dynamsoft.Core.CoreModule.engineResourcePaths.dip = "./node_modules/dynamsoft-image-processing/dist/";
  ```
