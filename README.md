# Pro
Trabajo
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Miel Santa Rita</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet" />
    <style>
        body {
            background: #fff8e1;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        h2 {
            color: #8d6e63;
            font-weight: 700;
        }
        .btn-primary {
            background-color: #6f4e37;
            border-color: #6f4e37;
        }
        .btn-primary:hover {
            background-color: #5d3a1a;
            border-color: #5d3a1a;
        }
        .btn-success {
            background-color: #a1887f;
            border-color: #a1887f;
        }
        .btn-success:hover {
            background-color: #6d4c41;
            border-color: #6d4c41;
        }
        .btn-danger {
            background-color: #d84315;
            border-color: #d84315;
        }
        .btn-danger:hover {
            background-color: #bf360c;
            border-color: #bf360c;
        }
        .card {
            border-radius: 15px;
            box-shadow: 0 4px 8px rgba(150, 111, 51, 0.2);
        }
        .form-label {
            font-weight: 600;
            color: #6d4c41;
        }
        footer {
            margin-top: 40px;
            text-align: center;
            color: #6d4c41;
            font-style: italic;
        }
        .section {
            display: none;
        }
        .section.active {
            display: block;
        }
        .nav-buttons button {
            margin-right: 10px;
        }
        table input {
            width: 100%;
            border: none;
            background: transparent;
            outline: none;
        }
        table input:focus {
            background: #fff3e0;
            border-radius: 4px;
        }
    </style>
