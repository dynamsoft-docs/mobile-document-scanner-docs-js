<!-- 
### `DocumentScanner.dispose()`

Release and clean up DDS's Views and resources (DCE, DCV, scanned document), then clear and hide all containers in the DOM.

#### Syntax

```javascript
DocumentScanner.dispose(): void;
```

#### Parameters

None

#### Return

`void`

#### Example

```javascript
documentScanner.dispose();
```

### `DocumentScanner.isCapturingImage()`

Returns whether or not the `DocumentScanner` is in the process of capturing an image. This returns true throughout `DocumentScanner.launch()`.

#### Syntax

```javascript
isCapturingImage(): boolean
```

#### Parameters

None

#### Return

`true` while the `DocumentScanner` is capturing an image, false otherwise.

#### Example

```javascript
dds.isCapturingImage();
``` -->

<!-- 
### `DocumentScannerView.initialize()`

Initialize an instantiated `DocumentScannerView` object. This creates styling for the loading screen, and starts up the View with settings provided by `SharedResources` and `DocumentScannerViewConfig` passed during instantiation.

#### Syntax

```javascript
async DocumentScannerView.initialize(): Promise<void>;
```

#### Parameters

None

#### Return

`Promise<void>`

#### Example

```javascript
let documentScannerView = new DocumentScannerView(resources, documentScannerConfig); // Instantiate View object with resources and config objects
documentScannerView.initialize();
```

### `DocumentScannerView.handleCloseBtn()`

