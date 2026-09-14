# 💳 Room4-xrp – Gestión de Billeteras y Trust Lines en XRP Ledger

Desarrollada con **Node.js + TypeScript** en el backend y **React + TypeScript** en el frontend.

---

## 📑 Tabla de contenidos

- [Características](#-características)
- [Tecnologías](#-tecnologías)
- [Arquitectura del proyecto](#-arquitectura-del-proyecto)
- [Requisitos previos](#-requisitos-previos)
- [Instalación](#-instalación)
  - [Frontend (capa_grafica)](#frontend-capa_grafica)
  - [Backend (capa_logica)](#backend-capa_logica)
- [Scripts disponibles](#-scripts-disponibles)
- [Documentación de la API](#-documentación-de-la-api)
- [Testing](#-testing)
- [Funcionamiento](#-funcionamiento)
- [Licencia](#-licencia)

---

## 🚀 Características

| Función | Descripción |
|---|---|
| 🔐 **Autenticación JWT** | Registro e inicio de sesión con validación robusta. |
| 👛 **Gestión de billeteras** | Agregar billeteras manualmente o simulando conexión con Xaman. |
| 🔗 **Trust Lines (RLUSD)** | Crear, sincronizar (consultar balance real en blockchain) y eliminar Trust Lines. |
| 💰 **Balances en tiempo real** | Consulta de saldo XRP y RLUSD directamente desde la testnet. |
| 🎨 **Diseño moderno** | Interfaz oscura con efectos de vidrio y animaciones. |
| 🧪 **Demo lista** | Conexión simulada con Xaman para pruebas sin necesidad de una wallet real. |

---

## 🛠 Tecnologías

### Backend
- **Node.js + Express**
- **TypeScript**
- **Sequelize (ORM)** + **MySQL**
- **JWT** para autenticación
- **XRPL.js** para interacción con el XRP Ledger
- **express-validator** para validaciones

### Frontend
- **React + TypeScript**
- **Vite** (build tool)
- **Tailwind CSS**
- **React Router DOM**
- **React Icons**

---

## 🏗 Arquitectura del proyecto

```
room4-xrp/
├── capa_logica/     # Backend: API REST (Node + Express + TypeScript + Sequelize + XRPL.js)
└── capa_grafica/    # Frontend: cliente web (React + Vite + Tailwind)
```

---

## 📋 Requisitos previos

- **Node.js ≥ 18**
- **MySQL** (local o remoto)
- **npm** o **yarn**
- *(Opcional)* Cuenta en Clever Cloud o similar para base de datos en la nube

---

## 📦 Instalación

### 1. Clonar el repositorio

```bash
git clone https://github.com/nicolasMLdev93/mi-app-xrpl.git
cd mi-app-xrpl
```

### Frontend (`capa_grafica`)

**2. Instalar dependencias**

```bash
cd capa_grafica
npm i
```

### Backend (`capa_logica`)

**3. Instalar dependencias y compilar**

```bash
cd capa_logica
npm i
npm run build
```

> 💡 El archivo `.env.example` (dentro de `capa_logica`) indica qué variables debés completar para conectar una base de datos en la nube. Copialo como `.env` y cargá tus propios valores:
> ```bash
> cd capa_logica
> cp .env.example .env
> ```

**4. Migrar la base de datos**

```bash
cd capa_logica
npm run migrate
```

---

## ▶️ Modo desarrollo

**Frontend** — ejecutar desde `capa_grafica`:

```bash
cd capa_grafica
npm run dev
```

**Backend** — ejecutar desde `capa_logica`:

```bash
cd capa_logica
npm start
```

---

## 🏗️ Compilar el backend para producción

```bash
cd capa_logica
npm run build
```

---

## 📜 Scripts disponibles

| Comando | Directorio | Descripción |
|---|---|---|
| `npm i` | `capa_grafica` | Instala las dependencias del frontend. |
| `npm run dev` | `capa_grafica` | Levanta el frontend en modo desarrollo. |
| `npm i` | `capa_logica` | Instala las dependencias del backend. |
| `npm run build` | `capa_logica` | Compila los archivos de `/src` a `/dist`. |
| `npm start` | `capa_logica` | Levanta el servidor backend (usa los archivos compilados en `/dist`). |
| `npm run migrate` | `capa_logica` | Aplica las migraciones de la base de datos. |
| `npm test` | `capa_logica` | Ejecuta la suite de tests del backend. |

---

## 📚 Documentación de la API

Una vez levantado el backend, la documentación de rutas y endpoints (Swagger) está disponible en:

```
http://localhost:3000/api-docs/
```

---

## 🧪 Testing

Ejecutar desde **`capa_logica`**:

```bash
cd capa_logica
npm test
```

---

## ⚙️ Funcionamiento

1. **Registro e inicio de sesión**
   Si aún no tenés una cuenta, registrate; si ya tenés una, iniciá sesión.

2. **Creación de una billetera simulada**
   Presioná el botón **"Conectar con Xaman"**. Esto genera automáticamente una billetera de prueba con 100 XRP de saldo inicial para operar en la Devnet.

3. **Manejo de la seed (clave privada)**
   Al tratarse de una aplicación de pruebas sin riesgo real de seguridad, la seed de la billetera se almacena en el `sessionStorage` del navegador para poder firmar transacciones. En un entorno real conectado a Mainnet, la seed se obtendría mediante la autorización de Xaman, sin exponerla nunca en el navegador.

4. **Alcance de las operaciones**
   Únicamente las billeteras simuladas pueden firmar y enviar transacciones, ya que son las únicas que tienen su seed asociada en el `sessionStorage`. Si se agrega una billetera ya existente en la Devnet (solo dirección pública), únicamente será posible consultar sus fondos, no operar con ella.

   Recordá que las billeteras de la Devnet y Testnet son usadas por múltiples usuarios en línea, por lo cual su saldo puede variar todo el tiempo de forma activa. Si las agregás a tu cuenta, serán almacenadas en la base de datos global de la aplicación y no se podrán agregar nuevamente con otro usuario.

   > ⚠️ El sistema siempre opera con la **billetera que se encuentra al tope de la lista**, ya que es la que tiene la seed asociada en el `sessionStorage`.
   > ⚠️ Si se borra de forma manual dicha seed, se creará otra billetera y se almacenará su nueva seed en el `sessionStorage`.

5. **Cierre de sesión**
   Si cerrás sesión y accedés con **otra cuenta**, la seed se elimina del `sessionStorage` de forma automática. Esto permite que la nueva cuenta pueda generar su propia billetera simulada.

   > ⚠️ Recordá: el sistema siempre usa la billetera simulada ubicada al tope, ya que es la que tiene la seed asociada. Si esa seed no está disponible, se mostrará un mensaje de error, ya que **es necesaria para firmar la transacción**. La aplicación es una simulación; en la vida real, la seed siempre se obtiene desde Xaman para poder firmar las transacciones.

6. **Documentación de la API**
   La documentación de las rutas y endpoints del backend está disponible en:
   ```bash
   http://localhost:3000/api-docs/
   ```

---
