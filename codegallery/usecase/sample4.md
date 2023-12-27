---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
title: Document Web Capture from Mobile Camera - Use Cases - Detect boundaries on the existing images
keywords: Documentation, Document Web Capture from Mobile Camera, Use Cases, Detect boundaries on the existing images
breadcrumbText: Detect boundaries on the existing images
description: Document Web Capture from Mobile Camera Documentation Use Cases Detect boundaries on the existing images
permalink: /codegallery/usecases/sample4.html
---

# Detect boundaries on the existing images

This sample demonstrates how to detect the boundaries on the existing images which are from local directory/album. 

Check out [this sample]()

In this sample, we would like to achieve the workflow as below.

![Flow chart for sample4](/assets/imgs/sample4.png)

We’ll build on this skeleton page:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>DWC from Mobile Camera - Detect boundaries on the existing images</title>
</head>
<body>
</body>
<script type="module">
// Write your code here
</script>
</html>
```

## Adding the dependency

Please refer to [Adding the dependency]({{ site.gettingstarted }}add_dependency.html).

## Define necessary HTML elements

For this sample, we define below element.

- Container to hold the viewer

```html
<div id="container"></div>
```

## Link CSS to HTML

`ddv.css` is the necessary css file which defines the viewer style of Dynamsoft Document Viewer.
`index.css` defines the style of elements which is in this sample.

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/dynamsoft-document-viewer@1.0.0/dist/ddv.css">
<link rel="stylesheet" href="./index.css">
```

`index.css` content:

```css
html,body {
    width: 100%;
    height: 100%;
    margin:0;
    padding:0;
    overscroll-behavior-y: none;
}

#container {
    width: 100%;
    height: 100%;
}

.addNewButton {
    background: #fe8e14;
}
```

## Related SDK initialization

```javascript
// Initialize DDV
await Dynamsoft.DDV.setConfig({
    license: "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9",
    engineResourcePath: "https://cdn.jsdelivr.net/npm/dynamsoft-document-viewer@latest/dist/engine",
});

// Initialize DDN
Dynamsoft.License.LicenseManager.initLicense("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
Dynamsoft.CVR.CaptureVisionRouter.preloadModule(["DDN"]);

const router = await Dynamsoft.CVR.CaptureVisionRouter.createInstance();
router.maxCvsSideLength = 99999;
```

## Create a perspective viewer

To review the detected boundaries on the loaded image(s), we will create a perspective viewer. 

- Customize the perspective viewer `UiConfig`
    - Bind click event to "PerspectiveAll" button.
    - Replace the default "RotateRight" button with an "AddNew" button in perspective viewer's footer and bind event to the new button.

    ```javascript
    const newPerspectiveUiConfig = {
        type: Dynamsoft.DDV.Elements.Layout,
        flexDirection: "column",
        children: [
            {
                type: Dynamsoft.DDV.Elements.Layout,
                className: "ddv-perspective-viewer-header-mobile",
                children: [
                    Dynamsoft.DDV.Elements.Blank,
                    Dynamsoft.DDV.Elements.Pagination,
                    {
                        // Bind event for "PerspectiveAll" button
                        // The event will be registered later
                        type: Dynamsoft.DDV.Elements.PerspectiveAll,
                        events: {
                            click: "downloadPDF"
                        }
                    }
                ],
            },
            Dynamsoft.DDV.Elements.MainView,
            {
                type: Dynamsoft.DDV.Elements.Layout,
                className: "ddv-perspective-viewer-footer-mobile",
                children: [
                    Dynamsoft.DDV.Elements.FullQuad,
                    Dynamsoft.DDV.Elements.RotateLeft,
                    {
                        // Replace the default "RotateRight" button with an "AddNew" button in perspective viewer's footer and bind event to the new button
                        // The event will be registered later
                        type: Dynamsoft.DDV.Elements.Button,
                        className: "ddv-load-image2 addNewButton", 
                        events: {
                            click: "addNew"
                        }
                    }
                    Dynamsoft.DDV.Elements.DeleteCurrent,
                    Dynamsoft.DDV.Elements.DeleteAll,
                ],
            },
        ],
    };
    ```

- Create the viewer by using the new `UiConfig`.

    ```javascript
    const perspectiveViewer = new Dynamsoft.DDV.PerspectiveViewer({
        container: "container",
        uiConfig: newPerspectiveUiConfig, // Configure the new UiConfig
        viewerConfig: {
            scrollToLatest: true, // Navigate to the latest image automatically
        }
    });
    ```

- Create a document and open it in the perspective viewer

    ```javascript
    const doc = Dynamsoft.DDV.documentManager.createDocument();
    perspectiveViewer.openDocument(doc.uid);
    ```

## Configure image input function with document boundaries detection

- Step one: The related function code is packaged in `utils.js`, so it should be imported.

```javascript
import { isMobile, createFileInput } from "./utils.js";
```

- Step two: Create the following function

