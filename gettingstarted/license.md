---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
title: Mobile Web Capture - License
keywords: Documentation, Mobile Web Capture, License
breadcrumbText: License
description: Mobile Web Capture Documentation License
permalink: /gettingstarted/license.html
---

# License

## Get a trial key

- A free public trial license is available for every new device for first use of Mobile Web Capture. The public trial license is the default key used in samples. You can also find the public trial license on the following parts of this page.
- If your free key is expired, please visit <a href="https://www.dynamsoft.com/customer/license/trialLicense?product=mwc&source=docs" target="_blank">Private Trial License Page</a> to get a 30-day trial extension.

## Get a full license

- [Contact us](https://www.dynamsoft.com/company/contact/)  to purchase a full license

## Initialize the license

The following code snippet is using the public trial license to initialize the license. You can replace the public trial license with your own license key.

```javascript
await Dynamsoft.License.LicenseManager.initLicense(
    "DLS2eyJoYW5kc2hha2VDb2RlIjoiMjAwMDAxLTEwMjQ5NjE5NyJ9",
    true
); // Public trial license which is valid for 24 hours
```
