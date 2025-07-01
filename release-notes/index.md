---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
title: Document Scanner - Release Notes
keywords: Documentation, Mobile Document Scanner, Web, JS Edition, Document Scanner, DDS, MWC, Release Notes
breadcrumbText: Release Notes
description: Mobile Web Capture Documentation Release Notes
permalink: /release-notes/document-scanner.html
---

# Document Scanner Release Notes

## 1.2.0 (30/04/2025)

# Release Notes

### Features

- `DocumentScanner` configuration options
  - `ScannerViewConfig`
    -   `enableAutoCropMode?: boolean; // False by default`
    -  `enableSmartCaptureMode?: boolean; // False by default`
    - `showSubfooter`: Toggle showing the sub-footer container that allows users to toggle scan modes (Detect border, Smart capture, Auto crop). `true` by default
    - `showPoweredByDynamsoft`: Toggle showing `Powered by Dynamsoft` message on the scanner view. `true` by default
    - `minVerifiedFramesForAutoCapture`: Change the minimum verified frames to auto capture the document. `2` frames are needed by default. Lower this number to make the capture faster (this could have an effect on accuracy/quality of image scanned).
    - `scanRegion`: allows users to set a scan region while scanning a document

```
export interface ScanRegion {
  ratio: {
    width: number;
    height: number;
  }; // Ratio of the scan region
  regionBottomMargin: number; // Bottom margin calculated in pixel. This will "push" the scan region upwards
  style: {
    strokeWidth: number; // width of the scan region border
    strokeColor: string; // color of the scan region border
  };
}
```
  - Added `Re-take` button in Correction View. this will allow users to retake/rescan the document through the correction view.
  - Provide landscape support for the Document Scanner View (implemented in`document-scanner.ui.html`)
  - Template optimization: Updated `scaleDownThreshold` to `1000`
  - Allow `launch()` with a static image. A sample is provided under `sample/scenarios/use-file-input.html`
  - Set the default resolution when opening camera to `2K` resolution

### Fixes

- Enable `OutputOriginalImage` on the template by default. Before, it required us to enable it manually if we use a custom template.
- Set `engineResourcePaths` before `initLicense` to prevent a bug when a user implements a custom engineResourcePath.
- Update trial license banner link to lead to `https://www.dynamsoft.com/customer/license/trialLicense?product=mwc&deploymenttype=web`

### Dependencies

- [Dynamsoft Capture Vision JavaScript 2.6.1000](https://www.dynamsoft.com/capture-vision/docs/web/programming/javascript/release-notes/dcvb.html#261000-01032025)
  - Dynamsoft Camera Enhancer 4.1.1
  - Dynamsoft Document Normalizer 2.6.11


## 1.1.1 (07/02/2025)

### Fixes

- Fixed icon not showing up on Firefox mobile

### Dependencies

- [Dynamsoft Capture Vision JavaScript 2.6.1000](https://www.dynamsoft.com/capture-vision/docs/web/programming/javascript/release-notes/dcvb.html#261000-01032025)
  - Dynamsoft Camera Enhancer 4.1.1
  - Dynamsoft Document Normalizer 2.6.11

## 1.1.0 (07/02/2025)

### Features

- `DocumentScanner` configuration options
  - View Control
    - `showCorrectionView`: Toggle correction view visibility and workflow
    - `showResultView`: Togle result view visibility and workflow
    - UI Changes
      - Hidden `DocumentCorrectionView` -> `Smart Capture` button hidden on `DocumentScannerView`
      - Hidden `DocumentResultView` -> `Apply` changes to `Done` in `DocumentCorrectionView`
  - Resource Configuration
    - `engineResourcePaths`: to configure DCV engine resources
    - `templateFilePath`: set template file location
- Added `EnumDDSViews` enum (Scanner/Result/Correction)

### Fixes

- Fixed button state issues with hidden views
- Fixed `container` type flexibility in `DocumentScannerViewConfig`

### Dependencies

- [Dynamsoft Capture Vision JavaScript 2.6.1000](https://www.dynamsoft.com/capture-vision/docs/web/programming/javascript/release-notes/dcvb.html#261000-01032025)
  - Dynamsoft Camera Enhancer 4.1.1
  - Dynamsoft Document Normalizer 2.6.11

## 1.0.3 (07/02/2025)

### Fixes

- Fixed missing devDependency in `package.json`.

### Dependencies

- [Dynamsoft Capture Vision JavaScript 2.6.1000](https://www.dynamsoft.com/capture-vision/docs/web/programming/javascript/release-notes/dcvb.html#261000-01032025)
  - Dynamsoft Camera Enhancer 4.1.1
  - Dynamsoft Document Normalizer 2.6.11

## 1.0.2 (05/02/2025)

### BREAKING CHANGES

- Renamed `ScanResultView` to `DocumentResultView`.
- Renamed `DocumentScanResult` to `DocumentResult`.
- Renamed `scanResultViewConfig` to `resultViewConfig`.
- Renamed `ControlButton` to `ToolbarConfig`.
- Changed `text` property for toolbar buttons to `label`.

### New Features

- Added support for `<img>` tags on toolbar icons.
- Updated the **Share PNG** icon to match other products.
- Added an **upload button** when users specify the `onUpload` function.

### Fixes

- Fixed an issue where importing an image larger than the canvas dimensions caused the detected border to be positioned incorrectly. This was due to **scaleDown** not working properly.

### Samples

#### Features

- Added a **rotate message** on `/demo.html` to handle landscape mode.

#### Fixes

- Fixed an issue where the **camera and resolution outline logic** would break when transitioning from the **Demo Camera** to the **Live Camera**.

### Built-in Server

- Changed the configuration to allow showcasing the **"Upload"** feature.
- Added URLs to specific pages when running `npm run serve`.

### Dependencies

- [Dynamsoft Capture Vision JavaScript 2.6.1000](https://www.dynamsoft.com/capture-vision/docs/web/programming/javascript/release-notes/dcvb.html#261000-01032025)
  - Dynamsoft Camera Enhancer 4.1.1
  - Dynamsoft Document Normalizer 2.6.11

## 1.0.0 (27/01/2025)

Initial release

### Dependencies

- [Dynamsoft Capture Vision JavaScript 2.6.1000](https://www.dynamsoft.com/capture-vision/docs/web/programming/javascript/release-notes/dcvb.html#261000-01032025)
  - Dynamsoft Camera Enhancer 4.1.1
  - Dynamsoft Document Normalizer 2.6.11
