---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
title: Document Web Capture from Mobile Camera - License
keywords: Documentation, Document Web Capture from Mobile Camera, License
breadcrumbText: License
description: Document Web Capture from Mobile Camera Documentation License
permalink: /gettingstarted/license.html
---

# License

## Get a trial key

- A free public trial license is available for every new device for first use of Document. The public trial license is the default key used in samples. You can also find the public trial license on the following parts of this page.
- If your free key is expired, please visit <a href="https://www.dynamsoft.com/customer/license/trialLicense?product=dwc&utm_source=docs" target="_blank">Private Trial License Page</a> to get a 30-day trial extension.

## Get a full license

- [Contact us](https://www.dynamsoft.com/company/contact/)  to purchase a full license

## Initialize the license

The following code snippets are using the public trial license to initialize the license. You can replace the public trial license with your own license key.

```javascript
// DDV license initialization
await Dynamsoft.DDV.setConfig({
    license: "*Your-License-String*",
    engineResourcePath: "*lead to a folder containing the distributed WASM files*",
});

// DDN license initialization
Dynamsoft.License.LicenseManager.initLicense("*Your-License-String*");
```