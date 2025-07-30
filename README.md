<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>CIBAO TAXI</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    * {
      margin: 0; padding: 0; box-sizing: border-box;
      font-family: 'Segoe UI', sans-serif;
    }

    body {
      height: 100vh;
      background: linear-gradient(-45deg, #0f0, #0ff, #f0f, #f00);
      background-size: 400% 400%;
      animation: neonBG 8s ease infinite;
      color: white;
      overflow: hidden;
    }

    @keyframes neonBG {
      0% { background-position: 0% 50% }
      50% { background-position: 100% 50% }
      100% { background-position: 0% 50% }
    }

    h1.logo {
      text-align: center;
      font-size: 3rem;
      color: #ff0000;
      text-shadow: 0 0 15px black;
      margin: 20px 0;
    }

    .container {
      max-width: 400px;
      margin: auto;
      background: rgba(0,0,0,0.6);
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px #0ff;
    }

    .container h2 {
      text-align: center;
      margin-bottom: 15px;
      color: #0ff;
    }

    input, select {
      width: 100%;
      padding: 10px;
      margin: 8px 0;
      border: none;
      border-radius: 5px;
      outline: none;
      background: #222;
      color: white;
    }

    button {
      width: 100%;
      padding: 10px;
      background: #00ffff;
      border: none;
      border-radius: 5px;
      font-weight: bold;
      cursor: pointer;
      transition: background 0.3s;
    }

    button:hover {
      background: #00ff99;
    }

    #loginPage, #mainPage {
      display: none;
    }

    .sidebar {
      position: fixed;
      top: 0; left: -220px;
      width: 220px;
      height: 100%;
      background: rgba(0,0,0,0.9);
      color: white;
      transition: left 0.3s;
      z-index: 999;
      padding-top: 50px;
    }

    .sidebar.open {
      left: 0;
    }

    .sidebar ul {
      list-style: none;
      padding: 0 20px;
    }

    .sidebar ul li {
      margin: 20px 0;
      font-size: 18px;
      cursor: pointer;
      color: #0ff;
    }

    .topbar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: rgba(0,0,0,0.8);
      padding: 10px 20px;
      color: white;
    }

    .toggle-btn, .status-switch {
      cursor: pointer;
      font-size: 24px;
    }

    .status-switch {
      padding: 5px 15px;
      border-radius: 30px;
      background: #f00;
      transition: background 0.3s;
    }

    .status-online {
      background: #0f0 !important;
    }

    .main-content {
      text-align: center;
      margin-top: 60px;
    }

    .main-content h2 {
      font-size: 2rem;
      color: #fff;
    }

  </style>
</head>
<body>

  <h1 class="logo">CIBAO TAXI</h1>

  <div class="container" id="registerPage">
    <h2>Registro de Conductor</h2>
    <input type="text" id="nombre" placeholder="Nombre completo">
    <input type="text" id="telefono" placeholder="Teléfono">
    <input type="email" id="correo" placeholder="Correo electrónico">
    <input type="password" id="clave" placeholder="Contraseña">
    <input type="text" id="direccion" placeholder="Dirección">
    <input type="text" id="vehiculo" placeholder="Modelo del vehículo">
    <input type="text" id="placa" placeholder="Placa del vehículo">
    <input type="number" id="año" placeholder="Año del vehículo">
    <input type="file" id="foto" accept="image/*">
    <button onclick="registrar()">Registrarse</button>
    <p style="text-align:center;margin-top:10px;">¿Ya tienes cuenta? <a href="#" onclick="mostrarLogin()">Iniciar sesión</a></p>
  </div>

  <div class="container" id="loginPage">
    <h2>Iniciar Sesión</h2>
    <input type="email" id="loginCorreo" placeholder="Correo electrónico">
    <input type="password" id="loginClave" placeholder="Contraseña">
    <button onclick="iniciarSesion()">Entrar</button>
  </div>

  <div id="mainPage">
    <div class="topbar">
      <div class="toggle-btn" onclick="toggleSidebar()">☰</div>
      <div class="status-switch" onclick="toggleEstado()" id="estadoBtn">DESCONECTADO</div>
    </div>

    <div class="main-content">
      <h2 id="bienvenida"></h2>
      <p style="margin-top:20px;font-size:18px;">Esperando solicitud de viaje...</p>
    </div>

    <div class="sidebar" id="sidebar">
      <ul>
        <li>👤 Perfil</li>
        <li>💰 Ganancias</li>
        <li>📜 Historial</li>
        <li>📈 Comisión</li>
        <li>❓ Ayuda</li>
        <li>💳 Tu dinero</li>
        <li onclick="toggleSidebar()">❌ Cerrar</li>
      </ul>
    </div>
  </div>

  <script>
    let usuario = {};

    function registrar() {
      usuario.nombre = document.getElementById('nombre').value;
      usuario.correo = document.getElementById('correo').value;
      usuario.clave = document.getElementById('clave').value;

      if (!usuario.nombre || !usuario.correo || !usuario.clave) {
        alert('Completa todos los campos.');
        return;
      }

      localStorage.setItem('conductor', JSON.stringify(usuario));
      alert('Registro exitoso.');
      mostrarLogin();
    }

    function mostrarLogin() {
      document.getElementById('registerPage').style.display = 'none';
      document.getElementById('loginPage').style.display = 'block';
    }

    function iniciarSesion() {
      let correo = document.getElementById('loginCorreo').value;
      let clave = document.getElementById('loginClave').value;
      let guardado = JSON.parse(localStorage.getItem('conductor'));

      if (guardado && guardado.correo === correo && guardado.clave === clave) {
        document.getElementById('loginPage').style.display = 'none';
        document.getElementById('mainPage').style.display = 'block';
        document.getElementById('bienvenida').innerText = `Bienvenido, ${guardado.nombre}`;
      } else {
        alert('Credenciales incorrectas.');
      }
    }

    function toggleSidebar() {
      let sidebar = document.getElementById('sidebar');
      sidebar.classList.toggle('open');
    }

    function toggleEstado() {
      let btn = document.getElementById('estadoBtn');
      if (btn.classList.contains('status-online')) {
        btn.classList.remove('status-online');
        btn.textContent = 'DESCONECTADO';
      } else {
        btn.classList.add('status-online');
        btn.textContent = 'CONECTADO';
      }
    }
  </script>
</body>
</html>
