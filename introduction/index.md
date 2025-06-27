---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: false
title: Mobile Document Scanner JS Edition - Introduction
keywords: Documentation, Mobile Document Scanner, Web, JS Edition, Introduction
breadcrumbText: Introduction
description: Mobile Document Scanner JS Edition Documentation Introduction
---

# Introduction

When digitizing physical documents — whether for easier storage, better accessibility, or streamlined processing — a hardware scanner is often the preferred choice. For integrating this functionality into web applications, **Dynamsoft’s** [**Dynamic Web TWAIN**](https://www.dynamsoft.com/web-twain/docs/introduction/index.html) is a widely popular option.

However, when hardware scanners are not feasible or convenient, mobile device cameras can serve as effective alternatives. **Dynamsoft's Mobile Document Scanner (MDC)** is a document scanning SDK specifically designed to address this need.

## Common Usage Scenarios

1. **Document Capture** – Capture a *single* clear image of a physical document, such as a patient intake form or the biographical page of a passport.
2. **Document Management** – Capture images of *multiple* document pages (e.g., a contract) and compile them into a single PDF.
3. **Document Processing** – Add *annotations* to scanned pages and perform common editing tasks such as cropping and color filtering.

> [!TIP]
> Not sure if it is the right fit? **Try the** [DDS demo](https://demo.dynamsoft.com/document-scanner/) first. If it meets your needs, you can skip the rest of this introduction and proceed directly to the [DDS user guide]({{ site.guides }}document-scanner.html).

> [!NOTE]
> To deliver these features, we built the **Document Scanner** using two core Dynamsoft products: [**Dynamsoft Camera Enhancer**](https://www.dynamsoft.com/camera-enhancer/docs/web/programming/javascript/user-guide/index.html?lang=javascript) (DCE) and [**Dynamsoft Document Normalizer**](https://www.dynamsoft.com/document-normalizer/docs/web/programming/javascript/user-guide/index.html?lang=javascript) (DDN).

## Design Principles

We designed the **Document Scanner** with three core principles in mind:

1. **Minimal Code** - High-level APIs provide full functionality with just **a couple of lines of code**, significantly reducing development and maintenance costs.
2. **Ready-to-Use UI** - Pre-integrated components and a pre-designed UI enable a **quick setup** while minimizing design efforts.
3. **Effortless Customization** - Tailored configuration objects allow for **easy workflow customization**, addressing common document scenarios without adding development complexity.

The following example demonstrates how simple it is to power a complete document scanning workflow, including all UI elements:

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE",
});
await documentScanner.launch();
```
The UI elements are modularized into distinct Views, each offering developer-friendly configuration options. These options enable flexible business logic through straightforward configuration objects, helping to reduce development costs and streamline maintenance.

The following section provides a high-level overview of the Views, followed by a detailed look at two specific Views.

## Views

Document Scanner workflows are composed of **Views**, each of which comes **ready-to-use** and can be easily customized using configuration objects. These include the following Views for document scanning and correction:

1. **Document Scanner View** – Captures documents using a camera scanner.
2. **Document Correction View** – Applies further perspective cropping and adjustments.
3. **Document Result View** – Provides a preview of the scanned document.

### Highlight: Document Scanner View

![Document Scanner View](/assets/imgs/document-scanner-view-demo.png)

The **Document Scanner View** captures scans of documents. It automatically detects document boundaries in the video feed and can optionally select the best frame for scanning, eliminating the need for users to manually snap the image.

## System Requirements

### Secure Context (HTTPS Deployment)

When deploying your web application for production, ensure it is served over a **secure HTTPS connection**. This is required for the following reasons:

1. **Browser Security Restrictions** – Most browsers only allow access to camera video streams in a secure context.
    > [!NOTE]
    > Some browsers like Chrome may grant access to camera video streams for `http://127.0.0.1`, `http://localhost`, or even pages opened directly from the local file system (`file:///...`). This can be helpful during development and testing.

2. **Dynamsoft License Requirements** – A secure context is required for **Dynamsoft licenses** to function properly.

### Required Browser Features

The following** browser features** are required for the **DCE** and **DDN** components of the **Document Scanner**:

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

- If you only need to scan single-page documents, proceed to the [Dynamsoft Document Scanner Developer Guide]({{ site.guides }}document-scanner.html).

- If you need to handle multi-page documents, multi-**document** scenarios, PDF files, annotations, and more, you will need the fully-featured **Mobile Web Capture (MWC)**. In this case, proceed to the [Mobile Web Capture Developer Guide]({{ site.guides }}mobile-web-capture.html).
