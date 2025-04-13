# PruebaTecnicaPIXStudio
Este proyecto, desarrollado en **PIX**, automatiza el procesamiento de productos obtenidos desde un API pública, su almacenamiento en una base de datos SQL Server, la generación de reportes en Excel y el posterior envío de estos reportes a OneDrive usando Microsoft Graph API. Además, realiza un respaldo del archivo JSON ejecutado y finaliza diligenciando un formulario en Google Forms con los resultados del procesamiento.

## ⚙️ Tecnologías utilizadas

- **PIX (Plataforma de Automatización)** – Ejecución del flujo automatizado.
- **Microsoft Graph API** – Autenticación y subida de archivos a OneDrive.
- **SQL Server** – Almacenamiento de los productos procesados.
- **Excel (OpenXML)** – Generación de reportes dinámicos en formato `.xlsx`.
- **Google Forms** – Registro automatizado del resumen final del proceso.
- **HTTP Requests y JSON** – Consumo de APIs externas.

## 📦 Funcionalidades principales

- Obtención de productos desde [Fake Store API](https://fakestoreapi.com/products)
- Almacenamiento de datos en SQL Server
- Generación de reportes con estadísticas por categoría
- Backup del archivo JSON procesado
- Subida del reporte a OneDrive (Graph API)
- Diligenciamiento automático de un Google Form

---

## 🔑 Autenticación Microsoft Graph API

Antes de ejecutar el proceso, debes configurar tu propia aplicación en Azure para obtener las credenciales necesarias:

### 🔧 Pasos para configurar la app en Azure:

1. Ve a [Azure Portal - App Registrations](https://portal.azure.com/#view/Microsoft_AAD_RegisteredApps/ApplicationsListBlade).
2. Crea una **nueva app**.
3. Copia:
   - `ClientId`
   - `RedirectUri` (ejemplo: `http://localhost`)
4. Genera un **Client Secret** desde la sección **Certificates & Secrets**.
   - Copia el secreto generado inmediatamente (se muestra solo una vez).
   - Guarda este valor en el archivo `ClientSecret.txt` dentro del proyecto.
5. En la sección **API Permissions**, agrega los siguientes permisos:
   - `Files.ReadWrite.All`
   - `offline_access`
   - `User.Read`

### 🔗 Generar URL de autorización

Una vez configurado lo anterior, genera la URL para obtener el código de autorización. Reemplaza `TU_CLIENT_ID` con el que obtuviste:

```
https://login.microsoftonline.com/consumers/oauth2/v2.0/authorize
  ?client_id=TU_CLIENT_ID
  &response_type=code
  &redirect_uri=http://localhost
  &scope=User.Read Files.ReadWrite Files.ReadWrite.All offline_access
```

1. Abre esa URL en el navegador.
2. Inicia sesión con tu cuenta Microsoft.
3. Otorga los permisos requeridos.
4. Copia el código largo que aparece en la URL de redirección (comienza con `M.`).
5. Pega ese código en el archivo `AuthorizationCode.txt`.

---

## 🗂️ Estructura del proyecto

```
📁 Data
├── 📁 Input            # Archivos JSON de entrada
├── 📁 Output           # Archivos generados
├── 📁 Temp             # Archivos temporales
├── 📁 Reportes         # Reportes Excel generados
├── 📁 Evidencias       # Logs, respaldos, capturas

📁 Input
├── AuthorizationCode.txt     # Código de autorización Microsoft
├── ClientSecret.txt          # Secreto del cliente de Graph API
├── Plantilla API Archivo.txt # Plantilla para armar llamadas al API

📄 config.xlsx                 # Archivo con todas las configuraciones clave-valor (LAS CELDAS AMARILLAS SON PARA MODIFICACION)
```

## 📝 Requisitos

- PIX instalado y configurado.
- SQL Server Express con la base de datos `ProductosDB`.
- Cuenta Microsoft y App registrada en Azure.
- Conexión a Internet activa para llamadas a APIs y subida a OneDrive.

## ✅ Ejecución del proyecto

1. Ajusta los parámetros en `config.xlsx` según tu entorno.
2. Configura tu app en Azure, guarda el `ClientSecret` en `ClientSecret.txt` y el código de autorización en `AuthorizationCode.txt`.
3. Verifica la conexión a SQL Server.
4. Ejecuta el flujo automatizado desde PIX.

---

## 📌 Notas importantes

- El token de acceso expira, por lo que deberás renovar el código de autorización periódicamente.
- El nombre del archivo JSON a procesar debe coincidir con el valor configurado en `NombreArchivoJson` dentro del archivo `config.xlsx`.

---

## 🗄️ Script de creación de base de datos y tabla SQL Server

Antes de ejecutar el proyecto, asegúrate de tener creada la base de datos y la tabla correspondiente en SQL Server. Puedes usar el siguiente script:

```sql
-- Crear base de datos
CREATE DATABASE ProductosDB;
GO

-- Usar base de datos
USE ProductosDB;
GO

-- Crear tabla Productos
CREATE TABLE Productos (
    id INT PRIMARY KEY,
    title NVARCHAR(255),
    price DECIMAL(18, 2),
    description NVARCHAR(MAX),
    category NVARCHAR(100),
    fecha_insercion DATETIME DEFAULT GETDATE()
);
```

---

## 👨‍💻 Autor

Juan David Rodriguez – *Desarrollador RPA*
