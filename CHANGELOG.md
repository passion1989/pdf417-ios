# Release notes

## 5.0.5

- US Driver's Licence:
    - fixed parsing of Virgin Islands DL
    - added support for Arkansas DL
    - added support for new South Carolina DL

## 5.0.4

- iOS fixes:
	- Fixed some issues related to internal camera management

## 5.0.3

- iOS fixes:
	- Fixed problems with string localizations

## 5.0.2

- iOS fixes:
	- CFBundleSUpportedPlatforms removed from Info.plist files
	- Applying affine transformation to `PPQuadrangle` now correctly assigns points.
	- When using both Direct API and `PPCameraCoordinator`, scanning results will now be correctly outputted to `PPCoordinatorDelegate` and `PPScanningDelegate` respectively
	- Fixed crashes related to camera permissions and added dummy view when camera permission is disabled
	- Fixed issues related to topLayoutGuide on iOS6

## 5.0.1

- iOS updates:

	- Added option to mirror input images to PPCameraSettings
	
- iOS bugfixes:

	- ITF reader now accepts barcode sizes ranging between [6,50]

## 5.0.0

- iOS updates:

	- Implemented `PPCameraCoordinator`. `PPCameraCoordinator` assumes the role of `PPCoordinator` from previous versions while new `PPCoordinator` is used for Direct API (image processing without camera out management).
	- Increased speed of scanning for barcode type recognizers.
	- Implemented `PPImage`. When using Direct API you can wrap `UIImage` and `CMSampleBufferRef` into `PPImage` to ensure optimal performance.
	- Improved performance of Direct API. In addition, you can now use Direct API with your own camera management without any performance drawbacks.
	- Added method `isCameraPaused` to `PPScanningViewController`.
	- Added option to fllip input images upside down for processing with `cameraFlipped` property of `PPCameraSettings`.
	- Implemented `PPViewControllerFactory` for managing creation of `PPScanningViewController` objects.
	- `PPImageMetadata` now contains `PPImageMetadataType` property, which describes which image type was outputted.
	
- iOS bugfixes:
	- New Direct API fixed possible deadlocks when sending large amounts of data

## 4.3.0

- Added better integration for Swift

        - Added Nullability Attributes
        - Added modulemap file
        - Added sample app in Swift

- Refactored PPMetadataSettings

        - Added debug metadata settings for debugging payslip detection and image processing
        - `successfulScanFrame` renamed to `successfulFrame`
        - `currentVideoFrame` renamed to `currentFrame`

- Exposed `PPModernViewfinderOverlaySubview` overlay subview class in public headers. This enables you to more easily recreate default overlay UI in your custom Overlay view controllers.

- in `PPCoordinator`, renamed method `isScanningUnsupported:` to `isScanningUnsupportedForCameraType:error:`. This was introduced to provide more granularity in checking if scanning is supported.

- `PPOverlayViewController`changed the way Overlay Subviews are added to the view hierarchy. Instead of calling `addOverlaySubview:` (which automatically added a view to view hierarachy), you now need to call `registerOverlaySubview:` (which registers subview for scanning events), and manually add subview to view hierarchy using `addSubview`: method. This change gives you more flexibility for adding views and managing autolayout and autoresizing masks.

- Localization Macros MB_LOCALIZED and MB_LOCALIZED_FORMAT can now be overriden in your app to provide completely custom localization mechanisms.

- Increased speed of scanning cancellation when Cancel button is pressed.

## 4.2.2

- Fixed several issues in USDL parsing
        - implemented special cases for barcodes which don't have keys according to the AAMVA version written in the barcode
        - implemented heuristics for extraction of firstName, middleName, lastName, address, and other fields, if they can be determined based on the other fields.

- Bugfixes and tweaks in camera management code
        - fixed potential deadlock when multiple instances of `PPCoordinator` objects are instantiated.
        - exiting from the scanning when user presses "cancel" button is now faster
        - fixed race condition which potentially crashed the scanner when user exited and entered camera screen consecutively very fast.

## 4.2.1

- Added new callback method to `PPScanDelegate` which is called when license key is invalid: `scanningViewController:invalidLicenseKeyWithError:`

## 4.2.0

- Added bitcode support for Xcode 7