```javascript
const loadImageInput = createFileInput(perspectiveViewer, router);
```

## Configure the workflow

Since the workflow in this sample is very simple, only the two events mentioned above need to be registered.

- Register an event in `perspectiveViewer` to add existing image(s).

    ```javascript
    perspectiveViewer.on("addNew",() => {
        delete loadImageInput.files;
        loadImageInput.click();
    });
    ```

- Register an event in `perspectiveViewer` to download the result image(s) in PDF format.

    ```javascript
    perspectiveViewer.on("downloadPDF",() => {
        perspectiveViewer.currentDocument.saveToPdf({mimeType:"application/octet-stream"}).then((blob) => {
            const url = URL.createObjectURL(blob);
            const a = document.createElement("a");
            a.href = url;
            a.download = `test.pdf`;
            a.click();
        });
    });
    ```

## Review the complete code

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>DWC from Mobile Camera - Detect boundaries on the existing images</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/dynamsoft-document-viewer@1.0.0/dist/ddv.css">
    <link rel="stylesheet" href="./index.css">
</head>
<body>
    <div id="container"></div>
</body>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-viewer@1.0.0/dist/ddv.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-core@3.0.10/dist/core.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-normalizer@2.0.11/dist/ddn.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-capture-vision-router@2.0.11/dist/cvr.js"></script>
<script type="module">
    import { isMobile, createFileInput } from "./utils.js";

    (async () => {
        // Initialize DDV
        await Dynamsoft.DDV.setConfig({
            license: "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9",
            engineResourcePath: "https://cdn.jsdelivr.net/npm/dynamsoft-document-viewer@1.0.0/dist/engine",
        });

        // Initialize DDN
        Dynamsoft.License.LicenseManager.initLicense("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
        Dynamsoft.CVR.CaptureVisionRouter.preloadModule(["DDN"]);

        const router = await Dynamsoft.CVR.CaptureVisionRouter.createInstance();
        router.maxCvsSideLength = 99999;

        // Define new UiConfig for perspecitve viewer
        const newPerspectiveUiConfig = {
            type: Dynamsoft.DDV.Elements.Layout,
            flexDirection: "column",
            children: [
                {
                    type: Dynamsoft.DDV.Elements.Layout,
                    className: "ddv-perspective-viewer-header-mobile",
                    children: [
                        Dynamsoft.DDV.Elements.Blank,
                        Dynamsoft.DDV.Elements.Pagination,
                        {
                            // Bind event for "PerspectiveAll" button
                            // The event will be registered later
                            type: Dynamsoft.DDV.Elements.PerspectiveAll,
                            events: {
                                click: "downloadPDF"
                            }
                        }
                    ],
                },
                Dynamsoft.DDV.Elements.MainView,
                {
                    type: Dynamsoft.DDV.Elements.Layout,
                    className: "ddv-perspective-viewer-footer-mobile",
                    children: [
                        Dynamsoft.DDV.Elements.FullQuad,
                        Dynamsoft.DDV.Elements.RotateLeft,
                        {
                            // Replace the default "RotateRight" button with an "AddNew" button in perspective viewer's footer and bind event to the new button
                            // The event will be registered later
                            type: Dynamsoft.DDV.Elements.Button,
                            className: "ddv-load-image2 addNewButton", 
                            events: {
                                click: "addNew"
                            }
                        }
                        Dynamsoft.DDV.Elements.DeleteCurrent,
                        Dynamsoft.DDV.Elements.DeleteAll,
                    ],
                },
            ],
        };

        // Create a perspective viewer
        const perspectiveViewer = new Dynamsoft.DDV.PerspectiveViewer({
            container: "container",
            uiConfig: newPerspectiveUiConfig, // Configure the new UiConfig
            viewerConfig: {
                scrollToLatest: true, // Navigate to the latest image automatically
            }
        });

        // Register an event in `perspectiveViewer` to add existing image(s)
        perspectiveViewer.on("addNew",() => {
            delete loadImageInput.files;
            loadImageInput.click();
        });

        // Register an event in `perspectiveViewer` to download the result image(s) in PDF format
        perspectiveViewer.on("downloadPDF",() => {
            perspectiveViewer.currentDocument.saveToPdf({mimeType:"application/octet-stream"}).then((blob) => {
                const url = URL.createObjectURL(blob);
                const a = document.createElement("a");
                a.href = url;
                a.download = `test.pdf`;
                a.click();
            });
        });
    })();
</script>
</html>
```

## Download the whole project

Please note that in order to be compatible with desktop devices as much as possible, some compatibility codes have been added to the whole project code.

`UiConfig` part is organized into `uiConfig.js` and referenced in the core code to minimize the length of the core code.


## Add auxiliary text if necessary

Sometimes, you may want to add some auxiliary text to icons to show better user guidance.

### Refer to

- [Customize Elements' Display Text](https://officecn.dynamsoft.com:808/document-viewer/docs/ui/customize/elements.html#display-text)