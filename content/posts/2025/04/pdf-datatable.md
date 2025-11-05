---
title: "I've built a tool to convert PDF files to CSV - PDF DataTable"
summary: "The development process behind PDF DataTable"
date: 2025-04-28
tags:
  - SoftwareEngineering
---
I launched [PDF DataTable](https://pdf-datatable.hanwg.top) on 21 April 2025.
It was my latest personal project and I thought it would be interesting to share the development process that went into building the app.

## Why Did I Build PDF DataTable?

I have an aversion regarding the use of free online tools.

The adage, "if you aren't paying for the product, you are the product" perfectly describes my concern regarding data privacy that my data will be harvested and sold to marketing companies (or worse, scammers).

Besides the privacy concern, I wanted to refresh my frontend development skills and also experiment with a different approach in handling PDF conversions.

## PDF Conversions - The Typical Way

The typical PDF conversion works like this:
1) The user uploads the PDF file to the server
2) The server performs the conversion in the backend
3) The user downloads the output file when the conversion is completed
4) The user (or the server) filters out unwanted rows in the CSV (e.g. by using regular expressions to match desired rows)

While regular expressions work well for most cases, there are situations where records can't be parsed or get filtered out unintentionally.
A thought came to my mind - what if the user can interact directly with the PDF file and have a real-time preview of their CSV output?

## PDF Conversions - The New Approach

With real-time previews as a key consideration, the flow of the new app is as follows:
1) The user selects the PDF file
2) The server renders the file on the screen
3) The user specifies the relevant content
4) The server updates the render to indicate which content has become irrelevant
5) The user confirms the selection
6) The server converts the selected content to CSV
7) (Optional) The server saves step 3 as a template.

## Designing the UI

The UI needs to allow the user specify the relevant content on the PDF.
I thought about a few possible ideas but eventually settled on text selection.
(They aren't mutually exclusive, but it will be too much effort to implement all of them)

### Lasso Tool

This idea came naturally - the user would click on the lasso tool and draw an outline on the `canvas`.
I would then select the `left`, `right`, `top` and `bottom` points of the drawing to determine the bounding box.
Any PDF content within the bounding box will be converted to CSV.

This is fairly easy to implement but seems rather messy if the PDF file spans multiple pages. 

### Hough Line Transform

This approach uses `houghLines` from the [OpenCV](https://opencv.org/) (Open source Computer Vision) library to detect lines of the table.
The user will then select the top and bottom lines to indicate the CSV content.

This idea was abandoned since it requires the tables to have borders. 

### Regular Expressions (Regex)

The most basic of all - The user enters the regular expression in a textbox and I match the text content line-by-line against the regex.

Match - Include the text in CSV

No match - Update the PDF render (e.g. gray-out the elements) and exclude from CSV

This works so it might be a possible feature in the future.

### Text Selection

This approach is described as follows:
1) The user selects a piece of text from the PDF render.
2) Anything that comes before the selection is excluded.
3) Elements that appear on the same row as the selection forms the header or the first record of the CSV.
4) Compute the intervals of this row. The intervals are boundaries that determines the columns of the CSV. 
5) Iterate through all elements in the PDF. For each element, 
    - If it doesn't overlap with any of the intervals, exclude it 
    - Determine which interval the element overlaps with and assign it to the respective column

This approach is more complex to develop, but it is more user-friendly compared to regex.

## Other Considerations

My budget for this app is $0, so I decided to run the app entirely on [GitHub Pages](https://pages.github.com/).
There would be no servers for backend processing so all the logic will run on the user's device.

## Building the App

The app consists of the following components:
- File selection
- PDF render
- CSV download

### File Selection

File selection is pretty straight-forward.

I first create a hidden `<input type="file" />` for the file input.

Next step is to create a drag-and-drop `div` and attaching the `onDrop` and `onClick` event listeners to trigger the file input.
The tricky bit is to use `event.preventDefault()` for the `onDrop` event to prevent the browser from navigating away from the app and opening the file.

### PDF Render

I used [React-pdf](https://react-pdf.org/), a React component built using [PDF.js](https://mozilla.github.io/pdf.js/), a browser-based PDF library by Mozilla to handle PDFs.

A snippet of code for rendering the PDF in a `div`:
```
<div>
   <Document file={file}
       onPassword={promptPassword}>
       {Array.from(
           new Array(numPages),
           (el, index) => (
               <Page
                   key={`page_${index + 1}`}
                   pageNumber={index + 1}
                   onRenderTextLayerSuccess={onTextLayerRender}
               />
           )
       )}
   </Document>
</div>
```

A few important parameters used in the above snippet:
- `file`: This value is from the file input
- `onPassword`: Event handler to prompt user for the password
- `onRenderTextLayerSuccess`: Event handler to invoke after page rendering is completed

The rendering process is done by the library, and it involves creating and inserting `span` elements into the HTML DOM.
Each `span` element corresponds to a text element from the PDF and has the following properties:
- `innerHTML`: the text content
- `boundingClientRect`: a rectangle describing the position and size of the element using the `x`, `y`, `width` and `height` properties.

After an element is created, I added a `onClick` event handler to allow the user to select the element and execute the text selection logic. 

### CSV Download

For this part, I created an invisible anchor which can initiate file download:
```
<a ref={downloadLink} className="hidden" href="/">Download CSV</a>
```

Next, I define a custom function named `renderCsv()`.
This function collects all the text elements which are not excluded in the PDF into an array strings.
Each element in the array corresponds to a record in the CSV.

The final step is to serialize the array into a `Blob` for download and trigger the anchor which was created earlier.
```javascript
function downloadCsv() {
    const csv = renderCsv();
    const blob = new Blob([csv], { type: "text/csv" });

    downloadLink.current.download = csvFilename;
    downloadLink.current.href = URL.createObjectURL(blob);

    downloadLink.current.click();
}
```

## Conclusion

A PDF file is like a random bag of text/images elements. Sometimes, the elements overlap each other, sometimes they are rotated, scaled or even sheared.

My initial implementation did not account for such transformations and resulted in missing text elements and elements appearing in the wrong order in my CSV output.
The logic was way messier and complex than I would have desired but overall, I was happy that I managed to release an app that worked for most of my use cases. 
