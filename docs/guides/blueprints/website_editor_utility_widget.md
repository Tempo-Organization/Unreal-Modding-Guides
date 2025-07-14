

https://github.com/user-attachments/assets/b5d0e929-2fd9-48c0-99e1-e18755ef2f80

Keep in mind, what sites will work, depends on the cef version in your unreal engine install.\
For 4.27 and below, many newer sites using newer technologies may not load

The example site from the video:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Color to PNG Generator</title>
  <style>
    body {
      font-family: sans-serif;
      background-color: #1e1e1e;
      color: #fff;
      text-align: center;
      padding: 2em;
    }
    input, button {
      margin: 0.5em;
      padding: 0.5em;
      border-radius: 5px;
    }
    #preview {
      margin-top: 1em;
    }
    #preview img {
      border: 1px solid #555;
      image-rendering: pixelated;
    }
    canvas {
      display: none;
    }
  </style>
</head>
<body>
  <h1>Color to PNG Generator</h1>

  <label>Hex (e.g. #ff000080): 
    <input type="text" id="hex" value="#ff0000ff" maxlength="9" />
  </label>
  <br/>
  <label>R: <input type="number" id="r" min="0" max="255" value="255" /></label>
  <label>G: <input type="number" id="g" min="0" max="255" value="0" /></label>
  <label>B: <input type="number" id="b" min="0" max="255" value="0" /></label>
  <label>A (0.0–1.0): <input type="number" id="a" min="0" max="1" step="0.01" value="1.0" /></label>
  <br/>
  <button onclick="downloadPNG()">Download PNG</button>

  <div id="preview">
    <strong>Live Preview:</strong><br/>
    <img id="previewImg" width="128" height="128" />
  </div>

  <!-- Hidden canvas used for PNG generation -->
  <canvas id="canvas" width="256" height="256"></canvas>

  <script>
    const hexInput = document.getElementById("hex");
    const rInput = document.getElementById("r");
    const gInput = document.getElementById("g");
    const bInput = document.getElementById("b");
    const aInput = document.getElementById("a");
    const previewImg = document.getElementById("previewImg");
    const canvas = document.getElementById("canvas");
    const ctx = canvas.getContext("2d");

    function componentToHex(c) {
      const hex = c.toString(16);
      return hex.length == 1 ? "0" + hex : hex;
    }

    function rgbaToHex(r, g, b, a) {
      const alpha255 = Math.round(a * 255);
      return (
        "#" +
        componentToHex(r) +
        componentToHex(g) +
        componentToHex(b) +
        componentToHex(alpha255)
      );
    }

    function hexToRGBA(hex) {
      hex = hex.replace("#", "");
      if (hex.length === 6 || hex.length === 8) {
        const r = parseInt(hex.slice(0, 2), 16);
        const g = parseInt(hex.slice(2, 4), 16);
        const b = parseInt(hex.slice(4, 6), 16);
        const a = hex.length === 8 ? parseInt(hex.slice(6, 8), 16) / 255 : 1.0;
        rInput.value = r;
        gInput.value = g;
        bInput.value = b;
        aInput.value = a.toFixed(2);
        updateCanvas(false); // Prevent circular update
      }
    }

    function updateCanvas(updateHex = true) {
      const r = parseInt(rInput.value);
      const g = parseInt(gInput.value);
      const b = parseInt(bInput.value);
      const a = parseFloat(aInput.value);

      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = `rgba(${r},${g},${b},${a})`;
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      const dataURL = canvas.toDataURL("image/png");
      previewImg.src = dataURL;

      if (updateHex) {
        hexInput.value = rgbaToHex(r, g, b, a);
      }
    }

    function downloadPNG() {
      const link = document.createElement("a");
      link.download = "color.png";
      link.href = canvas.toDataURL("image/png");
      link.click();
    }

    // Sync updates
    [rInput, gInput, bInput, aInput].forEach(input => {
      input.addEventListener("input", () => updateCanvas(true));
    });

    hexInput.addEventListener("input", () => hexToRGBA(hexInput.value));

    // Initial preview
    updateCanvas();
  </script>
</body>
</html>

```
