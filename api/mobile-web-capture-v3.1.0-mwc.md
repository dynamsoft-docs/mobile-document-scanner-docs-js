---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: false
title: Dynamsoft Mobile Web Capture - API Reference
keywords: Documentation, Mobile Web Capture, Dynamsoft Document Scanner, API, APIs
breadcrumbText: API Reference
description: Mobile Web Capture API Reference
---

# Mobile Web Capture API Reference

The `MobileWebCapture` class manages document scanning, viewing, and management.

<!--  Keep TOC only for npm /github as readme
- [Constructor](#constructor)
  - [MobileWebCapture](#mobilewebcapture)
- [Methods](#methods)
  - [launch()](#launch)
  - [dispose()](#dispose)
- [Properties](#properties)
  - [hasLaunched](#haslaunched)
- [Configuration Interfaces](#configuration-interfaces)
  - [MobileWebCaptureConfig](#mobilewebcaptureconfig)
  - [LibraryViewConfig](#libraryviewconfig)
  - [DocumentViewConfig](#documentviewconfig)
  - [PageViewConfig](#pageviewconfig)
  - [HistoryViewConfig](#historyviewconfig)
  - [TransferViewConfig](#transferviewconfig)
- [Toolbar Button Configurations](#toolbar-button-configurations)
  - [ToolbarButtonConfig](#toolbarbuttonconfig)
  - [Configurable Buttons Per Each View](#configurable-buttons-per-each-view)
- [Assisting Interfaces](#assisting-interfaces)
  - [ExportConfig](#exportconfig) -->

## Constructor

### `MobileWebCapture`

#### Syntax
```typescript
constructor(config: MobileWebCaptureConfig)
```

#### Parameters

| Parameter | Type                                                | Description                                          |
| --------- | --------------------------------------------------- | ---------------------------------------------------- |
| `config`  | [`MobileWebCaptureConfig`](#mobilewebcaptureconfig) | The configuration settings for **MobileWebCapture**. |

#### Example
```javascript
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    documentScannerConfig: {
        showResultView: false,
        showCorrectionView: false,
    },
});
(async () => {
    // Launch the Mobile Web Capture Instance
    const fileName = `New_Document_${Date.now().toString().slice(-5)}`;
    await mobileWebCapture.launch(fileName);
})();
```

## Methods

### `launch()`

Starts the **Mobile Web Capture** workflow.  

#### Syntax
```typescript
async launch(file?: File | string, view?: EnumMWCStartingViews)
```

#### Parameters

| Parameter | Type                                                       | Description                                |
| --------- | ---------------------------------------------------------- | ------------------------------------------ |
| `file`    | ``File | string`` (optional)                               | A file or document name to open at launch. |
| `view`    | [`EnumMWCStartingViews`](#enummwcstartingviews) (optional) | The first View to display upon launch.     |

#### Throws
- An error if **MWC** is already running.

#### Example
```typescript
await mobileWebCapture.launch();
console.log("MWC launched successfully.");
```

**Launching with a File**

```html
<input type="file" id="initialFile" accept="image/*,application/pdf" />
```

```javascript
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
});
document.getElementById("initialFile").onchange = async function () {
    const files = Array.from(this.files || []);
    if (files.length) {
        // Launch the Mobile Web Capture instance with an initial file
        if (mobileWebCapture.hasLaunched) 
            await mobileWebCapture.dispose();
        await mobileWebCapture.launch(files[0]);
    }
};
```

### `dispose()`

Cleans up resources and closes the `MobileWebCapture` instance.

#### Syntax
```typescript
dispose(): Promise<void>
```

#### Example
```javascript
await mobileWebCapture.dispose();
console.log("MWC resources released.");
```

## Properties

### `hasLaunched`

Returns whether the `MobileWebCapture` instance is running.

#### Syntax
```typescript
readonly hasLaunched: boolean;
```

```html
<input type="file" id="initialFile" accept="image/*,application/pdf" />
```

```javascript
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
});
document.getElementById("initialFile").onchange = async function () {
    const files = Array.from(this.files || []);
    if (files.length) {
        // Launch the Mobile Web Capture instance with an initial file
        if (mobileWebCapture.hasLaunched) 
            await mobileWebCapture.dispose();
        await mobileWebCapture.launch(files[0]);
    }
};
```

## Configuration Interfaces

### `MobileWebCaptureConfig`

#### Syntax
```typescript
interface MobileWebCaptureConfig {
  license?: string;
  container?: HTMLElement | string;
  exportConfig?: ExportConfig;
  showLibraryView?: boolean;
  onClose?: () => void;

  // View Configs
  libraryViewConfig?: LibraryViewConfig;
  documentViewConfig?: DocumentViewConfig;
  pageViewConfig?: PageViewConfig;
  transferViewConfig?: TransferViewConfig;
  historyViewConfig?: HistoryViewConfig;

  // DDS Config
  documentScannerConfig?:DocumentScannerConfig
}
```

#### Properties

| Property                | Type                                        | Description                                                                      |
| ----------------------- | ------------------------------------------- | -------------------------------------------------------------------------------- |
| `license`               | `string`                                    | The license key for using **Mobile Web Capture (MWC)**.                          |
| `container`             | ``HTMLElement | string``                    | The container element or selector for rendering the `MobileWebCapture` instance. |
| `ddvResourcePath`       | `string`                                    | File path to DDV resources.                                                      |
| `exportConfig`          | [`ExportConfig`](#exportconfig)             | Configuration for exporting captured documents.                                  |
| `showLibraryView`       | `boolean`                                   | Determines if the **LibraryView** is shown (default: `true`).                    |
| `onClose`               | `() => void`                                | Callback function triggered when the `MobileWebCapture` instance is closed.      |
| `libraryViewConfig`     | [`LibraryViewConfig`](#libraryviewconfig)   | Configuration for the **LibraryView**.                                           |
| `documentViewConfig`    | [`DocumentViewConfig`](#documentviewconfig) | Configuration for the **DocumentView**.                                          |
| `pageViewConfig`        | [`PageViewConfig`](#pageviewconfig)         | Configuration for the **PageView**.                                              |
| `transferViewConfig`    | [`TransferViewConfig`](#transferviewconfig) | Configuration for the **TransferView**.                                          |
| `transferViewConfig`    | [`TransferViewConfig`](#transferviewconfig) | Configuration for the **TransferView**.                                          |
| `scanner`               | [`MWCScanner`](#mwcscanner)                 | Configured document scanner module (uses DDS by default).                        |
| `documentScannerConfig` | `DocumentScannerConfig`                     | Configuration for the built-in **DocumentScanner**.                              |

##### See Also

- [`DocumentScannerConfig`]({{ site.api }}document-scanner.html#documentscannerconfig)

#### Example

```javascript
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    ddvResourcePath: "./dist/libs/dynamsoft-document-viewer/dist/",
    documentScannerConfig: {
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
    },
});
```

### `LibraryViewConfig`

#### Syntax
```typescript
interface LibraryViewConfig {
  emptyContentConfig?: EmptyContentConfig;
  toolbarButtonsConfig?: LibraryToolbarButtonsConfig;
}
```

#### Properties

| Property               | Type                                                          | Description                                                                  |
| ---------------------- | ------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| `emptyContentConfig`   | [`EmptyContentConfig`](#emptycontentconfig)                   | Configuration for the content displayed on the empty **LibraryView** screen. |
| `toolbarButtonsConfig` | [`LibraryToolbarButtonsConfig`](#librarytoolbarbuttonsconfig) | Configuration for the toolbar buttons in **LibraryView**.                    |

#### Example 1: Display A Message In An Empty Library

By default, the `LibraryView` displays the following when empty:

![Empty Library View]({{ site.assets }}imgs/empty-library-view.png)

You can customize its appearance using the [`emptyContentConfig`](#emptycontentconfig) property.

```html
<div id="customizedLibraryViewContent">Create Your First Document!</div>
```

```javascript
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    showLibraryView: true, // Enable LibraryView
    libraryViewConfig: {
        emptyContentConfig: document.getElementById("customizedLibraryViewContent"),
    },
});
```

#### Example 2: Disable Upload in LibraryView

When `exportConfig.uploadToServer` is defined and `showLibraryView` is `true`, an **Upload** button will appears in `LibraryView`. The following example demonstrates how to hide the button.

```javascript
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    showLibraryView: true // Enable LibraryView
    libraryViewConfig: {
        toolbarButtonsConfig: {
            upload: {
                isHidden: true
            }
        }
    }
});
```

### `DocumentViewConfig`

#### Syntax
```typescript
interface DocumentViewConfig {
  emptyContentConfig?: EmptyContentConfig;
  toolbarButtonsConfig?: DocumentToolbarButtonsConfig;
}
```

#### Properties

| Property               | Type                                                            | Description                                                                   |
| ---------------------- | --------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| `emptyContentConfig`   | [`EmptyContentConfig`](#emptycontentconfig)                     | Configuration for the content displayed on the empty **DocumentView** screen. |
| `toolbarButtonsConfig` | [`DocumentToolbarButtonsConfig`](#documenttoolbarbuttonsconfig) | Configuration for the toolbar buttons in **DocumentView**.                    |

#### Example 1: Display A Message In An Empty Document

By default, the `DocumentView` displays the following when empty:

![Empty Document View](https://www.dynamsoft.com/mobile-web-capture/docs/assets/imgs/empty-document-view.png)

You can customize its appearance using the [`emptyContentConfig`](#emptycontentconfig) property.

```html
<div id="customizedDocViewContent">Start Your Document!</div>
```

```javascript
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    documentViewConfig: {
        emptyContentConfig: document.getElementById("customizedDocViewContent"),
    },
});
```

#### Example 2: Disable Upload in DocumentView

When `exportConfig.uploadToServer` is defined, the **Upload** button appears in both `DocumentView` and `PageView`. The following example demonstrates how to disable this feature by hiding it in `DocumentView`, ensuring that the **Upload** button only appears in `PageView`.

```javascript
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    documentViewConfig: {
        toolbarButtonsConfig: { // Note that there are two upload buttons in DocumentView
            uploadDocument: {
                isHidden: true
            },
            uploadImage: {
                isHidden: true
            }
        }
    }
});
```

#### Example 3: Update the Button Icon

If you don't like a button's icon, you can customize it. The following example shows how to change the icon of the **"Share Document"** button:

```javascript
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    documentViewConfig: {
        toolbarButtonsConfig: { // Note that there are two upload buttons in DocumentView
            shareDocument: {
                icon: "path/to/new_icon.png", // Change to the actual path of the new icon
                label: "Custom Label"
            }
        }
    }
});
```

### `PageViewConfig`

#### Syntax
```typescript
interface PageViewConfig {
  toolbarButtonsConfig?: PageViewToolbarButtonsConfig;
  annotationToolbarLabelConfig?: DDVAnnotationToolbarLabelConfig;
}
```

#### Properties

| Property                       | Type                                                                   | Description                                        |
| ------------------------------ | ---------------------------------------------------------------------- | -------------------------------------------------- |
| `toolbarButtonsConfig`         | [`PageViewToolbarButtonsConfig`](#pageviewtoolbarbuttonsconfig)        | Configuration for toolbar buttons in **PageView**. |
| `annotationToolbarLabelConfig` | [`DDVAnnotationToolbarLabelConfig`](#ddvannotationstoolbarlabelconfig) | Configuration for annotation toolbar labels.       |

#### Example 1: Disable Upload in PageView

In this example, we'll demonstrate how to hide the **Upload** button in `PageView` even when `exportConfig.uploadToServer` is defined, ensuring that it only appears in `DocumentView`.

```javascript
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    pageViewConfig: {
        toolbarButtonsConfig: {
            upload: {
                isHidden: true
            }
        }
    }
});
```

#### Example 2: Change the Labels of the Annotation Toolbar Buttons

You can customize the labels of the annotation toolbar buttons as follows:

```javascript
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    pageViewConfig: {
        annotationToolbarLabelConfig: {
            TextBoxAnnotation: "Input Text",
        },
    }
});
```

### `HistoryViewConfig`

#### Syntax
```typescript
interface HistoryViewConfig {
  emptyContentConfig?: EmptyContentConfig;
  toolbarButtonsConfig?: HistoryToolbarButtonsConfig;
}
```

#### Properties

| Property               | Type                                                          | Description                                                                  |
| ---------------------- | ------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| `emptyContentConfig`   | [`EmptyContentConfig`](#emptycontentconfig)                   | Configuration for the content displayed on the empty **HistoryView** screen. |
| `toolbarButtonsConfig` | [`HistoryToolbarButtonsConfig`](#historytoolbarbuttonsconfig) | Configuration for the toolbar buttons in **HistoryView**.                    |

### `TransferViewConfig`

#### Syntax
```typescript
interface TransferViewConfig {
  toolbarButtonsConfig?: TransferToolbarButtonsConfig;
}
```

#### Properties

| Property               | Type                                                            | Description                                                |
| ---------------------- | --------------------------------------------------------------- | ---------------------------------------------------------- |
| `toolbarButtonsConfig` | [`TransferToolbarButtonsConfig`](#transfertoolbarbuttonsconfig) | Configuration for the toolbar buttons in **TransferView**. |

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
```javascript
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    documentViewConfig: {
        toolbarButtonsConfig: { // Note that there are two upload buttons in DocumentView
            uploadDocument: {
                isHidden: true
            },
            uploadImage: {
                isHidden: true
            }
        }
    }
});
```

### Configurable Buttons Per Each View

#### `LibraryToolbarButtonsConfig`
```typescript
interface LibraryToolbarButtonsConfig {
  newDoc?: ToolbarButtonConfig;
  capture?: ToolbarButtonConfig;
  import?: ToolbarButtonConfig;
  uploads?: ToolbarButtonConfig;

  // Selected Toolbar Options
  delete?: ToolbarButtonConfig;
  print?: ToolbarButtonConfig;
  share?: ToolbarButtonConfig;
  upload?: ToolbarButtonConfig;
  back?: ToolbarButtonConfig;
}
```

#### `DocumentToolbarButtonsConfig`
```typescript
interface DocumentToolbarButtonsConfig {
  backToLibrary?: ToolbarButtonConfig;
  capture?: ToolbarButtonConfig;
  import?: ToolbarButtonConfig;
  shareDocument?: ToolbarButtonConfig;
  uploadDocument?: ToolbarButtonConfig;
  manage?: ToolbarButtonConfig;

  // Selected Toolbar Options
  copyTo?: ToolbarButtonConfig;
  moveTo?: ToolbarButtonConfig;
  selectAll?: ToolbarButtonConfig;
  deleteImage?: ToolbarButtonConfig;
  shareImage?: ToolbarButtonConfig;
  uploadImage?: ToolbarButtonConfig;
  back?: ToolbarButtonConfig;
}
```

#### `PageViewToolbarButtonsConfig`
```typescript
interface PageViewToolbarButtonsConfig {
  back?: ToolbarButtonConfig;
  delete?: ToolbarButtonConfig;
  addPage?: ToolbarButtonConfig;
  upload?: ToolbarButtonConfig;
  share?: ToolbarButtonConfig;
  edit?: ToolbarButtonConfig;

  // Edit mode toolbar
  crop?: ToolbarButtonConfig;
  rotate?: ToolbarButtonConfig;
  filter?: ToolbarButtonConfig;
  annotate?: ToolbarButtonConfig;
  done?: ToolbarButtonConfig;
}

```

#### `HistoryToolbarButtonsConfig`
```typescript
interface HistoryToolbarButtonsConfig {
  back?: ToolbarButtonConfig;
}
```

#### `TransferToolbarButtonsConfig`
```typescript
interface TransferToolbarButtonsConfig {
  cancel?: ToolbarButtonConfig;
  action?: ToolbarButtonConfig;
}
```

#### `DDVAnnotationsToolbarLabelConfig`

```typescript
interface DDVAnnotationToolbarLabelConfig {
  Undo: string;
  Redo: string;
  SelectAnnotation: string;
  EraseAnnotation: string;
  RectAnnotation: string;
  EllipseAnnotation: string;
  PolygonAnnotation: string;
  PolylineAnnotation: string;
  LineAnnotation: string;
  InkAnnotation: string;
  TextBoxAnnotation: string;
  TextTypewriterAnnotation: string;
  StampIconAnnotation: string;
  StampImageAnnotation: string;
  BringForward: string;
  BringToFront: string;
  SendBackward: string;
  SendToBack: string;
}
```

## Assisting Interfaces 

### `ExportConfig`

The `ExportConfig` interface defines methods for handling document export functionality, such as uploading, downloading, deleting files from a server, and managing the upload success process. This allows full customization of how documents are exported and managed in your application.

#### Properties

##### `uploadToServer`

Uploads a document to the server. The function receives the document's file name and its binary data (`Blob`). It returns a `Promise` that resolves with `void` or an `UploadedDocument` object.

> [!IMPORTANT]
> Returning `{ status: "success" }` in this function is required to trigger [`onUploadSuccess`](#onuploadsuccess)

###### **Type**  
```typescript
(fileName: string, blob: Blob) => Promise<void | UploadedDocument>
```

##### `downloadFromServer`

Downloads a document from the server. The function receives an `UploadedDocument` object and returns a `Promise` that resolves once the download is complete.

###### **Type**  
```typescript
(doc: UploadedDocument) => Promise<void>
```

##### `deleteFromServer`

Deletes a document from the server. The function receives an `UploadedDocument` object and returns a `Promise` that resolves once the deletion is successful.

###### **Type**  
```typescript
(doc: UploadedDocument) => Promise<void>
```

##### `onUploadSuccess`

Called after a successful upload. It receives the file name, file type, the current view ([`EnumMWCViews`](#enummwcviews)), and the binary data (`Blob`). The function should return a `Promise` that resolves to `true` if the `MobileWebCapture` instance should close after uploading or `false` if it should remain open.

> [!IMPORTANT]
> Returning `{ status: "success" }` in [`uploadToServer`](#uploadtoserver) is required to trigger this function.

###### **Type**  
```typescript
(fileName: string, fileType: string, view: EnumMWCViews, blob: Blob) => Promise<boolean>
```

#### Example Usage

```javascript
const uploadToServer = async (fileName, blob) => {
    const host = window.location.origin;
    // Create form data
    const formData = new FormData();
    formData.append("uploadFile", blob, fileName);
    // Upload file
    const response = await fetch(
        `${host}/upload`, // Change this to your actual upload URL
        {
        method: "POST",
        body: formData,
        }
    );
    if (response.status === 200) {
        // **IMPORTANT**: Returning { status: "success" } is required to trigger onUploadSuccess.
        return {
            status: "success",
        };
    } else {
        return {
            status: "failed",
        };
    }
};
const onUploadSuccess = async (fileName) => {
    console.log(`${fileName} uploaded successfully!`);
    return true; // Exit the Mobile Web Capture Instance
};
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    exportConfig: {
        uploadToServer,
        onUploadSuccess,
    },
});
(async () => {
    // Launch the Mobile Web Capture Instance
    const fileName = `New_Document_${Date.now().toString().slice(-5)}`;
    await mobileWebCapture.launch(fileName);
})();
```

### `MWCScanner`

MWC can scan from objects that satisfy this interface. When unassigned, MWC uses DDS by default.

#### Properties

##### `initialize()`

Initialize scanner resources.

###### **Type**

```typescript
async initialize(): Promise<any>
```

#### `launch()`

Launch the scanner, and return a promise of a scan result that resolves once the scan completes. This scan result contains an `_imageData` object which contains a `toBlob()` that returns the scanned image as a blob. `imageData` (**not `_imageData`**) must be set to `true`.

##### **Type**

```typescript
async launch(): Promise<{
    _imageData: {
        toBlob()
    },
    imageData: boolean,
}>
```

#### `dispose()`

Destructor for the scanner object.

##### **Type**

```typescript
dispose(): void
```

### `EmptyContentConfig`

Configuration for the content displayed on an empty View screen.

#### Syntax

```typescript
type EmptyContentConfig =
  | string
  | HTMLElement
  | HTMLTemplateElement
  | {
      templatePath: string; // Path to HTML template file
    };
```

## Enums

### `EnumMWCViews`

#### Type

```typescript
enum EnumMWCViews {
  Library = "library",
  Page = "page",
  Document = "document",
  Transfer = "transfer",
  History = "history",
  Scanner = "Scanner",
}
```

### `EnumMWCStartingViews`

Use with [`MobileWebCaptureConfig`](#mobilewebcaptureconfig) to set which View to show first upon launching MWC.

#### Type

```typescript
type EnumMWCStartingViews = EnumMWCViews.Library | EnumMWCViews.Document | EnumMWCViews.Scanner
```