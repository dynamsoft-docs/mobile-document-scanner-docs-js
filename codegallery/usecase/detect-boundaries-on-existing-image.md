---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
title: Mobile Web Capture - Use Cases - Detect Boundaries on the Existing Image
keywords: Documentation, Mobile Web Capture, Use Cases, Detect Boundaries on the Existing Image
breadcrumbText: Detect Boundaries on the Existing Image
description: Mobile Web Capture Documentation Use Cases Detect Boundaries on the Existing Image
permalink: /codegallery/usecases/detect-boundaries-on-existing-image.html
---

# Detect Boundaries on the Existing Image

This sample demonstrates how to detect the boundaries on the existing image which are from local directory/album.

[Check out it online](https://dynamsoft.github.io/mobile-web-capture/samples/detect-boundaries-on-existing-image/)

In this sample, we would like to achieve the workflow as below.

![Flow chart for detect-boundaries-on-existing-images](/assets/imgs/detect-boundaries-on-existing-images.png)

We’ll build on this skeleton page:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Mobile Web Capture - Detect Boundaries on the Existing Image</title>
</head>
<body>
</body>
<script type="module">
// Write your code here
</script>
</html>
```

## Adding the dependency

This sample is using a CDN to include the SDKs. Please refer to [Adding the dependency - Use a CDN]({{ site.gettingstarted }}add_dependency.html#use-a-cdn).

If you would like to host the resources files on your own server. Please refer to [Adding the dependency - Host yourself]({{ site.gettingstarted }}add_dependency.html#host-yourself).

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
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/dynamsoft-document-viewer@2.0.0/dist/ddv.css">
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

#imageContainer {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: space-around;
    box-sizing: border-box;
    align-items: center;
    flex-direction: column;
    padding: 10px 0px;
}

#imageContainer img {
    width: 80%;
    height: 40%;
    object-fit: contain;
    border:none;
}

#restore {
    display: flex;
    width: 80px;
    height: 40px;
    align-items: center;
    background: #fe8e14;
    justify-content: center;
    color: white;
    cursor: pointer;
    user-select: none;
}
```

## Related SDK initialization

```javascript
//Preloads the Document Normalizer module.
Dynamsoft.Core.CoreModule.loadWasm(["DDN"]);
//Preloads the Document Viewer module.
Dynamsoft.DDV.Core.loadWasm();

// Initialize DDN
await Dynamsoft.License.LicenseManager.initLicense(
    "DLS2eyJoYW5kc2hha2VDb2RlIjoiMjAwMDAxLTEwMjQ5NjE5NyJ9",
    true
);
// Initialize DDV
await Dynamsoft.DDV.Core.init();

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
                            click: "showPerspectiveResult"
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
                        },
                    },
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

`utils.js` content:

```javascript
export function isMobile(){
    return "ontouchstart" in document.documentElement;
}

export function createFileInput(viewer, router){
    const input = document.createElement("input");
    input.accept = "image/png,image/jpeg,image/bmp";
    input.type = "file";
    input.multiple = true;

    input.addEventListener("change", async () => {
        const { files } = input;
        const len = files.length;
        const sourceArray = [];

        for (let i = 0; i < len; i++) {
            const blob = new Blob([files[i]], {
                type: files[i].type,
            });
            const detectResult = await router.capture(blob, "detect-document-boundaries");

            if(detectResult.items.length >0) {
                const quad = [];
                detectResult.items[0].location.points.forEach(p => {
                    quad.push([p.x, p.y]);
                });

                sourceArray.push({
                    fileData: blob,
                    extraPageData:[{
                        index: 0,
                        perspectiveQuad: quad
                    }]
                })
            } else {
                sourceArray.push({
                    fileData: blob,
                })
            }
        }

        if(sourceArray.length > 0) {
            viewer.currentDocument.deleteAllPages();
            viewer.currentDocument.loadSource(sourceArray);
        }

        input.value = null;
        input.files = null;
    },true)

    return input;
}
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

- Register an event in `perspectiveViewer` to display the result image

    ```javascript
    perspectiveViewer.on("showPerspectiveResult", async () => {
        document.getElementById("container").style.display = "none";
        document.getElementById("imageContainer").style.display = "flex";

        const pageData =  await perspectiveViewer.currentDocument.getPageData(perspectiveViewer.getCurrentPageUid());
        // Original image
        document.getElementById("original").src = URL.createObjectURL(pageData.raw.data);
        // Normalized image
        document.getElementById("normalized").src = URL.createObjectURL(pageData.display.data);
    });
    ```


For now, we finish the main workflow, can add the restore function to readjust the boundaries or load a new existing image to adjust its boundaries.

```javascript
document.getElementById("restore").onclick = () => {
    captureViewer.currentDocument.deleteAllPages();
    captureViewer.play();
    document.getElementById("container").style.display = "";
};
```

## Review the complete code

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Mobile Web Capture - Detect Boundaries on the Existing Image</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/dynamsoft-document-viewer@2.0.0/dist/ddv.css">
    <link rel="stylesheet" href="./index.css">
</head>
<body>
    <div id="container"></div>
</body>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-core@3.2.10/dist/core.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-license@3.2.10/dist/license.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-normalizer@2.2.10/dist/ddn.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-capture-vision-router@2.2.10/dist/cvr.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-viewer@2.0.0/dist/ddv.js"></script>
<script type="module">
    import { isMobile, createFileInput } from "./utils.js";

    (async () => {
        //Preloads the Document Normalizer module.
        Dynamsoft.Core.CoreModule.loadWasm(["DDN"]);
        //Preloads the Document Viewer module.
        Dynamsoft.DDV.Core.loadWasm();

        // Initialize DDN
        await Dynamsoft.License.LicenseManager.initLicense(
            "DLS2eyJoYW5kc2hha2VDb2RlIjoiMjAwMDAxLTEwMjQ5NjE5NyJ9",
            true
        );
        // Initialize DDV
        await Dynamsoft.DDV.Core.init();

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
                                click: "showPerspectiveResult"
                            },
                        },
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
                            },
                        },
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

        // Create a document and open it in the perspectiveViewer
        const doc = Dynamsoft.DDV.documentManager.createDocument();
        perspectiveViewer.openDocument(doc.uid);

        // Create an image input with document boundaries detection
        const loadImageInput = createFileInput(perspectiveViewer, router);

        // Register an event in `perspectiveViewer` to add existing image(s)
        perspectiveViewer.on("addNew",() => {
            delete loadImageInput.files;
            loadImageInput.click();
        });

        // Register an event in `perspectiveViewer` to display the result image
        perspectiveViewer.on("showPerspectiveResult", async () => {
            document.getElementById("container").style.display = "none";
            document.getElementById("imageContainer").style.display = "flex";

            const pageData =  await perspectiveViewer.currentDocument.getPageData(perspectiveViewer.getCurrentPageUid());
            // Original image
            document.getElementById("original").src = URL.createObjectURL(pageData.raw.data);
            // Normalized image
            document.getElementById("normalized").src = URL.createObjectURL(pageData.display.data);
        });

        // Restore Button function
        document.getElementById("restore").onclick = () => {
            document.getElementById("container").style.display = "";
            document.getElementById("imageContainer").style.display = "none";
        };
    })();
</script>
</html>
```

## Download the whole project

[Github](https://github.com/Dynamsoft/mobile-web-capture/tree/master/samples/detect-boundaries-on-existing-image) \| [Run](https://dynamsoft.github.io/mobile-web-capture/samples/detect-boundaries-on-existing-image/)

Please note that in order to be compatible with desktop devices as much as possible, some compatibility codes have been added to the whole project code.

`UiConfig` part is organized into `uiConfig.js` and referenced in the core code to minimize the length of the core code.


## Add auxiliary text if necessary

Sometimes, you may want to add some auxiliary text to icons to show better user guidance.

### Refer to

- [Customize Elements' Display Text](https://www.dynamsoft.com/document-viewer/docs/ui/customize/elements.html#display-text)
