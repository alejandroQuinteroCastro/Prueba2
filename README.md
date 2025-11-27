<!DOCTYPE html>
<html lang="es">
<head>


<script async src="https://www.googletagmanager.com/gtag/js?id=G-3WPE23GZK2"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-3WPE23GZK2');
</script>

    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>GameTunes - PWA</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --primary: #8A2BE2;
            --secondary: #00CED1;
            --accent: #FF6B6B;
            --dark: #121212;
            --darker: #0A0A0A;
            --light: #F8F9FA;
            --gray: #2D2D2D;
            --card-bg: rgba(255, 255, 255, 0.05);
        }

        body {
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            background: linear-gradient(135deg, var(--darker) 0%, var(--dark) 100%);
            color: var(--light);
            height: 100vh;
            overflow: hidden;
            line-height: 1.6;
        }

        /* PANTALLA DE INICIO */
        #homescreen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(145deg, var(--darker) 0%, #1a0a2e 100%);
            z-index: 1000;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            transition: transform 0.5s cubic-bezier(0.25, 0.46, 0.45, 0.94);
            padding: 20px;
        }

        #homescreen.hidden {
            transform: translateY(-100%);
        }

        .homescreen-content {
            text-align: center;
            width: 100%;
            max-width: 400px;
        }

        .app-logo {
            font-size: 4rem;
            margin-bottom: 1.5rem;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            filter: drop-shadow(0 0 10px rgba(138, 43, 226, 0.3));
        }

        .app-title {
            font-size: 2.2rem;
            font-weight: 800;
            margin-bottom: 0.5rem;
            background: linear-gradient(to right, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .app-subtitle {
            font-size: 1rem;
            opacity: 0.7;
            margin-bottom: 3rem;
        }

        .time-display {
            font-size: 3.5rem;
            font-weight: 300;
            margin-bottom: 0.5rem;
            letter-spacing: 2px;
        }

        .date-display {
            font-size: 1.1rem;
            opacity: 0.8;
            margin-bottom: 3rem;
        }

        .app-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
            margin-top: 2rem;
        }

        .app-icon {
            width: 70px;
            height: 70px;
            border-radius: 18px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            font-size: 1.8rem;
            cursor: pointer;
            transition: all 0.3s ease;
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .app-icon:active {
            transform: scale(0.92);
        }

        .app-icon-label {
            font-size: 0.7rem;
            margin-top: 5px;
            opacity: 0.8;
        }

        .gametunes-icon {
            background: linear-gradient(135deg, var(--primary), #6A0DAD);
            box-shadow: 0 6px 12px rgba(138, 43, 226, 0.3);
        }

        /* BARRA DE ESTADO */
        .status-bar {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 44px;
            background: rgba(10, 10, 10, 0.9);
            backdrop-filter: blur(10px);
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 20px;
            font-size: 14px;
            z-index: 999;
            border-bottom: 1px solid rgba(255, 255, 255, 0.05);
        }

        .time {
            font-weight: 600;
        }

        .status-icons {
            display: flex;
            gap: 8px;
        }

        /* INTERFAZ DE LA APLICACIÓN */
        #gametunes-app {
            position: fixed;
            top: 44px;
            left: 0;
            width: 100%;
            height: calc(100% - 44px);
            background: linear-gradient(145deg, var(--darker) 0%, var(--dark) 100%);
            z-index: 900;
            transform: translateY(100%);
            transition: transform 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
            overflow-y: auto;
            display: flex;
            flex-direction: column;
        }

        #gametunes-app.active {
            transform: translateY(0);
        }

        .app-header {
            background: rgba(18, 18, 18, 0.95);
            backdrop-filter: blur(15px);
            padding: 16px 20px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.08);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .app-brand {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .app-logo-small {
            font-size: 1.8rem;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .app-name {
            font-size: 1.3rem;
            font-weight: 700;
        }

        .user-actions {
            display: flex;
            gap: 15px;
        }

        .nav-tabs {
            display: flex;
            background: rgba(30, 30, 30, 0.7);
            border-radius: 50px;
            margin: 15px 20px;
            padding: 5px;
            backdrop-filter: blur(10px);
        }

        .nav-tab {
            flex: 1;
            text-align: center;
            padding: 12px 5px;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 0.9rem;
            font-weight: 500;
        }

        .nav-tab.active {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            box-shadow: 0 4px 8px rgba(138, 43, 226, 0.3);
        }

        .content-area {
            flex: 1;
            padding: 0 20px 20px;
            overflow-y: auto;
        }

        .search-container {
            position: relative;
            margin-bottom: 20px;
        }

        .search-bar {
            width: 100%;
            background: rgba(255, 255, 255, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 50px;
            padding: 15px 20px 15px 50px;
            color: var(--light);
            font-size: 1rem;
            outline: none;
            transition: all 0.3s ease;
        }

        .search-bar:focus {
            background: rgba(255, 255, 255, 0.12);
            border-color: var(--primary);
        }

        .search-icon {
            position: absolute;
            left: 20px;
            top: 50%;
            transform: translateY(-50%);
            font-size: 1.2rem;
            opacity: 0.7;
        }

        .category-filters {
            display: flex;
            gap: 10px;
            overflow-x: auto;
            padding-bottom: 10px;
            margin-bottom: 20px;
            scrollbar-width: none;
        }

        .category-filters::-webkit-scrollbar {
            display: none;
        }

        .filter-chip {
            background: rgba(255, 255, 255, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 50px;
            padding: 10px 20px;
            font-size: 0.85rem;
            white-space: nowrap;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .filter-chip.active {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            border-color: transparent;
        }

        .section-title {
            font-size: 1.4rem;
            font-weight: 700;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .section-title::after {
            content: '';
            flex: 1;
            height: 1px;
            background: linear-gradient(to right, var(--primary), transparent);
        }

        .product-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
        }

        .product-card {
            background: var(--card-bg);
            border-radius: 16px;
            padding: 15px;
            border: 1px solid rgba(255, 255, 255, 0.08);
            transition: all 0.3s ease;
            backdrop-filter: blur(10px);
            display: flex;
            flex-direction: column;
        }

        .product-card:active {
            transform: scale(0.98);
        }

        .product-image {
            width: 100%;
            height: 140px;
            border-radius: 12px;
            margin-bottom: 12px;
            object-fit: cover;
            background: var(--gray);
        }

        .product-badge {
            align-self: flex-start;
            background: var(--accent);
            color: white;
            font-size: 0.7rem;
            padding: 4px 10px;
            border-radius: 50px;
            margin-bottom: 8px;
        }

        .product-name {
            font-weight: 600;
            margin-bottom: 5px;
            font-size: 0.95rem;
            line-height: 1.3;
        }

        .product-meta {
            font-size: 0.75rem;
            opacity: 0.7;
            margin-bottom: 10px;
            line-height: 1.3;
        }

        .product-footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: auto;
        }

        .product-price {
            color: var(--secondary);
            font-weight: 700;
            font-size: 1.1rem;
        }

        .product-actions {
            display: flex;
            gap: 8px;
        }

        .action-btn {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            border: none;
            cursor: pointer;
            transition: all 0.2s ease;
            font-size: 1rem;
        }

        .action-btn:active {
            transform: scale(0.9);
        }

        .add-cart {
            background: var(--primary);
            color: white;
        }

        .favorite {
            background: rgba(255, 255, 255, 0.1);
            color: white;
        }

        /* BOTÓN HOME */
        .home-indicator {
            position: fixed;
            bottom: 12px;
            left: 50%;
            transform: translateX(-50%);
            width: 120px;
            height: 5px;
            background: rgba(255, 255, 255, 0.5);
            border-radius: 3px;
            z-index: 1000;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .home-indicator:active {
            background: rgba(255, 255, 255, 0.8);
            transform: translateX(-50%) scale(0.95);
        }

        /* BANNER DE INSTALACIÓN */
        .install-banner {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            background: rgba(30, 30, 30, 0.95);
            backdrop-filter: blur(15px);
            padding: 16px 20px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            display: none;
            justify-content: space-between;
            align-items: center;
            z-index: 950;
            transform: translateY(100%);
            transition: transform 0.3s ease;
        }

        .install-banner.show {
            transform: translateY(0);
        }

        .install-text {
            flex: 1;
        }

        .install-title {
            font-weight: 600;
            margin-bottom: 4px;
        }

        .install-subtitle {
            font-size: 0.85rem;
            opacity: 0.7;
        }

        .install-btn {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 50px;
            font-weight: 600;
            cursor: pointer;
            margin-right: 10px;
        }

        .close-btn {
            background: none;
            border: none;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
            width: 30px;
            height: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            transition: background 0.3s;
        }

        .close-btn:active {
            background: rgba(255, 255, 255, 0.1);
        }

        /* ANIMACIONES */
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .fade-in-up {
            animation: fadeInUp 0.5s ease-out;
        }

        /* PESTAÑA CARRITO */
        .cart-empty {
            text-align: center;
            padding: 60px 20px;
        }

        .cart-empty-icon {
            font-size: 4rem;
            margin-bottom: 20px;
            opacity: 0.5;
        }

        .cart-empty-text {
            opacity: 0.7;
            margin-bottom: 25px;
        }

        .explore-btn {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 50px;
            font-weight: 600;
            cursor: pointer;
            box-shadow: 0 4px 12px rgba(138, 43, 226, 0.3);
        }
    </style>
</head>
<body>
    <!-- BARRA DE ESTADO -->
    <div class="status-bar">
        <div class="time" id="current-time">9:41</div>
        <div class="status-icons">
            <span>📶</span>
            <span>📡</span>
            <span>🔋</span>
        </div>
    </div>

    <!-- PANTALLA DE INICIO -->
    <div id="homescreen">
        <div class="homescreen-content">
            <div class="app-logo">🎮</div>
            <h1 class="app-title">GameTunes</h1>
            <p class="app-subtitle">Juegos y Música en un solo lugar</p>
            
            <div class="time-display" id="homescreen-time">9:41</div>
            <div class="date-display" id="homescreen-date">Lunes 25 de Noviembre</div>
            
            <div class="app-grid">
                <div class="app-icon gametunes-icon" onclick="openApp()">
                    🎵
                    <div class="app-icon-label">GameTunes</div>
                </div>
                <div class="app-icon">
                    📱
                    <div class="app-icon-label">Apps</div>
                </div>
                <div class="app-icon">
                    🌐
                    <div class="app-icon-label">Web</div>
                </div>
                <div class="app-icon">
                    🎬
                    <div class="app-icon-label">Videos</div>
                </div>
                <div class="app-icon">
                    🛒
                    <div class="app-icon-label">Tienda</div>
                </div>
                <div class="app-icon">
                    ⚙️
                    <div class="app-icon-label">Ajustes</div>
                </div>
            </div>
        </div>
    </div>

    <!-- APLICACIÓN GAMETUNES -->
    <div id="gametunes-app">
        <div class="app-header">
            <div class="app-brand">
                <div class="app-logo-small">🎵</div>
                <div class="app-name">GameTunes</div>
            </div>
            <div class="user-actions">
                <span>🔔</span>
                <span>👤</span>
            </div>
        </div>

        <div class="nav-tabs">
            <div class="nav-tab active" onclick="switchTab('all')">Todos</div>
            <div class="nav-tab" onclick="switchTab('games')">Juegos</div>
            <div class="nav-tab" onclick="switchTab('music')">Música</div>
            <div class="nav-tab" onclick="switchTab('cart')">Carrito</div>
        </div>

        <div class="content-area">
            <!-- PESTAÑA TODOS -->
            <div id="all-tab" class="tab-content">
                <div class="search-container">
                    <div class="search-icon">🔍</div>
                    <input type="text" class="search-bar" placeholder="Buscar juegos, música...">
                </div>

                <div class="category-filters">
                    <div class="filter-chip active">Todos</div>
                    <div class="filter-chip">Acción</div>
                    <div class="filter-chip">Aventura</div>
                    <div class="filter-chip">Rock</div>
                    <div class="filter-chip">Pop</div>
                    <div class="filter-chip">Digital</div>
                    <div class="filter-chip">Físico</div>
                </div>

                <h2 class="section-title">Destacados</h2>
                
                <div class="product-grid">
                    <!-- Producto 1 -->
                    <div class="product-card fade-in-up">
                        <div class="product-badge">Nuevo</div>
                        <img src="https://via.placeholder.com/300x200?text=Videojuego+5" alt="Aventura Cósmica" class="product-image">
                        <div class="product-name">Aventura Cósmica</div>
                        <div class="product-meta">Ciencia Ficción | PC</div>
                        <div class="product-footer">
                            <div class="product-price">$49.99</div>
                            <div class="product-actions">
                                <button class="action-btn favorite">❤</button>
                                <button class="action-btn add-cart" onclick="addToCart('Aventura Cósmica', 49.99, 'https://via.placeholder.com/100x100?text=Videojuego+5', 'PC, Digital')">+</button>
                            </div>
                        </div>
                    </div>

                    <!-- Producto 2 -->
                    <div class="product-card fade-in-up">
                        <img src="https://via.placeholder.com/300x200?text=Album+5" alt="Melodías Relajantes" class="product-image">
                        <div class="product-name">Melodías Relajantes</div>
                        <div class="product-meta">Sonidos Zen | Digital</div>
                        <div class="product-footer">
                            <div class="product-price">$9.99</div>
                            <div class="product-actions">
                                <button class="action-btn favorite">❤</button>
                                <button class="action-btn add-cart" onclick="addToCart('Melodías Relajantes', 9.99, 'https://via.placeholder.com/100x100?text=Album+5', 'Digital')">+</button>
                            </div>
                        </div>
                    </div>

                    <!-- Producto 3 -->
                    <div class="product-card fade-in-up">
                        <div class="product-badge">Popular</div>
                        <img src="https://via.placeholder.com/300x200?text=Videojuego+6" alt="Fútbol Extremo 2025" class="product-image">
                        <div class="product-name">Fútbol Extremo 2025</div>
                        <div class="product-meta">Deportes | PS5</div>
                        <div class="product-footer">
                            <div class="product-price">$69.99</div>
                            <div class="product-actions">
                                <button class="action-btn favorite">❤</button>
                                <button class="action-btn add-cart" onclick="addToCart('Fútbol Extremo 2025', 69.99, 'https://via.placeholder.com/100x100?text=Videojuego+6', 'PS5, Físico')">+</button>
                            </div>
                        </div>
                    </div>

                    <!-- Producto 4 -->
                    <div class="product-card fade-in-up">
                        <img src="https://via.placeholder.com/300x200?text=Album+6" alt="Concierto en Vivo" class="product-image">
                        <div class="product-name">Concierto en Vivo</div>
                        <div class="product-meta">Banda Legendaria | CD</div>
                        <div class="product-footer">
                            <div class="product-price">$18.99</div>
                            <div class="product-actions">
                                <button class="action-btn favorite">❤</button>
                                <button class="action-btn add-cart" onclick="addToCart('Concierto en Vivo', 18.99, 'https://via.placeholder.com/100x100?text=Album+6', 'CD Físico')">+</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- PESTAÑA JUEGOS -->
            <div id="games-tab" class="tab-content" style="display: none;">
                <h2 class="section-title">Videojuegos</h2>
                
                <div class="product-grid">
                    <!-- Producto 1 -->
                    <div class="product-card fade-in-up">
                        <div class="product-badge">Nuevo</div>
                        <img src="https://via.placeholder.com/300x200?text=Videojuego+5" alt="Aventura Cósmica" class="product-image">
                        <div class="product-name">Aventura Cósmica</div>
                        <div class="product-meta">Ciencia Ficción | PC</div>
                        <div class="product-footer">
                            <div class="product-price">$49.99</div>
                            <div class="product-actions">
                                <button class="action-btn favorite">❤</button>
                                <button class="action-btn add-cart" onclick="addToCart('Aventura Cósmica', 49.99, 'https://via.placeholder.com/100x100?text=Videojuego+5', 'PC, Digital')">+</button>
                            </div>
                        </div>
                    </div>

                    <!-- Producto 3 -->
                    <div class="product-card fade-in-up">
                        <div class="product-badge">Popular</div>
                        <img src="https://via.placeholder.com/300x200?text=Videojuego+6" alt="Fútbol Extremo 2025" class="product-image">
                        <div class="product-name">Fútbol Extremo 2025</div>
                        <div class="product-meta">Deportes | PS5</div>
                        <div class="product-footer">
                            <div class="product-price">$69.99</div>
                            <div class="product-actions">
                                <button class="action-btn favorite">❤</button>
                                <button class="action-btn add-cart" onclick="addToCart('Fútbol Extremo 2025', 69.99, 'https://via.placeholder.com/100x100?text=Videojuego+6', 'PS5, Físico')">+</button>
                            </div>
                        </div>
                    </div>

                    <!-- Producto 5 -->
                    <div class="product-card fade-in-up">
                        <img src="https://via.placeholder.com/300x200?text=Videojuego+7" alt="Estrategia de Guerra" class="product-image">
                        <div class="product-name">Estrategia de Guerra</div>
                        <div class="product-meta">Estrategia | PC</div>
                        <div class="product-footer">
                            <div class="product-price">$34.99</div>
                            <div class="product-actions">
                                <button class="action-btn favorite">❤</button>
                                <button class="action-btn add-cart" onclick="addToCart('Estrategia de Guerra', 34.99, 'https://via.placeholder.com/100x100?text=Videojuego+7', 'PC, Digital')">+</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- PESTAÑA MÚSICA -->
            <div id="music-tab" class="tab-content" style="display: none;">
                <h2 class="section-title">Música</h2>
                
                <div class="product-grid">
                    <!-- Producto 2 -->
                    <div class="product-card fade-in-up">
                        <img src="https://via.placeholder.com/300x200?text=Album+5" alt="Melodías Relajantes" class="product-image">
                        <div class="product-name">Melodías Relajantes</div>
                        <div class="product-meta">Sonidos Zen | Digital</div>
                        <div class="product-footer">
                            <div class="product-price">$9.99</div>
                            <div class="product-actions">
                                <button class="action-btn favorite">❤</button>
                                <button class="action-btn add-cart" onclick="addToCart('Melodías Relajantes', 9.99, 'https://via.placeholder.com/100x100?text=Album+5', 'Digital')">+</button>
                            </div>
                        </div>
                    </div>

                    <!-- Producto 4 -->
                    <div class="product-card fade-in-up">
                        <img src="https://via.placeholder.com/300x200?text=Album+6" alt="Concierto en Vivo" class="product-image">
                        <div class="product-name">Concierto en Vivo</div>
                        <div class="product-meta">Banda Legendaria | CD</div>
                        <div class="product-footer">
                            <div class="product-price">$18.99</div>
                            <div class="product-actions">
                                <button class="action-btn favorite">❤</button>
                                <button class="action-btn add-cart" onclick="addToCart('Concierto en Vivo', 18.99, 'https://via.placeholder.com/100x100?text=Album+6', 'CD Físico')">+</button>
                            </div>
                        </div>
                    </div>

                    <!-- Producto 6 -->
                    <div class="product-card fade-in-up">
                        <img src="https://via.placeholder.com/300x200?text=Album+7" alt="Clásicos del Rock" class="product-image">
                        <div class="product-name">Clásicos del Rock</div>
                        <div class="product-meta">Varios | Vinilo</div>
                        <div class="product-footer">
                            <div class="product-price">$29.99</div>
                            <div class="product-actions">
                                <button class="action-btn favorite">❤</button>
                                <button class="action-btn add-cart" onclick="addToCart('Clásicos del Rock', 29.99, 'https://via.placeholder.com/100x100?text=Album+7', 'Vinilo Físico')">+</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- PESTAÑA CARRITO -->
            <div id="cart-tab" class="tab-content" style="display: none;">
                <div class="cart-empty">
                    <div class="cart-empty-icon">🛒</div>
                    <h3>Tu carrito está vacío</h3>
                    <p class="cart-empty-text">Agrega algunos productos increíbles</p>
                    <button class="explore-btn" onclick="switchTab('all')">Explorar Productos</button>
                </div>
            </div>
        </div>

        <!-- BANNER DE INSTALACIÓN -->
        <div class="install-banner" id="install-banner">
            <div class="install-text">
                <div class="install-title">Instalar GameTunes</div>
                <div class="install-subtitle">Para una experiencia completa</div>
            </div>
            <div style="display: flex; align-items: center;">
                <button class="install-btn" onclick="simulateInstall()">Instalar</button>
                <button class="close-btn" onclick="closeInstallBanner()">×</button>
            </div>
        </div>
    </div>

    <!-- BOTÓN HOME -->
    <div class="home-indicator" onclick="goToHomeScreen()"></div>

    <script>
        // ACTUALIZAR HORA
        function updateTime() {
            const now = new Date();
            const time = now.toLocaleTimeString('es-ES', { hour: '2-digit', minute: '2-digit' });
            const date = now.toLocaleDateString('es-ES', { 
                weekday: 'long', 
                year: 'numeric', 
                month: 'long', 
                day: 'numeric' 
            });
            
            document.getElementById('current-time').textContent = time;
            document.getElementById('homescreen-time').textContent = time;
            document.getElementById('homescreen-date').textContent = date.charAt(0).toUpperCase() + date.slice(1);
        }

        // ABRIR APLICACIÓN
        function openApp() {
            const homescreen = document.getElementById('homescreen');
            const app = document.getElementById('gametunes-app');
            
            homescreen.classList.add('hidden');
            setTimeout(() => {
                app.classList.add('active');
            }, 300);
        }

        // VOLVER A PANTALLA DE INICIO
        function goToHomeScreen() {
            const homescreen = document.getElementById('homescreen');
            const app = document.getElementById('gametunes-app');
            
            app.classList.remove('active');
            setTimeout(() => {
                homescreen.classList.remove('hidden');
            }, 300);
        }

        // CAMBIAR PESTAÑAS
        function switchTab(tabName) {
            // Ocultar todos los contenidos
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.style.display = 'none';
            });
            
            // Remover active de todos los nav items
            document.querySelectorAll('.nav-tab').forEach(item => {
                item.classList.remove('active');
            });
            
            // Mostrar contenido seleccionado
            document.getElementById(tabName + '-tab').style.display = 'block';
            
            // Activar nav item
            event.target.classList.add('active');
        }

        // AÑADIR AL CARRITO
        function addToCart(name, price, imageUrl, details) {
            alert(`"${name}" ($${price.toFixed(2)}) ha sido añadido al carrito. ¡Felicidades por tu excelente gusto!`);
            console.log(`Producto añadido: ${name}, Precio: ${price}, Imagen: ${imageUrl}, Detalles: ${details}`);
        }

        // SIMULAR INSTALACIÓN DE PWA
        function simulateInstall() {
            const banner = document.getElementById('install-banner');
            banner.classList.remove('show');
            
            setTimeout(() => {
                banner.style.display = 'none';
                alert('¡GameTunes instalada! 🎉\n\nAhora la app está disponible en tu pantalla de inicio.');
            }, 300);
        }

        // CERRAR BANNER DE INSTALACIÓN
        function closeInstallBanner() {
            const banner = document.getElementById('install-banner');
            banner.classList.remove('show');
            setTimeout(() => {
                banner.style.display = 'none';
            }, 300);
        }

        // GESTOS TÁCTILES PARA VOLVER AL INICIO
        let startY = 0;
        let currentY = 0;

        document.getElementById('gametunes-app').addEventListener('touchstart', (e) => {
            startY = e.touches[0].clientY;
        });

        document.getElementById('gametunes-app').addEventListener('touchmove', (e) => {
            currentY = e.touches[0].clientY;
        });

        document.getElementById('gametunes-app').addEventListener('touchend', (e) => {
            const diff = currentY - startY;
            if (diff > 100) { // Deslizar hacia abajo más de 100px
                goToHomeScreen();
            }
        });

        // INICIALIZACIÓN
        document.addEventListener('DOMContentLoaded', () => {
            updateTime();
            setInterval(updateTime, 60000); // Actualizar cada minuto
            
            // Mostrar banner de instalación después de 3 segundos
            setTimeout(() => {
                const banner = document.getElementById('install-banner');
                banner.style.display = 'flex';
                setTimeout(() => {
                    banner.classList.add('show');
                }, 10);
            }, 3000);

            console.log('📱 PWA de GameTunes cargada');
            console.log('🎮 Haz clic en el icono de GameTunes para abrir la app');
        });
    </script>
</body>
</html>
