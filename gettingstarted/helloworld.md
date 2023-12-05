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
// Write your code here.
</script>
</html>
```

## Include the Related SDK

To build the solution, we need to include five packages

1. `dynamsoft-document-viewer`: Required, it provides functions to create the viewers.
2. `dynamsoft-core`: Required, it includes `LicenseManager` class for `dynamsoft-document-normalizer`.
3. `dynamsoft-document-normalizer`: Required, it provides functions to detect boundaries or perform normalization.
4. `dynamsoft-capture-vision-router`: Required, it defines the class `CaptureVisionRouter`, which controls the whole process.

### Use a CDN

The simplest way to include the SDK is to use either the [jsDelivr](https://jsdelivr.com/) or [UNPKG](https://unpkg.com/) CDN.

- jsDelivr

  ```html
  <script src="https://cdn.jsdelivr.net/npm/ddv"></script>
  <script src="https://cdn.jsdelivr.net/npm/dynamsoft-core@3.0.10/dist/core.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-normalizer@2.0.11/dist/ddn.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/dynamsoft-capture-vision-router@2.0.11/dist/cvr.js"></script>
  ```

- UNPKG

  ```html
  <script src="https://unpkg.com/ddv"></script>
  <script src="https://unpkg.com/dynamsoft-core@3.0.10/dist/core.js"></script>
  <script src="https://unpkg.com/dynamsoft-document-normalizer@2.0.11/dist/ddn.js"></script>
  <script src="https://unpkg.com/dynamsoft-capture-vision-router@2.0.11/dist/cvr.js"></script>
  ```

### Host the SDK yourself

Besides using the CDN, you can also download the Solution and host related files on your own website/server before including it in your application.

Options to download the SDK:

- From the website

  [Download the JavaScript ZIP package]()

- yarn

  ```cmd
  yarn add ddv
  yarn add dynamsoft-capture-vision-router@2.0.11
  ```

- npm

  ```cmd
  npm install ddv
  npm install dynamsoft-capture-vision-router@2.0.11
  ```

> Note: Upon installation of `dynamsoft-capture-vision-router`, the compatible versions of `dynamsoft-document-normalizer` and `dynamsoft-core` will be automatically downloaded.

Depending on how you downloaded the SDK and where you put it, you can typically include it like this:

  ```html
  <!-- Upon extracting the zip package into your project, you can generally include it in the following manner -->
  <script src="./distributables/ddvjs"></script>
  <script src="./distributables/dynamsoft-core@3.0.10/dist/core.js"></script>
  <script src="./distributables/dynamsoft-document-normalizer@2.0.11/dist/ddn.js"></script>
  <script src="./distributables/dynamsoft-capture-vision-router@2.0.11/dist/cvr.js"></script>
  ```

or

  ```html
  <script src="./node_modules/ddvjs"></script>
  <script src="./node_modules/dynamsoft-core/dist/core.js"></script>
  <script src="./node_modules/dynamsoft-document-normalizer/dist/ddn.js"></script>
  <script src="./node_modules/dynamsoft-capture-vision-router/dist/cvr.js"></script>
  ```

## Define necessary HTML elements

For HelloWorld, we define below elements.

```html
<div id="container"></div>
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
<link rel="stylesheet" href="./helloworld.css">
```

`helloworld.css` content:

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
    engineResourcePath: "********",
});

// Initialize DDN
Dynamsoft.License.LicenseManager.initLicense("********************");
Dynamsoft.CVR.CaptureVisionRouter.preloadModule(["DDN"]);

const router = await Dynamsoft.CVR.CaptureVisionRouter.createInstance();
```

## Configure document boundaries function

```javascript
Dynamsoft.DDV.setProcessingHandler("documentBoundariesDetect", new DDNNormalizeHandler());
```

## Create capture viewer

```javascript
const captureViewer = new Dynamsoft.DDV.CaptureViewer({
    container: "container",
    viewerConfig: {
        acceptedPolygonConfidence: 60,
        enableAutoCapture: true,
        enableAutoDetect: true
    }
});
// Play video stream
captureViewer.play({ 
    resolution: [1920,1080],
    fill: true
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
        fill: true,
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
    <title>DocWebCapture</title>
    <link rel="stylesheet" href="../Resources/ddv.css">
    <link rel="stylesheet" href="./helloworld.css">
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
<script src="https://cdn.jsdelivr.net/npm/ddv"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-core@3.0.10/dist/core.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-document-normalizer@2.0.11/dist/ddn.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dynamsoft-capture-vision-router@2.0.11/dist/cvr.js"></script>
<script type="module">
    // Initialize DDV
    await Dynamsoft.DDV.setConfig({
        license: "*******",
        engineResourcePath: "********",
    });

    // Initialize DDN
    Dynamsoft.License.LicenseManager.initLicense("********************");
    Dynamsoft.CVR.CaptureVisionRouter.preloadModule(["DDN"]);

    const router = await Dynamsoft.CVR.CaptureVisionRouter.createInstance();

    // Configure document boundaries function
    class DDNDetectHandler extends Dynamsoft.DDV.DocumentDetect {
        async detect(image, config) {
            if(!router) {
                return Promise.resolve({
                    success: false,
                });
            }
            // Define DSImage according to the usage of DDN.
            const DSImage = {
                bytes: new Uint8Array(image.data.slice(0)),
                width: image.width,
                height: image.height,
                stride: image.width * 4,
                format: 10,
            };
            // Use DDN normalized module
            const results = await router.capture(DSImage, "detect-document-boundaries");

             // Filter the results and generate corresponding return values.
             if (results.items.length <= 0) {
                return Promise.resolve({
                    success: false,
                });
            }

            const quad = [];
            results.items[0].location.points.forEach(p => {
                quad.push([p.x, p.y]);
            });

            const detectResult = this.processDetectResult(
                quad,
                image.width,
                image.height,
                config,
            );

            return Promise.resolve(detectResult);
        }
    }
    Dynamsoft.DDV.setProcessingHandler("documentBoundariesDetect", new DDNDetectHandler());

    //Create capture viewer
    const captureViewer = new Dynamsoft.DDV.CaptureViewer({
        container: "container",
        viewerConfig: {
            acceptedPolygonConfidence: 60,
            enableAutoCapture: true,
            enableAutoDetect: true
        }
    });
    // Play video stream
    captureViewer.play({ 
        resolution: [1920,1080],
        fill: true
    });

    // Display the result image
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

    // Restore function
    document.getElementById("restore").onclick = () => {
        captureViewer.currentDocument.deleteAllPages();
        captureViewer.play({
            resolution: [1920,1080],
            fill: true,
        });
        document.getElementById("container").style.display = "";
    };
</script>
</html>
```

## More use cases

We provide some samples which demonstrate the popular use cases, for example, review and adjust the boundaries, edit the result images, export the result images in PDF format and so on.

Please refer to the [Use Case]({{ site.codegallery }}usecases/index.html) section.

## [Demo]({{ site.codegallery }}demo/index.html)