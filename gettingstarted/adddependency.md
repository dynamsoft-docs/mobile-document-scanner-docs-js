---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
title: Mobile Web Capture - Adding the dependency
keywords: Documentation, Mobile Web Capture, Adding the dependency
breadcrumbText: Adding the dependency
description: Mobile Web Capture Documentation Adding the dependency
permalink: /gettingstarted/add_dependency.html
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

Options to download the SDK:
{% comment %}
- From the website

  [Download the JavaScript ZIP package](https://www.dynamsoft.com/mobile-web-capture/downloads/)
{% endcomment %}
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



Depending on how you downloaded the SDK and where you put it, you can typically include it like this:

  ```html
  <!-- Upon extracting the zip package into your project, you can generally include it in the following manner -->
  <script src="./distributables/dynamsoft-document-viewer@1.1.0/dist/ddv.js"></script>
  <script src="./distributables/dynamsoft-core@3.0.30/dist/core.js"></script>
  <script src="./distributables/dynamsoft-license@3.0.20/dist/license.js"></script>
  <script src="./distributables/dynamsoft-document-normalizer@2.0.20/dist/ddn.js"></script>
  <script src="./distributables/dynamsoft-capture-vision-router@2.0.30/dist/cvr.js"></script>
  ```

or

  ```html
  <script src="./node_modules/dynamsoft-document-viewer@1.1.0/dist/ddv.js"></script>
  <script src="./node_modules/dynamsoft-core@3.0.30/dist/core.js"></script>
  <script src="./node_modules/dynamsoft-license@3.0.20/dist/license.js"></script>
  <script src="./node_modules/dynamsoft-document-normalizer@2.0.20/dist/ddn.js"></script>
  <script src="./node_modules/dynamsoft-capture-vision-router@2.0.30/dist/cvr.js"></script>
  ```

## Specify the location of the engine files

This is usually only required with frameworks like Angular or React, etc. where the referenced JavaScript files such as cvr.js, ddn.js are compiled into another file, or using the SDKs completely offline.

The purpose is to tell the SDK where to find the engine files (*.worker.js, *.wasm.js and *.wasm, etc.). The API is called [`Dynamsoft.Core.CoreModule.engineResourcePaths`](https://www.dynamsoft.com/capture-vision/docs/web/programming/javascript/api-reference/core/core-module-class.html?product=ddn&lang=javascript#engineresourcepaths):

  ```typescript
  //The following code uses the jsDelivr CDN, feel free to change it to your own location of these files
  Dynamsoft.DDV.Core.engineResourcePath = "https://cdn.jsdelivr.net/npm/dynamsoft-document-viewer@1.1.0/dist/engine";
  Dynamsoft.Core.CoreModule.engineResourcePaths.core = "https://cdn.jsdelivr.net/npm/dynamsoft-core@3.0.30/dist/";
  Dynamsoft.Core.CoreModule.engineResourcePaths.license = "https://cdn.jsdelivr.net/npm/dynamsoft-license@3.0.20/dist/";
  Dynamsoft.Core.CoreModule.engineResourcePaths.ddn = "https://cdn.jsdelivr.net/npm/dynamsoft-document-normalizer@2.0.20/dist/";
  Dynamsoft.Core.CoreModule.engineResourcePaths.cvr = "https://cdn.jsdelivr.net/npm/dynamsoft-capture-vision-router@2.0.30/dist/";
  Dynamsoft.Core.CoreModule.engineResourcePaths.std = "https://cdn.jsdelivr.net/npm/dynamsoft-capture-vision-std@1.0.0/dist/";
  Dynamsoft.Core.CoreModule.engineResourcePaths.dip = "https://cdn.jsdelivr.net/npm/dynamsoft-image-processing@2.0.30/dist/";
  ```