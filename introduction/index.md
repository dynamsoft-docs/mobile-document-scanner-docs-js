---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: false
title: Mobile Web Capture - Introduction
keywords: Documentation, Mobile Document Scanner, Web, JS Edition, Introduction
breadcrumbText: Introduction
description: Mobile Web Capture Documentation Introduction
permalink: /introduction/index.html
---

# Introduction

When digitizing physical documents — whether for easier storage, better accessibility, or streamlined processing — a hardware scanner is often the preferred choice. For integrating this functionality into web applications, **Dynamsoft’s** [Dynamic Web TWAIN](https://www.dynamsoft.com/web-twain/docs/introduction/index.html) is a widely popular option.

However, when hardware scanners are not feasible or convenient, mobile device cameras can serve as effective alternatives. **Mobile Web Capture (MWC)** is a document scanning SDK specifically designed to address this need.

## Common Usage Scenarios

1. **Document Capture** – Capture a *single* clear image of a physical document, such as a patient intake form or the biographical page of a passport.
2. **Document Management** – Capture images of *multiple* document pages (e.g., a contract) and compile them into a single PDF.
3. **Document Processing** – Add *annotations* to scanned pages and perform common editing tasks such as cropping and color filtering.

## Choosing the Right Solution

**MWC** is designed to handle all three scenarios seamlessly. However, for **single-page document capture (Scenario 1)**, **MWC** may feel overly feature-rich. To address this, we developed **Dynamsoft Document Scanner (DDS)** — a streamlined solution tailored for capturing single-page documents using mobile cameras.

**DDS** not only captures documents but also enhances their quality to meet professional standards, making it an ideal tool for **Scenario 1** as described above.

> [!TIP]
> Not sure if it is the right fit? **Try the** [DDS demo](https://demo.dynamsoft.com/document-scanner/) first. If it meets your needs, you can skip the rest of this introduction and proceed directly to the [DDS user guide]({{ site.guides }}document-scanner.html).

However, if you need multi-page capture, **MWC** extends DDS’s functionality by supporting multi-page documents. Additionally, **MWC** offers advanced features such as document processing, editing, and annotation, making it a comprehensive solution for managing complex document workflows.

In short, for scenarios requiring **document management beyond single-page capture, MWC provides the flexibility and functionality needed to streamline the entire process.**

## Key Features

| Feature                                                                                             | DDS  | MWC  |
| :-------------------------------------------------------------------------------------------------- | :--- | :--- |
| Capture single documents using mobile devices or webcams                                            | ✓    | ✓    |
| Import a single local image of a document                                                           | ✓    | ✓    |
| Import multiple images or PDF files                                                                 |      | ✓    |
| Automatically detect document borders during image capture                                          | ✓    | ✓    |
| Automatically capture and correct images to match detected document boundaries                      | ✓    | ✓    |
| Organize and manage a multi-page document                                                           |      | ✓    |
| Organize and manage multiple documents in a library                                                 |      | ✓    |
| Annotate documents with comments, highlights, or other markings                                     |      | ✓    |
| Easily transfer pages between documents                                                             |      | ✓    |
| Export a single-page document as an image                                                           | ✓    | ✓    |
| Export multi-page documents as PDFs with options for selected pages, one or multiple full documents |      | ✓    |

> [!NOTE]
> To deliver these features, we built **DDS** using two core Dynamsoft products: [**Dynamsoft Camera Enhancer**](https://www.dynamsoft.com/camera-enhancer/docs/web/programming/javascript/user-guide/index.html?lang=javascript) (DCE) and [**Dynamsoft Document Normalizer**](https://www.dynamsoft.com/document-normalizer/docs/web/programming/javascript/user-guide/index.html?lang=javascript) (DDN).
>
> **MWC** extends this foundation by adding document management, processing, and editing features through [**Dynamsoft Document Viewer**](https://www.dynamsoft.com/document-viewer/docs/introduction/index.html) (DDV). Both products operate within the [**Dynamsoft Capture Vision**](https://www.dynamsoft.com/capture-vision/docs/core/architecture/?lang=javascript) (DCV) architecture.

## Design Principles

We designed **DDS** and **MWC** with three core principles in mind:

1. **Minimal Code** - High-level APIs provide full functionality with just **a couple of lines of code**, significantly reducing development and maintenance costs.
2. **Ready-to-Use UI** - Pre-integrated components and a pre-designed UI enable a **quick setup** while minimizing design efforts.
3. **Effortless Customization** - Tailored configuration objects allow for **easy workflow customization**, addressing common document scenarios without adding development complexity.

The following example demonstrates how simple it is to power a complete document scanning and management workflow, including all UI elements:

```javascript
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE",
});
await mobileWebCapture.launch();
```
The UI elements are modularized into distinct Views, each offering developer-friendly configuration options. These options enable flexible business logic through straightforward configuration objects, helping to reduce development costs and streamline maintenance.

The following section provides a high-level overview of the Views, followed by a detailed look at two specific Views.

## Views

**DDS** and **MWC** workflows are composed of **Views**, each of which comes **ready-to-use** and can be easily customized using configuration objects.

### DDS Views

**DDS** includes the following Views for document scanning and correction:

1. **Document Scanner View** – Captures documents using a camera scanner.
2. **Document Correction View** – Applies further perspective cropping and adjustments.
3. **Document Result View** – Provides a preview of the scanned document.

### MWC Views

Building on **DDS**, **MWC** extends functionality with additional Views for full document management, editing, and annotation capabilities:

1. **Library View** – Manage all multi-page documents.
2. **Document View** – Organize and manage all pages within a specific document.
3. **Page View** – View and edit a single page in a document with features including "Cropy", "Rotate", "Filter", and "Annotate".
4. **Transfer View** – Copy or move pages between documents.

### Detailed View Breakdown

Here is a closer look at two specific Views:

#### Document Scanner View

![Document Scanner View](/assets/imgs/document-scanner-view-demo.png)

The **Document Scanner View** (available in both **DDS** and **MWC**) captures scans of documents. It automatically detects document boundaries in the video feed and can optionally select the best frame for scanning, eliminating the need for users to manually snap the image.

#### Page View

![Editor View](/assets/imgs/editor-view-demo.png)

The **Page View** (**MWC** only) allows users to view and edit an individual page of a document with a variety of tools. It supports common editing tasks such as cropping, color filtering, and comprehensive annotation options to enhance document clarity and presentation.

<!-- ### Capture Result View

![Capture Result View](/assets/imgs/scanner-view-demo.png)

The Capture Result View (DDS and MWC) displays the scan of the document with any applied visual corrections such as perspective cropping using boundaries detected from the Scanner View. The user may also manually alter the detected boundaries by navigating to the Correction View.

### Correction View

![Correction View](/assets/imgs/correction-view-demo.png)

The Correction View (DDS and MWC) presents a visual interface for the user to adjust perspective cropping. -->

<!-- #### Library View

![Library View](/assets/imgs/library-view-demo.png)

The Library View (MWC only) displays all documents scanned by MWC. From here, the user can create or scan into new documents, upload documents to the upstream server, view upload history, and more. Clicking into a document brings the user into the Document View to manage individual documents in further detail. One of the key customization options of the Library View is specifying file upload behavior so as to integrate MWC seamlessly into a wide variety of web applications and their backends. -->

<!-- ![Document View](/assets/imgs/document-view-demo.png)

The Document View displays a thumbnail gallery of pages within the selected document. The user can import (scan or load file) pages, or export the document at hand. Here the user has access to the Transfer View, which can copy or move pages across documents. Finally, the user can click into an individual page to enter the Page View.

![Page View](/assets/imgs/page-view-demo.png)

The Page View gives the user a fullscreen view of the document page, and gives the user more I/O options on a page level (i.e. to be able to delete the page)



![Transfer View](/assets/imgs/transfer-view-demo.png)

The Transfer View  -->

## System Requirements

### Secure Context (HTTPS Deployment)

When deploying your web application for production, ensure it is served over a **secure HTTPS connection**. This is required for the following reasons:

1. **Browser Security Restrictions** – Most browsers only allow access to camera video streams in a secure context.
    > [!NOTE]
    > Some browsers like Chrome may grant access to camera video streams for `http://127.0.0.1`, `http://localhost`, or even pages opened directly from the local file system (`file:///...`). This can be helpful during development and testing.

2. **Dynamsoft License Requirements** – A secure context is required for **Dynamsoft licenses** to function properly.

### Required Browser Features

The following** browser features** are required for the **DCE** and **DDN** components of **DDS** and **MWC**:

1. [`WebAssembly`](https://caniuse.com/?search=WebAssembly)
2. [`Blob`](https://caniuse.com/?search=Blob)
3. [`URL`](https://caniuse.com/?search=URL)/[`createObjectURL`](https://caniuse.com/?search=createObjectURL)
4. [`Web Workers`](https://caniuse.com/?search=Worker)

### Supported Browsers

The table below lists the **minimum supported versions** of browsers based on these requirements:

| Browser | Version |
| :-----: | :-----: |
| Chrome  |  v78+   |
| Firefox |  v79+   |
| Safari  |  v15+   |
|  Edge   |  v92+   |

## Next Step

- If you only need to scan single-page documents and do not require handling PDF files (import or export), you can proceed to the [Dynamsoft Document Scanner Developer Guide]({{ site.guides }}document-scanner.html).

- If you need to handle multi-page documents, PDF files, annotations, and more, you will need the full-featured **Mobile Web Capture (MWC)**. In this case, proceed to the [Mobile Web Capture Developer Guide]({{ site.guides }}mobile-web-capture.html).
