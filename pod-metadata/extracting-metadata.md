# Extracting metadata

Most images (if taken by a smartphone camera) actually store metadata inside - things like time, location and others. Those can be extracted from the image (for example at upload time). This is known as "EXIF".

## What the meta data looks like

Real example from my phone:

```json
{
  "ImageWidth": 4128,
  "ImageHeight": 3096,
  "Make": "samsung",
  "Model": "SM-A105FN",
  "Orientation": 1,
  "ResolutionUnit": 2,
  "Software": "A105FNXXU3ATA1",
  "DateTime": "2020:05:19 15:21:27",
  "YCbCrPositioning": 1,
  "ExifIFDPointer": 240,
  "GPSInfoIFDPointer": 658,
  "ExposureProgram": "Normal program",
  "ISOSpeedRatings": 40,
  "ExifVersion": "0220",
  "DateTimeOriginal": "2020:05:19 15:21:27",
  "DateTimeDigitized": "2020:05:19 15:21:27",
  "BrightnessValue": 14.68,
  "ExposureBias": 0,
  "MeteringMode": "CenterWeightedAverage",
  "Flash": "Flash did not fire",
  "ColorSpace": 1,
  "PixelXDimension": 4128,
  "PixelYDimension": 3096,
  "ExposureMode": 0,
  "WhiteBalance": "Auto white balance",
  "FocalLengthIn35mmFilm": 27,
  "SceneCaptureType": "Standard",
  "ImageUniqueID": "D13LLMA00HM",
  "GPSLatitudeRef": "N",
  "GPSLatitude": [
    50,
    48,
    1.065959
  ],
  "GPSLongitudeRef": "E",
  "GPSLongitude": [
    4,
    20,
    34.243439
  ],
  "thumbnail": {
    "ImageWidth": 512,
    "ImageHeight": 384,
    "Compression": 6,
    "ResolutionUnit": 2,
    "JpegIFOffset": 878,
    "JpegIFByteCount": 56361,
    "blob": {}
  }
}
```

(Most) interesting parts are:

- DateTimeOriginal
- GPSLongitude/GPSLatitude

## Extracting EXIF in JavaScript

This can be done ideally at image upload time - do it once, save in metadata file so it can be used/searched on.

```bash
yarn add exif-js
```

```javascript
import EXIF from "exif-js"

let raw = ... //this is the raw image as obtained from getFile()

// EXIF lib require a buffer - we have a blob, that's not the same thing
// Careful - arrayBuffer() is async - need await or then here
let arrayBuffer = await new Response(raw).arrayBuffer()
let exifData = EXIF.readFromBinaryFile(arrayBuffer)
console.log(exifData)

let dateTime = exifData.DateTime ? exifData.DateTime.replace(":","/").replace(":","/") : undefined
let latitude = exifData.GPSLatitude && exifData.GPSLatitude[0] ? convertGPS(exifData.GPSLatitude) : null
let longitude = exifData.GPSLongitude && exifData.GPSLongitude[0] ? convertGPS(exifData.GPSLongitude) : null
```

