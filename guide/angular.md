---
layout: default-layout
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: false
breadcrumbText: Angular Guide
title: Mobile Document Scanner JS Edition - Angular
keywords: Documentation, Mobile Document Scanner, Web, JS Edition, Dynamsoft Document Scanner, Angular
description: Mobile Document Scanner JS Edition Angular User Guide
---

# Document Scanner - Angular TypeScript

> [!IMPORTANT]
> This article builds on the prerequisite [MDS developer guide]({{ site.developer-guide }}index.html) for plain JavaScript, please read it before proceeding.

Mobile Document Scanner integrates easily with Angular applications. Just as in plain JavaScript, you can add document scanning in your Angular application in just a few lines of code, and achieve most customizations through the same accessible configuration object.

## Features

- Easy integration with pre-built UI
- Render MDS inside an Angular component
- **TypeScript support** with proper type definitions
- **Standalone components** (modern Angular architecture)
- Capture and process documents from video stream
- Automatic document detection and cropping
- Mobile-optimized scanning interface

## Requirements

- **Node.js 20.19.0 or later**
- [Base requirements]({{ site.introduction }}index.html#system-requirements)

## License

### Get a Trial License

Try **MDS** by requesting a trial license through our [customer portal](https://www.dynamsoft.com/customer/license/trialLicense?product=mwc&source=guide). The trial can be renewed twice, providing a total of two months of free access.

### Get a Full License

[Contact us](https://www.dynamsoft.com/company/contact?product=mwc&utm_source=github_angular_readme) to purchase a full license.

## Quick Start

### Install Dependencies

```bash
npm install
```

### Configure License Key

Search for the string `"YOUR_LICENSE_KEY_HERE"` in the sample and replace it with your actual license key.

### Get a Trial License

You can try **MDS** by requesting a trial license through our [customer portal](https://www.dynamsoft.com/customer/license/trialLicense?product=mwc&utm_source=github_angular_readme). The trial can be renewed twice for up to two months of free access.

> [!NOTE] > **MDS** and **MWC** share the same license keys. The license portal currently shows "Mobile Web Capture" but requesting this license will provide access to both **MDS** and **MWC** functionality. If you already have a **MWC** license, you can use it for **MDS**, and vice versa.

### Get a Full License

To purchase a full license, [contact us](https://www.dynamsoft.com/company/contact?product=mwc&utm_source=github_angular_readme).

### 3. Start the App

```bash
ng serve
```

> [!TIP]
> If you installed Angular locally, you can call the local version with
> ```shell
> npx ng serve
> ```

Then open http://localhost:4200/ to view the sample app.

> **Note:** HTTPS is required for camera access on mobile devices. Angular CLI serves over HTTP by default. For mobile testing, you may need to configure HTTPS or use a reverse proxy.

## 📌 Customization

Please check the official [documentation](https://www.dynamsoft.com/mobile-document-scanner/docs/web/introduction/index.html).

## 📄 Support

If you have any questions, feel free to [contact Dynamsoft Support](https://www.dynamsoft.com/company/contact?product=mwc&utm_source=github_angular_readme).
