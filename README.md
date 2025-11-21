
# 📘 **README.md – Games Shelf**

```md
<div align="center">

# 🎮 **Games Shelf**


![Banner](https://i.ibb.co/1zfbcR8/gameshelf-banner.png)

[![GitHub Repo](https://img.shields.io/badge/Repo-Kimp25%2FMulticore-blue?style=for-the-badge)](https://github.com/Kimp25/Multicore)
![Node Version](https://img.shields.io/badge/Node-18+-green?style=for-the-badge)
![Firebase](https://img.shields.io/badge/Firebase-Connected-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/AutoUpdate-Enabled-success?style=for-the-badge)
![Cron](https://img.shields.io/badge/CRON-3H-blue?style=for-the-badge)

---

</div>

## 🧩 **¿Qué es Games Shelf?**
**Games Shelf** es un sistema que busca automáticamente los precios, ofertas y datos relevantes de **200 videojuegos reales** usando web scraping, APIs oficiales y actualización continua mediante **GitHub Actions**.
https://frontend-six-jade-49.vercel.app/
Su objetivo es:

✔ Encontrar el precio mínimo disponible  
✔ Indicar si un juego está en oferta  
✔ Registrar datos reales desde Steam, GOG, Nintendo, Metacritic y HowLongToBeat  
✔ Mantener la base actualizada **cada 3 horas automáticamente**  
✔ Guardar todo en Firebase para ser consumido por tu Frontend

---

## 🏗️ **Arquitectura del Proyecto**

```

├── /worker
│     └── updateGames.js   
├── /scrapers
│     ├── steam_api.js
│     ├── gog.js
│     ├── nintendo.js
│     ├── metacritic.js
│     └── hltb.js
├── /firebase.js
├── package.json
└── games_base.json

````

---

## ⚙️ **¿Cómo funciona?**

### 1️⃣ Repositorio de Juegos  
El sistema mantiene un archivo `games_base.json` con **200 juegos reales**.  
No está quemado: puedes agregar o quitar juegos cuando quieras.

Ejemplo:

```json
{
  "name": "Stardew Valley",
  "slug": "stardew-valley",
  "platforms": ["PC", "Nintendo Switch"],
  "reference_price_usd": 14.99
}
````

---

### 2️⃣ Scrapers paralelos

Para cada juego, se consultan **5 fuentes en paralelo**:

| Fuente        | Tipo          | Datos extraídos      |
| ------------- | ------------- | -------------------- |
| Steam API     | Oficial       | Precios, descuentos  |
| GOG           | HTML scraping | Precio base y oferta |
| Nintendo      | HTML/APIs     | Precios por región   |
| Metacritic    | HTML          | Score, plataforma    |
| HowLongToBeat | HTML/API      | Horas de juego       |

Esto permite obtener información **rápida y robusta**.

---

### 3️⃣ Worker inteligente

El archivo `worker/updateGames.js`:

* Lee los 200 juegos
* Llama a todos los scrapers en paralelo
* Maneja timeouts para evitar bloqueos
* Limpia y unifica datos
* Calcula:

  * Precio mínimo
  * Si está en oferta
  * Score
  * Consolas disponibles
* Sube todo a Firebase

---

### 4️⃣ Base de datos en Firebase

Los resultados se guardan en:

```
/games/{slug}
```

Ejemplo:

```json
{
  "name": "Hades",
  "price_min_usd": 9.99,
  "is_sale": true,
  "score": 93,
  "platforms": ["PC", "Switch"]
}
```

---

## 🤖 **Automatización con GitHub Actions**

El proyecto se actualiza solo **cada 3 horas**.

```yml
on:
  schedule:
    - cron: "0 */3 * * *"
```

El workflow:

* Instala Node
* Instala dependencias
* Ejecuta el worker
* Usa tu secreto de Firebase:

```yml
env:
  FIREBASE_SERVICE_ACCOUNT_JSON: ${{ secrets.FIREBASE_SERVICE_ACCOUNT_JSON }}
```

---

## 🚀 **Deploy en Railway (opcional)**

Si deseas que tu backend quede disponible como API:

1. Crear proyecto en Railway
2. Conectar GitHub
3. Añadir variables de entorno necesarias
4. Railway genera automáticamente una URL para tu API
5. Usar esa URL en tu frontend

---

## 🖼️ **Capturas / Ilustraciones del Sistema**

### 📌 Flujo completo del sistema

![Flow](https://i.ibb.co/6njMq7W/flow-gameshelf.png)

---

## 🛠️ **Instalación local**

### 1️⃣ Instalar dependencias

```bash
npm install
```

### 2️⃣ Crear variable de entorno Firebase

```bash
export FIREBASE_SERVICE_ACCOUNT_JSON="(pegar JSON aquí)"
```

### 3️⃣ Ejecutar actualización manual

```bash
node worker/updateGames.js
```

---

## 📡 **Tecnologías Utilizadas**

* **Node.js 18+**
* **Firebase Admin**
* **HTML Scraping (Cheerio / Fetch)**
* **Steam Web API**
* **Metacritic Scraping**
* **HLTB Parsing**
* **GitHub Actions**
* **CRON Jobs**

---

## ✨ **Autores**

**Games Shelf – Multicore Project**
🔹 *Kimberly Padilla*
🔹 *Sebastián Garbanzo*
🔹 *Kevin Montano*
🔹 *KSK*

---

<div align="center">

### 🎮 Gracias por usar **Games Shelf**


</div>
```
