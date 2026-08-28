# 📚 Instrucciones para Publicar en GitHub Pages

## 🔧 CONFIGURACIÓN PREVIA (Importante)

Antes de publicar, necesitas configurar estos servicios:

### 1. HubSpot Meetings (YA CONFIGURADO ✅)

Link de reuniones: `https://meetings-na2.hubspot.com/luis-o`

### 2. Stripe o PayPal (para cobrar los $97)

**Opción A - Stripe Payment Link:**
1. Crea cuenta en [stripe.com](https://stripe.com)
2. Ve a Payment Links → Create
3. Producto: "Consulta de Diagnóstico Veterinario" - $97 USD
4. Copia el link y envíalo manualmente tras confirmar cita

**Opción B - PayPal.me:**
1. Configura tu link en [paypal.me](https://paypal.me)
2. Envía: `paypal.me/tu-usuario/97` tras confirmar cita

---

## Paso 1: Crear el Repositorio en GitHub

1. Ve a [github.com](https://github.com) e inicia sesión
2. Haz clic en el botón **"+"** (esquina superior derecha) → **"New repository"**
3. Configura el repositorio:
   - **Repository name**: `vet-diagnostic-survey` (o el nombre que prefieras)
   - **Description**: `Herramienta de autodiagnóstico para clínicas veterinarias - NexusVet.AI`
   - **Public** ✅ (necesario para GitHub Pages gratuito)
   - **Add a README file**: ❌ NO marcar (ya tenemos uno)
   - **Add .gitignore**: ❌ NO marcar (ya tenemos uno)
   - **Choose a license**: ❌ NO marcar (ya tenemos uno)
4. Clic en **"Create repository"**

---

## Paso 2: Subir los Archivos

### Opción A: Subir directamente desde GitHub (Más fácil)

1. En tu nuevo repositorio, haz clic en **"uploading an existing file"**
2. Arrastra TODOS los archivos descargados:
   - `index.html`
   - `README.md`
   - `LICENSE`
   - `.gitignore`
3. En "Commit changes":
   - Título: `Initial commit - Veterinary Diagnostic Survey`
   - Descripción: `Herramienta de diagnóstico de vulnerabilidades para clínicas veterinarias`
4. Clic en **"Commit changes"**

### Opción B: Usando Git desde terminal

```bash
# 1. Clonar el repositorio vacío
git clone https://github.com/lmarotomar/vet-diagnostic-survey.git
cd vet-diagnostic-survey

# 2. Copiar los archivos descargados a esta carpeta

# 3. Agregar todos los archivos
git add .

# 4. Hacer commit
git commit -m "Initial commit - Veterinary Diagnostic Survey"

# 5. Subir a GitHub
git push origin main
```

---

## Paso 3: Activar GitHub Pages 🌐

1. Ve a tu repositorio en GitHub
2. Haz clic en **"Settings"** (pestaña superior)
3. En el menú lateral, busca **"Pages"** (sección "Code and automation")
4. En **"Source"**:
   - Selecciona **"Deploy from a branch"**
5. En **"Branch"**:
   - Selecciona **"main"** (o "master")
   - Carpeta: **"/ (root)"**
6. Clic en **"Save"**

⏳ **Espera 1-2 minutos** mientras GitHub construye tu sitio.

---

## Paso 4: Obtener tu Enlace

1. Actualiza la página de Settings → Pages
2. Verás un mensaje verde: **"Your site is live at..."**
3. Tu URL será: `https://lmarotomar.github.io/vet-diagnostic-survey/`

🎉 **¡Listo! Ya puedes compartir este enlace.**

---

## Paso 5: Actualizar el README con tu enlace

1. Ve a tu repositorio
2. Abre `README.md`
3. Haz clic en el ícono de lápiz (editar)
4. Reemplaza todas las instancias de:
   - `lmarotomar` → tu nombre de usuario de GitHub
5. Clic en **"Commit changes"**

---

## 🔧 Configuración Opcional: Dominio Personalizado

Si quieres usar un dominio como `diagnostico.nexusvet.ai`:

1. En Settings → Pages → Custom domain
2. Ingresa tu dominio
3. Crea un registro CNAME en tu DNS apuntando a `lmarotomar.github.io`

---

## 📊 Verificar que funciona

1. Abre tu URL: `https://lmarotomar.github.io/vet-diagnostic-survey/`
2. Deberías ver la pantalla de inicio con:
   - Logo de diagnóstico veterinario
   - Formulario para nombre de clínica y rol
   - Botón "Comenzar Diagnóstico"

---

## 🐛 Solución de Problemas

### El sitio muestra 404
- Espera unos minutos más, GitHub Pages puede tardar
- Verifica que el archivo se llame exactamente `index.html`

### Los estilos no cargan
- Asegúrate de tener conexión a internet (usa CDN)
- Prueba en modo incógnito

### Error de CORS
- GitHub Pages no tiene este problema para archivos estáticos

---

## 📱 Compartir

Una vez publicado, puedes compartir:

**Enlace directo:**
```
https://lmarotomar.github.io/vet-diagnostic-survey/
```

**Código QR:**
Genera uno gratis en [qr-code-generator.com](https://www.qr-code-generator.com/)

**Redes sociales:**
```
🏥 Nueva herramienta gratuita para clínicas veterinarias!

📊 Diagnóstico de Vulnerabilidades - Evalúa las áreas clave de tu clínica (con score en tecnología/procesos/IA, contexto informativo en el resto)

👉 https://lmarotomar.github.io/vet-diagnostic-survey/

#Veterinaria #Gestión #NexusVet
```

---

## ✅ Checklist Final

### Configuración de Servicios
- [x] HubSpot Meetings configurado
- [x] Link de reuniones integrado en la app
- [ ] Stripe Payment Link O PayPal.me configurado

### GitHub
- [ ] Repositorio creado
- [ ] Archivos subidos
- [ ] GitHub Pages activado
- [ ] URL funcionando

### Pruebas
- [ ] Probar flujo completo de encuesta
- [ ] Verificar que botón "Agendar Mi Consulta" abra Calendly
- [ ] Probar en móvil
- [ ] Probar exportación JSON/CSV

### Marketing
- [ ] Compartir link en redes sociales
- [ ] Agregar a firma de email
- [ ] Crear QR Code
- [ ] Publicar en LinkedIn

---

## 💰 Proyección de Ingresos

| Visitas/mes | Conversión | Consultas | Ingresos |
|-------------|------------|-----------|----------|
| 100 | 3% | 3 | $291 USD |
| 500 | 3% | 15 | $1,455 USD |
| 1000 | 3% | 30 | $2,910 USD |

*Basado en tasa de conversión conservadora del 3%*

---

**¿Necesitas ayuda?** 
📧 lmarotomar@biovetai.org

---

*Ecosistema Integrado BioVetAI™ NexusVet.AI/VetConnect*
