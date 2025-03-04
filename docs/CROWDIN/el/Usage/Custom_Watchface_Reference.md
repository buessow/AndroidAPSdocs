# Custom Watchface reference document

This page is for Designer of new Watchfaces, it will list all the key words and features available when you want to create or animate a new watchface

## Custom Watchface format

Custom Watchface is an open format designed for AAPS and associated to the new "Custom Watchface" available on Watch

Watchface file is a simple zip file, but to be recognize as a Watchface file, it must containts:

- One image file named CustomWatchface (can be bitmap files `CustomWatchface.jpg`, `CustomWatchface.png` or a vector `CustomWatchface.svg`). This file is the little icon used to select the watchface when you click on "Load Watchface" button, and also the image visible within AAPS Wear plugin
- One file named `CustomWatchface.json`. This second file is the core file that will include all information required to design the watchface. This json file must be valid (it's probably the most tricky point when you edit manually this file within a text editor, because an missing or additional comma is enough to brake the json format). This json file must also include a `"metadata"` bloc with a `"name"` key with not empty value. This will be the name of your custom watchface (see §§§ below)
- the size of this zip should be as small as possible (less than about 350kb). If this file is too big, it will just be blocked and not transmitted to the watch.

The zip file can also containts some additional resource files:

- HardCoded file names for images that will be used used into standard views included into watchface (like `Background`, `CoverChart`... see §§§ below). All these files can be either `jpg`, `png` or `svg` format. but for most of them, you will have to use `png` or `svg` that manage transparency (jpg are smaller in size compared to png, but with no transparency). Note that the best quality associated to the smallest size will generally be with svg files (vector format).
- addtional resource files with free names. These additional files can be either image files, or font files (`ttf` and `otf` format are accepted for fonts). Note that for these additional files, the `filename` (without extension) will be used as the keyValue, within json file, to specify where or when these files should be used
  - image files are often used as background of text views or for dynamic animation (like battery level from 0% to 100%)
  - font files allow you to use dedicated fonts within your watchface



## Json structure

json files can be edited in Notepad (or notepad++) text editor (prefer notepad++ that recognize json and use color formating)

- it contains string keys `"string_key":` and key values that can be strings like `"key value"`, integer, boolean like `true`or `false` or block of data.
- each value is seperated by a comma `,`
- A block of data starts by `{`  and ends by `}`
- the json file is a whole block so it starts by  `{`  and ends by `}`, and inside this file all embeded blocks are associated to a `"key"` that should be unique within the block
- To improve readibility of json file, it's generally indented (each new key is on a new line, each new block is shifted on the right by 4 spaces characters)

### metadata settings

This block is the first block included into the json file and is mandatory. It contains all the informations associated to this watchface, like the name, the author, the date of creation or update, the author version or the plugin version.

See below an example of metadata block:

```json
    "metadata": {
        "name": "Default Watchface",
        "author": "myName",
        "created_at": "07\/10\/2023",
        "author_version": "1.0",
        "cwf_version": "1.0",
        "comment": "Default watchface, you can click on EXPORT WATCHFACE button to generate a template"
    },
```

Note that `/` used for the date is a special character, so to be recognize correctly within json file, you have to put before an "escape" character `\`

You can see in some json file an additional key `"filename"`, this key will be automatically created or updated when the custom watchface will be loaded within AAPS (it will be used to show to the user the zip filename within exports folder), so you can remove this key within metadata block.

### General parameter settings

After the first block with metadata, you will set some global parameters (see §§§), this allow you to set Graph colors (Carbs, Bolus, BG values...), and also default colors for value in range, hyper or hypo (default colors of BG value and arrows)

See below an example of general parameters

```json
"highColor": "#FFFF00",
"midColor": "#00FF00",
"lowColor": "#FF0000",
"lowBatColor": "#E53935",
"carbColor": "#FB8C00",
"basalBackgroundColor": "#0000FF",
"basalCenterColor": "#64B5F6",
"gridColor": "#FFFFFF",
"pointSize": 2,
"enableSecond": true,
```
### ImageView settings

Custom image can be tuned using correct filename associated to each ImageView included into custom watchface Layout, then the json block is only here to define the position, the size, if the view is visible or not, and optionally tune the color:

See below an example of an Image block for second_hand, (in this case there are no image included into zip file so default second hand image will be used, but tuned with a custom color.

```json
"second_hand": {
    "width": 400,
    "height": 400,
    "topmargin": 0,
    "leftmargin": 0,
    "visibility": "visible",
    "color": "#BC906A"
}
```
To have second_hand colored with default BG color (lowRange, midRange or highRange), you just have to modify the latest ligne with the keyValue `bgColor`

```json
    "color": "bgColor"
```

### TextView settings

TexView have more available parameters compare to ImageView: you can tune rotation (integer value in degrees), textsize (integer value in pixel), gravity (to define if text value will be centered (default value), or aligned left or right), and set the font, fontStyle and fontColor

```json
"basalRate": {
    "width": 91,
    "height": 32,
    "topmargin": 133,
    "leftmargin": 249,
    "rotation": 0,
    "visibility": "visible",
    "textsize": 23,
    "gravity": "center",
    "font": "default",
    "fontStyle": "bold",
    "fontColor": "#BDBDBD"
},
```
Note that if you don't want to manage one view within your watchface, then put the `"visibility"`key to `"gone"`but also set size and position outside visible area like that:

```json
"second": {
    "width": 0,
    "height": 0,
    "topmargin": 0,
    "leftmargin": 0,
    "rotation": 0,
    "visibility": "gone",
    "textsize": 46,
    "gravity": "center",
    "font": "default",
    "fontStyle": "bold",
    "fontColor": "#BDBDBD"
},
```
If size and position are within visible area, you can get some "flash" of the hidden value during the refresh of the watchface.

If you want to customize background image of a text view, then you can use the key `"background":` and put the filename of image included into zip file as keyValue.

```json
    "background": "fileName"
```

You also have 4 specific textViews (named freetext1 to freetext4) that have a specific parameter `"textvalue":` that can be used to set for example a label

### ChartView settings

Chart view is a very specific view that can share some parameters with ImageView or with TextView...

Standard settings for this view is very simple:

```json
"chart": {
    "width": 400,
    "height": 170,
    "topmargin": 230,
    "leftmargin": 0,
    "visibility": "visible"
},
```
The 2 additional parameters you can include for Chart view is a background color (default is transparent), using `"color"` key or a background image using `"background"` key.

## How to build/design your first Watchface

### Tools required

- Text editor: my advice is to use NotePad++ (or equivalent) that is a simple text editor, but added value is you can see formated text with color code, so it's easier to detect errors. any simple text editor do the jod. it will be used to tune json informations
- Image editor (bitmap and/or vector)
  - If you use Bitmap
    - Image editor should be able to manage transparency (required for all image above background), and png format (if you used bitmap image)
    - Background image can  be in jpg format (smaller that png)
    - Image editor should allow you to measure in pixel graphical objects (can be a simple square) (top, left, width, heigth)
    - Image editor should be able to show you colors with RRVVBB code in hexadecimal
    - Image editor should be able to resize image to 400px x 400px (very important to work with this resolution)
  - If you use Vector
    - Vector image should be exported in svg format

### Get template to not start from scratch

When you want to design your first watchface, the best is to start by the default watchface (this will ensure you to have latest version with all available views correctly sorted)

- You can get zip file by clicking on "Export Template" button within Wear plugin and get zip file within AAPS/exports folder
- Note that you will need to have a watch connected to AAPS to see Custom Watchface buttons (but watch is also required to check, test and tune your custom watchface)

Default watchface is very simple and zip file will containts only the 2 files:

- CustomWatchface.png (image of default watchface for WF selection)
- CustomWatchface.json

### Organize your files within your computer

The easiest way to work is to have phone connected to the computer, and work with to specific folders:

- one explorer opened on a specific folder that will have all files (json, bitmap images, vector images, fonts), and the CustomWatchface.zip file within it
- another explorer (or navigation tree tuned) opened to have Phone/AAPS/exports folder available

That way working is very easy: each time you tune json file with a text editor, image with image editor (bitmap or vector) you have to just:

1. save your modifications in each app
2. drag and drop all files within CustomWatchface.zip file
3. drag and drop CustomWatchface.zip into AAPS/exports folder of the phone
4. send CustomWatchface to the watch to check the results

### Initialize Watchface customization

First step you will have to define a watchface Name (necessary to select it easily for testing), and start to tune metadata keys at the begining of json file

Then you will have to define which information you want to show so which view should be visible or hidden.

- will you manage second or not?
- do you want to design an analog watch or a digital watch (or both...)

Now you can start to modify json file with the `"visibility":` key of each view set to `"visible"` or `"gone"` (if you want to keep or not the view)

You can also start to tune approximativaly top, left margin and width heigth values to start organizing the watchface (these values will be tuned later using image editor)

Note: everything is design within a **400px x 400px rectangle**. So everything will be position in absolute coordinates within this size.

When you design your first watchface, you have to know that everything is organized by layer from the Back to the Top, so each view (ImageView or TextView) can hide something that is behind...



![CustomWatchface layers](../images/CustomWatchface_1.jpg)



Then within json file all views are sorted from the Back to the Top (this will help you to remember what is behind what...)







## Advanced features

### Preferences Feature

CustomWatchface can automatically tune some watch preferences to have the correct visualization of the watchface (if authorization is given within Wear preferencesby the user).

But this feature should be used with care. Preferences are common with all other watchfaces. So several rules to respect with this feature:

- never set preferences concerning hidden views
- try to maximize the visible views
- feel free to oversize the width of certain views:
  - TBR can be shown as percentage (small width, but also as absolue values much wider)
  - delta or avg delta with detailed information can be wide
  - same for iob2: this view can have total iob, but if detailed iob is selected, then text size can be very long

If you still need some very specific settings to have a correct display (in example below, if there is not enough space for detailed iob, you can "force" this parameter to `false` of your watch, you can include within metadata block some settings constraint like that

```json
    "metadata": {
        "name": "Default Watchface",
        "author": "myName",
        "created_at": "07\/10\/2023",
        "author_version": "1.0",
        "cwf_version": "1.0",
        "comment": "Default watchface, you can click on EXPORT WATCHFACE button to generate a template",
        "key_show_detailed_iob": false
    },
```

If user authorize custom watchface to modify watch parameter (setting within wear plugin) then Show detailed iob will be set to "disable", and locked to disable (no modification of this parameter possible, until authorization is disabled within wear plugin parameter, or another watchface is selected)

- Note that when a user select a watchface, he can see the number of "required parameter" during watchface selection

In example below Gota watchface has one required parameter. If authorization is not given it will be shown in white color, but authorization is given, then this parameter will be set and locked on the watch (in this case the number is in orange color)

![Required parameters](../images/CustomWatchface_2.jpg)



### TwinView Feature

Twin views provide an easy way to adjust the view position based on the visible views. This does not have the power of a layout entirely made up of LinearLayout, but can handle many common cases.

In example below you can see AAPS (Cockpit) watchface with all views visible within settings, and the same watchface with "Show rig battery" disabled and "Show avg delta" disabled

![Twin Views](../images/CustomWatchface_3.jpg)

You can see that when one of the twin views is hidden, the other is shifted to be centered

in this example, you can see that within `"uploader_battery"` block, we have `"twinView":` key is added to define `"rig_battery"` view, and in `"rig_battery"` block  `"twinView":` key define `"uploader_battery"` as twin. Then then additional key `"leftOffsetTwinHidden":` define the number of pixel to shift the view when twin is hidden.

To calculate this number, you can see that the difference between the leftMargin of each of the twin views is 50 pixels, so the offset to stay centered is half in one direction or the other.

If the twin views are positioned vertically, in this case you must use the key `"topOffsetTwinHidden":`

```json
"uploader_battery": {
    "width": 49,
    "height": 30,
    "topmargin": 354,
    "leftmargin": 150,
    "rotation": 0,
    "visibility": "visible",
    "textsize": 23,
    "gravity": "center",
    "font": "roboto_condensed_bold",
    "fontStyle": "bold",
    "fontColor": "#FFFFFF",
    "twinView": "rig_battery",
    "leftOffsetTwinHidden": 25
},
"rig_battery": {
    "width": 49,
    "height": 30,
    "topmargin": 354,
    "leftmargin": 200,
    "rotation": 0,
    "visibility": "visible",
    "textsize": 23,
    "gravity": "center",
    "font": "roboto_condensed_bold",
    "fontStyle": "bold",
    "fontColor": "#FFFFFF",
    "twinView": "uploader_battery",
    "leftOffsetTwinHidden": -25
},
```
### DynData Feature

DynData is the most powerfull feature if you want to include some animation within you watchface, according to some internal values (like BG value, BG level, delta, % of battery... see list of available data here §§§)

To illustrate this feature, I will take the example of AAPS (SteamPunk) watchface:

![CustomWatchface_4](../images/CustomWatchface_4.png)

In this watchface, we will have to manage the rotation of BG value (from 30 degrees to 330 degrees) on the right, the dynamic range of avg_delta (scale up to 5mgdl, 10mgdl or 20mgdl according to value), the rotation of pointer that should be synchronized to the scale, and also the different layer of the views...

To be able to manage this Watchface, see below all the images included into the zip file:

Note: to be able to see the transparency, all these images are on a yellow background and surrounded by a red square

![Steampunk images](../images/CustomWatchface_5.jpg)

- On the first row, Background.jpg and CoverPlate.png will be automatically mapped with associated view (default views filename), and steampunk_pointer.png will be managed by dynData
- On the second row you see the 3 scales of dynamic range for avg_delta that will also be managed by dynData
- On the third row, chartBackground.jpg will be linked manually within chart view, HourHand.png and finally MinuteHand.png files will be automatically mapped with associated views

#### **Background management**

First, concerning BG value image, no choice here, it can only be in the background layer (otherwize it will be in front of the chart view and chart will not be visible!). So we will have to map BG value to the background, and then rotate background image according to BG value.

Within `"background"` block, we will include 2 dedicated keys to make this rotation:

```json
"background": {
    "width": 400,
    "height": 400,
    "topmargin": 0,
    "leftmargin": 0,
    "dynData": "rotateSgv",
    "rotationOffset": true,
    "visibility": "visible"
},
```
`"dynData":` key will define which block should be used to define the animation (value, range, convertion...) here this block was named "rotateSgv" (choose an explicit name when you use this feature),

`"rotationOffset": true,` will define that the expected animation according to value should be a rotation. (others available keys are `"leftOffset"` and `"topOffset"` if you want to create a slider)

Now we will go at the end of the file, after the last view:

```json
"second_hand": {
    "width": 120,
    "height": 120,
    "topmargin": 140,
    "leftmargin": 140,
    "visibility": "gone"
},
"dynData": {
    "rotateSgv": {
        "valueKey": "sgv",
        "minData": 30,
        "maxData": 330
    },
```
You can see that after the latest view (`"second_hand"`), we added a new `"dynData": { ... }` block that  will containts all the animations:

The block defined within `"background"`view was named `"rotateSgv"`, it's the first block you will find into `"dynData"`!

This block is simple: you have a first key named `"valueKey":` that will be used to define which value should be used. in this case `"sgv"` is a "keyValue" that defines BG value (note that in most cases the keyValue has the same name that the view that shows this information).

Concerning BG value, default min data is set to 39mgdl and max  data is set to 400mgdl (see below §§§ all available keyValues and associated min/max data values).

Within `"rotateSgv"` block the two additional keys (`"minData":` and `"maxData":`) will be used to tune min and max data to 30 and 330. With these min and max values, we will be able to directly use data value (without any convertion) to rotate background in degrees. In this situation all BG values above 330mgdl will be limited to 330, upper limit of the image.

#### **Chart management**

Default background of chart is transparent, so to hide BG scale included into background image, we will need to include a dedicated background image (this image will include the overall shadows of Steampunk watchface). The link to charBackground.jpg file is done with `"background":` key

Of course, the sizing and positioning of the view must be done to the pixel!

```json
"chart": {
    "width": 216,
    "height": 107,
    "topmargin": 280,
    "leftmargin": 80,
    "visibility": "visible",
    "background": "chartBackground"
},
```
#### **Avg delta management**

To be able to manage dynamic range of avg delta, we will use the four freetext views. freetext1 will be used to manage the image scale, and freetext2 to freetext4 will be used to manage pointer rotation according to scale.

**freetext1**

As explain before, freetext views are in front of chart and in front of background, that's why we included transparent area to see these images (right side and bottom side of the image)

Note that the removed bottom part of these images has been used as background of chart to have a perfect integration.

```json
"freetext1": {
    "width": 400,
    "height": 400,
    "topmargin": 0,
    "leftmargin": 0,
    "rotation": 0,
    "visibility": "visible",
    "dynData": "avgDeltaBackground"
},
```
For this view we include the link to another `"dynData"`block named `avgDeltaBackground`. This block will manage avgDelta scale according to avgDelta value.

```json
    "avgDeltaBackground": {
        "valueKey": "avg_delta",
        "minData": -20,
        "maxData": 20,
        "invalidImage": "steampunk_gauge_mgdl_5",
        "image1": "steampunk_gauge_mgdl_20",
        "image2": "steampunk_gauge_mgdl_20",
        "image3": "steampunk_gauge_mgdl_10",
        "image4": "steampunk_gauge_mgdl_5",
        "image5": "steampunk_gauge_mgdl_5",
        "image6": "steampunk_gauge_mgdl_10",
        "image7": "steampunk_gauge_mgdl_20",
        "image8": "steampunk_gauge_mgdl_20"
    },
```
- `"valueKey":` will make the link with `"avg_delta"` value
- min and max Data will also limit the range to the maximum value available within this watchface (from -20mgdl to 20mgdl). For mmol users, keep in mind that all internal values are always in mgdl within AAPS.

Then we will see here how to manage dynamic background image according to value.

`"invalidImage":` is the key to manage image to show when we have an invalid data (or missing data). Here we make the link to additional resource image including into zip file with 5 mgdl scale

Then we will use a serie of images, starting from `"image1":` to `"image8":`. The number of provided images will define the number of steps between `minData` and `maxData`.

- `image1` will define image to show when avg_delta is equal or close to `minData` and the image with the highest number (here `image8`) will be used to define the image that should be shown when avg_delta is equal or close to `maxData`
- between -20mgdl and 20mgdl, the overall range is 40mgdl, devided by 8 (number of images provided), we will have 8 steps of 5mgdl
- Now we can map background images according to avg_delta value, starting from the lowest values: between -20 and -15, and also between -15 and -10 we will use  `steampunk_gauge_mgdl_20` for the scale, then between -10 and -5 `steampunk_gauge_mgdl_10`, and so on until +15 and +20 where we will again use `steampunk_gauge_mgdl_20` background image

**freetext2 to freetext4**

For these views will will combine dynamic images and rotation feature explained before:

```json
"freetext2": {
    "width": 276,
    "height": 276,
    "topmargin": 64,
    "leftmargin": 64,
    "rotation": 0,
    "visibility": "visible",
    "dynData": "avgDelta5",
    "rotationOffset": true
},
"freetext3": {
    "width": 276,
    "height": 276,
    "topmargin": 64,
    "leftmargin": 64,
    "rotation": 0,
    "visibility": "visible",
    "dynData": "avgDelta10",
    "rotationOffset": true
},
"freetext4": {
    "width": 276,
    "height": 276,
    "topmargin": 64,
    "leftmargin": 64,
    "rotation": 0,
    "visibility": "visible",
    "dynData": "avgDelta20",
    "rotationOffset": true
},
```
Here each view is dedicated to a specific scale (so is linked to a dedicated dynData block), you can alos notice that `"rotationOffset":` key is enabled for these 3 views.Now take a look on the first dynData block:

```json
    "avgDelta5": {
        "valueKey": "avg_delta",
        "minData": -20,
        "maxData": 20,
        "rotationOffset": {
            "minValue": -120,
            "maxValue": 120
        },
        "invalidImage": "null",
        "image1": "null",
        "image2": "null",
        "image3": "null",
        "image4": "steampunk_pointer",
        "image5": "steampunk_pointer",
        "image6": "null",
        "image7": "null",
        "image8": "null"
    },
```
Here, even if dynamic range will be used only between -5 and +5 avg_delta datas, it's important to keep the overall range of -20, +20mgdl to ensure that the pointer will be synchronize with the background during scale switches. That's why we keep the same overall range than for `avgDeltaBackground`  and the same number ot steps (8 images).

You can note that either `"invalidImage"` or several `"imagexx"` are with `"null"` key value (it could be any string not existing as a filename within zip file). When a filename is not found, then view background image will be transparent. So the setting ensure that pointer will only be visible for step 4 and step 5 (avg delta between -5mgdl and +5 mgdl), and will not be visible outside this range.

Now we can see a new block `"rotationOffset":` that will have inside two keys `"minValue":` and `"maxValue":`. These values are used to make the convertion between internal datas (in mgdl), and the angle rotation we want to have.

- Steampunk watchface is designed to have maximum from -30 degrees to 30 degrees rotation for the pointer. So according to the scale (here from -5mgdl to 5mgdl), we will want to have 30 degrees for these values. Because `minData` and `maxData`are 4 times greater, then the corresponding minValues and maxValues are 4 * 30 degrees so -120 and +120 degrees. But for all rotation above or below +-30 degrees the pointer will be hidden (no image visible), and the pointer will only be visible for values between -5 and +5mgdl... So it's exactly what is expected here.

The other dynData blocks are defined the same way to tune `"avgDelt10"`and `"avgDelta20"`

#### loop view

in Steampunk watchface loop green and red arrows (for status) are disabled, this is also managed with a dedicated dynData block associated to loop view.

```json
    "loopArrows": {
        "invalidImage": "greyArrows",
        "image1": "greenArrows",
        "image2": "redArrows"
    }
```
Because this block is only called by loop View, and default data managed by this view is loop information, then `"valueKey":` key is optional.

Default `minData` and `maxData` for loop are defined to 0min and 28min, so with two images, all data values below 14 min will be shown with background `image1` and all data values above 14 min will be shown with `image2`. 14 min is exactly the threshold to switch from green arrow to red arrow.

In this example, `greyArrows`, `greenArrows` and `redArrows` files are not included into zip file, so these arrows are just removed (invisible), but you can use this block "as is" if you want to tune status arrows with custom background images.

#### rig_battery and uploader_battery views

To finish the overview of dynData feature, we will take a look on battery management. The idea here is to customize text color according to battery level (from 0 to 100%)

```json
"uploader_battery": {
    "width": 60,
    "height": 28,
    "topmargin": 100,
    "leftmargin": 170,
    "rotation": 0,
    "visibility": "visible",
    "textsize": 20,
    "gravity": "center",
    "font": "default",
    "fontStyle": "bold",
    "fontColor": "#00000000",
    "dynData": "batteryIcons",
    "twinView": "rig_battery",
    "topOffsetTwinHidden": -13
},
"rig_battery": {
    "width": 60,
    "height": 28,
    "topmargin": 74,
    "leftmargin": 170,
    "rotation": 0,
    "visibility": "visible",
    "textsize": 20,
    "gravity": "center",
    "font": "default",
    "fontStyle": "bold",
    "fontColor": "#00000000",
    "dynData": "batteryIcons",
    "twinView": "uploader_battery",
    "topOffsetTwinHidden": 13
},
```
You can see here that these both views share the same `dynData` block named `batteryIcons`. It's possible because by default attached data is the one of the view (to without specifying a  `"valueKey":` key within  `batteryIcons` block, it will be applied with `uploader_battery` data or `rig_battery` data according to the view).

Note these two views also use TwinView feature explain here §§§.

Now lets take a look on dynData block:

```json
    "batteryIcons": {
        "invalidFontColor": "#00000000",
        "fontColor1": "#A00000",
        "fontColor2": "#000000",
        "fontColor3": "#000000",
        "fontColor4": "#000000",
        "fontColor5": "#000000"     
    },
```
Here we use exactly the same logic that for dynamic background image, but with dedicated keys (`"invalidFontColor"` and  `"fontColor1"` to `"fontColor5"` to specify 5 steps of 20% each one).

- `"fontColor1"` (dark red) will be used for all values below 20%, and white will be used for all values above this threshold.
- If you want to lower the threshold to "below 10%", you just have to add 5 additional keys from `"fontColor6"` to `"fontColor10"` , but you can also adjust each color if you want progressive variation from green to yellow, orange and red...



## Key and Key value reference

### List of Metadata keys

#### List of Standard information metadata keys

| Key            | Comment                                                                                                         |
| -------------- | --------------------------------------------------------------------------------------------------------------- |
| name           | Name of custom watchface                                                                                        |
| author         | Name or pseudo of the author(s)                                                                                 |
| created_at     | Creation (or update) date, be carefull `/` is a special character, so if you use it for the date put `\`before |
| cwf_version    | Watchface plugin compatible with the design of your watchface                                                   |
| author_version | The author can specify here the version of his watchface                                                        |
| comment        | Free text that can be used to give some information or limitation of current watchface                          |

#### Preference keys

| Key                         | Comment and                                                                                                                                                                                                                                                  |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| key_show_detailed_iob     | true will lock detailed IOB data on view `iob2`, then `iob1` (if visible and not replaced by an icon) will show iob total.<br />false will lock total iob on `iob2`view. can be used if the width of `iob2`is too small to show correctly detailed iob |
| key_show_detailed_delta   | false (only if design is not compatible with the width of detailed delta for `delta`and `avg_delta` views)                                                                                                                                                   |
| key_show_bgi              | true if your design requires `bgi` information                                                                                                                                                                                                               |
| key_show_iob              | true if your design requires `iob1` or `iob2`views                                                                                                                                                                                                           |
| key_show_cob              | true if your design requires `cob1` or `cob2`views                                                                                                                                                                                                           |
| key_show_delta            | true if your design requires `delta` information                                                                                                                                                                                                             |
| key_show_avg_delta        | true if your design requires `avg_delta` information                                                                                                                                                                                                         |
| key_show_uploader_battery | true if your design requires `uploader_battery` (phone battery) information                                                                                                                                                                                  |
| key_show_rig_battery      | true if your design requires `rig_battery` information                                                                                                                                                                                                       |
| key_show_temp_basal       | true if your design requires `basalRate` information                                                                                                                                                                                                         |
| key_show_direction        | true if your design requires `direction` information (BG variation arrows)                                                                                                                                                                                   |
| key_show_ago              | true if your design requires `timestamp` information (minutes ago for last received BG)                                                                                                                                                                      |
| key_show_bg               | true if your design requires `sgv` information (BG value)                                                                                                                                                                                                    |
| key_show_loop_status      | true if your design requires `loop` information (loop status and ago)                                                                                                                                                                                        |
| key_show_week_number      | true if your design requires `week_number` information (loop status and ago)                                                                                                                                                                                 |

#### Internal keys

| Key               | Comment and                                                                                                                                                                                   |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filename          | This key will be created (or updated) automatically when the watchface is loaded and will contains local zip filename within exports folder                                                   |
| cwf_authorization | this key will be created (when the watchface is loaded) and updated each time authorization preference is changed in Wear settings, and it will be used to synchronize authorization to watch |

### List of General parameters

| Key                  | Comment                                                                                                                                                                                                                                                   |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| highColor            | `"#FFFF00"`(default Yellow): Color of BG value, trend arrows and bg value in graph if bg is above upper limit (Hyper)                                                                                                                                     |
| midColor             | `"#00FF00"`(default Green): Color of BG value, trend arrows and bg value in graph if bg is within range                                                                                                                                                   |
| lowColor             | `"#FF0000"`(default Red): Color of BG value, trend arrows and bg value in graph if bg is below lower limit (Hypo)                                                                                                                                         |
| lowBatColor          | `"#E53935"`(default Dark Red): Color of `uploader_battery` when value is low (below 20% tbc)                                                                                                                                                              |
| carbColor            | `"#FB8C00"`(default Orange): Color of Carbs points within graph                                                                                                                                                                                           |
| basalBackgroundColor | `"#0000FF"`(default Dark blue): Color of TBR curve within graph                                                                                                                                                                                           |
| basalCenterColor     | `"#64B5F6"`(default Light blue): Color of Bolus or SMB points within graph                                                                                                                                                                                |
| gridColor            | `"#FFFFFF"`(default White): Color of lines and text scale within graph                                                                                                                                                                                    |
| pointSize            | 2 (default): size of points in graph (1 for small point, 2 for big points)                                                                                                                                                                                |
| enableSecond         | false (default): specify if watchface will manage seconds or not within `time`, `second` or `second_hand` views. it's important to be consistent between view visibility and this overall setting that will allow update every second of time information |
| dayNameFormat        | "E" (default): from "E" to "EEEE" specify dayname format (number, short name, full name) tbc                                                                                                                                                              |
| monthFormat          | "MMM" (default): from "M" to "MMMM" specify month format (number, short name, full name)                                                                                                                                                                  |



### List of HardCoded resource files

For most images, High and Low suffix allow tuning of image according to BG level (in Range, Hyper or Hypo)

| Filenames                                                       | Comment                                                                                                                                                                  |
| --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| CustomWatchface                                                 | Image shown for watchface selection and within Wear plugin                                                                                                               |
| Background,<br />BackgroundHigh,<br />BackgroundLow | none (default black): Background image. background is allways visible and default color is black if no image provided. Color can be modified to fit watchface design     |
| CoverChart,<br />CoverChartHigh,<br />CoverChartLow | none (default): Image in front of Chart (transparency should be available to see Chart behind) Can be used to limit boundaries of graph                                  |
| CoverPlate,<br />CoverPlateHigh,<br />CoverPlateLow | simple dial (default): image in front of all text values. transparency mandatory to see all values that are behind                                                       |
| HourHand,<br />HourHandHigh,<br />HourHandLow       | hour_hand (default): image of hour hand. a default image is provided and can be colored to fit analog design. Note axis for rotation will be the center of the image     |
| MinuteHand,<br />MinuteHandHigh,<br />MinuteHandLow | minute_hand (default): image of minute hand. a default image is provided and can be colored to fit analog design. Note axis for rotation will be the center of the image |
| SecondHand,<br />SecondHandHigh,<br />SecondHandLow | second_hand (default): image of second hand. a default image is provided and can be colored to fit analog design. Note axis for rotation will be the center of the image |
| ArrowNone                                                       | ?? (default): image shown when no valid arrow is available.                                                                                                              |
| ArrowDoubleUp                                                   | ↑↑ (default): image of double arrow up                                                                                                                                   |
| ArrowSingleUp                                                   | ↑ (default): image of single arrow up                                                                                                                                    |
| Arrow45Up                                                       | ↗ (default): image of fortyfive arrow up                                                                                                                                 |
| ArrowFlat                                                       | → (default): image of flat arrow                                                                                                                                         |
| Arrow45Down                                                     | ↘ (default): image of fortyfive arrow down                                                                                                                               |
| ArrowSingleDown                                                 | ↓ (default): image of single arrow down                                                                                                                                  |
| ArrowDoubleDown                                                 | ↓↓ (default): image of double arrow down                                                                                                                                 |

For each above filenames, extension can be either .jpg, .png or .svg. But be carefull, .jpg doesn't manage transparency (so most of the files should be with .png or .svg to not hide view that are behind...)

### List of View keys

This list is sorted from background to foreground this is very important when you organize your watchface to know this order because some image or text can be hidden by other images

| Key              | Type of view        | Data attached                                                                                          | DynData Key             |
| ---------------- | ------------------- | ------------------------------------------------------------------------------------------------------ | ----------------------- |
| background       | Image View          |                                                                                                        |                         |
| chart            | Specific Chart View | Graphical curves                                                                                       |                         |
| cover_chart      | Image View          |                                                                                                        |                         |
| freetext1        | Text View           |                                                                                                        |                         |
| freetext2        | Text View           |                                                                                                        |                         |
| freetext3        | Text View           |                                                                                                        |                         |
| freetext4        | Text View           |                                                                                                        |                         |
| iob1             | Text View           | IOB label or IOB Total                                                                                 |                         |
| iob2             | Text View           | IOB Total or IOB Detailed                                                                              |                         |
| cob1             | Text View           | Carb label                                                                                             |                         |
| cob2             | Text View           | COB Value                                                                                              |                         |
| delta            | Text View           | Short delta (5 min)                                                                                    | delta                   |
| avg_delta        | Text View           | Avg Delta (15 min)                                                                                     | avg_delta               |
| uploader_battery | Text View           | phone battery level (%)                                                                                | uploader_battery        |
| rig_battery      | Text View           | rig battery level (%)                                                                                  | rig_battery             |
| basalRate        | Text View           | % or absolute value                                                                                    |                         |
| bgi              | Text View           | mgdl/(5 min) or mmol/(5 min)                                                                           |                         |
| time             | Text View           | HH:MM or HH:MM:SS                                                                                      |                         |
| hour             | Text View           | HH                                                                                                     |                         |
| minute           | Text View           | MM                                                                                                     |                         |
| δεύτερο          | Text View           | SS                                                                                                     |                         |
| timePeriod       | Text View           | AM or PM                                                                                               |                         |
| day_name         | Text View           | name of the day (cf. dayNameFormat)                                                                    | day_name                |
| day              | Text View           | DD date                                                                                                | day                     |
| week_number      | Text View           | (WW) week number                                                                                       | week_number             |
| month            | Text View           | month name (cf. monthFormat)                                                                           |                         |
| loop             | Text View           | min ago since last run and status (color arrows in background), color arrows can be tuned with DynData | loop                    |
| direction        | Image View          | TrendArrows                                                                                            | direction               |
| timestamp        | Text View           | integer (min ago)                                                                                      | timestamp               |
| sgv              | Text View           | sgv value (mgdl or mmol)                                                                               | sgv<br />sgvLevel |
| cover_plate      | Image View          |                                                                                                        |                         |
| hour_hand        | Image View          |                                                                                                        |                         |
| minute_hand      | Image View          |                                                                                                        |                         |
| second_hand      | Image View          |                                                                                                        |                         |



### List of Json keys

#### Common keys

 that can be used on all view types (Text View, image View, graph view)

| Key                  | type    | comment / value                                                                                                                                                                        |
| -------------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| width                | int     | width of view in pixel                                                                                                                                                                 |
| height               | int     | heigth of view in pixel                                                                                                                                                                |
| topmargin            | int     | top margin in pixel                                                                                                                                                                    |
| leftmargin           | int     | left margin in pixel                                                                                                                                                                   |
| rotation             | int     | rotation angle in degrees                                                                                                                                                              |
| visibility           | string  | see key value table                                                                                                                                                                    |
| dynData              | string  | key block name that will specify dynamic data to link and associated animation (colors, image, shift, rotation)<br />`"dynData": "customName",` (see below )                     |
| leftOffset           | boolean | include this key with key value true to enable horizontal shift (positive or negative value) due to dynData value                                                                      |
| topOffset            | boolean | include this key with key value true to enable vertical shift (positive or negative value) due to dynData value                                                                        |
| rotationOffset       | boolean | include this key with key value true to enable rotation (positive or negative value) due to dynData value                                                                              |
| twinView             | string  | key of the other view (generally the other view also include the twinView parameter with the key of this view in it)                                                                   |
| topOffsetTwinHidden  | int     | number of pixel to shift view position vertically if twin view is hidden (positive or negative value)<br />topOffsetTwinHidden = (topOffset twinView - topOffset thisView)/2     |
| leftOffsetTwinHidden | int     | number of pixel to shift view position horizontally if twin view is hidden (positive or negative value)<br />leftOffsetTwinHidden= (leftOffset twinView - leftOffset thisView)/2 |

#### TextView keys

| Key        | type    | comment                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| textsize   | int     | size of font in pixel (keep in mind that font can include top and bottom margin so the real text size will generally be smaller than the number of pixel set). Note that size should be smaller than view heigth to not be truncated                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| gravity    | string  | see key value table                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| font       | string  | see key value table for available fonts.<br />Can also be font filename (without extension) for fonts included into zip file                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| fontStyle  | string  | see key value table                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| fontColor  | string  | Manage color of the font<br />`"#RRVVBB"`: color code in RVB format, hexdecimal values #FF0000 is red<br />`"#AARRVVBB"`: AA include Alpha information (transparency), 00 is transparent, FF is opaque<br />`"bgColor"`: keyValue bgColor is an easy way to use highColor, midColor or lowColor according to BG value                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| allCaps    | boolean | true if you want text in uppercase (mainly day name or month name)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| background | string  | `resource_filename` you can include a resource image as background of the text view (resource file will be resized to fit heigth and width of text view, but keeping image ratio). text value will be in front of background image.<br />- Note that this key can also be used for `chart` view to set a custom background to the chart, infront of background image                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| color      | string  | Manage the color of view Background or tune color of image (if bitmap only)<br />`"#RRVVBB"`: color code in RVB format, hexdecimal values #FF0000 is red<br />`"#AARRVVBB"`: AA include Alpha information (transparency), 00 is transparent, FF is opaque<br />`"bgColor"`: keyValue bgColor is an easy way to use highColor, midColor or lowColor according to BG value<br />- For default embeded image (hand, dial) color will be applied directly, for bitmap image (jpg or png) this will apply a saturation gradient filter on imagae<br />- For svg this parameter will have no effect (color of svg files cannot be modified)<br />- Note that this key can also be used for `chart` view to set a custom background to the chart, infront of background image |
| textvalue  | string  | Key specific to the 4 free text views included into the layout (from freetext1 to freetext4), this allow you to set the text that should be included (can be a label, or just `:` if you want to add a separator between hour view and minute view...)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

#### ImageView key

| Key   | type   | comment                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ----- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| color | string | Manage the color of view Background or tune color of image (if bitmap only)<br />`"#RRVVBB"`: color code in RVB format, hexdecimal values #FF0000 is red<br />`"#AARRVVBB"`: AA include Alpha information (transparency), 00 is transparent, FF is opaque<br />`"bgColor"`: keyValue bgColor is an easy way to use highColor, midColor or lowColor according to BG value<br />- For default embeded image (hand, dial) color will be applied directly, for bitmap image (jpg or png) this will apply a saturation gradient filter on imagae<br />- For svg this parameter will have no effect (color of svg files cannot be modified)<br />- Note that this key can also be used for `chart` view to set a custom background to the chart, infront of background image |

#### chartView keys

| Key        | type   | comment                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| color      | string | Manage the color of view Background or tune color of image (if bitmap only)<br />`"#RRVVBB"`: color code in RVB format, hexdecimal values #FF0000 is red<br />`"#AARRVVBB"`: AA include Alpha information (transparency), 00 is transparent, FF is opaque<br />`"bgColor"`: keyValue bgColor is an easy way to use highColor, midColor or lowColor according to BG value<br />- For default embeded image (hand, dial) color will be applied directly, for bitmap image (jpg or png) this will apply a saturation gradient filter on imagae<br />- For svg this parameter will have no effect (color of svg files cannot be modified)<br />- Note that this key can also be used for `chart` view to set a custom background to the chart, infront of background image |
| background | string | `resource_filename` you can include a resource image as background of the text view (resource file will be resized to fit heigth and width of text view, but keeping image ratio). text value will be in front of background image.<br />- Note that this key can also be used for `chart` view to set a custom background to the chart, infront of background image                                                                                                                                                                                                                                                                                                                                                                                                                                 |

### key values

| Key value                  | key        | comment                                                                           |
| -------------------------- | ---------- | --------------------------------------------------------------------------------- |
| gone                       | visibility | view hidden                                                                       |
| visible                    | visibility | view visible in watchface (but visibility can be enable or disable in parameters) |
| center                     | gravity    | text is vertical and horizontal centered into the view                            |
| left                       | gravity    | text is vertical centered but left aligned into the view                          |
| right                      | gravity    | text is vertical centered but right aligned into the view                         |
| sans_serif                 | font       |                                                                                   |
| default                    | font       |                                                                                   |
| default_bold               | font       |                                                                                   |
| monospace                  | font       |                                                                                   |
| serif                      | font       |                                                                                   |
| roboto_condensed_bold    | font       |                                                                                   |
| roboto_condensed_light   | font       |                                                                                   |
| roboto_condensed_regular | font       |                                                                                   |
| roboto_slab_light        | font       |                                                                                   |
| normal                     | fontStyle  |                                                                                   |
| bold                       | fontStyle  |                                                                                   |
| bold_italic                | fontStyle  |                                                                                   |
| italic                     | fontStyle  |                                                                                   |

### DynData keys

| Key                 | type   | comment                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| dynData             | block  | define the block of all dynamic data blocs that will be used for the views. generally aafter the last view.<br />All the keys defined within this block will be used as key Value within view block:<br />`"dynData": { dynData blocks }`<br />and each block is defined by a custom name and several keys inside:<br />`"customName": { one dynData block }`                                                                                    |
| valueKey            | string | name of dynamic data to use (generally same that associated view key).<br />If not existing, the default will be the values used for the view that uses this block. <br />for example you can define one block to customize battery level percentage without specifying valueKey, and then use the same block to customize uploader_battery and rig_battery                                                                                                |
| minData             | int    | specify the minimum value to take into account for AAPS data : for example if value is sgv (unit mgdl internaly), if minData is set to 50, all bg values below 50mgdl will be set to 50.<br />- Note that minData and maxData will be used to calculate dynamic values (in pixel or in degrees).                                                                                                                                                                   |
| maxData             | int    | specify the maximum value to take into account for AAPS data : for example if value is sgv (unit mgdl internaly), if maxData is set to 330, all bg values above 330mgdl will be set to 330.                                                                                                                                                                                                                                                                              |
| leftOffset          | block  | Specify the horizontal shift of the view according to min and max values in pixels.<br />- It includes minValue key, maxValueKey and invalidValue key (optional)<br />- If data is below or equal minData, then the view will be shifted to minValue pixels, and if data is above or equal to maxData, then the view will be shifted to maxValue pixels<br />Note that to apply this shift leftOffset should be set to true within the view            |
| topOffset           | block  | Specify the vertical shift of the view according to min and max values in pixels.<br />- It includes minValue key, maxValueKey and invalidValue key (optional)<br />- If data is below or equal minData, then the view will be shifted to minValue pixels, and if data is above or equal to maxData, then the view will be shifted to maxValue pixels<br />Note that to apply this shift topOffset should be set to true within the view               |
| rotationOffset      | block  | Specify the rotation angle in degrees of the view according to min and max values in pixels.<br />- It includes minValue key, maxValueKey and invalidValue key (optional)<br />- If data is below or equal minData, then the view will rotate by minValue degrees, and if data is above or equal to maxData, then the view will rotate by maxValue degrees<br />Note that to apply this rotation, rotationOffset should be set to true within the view |
| minValue            | int    | result value to apply to the view (key only applicable within a leftOffset, topOffset or rotationOffset block)                                                                                                                                                                                                                                                                                                                                                           |
| maxValue            | int    | result value to apply to the view (key only applicable within a leftOffset, topOffset or rotationOffset block)                                                                                                                                                                                                                                                                                                                                                           |
| invalidValue        | int    | result value to apply to the view if data is invalid (key only applicable within a leftOffset, topOffset or rotationOffset block)                                                                                                                                                                                                                                                                                                                                        |
| invalidImage        | string | `resource_filename` to use for the ImageView or background TextView if the data is invalid                                                                                                                                                                                                                                                                                                                                                                               |
| image*1_to_n*     | string | `resource_filename` image to use for each step between minData (or close to minData) with image1 and maxData (or close to maxData) with image*n*<br />If for example your put 5 images (from image1 to image5), the range between minData and maxData will be divided in 5 steps and according to data value, the corresponding image will be shown                                                                                                                |
| invalidFontColor    | string | Manage invalid fontColor<br />`"#RRVVBB"` or `"#AARRVVBB"`: Color to use if an invalid data is received (can be transparent if AA=00)                                                                                                                                                                                                                                                                                                                              |
| fontColor*1_to_n* | string | Manage fontColor<br />`"#RRVVBB"` or `"#AARRVVBB"`: color to use for each step between minData (or close to minData) with fontColor1 and maxData (or close to maxData) with fontColor*n*                                                                                                                                                                                                                                                                           |
| invalidColor        | string | Manage invalid background color or image color<br />`"#RRVVBB"` or `"#AARRVVBB"`: Color to use if an invalid data is received (can be transparent if AA=00)                                                                                                                                                                                                                                                                                                        |
| color*1_to_n*     | string | Manage background color or image Color<br />`"#RRVVBB"` or `"#AARRVVBB"`: color to use for each step between minData (or close to minData) with color1 and maxData (or close to maxData) with color*n*                                                                                                                                                                                                                                                             |

### DynData key values

| Key value        | key      | comment                                                                                                                                                                                                                                                           |
| ---------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sgv              | valueKey | default minData = 39 mgdl<br />default maxData = 400 mgdl<br />- Note that real maxData is linked to your sensor and units are always in mgdl for internal values                                                                                     |
| sgvLevel         | valueKey | default minData = -1 (Hypo)<br />default maxData = 1 (Hyper)<br />if BG is within Range = 0                                                                                                                                                           |
| direction        | valueKey | default minData = 1 (double Down)<br />default maxValue = 7 (double Up)<br />flat arrow data = 4<br />Error or missing data = 0 (??)                                                                                                            |
| delta            | valueKey | default minData = -25 mgdl<br />default maxData = 25 mgdl<br />- Note that real min and maxData can be above, and units are always mgdl for internal values                                                                                           |
| avg_delta        | valueKey | default minData = -25 mgdl<br />default maxData = 25 mgdl<br />- Note that real min and maxData can be above, and units are always mgdl for internal values                                                                                           |
| uploader_battery | valueKey | default minData = 0 %<br />default maxData = 100%                                                                                                                                                                                                           |
| rig_battery      | valueKey | default minData = 0 %<br />default maxData = 100%                                                                                                                                                                                                           |
| timestamp        | valueKey | default minData = 0 min<br />default maxData = 60 min                                                                                                                                                                                                       |
| loop             | valueKey | default minData = 0 min<br />default maxData = 28 min<br />- Note that status arrows are in green below 14 min and in red above 14 min so if you put 2 images, you can replace status background with your custom images with default min and maxData |
| day              | valueKey | default minData = 1<br />default maxData = 31                                                                                                                                                                                                               |
| day_name         | valueKey | default minData = 1<br />default maxData = 7                                                                                                                                                                                                                |
| month            | valueKey | default minData = 1<br />default maxData = 12                                                                                                                                                                                                               |
| week_number      | valueKey | default minData = 1<br />default maxData = 53                                                                                                                                                                                                               |

