<div align="center">

# 🖨️ HTML to PDF Generator

**Convert any HTML element or string to a downloadable PDF — entirely in the browser. No server, no puppeteer, no headless Chrome.**

[![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?style=flat-square&logo=javascript&logoColor=black)](https://developer.mozilla.org)
[![jsPDF](https://img.shields.io/badge/jsPDF-2.x-FF6B35?style=flat-square)](https://github.com/parallax/jsPDF)
[![html2canvas](https://img.shields.io/badge/html2canvas-1.x-4479A1?style=flat-square)](https://html2canvas.hertzen.com)
[![No server](https://img.shields.io/badge/server-not%20required-brightgreen?style=flat-square)](https://github.com/harryatwork/html-pdf-generator)
[![License](https://img.shields.io/badge/license-MIT-a855f7?style=flat-square)](LICENSE)

[Quick Start](#-quick-start) · [Usage](#-usage) · [Options](#️-options) · [Multi-page](#-multi-page-output)

</div>

---

## The Problem

Most PDF-from-HTML solutions require a server process running puppeteer or wkhtmltopdf. That's infrastructure, security surface, and cold-start latency — for something the browser can do natively. This library composes html2canvas + jsPDF to render any styled DOM node to a pixel-accurate PDF, client-side, with one function call.

---

## ✨ Features

- 🚀 **100% client-side** — no server, no puppeteer, no headless Chrome needed
- 🎨 **Pixel-accurate rendering** — html2canvas captures your CSS styles faithfully
- 📄 **Multi-page support** — auto-splits tall content across A4 pages
- 📐 **Configurable page size** — A4, A3, Letter, custom dimensions
- 🔤 **Preserves fonts** — loads @font-face fonts before capture
- 💾 **Auto-download** — triggers browser save dialog on completion
- 🖼️ **Images included** — base64-encodes linked images into the PDF
- ⚡ **Progress callback** — show a spinner while rendering

---

## 🔧 How It Works

```
DOM element (with all CSS applied)
          │
    html2canvas.render()
          │
    <canvas> element (pixel snapshot of the DOM)
          │
    canvas.toDataURL('image/jpeg', quality)
          │
    jsPDF.addImage(dataUrl, format, x, y, width, height)
          │
    Tall content? slice into page-height chunks → addPage() per chunk
          │
    jsPDF.save('filename.pdf')   ← triggers browser download
```

---

## 🚀 Quick Start

### 1 — Include the libraries

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="html-pdf-generator.js"></script>
```

### 2 — Add a trigger button

```html
<div id="printable-area">
  <h1>Invoice #1042</h1>
  <p>Bill to: Acme Corp</p>
  <!-- Your full HTML content -->
</div>

<button id="download-pdf">Download PDF</button>
```

### 3 — Wire it up

```javascript
document.getElementById('download-pdf').addEventListener('click', function() {
  HtmlPdfGenerator.generate({
    element: document.getElementById('printable-area'),
    filename: 'invoice-1042.pdf'
  });
});
```

---

## 📋 Usage

### Generate from a DOM element

```javascript
HtmlPdfGenerator.generate({
  element: document.querySelector('#my-report'),
  filename: 'monthly-report.pdf',
  orientation: 'portrait',   // 'portrait' | 'landscape'
  format: 'a4',              // 'a4' | 'a3' | 'letter'
  quality: 0.95,             // Image quality 0–1
  onStart:    () => showSpinner(),
  onComplete: () => hideSpinner(),
});
```

### Generate from an HTML string

```javascript
HtmlPdfGenerator.fromHtml(
  '<h1>Dynamic Report</h1><p>Generated at ' + new Date() + '</p>',
  { filename: 'dynamic-report.pdf' }
);
```

---

## 📄 Multi-Page Output

For long content, the generator automatically slices the canvas at A4 page boundaries:

```javascript
HtmlPdfGenerator.generate({
  element: document.getElementById('long-table'),
  filename: 'full-data-export.pdf',
  pageBreaks: true,         // Default true — auto splits at page height
  margin: { top: 20, right: 15, bottom: 20, left: 15 }  // mm
});
```

---

## ⚙️ Options

| Option | Type | Default | Description |
|---|---|---|---|
| `element` | `HTMLElement` | *required* | The DOM node to capture |
| `filename` | `string` | `'download.pdf'` | Output filename |
| `orientation` | `string` | `'portrait'` | `'portrait'` or `'landscape'` |
| `format` | `string` | `'a4'` | Page format: `'a4'`, `'a3'`, `'letter'`, or `[width, height]` |
| `quality` | `number` | `0.95` | JPEG quality 0–1 for image encoding |
| `scale` | `number` | `2` | Device pixel ratio for retina-quality output |
| `margin` | `object` | `{top:10,right:10,bottom:10,left:10}` | Page margins in mm |
| `pageBreaks` | `boolean` | `true` | Auto-split tall content across pages |
| `onStart` | `function` | `null` | Fired before rendering starts |
| `onComplete` | `function` | `null` | Fired after save dialog triggers |

---

## 📁 Project Structure

```
html-pdf-generator/
├── html-pdf-generator.js       # Core generator class
├── html-pdf-generator.min.js   # Minified build
├── demo/
│   ├── index.html              # Demo with invoice + report examples
│   ├── invoice.html            # Sample invoice template
│   └── demo.css                # Demo styles
└── README.md
```

---

<details>
<summary><strong>Common issues and fixes</strong></summary>

| Issue | Fix |
|---|---|
| PDF is blank | The element must be visible in the DOM and not `display:none` during capture |
| Images not appearing in PDF | Serve images from the same origin or enable CORS headers on the image server |
| Text looks blurry | Increase `scale` to `3` for sharper text at the cost of file size |
| Font not matching browser | Ensure `@font-face` fonts are fully loaded before calling `generate()` — use `document.fonts.ready` |
| Download not triggering | Some browsers block programmatic downloads — call from a real user click event |

</details>

---

<div align="center">

Built by [Harish K](https://github.com/harryatwork)

</div>