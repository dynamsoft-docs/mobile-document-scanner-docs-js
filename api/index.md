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

The `DocumentScanner` class provides a complete document scanning solution that integrates camera access, real-time document boundary detection, manual boundary adjustment, and image perspective correction.

<!--  Keep TOC only for npm /github as readme
- [Constructor](#constructor)
  - [DocumentScanner](#documentscanner)
- [Methods](#methods)
  - [launch()](#launch)
  - [initialize()](#initialize)
  - [stopContinuousScanning()](#stopcontinuousscanning)
  - [dispose()](#dispose)
- [Configuration Interfaces](#configuration-interfaces)
  - [DocumentScannerConfig](#documentscannerconfig)
  - [DocumentScannerViewConfig](#documentscannerviewconfig)
  - [DocumentCorrectionViewConfig](#documentcorrectionviewconfig)
  - [DocumentResultViewConfig](#documentresultviewconfig)
  - [DocumentResult](#documentresult)
- [Toolbar Button Configurations](#toolbar-button-configurations)
  - [ToolbarButtonConfig](#toolbarbuttonconfig)
  - [ToolbarButton](#toolbarbutton)
  - [Configurable Buttons Per Each View](#configurable-buttons-per-each-view)
-->

## Constructor

### `DocumentScanner`

Create a DocumentScanner instance with settings specified by a `DocumentScannerConfig` object.

The `DocumentScanner` class orchestrates three main views:
- `DocumentScannerView`: Camera interface with document detection and capture modes
- `DocumentCorrectionView`: Manual boundary adjustment interface
- `DocumentResultView`: Result preview and action interface

The class supports both single-scan and continuous scanning modes. In continuous mode, the scanner loops back after each successful scan, allowing multiple documents to be captured in sequence.

#### Syntax
```typescript
new DocumentScanner(config: DocumentScannerConfig)
```

#### Parameters
- `config` ([`DocumentScannerConfig`](#documentscannerconfig)) : Configuration settings for the scanner. You must set a valid license key with the `license` property. See [`DocumentScannerConfig`](#documentscannerconfig) for a complete description.

#### Example

HTML:
```html
<div id="myDocumentScannerContainer" style="width: 80vw; height: 80vh;"></div>
```

JavaScript:
```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    scannerViewConfig: {
        container: document.getElementById("myDocumentScannerContainer") // Use this container for the scanner view
    }
});
```

#### Example: Continuous Scanning Mode
```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE",
    enableContinuousScanning: true,
    onDocumentScanned: async (result) => {
        // Process each scanned document
        await uploadToServer(result.correctedImageResult);
    }
});

await documentScanner.launch();
```

## Methods

### `launch()`

Start the document scanning workflow.

This is the primary method for initiating document scanning. It performs the following:
1. Automatically calls `initialize()` if not already initialized
2. Opens the camera and displays the `DocumentScannerView` (unless a file is provided)
3. Guides the user through the configured workflow (scan → correction → result)
4. Returns the final `DocumentResult` when the workflow completes
5. Automatically calls `dispose()` to clean up resources

#### Scanning Modes
- **Single-scan mode (default)**: Captures one document and returns the result
- **Continuous scanning mode** (`enableContinuousScanning`): Loops after each scan, invoking `onDocumentScanned` with each result. The loop continues until the user clicks the close button (X) or `stopContinuousScanning()` is called. Returns the last scanned result.

#### File Processing
Passing a `File` object allows processing an existing image file, bypassing camera input and the `DocumentScannerView`.

#### Syntax
```typescript
async launch(file?: File): Promise<DocumentResult>
```

#### Parameters
- `file` (optional): Image file to process instead of using the camera

#### Returns
- A `Promise` resolving to a [`DocumentResult`](#documentresult) object, which includes:
  - `status`: Scan status (success, cancelled, or failed)
  - `correctedImageResult`: Perspective-corrected document image
  - `originalImageResult`: Original captured image
  - `detectedQuadrilateral`: Detected document boundaries

#### Throws
- `Error` if a capture session is already in progress

#### Example: Basic Single-Scan Usage
```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE"
});

const result = await documentScanner.launch();

if (result?.correctedImageResult) {
    resultContainer.innerHTML = "";
    const canvas = result.correctedImageResult.toCanvas();
    resultContainer.appendChild(canvas);
} else {
    resultContainer.innerHTML = "<p>No image scanned. Please try again.</p>";
}
```

#### Example: Process an Existing Image File
```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE"
});

const fileInput = document.querySelector('input[type="file"]');
const file = fileInput.files[0];
const result = await documentScanner.launch(file);
```

#### Example: Continuous Scanning Mode
```javascript
const scannedDocs = [];
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE",
    enableContinuousScanning: true,
    onDocumentScanned: async (result) => {
        scannedDocs.push(result);
        console.log(`Scanned ${scannedDocs.length} documents`);
    }
});

// This will return the last scanned result when user exits
const lastResult = await documentScanner.launch();
```

### `initialize()`

> [!NOTE]
> This method is called automatically by `launch()` and typically does not need to be invoked manually.

Initialize the DocumentScanner by setting up Dynamsoft Capture Vision resources and view components.

This method performs the following initialization steps:
1. Validates and processes the configuration provided to the constructor
2. Initializes Dynamsoft Capture Vision engine resources (license, camera, router)
3. Creates and initializes the configured view components (scanner, correction, result)
4. Sets up shared resources and callbacks for communication between views

The method is idempotent - calling it multiple times will return the same resources and components without re-initialization.

#### Syntax
```typescript
async initialize(): Promise<{
  resources: SharedResources;
  components: {
    scannerView?: DocumentScannerView;
    correctionView?: DocumentCorrectionView;
    scanResultView?: DocumentResultView;
  };
}>
```

#### Returns
- A promise that resolves to an object containing:
  - `resources`: The `SharedResources` object containing camera, router, and state
  - `components`: An object with references to the initialized view components (`scannerView`, `correctionView`, `scanResultView`)

#### Throws
- `Error` if initialization fails due to invalid configuration, missing license, or resource loading errors

#### Example: Manual Initialization (rarely needed)
```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE"
});

try {
    const { resources, components } = await documentScanner.initialize();
    console.log("Scanner initialized successfully");
} catch (error) {
    console.error("Initialization failed:", error);
}
```

### `stopContinuousScanning()`

Stop continuous scanning and exit the scanning loop.

When called with `enableContinuousScanning` enabled and `launch()` running, signal the scanner to stop looping and return from `launch()` with the last scanned result.

This provides an alternative to using the close button (X) for exiting continuous scanning mode, allowing you to implement custom exit logic based on conditions such as:
- Maximum number of scanned documents reached
- Time limits
- User interaction with custom UI elements
- External events or triggers

#### Syntax
```typescript
stopContinuousScanning(): void
```

#### Example: Stop After Scanning 5 Documents
```javascript
let scannedCount = 0;
const scanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE",
    enableContinuousScanning: true,
    onDocumentScanned: async (result) => {
        scannedCount++;
        console.log(`Scanned document ${scannedCount}`);
        
        if (scannedCount >= 5) {
            scanner.stopContinuousScanning();
        }
    }
});

await scanner.launch(); // Exits after 5 scans
```

#### Example: Stop from External Button
```javascript
const scanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE",
    enableContinuousScanning: true,
    onDocumentScanned: async (result) => {
        // Process each scanned document
        saveDocument(result);
    }
});

// Bind to custom stop button
document.getElementById('stopBtn').addEventListener('click', () => {
    scanner.stopContinuousScanning();
});

await scanner.launch(); // Will exit when stopBtn is clicked
```

### `dispose()`

> [!NOTE]
> This method is called automatically at the end of `launch()`, so manual invocation is typically only needed if you want to clean up resources before the scanning workflow completes.

Clean up and release all resources used by the DocumentScanner.

This method performs comprehensive cleanup by:
- Disposing all view components (scanner, correction, result)
- Releasing Dynamsoft Capture Vision resources (camera, router)
- Clearing all container elements
- Resetting internal state

After calling dispose, you can create a new DocumentScanner instance if you need to scan again.

#### Syntax
```typescript
dispose(): void
```

#### Example: Manual Cleanup
```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE"
});

await documentScanner.launch();

// Clean up is automatic after launch completes
// But you can also call it manually if needed:
documentScanner.dispose();
console.log("Scanner resources released");
```

## Configuration Interfaces

### `DocumentScannerConfig`

The `DocumentScannerConfig` interface passes settings to the `DocumentScanner` constructor to apply a comprehensive set of UI and business logic customizations.

Only advanced scenarios require editing the UI template or MDS source code. `license` is the only property required to instantiate a `DocumentScanner` object. MDS uses sensible default values for all other omitted properties.

#### Syntax
```typescript
interface DocumentScannerConfig {
  license?: string;
  container?: HTMLElement | string;
  templateFilePath?: string;
  utilizedTemplateNames?: UtilizedTemplateNames;
  engineResourcePaths?: EngineResourcePaths;
  scannerViewConfig?: DocumentScannerViewConfig;
  resultViewConfig?: DocumentResultViewConfig;
  correctionViewConfig?: DocumentCorrectionViewConfig;
  showResultView?: boolean;
  showCorrectionView?: boolean;
  enableContinuousScanning?: boolean;
  enableFrameVerification?: boolean;
  onDocumentScanned?: (result: DocumentResult) => void | Promise<void>;
  onThumbnailClicked?: (result: DocumentResult) => void | Promise<void>;
}
```

#### Properties

| Property                   | Type                                                            | Default | Description                                                                                                                                                                                                                                                                                                                                                                     |
| -------------------------- | --------------------------------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `license`                  | `string`                                                        | -       | **Required.** The license key for using the `DocumentScanner`. This is the only required property to instantiate a `DocumentScanner` object.                                                                                                                                                                                                                                    |
| `container`                | ``HTMLElement \| string``                                       | -       | The container element or selector for the `DocumentScanner` UI.                                                                                                                                                                                                                                                                                                                 |
| `templateFilePath`         | `string`                                                        | -       | The file path to the Capture Vision template used for document scanning. You may set custom paths to self-host the template or fully self-host MDS - see [self-hosting resources]({{ site.guide }}index.html#self-host-resources) for details.                                                                                                                                 |
| `utilizedTemplateNames`    | [`UtilizedTemplateNames`](#utilizedtemplatenames)               | See [UtilizedTemplateNames](#utilizedtemplatenames)       | Capture Vision template names for document detection and normalization. This typically does not need to be set as MDS provides a default template for general use. You may set custom names to self-host resources or fully self-host MDS - see [self-hosting resources]({{ site.guide }}index.html#self-host-resources) for details.                                          |
| `engineResourcePaths`      | [`EngineResourcePaths`](#engineresourcepaths)                   | CDN URLs       | Paths to the necessary engine resources (such as `.wasm` files) for the scanning engine. The default paths point to CDNs so this may be left unset. You may set custom paths to self-host resources or fully self-host MDS - see [self-hosting resources]({{ site.guide }}index.html#self-host-resources) for details.                                                          |
| `scannerViewConfig`        | [`DocumentScannerViewConfig`](#documentscannerviewconfig)       | -       | Configuration settings for the `DocumentScannerView`. See [workflow customization]({{ site.guide }}index.html#workflow-customization) for details.                                                                                                                                                                                                                              |
| `resultViewConfig`         | [`DocumentResultViewConfig`](#documentresultviewconfig)         | -       | Configuration settings for the `DocumentResultView`. See [workflow customization]({{ site.guide }}index.html#workflow-customization) for details.                                                                                                                                                                                                                               |
| `correctionViewConfig`     | [`DocumentCorrectionViewConfig`](#documentcorrectionviewconfig) | -       | Configuration settings for the `DocumentCorrectionView`.                                                                                                                                                                                                                                                                                                                        |
| `showResultView`           | `boolean`                                                       | `true`  | Sets the visibility of the `DocumentResultView`.                                                                                                                                                                                                                                                                                                                                |
| `showCorrectionView`       | `boolean`                                                       | `true`  | Sets the visibility of the `DocumentCorrectionView`.                                                                                                                                                                                                                                                                                                                            |
| `enableContinuousScanning` | `boolean`                                                       | `false` | Enable continuous scanning mode where the scanner loops back after each successful scan instead of exiting. `launch()` only resolves to the last scanned result. Use `onDocumentScanned` callback to get scan results. When enabled, the scanner automatically loops back to capture another document after each successful scan, the `onDocumentScanned` callback triggers after each scan with the result, users can exit by clicking the close button (X) or by calling `stopContinuousScanning()`, and the DocumentScanner only retains the most recent scan result. |
| `enableFrameVerification`  | `boolean`                                                       | `true`  | Enable automatic frame verification for best quality capture. When enabled, uses clarity detection and cross-filtering to automatically find the clearest frame.                                                                                                                                                                                                                |
| `onDocumentScanned`        | `(result: DocumentResult) => void \| Promise<void>`             | -       | Callback invoked after each successful scan in continuous scanning mode. This callback is only called when `enableContinuousScanning` is true. The scanner loops back to capture another document after this callback completes. The callback receives a `DocumentResult` containing the original image, corrected image, detected boundaries, and scan status.                  |
| `onThumbnailClicked`       | `(result: DocumentResult) => void \| Promise<void>`             | -       | Callback invoked when the thumbnail preview is clicked in continuous scanning mode. This callback is only invoked when `enableContinuousScanning` is enabled, `showCorrectionView` is disabled, and `showResultView` is disabled. The thumbnail preview displays the most recently scanned document. By default, clicking it does nothing unless this callback is defined, allowing you to implement custom behavior such as re-editing the image. |

#### Example: Basic Configuration
```javascript
const config = {
    license: "YOUR_LICENSE_KEY_HERE",
    scannerViewConfig: {
        cameraEnhancerUIPath: "./dist/document-scanner.ui.xml", // Use the local file
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

#### Example: Continuous Scanning with Callback
```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE",
    enableContinuousScanning: true,
    onDocumentScanned: async (result) => {
        // Process each scanned document
        const canvas = result.correctedImageResult.toCanvas();
        document.getElementById("results").appendChild(canvas);
    }
});
```

#### Example: Thumbnail Click Handler
```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE",
    enableContinuousScanning: true,
    showCorrectionView: false,
    showResultView: false,
    onThumbnailClicked: async (result) => {
        // Handle thumbnail click event
        console.log('Thumbnail clicked', result);
        // Could open a custom editor, display metadata, etc.
    }
});
```

### `DocumentScannerViewConfig`

The `DocumentScannerViewConfig` interface passes settings to the `DocumentScanner` constructor through the `DocumentScannerConfig` to apply UI and business logic customizations for the `DocumentScannerView`.

Simple scenarios do not require editing the UI template or MDS source code. MDS uses sensible default values for all omitted properties.

#### Syntax
```typescript
interface DocumentScannerViewConfig {
  container?: HTMLElement | string;
  cameraEnhancerUIPath?: string;
  templateFilePath?: string;
  utilizedTemplateNames?: UtilizedTemplateNames;
  enableBoundsDetectionMode?: boolean;
  enableSmartCaptureMode?: boolean;
  enableAutoCropMode?: boolean;
  enableFrameVerification?: boolean;
  scanRegion?: ScanRegion;
  minVerifiedFramesForAutoCapture?: number;
  showSubfooter?: boolean;
  showPoweredByDynamsoft?: boolean;
}
```

#### Properties

| Property                          | Type                                              | Default | Description                                                                                                                                                                                                                                                                      |
| --------------------------------- | ------------------------------------------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `container`                       | ``HTMLElement \| string``                         | -       | The HTML container element or selector for the `DocumentScannerView` UI.                                                                                                                                                                                                         |
| `cameraEnhancerUIPath`            | `string`                                          | CDN URL | Path to the UI definition file (`.xml`) for the `DocumentScannerView`. This typically does not need to be set as MDS provides a default template for general use. You may set custom paths to self-host or customize the template, or fully self-host MDS - see [self-hosting resources]({{ site.guide }}index.html#self-host-resources) for details. |
| `templateFilePath`                | `string`                                          | -       | Path to the Capture Vision template file for scanning configuration.                                                                                                                                                                                                             |
| `utilizedTemplateNames`           | [`UtilizedTemplateNames`](#utilizedtemplatenames) | See [UtilizedTemplateNames](#utilizedtemplatenames)      | Capture Vision template names for document detection and normalization. This typically does not need to be set as MDS provides a default template for general use. You may set custom names to self-host resources or fully self-host MDS - see [self-hosting resources]({{ site.guide }}index.html#self-host-resources) and [DCV templates](https://www.dynamsoft.com/capture-vision/docs/core/parameters/file/capture-vision-template.html?lang=javascript) for details. |
| `enableBoundsDetectionMode`       | `boolean`                                         | `true`  | Set the document Bounds Detection mode effective upon entering the `DocumentScannerView` UI. Bounds Detection mode gets enabled when Smart Capture mode is enabled.                                                                                                              |
| `enableSmartCaptureMode`          | `boolean`                                         | `false` | Set the Smart Capture mode effective upon entering the `DocumentScannerView` UI. Enabling Smart Capture mode also enables Bounds Detection mode. Smart Capture mode is enabled when Auto-Capture mode is enabled.                                                               |
| `enableAutoCropMode`              | `boolean`                                         | `false` | Set the Auto-Crop mode effective upon entering the `DocumentScannerView` UI. Enabling Auto-Crop mode also enables Smart Capture mode.                                                                                                                                            |
| `enableFrameVerification`         | `boolean`                                         | `true`  | Enable automatic frame verification for best quality capture. When enabled, track clarity scores to find the clearest frame.                                                                                                                                                     |
| `scanRegion`                      | [`ScanRegion`](#scanregion)                       | -       | Define the region within the viewport to detect documents. See [`ScanRegion`](#scanregion) for details.                                                                                                                                                                          |
| `minVerifiedFramesForAutoCapture` | `number`                                          | `2`     | Set the minimum number of camera frames to detect document boundaries in Smart Capture mode. Accepts integer values between 1 and 5, inclusive.                                                                                                                                  |
| `showSubfooter`                   | `boolean`                                         | `true`  | Set the visibility of the mode selector menu.                                                                                                                                                                                                                                    |
| `showPoweredByDynamsoft`          | `boolean`                                         | `true`  | Set the visibility of the Dynamsoft branding message.                                                                                                                                                                                                                            |

#### Example
```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE", // Replace with your actual license key
    scannerViewConfig: {
        cameraEnhancerUIPath: "../dist/document-scanner.ui.xml", // Use the local file
        enableSmartCaptureMode: true, // Enable smart capture by default
        minVerifiedFramesForAutoCapture: 3 // Stricter boundary detection threshold
    },
});
```

### `DocumentCorrectionViewConfig`

The `DocumentCorrectionViewConfig` interface passes settings to the `DocumentScanner` constructor through the `DocumentScannerConfig` to apply UI and business logic customizations for the `DocumentCorrectionView`.

Only rare and edge-case scenarios require editing MDS source code. MDS uses sensible default values for all omitted properties.

#### Syntax
```typescript
interface DocumentCorrectionViewConfig {
  container?: HTMLElement | string;
  toolbarButtonsConfig?: DocumentCorrectionViewToolbarButtonsConfig;
  templateFilePath?: string;
  utilizedTemplateNames?: UtilizedTemplateNames;
  onFinish?: (result: DocumentResult) => void;
}
```

#### Properties

| Property                | Type                                                                                        | Default | Description                                                                                                                                                                                                                                                                           |
| ----------------------- | ------------------------------------------------------------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `container`             | ``HTMLElement \| string``                                                                   | -       | The HTML container element or selector for the `DocumentCorrectionView` UI.                                                                                                                                                                                                          |
| `toolbarButtonsConfig`  | [`DocumentCorrectionViewToolbarButtonsConfig`](#documentcorrectionviewtoolbarbuttonsconfig) | -       | Configure the appearance and labels of the buttons for the `DocumentCorrectionView` UI. See [`DocumentCorrectionViewToolbarButtonsConfig`](#documentcorrectionviewtoolbarbuttonsconfig) for details.                                                                                  |
| `templateFilePath`      | `string`                                                                                    | -       | Path to the Capture Vision template file for scanning configuration. This typically does not need to be set as MDS provides a default template for general use. You may set custom paths to self-host resources or fully self-host MDS - see [self-hosting resources]({{ site.guide }}index.html#self-host-resources) for details. |
| `utilizedTemplateNames` | [`UtilizedTemplateNames`](#utilizedtemplatenames)                                           | See [UtilizedTemplateNames](#utilizedtemplatenames)       | Capture Vision template names for document detection and normalization. This typically does not need to be set as MDS provides a default template for general use. You may set custom names to self-host resources or fully self-host MDS - see [self-hosting resources]({{ site.guide }}index.html#self-host-resources) and [DCV templates](https://www.dynamsoft.com/capture-vision/docs/core/parameters/file/capture-vision-template.html?lang=javascript) for details. |
| `onFinish`              | `(result: DocumentResult) => void`                                                          | -       | Handler called when the user clicks the "Apply" button. The handler receives a `DocumentResult` of the scan, including the original image, corrected image, detected boundaries, and scan status.                                                                                    |

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

The `DocumentResultViewConfig` interface passes settings to the `DocumentScanner` constructor through the `DocumentScannerConfig` to apply UI and business logic customizations for the `DocumentResultView`.

Only rare and edge-case scenarios require editing MDS source code. MDS uses sensible default values for all omitted properties.

#### Syntax
```typescript
interface DocumentResultViewConfig {
  container?: HTMLElement | string;
  toolbarButtonsConfig?: DocumentResultViewToolbarButtonsConfig;
  onDone?: (result: DocumentResult) => Promise<void>;
  onUpload?: (result: DocumentResult) => Promise<void>;
}
```

#### Properties

| Property               | Type                                                                                | Default | Description                                                                                                                                                                   |
| ---------------------- | ----------------------------------------------------------------------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `container`            | ``HTMLElement \| string``                                                           | -       | The HTML container element or selector for the `DocumentResultView` UI.                                                                                                      |
| `toolbarButtonsConfig` | [`DocumentResultViewToolbarButtonsConfig`](#documentresultviewtoolbarbuttonsconfig) | -       | Configure the appearance and labels of the buttons for the `DocumentResultView` UI. See [`DocumentResultViewToolbarButtonsConfig`](#documentresultviewtoolbarbuttonsconfig). |
| `onDone`               | `(result: DocumentResult) => Promise<void>`                                         | -       | Handler called when the user clicks the "Done" button. The handler receives a `DocumentResult` of the scan, including the original image, corrected image, detected boundaries, and scan status. |
| `onUpload`             | `(result: DocumentResult) => Promise<void>`                                         | -       | Handler called when the user clicks the "Upload" button. The handler receives a `DocumentResult` of the scan, including the original image, corrected image, detected boundaries, and scan status. |

#### Example
```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    resultViewConfig: {
        onDone: async (result) => {
            const canvas = result.correctedImageResult.toCanvas();
            resultContainer.appendChild(canvas);
        }
    }
});
```

### `DocumentResult`

Represent the output of a scan, including the original image, the corrected image, detected boundaries, and scan status.

#### Syntax
```typescript
interface DocumentResult {
  status: ResultStatus;
  correctedImageResult?: DeskewedImageResultItem;
  originalImageResult?: DSImageData;
  detectedQuadrilateral?: Quadrilateral;
}
```

#### Properties

| Property                | Type                       | Description                                                  |
| ----------------------- | -------------------------- | ------------------------------------------------------------ |
| `status`                | [`ResultStatus`](#resultstatus)             | The status of the document scan (success, failed, canceled). |
| `correctedImageResult`  | `DeskewedImageResultItem`  | The processed (corrected) image.                             |
| `originalImageResult`   | `DSImageData`              | The original captured image before correction.               |
| `detectedQuadrilateral` | `Quadrilateral`            | The detected document boundaries.                            |

### `ScanRegion`

Set the scan region within the `DocumentScannerView` viewfinder for document scanning from the `DocumentScannerViewConfig`.

MDS determines the scan region with the following steps:
1. Use `ratio` to set the height-to-width ratio of the rectangular scanning region, then scale the rectangle up to fit within the viewfinder.
2. Translate the rectangle upward by the number of pixels specified by `regionBottomMargin`.
3. Create a visual border for the scanning region boundary on the viewfinder with a given stroke width in pixels and stroke color.

#### Syntax
```typescript
interface ScanRegion {
  ratio: {
    width: number;
    height: number;
  };
  regionBottomMargin: number; // Bottom margin calculated in pixels
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
| »`width`             | `number` | The width of the rectangular scan region.                  |
| »`height`            | `number` | The height of the rectangular scan region.                 |
| `regionBottomMargin` | `number` | Bottom margin below the scan region measured in pixels.    |
| `style`              | `object` | The styling for the scan region outline in the viewfinder. |
| »`strokeWidth`       | `number` | The pixel width of the outline of the scan region.         |
| »`strokeColor`       | `string` | The color of the outline of the scan region.               |

#### Example

Create a scan region with a height-to-width ratio of 3:2, translated upwards by 20 pixels, with a green, 3 pixel-wide border in the viewfinder:

```javascript
scanRegion: {
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

This type allows you to customize the appearance and behavior of toolbar buttons by providing a subset of properties from the complete `ToolbarButton` interface.

#### Syntax
```typescript
export type ToolbarButtonConfig = Pick<ToolbarButton, "icon" | "label" | "className" | "isHidden">;
```

#### Properties

| Property    | Type      | Description                                                       |
| ----------- | --------- | ----------------------------------------------------------------- |
| `icon`      | `string`  | Path or data URL to the button icon image.                        |
| `label`     | `string`  | Text label displayed below the button icon.                       |
| `className` | `string`  | Additional CSS class name to apply to the button element.         |
| `isHidden`  | `boolean` | Flag indicating whether the button is hidden from the toolbar.    |

#### Example
```javascript
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

### `ToolbarButton`

Interface defining the properties and behavior of a toolbar button.

This interface is used internally to create toolbar buttons in the `DocumentResultView` and `DocumentCorrectionView`. While developers typically use `ToolbarButtonConfig` to customize buttons, this interface defines the complete button structure including the click handler. See [Configurable Buttons Per Each View](#configurable-buttons-per-each-view) for examples.

#### Syntax
```typescript
interface ToolbarButton {
  id: string;
  icon: string;
  label: string;
  onClick?: () => void | Promise<void>;
  className?: string;
  isDisabled?: boolean;
  isHidden?: boolean;
}
```

#### Properties

| Property     | Type                                | Default | Description                                                                                                                                                                              |
| ------------ | ----------------------------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`         | `string`                            | -       | Unique identifier for the button.                                                                                                                                                        |
| `icon`       | `string`                            | -       | Path or data URL to the button icon image.                                                                                                                                               |
| `label`      | `string`                            | -       | Text label displayed below the button icon.                                                                                                                                              |
| `onClick`    | `() => void \| Promise<void>`       | -       | Click handler function invoked when the button is clicked. This handler can be synchronous or asynchronous. When provided through `ToolbarButtonConfig`, it overrides the button's default behavior. |
| `className`  | `string`                            | -       | Additional CSS class name to apply to the button element.                                                                                                                                |
| `isDisabled` | `boolean`                           | `false` | Flag indicating whether the button is disabled (non-interactive).                                                                                                                        |
| `isHidden`   | `boolean`                           | `false` | Flag indicating whether the button is hidden from the toolbar.                                                                                                                           |

### Configurable Buttons Per Each View

#### `DocumentCorrectionViewToolbarButtonsConfig`

Configuration interface for customizing toolbar buttons in the `DocumentCorrectionView`.

This interface allows you to customize the appearance and behavior of the toolbar buttons displayed in the `DocumentCorrectionView`. Each button can be configured using a `ToolbarButtonConfig` object to modify its icon, label, CSS class, or visibility.

The behaviors described for each button below are the default behaviors. You can override the default behavior by providing a custom `onClick` handler through the `ToolbarButtonConfig`.

##### Syntax
```typescript
interface DocumentCorrectionViewToolbarButtonsConfig {
  retake?: ToolbarButtonConfig;
  fullImage?: ToolbarButtonConfig;
  detectBorders?: ToolbarButtonConfig;
  apply?: ToolbarButtonConfig;
}
```

##### Properties

| Property        | Type                                          | Description                                                                                                                                                                                                                                                            |
| --------------- | --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `retake`        | [`ToolbarButtonConfig`](#toolbarbuttonconfig) | Configuration for the retake button. **Default behavior:** returns to the `DocumentScannerView` to capture a new image.                                                                                                                                                |
| `fullImage`     | [`ToolbarButtonConfig`](#toolbarbuttonconfig) | Configuration for the full image button. **Default behavior:** sets the document boundaries to match the full image dimensions.                                                                                                                                        |
| `detectBorders` | [`ToolbarButtonConfig`](#toolbarbuttonconfig) | Configuration for the detect borders button. **Default behavior:** automatically detects document boundaries using the document detection algorithm.                                                                                                                   |
| `apply`         | [`ToolbarButtonConfig`](#toolbarbuttonconfig) | Configuration for the apply button. **Default behavior:** applies the current boundary adjustments and proceeds with the workflow. In continuous scanning mode (`enableContinuousScanning`) when `DocumentResultView` is disabled, this button is labeled "Keep Scan" by default and returns to the `DocumentScannerView` for the next scan. Otherwise, it is labeled "Apply" when `DocumentResultView` is shown, or labeled "Done" otherwise. |

##### Example: Customize Button Appearance
```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE",
    correctionViewConfig: {
        toolbarButtonsConfig: {
            fullImage: {
                isHidden: true
            },
            detectBorders: {
                icon: "path/to/new_icon.png",
                label: "Custom Label"
            }
        }
    }
});
```

##### Example: Override Button Behavior
```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE",
    correctionViewConfig: {
        toolbarButtonsConfig: {
            apply: {
                label: "Confirm",
                onClick: async () => {
                    // Custom confirmation logic
                    await validateBoundaries();
                    console.log("Boundaries confirmed!");
                }
            }
        }
    }
});
```

#### `DocumentResultViewToolbarButtonsConfig`

Configuration interface for customizing toolbar buttons in the `DocumentResultView`.

This interface allows you to customize the appearance and behavior of the toolbar buttons displayed in the `DocumentResultView`. Each button can be configured using a `ToolbarButtonConfig` object to modify its icon, label, CSS class, or visibility.

The behaviors described for each button below are the default behaviors. You can override the default behavior by providing a custom `onClick` handler through the `ToolbarButtonConfig`.

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

##### Properties

| Property  | Type                                          | Description                                                                                                                                                                                                                      |
| --------- | --------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `retake`  | [`ToolbarButtonConfig`](#toolbarbuttonconfig) | Configuration for the retake button. **Default behavior:** returns to the `DocumentScannerView` to capture a new image.                                                                                                          |
| `correct` | [`ToolbarButtonConfig`](#toolbarbuttonconfig) | Configuration for the correct button. **Default behavior:** enters the `DocumentCorrectionView` to adjust document boundaries.                                                                                                   |
| `share`   | [`ToolbarButtonConfig`](#toolbarbuttonconfig) | Configuration for the share button. **Default behavior:** shares or downloads the scanned document. On mobile devices with Web Share API support, this button triggers the native share dialog. On desktop or devices without share support, it downloads the image instead. |
| `upload`  | [`ToolbarButtonConfig`](#toolbarbuttonconfig) | Configuration for the upload button. **Default behavior:** triggers the `onUpload` callback. This button is only visible when `onUpload` is defined.                                                                             |
| `done`    | [`ToolbarButtonConfig`](#toolbarbuttonconfig) | Configuration for the done button. **Default behavior:** completes the scanning workflow. In continuous scanning mode (`enableContinuousScanning`), this button is labeled "Keep Scan" by default and returns to the `DocumentScannerView` for the next scan. |

##### Example: Customize Button Appearance
```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE",
    resultViewConfig: {
        toolbarButtonsConfig: {
            retake: {
                isHidden: true
            },
            share: {
                icon: "path/to/new_icon.png",
                label: "Custom Label"
            }
        }
    }
});
```

##### Example: Override Button Behavior
```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE",
    resultViewConfig: {
        toolbarButtonsConfig: {
            done: {
                label: "Save",
                onClick: async () => {
                    // Custom save logic
                    await saveToServer(documentScanner.result);
                    console.log("Document saved!");
                }
            },
            share: {
                onClick: async () => {
                    // Custom share logic
                    await sendViaEmail(documentScanner.result);
                }
            }
        }
    }
});
```

## Assisting Interfaces

### `UtilizedTemplateNames`

Capture Vision template names for document detection and normalization.

You may set custom names to self-host resources or fully self-host MDS. See [self-hosting resources]({{ site.guide }}index.html#self-host-resources) and [DCV templates](https://www.dynamsoft.com/capture-vision/docs/core/parameters/file/capture-vision-template.html?lang=javascript) for details.

#### Syntax
```typescript
interface UtilizedTemplateNames {
  detect: string;
  normalize: string;
}
```

#### Default Value
```javascript
{
  detect: "DetectDocumentBoundaries_Default",
  normalize: "NormalizeDocument_Default"
}
```

### `ResultStatus`

Represents the status of a document scanning operation.

#### Syntax
```typescript
type ResultStatus = {
  code: EnumResultStatus;
  message?: string;
};
```

#### Properties

| Property  | Type                                        | Description                                         |
| --------- | ------------------------------------------- | --------------------------------------------------- |
| `code`    | [`EnumResultStatus`](#enumresultstatus)     | Status code indicating success, cancellation, or failure. |
| `message` | `string`                                    | Optional message providing additional details.      |

### `EnumResultStatus`

Enumeration of possible result status codes.

#### Syntax
```typescript
enum EnumResultStatus {
  RS_SUCCESS = 0,
  RS_CANCELLED = 1,
  RS_FAILED = 2
}
```

#### Values

| Value          | Code | Description                                    |
| -------------- | ---- | ---------------------------------------------- |
| `RS_SUCCESS`   | 0    | The scanning operation completed successfully. |
| `RS_CANCELLED` | 1    | The scanning operation was cancelled by user.  |
| `RS_FAILED`    | 2    | The scanning operation failed.                 |

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
