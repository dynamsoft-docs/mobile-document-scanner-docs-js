---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: false
title: Mobile Web Capture - Scan Multi-Page Documents
keywords: Documentation, Mobile Web Capture, Dynamsoft Document Scanner,
description: Mobile Web Capture User Guide
---

# How to Customize Mobile Web Capture

> [!TIP]
> Prerequisites: read the [MWC Getting Started Guide]({{ site.guides }}mobile-web-capture.html) before proceeding.

This guide expands on the **Hello World** sample from the **MWC Getting Started Guide** and explores the available customization options.

<!--
Keep TOC only for npm /github as readme
**Table of Contents**
- [MobileWebCaptureConfig Overview](#mobilewebcaptureconfig-overview)
- [Overall UI and Workflow Customization](#overall-ui-and-workflow-customization)
  - [Specify the UI Container](#specify-the-ui-container)
  - [Enable the LibraryView](#enable-the-libraryview)
  - [Start by Opening a Document](#start-by-opening-a-document)
  - [Scan Directly to Document](#scan-directly-to-document)
  - [Enable File Upload](#enable-file-upload)
  - [Enable Upload History](#enable-upload-history)
- [View-Based Customization](#view-based-customization)
  - [DocumentView Configuration](#documentview-configuration)
  - [LibraryView Configuration](#libraryview-configuration)
  - [PageView Configuration](#pageview-configuration)
  - [TransferView and HistoryView Configuration](#transferview-and-historyview-configuration)
- [Self-Hosting Resource Files](#self-hosting-resource-files)
  - [Modify the Build Script](#modify-the-build-script)
  - [Build the Project](#build-the-project)
  - [Serve the Project Locally](#serve-the-project-locally)
- [Next Step](#next-step)
-->

## MobileWebCaptureConfig Overview

`MobileWebCaptureConfig` is the primary configuration object for customizing **MWC**. It includes the following properties:

1. `license`: The license key.
2. `container`: The HTML container for the entire workflow. If not specified (like in the **Hello World** Sample), one is created automatically.
3. `exportConfig`: Configures functions for handling document export operations.
   1. `uploadToServer`: Specifies a function to upload a file to a server.
   2. `downloadFromServer`: Specifies a function to download a document from the server.
   3. `deleteFromServer`: Specifies a function to delete a document from the server.
   4. `onUploadSuccess`: Specifies a function that is triggered when the upload operation succeeds.
4. `showLibraryView`: Configures where or not this **MWC** instance starts with the `LibraryView`.
5. `onClose`: Specifies a function that is triggered when the user closes this **MWC** instance.
6. `documentScannerConfig`: Configures the behavior of the built-in `DocumentScanner` instance. See the details in [`DocumentScannerConfig`]({{ site.guides }}document-scanner.html#documentscannerconfig-overview).
7. `libraryViewConfig`: Configures the library view with the following properties:
   1. `emptyContentConfig`: Specifies the content displayed in the library view when it is empty (no document).
   2. `toolbarButtonsConfig`: Configures the buttons in the toolbar of the library view.
8. `documentViewConfig`: Configures the document view with the following properties:
   1. `emptyContentConfig`: Specifies the content displayed in the document view when it is empty (no page).
   2. `toolbarButtonsConfig`: Configures the buttons in the toolbar of the document view.
9.  `pageViewConfig`: Configures the page view.
   1. `toolbarButtonsConfig`: Configures the buttons in the toolbar of the page view.
   2. `annotationToolbarLabelConfig`: Configures the labels of the annotation options.
10. `transferViewConfig`: Configures the transfer view.
   1. `toolbarButtonsConfig`: Configures the buttons in the toolbar of the transfer view.
11. `historyViewConfig`: Configures the history view.
   1. `emptyContentConfig`: Specifies the content displayed in the history view when it is empty (no uploads).
   2. `toolbarButtonsConfig`: Configures the button in the toolbar of the history view.
12. `ddvResourcePath`: Paths to extra resources such as `.wasm` engine files and CSS files.

API Reference: [`MobileWebCaptureConfig`]({{ site.api }}mobile-web-capture.html#mobilewebcaptureconfig)

## Overall UI and Workflow Customization

**MWC** automatically creates containers that fill the entire viewport for its **Views** if none are specified in the configuration. These views come with a predefined workflow. The following demonstrates a few ways to customize the overall UI.

### Specify the UI Container

When a container is assigned, **MWC** UI will be confined in that container element:

```html
<div id="myMobileWebCapture" style="width: 80vw; height: 80vh;"></div>
```

```javascript
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    container: document.getElementById("myMobileWebCapture") , // Use this container for the full workflow
});
(async () => {
    // Launch the Mobile Web Capture Instance
    const fileName = `New_Document_${Date.now().toString().slice(-5)}`;
    await mobileWebCapture.launch(fileName);
})();
```

### Enable the LibraryView

By default, **MWC** starts with `DocumentView`, disabling `LibraryView`, so only one document is created and managed. Enabling `LibraryView` allows multiple documents.

> [!NOTE]
> Since **MWC** now starts with `LibraryView`, a document name is no longer required. If specified, `LibraryView` is still enabled, but **MWC** will start with `DocumentView`.

```javascript
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    showLibraryView: true // Enable LibraryView
});
(async () => {
    // Launch the Mobile Web Capture Instance
    await mobileWebCapture.launch(); // No need to specify a document name.
})();
```

On the `LibraryView`, the user can

1. **New** : Create a new document.
2. **Capture** : Create a new document by capturing the first image for it.
3. **Import** : Create a new document by importing one or multiple images or PDF files.

The user can also enable the **"Upload"** feature. Check out [Enable File Upload](#enable-file-upload) and [Enable Upload History](#enable-upload-history). When the **Upload History** feature is enabled, the user can

- **Uploads**: View uploaded documents from this session, download, or delete them.

### Start by Opening a Document

By default, **MWC** starts empty. However, you can specify a file to be opened as the initial document.

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

API Reference:
- [`hasLaunched`]({{ site.api }}mobile-web-capture.html#haslaunched)
- [`dispose`]({{ site.api }}mobile-web-capture.html#dispose)

### Scan Directly to Document

When **capturing** a document, it goes through three views:

1. **`DocumentScannerView`**
2. **`DocumentCorrectionView`** (optional)
3. **`DocumentResultView`** (optional)

The latter two views can be skipped to speed up the process.

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

### Enable File Upload

When `exportConfig.uploadToServer` is defined, an **Upload** button appears in both `DocumentView` and `PageView`.

> [!NOTE]
> If `LibraryView` is enabled, the **Upload** button will also appear there.

The following example demonstrates how to enable file upload and exit the Mobile Web Capture Instance after a successful upload.

> [!TIP]
> If you followed the steps in [Build from Source](#option-1-build-from-source) and are still using the predefined Express server setup, the following upload code will work correctly. See the server configuration details in [`/dev-server/index.js`](https://github.com/Dynamsoft/mobile-web-capture/blob/main/dev-server/index.js).

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

> [!IMPORTANT]
> The **Upload** feature is enabled simultaneously in `DocumentView` and `PageView` (and in `LibraryView` if it is enabled). If this is not intended, you can hide the **Upload** button in these **Views**.
> Read more:
> 1. [Disable Upload in DocumentView](#example-2-disable-upload-in-documentview)
> 2. [Disable Upload in PageView](#example-1-disable-upload-in-pageview)
> 3. [Disable Upload in LibraryView](#example-2-disable-upload-in-libraryview)

### Enable Upload History

When **File Upload** feature is on and `LibraryView` is enabled, we can enable the **Upload History** feature in the `LibraryView` by defining all the following:
1. `exportConfig.uploadToServer`
2. `exportConfig.downloadFromServer`
3. `exportConfig.deleteFromServer`

The following example demonstrates how to enable this feature.

> [!NOTE]
> If you followed the steps in [Build from Source](#option-1-build-from-source) and are still using the predefined Express server setup, the following code will work correctly. See the server configuration details in [`/dev-server/index.js`](https://github.com/Dynamsoft/mobile-web-capture/blob/main/dev-server/index.js).

```javascript
const host = window.location.origin;
const shortSessionID = Math.random().toString(36).substring(2, 12);
const uploadToServer = async (fileName, blob) => {
    const formData = new FormData();
    formData.append("uploadFile", blob, fileName);
    formData.append("fileName", fileName);
    formData.append("sessionID", shortSessionID);
    // Upload file
    const response = await fetch(
        `${host}/upload`, // Change this to your actual upload URL
        {
        method: "POST",
        body: formData,
        }
    );
    const responseText = await response.text();
    if (!responseText || !responseText.includes("UploadedFileName")) {
        throw new Error("Invalid server response");
    }
    const serverFileName = responseText.match(
        /UploadedFileName:(.+)_(\d+)_(.+)$/
    );
    if (!serverFileName) {
        throw new Error("Failed to parse server response");
    }
    const [, sessionID, uploadTime, realFileName] = serverFileName;
    const downloadUrl = `${host}/download?fileName=${encodeURIComponent(
        `${sessionID}_${uploadTime}_${realFileName}`
    )}`;
    // NOTE: Ensure the object returned contains status, fileName, and downloadUrl
    return {
        status: "success",
        fileName: realFileName,
        downloadUrl,
        uploadTime,
    };
};
const deleteFromServer = async (doc) => {
    const response = await fetch(
    `${host}/delete?fileName=${encodeURIComponent(
        `${shortSessionID}_${doc.uploadTime}_${doc.fileName}`
    )}`,
    {
        method: "POST",
    }
    );
};
const downloadFromServer = async (doc) => {
    window.open(doc.downloadUrl);
};
const mobileWebCapture = new Dynamsoft.MobileWebCapture({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    showLibraryView: true, // Enable LibraryView
    exportConfig: {
        uploadToServer,
        downloadFromServer,
        deleteFromServer,
    }
});
(async () => {
    // Launch the Mobile Web Capture Instance
    const fileName = `New_Document_${Date.now().toString().slice(-5)}`;
    await mobileWebCapture.launch(fileName);
})();
```

## View-Based Customization
### DocumentView Configuration

Consider the following configuration interface used for customizing the `DocumentView`:

```javascript
interface DocumentViewConfig {
  emptyContentConfig?: EmptyContentConfig;
  toolbarButtonsConfig?: DocumentToolbarButtonsConfig;
}
```

#### Example 1: Display A Message In An Empty Document

By default, the `DocumentView` displays the following when empty:

![Empty Document View]({{ site.assets }}imgs/empty-document-view.png)

You can customize its appearance using the `emptyContentConfig` property.

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

> [!TIP]
> Read more in [Enable File Upload](#enable-file-upload).

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

### LibraryView Configuration

Consider the following configuration interface used for customizing the `LibraryView`:

```javascript
interface LibraryViewConfig {
  emptyContentConfig?: EmptyContentConfig;
  toolbarButtonsConfig?: LibraryToolbarButtonsConfig;
}
```

#### Example 1: Display A Message In An Empty Library

By default, the `LibraryView` displays the following when empty:

![Empty Library View]({{ site.assets }}imgs/empty-library-view.png)

You can customize its appearance using the `emptyContentConfig` property.

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

> [!NOTE]
> Read more in [Enable File Upload](#enable-file-upload) & [Enable Upload History](#enable-upload-history).

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

### PageView Configuration

Consider the following configuration interface used for customizing the `PageView`:

```javascript
interface PageViewConfig {
  toolbarButtonsConfig?: PageViewToolbarButtonsConfig;
  annotationToolbarLabelConfig?: DDVAnnotationToolbarLabelConfig;
}
```

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

### TransferView and HistoryView Configuration

The configuration follows similar patterns, so we won’t cover them here for brevity.

## Self-Hosting Resource Files

By default, **MWC** relies on a **CDN** for resources such as `.wasm` engine files. If you require a **fully offline setup**, follow these steps:

> [!IMPORTANT]
> These steps are based on [Build from Source](#option-1-build-from-source), meaning that all **MWC** source files must be available on your local machine.

#### Update the Resource Paths

The following code modifies how resource files are referenced:

> [!TIP]
> In this case, we reference local resource files that are copied during the build process. See [Modify the Build Script](#modify-the-build-script) for details. However, you can also reference your own copies, such as files hosted on your own server. If you need assistance, feel free to [contact us](https://www.dynamsoft.com/company/contact/).

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

### Modify the Build Script

Update the `scripts` section in `package.json` to automatically copy the libraries during the build process:

```json
"scripts": {
    "serve": "node dev-server/index.js",
    "build": "rollup -c && npm run copy-libs",
    "copy-libs": "npx mkdirp dist/libs && npx cpx \"node_modules/dynamsoft-*/**/*\" dist/libs/ --dereference",
    "build:production": "rollup -c --environment BUILD:production"
},
```

### Build the Project

Once all dependencies are installed, build the project by running:

```bash
npm run build
```

### Serve the Project Locally

Start the local development server by running:
```bash
npm run serve
```

Once the server is running, open the application in a browser using the address provided in the terminal output.

Now, all required files will be **served locally** without relying on a CDN.

> [!IMPORTANT]
>* Certain legacy web application servers may lack support for the `application/wasm` mimetype for .wasm files. To address this, you have two options:
>  1. Upgrade your web application server to one that supports the `application/wasm` mimetype.
>  2. Manually define the mimetype on your server. You can refer to the guides for [apache](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Apache_Configuration_htaccess#media_types_and_character_encodings) / [IIS](https://docs.microsoft.com/en-us/iis/configuration/system.webserver/staticcontent/mimemap) / [nginx](https://www.nginx.com/resources/wiki/start/topics/examples/full/#mime-types).
>
>* To work properly, the SDK requires a few engine files, which are relatively large and may take quite a few seconds to download. We recommend that you set a longer cache time for these engine files, to maximize the performance of your web application.
>
>  ```
>  Cache-Control: max-age=31536000
>  ```
>
>  Reference: [Cache-Control](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control).

## Next Step

Start building your own mobile document capture and management solution with **MWC**! If you encounter any technical issues or have suggestions, feel free to [contact us](https://www.dynamsoft.com/company/contact/).
