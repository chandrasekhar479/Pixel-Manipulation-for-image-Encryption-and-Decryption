<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Image Encryption</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            background-color:yellowgreen;
        }
        canvas {
            border: 1px solid #000;
            margin-top: 20px;
        }
        #controls {
            margin-top: 20px;
        }
        button {
            padding: 10px 15px;
            margin-right: 10px;
            font-size: 1em;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h1>Simple Image Encryption Tool</h1>
    <input type="file" id="imageInput" accept="image/*">
    <div id="controls">
        <button onclick="encryptImage()">Encrypt Image</button>
        <button onclick="decryptImage()">Decrypt Image</button>
    </div>
    <canvas id="imageCanvas"></canvas>

    <script>
        const canvas = document.getElementById('imageCanvas');
        const ctx = canvas.getContext('2d');
        let originalImageData = null;

        document.getElementById('imageInput').addEventListener('change', function(event) {
            const file = event.target.files[0];
            const reader = new FileReader();
            reader.onload = function(e) {
                const img = new Image();
                img.onload = function() {
                    canvas.width = img.width;
                    canvas.height = img.height;
                    ctx.drawImage(img, 0, 0);
                    originalImageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
                };
                img.src = e.target.result;
            };
            reader.readAsDataURL(file);
        });

        function encryptImage() {
            if (!originalImageData) return;
            const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
            const data = imageData.data;

            for (let i = 0; i < data.length; i += 4) {
                data[i] = 255 - data[i];     
                data[i + 1] = 255 - data[i + 1]; 
                data[i + 2] = 255 - data[i + 2]; 
                
            }

            ctx.putImageData(imageData, 0, 0);
        }

        function decryptImage() {
            if (!originalImageData) return;
            ctx.putImageData(originalImageData, 0, 0);
        }
    </script>
</body>
</html>
