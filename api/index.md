---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: false
title: Mobile Document Scanner JS Edition - API Reference
keywords: Documentation, Mobile Document Scanner, Web, JS Edition, Dynamsoft Document Scanner, API, APIs
breadcrumbText: API References
description: Mobile Document Scanner JS Edition - API Reference
---

# Mobile Document Scanner API Reference

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

### `DocumentScanner`

#### Syntax
```typescript
new DocumentScanner(config: DocumentScannerConfig)
```

#### Parameters
- `config` ([`DocumentScannerConfig`](#documentscannerconfig)) : Configuration settings for the scanner, including license, container, view settings and more.

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

### `launch()`

Starts the **document scanning workflow**.

#### Syntax
```typescript
async launch(file?: File): Promise<DocumentResult>
```

#### Returns
- A `Promise` resolving to a [`DocumentResult`](#documentresult) object.

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

### `dispose()`

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

### `DocumentScannerConfig`

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

| Property                | Type                                                            | Description                                                                                                                                                                                                                         |
| ----------------------- | --------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `license`               | `string`                                                        | The license key for using the `DocumentScanner`.                                                                                                                                                                                    |
| `container`             | ``HTMLElement | string``                                        | The container element or selector for the `DocumentScanner` UI.                                                                                                                                                                     |
| `scannerViewConfig`     | [`DocumentScannerViewConfig`](#documentscannerconfig)           | Configuration settings for the scanner view.                                                                                                                                                                                        |
| `resultViewConfig`      | [`DocumentResultViewConfig`](#documentresultviewconfig)         | Configuration settings for the result view.                                                                                                                                                                                         |
| `correctionViewConfig`  | [`DocumentCorrectionViewConfig`](#documentcorrectionviewconfig) | Configuration settings for the correction view.                                                                                                                                                                                     |
| `templateFilePath`      | `string`                                                        | The file path to the document template used for scanning. You may set custom paths to self-host the template, or host MDS fully offline - see [self-hosting resources]({{ site.guide }}index.html#self-host-resources) for details. |
| `utilizedTemplateNames` | [`UtilizedTemplateNames`](#utilizedtemplatenames)               | Specifies detection and correction templates. You may set custom names to self-host resources, or host MDS fully offline - see [self-hosting resources]({{ site.guide }}index.html#self-host-resources) for details.                |
| `engineResourcePaths`   | [`EngineResourcePaths`](#engineresourcepaths)                   | Paths to the necessary resources for the scanning engine. You may set custom paths to self-host resources, or host MDS fully offline - see [self-hosting resources]({{ site.guide }}index.html#self-host-resources) for details.    |

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

### `DocumentScannerViewConfig`

Configures the scanner view for capturing documents.

#### Syntax
```typescript
interface DocumentScannerViewConfig {
  templateFilePath?: string;
  cameraEnhancerUIPath?: string;
  container?: HTMLElement | string;
  utilizedTemplateNames?: UtilizedTemplateNames;
  enableAutoCropMode?: boolean;
  enableSmartCaptureMode?: boolean;
  scanRegion: ScanRegion;
  minVerifiedFramesForAutoCapture: number;
  showSubfooter?: boolean;
  showPoweredByDynamsoft?: boolean;
}
```

#### Properties

| Property                          | Type                                                | Description                                                                                                                                                                                                                                         |
| --------------------------------- | --------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `templateFilePath`                | `string`                                            | Path to a Capture Vision template for scanning configuration.                                                                                                                                                                                       |
| `cameraEnhancerUIPath`            | `string`                                            | Path to the UI (`.html` template file) for the scanner view. You may set custom paths to self-host or customize the template, or host MDS fully offline - see [self-hosting resources]({{ site.guide }}index.html#self-host-resources) for details. |
| `container`                       | `HTMLElement`                                       | The container element for the scanner view.                                                                                                                                                                                                         |
| `utilizedTemplateNames`           | `[`UtilizedTemplateNames`](#utilizedtemplatenames)` | Capture Vision template names for detection and correction.                                                                                                                                                                                         |
| `enableAutoCropMode`              | `boolean`                                           | The default auto-crop mode state.                                                                                                                                                                                                                   |
| `enableSmartCaptureMode`          | `boolean`                                           | The default smart capture mode state.                                                                                                                                                                                                               |
| `scanRegion`                      | [`ScanRegion`](#scanregion)                         | Defines the region within the viewport to detect documents.                                                                                                                                                                                         |
| `minVerifiedFramesForAutoCapture` | `number`                                            | The minimum number of camera frames to detect document boundaries on Smart Capture mode.                                                                                                                                                            |
| `showSubfooter`                   | `boolean`                                           | Mode selector menu visibility.                                                                                                                                                                                                                      |
| `showPoweredByDynamsoft`          | `boolean`                                           | Visibility of the Dynamsoft branding message.                                                                                                                                                                                                       |

#### Example

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE", // Replace with your actual license key
    scannerViewConfig: {
        cameraEnhancerUIPath: "../dist/document-scanner.ui.html", // Use the local file
    },
});
```

### `DocumentCorrectionViewConfig`

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

| Property               | Type                                                                                        | Description                                               |
| ---------------------- | ------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| `container`            | `HTMLElement`                                                                               | The container element for the correction view.            |
| `toolbarButtonsConfig` | [`DocumentCorrectionViewToolbarButtonsConfig`](#documentcorrectionviewtoolbarbuttonsconfig) | Configuration for toolbar buttons in the correction view. |
| `onFinish`             | `(result: DocumentResult) => void`                                                          | Callback function triggered when correction is finished.  |

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

### `DocumentResultViewConfig`

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

| Property               | Type                                                                                | Description                                                 |
| ---------------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `container`            | `HTMLElement`                                                                       | The container element for the result view.                  |
| `toolbarButtonsConfig` | [`DocumentResultViewToolbarButtonsConfig`](#documentresultviewtoolbarbuttonsconfig) | Configuration for toolbar buttons in the result view.       |
| `onDone`               | `(result: DocumentResult) => Promise<void>`                                         | Callback function triggered when scanning is done.          |
| `onUpload`             | `(result: DocumentResult) => Promise<void>`                                         | Callback function triggered when uploading the scan result. |

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

### `DocumentResult`

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

| Property                | Type                                        | Description                                                  |
| ----------------------- | ------------------------------------------- | ------------------------------------------------------------ |
| `status`                | `ResultStatus`                              | The status of the document scan (success, failed, canceled). |
| `originalImageResult`   | `OriginalImageResultItem["imageData"]`      | The original captured image before correction.               |
| `correctedImageResult`  | ``NormalizedImageResultItem | DSImageData`` | The processed (corrected) image.                             |
| `detectedQuadrilateral` | `Quadrilateral`                             | The detected document boundaries.                            |

### `ScanRegion`

Describes the scan region within the viewfinder for document scanning:

1. Use the `ratio` property to set the height-to-width of the rectangular scanning region, then scale the rectangle up to fit within the viewfinder.
2. Translate the rectangular up by the number of pixels specified by `regionBottomMargin`.
3. Create a visual border for the scanning region boundary on the viewfinder with a given stroke width in pixels, and a stroke color.

#### Syntax

```typescript
interface ScanRegion {
  ratio: {
    width: number;
    height: number;
  };
  regionBottomMargin: number; // Bottom margin calculated in pixel
  style: {
    strokeWidth: number;
    strokeColor: string;
  };
}
```

#### Properties

| Property             | Type     | Description                                                |
| -------------------- | -------- | ---------------------------------------------------------- |
| `ratio`              | `object` | The aspect ratio of the rectangular scan region.           |
| »`height`            | `number` | The height of the rectangular scan region.                 |
| »`width`             | `number` | The width of the rectangular scan region.                  |
| `regionBottomMargin` | `number` | Bottom margin below the scan region measured in pixels.    |
| »`strokeWidth`       | `number` | The pixel width of the outline of the scan region.         |
| »`strokeColor`       | `string` | The color of the outline of the scan region.               |
| `style`              | `object` | The styling for the scan region outline in the viewfinder. |

#### Example

Create a scan region with a height-to-width ratio of 3:2, translated upwards by 20 pixels, with a green, 3 pixel-wide border in the viewfinder:

```javascript
scanRegion {
  ratio: {
    width: 2,
    height: 3,
  },
  regionBottomMargin: 20,
  style: {
    strokeWidth: 3,
    strokeColor: "green",
  },
}
```

## Toolbar Button Configurations

### `ToolbarButtonConfig`

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

##### Syntax

```typescript
interface DocumentCorrectionViewToolbarButtonsConfig {
  fullImage?: ToolbarButtonConfig;
  detectBorders?: ToolbarButtonConfig;
  apply?: ToolbarButtonConfig;
}
```

#### DocumentResultViewToolbarButtonsConfig

##### Syntax

```typescript
interface DocumentResultViewToolbarButtonsConfig {
  retake?: ToolbarButtonConfig;
  correct?: ToolbarButtonConfig;
  share?: ToolbarButtonConfig;
  upload?: ToolbarButtonConfig;
  done?: ToolbarButtonConfig;
}
```

## Assisting Interfaces

### `UtilizedTemplateNames`

[Dynamsoft Capture Vision template](https://www.dynamsoft.com/capture-vision/docs/core/parameters/file/capture-vision-template.html?lang=javascript) names for detection and correction. This typically does not need to be set, as DDS uses the default template.

#### Syntax

```typescript
interface UtilizedTemplateNames {
  detect: string;
  normalize: string;
}
```

### `EngineResourcePaths`

Paths to extra resources such as `.wasm` engine files. The default paths point to CDNs and so may be left unset. You may set custom paths to self-host resources, or host MDS fully offline - see [self-hosting resources]({{ site.guide }}index.html#self-host-resources) for details.

#### Syntax

```typescript
interface EngineResourcePaths {
    "rootDirectory"?: string;
    "std"?: string | PathInfo;
    "dip"?: string | PathInfo;
    "dnn"?: string | PathInfo;
    "core"?: string | PathInfo;
    "license"?: string | PathInfo;
    "cvr"?: string | PathInfo;
    "utility"?: string | PathInfo;
    "dbr"?: string | PathInfo;
    "dlr"?: string | PathInfo;
    "ddn"?: string | PathInfo;
    "dcp"?: string | PathInfo;
    "dce"?: string | PathInfo;
    "dlrData"?: string | PathInfo;
    "ddv"?: string | PathInfo;
    "dwt"?: string | DwtInfo;
}
```
