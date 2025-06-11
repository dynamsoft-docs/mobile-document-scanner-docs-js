---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
title: Mobile Web Capture - Release Notes
keywords: Documentation, Mobile Web Capture, Release Notes
breadcrumbText: Release Notes
description: Mobile Web Capture Documentation Release Notes
permalink: /release-notes/mobile-web-capture.html
---

# Release Notes

## 3.1.0 (05/01/2025)

The most notable improvement in this version is the pluggable scanner feature. This allows MWC to integrate any custom scanner.<!-- , e.g. our existing [MRZ scanner](https://www.dynamsoft.com/mrz-scanner/docs/web/introduction/index.html). -->

### Features

1. Add Pluggable Scanner feature which integrates any scanner satisfying the following:
   1. Implements the [`MWCScanner`](https://www.dynamsoft.com/mobile-web-capture/docs/api/mobile-web-capture.html#mwcscanner) interface
   2. Implements a `launch()` method to return a result that includes:
      1. `_imageData` with a `toBlob()` function
      2. `imageData: true`
      3. `status.code` equal to `EnumresultStatus.RS_SUCCESS`
2. Update Document Scanner to [version 1.2](https://github.com/Dynamsoft/document-scanner-javascript/releases/tag/v1.2.0)
3. Change MWC Header color to make the component visually distinct
4. Move the "Select All" and "Cancel" buttons to the header in the Document View

### Fixes

1. The select mode now toggles off after deleting a page on Document View

## 3.0.1 (02/07/2025)

* Fixed a bug in the Document Result View where the **Download** button icon was not displayed.

## 3.0.0 (02/07/2025)

> [!IMPORTANT]
> **This is a Beta Version**

In this release, **Mobile Web Capture (MWC)** has been completely redesigned from the ground up. It is now a **ready-to-use, fully developed,** browser-based document management system for mobile devices.

### Highlighted Features

- Automatic document detection and image capture
- Scan and organize pages into a document
- Organize multiple documents in a library
- Full suite of PDF annotation and editing tools
- Upload documents to a server or save them locally
- Ready-to-use and flexible document scanning workflow
- Modular, view-based design for easy maintenance and customization

### Views

**MWC** features are organized into configurable UI views. Below is an overview of their main functionalities:

> [!TIP]
> Learn more in the [MWC user guide]({{ site.guides }}mobile-web-capture.html).

#### Library View
- Organize and manage multiple scanned documents
- Export, delete, share, or print documents
- View upload history for server-stored documents

#### Document View
- View page thumbnails
- Reorder or delete pages
- Export or share documents

#### Page View
- Close-up view of single pages
- Upload or save individual pages
- Full-featured annotations, including inking, shapes, highlighting, text boxes, signatures, and more

#### Transfer View
- Copy or move pages between documents

#### History View
- View upload history

> [!NOTE]
> The following three views are powered by **Dynamsoft Document Scanner (DDS)**. Learn more in the [DDS user guide]({{ site.guides }}document-scanner.html).

#### Document Scanner View
- Camera viewfinder with resolution toggle and more
- Automatic document detection and capture from the optimal frame in the video feed
- Manual capture for greater control

#### Document Correction View
- Fine-tune automatically detected document boundaries

#### Document Result View
- Preview the corrected scan
- Quickly re-scan and inspect documents until the desired result is achieved
- Share the scanned document or save it locally

## 2.0.0 (09/03/2024)

Includes

- [Dynamsoft Document Viewer Javascript Edition 2.0](https://www.dynamsoft.com/document-viewer/docs/releasenotes/index.html#20-20082024)
- [Dynamsoft Document Normalizer Javascript Edition 2.2.10](https://www.dynamsoft.com/document-normalizer/docs/web/programming/javascript/release-notes/javascript-2.html#2210-04092024)

## 1.1.0 (01/16/2024)

Includes

- [Dynamsoft Document Viewer Javascript Edition 1.1](https://www.dynamsoft.com/document-viewer/docs/releasenotes/index.html#11-01122024)
- [Dynamsoft Document Normalizer Javascript Edition 2.0.20](https://www.dynamsoft.com/document-normalizer/docs/web/programming/javascript/release-notes/javascript-2.html#2020-01112024)

## 1.0.0.0 (12/28/2023)

Includes

- [Dynamsoft Document Viewer Javascript Edition 1.0.0](https://www.dynamsoft.com/document-viewer/docs/releasenotes/index.html#100-12262023)
- [Dynamsoft Document Normalizer Javascript Edition 2.0.11](https://www.dynamsoft.com/document-normalizer/docs/web/programming/javascript/release-notes/javascript-2.html#2011-08242023)
