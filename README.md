<!DOCTYPE html>
<html>
<head>
  <title>Riego Automático Estancia 2</title>
  <meta charset="UTF-8">
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #eaf4ea;
      color: #2c3e50;
      padding: 20px;
    }
    h1 {
      color: #27ae60;
      font-size: 28px;
    }
    button {
      background-color: #2ecc71;
      color: white;
      border: none;
      margin: 10px;
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
      border-radius: 5px;
    }
    button:hover {
      background-color: #27ae60;
    }
    #estado {
      margin-top: 20px;
      font-size: 18px;
      background: #ffffff;
      padding: 15px;
      border-radius: 5px;
      box-shadow: 0 0 5px rgba(0,0,0,0.1);
    }
  </style>
</head>
<body>
  <h1>Panel de Control - Riego Automático Estancia 2</h1>

  <div>
    <button onclick="setModo('Automatico')">Modo Automático</button>
    <button onclick="setModo('Manual')">Modo Manual</button>
  </div>

  <div>
    <button onclick="setBomba(true)">Encender Bomba</button>
    <button onclick="setBomba(false)">Apagar Bomba</button>
  </div>

  <div id="estado">Cargando estado...</div>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>

  <script>
    const firebaseConfig = {
      apiKey: "TU_API_KEY",
      authDomain: "TU_PROYECTO.firebaseapp.com",
      databaseURL: "https://gardenest2-76081-default-rtdb.firebaseio.com",
      projectId: "TU_PROYECTO",
      storageBucket: "TU_PROYECTO.appspot.com",
      messagingSenderId: "TU_SENDER_ID",
      appId: "TU_APP_ID"
    };

    firebase.initializeApp(firebaseConfig);
    const dbRef = firebase.database().ref("Read");

    function setModo(modo) {
      firebase.database().ref("Read/Modo").set(modo);
    }

    function setBomba(valor) {
      firebase.database().ref("Read/Bomba/Bomba").set(valor);
    }

    dbRef.on("value", (snapshot) => {
      const data = snapshot.val();
      document.getElementById("estado").innerHTML = `
        <strong>Estancia:</strong> ${data.Estancia ?? "Estancia 2"}<br>
        <strong>Modo:</strong> ${data.Modo}<br>
        <strong>Humedad:</strong> ${data.Humedad?.Humedad ?? "N/A"}%<br>
        <strong>Estado:</strong> ${data.Estado}<br>
        <strong>Aviso:</strong> ${data.Aviso}<br>
        <strong>Bomba:</strong> ${data.Bomba?.Bomba ? "Encendida" : "Apagada"}
      `;
    });
  </script>
</body>
</html>
