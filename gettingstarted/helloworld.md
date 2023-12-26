---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
title: Document Web Capture from Mobile Camera - Creating HelloWorld
keywords: Documentation, Document Web Capture from Mobile Camera, Creating HelloWorld
breadcrumbText: Creating HelloWorld
description: Document Web Capture from Mobile Camera Documentation Creating HelloWorld
permalink: /gettingstarted/helloworld.html
---

# Creating HelloWorld

In this section, we’ll break down and show all the steps needed to build the HelloWorld solution in a web page.

- Check out [HelloWorld]()

We’ll build on this skeleton page:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>DWC from Mobile Camera HelloWorld</title>
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

For HelloWorld, we define below elements.

- Container to hold the viewer

```html
<div id="container"></div>
```

- Restore button and `img` element for displaying the result image

```html
<div id="imageContainer">
    <div id="restore">Restore</div>
    <span>Original Image:</span>
    <img id="original">
    <span>Normalized Image:</span>
    <img id="normalized">
</div>
```

## Link CSS to HTML

`ddv.css` is the necessary css file which defines the viewer style of Dynamsoft Document Viewer.
`index.css` defines the style of elements which is in Helloworld.

```html
<link rel="stylesheet" href="../Resources/ddv.css">
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
    overflow: hidden;
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
    user-select: none;
    cursor: pointer;
}
```

## Related SDK initialization

```javascript
// Initialize DDV
await Dynamsoft.DDV.setConfig({
    license: "*******",
    engineResourcePath: "*******",
});

// Initialize DDN
Dynamsoft.License.LicenseManager.initLicense("*******");
Dynamsoft.CVR.CaptureVisionRouter.preloadModule(["DDN"]);
```

## Configure document boundaries function

- Step one: The related configuration code is packaged in `utils.js`, so it should be imported.

    ```javascript
    import { isMobile, initDocDetectModule } from "./utils.js";
    ```

- Step two: Call the following function.

    ```javascript
    await initDocDetectModule(Dynamsoft.DDV, Dynamsoft.CVR);
    ```

## Create a capture viewer

```javascript
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
});
```

## Display the result image

Use the capture event to obtain the result image.

```javascript
captureViewer.on("captured", async (e) => {
    const pageData =  await captureViewer.currentDocument.getPageData(e.pageUid);
    //Original image
    document.getElementById("original").src = URL.createObjectURL(pageData.raw.data); 
    // Normalized image
    document.getElementById("normalized").src = URL.createObjectURL(pageData.display.data); 
    // Stop video stream and hide capture viewer's container
    captureViewer.stop();
    document.getElementById("container").style.display = "none";
});
```

For now, we finish the main workflow for HelloWorld, can add the restore function to capture new image additionally.

```javascript
document.getElementById("restore").onclick = () => {
    captureViewer.currentDocument.deleteAllPages();
    captureViewer.play({
        resolution: [1920,1080],
    });
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
    <title>DWC from Mobile Camera HelloWorld</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/dynamsoft-document-viewer@1.0.0/dist/ddv.css">
    <link rel="stylesheet" href="./index.css">
</head>
<body>
    <div id="container"></div>
    <div id="imageContainer">
        <div id="restore">Restore</div>
        <span>Original Image:</span>
        <img id="original">
        <span>Normalized Image:</span>
        <img id="normalized">
    </div>
</body>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-viewer@1.0.0/dist/ddv.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-core@3.0.10/dist/core.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-normalizer@2.0.11/dist/ddn.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-capture-vision-router@2.0.11/dist/cvr.js"></script>
<script type="module">
    import { isMobile, initNormalizedModule } from "./utils.js";

    (async () => {
        // Initialize DDV
        await Dynamsoft.DDV.setConfig({
            license: "*******",
            engineResourcePath: "********",
        });

        // Initialize DDN
        Dynamsoft.License.LicenseManager.initLicense("********************");
        Dynamsoft.CVR.CaptureVisionRouter.preloadModule(["DDN"]);

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
        });

        // Display the result image
        captureViewer.on("captured", async (e) => {
            // Stop video stream and hide capture viewer's container
            captureViewer.stop();
            document.getElementById("container").style.display = "none";

            const pageData =  await captureViewer.currentDocument.getPageData(e.pageUid);
            // Original image
            document.getElementById("original").src = URL.createObjectURL(pageData.raw.data); 
            // Normalized image
            document.getElementById("normalized").src = URL.createObjectURL(pageData.display.data); 
        });

        // Restore Button function
        document.getElementById("restore").onclick = () => {
            captureViewer.currentDocument.deleteAllPages();
            captureViewer.play();
            document.getElementById("container").style.display = "";
        };
    })();
</script>
</html>
```

## Download the whole project

- Hello World - [Github]() \| [Run]()
- Angular App - [Github]() \| [Run]()
- React App - [Github]() \| [Run]()
- Vue App - [Github]() \| [Run]()

## More use cases

We provide some samples which demonstrate the popular use cases, for example, review and adjust the boundaries, edit the result images, export the result images in PDF format and so on.

Please refer to the [Use Case]({{ site.codegallery }}usecases/index.html) section.

<!-- ## [Demo]({{ site.codegallery }}demo/index.html) -->