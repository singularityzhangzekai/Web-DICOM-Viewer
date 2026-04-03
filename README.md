# OHIF + Orthanc Web Medical Imaging Viewer

A customizable web-based medical imaging viewer built with **OHIF** and **Orthanc**, supporting **DICOM upload**, **study list browsing**, and **web-based image viewing** through **DICOMweb**.

## Preview

> Add your screenshots here

- Study List
- Viewer Page
- Deployment Architecture

## Features

- Web-based medical image viewing
- Orthanc integration via DICOMweb
- Study list browsing
- Local DICOM upload and test workflow
- Custom OHIF configuration
- Ready for reverse-proxy deployment with Nginx
- Easy to extend for UI customization and workflow changes

## Tech Stack

- [OHIF Viewer](https://github.com/OHIF/Viewers)
- [Orthanc](https://www.orthanc-server.com/)
- DICOMweb
- Nginx
- Yarn / Bun

## Architecture

```text
Browser
  -> OHIF Viewer
  -> Nginx (static hosting + reverse proxy)
  -> Orthanc
  -> DICOM Storage
```

## Project Structure

```text
.
├── config/
│   └── local_orthanc.js
├── nginx/
│   └── nginx.conf
├── screenshots/
├── README.md
├── LICENSE
└── .gitignore
```

## Local Development

Make sure Orthanc is running locally and DICOMweb is available.

Start OHIF in development mode with Orthanc proxy:

```bash
PATH="$HOME/.bun/bin:$PATH" yarn run dev:orthanc
```

Then open:

```text
http://localhost:3000
```

## Production Build

Build the viewer with the Orthanc config:

```bash
APP_CONFIG=config/local_orthanc.js PATH="$HOME/.bun/bin:$PATH" yarn run build
```

After build, serve the generated files in:

```text
platform/app/dist
```

A reverse proxy such as Nginx should forward the following paths to Orthanc:

- `/pacs/dicom-web`
- `/pacs/wado`

## Example OHIF Configuration

`config/local_orthanc.js`

```javascript
window.config = {
  routerBasename: null,
  extensions: [],
  modes: [],
  showStudyList: true,
  dataSources: [
    {
      namespace: '@ohif/extension-default.dataSourcesModule.dicomweb',
      sourceName: 'dicomweb',
      configuration: {
        friendlyName: 'Local Orthanc',
        name: 'Orthanc',
        qidoRoot: '/pacs/dicom-web',
        wadoRoot: '/pacs/dicom-web',
        wadoUriRoot: '/pacs/wado',
        qidoSupportsIncludeField: true,
        supportsReject: true,
        imageRendering: 'wadors',
        thumbnailRendering: 'wadors',
        enableStudyLazyLoad: true,
        supportsFuzzyMatching: true,
        supportsWildcard: true,
      },
    },
  ],
  defaultDataSourceName: 'dicomweb',
};
```

## Deployment Notes

For deployment, it is recommended to:

- serve OHIF static assets with Nginx
- reverse proxy DICOMweb requests to Orthanc
- enable HTTPS
- add authentication before using in real environments

## UI Customization

This project is intended to support further customization, such as:

- branding / logo replacement
- toolbar simplification
- mode configuration
- custom overlays and panels
- workflow-specific viewer adjustments

## Roadmap

- [x] Run OHIF locally with Orthanc
- [x] Upload and test DICOM studies
- [x] Verify study list and viewer workflow
- [ ] Add deployment configuration
- [ ] Add custom UI branding
- [ ] Optimize toolbar and workflow
- [ ] Add authentication and HTTPS support

## Important Notes

- Do **not** upload real patient data to this repository
- Use anonymized or demo DICOM files only
- Check deployment security before public use

## License

MIT
