# 📄 HTML to PDF Generator

A PHP HTML-to-PDF generator — pass any HTML content and receive a clean, downloadable PDF. Built on a lightweight PHP PDF library with no external binary dependencies.

## Usage
```php
require 'pdf-generator.php';
\ = new PDFGenerator();
\->loadHTML('<h1>Hello</h1><p>World</p>');
\->download('output.pdf');
```