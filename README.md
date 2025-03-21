<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Décodage QR Code Wi‑Fi</title>
  <!-- Intégration d'une police professionnelle depuis Google Fonts -->
  <link href="https://fonts.googleapis.com/css?family=Roboto:400,500,700&display=swap" rel="stylesheet">
  <style>
    /* Style global */
    body { 
      font-family: 'Roboto', sans-serif; 
      margin: 0;
      padding: 0;
      background: linear-gradient(135deg, #141e30, #243b55);
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
      color: #fff;
    }
    .container {
      background: rgba(255, 255, 255, 0.1);
      border: 1px solid rgba(255, 255, 255, 0.2);
      border-radius: 10px;
      padding: 30px;
      text-align: center;
      width: 90%;
      max-width: 600px;
      box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
    }
    h1 { 
      color: #00BFFF; 
      margin-bottom: 20px; 
    }
    p { 
      font-size: 1.1rem; 
      margin-bottom: 15px;
    }
    input[type="file"] {
      margin-top: 10px;
      padding: 10px 15px;
      background-color: #00BFFF;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      color: #fff;
      font-weight: bold;
      transition: background-color 0.3s ease;
    }
    input[type="file"]:hover {
      background-color: #009acd;
    }
    #result { 
      white-space: pre-wrap; 
      margin-top: 20px; 
      padding: 15px; 
      background-color: #eaeaea; 
      border-radius: 5px;
      color: #000;
      font-size: 1.1rem;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Décodage d'un QR Code Wi‑Fi</h1>
    <p>Sélectionnez une image contenant le QR Code :</p>
    <input type="file" id="fileInput" accept="image/*">
    <canvas id="canvas" style="display: none;"></canvas>
    <p id="result"></p>
  </div>

  <!-- Inclusion de la bibliothèque jsQR -->
  <script src="https://unpkg.com/jsqr/dist/jsQR.js"></script>
  <script>
    document.getElementById('fileInput').addEventListener('change', function(e) {
      const file = e.target.files[0];
      if (!file) return;
      
      const reader = new FileReader();
      reader.onload = function(event) {
        const img = new Image();
        img.onload = function() {
          const canvas = document.getElementById('canvas');
          const ctx = canvas.getContext('2d');
          canvas.width = img.width;
          canvas.height = img.height;
          ctx.drawImage(img, 0, 0, img.width, img.height);

          const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
          // Décodage du QR Code avec jsQR
          const code = jsQR(imageData.data, canvas.width, canvas.height);
          
          if (code) {
            let output = "Contenu du QR Code : " + code.data + "\n";
            // Expression régulière pour extraire le mot de passe Wi‑Fi
            const wifiRegex = /WIFI:T:[^;]+;S:[^;]+;P:([^;]+);/;
            const match = code.data.match(wifiRegex);
            if(match) {
              output += "Mot de passe Wi‑Fi : " + match[1];
            } else {
              output += "Format QR non reconnu pour une connexion Wi‑Fi.";
            }
            document.getElementById('result').textContent = output;
          } else {
            document.getElementById('result').textContent = "QR Code non détecté.";
          }
        };
        img.src = event.target.result;
      };

      reader.readAsDataURL(file);
    });
    
  </script>
  <style>
    footer {
        position: fixed;
        bottom: 0;
        width: 100%;
        background-color: black;
        color: white;
        text-align: center;
        padding: 10px;
    }
</style>

<footer>
    ©2025 KEMUEL NESSEMON
</footer>

</body>
</html>

