---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: false
title: Dynamsoft Document Scanner - API Reference
keywords: Documentation, Mobile Web Capture, Dynamsoft Document Scanner, API, APIs
breadcrumbText: API References
description: Dynamsoft Document Scanner - API Reference
---

# Dynamsoft Document Scanner API Reference

The `DocumentScanner` class handles the document scanning process, including image capture, correction, and result display.

<!--  Keep TOC only for npm /github as readme
- [Constructor](#constructor)
  - [DocumentScanner](#documentscanner)
- [Methods](#methods)
  - [launch()](#launch)
  - [dispose()](#dispose)
- [Configuration Interfaces](#configuration-interfaces)
  - [DocumentScannerConfig](#documentscannerconfig)
  - [DocumentScannerViewConfig](#documentscannerviewconfig)
  - [DocumentCorrectionViewConfig](#documentcorrectionviewconfig)
  - [DocumentResultViewConfig](#documentresultviewconfig)
  - [DocumentResult](#documentresult)
- [Toolbar Button Configurations](#toolbar-button-configurations)
  - [ToolbarButtonConfig](#toolbarbuttonconfig)
  - [Configurable Buttons Per Each View](#configurable-buttons-per-each-view)
-->

## Constructor
### DocumentScanner
#### Syntax
```typescript
new DocumentScanner(config: DocumentScannerConfig)
```

#### Parameters
- `config` *([DocumentScannerConfig](#documentscannerconfig))* : Configuration settings for the scanner, including license, container, view settings and more.

#### Example

```html
<div id="myDocumentScannerContainer" style="width: 80vw; height: 80vh;"></div>
```

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    scannerViewConfig: {
        container: document.getElementById("myDocumentScannerViewContainer") // Use this container for the scanner view
    }
});
```

## Methods

### launch()

Starts the **document scanning workflow**.

#### Syntax
```typescript
launch(): Promise<DocumentResult>
```

#### Returns
- A `Promise` resolving to a `DocumentResult` object.

#### Example
```typescript
const result = await documentScanner.launch();

if (result?.correctedImageResult) {
    resultContainer.innerHTML = "";
    const canvas = result.correctedImageResult.toCanvas();
    resultContainer.appendChild(canvas);
} else {
    resultContainer.innerHTML = "<p>No image scanned. Please try again.</p>";
}
```

### dispose()

Cleans up resources and hides UI components.

#### Syntax
```typescript
dispose(): void
```

#### Example
```typescript
documentScanner.dispose();
console.log("Scanner resources released.");
```

## Configuration Interfaces
### DocumentScannerConfig

#### Syntax
```typescript
interface DocumentScannerConfig {
  license?: string;
  container?: HTMLElement | string;
  scannerViewConfig?: DocumentScannerViewConfig;
  resultViewConfig?: DocumentResultViewConfig;
  correctionViewConfig?: DocumentCorrectionViewConfig;
  templateFilePath?: string;
  utilizedTemplateNames?: UtilizedTemplateNames;
  engineResourcePaths: EngineResourcePaths;
}
```

#### Properties

| Property                | Type                           | Description                                                     |
| ----------------------- | ------------------------------ | --------------------------------------------------------------- |
| `license`               | `string`                       | The license key for using the `DocumentScanner`.                |
| `container`             | `HTMLElement \| string`        | The container element or selector for the `DocumentScanner` UI. |
| `scannerViewConfig`     | `DocumentScannerViewConfig`    | Configuration settings for the scanner view.                    |
| `resultViewConfig`      | `DocumentResultViewConfig`     | Configuration settings for the result view.                     |
| `correctionViewConfig`  | `DocumentCorrectionViewConfig` | Configuration settings for the correction view.                 |
| `templateFilePath`      | `string`                       | The file path to the document template used for scanning.       |
| `utilizedTemplateNames` | `UtilizedTemplateNames`        | Specifies detection and correction templates.                   |
| `engineResourcePaths`   | `EngineResourcePaths`          | Paths to the necessary resources for the scanning engine.       |

#### Example
```typescript
const config = {
    license: "YOUR_LICENSE_KEY_HERE",
    scannerViewConfig: {
        cameraEnhancerUIPath: "./dist/document-scanner.ui.html", // Use the local file
    },
    engineResourcePaths: {
        std: "./dist/libs/dynamsoft-capture-vision-std/dist/",
        dip: "./dist/libs/dynamsoft-image-processing/dist/",
        core: "./dist/libs/dynamsoft-core/dist/",
        license: "./dist/libs/dynamsoft-license/dist/",
        cvr: "./dist/libs/dynamsoft-capture-vision-router/dist/",
        ddn: "./dist/libs/dynamsoft-document-normalizer/dist/",
    },
};
```

### DocumentScannerViewConfig

Configures the scanner view for capturing documents.

#### Syntax
```typescript
interface DocumentScannerViewConfig {
  cameraEnhancerUIPath?: string;
  container?: HTMLElement;
}
```

#### Properties

| Property               | Type          | Description                                                  |
| ---------------------- | ------------- | ------------------------------------------------------------ |
| `cameraEnhancerUIPath` | `string`      | Path to the UI (`.html` template file) for the scanner view. |
| `container`            | `HTMLElement` | The container element for the scanner view.                  |

#### Example

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE", // Replace with your actual license key
    scannerViewConfig: {
        cameraEnhancerUIPath: "../dist/document-scanner.ui.html", // Use the local file
    },
});
```

### DocumentCorrectionViewConfig

Configures the correction view for adjusting scanned documents, including toolbar buttons and event handlers for completion.

#### Syntax
```typescript
interface DocumentCorrectionViewConfig {
  container?: HTMLElement;
  toolbarButtonsConfig?: DocumentCorrectionViewToolbarButtonsConfig;
  onFinish?: (result: DocumentResult) => void;
}
```

#### Properties

| Property               | Type                                         | Description                                               |
| ---------------------- | -------------------------------------------- | --------------------------------------------------------- |
| `container`            | `HTMLElement`                                | The container element for the correction view.            |
| `toolbarButtonsConfig` | `DocumentCorrectionViewToolbarButtonsConfig` | Configuration for toolbar buttons in the correction view. |
| `onFinish`             | `(result: DocumentResult) => void`           | Callback function triggered when correction is finished.  |

#### Example

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    correctionViewConfig: {
        onFinish: (result) => {
            const canvas = result.correctedImageResult.toCanvas();
            resultContainer.appendChild(canvas);
        }
    }
});
```

### DocumentResultViewConfig

Configures the result view for reviewing scanned documents, including toolbar buttons and event handlers for uploads and completion.

#### Syntax
```typescript
interface DocumentResultViewConfig {
  container?: HTMLElement;
  toolbarButtonsConfig?: DocumentResultViewToolbarButtonsConfig;
  onDone?: (result: DocumentResult) => Promise<void>;
  onUpload?: (result: DocumentResult) => Promise<void>;
}
```

#### Properties

| Property               | Type                                        | Description                                                 |
| ---------------------- | ------------------------------------------- | ----------------------------------------------------------- |
| `container`            | `HTMLElement`                               | The container element for the result view.                  |
| `toolbarButtonsConfig` | `DocumentResultViewToolbarButtonsConfig`    | Configuration for toolbar buttons in the result view.       |
| `onDone`               | `(result: DocumentResult) => Promise<void>` | Callback function triggered when scanning is done.          |
| `onUpload`             | `(result: DocumentResult) => Promise<void>` | Callback function triggered when uploading the scan result. |

#### Example
```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    resultViewConfig: {
        onDone: async (result) => 
        {
            const canvas = result.correctedImageResult.toCanvas();
            resultContainer.appendChild(canvas);
        }
    }
});
```

### DocumentResult

Represents the output of a scan, including the original and corrected images, detected boundaries, and scan status.

#### Syntax
```typescript
interface DocumentResult {
  status: ResultStatus;
  correctedImageResult?: NormalizedImageResultItem | DSImageData;
  originalImageResult?: OriginalImageResultItem["imageData"];
  detectedQuadrilateral?: Quadrilateral;
}
```

#### Properties

| Property                | Type                                       | Description                                                  |
| ----------------------- | ------------------------------------------ | ------------------------------------------------------------ |
| `status`                | `ResultStatus`                             | The status of the document scan (success, failed, canceled). |
| `originalImageResult`   | `OriginalImageResultItem["imageData"]`     | The original captured image before correction.               |
| `correctedImageResult`  | `NormalizedImageResultItem \| DSImageData` | The processed (corrected) image.                             |
| `detectedQuadrilateral` | `Quadrilateral`                            | The detected document boundaries.                            |

## Toolbar Button Configurations

### ToolbarButtonConfig

A simplified configuration type for toolbar buttons.

#### Syntax
```typescript
export type ToolbarButtonConfig = Pick<"icon" | "label" | "isHidden">;
```

#### Properties  

| Property   | Type                 | Description                         |
| ---------- | -------------------- | ----------------------------------- |
| `icon`     | `string`             | The icon displayed on the button.   |
| `label`    | `string`             | The text label for the button.      |
| `isHidden` | `boolean` (optional) | Determines if the button is hidden. |

#### Example
```typescript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    correctionViewConfig: {
        toolbarButtonsConfig: {
            fullImage: {
                isHidden: true
            },
            detectBorders: {
                icon: "path/to/new_icon.png", // Change to the actual path of the new icon
                label: "Custom Label"
            }
        }
    }
});
```

### Configurable Buttons Per Each View

#### DocumentCorrectionViewToolbarButtonsConfig
```typescript
interface DocumentCorrectionViewToolbarButtonsConfig {
  fullImage?: ToolbarButtonConfig;
  detectBorders?: ToolbarButtonConfig;
  apply?: ToolbarButtonConfig;
}
```

#### DocumentCorrectionViewToolbarButtonsConfig

```typescript
interface DocumentCorrectionViewToolbarButtonsConfig {
  fullImage?: ToolbarButtonConfig;
  detectBorders?: ToolbarButtonConfig;
  apply?: ToolbarButtonConfig;
}
```

#### DocumentResultViewToolbarButtonsConfig

```typescript
interface DocumentResultViewToolbarButtonsConfig {
  retake?: ToolbarButtonConfig;
  correct?: ToolbarButtonConfig;
  share?: ToolbarButtonConfig;
  upload?: ToolbarButtonConfig;
  done?: ToolbarButtonConfig;
}
```