- Updated USDL parsing. Added better handling of FullName, FullAddress, Height and Weight of cardholder. Added full support for Magnetic PDF417 standard. 

- Small performance improvements in iOS camera management.

## 4.1.1

- Added special case for scanning some British Columbia DLs
- Stability improvements in USDL scanning

## 4.1.0

- Improved video frame quality detection: now only the sharpest and the most focused frames are being processed. This improves quality of the results, but at a slight expense of processing time

- Frame quality estimation can now be enabled using `PPScanSettings frameQualityEstimationMode` property:
    - when set to `PPFrameQualityEstimationModeOn`, frame quality estimation is always enabled
    - when set to `PPFrameQualityEstimationModeOff`, frame quality estimation is always disabled
    - when set to `PPFrameQualityEstimationModeDefault`, frame quality estimation is enabled internally, if the SDK determines it makes sense

- iOS 9 introduced new app multitasking features Split View and Slide Over. When the scanner is on screen and one of those features are used, iOS automatically pauses the Camera (this behaviour is default as of iOS 9 beta 5). This SDK version introduces new setting in `PPUISettings` class, called `cameraPausedView`, where you can define the `UIView` which is presented centered on screen when this happens.

- Known issue on iOS 9: if you use Autorotate overlay feature (`settings.uiSetttings.autorotateOverlay`), present `PPScanningViewController` as a modal view controller, and support Split View iOS 9 feature, then autorotation of camera overlays isn't correct. The best way is to opt-out of Split View feature, and wait for SDK fix when iOS 9 comes out of beta.

- `PPScanningViewController` methods `pauseScanning`, `isScanningPaused`, and `resumeScanningAndResetState:` should now be called only from Main thread, and they are effective immediately. E.g., if `pauseScanning` is called and there is a video frame being processed, result of processing of that frame will be discarded, if `resumeScanningAndResetState:` isn't called in the meantime.

- Re-introduced property `locationOnImage` for `PPPdf417RecognizerResult` objects, which existed prior to v4.0.0. It's now a `PPQuadrangle` object, with clear properties for obtaining corner points of the barcode on the image.

## 4.0.2

- Added support for `PPCameraPresetPhoto` camera preset. Use this if you need the same zoom level as in iOS Camera app. The resolution for video feed when using this preset is the same as devices screen resolution.

## 4.0.1

- Added support for several new special cases of US Driver Licenses in USDL recognizer. 

- Exposed two new properties in `PPUsdlRecognizerSettings`

    -  `scanUncertain` 
        - Set this to YES to scan even barcode not compliant with standards
        - Use only if necessary because it slows down the recognition process
        - Default: NO
        
    - `allowNullQuietZone`
        - set this to YES to scan barcodes which don't have quiet zone (white area) around it
        - slightly slows down recognition process
        - Default: YES

- Disabled Bitcode in Sample apps so that they are buildable with XCode 7 (Bitcode support in the framework coming soon!)

- Added library dependencies in podspec without which the app after integration couldn't be built 

## 4.0.0

- Series of bugfixes, performance improvements and API consolidation.

