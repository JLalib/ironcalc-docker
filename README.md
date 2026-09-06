# 📊 IronCalc Docker - Spreadsheet Engine Rust Autohospedado

[![GitHub Stars](https://img.shields.io/github/stars/ironcalc/IronCalc?style=for-the-badge&logo=github)](https://github.com/ironcalc/IronCalc)
[![Docker Pulls](https://img.shields.io/docker/pulls/ironcalc/ironcalc?style=for-the-badge&logo=docker)](https://hub.docker.com/r/ironcalc/ironcalc)
[![License](https://img.shields.io/github/license/ironcalc/IronCalc?style=for-the-badge)](https://github.com/ironcalc/IronCalc/blob/main/LICENSE-MIT)
[![Rust](https://img.shields.io/badge/Rust-1.75%2B-orange?style=for-the-badge&logo=rust)](https://www.rust-lang.org/)
[![Version](https://img.shields.io/github/v/release/ironcalc/IronCalc?style=for-the-badge)](https://github.com/ironcalc/IronCalc/releases)

---

## 📋 Descripción general

**IronCalc** es un motor de hojas de cálculo completamente autohospedado construido en **Rust** que proporciona una alternativa moderna a Excel, Google Sheets y LibreOffice Calc **sin vendor lock-in**. Ofrece soporte completo para archivos `.xlsx` (import/export preservando fórmulas y formato), **300+ funciones Excel-compatibles** (incluyendo `LET`, `LAMBDA`, `FILTER`), bindings oficiales para **Python** y **JavaScript** (WASM), y una interfaz web React lista para producción en el puerto **2080**.

> 🎯 **Propuesta clave**: Open source spreadsheet engine (Rust-powered) • Import/export .xlsx • 300+ funciones • Python/JS bindings • Multi-idioma • Self-hosted • MIT/Apache 2.0 • 2.5k+ ⭐

---

## ✨ Características principales

- 📁 **Import/Export .xlsx nativo** — Lee y escribe Excel preservando fórmulas, formato, estilos y rangos con nombre
- 🧮 **300+ funciones Excel-compatibles** — `LET`, `LAMBDA`, `FILTER`, `MAP`, `REDUCE`, dynamic arrays, formateo condicional
- 📑 **Múltiples hojas** — Workbooks multi-sheet, rangos con nombre, fórmulas cross-sheet
- 🎨 **Formatos avanzados** — Rich formatting, conditional formatting, alineación, bordes, colores
- 🐍 **Python bindings (PyPI)** — `pip install ironcalc` • Uso directo desde Python
- 🌐 **JavaScript bindings (npm + WASM)** — Node.js y browser support • `npm install ironcalc`
- ⚛️ **Web UI React 19** — Interfaz moderna, responsive, Docker-ready en puerto 2080
- 🌍 **Multi-idioma & multi-locale** — EN, DE, FR, IT, ES, etc. • Formato fecha/número por locale • Timezone handling
- 🔌 **API REST headless** — Integra con cualquier aplicación • Modo programático sin UI
- 💻 **CLI tools** — Procesa spreadsheets en scripts/bash
- ☁️ **Nextcloud integration** — App oficial (proof of concept) para abrir spreadsheets en Nextcloud
- ⚖️ **Licencia dual MIT + Apache 2.0** — Úsalo, modifícalo, despliégalo libremente

---

## 📋 Requisitos del sistema

- 🐳 **Docker & Docker Compose v2+**
- 💾 **2 GB - 4 GB RAM mínimo** (Rust app + Node.js WASM)
- 💿 **1 GB - 10 GB espacio disco** (aplicación + datos spreadsheets)
- 🔌 **Puerto TCP: 2080** (web UI, configurable via variable de entorno)
- 🦀 **Rust 1.75+** (opcional, solo si compilas from source)
- 📦 **Node.js 22+** (incluido en imagen Docker o necesario para desarrollo)
- ⚙️ **WASM toolchain** (incluido en imagen Docker)
- 🔒 **Opcional**: Reverse proxy (nginx/Caddy) + HTTPS para producción

> ⚠️ **Work-in-progress**: IronCalc está en desarrollo activo. Versión 1.0 aún en roadmap. Espera cambios. Lee el changelog antes de actualizar.

---

## 🐳 Instalación

### Opción A: Docker Compose (Recomendado)

```bash
# 1. Clonar repositorio oficial
git clone https://github.com/ironcalc/IronCalc.git
cd IronCalc

# 2. Compilar imagen + iniciar servicios (primera vez ~30-60s)
docker compose up --build

# 3. En segundo plano (después de la primera compilación)
docker compose up -d
```

### Opción B: Docker Hub (Imagen precompilada)

```bash
# Crear docker-compose.yml local
cat > docker-compose.yml << 'EOF'
services:
  ironcalc:
    image: ironcalc/ironcalc:latest
    container_name: ironcalc
    ports:
      - "2080:2080"
    volumes:
      - ./data:/app/data
    restart: unless-stopped
    environment:
      - RUST_LOG=info
EOF

# Iniciar
docker compose up -d
```

### Opción C: Desarrollo (Compilar from source)

```bash
# Requisitos: Rust 1.75+ + Node.js 22 + wasm-pack
git clone https://github.com/ironcalc/IronCalc.git
cd IronCalc

# Compilar backend Rust + frontend WASM + React UI
docker compose -f docker-compose.yml -f docker-compose.dev.yml up --build
```

---

## ⚙️ Configuración

1. **Puerto web UI** — Por defecto `2080`. Cambia en `docker-compose.yml`:
   ```yaml
   ports:
     - "8080:2080"  # Acceso en http://localhost:8080
   ```

2. **Persistencia de datos** — Volumen `./data:/app/data` guarda spreadsheets y configuración

3. **Log level** — Variable `RUST_LOG` (`trace`, `debug`, `info`, `warn`, `error`)

4. **Reverse proxy (producción)** — Configura nginx/Caddy apuntando a `http://ironcalc:2080` con HTTPS

5. **Autenticación** — No hay login por defecto. Añade auth en reverse proxy (Authelia, Authentik, basic auth)

6. **Variables de entorno adicionales**:
   ```yaml
   environment:
     - RUST_LOG=info
     - IRONCALC_BIND_ADDRESS=0.0.0.0:2080
     - IRONCALC_DATA_DIR=/app/data
   ```

---

## 🚀 Primeros pasos

1. **Accede a la interfaz web**
   ```
   http://localhost:2080          # Local
   http://192.168.1.100:2080      # Desde otro dispositivo en LAN
   ```

2. **Crea tu primer spreadsheet**
   - Interfaz limpia y moderna (React 19 + TypeScript)
   - Click "New Spreadsheet" o arrastra un `.xlsx` existente

3. **Edita y calcula**
   - Datos, fórmulas (`=SUM(A1:A10)`, `=LET(x, 5, x*2)`, `=FILTER(...)`)
   - Formato: fuentes, colores, bordes, alineación
   - Múltiples hojas (pestañas inferiores)

4. **Exporta tu trabajo**
   - Download → `.xlsx` (preserva fórmulas, formato, rangos con nombre)

> 💡 **Nota**: No hay autenticación por defecto. Para producción, configura auth en tu reverse proxy.

---

## 💡 Casos de uso

- 🏗️ **Aplicaciones con spreadsheets embebidos** — Integra IronCalc via Python/JS bindings en tu backend
- 📈 **Reportes automáticos** — Genera `.xlsx` con fórmulas + datos desde Python scripts sin Excel/LibreOffice
- ☁️ **SaaS con feature spreadsheet** — Multi-tenant • Proporciona UI Excel-like a tus usuarios
- 🔄 **Migración desde Excel** — Importa librerías `.xlsx` • Úsalas en Python/JS • Sin vendor lock-in
- ☁️ **Nextcloud integration** — Abre spreadsheets directamente en Nextcloud con IronCalc UI
- 📊 **Data analysis tools** — Carga datos, aplica fórmulas, exporta resultados `.xlsx`
- 🤖 **Automatización headless** — CLI + API REST para procesamiento batch de spreadsheets

---

## 🔒 Acceso remoto seguro

Para exponer IronCalc de forma segura en internet:

```nginx
# Ejemplo nginx + Let's Encrypt (Caddy es aún más simple)
server {
    listen 443 ssl http2;
    server_name calc.tudominio.com;

    ssl_certificate /etc/letsencrypt/live/calc.tudominio.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/calc.tudominio.com/privkey.pem;

    location / {
        proxy_pass http://localhost:2080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

**Recomendaciones**:
- ✅ Autenticación en reverse proxy (Authelia, Authentik, OAuth2 Proxy, Basic Auth)
- ✅ Rate limiting + fail2ban
- ✅ Solo HTTPS (HSTS, CSP headers)
- ✅ Red interna Docker aislada (`networks: internal`)

---

## 🛠️ Gestión y mantenimiento

```bash
# Ver estado de contenedores
docker compose ps

# Ver logs en tiempo real
docker compose logs -f

# Detener IronCalc
docker compose down

# Actualizar a última versión
docker compose pull
docker compose up -d --build

# Monitorear consumo de recursos
docker stats
# Típico: CPU 0-10% idle | RAM 400-800MB | Pico compilación: 2-4 GB

# Backup de datos (volumen ./data)
tar -czf ironcalc-backup-$(date +%F).tar.gz ./data

# Restaurar backup
tar -xzf ironcalc-backup-2026-09-15.tar.gz
```

---

## 📝 Licencia

**Dual licencia: MIT + Apache 2.0**

- ✅ Uso comercial permitido
- ✅ Modificación permitida
- ✅ Distribución permitida
- ✅ Uso privado permitido
- ❌ Sin garantía
- 📄 Ver [LICENSE-MIT](https://github.com/ironcalc/IronCalc/blob/main/LICENSE-MIT) y [LICENSE-APACHE](https://github.com/ironcalc/IronCalc/blob/main/LICENSE-APACHE)

---

## 🔗 Referencias oficiales

- 🌐 [IronCalc Official Website](https://ironcalc.com/)
- 📦 [IronCalc GitHub Repository](https://github.com/ironcalc/IronCalc)
- 📚 [Rust API Documentation](https://docs.rs/ironcalc/)
- 🐳 [Dockerfile - Cómo se compila](https://github.com/ironcalc/IronCalc/blob/main/Dockerfile)
- 💻 [Code Examples - Rust, Python, JavaScript](https://github.com/ironcalc/IronCalc/tree/main/examples)
- 💬 [GitHub Discussions - Comunidad](https://github.com/ironcalc/IronCalc/discussions)
- 💬 [Discord Community - Soporte](https://discord.gg/ironcalc)
- ☁️ [IronCalc para Nextcloud - App Nextcloud](https://github.com/ironcalc/nextcloud-ironcalc)

---

> 📖 **Artículo original**: [Cómo instalar IronCalc en Docker - Spreadsheet engine Rust autohospedado](https://genbyte.blogspot.com/2026/09/como-instalar-ironcalc-en-docker.html)  
> 🎥 **Vídeo tutorial**: [Canal GENBYTE en YouTube](https://www.youtube.com/@genbyte)  
> ☕ **Apoya el canal**: [Ko-fi](https://ko-fi.com/genbyte) | [Newsletter](https://genbyte.blogspot.com/newsletter)