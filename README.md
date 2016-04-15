# PHP QR Code Suite

[![Build Status](https://travis-ci.org/markenwerk/php-qr-code-suite.svg?branch=master)](https://travis-ci.org/markenwerk/php-qr-code-suite)
[![Test Coverage](https://codeclimate.com/github/markenwerk/php-qr-code-suite/badges/coverage.svg)](https://codeclimate.com/github/markenwerk/php-qr-code-suite/coverage)
[![Code Climate](https://codeclimate.com/github/markenwerk/php-qr-code-suite/badges/gpa.svg)](https://codeclimate.com/github/markenwerk/php-qr-code-suite)
[![Latest Stable Version](https://poser.pugx.org/markenwerk/qr-code-suite/v/stable)](https://packagist.org/packages/markenwerk/qr-code-suite)
[![Total Downloads](https://poser.pugx.org/markenwerk/qr-code-suite/downloads)](https://packagist.org/packages/markenwerk/qr-code-suite)
[![License](https://poser.pugx.org/markenwerk/qr-code-suite/license)](https://packagist.org/packages/markenwerk/qr-code-suite)

A collection of classes to QR enccode strings and render them as PNG, TIFF and vectorized EPS.

## Requirements

To use the QR Code Suite [`qrencode`](https://wiki.ubuntuusers.de/qrencode/) and [`imagick`](http://php.net/manual/de/book.imagick.php) have to be available. 

## Installation

```{json}
{
   	"require": {
        "markenwerk/qr-code-suite": "~3.0"
    }
}
```

## Usage

### Autoloading and namesapce

```{php}  
require_once('path/to/vendor/autoload.php');
```

### Encoding data as QR code block data

```{php}
use QrCodeSuite\QrEncode\QrEncoder;

// Encode the data as QR code block data
$encoder = new QrEncoder();
$qrCodeData = $encoder
	->setLevel(QrEncoder::QR_CODE_LEVEL_LOW)
	->setTempDir('path/to/writable/directory')
	->encodeQrCode('https://github.com/markenwerk/php-qr-code-suite');
```

### Render encoded QR code block data as image

```{php}
use QrCodeSuite\QrRender;
use QrCodeSuite\QrRender\Color\RgbColor;
use QrCodeSuite\QrRender\Color\CmykColor;

// Render the encoded QR code block data as RGB PNG
// Width and height should measure about 800 pixels
$renderer = new QrRender\QrCodeRendererPng();
$renderer
	->setApproximateSize(800)
	->setForegroundColor(new RgbColor(255, 0, 255))
	->setBackgroundColor(new RgbColor(51, 51, 51))
	->render($qrCodeData, 'path/to/qr-code.png');

// Get the effective width and height of the rendered image
$imageWidth = $renderer->getWidth();
$imageHeight = $renderer->getHeight();

// Render the encoded QR code block data as CMYK TIFF
// Width and height should measure about 800 pixels
$renderer = new QrRender\QrCodeRendererTiff();
$renderer
	->setApproximateSize(800)
	->setForegroundColor(new CmykColor(0, 100, 0, 0))
	->setBackgroundColor(new CmykColor(0, 100, 100, 0))
	->render($qrCodeData, 'path/to/qr-code.tif');

// Get the effective width and height of the rendered image
$imageWidth = $renderer->getWidth();
$imageHeight = $renderer->getHeight();

// Render the encoded QR code block data as CMYK vectorized EPS
// EPS has no background color. It is just the blocks on blank cnavas.
// EPS also has no approximate size. Scale the vectorized image as you like.
$renderer = new QrRender\QrCodeRendererEps();
$renderer
	->setForegroundColor(new CmykColor(0, 100, 0, 0))
	->render($qrCodeData, 'path/to/qr-code.eps');
```

---

## Contribution

Contributing to our projects is always very appreciated.  
**But: please follow the contribution guidelines written down in the `CONTRIBUTING.md` document.**

## License

QR Code Suite is under the MIT license.