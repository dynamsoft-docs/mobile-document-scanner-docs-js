---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: false
breadcrumbText: Developer Guide
title: Mobile Document Scanner JS Edition - Scan Single-Page Documents
keywords: Documentation, Mobile Document Scanner, Web, JS Edition, Dynamsoft Document Scanner,
description: Mobile Document Scanner JS Edition User Guide
---

# Scan Single-Page Documents with Mobile Document Scanner

> [!TIP]
> Read the [Introduction]({{ site.introduction }}index.html) for common use cases, an overview of the SDK architecture, and system requirements.

Dynamsoft's **Mobile Document Scanner JavaScript Edition (MDS)** is a web SDK designed for scanning single-page documents. MDS captures images of the documents and enhances their quality to professional standards, making it an ideal tool for mobile document scanning.

> [!NOTE]
> See it in action with the [Mobile Document Scanner Demo](https://demo.dynamsoft.com/document-scanner/).

This guide walks you through building a web application that scans single-page documents using **MDS** with pre-defined configurations.

<!--  Keep TOC only for npm /github as readme
**Table of Contents**
- [License](#license)
  - [Get a Trial License](#get-a-trial-license)
  - [Get a Full License](#get-a-full-license)
- [Quick Start](#quick-start)
  - [Build from Source](#build-from-source)
  - [Use Precompiled Script](#use-precompiled-script)
  - [Self-Host Resources](#self-host-resources)
- [Hello World Sample Explained](#hello-world-sample-explained)
  - [Reference MDS](#reference-dds)
  - [Instantiate MDS](#instantiate-dds)
  - [Launch MDS](#launch-dds)
  - [Display the Result](#display-the-result)
- [Custom Usage](#custom-usage)
  - [DocumentScannerConfig Overview](#documentscannerconfig-overview)
  - [Workflow Customization](#workflow-customization)
  - [View-Based Customization](#view-based-customization)
- [Next Step](#next-step) -->

## License

### Get a Trial License

If you do not have a trial license for **MDS**, you can request one here:

{% include trialLicense.html %}

The trial license can be renewed twice for a total of two months of free access.

### Get a Full License

[Contact us](https://www.dynamsoft.com/company/contact/) to purchase a full license.

## Quick Start

To use the **Mobile Document Scanner**, first obtain its library files. You can acquire them from one of the following sources:

1. [**GitHub**](https://github.com/Dynamsoft/document-scanner-javascript) – Contains the source files for the **MDS** SDK, which can be compiled into library files.
2. [**npm**](https://www.npmjs.com/package/dynamsoft-document-scanner) – Provides precompiled library files via npm for easier installation.
3. [**CDN**](https://cdn.jsdelivr.net/npm/dynamsoft-document-scanner) – Delivers precompiled library files through a CDN for quick and seamless integration.

You can choose one of the following methods to set up a Hello World page:

1. **Build from source** – Download the source files from GitHub and compile the library files yourself.
2. **Use precompiled scripts** – Use the precompiled resource scripts from npm or the CDN for a quicker setup.
3. **Self-host resources** - Self-host both MDS and its dependencies on your web server.

<div class="multi-panel-switching-prefix"></div>

<div class="multi-panel-start"></div>
<div class="multi-panel-title">Build from Source</div>

### Build from Source

This method retrieves all **MDS** source files from its [GitHub Repository](https://github.com/Dynamsoft/document-scanner-javascript), compiles them into a distributable package, and then runs a _ready-made_ Hello World sample page included in the repository:

1. Download **MDS** from [GitHub](https://github.com/Dynamsoft/document-scanner-javascript) as a compressed folder.

2. Extract the contents of the archive, and open the extracted directory in a code editor.

3. Set your [license key](#get-a-trial-license) in the Hello World sample:

    1. Open the Hello World sample at ([`/samples/hello-world.html`](https://github.com/Dynamsoft/document-scanner-javascript/blob/main/samples/hello-world.html)).

    2. Search for `"YOUR_LICENSE_KEY_HERE"`, then replace it with your actual license key.

4. In the terminal, navigate to the project root directory and run the following to install project dependencies:

    ```shell
    npm install
    ```

5. After installing dependencies, build the project by running:

    ```shell
    npm run build
    ```

6. Start the local server by running the following to serve the project locally:

    ```shell
      npm run serve
    ```

    Once the server is running, open the application in a browser using the address provided in the terminal output after running `npm run serve`.

    > [!TIP]
    > See the server configuration details in [`/dev-server/index.js`](https://github.com/Dynamsoft/document-scanner-javascript/blob/main/dev-server/index.js).

<div class="multi-panel-end"></div>

<div class="multi-panel-start"></div>
<div class="multi-panel-title">Use Precompiled Script</div>

### Use Precompiled Script

We publish **MDS** library files on [npm](https://www.npmjs.com/package/dynamsoft-document-scanner) to make them simple to reference from a CDN.

To use the precompiled script, simply include the following URL in a `<script>` tag:

```html
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-scanner@1.2/dist/dds.bundle.js"></script>
```

Below is the complete Hello World sample page that uses this precompiled script from a CDN.

> [!TIP]
> The code is identical to the [`/samples/hello-world.html`](https://github.com/Dynamsoft/document-scanner-javascript/blob/main/samples/hello-world.html) file mentioned in the [Build from Source](#build-from-source) section, except for the script source.

> [!WARNING]
> **Remember** to replace `"YOUR_LICENSE_KEY_HERE"` with your actual license key.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Mobile Document Scanner - Hello World</title>
    <script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-scanner@1.2/dist/dds.bundle.js"></script>
  </head>
  <body>
    <h1 style="font-size: large">Mobile Document Scanner</h1>
    <div id="results"></div>
    <script>
      const resultContainer = document.querySelector("#results");
      // Instantiate a Document Scanner Object
      const documentScanner = new Dynamsoft.DocumentScanner({
        license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
      });
      (async () => {
        // Launch the scanner and wait for the result
        const result = await documentScanner.launch();
        console.log(result);

        // Clear the result container and display the scanned result as a canvas
        if (result?.correctedImageResult) {
          resultContainer.innerHTML = ""; // Clear placeholder content
          const canvas = result.correctedImageResult.toCanvas();
          resultContainer.appendChild(canvas);
        } else {
          resultContainer.innerHTML =
            "<p>No image scanned. Please try again.</p>";
        }
      })();
    </script>
  </body>
</html>
```

To run the sample, create a new file called `hello-world.html`, then copy and paste the code above into the file. Next, serve the page directly by deploying it to a server.

If you are using VS Code, a quick and easy way to serve the project is using the [Live Server (Five Server) VSCode extension](https://marketplace.visualstudio.com/items?itemName=yandeu.five-server). Simply install the extension, open the `hello-world.html` file in the editor, and click "Go Live" in the bottom right corner of the editor. This will serve the application at `http://127.0.0.1:5500/hello-world.html`.

Alternatively, you can use other methods like `IIS` or `Apache` to serve the project, though we skip those here for brevity.

<div class="multi-panel-end"></div>

<div class="multi-panel-start"></div>
<div class="multi-panel-title">Self-Host Resources</div>

### Self-Host Resources

By default, the MDS library (whether pre-compiled or self-compiled) fetches resource files (Dynamsoft `node` dependencies and an HTML UI template) from CDNs. Self-hosting library resources gives you full control over hosting your application. Rather than using CDNs to serve these resources, you can instead host these resources on your own servers to deliver to your users directly when they use your application.

#### Download Resources

First, download a copy of the resources:

1. Download **MDS** from [GitHub](https://github.com/Dynamsoft/document-scanner-javascript) as a compressed folder.

2. Extract the contents of the archive, and open the extracted directory in a code editor.

3. Set your [license key](#get-a-trial-license) in the Hello World sample:

    1. Open the Hello World sample at ([`/samples/hello-world.html`](https://github.com/Dynamsoft/document-scanner-javascript/blob/main/samples/hello-world.html)).

    2. Search for `"YOUR_LICENSE_KEY_HERE"`, then replace it with your actual license key.

4. In the terminal, navigate to the project root directory and run the following to install project dependencies:

      ```shell
      npm install
      ```

#### Point to Resources

The library uses [`engineResourcePaths`]({{ site.api }}index.html#engineresourcepaths) to locate required Dynamsoft `node` dependencies by pointing to the location of the resources on your web server. The library also uses `scannerViewConfig.cameraEnhancerUIPath` similarly to set the path for the HTML UI template of the `ScannerView`. Later steps will place both the `node` dependencies and the HTML template in the local `dist` directory. Therefore, set `engineResourcePaths` in the MDS constructor to point to the local `dist` directory (along with setting your license key, and all other configurations):

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
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
});
```

API Reference:

- [`DocumentScanner()`]({{ site.api }}index.html#documentscanner)
- [`DocumentScannerConfig`]({{ site.api }}index.html#documentscannerconfig)
- [`DocumentScannerViewConfig`]({{ site.api }}index.html#documentscannerviewconfig)
- [`engineResourcePaths`]({{ site.api }}index.html#engineresourcepaths)
- [`cameraEnhancerUIPath`]({{ site.api }}index.html#cameraenhanceruipaths)

#### Modify the Build Script

Update the `scripts` section in `package.json` to automatically copy resources to the output `dist` directory during the build process.

```json
"scripts": {
    "serve": "node dev-server/index.js",
    "build": "rollup -c && npm run copy-libs",
    "copy-libs": "npx mkdirp dist/libs && npx cpx \"node_modules/dynamsoft-*/**/*\" dist/libs/ --dereference",
    "build:production": "rollup -c --environment BUILD:production"
},
```

#### Build the Project

Build the project by running:

```shell
npm run build
```

#### Serve the Project Locally

Start the local development server by running:

```shell
npm run serve
```

Once the server is running, open the application in a browser using the address provided in the terminal output.

#### Serve over HTTPS

**Place the `dist` directory** onto your web server for to serve the web application. When deploying your web application for production, you must serve it over a **secure HTTPS connection**. We require this for the following reasons:

1. **Browser Security Restrictions** – Most browsers only allow access to camera video streams in a secure context.

    > [!NOTE]
    > Some browsers like Chrome may grant access to camera video streams for `http://127.0.0.1`, `http://localhost`, or even pages opened directly from the local file system (`file:///...`). This can be helpful during development and testing.

2. **Dynamsoft License Requirements** – A secure context is required for **Dynamsoft licenses** to function properly.

#### Set MIME Type

Certain legacy web application servers may lack support for the `application/wasm` mimetype for .wasm files. To address this, you have two options:

1. Upgrade your web application server to one that supports the `application/wasm` mimetype.
2. Manually define the mimetype on your server by setting the MIME type for `.wasm` as `application/wasm`. This allows the user's browser to correctly processes resource files. Different web servers have their own way of configuring the MIME type. Here are instructions for [Apache](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Apache_Configuration_htaccess#media_types_and_character_encodings), [IIS](https://docs.microsoft.com/en-us/iis/configuration/system.webserver/staticcontent/mimemap), and [NGINX](https://www.nginx.com/resources/wiki/start/topics/examples/full/#mime-types).

#### Resource Caching

The `wasm` resource files are relatively large and may take quite a few seconds to download. We recommend setting a longer cache time for these resource files to maximize the performance of your web application using the `Cache-Control` HTTP header. For example, use the `max-age` directive to cache resources for a specified time in seconds:

```
Cache-Control: max-age=31536000
```

Reference:
[`Cache-Control`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control)

<div class="multi-panel-end"></div>

<div class="multi-panel-switching-end"></div>

## Hello World Sample Explained

Here we walk through the code in the Hello World sample to explain how it works.

> [!TIP]
> You can also view the full code by visiting the [MDS JS Hello World Sample on Github](https://github.com/Dynamsoft/document-scanner-javascript/blob/main/samples/hello-world.html).

### Reference MDS

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Mobile Document Scanner - Hello World</title>
    <script src="../dist/dds.bundle.js"></script>
    <!--Alternatively, reference the script from CDN
    <script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-scanner@1.2/dist/dds.bundle.js"></script>
    -->
  </head>
</html>
```

In this step, MDS is referenced using a relative local path in the `<head>` section of the HTML.

```html
<script src="../dist/dds.bundle.js"></script>
```

Alternatively, the script can be referenced from a CDN:

```html
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-scanner@1.2/dist/dds.bundle.js"></script>
```

**MDS** wraps all its dependency scripts, so a **MDS** project only needs to include **MDS** itself as a single script. No additional dependency scripts are required.

> [!WARNING]
> Even if you reference the script locally, supporting resources like `.wasm` engine files are still loaded from the CDN at runtime. If you require a **fully offline setup**, follow the [quick start instructions to self-host resources](#quick-start).

### Instantiate MDS

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
  license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
});
```

API Reference:

- [`DocumentScanner()`]({{ site.api }}index.html#documentscanner)

This step creates the **MDS** UI, which occupies the entire visible area of the browser window by default when launched. If needed, you can specify a container to restrict the UI's size. For more details, refer to [Confine DocumentScanner UI to a Specific Container](#example-1-confine-documentscanner-ui-to-a-specific-container).

> [!WARNING]
> Instantiating the `DocumentScanner` requires a valid license key.

### Launch MDS

```javascript
const result = await documentScanner.launch();
```

API Reference:

- [`launch()`]({{ site.api }}index.html#launch)

This step launches the user into the document scanning workflow, beginning in the `DocumentScannerView`, where they can scan a document using one of three methods:

- Option 1: Manually scan by pressing the **shutter button**.
- Option 2: Enable "**Smart Capture**" - the scanner will automatically capture an image once a document is detected.
- Option 3: Enable "**Auto Crop**" - the scanner will automatically capture an image, detect the document, and crop it out of the video frame.

> [!TIP]
> For Options 1 & 2: The user is directed to `DocumentCorrectionView` to review detected document boundaries and make any necessary adjustments before applying corrections. Afterward, they proceed to `DocumentResultView`.
>
> For Option 3: The `DocumentCorrectionView` step is skipped. Image correction is applied automatically, and the user is taken directly to `DocumentResultView`.

In `DocumentResultView`, if needed, the user can return to `DocumentCorrectionView` to make additional adjustments or press "Re-take" to restart the scanning process.

### Display the Result

The workflow returns a scanned image object of type `CorrectedImageResult`. To display the scanned `result` image, we use a `<div>` in the `<body>`:

```html
<body>
  <h1 style="font-size: large">Mobile Document Scanner</h1>
  <div id="results"></div>
</body>
```

API Reference:

- [`DocumentResult`]({{ site.api }}index.html#documentresult)

The following code clears the result container and displays the scanned result as a canvas:

```javascript
if (result?.correctedImageResult) {
  resultContainer.innerHTML = "";
  const canvas = result.correctedImageResult.toCanvas();
  resultContainer.appendChild(canvas);
} else {
  resultContainer.innerHTML = "<p>No image scanned. Please try again.</p>";
}
```

## Custom Usage

This section builds on the Hello World sample to demonstrate how to configure **MDS**, typically by adjusting the `DocumentScannerConfig` object.

### `DocumentScannerConfig` Overview

[`DocumentScannerConfig`]({{ site.api }}index.html#documentscannerconfig) is the primary configuration object for customizing **MDS**. It includes the following properties:

1. `license`: The license key.
2. `container`: The HTML container for the entire workflow. If not specified (like in the Hello World Sample), one is created automatically.
3. `showCorrectionView`: Configures whether or not to show `DocumentCorrectionView`.
4. `showResultView`: Configures whether or not to show `DocumentResultView`.
5. `scannerViewConfig`: Configures the main scanner view with the following properties:
   1. `container`: The HTML container for the `DocumentScannerView`.
   2. `cameraEnhancerUIPath`: Path to the UI definition file (.html) for the `DocumentScannerView`.
   3. `enableAutoCropMode`: sets the default Auto-Crop mode.
   4. `enableSmartCaptureMode`: sets the default Smart Capture mode.
   5. `scanRegion`: sets the scan region within the viewfinder for document scanning.
   6. `minVerifiedFramesForSmartCapture`: sets minimum number of camera frames to detect document boundaries on Smart Capture mode.
   7. `showSubfooter`: sets the visibility of the mode selector menu.
   8. `showPoweredByDynamsoft`: sets the visibility of the Dynamsoft branding message.
6. `correctionViewConfig`: Configures the document correction view.
   1. `container`: The HTML container for the `DocumentCorrectionView`.
   2. `toolbarButtonsConfig`: Configures the appearance and labels of the buttons for the `DocumentCorrectionView` UI.
   3. `onFinish`: Handler called when the user clicks the "Apply" button.
7. `resultViewConfig`: Configures the result view with the following properties:
   1. `container`: The HTML container for the `DocumentResultView`.
   2. `toolbarButtonsConfig`: Configures the appearance and labels of the buttons for the `DocumentResultView` UI.
   3. `onDone`: Handler called when the user clicks the "Done" button.
   4. `onUpload`: Handler called when the user clicks the "Upload" button.
8. `templateFilePath`: Path to a Capture Vision template for scanning configuration. Typically not needed as the default template is used.
9. `utilizedTemplateNames`: Template names for detection and correction. Typically not needed as the default template is used.
10. `engineResourcePaths`: Paths to extra resources such as `.wasm` engine files.

We will discuss two main methods of customizing **MDS** with `DocumentScannerConfig`:

1. [**Workflow Customization**](#workflow-customization): Through container definitions.
2. [**View-Based Customization**](#view-based-customization): Through configuration objects.

The customization examples below will build on the Hello World code from the [previous section](#use-precompiled-script). The only change required is adjusting the constructor argument.

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
  license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
  // Add more arguments
});
```

### Workflow Customization

In the Hello World sample, we use the complete workflow with minimum configuration:

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
  license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
});
// Launch the scanner and wait for the result
const result = await documentScanner.launch();
```

In this case, **MDS** automatically creates "containers" for its **Views**. In this section we discuss a few ways to adjust the **MDS** workflow.

#### Example 1: Confine DocumentScanner UI to a Specific Container

As long as the `DocumentScanner` container is assigned, **MDS** confines its **Views** within that container.

> [!NOTE]
> Containers assigned to its constituent **Views** will be ignored.

```html
<div id="myDocumentScannerContainer" style="width: 80vw; height: 80vh;"></div>
```

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
  license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
  container: document.getElementById("myDocumentScannerContainer"), // Use this container for the full workflow
  scannerViewConfig: {
    container: document.getElementById("myDocumentScannerViewContainer"), // This container is ignored
  },
});
```

API Reference:

- [`DocumentScanner()`]({{ site.api }}index.html#documentscanner)
- [`DocumentScannerConfig`]({{ site.api }}index.html#documentscannerconfig)

#### Example 2: Only Show `DocumentScannerView`

In some cases, `DocumentResultView` and `DocumentCorrectionView` may not be needed, so they can be hidden:

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
  license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
  showResultView: false,
  showCorrectionView: false,
});
```

API Reference:

- [`DocumentScanner()`]({{ site.api }}index.html#documentscanner)
- [`DocumentScannerConfig`]({{ site.api }}index.html#documentscannerconfig)

#### Example 3: Specify Individual View Containers

If only the `DocumentScannerView`, `DocumentResultView`, and `DocumentCorrectionView` containers are provided without the `DocumentScanner` container, **MDS** renders the full workflow using these three containers.

```html
<div
  id="myDocumentScannerViewContainer"
  style="width: 80vw; height: 80vh"
></div>
<div
  id="myDocumentCorrectionViewContainer"
  style="width: 80vw; height: 80vh"
></div>
<div id="myScanResultViewContainer" style="width: 80vw; height: 80vh"></div>
```

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
  license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
  scannerViewConfig: {
    container: document.getElementById("myDocumentScannerViewContainer"),
  },
  correctionViewConfig: {
    container: document.getElementById("myDocumentCorrectionViewContainer"),
  },
  resultViewConfig: {
    container: document.getElementById("myScanResultViewContainer"),
  },
});
```

API Reference:

- [`DocumentScanner()`]({{ site.api }}index.html#documentscanner)
- [`DocumentScannerConfig`]({{ site.api }}index.html#documentscannerconfig)

#### Example 4: Scan Static Image Directly

To scan an image file directly without opening the Scanner View at all, you can pass a `File` object to `launch()`. As an example, select an image file from the local disk:

```html
<input type="file" id="initialFile" accept="image/png,image/jpeg" />
```

Then get the input file as a `File` object, and pass that file object to `launch()` MDS with:

```js
document.getElementById("initialFile").onchange = async function () {
  const files = Array.from(this.files || []);
  if (files.length) {
    const result = await documentScanner.launch(files[0]);
    console.log(result);

    // Clear the result container and display the scanned result as a canvas
    if (result?.correctedImageResult) {
      resultContainer.innerHTML = ""; // Clear placeholder content
      const canvas = result.correctedImageResult.toCanvas();
      resultContainer.appendChild(canvas);
    } else {
      resultContainer.innerHTML = "<p>No image scanned. Please try again.</p>";
    }
  }
};
```

This bypasses the Scanner View entirely and brings up the Correction View as the first View, after having detected document boundaries on the static image. The user can proceed through the rest of the workflow and further alter the document boundaries, re-take another image (to open up the Scanner View), etc.

> [!IMPORTANT]
> `launch()` can accept images or PDFs. If launching with a PDF, MDS will **only process the first page**.

#### Example 5: Configure Scan Modes

The Document Scanner View comes with three scan modes:

1. Border Detection
2. Auto-Crop
3. Smart Capture

By default, Border Detection mode is enabled upon entering the Scanner View, while the other two are turned off by default. The user can then enable them by clicking their respective icons in the scanning mode sub-footer. From the `DocumentScannerViewConfig` interface, you can:

1. Set the default state of Auto-Crop mode with `enableAutoCropMode`
2. Set the default state of Smart Capture mode with `enableSmartCaptureMode`
3. Set the visibility of the scanning mode sub-footer with `showSubfooter`

> [!NOTE]
> Border Detection Mode is always enabled upon the Scanner View, and the scanning sub-footer is visible by default.

For example, the following config enables all three scanning modes and hides the scanning mode sub-footer to prevent the viewer from changing or viewing the scanning modes:

```js
const documentScanner = new Dynamsoft.DocumentScanner({
    license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
    scannerViewConfig: {
        enableAutoCropMode: true,
        enableSmartCaptureMode: true,
        false,
}});
```

API Reference:

- [`DocumentScanner()`]({{ site.api }}index.html#documentscanner)
- [`DocumentScannerConfig`]({{ site.api }}index.html#documentscannerconfig)
- [`DocumentScannerViewConfig`]({{ site.api }}index.html#documentscannerviewconfig)

### View-Based Customization

In addition to modifying the workflow, individual Views can be customized with configuration options for UI styling, button settings, and event handling.

#### `DocumentScannerView` Configuration

##### Customizing the `DocumentScannerView` UI

Consider the following configuration interface used for customizing the `DocumentScannerView`:

```javascript
interface DocumentScannerViewConfig {
    container?: HTMLElement;
    templateFilePath?: string;
    cameraEnhancerUIPath?: string;
}
```

We previously covered `container` in [Workflow Customization](#workflow-customization), and changing `templateFilePath` is usually not required. Now, let's focus on `cameraEnhancerUIPath`.

> [!TIP]
> If **MDS** performance does not meet your needs in your usage scenario, you may require a customized algorithm template for better results. In this case, please contact our experienced [Technical Support Team](https://www.dynamsoft.com/company/contact/) to discuss your requirements. They will help tailor a suitable template for you, which you can then apply by updating `templateFilePath`.

By default, `cameraEnhancerUIPath` points to a file hosted on the jsDelivr CDN:
[https://cdn.jsdelivr.net/npm/dynamsoft-document-scanner@1.2/dist/document-scanner.ui.html](https://cdn.jsdelivr.net/npm/dynamsoft-document-scanner@1.2/dist/document-scanner.ui.html).

This file defines the UI for `DocumentScannerView`. However, since files on the CDN **cannot be modified directly**, you need to use a **local version** to customize the UI. `cameraEnhancerUIPath` is used to specify the local version.

##### Steps to Customize the UI for `DocumentScannerView`

1. Follow the instructions in [Build from Source](#build-from-source) to obtain the source files for **MDS**.

2. Edit `/src/document-scanner.ui.html` to apply your customizations.

3. Build the project to generate the updated file in `/dist/document-scanner.ui.html`:

    ```shell
    npm run build
    ```

4. Update the configuration to use the local file instead of the CDN version:

    ```javascript
    const documentScanner = new Dynamsoft.DocumentScanner({
      license: "YOUR_LICENSE_KEY_HERE", // Replace with your actual license key
      scannerViewConfig: {
        cameraEnhancerUIPath: "../dist/document-scanner.ui.html", // Use the local file
      },
    });
    ```

API Reference:

- [`DocumentScanner()`]({{ site.api }}index.html#documentscanner)
- [`DocumentScannerConfig`]({{ site.api }}index.html#documentscannerconfig)
- [`DocumentScannerViewConfig`]({{ site.api }}index.html#documentscannerviewconfig)

##### Customizing the Scanning Region

We can customize the scanning region in the viewfinder with the `scanRegion` property in the configuration object:

```js
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

API Reference:

[`ScanRegion`]({{ site.api }}index.html#scanregion)

Here is how the scanning region is set:

1. Use the `ratio` property to set the height-to-width of the rectangular scanning region, then scale the rectangle up to fit within the viewfinder.
2. Translate the rectangular up by the number of pixels specified by `regionBottomMargin`.
3. Create a visual border for the scanning region boundary on the viewfinder with a given stroke width in pixels, and a stroke color.

For example:

```js
scanRegion {
  ratio: {
    width: 2;
    height: 3;
  };
  regionBottomMargin: 20;
  style: {
    strokeWidth: 3;
    strokeColor: "green";
  };
}
```

This creates a scan region with a height-to-width ratio of 3:2, translated upwards by 20 pixels, with a green, 3 pixel-wide border in the viewfinder.

#### `DocumentCorrectionView` Configuration

Consider the following configuration interface used for customizing the `DocumentCorrectionView`:

```javascript
interface DocumentCorrectionViewConfig {
    container?: HTMLElement;
    toolbarButtonsConfig?: DocumentCorrectionViewToolbarButtonsConfig;
    onFinish?: (result: DocumentScanResult) => void;
}
```

`container` is covered in [Workflow Customization](#workflow-customization). Below we discuss the other two properties.

##### Styling Buttons

The `toolbarButtonsConfig` property of type `DocumentCorrectionViewToolbarButtonsConfig` customizes the appearance and functionality of the UI buttons. Here is its definition:

```javascript
type ToolbarButtonConfig = Pick<ToolbarButton, "icon" | "label" | "isHidden">;
interface DocumentCorrectionViewToolbarButtonsConfig {
    fullImage?: ToolbarButtonConfig;
    detectBorders?: ToolbarButtonConfig;
    apply?: ToolbarButtonConfig;
}
```

We can use it to change the icon and label of each of the three buttons individually or even hide the buttons. Below is an example that sets a custom label and image icon for the "Detect Borders" button and hides the "Full Image" button:

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
  license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
  correctionViewConfig: {
    toolbarButtonsConfig: {
      fullImage: {
        isHidden: true,
      },
      detectBorders: {
        icon: "path/to/new_icon.png", // Change to the actual path of the new icon
        label: "Custom Label",
      },
    },
  },
});
```

API Reference:

- [`DocumentScanner()`]({{ site.api }}index.html#documentscanner)
- [`DocumentScannerConfig`]({{ site.api }}index.html#documentscannerconfig)
- [`DocumentCorrectionViewConfig`]({{ site.api }}index.html#documentcorrectionviewconfig)

##### Customizing Apply Button Callback

The `onFinish` callback triggers after the user's corrections have been applied. For example, the code below displays the corrected image in a `resultContainer` after the user clicks "Apply":

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
  license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
  correctionViewConfig: {
    onFinish: (result) => {
      const canvas = result.correctedImageResult.toCanvas();
      resultContainer.appendChild(canvas);
    },
  },
});
```

API Reference:

- [`DocumentScanner()`]({{ site.api }}index.html#documentscanner)
- [`DocumentScannerConfig`]({{ site.api }}index.html#documentscannerconfig)
- [`DocumentCorrectionViewConfig`]({{ site.api }}index.html#documentcorrectionviewconfig)

#### `DocumentResultView` Configuration

Consider the following configuration interface used for customizing the `DocumentResultView`:

```javascript
interface DocumentResultViewConfig {
  container?: HTMLElement;
  toolbarButtonsConfig?: DocumentResultViewToolbarButtonsConfig;
  onDone?: (result: DocumentResult) => Promise<void>;
  onUpload?: (result: DocumentResult) => Promise<void>;
}
```

Like with `DocumentCorrectionView`, we'll look at `toolbarButtonsConfig`, `onDone` and `onUpload`.

##### Styling Buttons

The `toolbarButtonsConfig` property, of type `DocumentResultViewToolbarButtonsConfig`, customizes the appearance and functionality of the UI buttons. Here is its definition:

```javascript
type ToolbarButtonConfig = Pick<ToolbarButton, "icon" | "label" | "isHidden">;
interface interface DocumentResultViewToolbarButtonsConfig {
  retake?: ToolbarButtonConfig;
  correct?: ToolbarButtonConfig;
  share?: ToolbarButtonConfig;
  upload?: ToolbarButtonConfig;
  done?: ToolbarButtonConfig;
}
```

We can use it to change the icon and label of each of the three buttons individually or even hide them.
Below is an example that sets a custom label and image icon for the "retake" button and hides the "share" button:

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
  license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
  resultViewConfig: {
    toolbarButtonsConfig: {
      retake: {
        icon: "path/to/new_icon.png", // Change to the actual path of the new icon
        label: "Custom Label",
      },
      share: {
        isHidden: true,
      },
    },
  },
});
```

API Reference:

- [`DocumentScanner()`]({{ site.api }}index.html#documentscanner)
- [`DocumentScannerConfig`]({{ site.api }}index.html#documentscannerconfig)
- [`DocumentResultViewConfig`]({{ site.api }}index.html#documentresultviewconfig)

##### Customizing the "Done" Button Callback

The `onDone` callback triggers when the "Done" button is pressed. For example, the code below displays the result image in a `resultContainer` after the user clicks "Done":

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
  license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
  resultViewConfig: {
    onDone: async (result) => {
      const canvas = result.correctedImageResult.toCanvas();
      resultContainer.appendChild(canvas);
    },
  },
});
```

API Reference:

- [`DocumentScanner()`]({{ site.api }}index.html#documentscanner)
- [`DocumentScannerConfig`]({{ site.api }}index.html#documentscannerconfig)
- [`DocumentResultViewConfig`]({{ site.api }}index.html#documentresultviewconfig)

##### Customizing the "Upload" Button

The `onUpload` callback triggers when the "Upload" button is pressed. Note that the Upload button _only appears_ if a callback function is defined for `onUpload`; the button remains hidden otherwise.

The following example demonstrates how to upload the result image to a server:

> [!TIP]
> If you followed the steps in [Build from Source](#build-from-source) and are still using the predefined Express server setup, the following upload code will work correctly. The image will be uploaded directly to the dev server as "uploadedFile.png". See the server configuration details in [`/dev-server/index.js`](https://github.com/Dynamsoft/document-scanner-javascript/blob/main/dev-server/index.js).

```javascript
const documentScanner = new Dynamsoft.DocumentScanner({
  license: "YOUR_LICENSE_KEY_HERE", // Replace this with your actual license key
  resultViewConfig: {
    onUpload: async (result) => {
      const host = window.location.origin;
      const blob = await result.correctedImageResult.toBlob();
      // Create form data
      const formData = new FormData();
      formData.append("uploadFile", blob, "uploadedFile.png");
      // Upload file
      const response = await fetch(
        `${host}/upload`, // Change this to your actual upload URL
        {
          method: "POST",
          body: formData,
        },
      );
    },
  },
});
```

API Reference:

- [`DocumentScanner()`]({{ site.api }}index.html#documentscanner)
- [`DocumentScannerConfig`]({{ site.api }}index.html#documentscannerconfig)
- [`DocumentResultViewConfig`]({{ site.api }}index.html#documentresultviewconfig)

## Next Step

**MDS** is a fully functional, ready-to-use **single page** scanning SDK with built-in UI layouts. There are two options which extend the features of MDS:

1. To scan multi-page documents as PDFs, please contact [Dynamsoft Support](https://www.dynamsoft.com/company/contact/) for further information.

2. For multi-page and multi-document processing, as well as advanced editing features, we developed **Mobile Web Capture (MWC)**. Read on to learn how to use this web-based wrapper SDK in the [**Mobile Web Capture User Guide**]({{ site.code-gallery }}mobile-web-capture/index.html).
