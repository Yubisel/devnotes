# Generating Optimized Image Formats with Node.js
![Cover image for Generating Optimized Image Formats with Node.js](https://res.cloudinary.com/practicaldev/image/fetch/s--wJilP_0w--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7zftq1hg9nz4y048klgk.jpg)

### [](#introduction)Introduction

Images are an important part of any web application, but they can also be a major source of performance issues if not optimized properly. In this article, we'll cover how to use Node.js and React to automatically generate optimized image formats and display them in the best format for the user's browser.

### [](#setting-up)Setting up

First we need a library that handle image processing for us, and `sharp` is what I chose  

```
npm i sharp

```

Sharp is a high-performance Node.js library for image processing and manipulation. It is designed to be fast and memory-efficient, making it ideal for processing large images and generating multiple image formats.

### [](#generation-script)Generation Script

The first step in optimizing images for the web is to generate multiple formats of each image, each with its own advantages and disadvantages. Some formats, such as JPEG, are good for complex images with many colors, while others, such as WebP, are better for simpler images with fewer colors.

To generate different image formats, we can use Node.js and the Sharp image processing library. Here's an example script that generates avif and webp formats for each image in the images folder:  

```js
const sharp = require('sharp');
const fs = require('fs');

const inputFolder = 'images';
const outputFolder = 'output';

const formats = ['avif', 'webp'];

if (!fs.existsSync(outputFolder)) {
  fs.mkdirSync(outputFolder);
}

fs.readdir(inputFolder, (err, files) => {
  if (err) {
    console.error(err);
    return;
  }

  files.forEach(file => {
    if (file.endsWith('.jpg') || file.endsWith('.jpeg') || file.endsWith('.png')) {
      const inputPath = `${inputFolder}/${file}`;
      const name = file.substring(0, file.lastIndexOf('.'));

      formats.forEach(format => {
        const outputPath = `${outputFolder}/${name}.${format}`;

        if (!fs.existsSync(outputPath)) {
          sharp(inputPath)
            .toFormat(format, { quality: 80 })
            .toFile(outputPath, (err) => {
              if (err) {
                console.error(err);
              } else {
                console.log(`${name}.${format} saved`);
              }
            });
        }
      });
    }
  });
});

```


\*_Explanation:  
\*_  

```js
const sharp = require('sharp');
const fs = require('fs');

const inputFolder = 'images';
const outputFolder = 'output';

const formats = ['avif', 'webp'];

```


In these lines, the script imports the `sharp` and `fs` libraries, sets the input folder to `images`, the output folder to `output`, and defines the formats to be generated as `avif` and `webp`.  

```js
if (!fs.existsSync(outputFolder)) {
  fs.mkdirSync(outputFolder);
}

```


Here, the script checks if the `outputFolder` exists, and if it doesn't, creates it using `fs.mkdirSync()`. This ensures that the output folder exists before generating any images.  

```js
fs.readdir(inputFolder, (err, files) => {
  if (err) {
    console.error(err);
    return;
  }

```


This code reads the contents of the `inputFolder` using `fs.readdir()`. If there is an error, it logs the error to the console and returns.  

```js
files.forEach(file => {
    if (file.endsWith('.jpg') || file.endsWith('.jpeg') || file.endsWith('.png')) {

```


This code loops through each file in the `inputFolder` using `files.forEach()`. If the file name ends with `.jpg`, `.jpeg`, or `.png`, it proceeds to generate the corresponding `avif` and `webp` files.  

```js
const inputPath = `${inputFolder}/${file}`;
      const name = file.substring(0, file.lastIndexOf('.'));

```


Here, the script defines the input file path as `inputPath`, and extracts the file name without the extension to be used as the output file name.  

```js
formats.forEach(format => {
        const outputPath = `${outputFolder}/${name}.${format}`;

        if (!fs.existsSync(outputPath)) {
          sharp(inputPath)
            .toFormat(format, { quality: 80 })
            .toFile(outputPath, (err) => {
              if (err) {
                console.error(err);
              } else {
                console.log(`${name}.${format} saved`);
              }
            });
        }
      });

```

Here, the script loops through each format (i.e. `avif` and `webp`) using `formats.forEach()`. For each format, it defines the output file path as `outputPath`.

If the output file does not already exist, it uses Sharp's `toFormat()` function to generate the corresponding image in the specified format with a quality of 80. It then saves the output file using `toFile()`, and logs a message to the console indicating that the file has been saved.

#### [](#display-optimized-images-in-react)Display Optimized Images in React

Once we have generated multiple optimized image formats for each input image, we can display them in our React application. To do this, we can use the HTML `<picture>` and `<source>` elements to specify the different image sources for different formats. Here's an example React component that takes an image name as a prop and displays the image in the best format for the user's browser:  

```js
import React from 'react';

const Image = ({ name }) => {
  const avifSrc = `/images/${name}.avif`;
  const webpSrc = `/images/${name}.webp`;
  const jpgSrc = `/images/${name}.jpg`;

  return (
    <picture>
      <source srcSet={avifSrc} type="image/avif" />
      <source srcSet={webpSrc} type="image/webp" />
      <img src={jpgSrc} alt={name} />
    </picture>
  );
};

export default Image;

```


Enter fullscreen mode Exit fullscreen mode

This code defines three different image source URLs based on the `name` prop passed in:

*   `avifSrc` corresponds to the `avif` format of the image.
*   `webpSrc` corresponds to the `webp` format of the image.
*   `jpgSrc` corresponds to the standard `jpg` format of the image, which will be used as a fallback for browsers that do not support `avif` or `webp`.

```js
  return (
    <picture>
      <source srcSet={avifSrc} type="image/avif" />
      <source srcSet={webpSrc} type="image/webp" />
      <img src={jpgSrc} alt={name} />
    </picture>
  );
};

```

Here, the script returns a `<picture>` element that displays the image in the best format for the user's browser, based on the available formats. Inside the `<picture>` element, there are two `<source>` elements, one for `avif` and one for `webp`. These elements specify the different image sources for different formats using the `srcSet` attribute and the `type` attribute to indicate the MIME type of each format.

Finally, there is a fallback `<img>` element that displays the image in the standard `jpg` format for browsers that do not support `avif` or `webp`. This element uses the `src` attribute to specify the image source and the `alt` attribute to provide alternate text for the image.

### [](#conclusion)Conclusion

Images on websites can be slow to load and don't always look good on different devices. It's important to make them load faster and look better so people can enjoy your website more. We learned how to use special tools like Sharp and HTML's `<picture>` and `<source>` to make different versions of the same image and show the best one for each device. By doing this, our website will be faster and look better for everyone who uses it!