- API now compatible with [other MicroBlink products](https://github.com/microblink/about).

- Naming changes in API (see Transition guide)

    - `PPBaseResult` renamed to `PPRecognizerResult`
    - `PPScanningResult` is now replaced with 4 different result classes, depending on where the result comes from
    
        - `PPUsdlRecognizerResult` for USDL scanning
        - `PPPdf417RecognizerResult` for PDF417 scanning
        - `PPBarDecoderRecognizerResult` for Code39 and Code128 scanning
        - `PPZXingRecognizerResult` for other barcode formats
        
- `PPScanningViewController`'s methods `resumeScanning` and `resumeScanningWithoutStateReset` merged into one `resumeScanningAndResetState:`. 

    - All calls to `resumeScanning` replace with `resumeScanningAndResetState:YES`.
    - All calls to `resumeScanningWithoutStateReset` replace with `resumeScanningAndResetState:NO`

- Added direct processing API which you can use to perform OCR on UIImage objects.

- Added NoCamera-sample project which shows how to use direct processing API

- Added didOutputMetadata: callback method to PPOverlayViewControllers

- This version uses a new license key format. If you had a license key generated prior to v4.0.0, contact us so we can regenerate the license key for you.

- Each `PPRecognizerResult` now has implemented `description` method for easier debugging

- Fixed orientation handling for case when overlay autorotates.

- Scanning region is now a property of Scanning view controller, and overlay view controller now delegates to this property.

- PPBarcodeElementType changed names:
        From PPTextElement to PPBarcodeElementTypeText
        From PPByteElement to PPBarcodeElementTypeByte

## 3.2.0
- Improvements in USDL parsing

## 3.1.5
- Workaround for random crashes when scanning uncertain barcodes

## 3.1.4
- Fixed a rare issue with decoding of pdf417 barcodes (when barcode last element was of type byte)

## 3.1.3
- Fixed issue with callback `cameraViewController:didMakeSuccessfulScanOnImage:` not being called 

## 3.1.2
- USDL License error message which was always displayed is now fixed

## 3.1.1
- Bugfixes
- More internal header files are now part of the API. This makes creation of custom UI easier.

## 3.1.0
- Fixed crashing on some iOS8 devices caused by AVFoundation video callback change
- Added scanning and data extracting of US Drivers licenses. To scan USDL, use the following initialization setting:

	```objective-c
	// Set YES/NO for scanning US drivers license barcode standards (default YES, if available by license)
	[coordinatorSettings setValue:@(NO) forKey:kPPRecognizeUSDLKey];
	```

- USDL results are returned as PPUSDLResult object. See a separate document [DriversLicenses](DriversLicenses.md) for information how to obtain data from USDL barcodes.

## 3.0.1
- Each `PPBaseResult` object now has attached information about it's location on processed video frame.
- Demo app updated to draw result position on image which resulted with successful scan

## 3.0.0
- Removed status bar properties from `PPScanningViewController` protocol. Replaced with `preferredStatusBarStyle` and `prefersStatusBarHidden` in `PPOverlayViewController`
- `kPPHudOrientation` replaced with `kPPOverlayShouldAutorotate`.
  - HUD orientation is now determined optimally inside pdf417 library
  - Autorotation can now be explicitly disallowed, e.g when presenting camera on `UINavigationController` or when presenting as FormSheet or PageSheet 
- Removed deprecated methods from `PPOverlayViewController`
- Fixed crash when Overlay those orientations which are not supported by the app. Now overlay works in that situations.
- Completely refactored "Complex Overlay" sample. Basically, it's open sourced implementation of internal pdf417 overlay.
- Fix for M/F values in Sex field. Now that value is standardized to 1 (male) and 2 (female)
- Workarounds for crash with nonstandard FullName data and in internal parseSubfile method

## 2.6.1
- Added support for Photo camera preset
- Added option to automatically detect scale of the barcode on the image. This makes sense only when Photo preset is used.
- Added option to customize the status bar on camera screen
- Fixed "private selectors" issue which appears when publishing the app to app store
- Added a callback for obtaining the image which resulted with successful scan:
	`cameraViewController:didMakeSuccessfulScanOnImage:`
- Changes to API method for retrieving scanning results:
- USDL api modified so that all date fields are in format MMDDYYYY

instead of using 

	- (void)cameraViewController:(id<PPScanningViewController>)cameraViewController
                  obtainedResult:(PPScanningResult*)result
         
Use:

	- (void)cameraViewController:(UIViewController<PPScanningViewController>*)cameraViewController
            	didOutputResults:(NSArray*)results;
            
New method adds an additional layers of abstraction to result obtaining. It makes possible several new features, most importantly, returning more than one recognition result, and returning results other than payslip scanning results (for example, pure OCR or barcode scanning results).

Objects passed in NSArray* `results` are always of type `PPBaseResult`. `PPScanningResult` (object passed in the all callback method) is now a subclass of `PPBaseResult`.

How to implement the new API?

1. Rename `cameraViewController:didFinishWithResult` to, i.e. `processScanningResult`
2. Implement `cameraViewController:didOutputResults`. Commonly, it can be implemented in the following way:

		- (void)cameraViewController:(UIViewController<PPScanningViewController> *)cameraViewController
            		didOutputResults:(NSArray *)results {
    		[results enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
        		if ([obj isKindOfClass:[PPBaseResult class]]) {
            		PPBaseResult* result = (PPBaseResult*)obj;
            		if ([result resultType] == PPBaseResultTypeBarcode && [result isKindOfClass:[PPScanningResult class]]) {
                		PPScanningResult* scanningResult = (PPScanningResult*)result;
                		[self processScanningResult:scanningResult cameraViewController:cameraViewController];
            		}
           			if ([result resultType] == PPBaseResultTypeUSDL && [result isKindOfClass:[PPUSDLResult class]]) {
                		PPUSDLResult* usdlResult = (PPUSDLResult*)result;
                		[self processUSDLResult:usdlResult cameraViewController:cameraViewController];
            		}
        		}
    		}];
		}
