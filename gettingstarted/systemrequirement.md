---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
title: Document Web Capture from Mobile Cameras - System Requirement
keywords: Documentation, Document Web Capture from Mobile Cameras, System Requirement
breadcrumbText: System Requirement
description: Document Web Capture from Mobile Cameras Documentation System Requirement
permalink: /gettingstarted/sys_requirement.html
---

# System Requirements

As DWC from Mobile Cameras solution is implemented by Dynamsoft Document Viewer and Dynamsoft Document Normalizer, these two SDKs requires the following features to work:

- **Secure context (HTTPS deployment)**

  When deploying your application / website for production, make sure to serve it via a secure HTTPS connection. This is required for two reasons
  
  - Access to the camera video stream is only granted in a security context. Most browsers impose this restriction.
    > Some browsers like Chrome may grant the access for `http://127.0.0.1` and `http://localhost` or even for pages opened directly from the local disk (`file:///...`). This can be helpful for temporary development and test.
  
  - Dynamsoft License requires a secure context to work.

- **`WebAssembly`, `Blob`, `URL`/`createObjectURL`, `Web Workers`**

  The above four features are required for the SDKs to work.

## Supported Browsers

The following table is a list of supported browsers based on the above requirements:

  | Browser Name |             Version              |
  | :----------: | :------------------------------: |
  |    Chrome    |             latest               |
  |   Firefox    |             latest               |
  |    Safari    |             latest               |
  |     Edge     |             latest               |

Apart from the browsers, the operating systems may impose some limitations of their own that could restrict the use of the SDKs.

## Reference

- [Dynamsoft Document Viewer - System Requirements](https://officecn.dynamsoft.com:808/document-viewer/docs/gettingstarted/sys_requirement.html)
- [Dynamsoft Document Normalizer - System Requirements](https://www.dynamsoft.com/document-normalizer/docs/web/programming/javascript/user-guide/index.html#system-requirements)