Close the camera view and set the status of the result as `EnumResultStatusCode.CANCELLED` (see [`EnumResultsStatusCode`](#enumresultsstatuscode) for details).

#### Syntax

```javascript
async documentScannerView.handleCloseBtn();
```

#### Parameters

None

#### Example

```javascript
documentScannerView.handleCloseBtn();
```

### `DocumentScannerView.toggleAutoCapture()`

Enable or disable the automatic image capture feature in the `DocumentScannerView`. When enabled, DDS automatically captures the image of the document after collecting a certain number of frames defined by the `DocumentScannerViewConfig.consecutiveResultFramesBeforeNormalization` (default value is 30 frames in the full workflow). Enabling auto-capture always enables bounds detection.

#### Syntax

```javascript
async DocumentScannerView.toggleAutoCapture(mode?: boolean);
```

#### Parameters

`mode?: boolean`: if provided, enable or disable the auto-capture feature. If no argument is provided, simply toggle the feature to the other state.

#### Example

```javascript
documentScannerView.toggleAutoCapture(true); // Enable auto-capture
documentScannerView.toggleAutoCapture(); // Toggle auto-capture
```

### `DocumentScannerView.toggleBoundsDetection()`

Enable or disable the document bounds detection feature in the `DocumentScannerView`. When enabled, DDS displays the detected document boundaries (edges) dynamically in the viewfinder. When an image is captured, the detected bounds are applied to the captured result.

#### Syntax

```javascript
async DocumentScannerView.toggleBoundsDetection(mode?: boolean);
```

#### Parameters

`mode?: boolean`: if provided, enable or disable bounds detection. If no argument is provided, simply toggle the feature to the other state.

#### Example

```javascript
documentScannerView.toggleBoundsDetection(true); // Enable bounds detection
documentScannerView.toggleBoundsDetection(); // Toggle bounds detection
```

### `DocumentScannerView.openCamera()`

Start the camera feed in the `DocumentScannerView`.

#### Syntax

```javascript
async DocumentScannerView.openCamera();
```

#### Parameters

None

#### Example

```javascript
documentScannerView.closeCamera();
```

### `DocumentScannerView.closeCamera()`

Close the camera feed in the `DocumentScannerView`.

#### Syntax

```javascript
async DocumentScannerView.closeCamera();
```

#### Parameters

None

#### Example

```javascript
documentScannerView.closeCamera();
```
### `DocumentScannerView.pauseCamera()`

Pause the camera feed in the `DocumentScannerView` and freeze the viewfinder on the frozen frame.

#### Syntax

```javascript
async DocumentScannerView.pauseCamera();
```

#### Parameters

None

#### Example

```javascript
documentScannerView.pauseCamera();
```

### `DocumentScannerView.takePhoto()`

Capture the document present in the viewfinder, with the boundary detection data if boundary detection is enabled. Process the image by performing perspective correction using the detected boundaries (while showing a loading animation in the `DocumentScannerView`), then pass the result to the `currentScanResolver()`.

#### Syntax

```javascript
async DocumentScannerView.takePhoto();
```

#### Parameters

None

#### Example

```javascript
documentScannerView.takePhoto();
```

### `DocumentScannerView.handleBoundsDetection()`

Unclear

#### Syntax

```javascript
async handleBoundsDetection(result: CapturedResult);
```

#### Parameters

`result: CapturedResult`: unclear

#### Example

```javascript
documentScannerView.handleBoundsDetection(result);
``` 

### `DocumentScannerView.normalizeImage()`

Perform perspective correction on the provided original image, according to the provided document boundaries. Then, output the resulting normalized image.

#### Syntax

```javascript
async normalizeImage(
    points: Quadrilateral["points"],
    originalImageData: OriginalImageResultItem["imageData"]
  	): Promise<NormalizedImageResultItem>;
```

#### Parameters

`points: Quadrilateral["points"]`: a quadrilateral of points representing the vertices of the document borders on the original image. The vertices are ordered clockwise, with the top left hand vertex in index 0.
`originalImageData: OriginalImageResultItem["imageData"]: the source image to normalize.

#### Return

`Promise<NormalizedImageResultItem>`: A Promise which resolves to the normalized image of the document.

#### Example

```javascript
DocumentScannerView.normalizeImage(
	[
		{ x: 0, y: 0 },
		{ x: 100, y: 0 },
		{ x: 120, y: 300 },
		{ x: 20, y: 270 }
	],
	originalImage
);
```-->

<!-- 
### `ScanResultView.initialize()`

Initialize an instantiated `ScanResultView` object. This styles the container, displays the corrected scanned document in the container, and sets up UI controls.

#### Syntax

```javascript
async initialize(): Promise<void>;
```

#### Parameters

None

#### Return

`Promise<void>`: resolves to `void` upon completion

#### Example

```javascript
scanResultView.initialize();
```

### `ScanResultView.hideView()`

Hide the container of the `ScanResultView` on the DOM.

#### Syntax

```javascript
scanResultView.hideView(): void;
```

#### Parameters

None

#### Return

`void`

#### Example

```javascript
scanResultView.hideView();
```

### `ScanResultView.dispose()`

Release and clean up resources used by the `ScanResultView` (DCE, DCV, scanned document), then clear and hide the HTML container.

#### Syntax

```javascript
ScanResultView.dispose(preserveResolver: boolean = false): void;
```

#### Parameters

`preserveResolver: boolean = false`: Clean up the resolver method if false (default behavior)

#### Return

`void`

#### Example

```javascript
scanResultView.dispose(false); // Dispose of resources and clean up resolver
``` -->

<!-- 
### `DocumentCorrectionView.initialize()`

Initialize an instantiated `DocumentCorrectionView` object. This creates the UI, and starts up the View with settings provided by `SharedResources` and `DocumentCorrectionViewConfig` passed during instantiation.

#### Syntax

```javascript
async DocumentCorrectionView.initialize();
```

#### Parameters

None

#### Example

```javascript
documentCorrectionView.initialize();
```

### `DocumentCorrectionView.setFullImageBoundary()`

Set the image boundaries to the full extent of the original captured image.

#### Syntax

```javascript
DocumentCorrectionView.setFullImageBoundary(): void
```

#### Parameters

None

#### Return

`void`

#### Example

```javascript
documentCorrectionView.setFullImageBoundary();
```

### `DocumentCorrectionView.setBoundaryAutomatically()`

Automatically detect and set the boundaries of the document in the original image.

#### Syntax

```javascript
DocumentCorrectionView.setBoundaryAutomatically(): void
```

#### Parameters

None

#### Return

`void`

#### Example

```javascript
documentCorrectionView.setBoundaryAutomatically();
```

### `DocumentCorrectionView.confirmCorrection()`

Apply the changes made to the scanned document, call the `onFinish` callback if provided, resolve the `Promise` with the corrected document, then release all resources used by the `DocumentCorrectionView` and clear the container.

#### Syntax

```javascript
async DocumentCorrectionView.confirmCorrection(): void;
```

#### Parameters

None

#### Return

`void`

#### Example

```javascript
documentCorrectionView.confirmCorrection();
```

### `DocumentCorrectionView.hideView()`

Hide the container of the `ScanResultView` on the DOM.

#### Syntax

```javascript
documentCorrectionView.hideView(): void;
```

#### Parameters

None

#### Return

`void`

#### Example

```javascript
documentCorrectionView.hideView();
```

### `DocumentCorrectionView.correctImage()`

Apply boundary and perspective normalization using the document boundaries to the original image and return the result (the original image is not destroyed).

#### Syntax

```javascript
async correctImage(points: Quadrilateral["points"]): Promise<NormalizedImageResultItem>;
```

#### Parameters

`points: Quadrilateral["points"]`: a quadrilateral of points representing the vertices of the document borders on the original image. The vertices are ordered clockwise, with the top left hand vertex in index 0.

#### Return

`Promise<NormalizedImageResultItem>`: the scan of the document with boundary and perspective normalization applied.

### `DocumentCorrectionView.dispose()`


Release and clean up resources used by the `DocumentCorrectionView`, then clear and hide the HTML container.

#### Syntax

```javascript
DocumentCorrectionView.dispose(): void;
```

#### Parameters

None

#### Return

`void`

#### Example

```javascript
documentCorrectionView.dispose();
``` -->

<!-- #### `DocumentScanner.initialize()`

Initialize the DDS Views of an instantiated DDS object: `DocumentScannerView`, `DocumentCorrectionView`, and `ScanResultView`. Then, return the Views in a Promise.

##### Syntax

```javascript
async initialize(): Promise<{
    scannerView: DocumentScannerView;
    correctionView: DocumentCorrectionView;
    scanResultView: ScanResultView;
}>
```
##### Parameters

None

##### Return

```javascript
Promise<{
    scannerView: DocumentScannerView;
    correctionView: DocumentCorrectionView;
    scanResultView: ScanResultView;
}>
```

##### Example

```javascript
let dds = new DocumentScanner();
let { scannerView, normalizerView, scanResultView } = dds.initialize();
```



### `DocumentScannerView()`

Create a `DocumentScannerView` which handles the image capture portion of the SDK, as well as the associated UI View. The `DocumentScannerConfig` sets the UI template, the HTML container, and also the number of consecutive video feed frames used for automatic document bounds detection. `SharedResources` also configures underlying Dynamsoft components. This constructor only instantiates two private properties of the `DocumentScannerView` object with the provided arguments. Call [`initialize()](#documentscannerviewinitialize) to initialize the View object.

#### Syntax

```javascript
DocumentScannerView(private resources: SharedResources, private config: DocumentScannerViewConfig);
```

#### Parameters

`resources: SharedResources`: configures underlying Dynamsoft components as well as the corresponding `CameraView` - see [`SharedResources`](#sharedresources) to learn more.
`config: DocumentScannerViewConfig`: configures the UI template, the HTML container, and also the number of consecutive video feed frames used for automatic document bounds detection. Learn more at [`DocumentScannerViewConfig`](#documentscannerviewconfig).

#### Example

```javascript
let defaultDocumentScannerView = new DocumentScannerView();
```

### `DocumentScannerView.launch()`

Run the `DocumentScannerView`-specific portion of the ready-to-use DDS workflow, by handling all UI and business logic. Return a promise which resolves to an object containing the image of the scanned document, along with any boundary detection data, upon successful image capture.

#### Syntax

```javascript
async DocumentScannerView.launch(): Promise<DocumentScanResult>;
```

#### Parameters

None

#### Return

`Promise<DocumentScanResults>`: A Promise which resolves to an object containing the scanned image and image processing data such as the detected document boundaries.

#### Example

```javascript
DocumentScannerView.launch();
```

### `ScanResultView()`

Create a `ScanResultView`, which is a View that serves to display the state of the scanned document with visual corrections applied. The `ScanResultViewConfig` sets the HTML container, and also defines the UI with `ScanResultViewControls`. The `ScanResultViewConfig` also sets the handlers to either on the scan result upon proceeding through the workflow, as well as exporting directly. This constructor only instantiates two private properties of the `DocumentScannerView` object with the provided arguments. Call [`initialize()`](#scanresultviewinitialize) to initialize the View object.

#### Syntax

```javascript
ScanResultView(
    private resources: SharedResources,
    private config: ScanResultViewConfig,
    private scannerView: DocumentScannerView,
    private correctionView: DocumentCorrectionView
)
```

#### Parameters

`resources: SharedResources,`: configures underlying Dynamsoft components as well as the corresponding `CameraView` - see [`SharedResources`](#sharedresources) to learn more.
`config: ScanResultViewConfig`: 
`scannerView: DocumentScannerView`: the `DocumentScannerView` to be associated with this `ScanResultView`.
`correctionView: DocumentCorrectionView`: the `DocumentScannerView` to be associated with this `ScanResultView`.

#### Example

```javascript
let scanResultView = new ScanResultView(
    resources: {
        cvRouter: myCVRouter,
        cameraEnhancer: myCameraEnhancer,
        cameraView: myCameraView,
        result: myResult
    },
    config: {
        container: myContainer,
        controlIcons: myScanResultViewControlIcons,
        onComplete: processScanResult, // Custom handler to process the scan upon clicking "Complete"
        onExport: uploadToServer, // Custom handler to upload to server
    },
    scannerView: myScannerView,
    correctionView: myCorrectionView
)
```

### `ScanResultView.launch()`

Run the `ScanResultView`-specific portion of the ready-to-use DDS workflow, by handling all UI and business logic. Return a promise which resolves to an object containing the image of the scanned document, along with any boundary detection data, once the user clicks the "Complete" button.

#### Syntax

```javascript
async ScanResultView.launch(): Promise<DocumentScanResult>
```

#### Parameters

None

#### Return

Returns a Promise that resolves with a [`DocumentScanResult`](#documentscanresult), which contains the corrected scan of the document.

#### Example

```javascript
scanResultView.launch();
```

### `DocumentCorrectionView()`

Create a `DocumentCorrectionView`. In this View, the user can graphically adjust the document boundaries detected by DDS. The `DocumentCorrectionViewConfig` sets the HTML container, and also defines the UI with `DocumentCorrectionViewControlIcons`. The `DocumentCorrectionViewConfig` also sets the handler to process the document scan after any document boundary alterations. This constructor only instantiates two private properties of the `DocumentCorrectionView` object with the provided arguments. Call [`initialize()`](#documentcorrectionviewinitialize) to initialize this View object.

#### Syntax

```javascript
DocumentCorrectionView(private resources: SharedResources, private config: DocumentCorrectionViewConfig);
```

#### Parameters

`resources: SharedResources`: configures underlying Dynamsoft components as well as the corresponding `CameraView` - see [`SharedResources`](#sharedresources) to learn more.
`config: DocumentCorrectionViewConfig`: sets the HTML container, the icons for each of the menu buttons, and the handler to be called upon exiting this View. Learn more at [`DocumentCorrectionViewConfig`](#documentcorrectionviewconfig).

#### Example

```javascript
let documentCorrectionView = new DocumentCorrectionView(
    resources: {
        cvRouter: myCVRouter,
        cameraEnhancer: myCameraEnhancer,
        cameraView: myCameraView,
        result: myResult
    },
    config: {
        container: myContainer,
        controlIcons: myDocumentCorrectionViewControlIcons,
        utilizedTemplateNames: myUtilizedTemplateNames,
        onFinish: uploadToServer // Custom handler to upload directly to server after clicking "Finish"
    },
)
```

### `SharedResources`

This interface configures underlying Dynamsoft components as well as the corresponding `CameraView`. This interface is used by all three DDS views to share common DCV components. This interface also configures a callback function that triggers upon updating the document scan.

#### Syntax

```javascript
interface SharedResources {
  cvRouter?: CaptureVisionRouter;
  cameraEnhancer?: CameraEnhancer;
  cameraView?: CameraView;
  result?: DocumentScanResult;
  onResultUpdated?: (result: DocumentScanResult) => void;
}
```

- [CaptureVisionRouter`]()
- [`CameraEnhancer`]()
- [`CameraView`]()
- [`DocumentScanResult`](#documentscanresult)

#### Properties

- `cvRouter: CaptureVisionRouter`: the underlying Capture Vision Router
- `cameraEnhancer: CameraEnhancer`: the underlying Dynamsoft Camera Enhancer
- `cameraView: CameraView`: the underlying DCE camera UI
- `result: DocumentScanResult`: the return object containing the scanned and corrected document
- `onResultUpdated: (result: DocumentScanResult) => void`: the handler called when updating the `DocumentScanResult`

### `DocumentCorrectionView.launch()`

Run the `DocumentCorrectionView`-specific portion of the ready-to-use DDS workflow, by handling all UI and business logic. Return a promise which resolves to an object containing the image of the scanned document with alterations made by the user, once the user clicks the "Finish" button.

#### Syntax

```javascript
async documentCorrectionView.launch(): Promise<DocumentScanResult>;
```

#### Parameters

None

#### Return

`Promise<DocumentScanResult>`: A `Promise` that resolves with a [`DocumentScanResult`](#documentscanresult), which contains the corrected scan of the document, once the user clicks "Finish" in the `DocumentCorrectionView`. -->