3. Test to see if it works.

## 2.6.0
- Improved scanning of Code39 and Code128 barcodes
- Added (basic) support for Aztec and DataMatrix barcodes
- Fixed autofocus problem on iPod Touch 4th Gen
- Package now contains additional file (buildCommit.txt) for easier point version bugfixes
- Fully updated license key generation and verification formats
- Improved scanning speed when using "allow null quiet zone" option
- Updated API for obtaining results in preparation for future API updates: see README.md file, Classic integration step 5.

## 2.5.3
- transition to new license key format

## 2.5.2
- Various bugfixes and performance improvements, especially when scanning uncertain barcodes

## 2.5.1
- Removed all redundant log outputs from iOS build

## 2.5.0
- Added feature for setting scanning region. See readme "Setting scanning region" for details on how to use it.

## 2.4.1
- iPod touch 5th gen now uses 720p video for scanning instead of 480p

## 2.4.0
- Added option to scan barcodes which have inverted intensities
- Added method `[PPBarcodeCoordinator getBuildVersionString]` that returns the internal library version string
- iOS7 deprecated methods are no longer used

## 2.3.1
- various bugfixes

## 2.3.0
- Added option to scan barcodes which don't have quite zone around them
- Improved scanning algorithm

## 2.2.0
- Added out of the box support for FormSheet/PageSheet presentation styles on iPads
- Fixed UI issues when presenting CameraViewController on NavigationViewController
- Refactored default OverlayViewController and provided in demo app. You can use it as a reference in creating your own custom overlays.

## 2.1.0
- Added support for arm64 platforms
- Raised deployment target to iOS 5.1.1. Older versions (up to iOS 4.3) are available on demand.

## 2.0.1
- Added support for front facing cameras

## 2.0.0
- Some type and naming changes in our API. But everything is still backwards compatible!
- Added very simple API for customizing camera overlay UI
- Added option for scanning uncertain barcodes (barcodes which are not encoded according to a standard). 
- API now supports pausing and resuming scanning without dismissing camera screen
- More advanced scanning library - able to scan barcodes on "worn out" plastic id cards

## 1.5.0
- The framework now has an interop feature. Barcode data can be serialized and deserialized in URLs. This means your app can easily subscribe for URL scan requests from other apps and return them scan results.
- Method name change fix in sample app.

## 1.4.1
- Fixed build issues with iOS Simulator when deployment target is set to iOS 7

## 1.4.0
- pdf417 framework can now be built for armv7s architecture which means faster performance on iPhone 5, 5C and 5S, and iPad 4.
- iOS 4.3 is no longer supported

## 1.3.0
- new, twice as fast method for scanning of pdf417 barcodes

## 1.2.4
- iPad 1 and other devices without camera now properly disabled.

## 1.2.3
- Fixed an issue which completely disabled pdf417 scanning on iPad2

## 1.2.2
- UI adjustments for iOS 7
- fixed name clashing with PayPal SDK

## 1.2.1
- bugfix for high resolution scanning on iPad mini.

## 1.2.0
- support for micro PDF417 standard
- support for damaged and non-standard PDF417 barcodes
- support for obtaining raw barcode result
- high resolution support (1080p)

## 1.1.1
- support for case insensitive bundle name matching when checking license key

## 1.1.0
- support for reading 1D barcodes

## 1.0.0
- Fixes for pdf417 scanning of barcodes with error correction 0 or 1
- Improved detection algorithm
- Licensing system fully functional
- Added Podfile for easier integration

## 0.9.0
- Scanning support for pdf417 and QR code
