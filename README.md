
# Chroma
![Chroma](http://i.imgur.com/8p9f3Gu.png)

Chroma is color conversion library that implements perceptually linear color spaces such as CIELab &amp; CIE-LCH. This library borrows CIE conversions from the very popular [Chroma.js](https://github.com/gka/chroma.js "Github Link") JavaScript color conversion library.

Download it here: [Chroma](http://neil.engineer/index.php?/articles/chroma/ "Chroma")

Fork it on Github: [Github Repository](https://github.com/neilpanchal/Chroma "Github Repository")


## Motivation

Artists & designers benefit tremendously from being able to control luminosity in a linear color space. In order to conceive visual distraction, emphasis, phase, depth, importance, or an abstract artistic visual hierarchy; the ability to control luminisity is indispensible.

The human vision has evolved to prioritize luminosity information during the object recognition process. Color spaces such as **HSL**, **HSV/HSB** and **RGB** are optimized for digital displays and lack luminosity control. For example, varying Hue while keeping Saturation and Lightness constant in HSL colorspace produces perceptually non-uniform colors. Moreover, Lightness is a non-linear function of both - Saturation and Hue. This severely limits the ability to predict the apparent and perceptual luminosity.

Chroma library allows for color production in **CIE-Lab** and **CIE-LCH** (cyclindrical transfomration of CIE-Lab) which are better suited for human vision and perceptual predictibility. Chroma objects can be instantiated with CIE color parameters, but represented and drawn in RGB color space.

<br />
![Hue Swatches](http://i.imgur.com/Jj8oScc.png)
<br />

As a demonstration, the swatches shown above exemplify the drawbacks of HSL colorspace. The top row of swatches are generated by changing the Hue `[H = {0, 45, 90, 135, 180, 225, 270, 315}]` while keeping Saturation `[S = 50]` and Lightness `[L = 50]` constant. The bottom row is generated in CIE-LCH color space by setting Luminosity `[L = 80]` and Chroma `[C = 50]` while changing the Hue `[H = {0, 45, 90, 135, 180, 225, 270, 315}]`. The result is a perceptually uniform palette and as a consequence, it is visually pleasant.

<br />
![Gray Scale Conversion](http://i.imgur.com/5sXovcz.png)
<br />
Perceptual uniformity is apparent when the colors are desatured in OSX Preview app (which probably uses CIELab space to color conversion). The bottom band of swatches have the same gray scale value.

It is important to understand that CIE-LCH color space is ~50% larger than sRGB and high chroma colors do not have a RGB representation as illustrated in the Hue vs. Chroma plot (Luminosity = 60) below. See API documentation under the `clipped()` section for further details on how to make sure the color conversions are accurate.

<br />
![Hue vs. Chroma Plot](http://i.imgur.com/VqbbGO2.png)
<br />

## Code Sample

```processing
import com.chroma.*; // Import Chroma library

int l = 50; // Luminosity, Range: 0-100
int c = 70; // Chroma, Range: 0-128
int h = 0; // Hue, Range: 0-360

Chroma testColor; // Declare a chroma object

void setup() {
    size(1280, 720);
    rectMode(CENTER);
    noStroke();

    testColor = new Chroma(ColorSpace.LCH, l, c, h, 255); 
    // Create a chroma object
    
    println("Valid RGB Color: " + !testColor.clipped()); 
    // Check if the RGB values are clipped
}

void draw() {

    background(255);


    fill(testColor.get()); 
    // To fetch RGB color, use the getRGB method.
    
    rect(width/2, height/2, 100, 100); 
    // Draw a magenta square
}
```

## API Reference

`testColor` is `#FF0000` or Red unless otherwise specified.

### Color Spaces

Colors can be created or converted to one of the following color spaces:
* RGB Color Space (Default)
* HSL Color Space (Cylindrical transformation of RGB)
* HSV Color Space (Cylindrical transformation of RGB)
* LAB Color Space (CIE-Lab)
* LCH Color Space (Cylindrical transformation of CIE-Lab)


### Color Initialization

Quick refernce for creating Chroma colors.
```processing
// All colors below produce a Red (#FF0000)

Chroma testColorString = new Chroma("Red");
Chroma testColorHex = new Chroma("#FF0000");
Chroma testColorRGB = new Chroma(ColorSpace.RGB, 255, 0, 0, 255);
Chroma testColorHSL = new Chroma(ColorSpace.HSL, 0, 1, 0.5, 255);
Chroma testColorHSV = new Chroma(ColorSpace.HSV, 0, 1, 1, 255);
Chroma testColorLAB = new Chroma(ColorSpace.LAB, 53.24, 80.09, 67.20, 255);
Chroma testColorLCH = new Chroma(ColorSpace.LCH, 53.24, 104.55, 40.00, 255);

```

### Constructors

##### Chroma ()

Default constructor with no arguments. Default Alpha is 255 (Opaque).
```processing
Chroma testColor = new Chroma();
// Create a White(#FFFFFF) color with Alpha = 255
```

##### Chroma (grayscale)

Constructor with only one argument.
```processing
Chroma testColor = new Chroma(128);
// Create a Mid Gray(#808080) color with Alpha = 255
```

##### Chroma (red, green, blue)

Constructor with three arguments.
```processing
Chroma testColor = new Chroma(255, 0, 0);
// Create a Red(#FF0000) color with Alpha = 255
```

##### Chroma (colorspace, input1, input2, input3)

Constructor with a specified colorspace and three arguments.
```processing
Chroma testColor = new Chroma(ColorSpace.RGB, 255, 0, 0);
// Create a Red(#FF0000) color with Alpha = 255
```

##### Chroma (colorspace, input1, input2, input3, input4)

Constructor with all arguments.
```processing
Chroma testColor = new Chroma(ColorSpace.RGB, 255, 0, 0, 255);
// Create a Red(#FF0000) color with Alpha = 255
```

##### Chroma (hex)

Constructor with a hexadecimal color string format "#FF0000". Input is not case sensitive.
```processing
Chroma testColor = new Chroma("#FF0000");
// Create a Red(#FF0000) color with Alpha = 255
```

Constructor with a hexadecimal color string format "#F00".

```processing
Chroma testColor = new Chroma("#F00");
// Create a Red(#FF0000) color with Alpha = 255
```

##### Chroma (colorname)

Constructor with a standard CSS color name. For example, "Red". Input is not case sensitive.
```processing
Chroma testColor = new Chroma("Red");
// Create a Red(#FF0000) color with Alpha = 255
```


### Color Information

#### Get Methods
##### Chroma.get ()
`Chroma.get()` method can be used with a number of ways. By default, it returns an integer representation of the RGB components. The output integer has the following bit assignments:
`AAAAAAAARRRRRRRRGGGGGGGGBBBBBBBB`, a total of 32 bits or 4 bytes, each representing (from most significant bit to the least) - Alpha, Red, Green and Blue channels.

```processing
testColor.get();
// Returns a 4 byte (32-bit) integer or 0xFFFF0000 for Red (#FF0000)
```

##### Chroma.getAlpha()

```processing
testColor.getAlpha();
// Returns Alpha Channel = 255 (Opaque)
```

##### Chroma.getLuminance ()
```processing
Chroma testColor = new Chroma ("red");
// Create a Red(#FF0000) color with Alpha = 255

testColor.getLuminance();
// Returns 0.212600
```

##### Chroma.get(ColorSpace)
Supported ColorSpace arguments: RGB, HSL, HSV, LAB, LCH. Color conversion will happen automatically if it was created in a different color space.
```processing
testColor.get(ColorSpace.RGB);
// Returns RGB component array: { 203.0, 59.0, 161.0 }

testColor.get(ColorSpace.HSL);
// Returns HSL component array: { 317.5, 0.5806, 0.5137 }

testColor.get(ColorSpace.HSV);
// Returns HSV component array: { 317.5, 0.7094, 0.7961 }

testColor.get(ColorSpace.LAB);
// Returns LAB component array: { 50.0, 65.7785, -23.9414 }

testColor.get(ColorSpace.LCH);
// Returns LCH component array: { 50.0, 70.0, 340.0 }
```

##### Chroma.get(ColorSpace, Channel)
Channel must be a valid component of the ColorSpace. Supported Channel arguments are dependent upon the ColorSpace in context.

```processing
testColor.get(ColorSpace.RGB, Channel.R);
// Returns 255
```

`testColor.get(ColorSpace.RGB, Channel.L);` will result in an invalid argument exception as `Channel.L` is not a component of `ColorSpace.RGB`.

#### Set methods

##### Chroma.set(ColorSpace, input1, input2, input3)

```processing
testColor.set(ColorSpace.RGB, 0, 255, 0); 
// Sets testColor to green
```

##### Chroma.set(ColorSpace, Channel, input)

```processing
testColor.set(ColorSpace.RGB, Channel.G, 255); 
// Sets testColor to green
```

### Color Conversions

##### RGB to LAB conversion
```processing
Chroma testColorRGB = new Chroma(ColorSpace.RGB, 255, 0, 0, 255);
// Create a red test color

testColorRGB.get(ColorSpace.LAB);
// Returns { 53.240794589926296,    80.09245948458054,  67.203196401666}
```

##### LCH to RGB conversion
```processing
Chroma testColorLCH = new Chroma(ColorSpace.LCH, 50, 70, 340, 255); 
// Creates a magenta test color

testColorLCH.get(ColorSpace.RGB);
// Returns { 203.0,     59.0,   161.0 }
```

##### Clipped colors in RGB space
Care must be taken when dealing with LAB/LCH colors. The sRGB color space is ~50% of the total colors represented in CIE domain. They may lie outside of the RGB color space and therefore the conversion is not possible and invalid. In the case when a color does not have a valid RGB color, its components are clamped between 0-255. Converting from CIE to RGB and back to CIE will not return the same initial CIE color due to the clipping.

If the color is clipped, the `clipped()` method can be used to check the clipped status flag.

```processing
testColorLCH.clipped();
// If True: One of the R, G or B channels is clipped
```

### Color Processing

#### Chromatic Processing
These methods manipulate the chromacity of the tint without changing luminosity or hue. 

##### saturate()
The `saturate()` method uses a binary search algorithm to find a valid RGB color with maximum chromacity in CIE-LCH color space. As previously noted, the luminosity and hue remains unchanged.

```processing
testColor = new Chroma(ColorSpace.LCH, 50, 20, 0);
// Create a magenta tint

fill(testColor.get()); // Draw tint as the background
rect(width / 2, height / 2, 600, 600);

fill(testColor.saturate().get()); // Fully saturated tint
rect(width / 2, height / 2, 300, 300);
```

<br />
![Saturate](http://i.imgur.com/aV8mj8a.png)
<br />



To better illustrate the `saturate()` method, below is a plot of Hue vs. Chromacity at a constant Luminosity (L = 71). Original tint is indicated by a square marker and their corresponding saturated color is shown as circular markers. 

<br />
![Maximize Chroma](http://i.imgur.com/uVx3sik.png)
<br />

##### saturate(amount)
By default, `saturate()` will return a fully saturated valid RGB color. Amount of saturation from the original tint can be supplied as an argument from 0 - 100 %. 

```processing
testColor = new Chroma(ColorSpace.LCH, 50, 20, 0);

for (int i = 0; i < squares; i++) {
    fill(testColor.saturate(map(i, 0, squares, 0, 100)).get());
    rect(width / 2, height / 2, 600* (squares-i)/squares, 600* (squares-i)/squares);
}
```
<br />
![Saturate Amount](http://i.imgur.com/NrLyWnt.png)
<br />

##### saturateTo(amount)
`saturateTo(amount)` can be used to push Chroma color to a specific level of intensity. The function will make sure that the Chroma color is not clipped in the CIE color space. If the requested chroma amount is larger than the maximum chroma (as returned by the `getMaxChroma()` function), the returned intensity is clipped at the max chroma of the color. This function is useful when matching different colors to an absolute (0-128) chroma values. 

```processing
testColor = new Chroma(ColorSpace.LCH, 50, 20, 0);
testColor.saturateTo(40);
// Returns a testColor with {50, 40, 0} LCH values.
}
```

##### saturateBy(amount)
`saturateBy(amount)` can be used to push Chroma color by a specific level of intensity. The function will make sure that the Chroma color is not clipped in the CIE color space. If the requested chroma amount is larger than the maximum chroma (as returned by the `getMaxChroma()` function), the returned intensity is clipped at the max chroma of the color. This function is useful when adding a fixed amount of intensity to a color.

```processing
testColor = new Chroma(ColorSpace.LCH, 50, 20, 0);
testColor.saturateBy(40);
// Returns a testColor with {50, 60, 0} LCH values.
}
```

##### tint(amount)
Similar to the `saturate(amount)` method, `tint()` returns a Chroma color with a chomacity proportional to the absolute range (0-maximum). 

```processing
testColor = new Chroma(ColorSpace.LCH, 50, 20, 0);

for (int i = 0; i < squares; i++) {
    fill(testColor.tint(map(i, 0, squares, 0, 100)).get());
    rect(width / 2, height / 2, 600* (squares-i)/squares, 600* (squares-i)/squares);
}
```

<br />
![Tint Amount](http://i.imgur.com/Fz9nk8T.png)
<br />

## Tests

Chroma includes several tests & examples and they are can be loaded within the Processing IDE by clicking: `File > Examples > Library Examples > Chroma`.

Alternatively, you can find the example source code under `Processing/libraries/Chroma/examples`.

## Installation

### How to install library Chroma


Install with the "Add Library..." tool

New for Processing 2.0: Add contributed libraries by selecting "Add Library..."
from the "Import Library..." submenu within the Sketch menu. Not all available
libraries have been converted to show up in this menu. If a library isn't there,
it will need to be installed manually by following the instructions below.


### Manual Install

Contributed libraries may be downloaded separately and manually placed within
the "libraries" folder of your Processing sketchbook. To find (and change) the
Processing sketchbook location on your computer, open the Preferences window
from the Processing application (PDE) and look for the "Sketchbook location"
item at the top.

Copy the contributed library's folder into the "libraries" folder at this
location. You will need to create the "libraries" folder if this is your first
contributed library.

By default the following locations are used for your sketchbook folder:
For Mac users, the sketchbook folder is located inside `~/Documents/Processing`.
For Windows users, the sketchbook folder is located inside
`My Documents/Processing`.

The folder structure for library Chroma should be as follows:

```
Processing
    libraries
        Chroma
              examples
              library
                    Chroma.jar
              reference
              src
```

Some folders like "examples" or "src" might be missing. After library
Chroma has been successfully installed, restart the Processing
application.


If you're having trouble, have a look at the Processing Wiki for more
information: http://wiki.processing.org/w/How_to_Install_a_Contributed_Library

## License

```
The MIT License (MIT)

Copyright (c) 2015 Neil Panchal, http://neil.engineer

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
