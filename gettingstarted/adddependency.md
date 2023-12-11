---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
title: Document Web Capture from Mobile Camera - Adding the dependency
keywords: Documentation, Document Web Capture from Mobile Camera, Adding the dependency
breadcrumbText: Adding the dependency
description: Document Web Capture from Mobile Camera Documentation Adding the dependency
permalink: /gettingstarted/add_dependency.html
---

# Adding the dependency

To build the solution, we need to include five packages

1. `dynamsoft-document-viewer`: Required, it provides functions to create the viewers.
2. `dynamsoft-core`: Required, it includes `LicenseManager` class for `dynamsoft-document-normalizer`.
3. `dynamsoft-document-normalizer`: Required, it provides functions to detect boundaries or perform normalization.
4. `dynamsoft-capture-vision-router`: Required, it defines the class `CaptureVisionRouter`, which controls the whole process.
5. `utils`: Optional, it includes the configuration code for document boundaries function. You can also copy the configuration code to your own code.

## Use a CDN

The simplest way to include the SDK is to use either the [jsDelivr](https://jsdelivr.com/) or [UNPKG](https://unpkg.com/) CDN.

- jsDelivr

  ```html
  <script src="https://cdn.jsdelivr.net/npm/ddv"></script>
  <script src="https://cdn.jsdelivr.net/npm/dynamsoft-core@3.0.10/dist/core.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-normalizer@2.0.11/dist/ddn.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/dynamsoft-capture-vision-router@2.0.11/dist/cvr.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/.../utils.js"></script>
  ```

- UNPKG

  ```html
  <script src="https://unpkg.com/ddv"></script>
  <script src="https://unpkg.com/dynamsoft-core@3.0.10/dist/core.js"></script>
  <script src="https://unpkg.com/dynamsoft-document-normalizer@2.0.11/dist/ddn.js"></script>
  <script src="https://unpkg.com/dynamsoft-capture-vision-router@2.0.11/dist/cvr.js"></script>
  <script src="https://unpkg.com//.../utils.js"></script>
  ```

## Host the SDK yourself

Besides using the CDN, you can also download the Solution and host related files on your own website/server before including it in your application.

Options to download the SDK:

- From the website

  [Download the JavaScript ZIP package]()

- yarn

  ```cmd
  yarn add ddv
  yarn add dynamsoft-capture-vision-router@2.0.11
  ```

- npm

  ```cmd
  npm install ddv
  npm install dynamsoft-capture-vision-router@2.0.11
  ```

> Note: Upon installation of `dynamsoft-capture-vision-router`, the compatible versions of `dynamsoft-document-normalizer` and `dynamsoft-core` will be automatically downloaded.

Depending on how you downloaded the SDK and where you put it, you can typically include it like this:

  ```html
  <!-- Upon extracting the zip package into your project, you can generally include it in the following manner -->
  <script src="./distributables/ddvjs"></script>
  <script src="./distributables/dynamsoft-core@3.0.10/dist/core.js"></script>
  <script src="./distributables/dynamsoft-document-normalizer@2.0.11/dist/ddn.js"></script>
  <script src="./distributables/dynamsoft-capture-vision-router@2.0.11/dist/cvr.js"></script>
  <script src="./distributables/untils"></script>
  ```

or

  ```html
  <script src="./node_modules/ddvjs"></script>
  <script src="./node_modules/dynamsoft-core/dist/core.js"></script>
  <script src="./node_modules/dynamsoft-document-normalizer/dist/ddn.js"></script>
  <script src="./node_modules/dynamsoft-capture-vision-router/dist/cvr.js"></script>
  <script src="./node_modules/untils"></script>
  ```