</head>
<body>
    <div class="container my-5">
        <h2 class="mb-4 text-center">🍯 Miel Santa Rita</h2>

        <div class="nav-buttons text-center mb-4">
            <button class="btn btn-primary" data-section="ventas">Ventas</button>
            <button class="btn btn-primary" data-section="agregarCarrito">Agregar al Carrito</button>
            <button class="btn btn-primary" data-section="nuevoProducto">Nuevo Producto</button>
            <button class="btn btn-primary" data-section="adminInventario">Administrar Inventario</button>
        </div>

        <!-- Sección Ventas -->
        <div id="ventas" class="section active">
            <div class="card p-4 mb-4">
                <h4 class="mb-3 text-center">🛒 Carrito de Compras</h4>
                <ul class="list-group mb-3" id="listaCarrito"></ul>
                <div class="d-flex justify-content-between align-items-center mb-3">
                    <h5>Total: $<span id="totalCompra">0.00</span></h5>
                    <button id="exportarExcelBtn" class="btn btn-success">Exportar Ventas a Excel</button>
                </div>

                <form id="formFinalizarCompra" class="row g-3 align-items-center">
                    <div class="col-auto">
                        <input type="number" id="pagoInput" class="form-control" placeholder="Pago recibido $" min="0" step="0.01" required />
                    </div>
                    <div class="col-auto">
                        <button type="submit" class="btn btn-primary">Finalizar Compra</button>
                    </div>
                    <div class="col-12 mt-3" id="cambioTexto" style="font-weight: 600;"></div>
                </form>
            </div>
        </div>

        <!-- Sección Agregar al Carrito -->
        <div id="agregarCarrito" class="section">
            <div class="card p-4 mb-4">
                <h4 class="mb-3 text-center">Agregar Producto al Carrito</h4>
                <form id="formAgregarProducto">
                    <div class="mb-3">
                        <label for="productoSelect" class="form-label">Producto</label>
                        <select id="productoSelect" class="form-select" required></select>
                    </div>
                    <div class="mb-3">
                        <label for="cantidadInput" class="form-label">Cantidad</label>
                        <input type="number" id="cantidadInput" class="form-control" value="1" min="1" required />
                    </div>
                    <button type="submit" class="btn btn-success w-100">Agregar al Carrito</button>
                </form>
            </div>
        </div>

        <!-- Sección Nuevo Producto -->
        <div id="nuevoProducto" class="section">
            <div class="card p-4 mb-4">
                <h4 class="mb-3 text-center">Agregar Nuevo Producto</h4>
                <form id="formNuevoProducto">
                    <div class="mb-3">
                        <label for="nombreNuevo" class="form-label">Nombre del Producto</label>
                        <input type="text" id="nombreNuevo" class="form-control" placeholder="Ejemplo: Miel de Azahar" required />
                    </div>
                    <div class="mb-3">
                        <label for="precioNuevo" class="form-label">Precio ($)</label>
                        <input type="number" id="precioNuevo" class="form-control" min="0" step="0.01" placeholder="Ejemplo: 150" required />
                    </div>
                    <div class="mb-3">
                        <label for="stockNuevo" class="form-label">Stock</label>
                        <input type="number" id="stockNuevo" class="form-control" min="0" placeholder="Ejemplo: 20" required />
                    </div>
                    <button type="submit" class="btn btn-primary w-100">Agregar Producto</button>
                </form>
            </div>
        </div>

        <!-- Sección Administrar Inventario -->
        <div id="adminInventario" class="section">
            <div class="card p-4 mb-4">
                <h4 class="mb-3 text-center">Administrar Inventario</h4>
                <div class="table-responsive">
                    <table class="table table-striped align-middle">
                        <thead>
                            <tr>
                                <th>Nombre</th>
                                <th>Precio ($)</th>
                                <th>Stock</th>
                                <th>Acciones</th>
                            </tr>
                        </thead>
                        <tbody id="tablaInventarioBody"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <footer>© 2025 Miel Santa Rita</footer>

    <script src="https://cdn.sheetjs.com/xlsx-latest/package/dist/xlsx.full.min.js"></script>

    <script>
        // Aquí va el JS que ya tenías (todo funciona igual)
        // ... (omito para ahorrar espacio en esta respuesta)
        // Lo importante es que todo tu JS original sigue tal como está en tu código anterior.
        // Si deseas que te lo combine todo en un archivo .html final, también puedo enviártelo.

        // Código del chatbot aquí abajo:
    </script>

    <!-- Chatbot flotante de búsqueda Google -->
    <style>
        #chatGoogle {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 300px;
            background: #fff8e1;
            border: 2px solid #6f4e37;
            border-radius: 15px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.2);
            display: none;
            flex-direction: column;
            font-family: 'Segoe UI', sans-serif;
            z-index: 9999;
        }

        #chatHeader {
            background: #6f4e37;
            color: #fff;
            padding: 10px;
            border-top-left-radius: 15px;
            border-top-right-radius: 15px;
            font-weight: bold;
            text-align: center;
        }

        #chatBody {
            padding: 10px;
            height: 200px;
            overflow-y: auto;
            font-size: 14px;
        }

        #chatInput {
            display: flex;
            border-top: 1px solid #ccc;
        }

        #chatInput input {
            flex: 1;
            border: none;
            padding: 10px;
            border-bottom-left-radius: 15px;
            outline: none;
            background: #fff;
        }

        #chatInput button {
            background: #6f4e37;
            color: white;
            border: none;
            padding: 10px 15px;
            cursor: pointer;
            border-bottom-right-radius: 15px;
        }

        #chatToggle {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: #6f4e37;
            color: white;
            border: none;
            border-radius: 50%;
            width: 60px;
            height: 60px;
            font-size: 24px;
            cursor: pointer;
            z-index: 9998;
        }
    </style>

    <div id="chatGoogle">
        <div id="chatHeader">🤖 Búsqueda Google</div>
        <div id="chatBody"></div>
        <div id="chatInput">
            <input type="text" id="googleQuery" placeholder="¿En qué puedo ayudarte?" />
            <button onclick="buscarEnGoogle()">🔎</button>
        </div>
    </div>

    <button id="chatToggle" onclick="toggleChat()">💬</button>

    <script>
        function toggleChat() {
            const chat = document.getElementById('chatGoogle');
            chat.style.display = chat.style.display === 'flex' ? 'none' : 'flex';
        }

        function buscarEnGoogle() {
            const input = document.getElementById('googleQuery');
            const query = input.value.trim();
            const chatBody = document.getElementById('chatBody');

            if (!query) return;

            let resumen = 'Aquí tienes más información:';
            const qLower = query.toLowerCase();

            if (qLower.includes("miel")) {
                resumen = "🍯 La miel es un producto natural con propiedades antioxidantes y antibacterianas.";
            } else if (qLower.includes("precio")) {
                resumen = "💰 El precio de la miel depende del tipo, origen y presentación. Aquí algunos ejemplos.";
            } else if (qLower.includes("beneficios")) {
                resumen = "✅ La miel ayuda con la tos, cicatrización y mejora la digestión.";
            } else if (qLower.includes("dónde comprar") || qLower.includes("donde comprar")) {
                resumen = "🛒 Puedes encontrar miel en tiendas locales, mercados o productores artesanales.";
            } else if (qLower.includes("abejas")) {
                resumen = "🐝 Las abejas son esenciales para la polinización y la biodiversidad.";
            } else {
                resumen = "🔎 Esto es lo que encontré para tu búsqueda:";
            }

            const enlace = https://www.google.com/search?q=${encodeURIComponent(query)};
            const msg = 
                <div><strong>Tú:</strong> ${query}</div>
                <div><strong>Bot:</strong> ${resumen}</div>
                <div><a href="${enlace}" target="_blank">📎 Ver en Google</a></div>
                <hr/>
            ;

            chatBody.innerHTML += msg;
            chatBody.scrollTop = chatBody.scrollHeight;
            input.value = '';
        }
    </script>
</body>
</html>
