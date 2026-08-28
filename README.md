# 🏥 VetClinic 360™ — Encuesta de Madurez Digital, Procesos e IA (Clínicas Veterinarias)

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![GitHub Pages](https://img.shields.io/badge/demo-GitHub%20Pages-blue)](https://lmarotomar.github.io/vet-diagnostic-survey/)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?logo=tailwind-css&logoColor=white)

<p align="center">
  <img src="https://img.shields.io/badge/BioVetAI™-NexusVet.AI-16a34a?style=for-the-badge" alt="BioVetAI">
  <img src="https://img.shields.io/badge/VetConnect-Ecosystem-166534?style=for-the-badge" alt="VetConnect">
</p>

> **Ecosistema Integrado BioVetAI™ NexusVet.AI/VetConnect**

Herramienta de autodiagnóstico para clínicas veterinarias — 26 preguntas en 8 bloques (marketing digital, tecnología, procesos, documentación clínica, adopción de IA, automatización de ingresos, barreras/inversión). Calcula un **Nivel de Madurez Digital** (Inicial / En Desarrollo / Avanzado) a partir de respuestas fácticas y objetivas — no autopercepción tipo "califícate 1-5" — evitando así afirmar autoridad certificadora que BioVetAI™ no tiene. Contenido completo y lógica de cálculo en `Encuesta_Madurez_Digital_Spec.md` (carpeta padre).

> Modelo anterior (10-12 dimensiones con score subjetivo tipo "Excelencia 5.0/5.0") quedó retirado 2026-08-04 tras feedback metodológico real de una prospecto (ver `01. Animal Hospital of Pembroke/`) — el nuevo modelo resuelve el problema por alcance (nunca pregunta fuera de la competencia de BioVetAI™), no por disclaimer.

## 🌐 Demo en Vivo

**[👉 Ver Demo](https://lmarotomar.github.io/vet-diagnostic-survey/)**

*(Actualiza este enlace después de publicar en GitHub Pages)*

---

## 📸 Capturas de Pantalla

<p align="center">
  <img src="docs/screenshot-intro.png" alt="Pantalla de inicio" width="45%">
  <img src="docs/screenshot-survey.png" alt="Encuesta" width="45%">
</p>
<p align="center">
  <img src="docs/screenshot-results.png" alt="Resultados" width="90%">
</p>

*(Las capturas se generarán automáticamente al usar la aplicación)*

---

## ⚡ Inicio Rápido

### Opción 1: Usar directamente (Sin instalación)
```bash
# Simplemente abre index.html en tu navegador
# O accede a la demo: https://lmarotomar.github.io/vet-diagnostic-survey/
```

### Opción 2: Servidor local
```bash
# Clonar repositorio
git clone https://github.com/lmarotomar/vet-diagnostic-survey.git
cd vet-diagnostic-survey

# Servir con Python
python -m http.server 8000

# O con Node.js
npx serve .
```

Luego visita `http://localhost:8000`

---

## 📋 Características

- ✅ **26 preguntas en 8 bloques**: Perfil, Marketing Digital, Tecnología, Procesos, Documentación Clínica, Adopción de IA, Automatización de Ingresos, Barreras/Inversión — ver `Encuesta_Madurez_Digital_Spec.md` (carpeta padre) para el detalle completo
- ✅ **Nivel de Madurez Digital** (Inicial / En Desarrollo / Avanzado) calculado de 20 preguntas fácticas — no autopercepción subjetiva
- ✅ **Datos de calificación de venta separados** (Bloque 7: barrera, disposición a invertir, solución preferida) — no se mezclan con el Nivel de Madurez
- ✅ **Tipos de pregunta**: single-choice, multi-choice, likert-1-5 (solo 2 preguntas), open
- ✅ **Exportación** a JSON y CSV
- ✅ **Diseño Responsivo** y profesional
- ✅ **Código modular** preparado para expansión

> **Nota de alcance:** el diseño evita afirmar autoridad certificadora fuera de la competencia de BioVetAI™ por construcción — nunca se pregunta sobre Legal, Finanzas o RRHH con intención de evaluarlos, y el Nivel de Madurez se calcula de hechos reportados (qué sistema usan, qué tan seguido publican), no de autopercepción de calidad.

---

## 🛠️ Stack Tecnológico Recomendado

| Capa | Tecnología | Justificación |
|------|------------|---------------|
| **Frontend** | React 18 + Tailwind CSS | Componentes reutilizables, estado reactivo, diseño responsivo |
| **Backend/DB** | Supabase (PostgreSQL) | Serverless, Auth integrado, API REST automática, Real-time |
| **Visualización** | Chart.js | Gráficos radar y barras ligeros |
| **Hosting** | Vercel / Netlify | Deploy instantáneo, SSL gratuito, CDN global |
| **IA (opcional)** | Claude API / OpenAI | Recomendaciones contextuales avanzadas |

---

## 🗄️ Esquema de Base de Datos (Supabase/PostgreSQL)

> ✅ **Migrado y en producción** (verificado 2026-08-28) — schema real del modelo de Madurez Digital, reusando el proyecto Supabase de VetPrompt Pro (RLS aísla estas 3 tablas de `casos_veterinarios`). Detalle de preguntas/bloques: `Encuesta_Madurez_Digital_Spec.md`.

```sql
-- ============================================
-- ESQUEMA DE BASE DE DATOS — VetClinic 360
-- Proyecto Supabase compartido con VetPrompt Pro
-- ============================================

CREATE TABLE clinics (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL,
    email TEXT,
    created_at TIMESTAMPTZ DEFAULT now()
);

CREATE TABLE survey_sessions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    clinic_id UUID REFERENCES clinics(id),
    maturity_score NUMERIC,
    maturity_tier TEXT,
    status TEXT DEFAULT 'completed',
    started_at TIMESTAMPTZ DEFAULT now(),
    completed_at TIMESTAMPTZ
);

CREATE TABLE survey_responses (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    session_id UUID NOT NULL REFERENCES survey_sessions(id),
    block_id TEXT NOT NULL,
    question_id TEXT NOT NULL,
    question_type TEXT NOT NULL, -- 'single-choice' | 'multi-choice' | 'likert-1-5' | 'open'
    value JSONB NOT NULL -- entero, array de enteros, o string según question_type
);

-- ============================================
-- ROW LEVEL SECURITY — solo INSERT para anon, sin SELECT
-- (nadie puede leer datos de otra clínica con la anon key)
-- ============================================
ALTER TABLE clinics ENABLE ROW LEVEL SECURITY;
ALTER TABLE survey_sessions ENABLE ROW LEVEL SECURITY;
ALTER TABLE survey_responses ENABLE ROW LEVEL SECURITY;

CREATE POLICY "anon insert clinics" ON clinics FOR INSERT TO anon WITH CHECK (true);
CREATE POLICY "anon update clinic email" ON clinics FOR UPDATE TO anon USING (true); -- captura tardía de email en resultados
CREATE POLICY "anon insert sessions" ON survey_sessions FOR INSERT TO anon WITH CHECK (true);
CREATE POLICY "anon insert responses" ON survey_responses FOR INSERT TO anon WITH CHECK (true);
```

**Nivel de Madurez Digital:** calculado client-side de las 20 preguntas `maturity=true` (bloques marketing, technology, processes, doc_clinical, ai_adoption, revenue_automation) — normalizado 0-1 por pregunta y promediado ×100. Tiers: 0-33 Inicial, 34-66 En Desarrollo, 67-100 Avanzado. Ver `computeMaturity()` en `index.html`.

**Bloques profile y barriers_investment** (inv1/inv2/inv3) se guardan igual pero no entran al cálculo de madurez — calificación de venta, mostrada aparte en resultados.

---

## 🔌 Integración con Supabase

### 1. Configurar Supabase Client

Crear archivo `supabase.js`:

```javascript
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = 'YOUR_SUPABASE_URL'
const supabaseAnonKey = 'YOUR_SUPABASE_ANON_KEY'

export const supabase = createClient(supabaseUrl, supabaseAnonKey)
```

### 2. Funciones de Base de Datos

```javascript
// database.js - Funciones para interactuar con Supabase

import { supabase } from './supabase'

// Crear nueva sesión de encuesta
export async function createSurveySession(clinicInfo) {
    // Primero, crear o buscar la clínica
    let { data: clinic } = await supabase
        .from('clinics')
        .select('id')
        .eq('name', clinicInfo.clinicName)
        .single()
    
    if (!clinic) {
        const { data: newClinic } = await supabase
            .from('clinics')
            .insert({ 
                name: clinicInfo.clinicName,
                email: clinicInfo.email 
            })
            .select()
            .single()
        clinic = newClinic
    }
    
    // Crear sesión
    const { data: session, error } = await supabase
        .from('survey_sessions')
        .insert({
            clinic_id: clinic?.id,
            respondent_role: clinicInfo.role,
            respondent_email: clinicInfo.email,
            status: 'in_progress'
        })
        .select()
        .single()
    
    if (error) throw error
    return session
}

// Guardar respuesta individual
export async function saveResponse(sessionId, questionData) {
    const { data, error } = await supabase
        .from('survey_responses')
        .upsert({
            session_id: sessionId,
            section_id: questionData.sectionId,
            question_id: questionData.questionId,
            question_type: questionData.type,
            response_value: questionData.type === 'likert' ? questionData.value : null,
            response_text: questionData.type === 'open' ? questionData.value : null
        }, {
            onConflict: 'session_id,question_id'
        })
        .select()
    
    if (error) throw error
    return data
}

// Guardar puntuaciones por sección
export async function saveSectionScores(sessionId, sectionScores) {
    const records = Object.entries(sectionScores).map(([sectionId, data]) => ({
        session_id: sessionId,
        section_id: sectionId,
        average_score: data.average,
        total_questions: data.total,
        answered_questions: data.total,
        low_score_count: data.lowScores,
        status: data.status
    }))
    
    const { error } = await supabase
        .from('section_scores')
        .upsert(records, {
            onConflict: 'session_id,section_id'
        })
    
    if (error) throw error
}

// Guardar insights generados
export async function saveInsights(sessionId, insights) {
    const records = insights.map(insight => ({
        session_id: sessionId,
        insight_type: insight.type,
        section_id: insight.section,
        title: insight.title,
        message: insight.message,
        priority: insight.priority
    }))
    
    // Eliminar insights anteriores
    await supabase
        .from('insights')
        .delete()
        .eq('session_id', sessionId)
    
    // Insertar nuevos
    const { error } = await supabase
        .from('insights')
        .insert(records)
    
    if (error) throw error
}

// Completar sesión
export async function completeSession(sessionId, overallScore) {
    const { error } = await supabase
        .from('survey_sessions')
        .update({
            status: 'completed',
            overall_score: overallScore,
            completed_at: new Date().toISOString()
        })
        .eq('id', sessionId)
    
    if (error) throw error
}

// Obtener estadísticas globales
export async function getGlobalStats() {
    const { data, error } = await supabase
        .from('v_section_averages')
        .select('*')
    
    if (error) throw error
    return data
}

// Obtener historial de sesiones de una clínica
export async function getClinicHistory(clinicId) {
    const { data, error } = await supabase
        .from('v_session_summary')
        .select('*')
        .eq('clinic_id', clinicId)
        .order('started_at', { ascending: false })
    
    if (error) throw error
    return data
}
```

---

## 🚀 Despliegue

### Opción 1: Vercel (Recomendado)

```bash
# Instalar Vercel CLI
npm i -g vercel

# Deploy
vercel

# Configurar variables de entorno en Vercel Dashboard:
# - VITE_SUPABASE_URL
# - VITE_SUPABASE_ANON_KEY
```

### Opción 2: Netlify

```bash
# Instalar Netlify CLI
npm i -g netlify-cli

# Deploy
netlify deploy --prod
```

### Opción 3: GitHub Pages (Solo frontend estático)

El archivo `index.html` puede servirse directamente desde GitHub Pages sin necesidad de build.

---

## 🤖 Integración con IA (Opcional)

Para recomendaciones más sofisticadas, integrar con Claude API:

```javascript
// ai-recommendations.js

async function getAIRecommendations(sectionScores, responses) {
    const prompt = `
    Analiza los siguientes resultados de diagnóstico de una clínica veterinaria:
    
    Puntuaciones por sección:
    ${JSON.stringify(sectionScores, null, 2)}
    
    Genera 3-5 recomendaciones específicas y accionables, priorizadas por impacto.
    Formato: JSON array con {title, message, priority, section}
    `
    
    const response = await fetch('https://api.anthropic.com/v1/messages', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'x-api-key': 'YOUR_API_KEY',
            'anthropic-version': '2023-06-01'
        },
        body: JSON.stringify({
            model: 'claude-sonnet-4-20250514',
            max_tokens: 1024,
            messages: [{ role: 'user', content: prompt }]
        })
    })
    
    const data = await response.json()
    return JSON.parse(data.content[0].text)
}
```

---

## 📁 Estructura del Proyecto (Versión Producción)

```
veterinary-survey/
├── public/
│   └── favicon.ico
├── src/
│   ├── components/
│   │   ├── Header.jsx
│   │   ├── IntroScreen.jsx
│   │   ├── SurveyScreen.jsx
│   │   ├── ResultsScreen.jsx
│   │   ├── LikertScale.jsx
│   │   ├── QuestionCard.jsx
│   │   ├── InsightCard.jsx
│   │   ├── RadarChart.jsx
│   │   └── SectionNav.jsx
│   ├── data/
│   │   ├── sections.js
│   │   ├── roles.js
│   │   └── likertOptions.js
│   ├── hooks/
│   │   ├── useAnalysis.js
│   │   └── useSurveySession.js
│   ├── services/
│   │   ├── supabase.js
│   │   ├── database.js
│   │   └── ai-recommendations.js
│   ├── utils/
│   │   ├── exportData.js
│   │   └── analytics.js
│   ├── App.jsx
│   ├── main.jsx
│   └── index.css
├── .env.example
├── package.json
├── vite.config.js
├── tailwind.config.js
└── README.md
```

---

## 📊 Métricas y Analytics

Considera integrar:

- **Google Analytics 4**: Tracking de flujo de usuarios
- **Hotjar/Clarity**: Mapas de calor y grabaciones
- **Mixpanel**: Eventos de conversión

---

## 🔒 Seguridad

1. **Variables de entorno**: Nunca exponer API keys en el frontend
2. **RLS de Supabase**: Activar Row Level Security
3. **Rate Limiting**: Configurar en Supabase o edge functions
4. **Validación**: Validar inputs en frontend Y backend

---

## 🤝 Contribuciones

Las contribuciones son bienvenidas. Por favor:

1. Fork el repositorio
2. Crea una rama (`git checkout -b feature/nueva-funcionalidad`)
3. Commit tus cambios (`git commit -m 'Añadir nueva funcionalidad'`)
4. Push a la rama (`git push origin feature/nueva-funcionalidad`)
5. Abre un Pull Request

---

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo [LICENSE](LICENSE) para más detalles.

---

## 👨‍⚕️ Créditos

**Desarrollado por:**
- **Prof. Luis Orlando Maroto Martín, DVM, PhD**
- BioVetAI™ - Ecosistema Integrado

---

## 📞 Soporte

**Ecosistema Integrado BioVetAI™**
- 📧 Email: lmarotomar@biovetai.org
- 🌐 NexusVet.AI | VetConnect

---

<p align="center">
  <i>Science with Soul. Strategy with Purpose. Intelligence with Humanity.</i>
</p>

<p align="center">
  © 2025 BioVetAI™ - Todos los derechos reservados
</p>
