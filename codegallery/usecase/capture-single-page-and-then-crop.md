---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
title: Mobile Web Capture - Use Cases - Capture Single Page and Then Crop
keywords: Documentation, Mobile Web Capture, Use Cases, Capture Single Page and Then Crop
breadcrumbText: Capture Single Page and Then Crop
description: Mobile Web Capture Documentation Use Cases Capture Single Page and Then Crop
permalink: /codegallery/usecases/capture-single-page-and-then-crop.html
---

# Capture Single Page and Then Crop

This sample adds a perspective viewer based on HelloWorld solution to review and adjust the detected boundaries on the captured image.

[Check out it online](https://dynamsoft.github.io/mobile-web-capture/samples/review-adjust-detected-boundaries/)

In this sample, we would like to achieve the workflow as below.

![Flow chart for review-adjust-detected-boundaries](/assets/imgs/review-adjust-detected-boundaries.png)

Since this sample is based on HelloWorld, the basic steps are introduced in [Creating HelloWorld]({{ site.gettingstarted }}helloworld.html) chapter, this chapter will not be further elaborated.

## Create a perspective viewer

- Customize the viewer's `UiConfig` based on the [default one](https://www.dynamsoft.com/document-viewer/docs/ui/default_ui.html#perspective-viewer) to implement the workflow.

    ```javascript
    const newPerspectiveUiConfig = {
        type: Dynamsoft.DDV.Elements.Layout,
        flexDirection: "column",
        children: [
            {
                type: Dynamsoft.DDV.Elements.Layout,
                className: "ddv-perspective-viewer-header-mobile",
                children: [
                    {   
                        // Add a "Back" button in perspective viewer's header and bind the event to go back to the capture viewer
                        // The event will be registered later
                        type: Dynamsoft.DDV.Elements.Button,
                        className: "ddv-button-back",
                        events:{
                            click: "backToCaptureViewer"
                        }
                    },
                    Dynamsoft.DDV.Elements.Pagination,
                    {
                        // Bind event for "PerspectiveAll" button
                        // The event will be registered later
                        type: Dynamsoft.DDV.Elements.PerspectiveAll,
                        events:{
                            click: "done"
                        }
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
                    Dynamsoft.DDV.Elements.RotateRight,
                    {
                        // Bind event for "DeleteCurrent" button
                        // The event will be registered later
                        type: Dynamsoft.DDV.Elements.DeleteCurrent,
                        events: {
                            click: "noImageBack"
                        },
                    },
                    {
                        // Bind event for "DeleteAll" button
                        // The event will be registered later
                        type:Dynamsoft.DDV.Elements.DeleteAll,
                        events: {
                            click: "noImageBack"
                        },
                    }
                ],
            },
        ],
    };
    ```

- Create the viewer by using the new `UiConfig`.

    ```javascript
    // Create a perspective viewer
    const perspectiveViewer = new Dynamsoft.DDV.PerspectiveViewer({
        container: "container",
        groupUid: captureViewer.groupUid, // Data synchronisation with the capture viewer
        uiConfig: newPerspectiveUiConfig, // Configure the new UiConfig
        viewerConfig:{
            scrollToLatest: true, // Navigate to the latest image automatically
        }
    });
    ```

- Since this viewer only shows when captured an image, it should be hidden at first.

    ```javascript
    perspectiveViewer.hide();
    ```

## Configure the workflow

- Define a function to control the viewers' visibility.

    ```javascript
    function switchViewer(capture, perspective){
        if(capture) {
            captureViewer.show();
            captureViewer.play();
        } else {
            captureViewer.hide();
            captureViewer.stop();
        }

        if(perspective) {
            perspectiveViewer.show();
        } else {
            perspectiveViewer.hide();
        }
    }
    ```

- Register the relavant events to customize the interaction behavior between viewers.
    - Register `captured` event in `captureViewer` to hide the capture viewer and show the perspective viewer once an image is captured.

        ```javascript
        captureViewer.on("captured", () => {
            switchViewer(false, true);
        });
        ```

    - Register an event in `perspectiveViewer` to delete the current image and go back the capture viewer.

        ```javascript
        // Event for clicking "Back" button
        perspectiveViewer.on("backToCaptureViewer",() => {
            switchViewer(true, false);
            perspectiveViewer.currentDocument.deleteAllPages();
        });
        ```

    - Register an event in `perspectiveViewer` to determine if there are no images in the viewer when clicking "DeleteCurrent" & "DeletedAll" buttons.

        ```javascript
        // Event for clicking "DeleteCurrent" & "DeletedAll" buttons
        perspectiveViewer.on("noImageBack", () => {
            // Determine if there are no images in the viewer
            const count = perspectiveViewer.currentDocument.pages.length;
            // If yes, back to the capture viewer
            if(count === 0) {
                switchViewer(true,false)
            }
        });
        ```

    - Register an event in `perspectiveViewer` to display the result image.

        ```javascript
        perspectiveViewer.on("done", async () => {
            // hide viewers and container
            switchViewer(false, false);
            document.getElementById("container").style.display = "none";

            const pageUid = perspectiveViewer.getCurrentPageUid()
            const pageData =  await captureViewer.currentDocument.getPageData(pageUid);
            // Normalized image
            document.getElementById("normalized").src = URL.createObjectURL(pageData.display.data);
        });
        ```

For now, we finish the main workflow for this sample, can add the restore function to capture new image additionally.

```javascript
document.getElementById("restore").onclick = () => {
    perspectiveViewer.currentDocument.deleteAllPages();
    document.getElementById("container").style.display = "";
    switchViewer(true, false)
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
    <title>Mobile Web Capture - Capture Single Page and Then Crop</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/dynamsoft-document-viewer@2.0.0/dist/ddv.css">
    <link rel="stylesheet" href="./index.css">
</head>
<body>
    <div id="container"></div>
    <div id="imageContainer">
        <div id="restore">Restore</div>
        <span>Normalized Image:</span>
        <img id="normalized">
    </div>
</body>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-viewer@2.0.0/dist/ddv.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-core@3.2.10/dist/core.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-license@3.2.10/dist/license.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-normalizer@2.2.10/dist/ddn.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-capture-vision-router@2.2.10/dist/cvr.js"></script>
<script type="module">

    import { isMobile, initDocDetectModule } from "./utils.js";

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

        // Configure document boundaries function
        await initDocDetectModule(Dynamsoft.DDV, Dynamsoft.CVR);

        //Create a capture viewer
        const captureViewer = new Dynamsoft.DDV.CaptureViewer({
            container: "container",
            viewerConfig: {
                acceptedPolygonConfidence: 60,
                enableAutoCapture: true,
                enableAutoDetect: true
            }
        });
        // Play video stream in 1080P
        captureViewer.play({ 
            resolution: [1920,1080],
            fill: true
        });
        // Register captured event
        captureViewer.on("captured", () => {
            switchViewer(false, true);
        });

        // Define new UiConfig for perspecitve viewer
        const newPerspectiveUiConfig = {
            type: Dynamsoft.DDV.Elements.Layout,
            flexDirection: "column",
            children: [
                {
                    type: Dynamsoft.DDV.Elements.Layout,
                    className: "ddv-perspective-viewer-header-mobile",
                    children: [
                        {   
                            // Add a "Back" button in perspective viewer's header and bind the event
                            // The event will be registered later
                            type: Dynamsoft.DDV.Elements.Button,
                            className: "ddv-button-back",
                            events:{
                                click: "backToCaptureViewer"
                            }
                        },
                        Dynamsoft.DDV.Elements.Pagination,
                        {
                            // Bind event for "PerspectiveAll" button
                            // The event will be registered later
                            type: Dynamsoft.DDV.Elements.PerspectiveAll,
                            events:{
                                click: "done"
                            }
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
                        Dynamsoft.DDV.Elements.RotateRight,
                        {
                            // Bind event for "DeleteCurrent" button
                            // The event will be registered later
                            type: Dynamsoft.DDV.Elements.DeleteCurrent,
                            events: {
                                click: "noImageBack"
                            },
                        },
                        {
                            // Bind event for "DeleteCurrent" button
                            // The event will be registered later
                            type:Dynamsoft.DDV.Elements.DeleteAll,
                            events: {
                                click: "noImageBack"
                            },
                        }
                    ],
                },
            ],
        };

        // Create a perspective viewer
        const perspectiveViewer = new Dynamsoft.DDV.PerspectiveViewer({
            container: "container",
            groupUid: captureViewer.groupUid, // Data synchronisation with the capture viewer
            uiConfig: newPerspectiveUiConfig, // Configure the new UiConfig
            viewerConfig:{
                scrollToLatest: true,
            }
        }); 

        // Register the event for "Back" button
        perspectiveViewer.on("backToCaptureViewer",() => {
            switchViewer(true, false);
            perspectiveViewer.currentDocument.deleteAllPages();
        });

        // Register the event for "DeleteCurrent" & "DeletedAll" buttons
        perspectiveViewer.on("noImageBack", () => {
            // Determine if there are no images in the viewer
            const count = perspectiveViewer.currentDocument.pages.length;
            if(count === 0) {
                switchViewer(true,false)
            }
        }); 

        // Register the event for "PerspectiveAll" button to display the result image
        perspectiveViewer.on("done", async () => {
            // hide viewers and container
            switchViewer(false, false);
            document.getElementById("container").style.display = "none";

            const pageUid = perspectiveViewer.getCurrentPageUid()
            const pageData =  await captureViewer.currentDocument.getPageData(pageUid);
            // Normalized image
            document.getElementById("normalized").src = URL.createObjectURL(pageData.display.data);
        });

        // Restore Button function
        document.getElementById("restore").onclick = () => {
            perspectiveViewer.currentDocument.deleteAllPages();
            document.getElementById("container").style.display = "";
            switchViewer(true, false)
        };

        // Control viewers' visibility.
        function switchViewer(capture, perspective){
            if(capture) {
                captureViewer.show();
                captureViewer.play();
            } else {
                captureViewer.hide();
                captureViewer.stop();
            }

            if(perspective) {
                perspectiveViewer.show();
            } else {
                perspectiveViewer.hide();
            }
        }
    })();
</script>
</html>
```

## Download the whole project

[Github](https://github.com/Dynamsoft/mobile-web-capture/tree/master/samples/review-adjust-detected-boundaries) \| [Run](https://dynamsoft.github.io/mobile-web-capture/samples/review-adjust-detected-boundaries/)

Please note that in order to be compatible with desktop devices as much as possible, some compatibility codes have been added to the whole project code.

`UiConfig` part is organized into `uiConfig.js` and referenced in the core code to minimize the length of the core code.

## Add auxiliary text if necessary

Sometimes, you may want to add some auxiliary text to icons to show better user guidance.

### Refer to

- [Customize Elements' Display Text](https://www.dynamsoft.com/document-viewer/docs/ui/customize/elements.html#display-text)