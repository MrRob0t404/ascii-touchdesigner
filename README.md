# ascii-touchdesigner

## Draft of instructions

Kinect ASCII Art Generator paired with TouchDesigner. An interactive art project that uses an Xbox Kinect to scan objects and generate real-time ASCII art using TouchDesigner.

## Requirements

Hardware:

- Xbox Kinect (v2)
- Kinect Power Adapter (v2)
- USB 3.0 Port (v2)
- Windows PC

## Software

- TouchDesigner
- Kinect SDK:
  - Kinect v2 SDK 2.0

## Concept

Capture depth or color data from a Kinect sensor.
Convert the image into a low-resolution grid.
Map pixel brightness values to ASCII characters.
Render the ASCII output in real-time using TouchDesigner.

## Setup Instructions

1. Kinect Setup
   For Kinect v2:

   - Install Kinect SDK 2.0.
   - Connect the Kinect and ensure it's detected by Windows.
   - In TouchDesigner, use the Kinect Azure TOP (or Kinect v2 TOP) to access the video feed.

2. Image Preprocessing

   1. Resolution TOP
      - Downsample the feed to a small size (e.g., 80x60) for ASCII conversion.
   1. Luma TOP
      - Convert the feed to grayscale.
   1. Level TOP (optional)
      - Adjust brightness and contrast for better contrast in ASCII.

3. Extract Brightness Data

   1. TOP to CHOP
      - Convert grayscale image to channels.
   1. CHOP to DAT
      - Output pixel values into a table format.

4. ASCII Mapping Script
   Use a Text DAT with the following Python script:

   ```python
   ascii_chars = "@%#*+=-:. "  # From dark to light
   table = op('chopto1')       # Replace with your CHOP to DAT path
   ascii_output = ""

   for row in range(table.numRows):
       line = ""
       for col in range(table.numCols):
           val = float(table[row, col])
           index = int(val * (len(ascii_chars)-1))  # Normalize brightness
           line += ascii_chars[index]
       ascii_output += line + "\n"

   op('text1').text = ascii_output  # Output to a Text TOP or panel
   ```

5. Display the ASCII Art
   - Use a Text TOP:
     - Connect to the text1 DAT.
     - Use a monospaced font (e.g., Courier New).
     - Scale font to fit resolution.

## Possible Enhancements

1. Visual Styling

   - Composite with a Constant TOP for background.
   - Add effects like glow or displacement using Composite TOP.

2. Interactivity

   - Add a button to toggle ASCII mode on/off.Use Kinect's body tracking to control parameters.
   - Change ASCII resolution or character set dynamically.

3. Output Options
   - Record video via Movie File Out TOP.
   - Stream via Spout / NDI to projection or VJ software.

✅ Checklist

- Kinect feed is visible in TouchDesigner
- Image is properly downsampled
- ASCII text appears and updates in real-time
- Characters accurately represent the scanned object
- Performance is smooth at target resolution

📁 Project Structure

```plaintext
/kinect-ascii-art/
├── assets/
│   └── screenshots/
├── .toe          # Main TouchDesigner project
├── README.md     # Project instructions
└── scripts/
    └── ascii_mapper.py  # ASCII conversion logic
```

By Simon
Built with TouchDesigner and Kinect

```plaintext
ASCII mapping inspired by terminal art and creative coding communities
```
