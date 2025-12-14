# 🏥 Diagnóstico de Vulnerabilidades - Clínicas Veterinarias

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![GitHub Pages](https://img.shields.io/badge/demo-GitHub%20Pages-blue)](https://YOUR_USERNAME.github.io/vet-diagnostic-survey/)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?logo=tailwind-css&logoColor=white)

<p align="center">
  <img src="https://img.shields.io/badge/MarDigital™-NexusVet.AI-16a34a?style=for-the-badge" alt="MarDigital">
  <img src="https://img.shields.io/badge/VetConnect-Ecosystem-166534?style=for-the-badge" alt="VetConnect">
</p>

> **Ecosistema Integrado MarDigital™ NexusVet.AI/VetConnect**

Una herramienta de autodiagnóstico estructurada para clínicas veterinarias que evalúa 10 dimensiones clave y genera insights en tiempo real.

## 🌐 Demo en Vivo

**[👉 Ver Demo](https://YOUR_USERNAME.github.io/vet-diagnostic-survey/)**

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
# O accede a la demo: https://YOUR_USERNAME.github.io/vet-diagnostic-survey/
```

### Opción 2: Servidor local
```bash
# Clonar repositorio
git clone https://github.com/YOUR_USERNAME/vet-diagnostic-survey.git
cd vet-diagnostic-survey

# Servir con Python
python -m http.server 8000

# O con Node.js
npx serve .
```

Luego visita `http://localhost:8000`

---

## 📋 Características

- ✅ **10 Dimensiones de Análisis**: Estrategia, Cliente, Procesos, Bioseguridad, RRHH, Tecnología, Finanzas, Marketing, Calidad, Cumplimiento Legal
- ✅ **Preguntas Likert (1-5)** + Preguntas Abiertas
- ✅ **Análisis en Tiempo Real** con alertas visuales (semáforos)
- ✅ **Dashboard de Resultados** con gráfico radar
- ✅ **Exportación** a JSON y CSV
- ✅ **Diseño Responsivo** y profesional
- ✅ **Código modular** preparado para expansión

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

```sql
-- ============================================
-- ESQUEMA DE BASE DE DATOS
-- Diagnóstico Veterinario - NexusVet.AI
-- ============================================

-- Habilitar extensiones necesarias
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- ============================================
-- TABLA: clinics (Clínicas registradas)
-- ============================================
CREATE TABLE clinics (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255),
    phone VARCHAR(50),
    address TEXT,
    city VARCHAR(100),
    country VARCHAR(100) DEFAULT 'España',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- ============================================
-- TABLA: survey_sessions (Sesiones de encuesta)
-- ============================================
CREATE TABLE survey_sessions (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    clinic_id UUID REFERENCES clinics(id) ON DELETE SET NULL,
    respondent_role VARCHAR(50) NOT NULL,
    respondent_email VARCHAR(255),
    status VARCHAR(20) DEFAULT 'in_progress', -- 'in_progress', 'completed', 'abandoned'
    overall_score DECIMAL(3,2),
    started_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    completed_at TIMESTAMP WITH TIME ZONE,
    ip_address INET,
    user_agent TEXT,
    metadata JSONB DEFAULT '{}'::jsonb
);

-- ============================================
-- TABLA: survey_responses (Respuestas individuales)
-- ============================================
CREATE TABLE survey_responses (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    session_id UUID NOT NULL REFERENCES survey_sessions(id) ON DELETE CASCADE,
    section_id VARCHAR(50) NOT NULL,
    question_id VARCHAR(50) NOT NULL,
    question_type VARCHAR(20) NOT NULL, -- 'likert', 'open'
    response_value INTEGER, -- Para Likert (1-5)
    response_text TEXT, -- Para preguntas abiertas
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    CONSTRAINT unique_session_question UNIQUE(session_id, question_id)
);

-- ============================================
-- TABLA: section_scores (Puntuaciones por sección)
-- ============================================
CREATE TABLE section_scores (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    session_id UUID NOT NULL REFERENCES survey_sessions(id) ON DELETE CASCADE,
    section_id VARCHAR(50) NOT NULL,
    average_score DECIMAL(3,2),
    total_questions INTEGER,
    answered_questions INTEGER,
    low_score_count INTEGER DEFAULT 0, -- Respuestas con valor 1 o 2
    status VARCHAR(20), -- 'good', 'warning', 'danger'
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    CONSTRAINT unique_session_section UNIQUE(session_id, section_id)
);

-- ============================================
-- TABLA: insights (Insights generados)
-- ============================================
CREATE TABLE insights (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    session_id UUID NOT NULL REFERENCES survey_sessions(id) ON DELETE CASCADE,
    insight_type VARCHAR(20) NOT NULL, -- 'danger', 'warning', 'info', 'success'
    section_id VARCHAR(50),
    title VARCHAR(255) NOT NULL,
    message TEXT NOT NULL,
    priority INTEGER DEFAULT 3,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- ============================================
-- ÍNDICES para optimizar consultas
-- ============================================
CREATE INDEX idx_sessions_clinic ON survey_sessions(clinic_id);
CREATE INDEX idx_sessions_status ON survey_sessions(status);
CREATE INDEX idx_sessions_created ON survey_sessions(started_at DESC);
CREATE INDEX idx_responses_session ON survey_responses(session_id);
CREATE INDEX idx_responses_section ON survey_responses(section_id);
CREATE INDEX idx_scores_session ON section_scores(session_id);
CREATE INDEX idx_insights_session ON insights(session_id);
CREATE INDEX idx_insights_type ON insights(insight_type);

-- ============================================
-- FUNCIONES Y TRIGGERS
-- ============================================

-- Función para actualizar updated_at automáticamente
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$ language 'plpgsql';

-- Trigger para clinics
CREATE TRIGGER update_clinics_updated_at
    BEFORE UPDATE ON clinics
    FOR EACH ROW
    EXECUTE FUNCTION update_updated_at_column();

-- Función para calcular score de sección
CREATE OR REPLACE FUNCTION calculate_section_score(p_session_id UUID, p_section_id VARCHAR)
RETURNS TABLE(avg_score DECIMAL, total_q INTEGER, answered_q INTEGER, low_count INTEGER) AS $$
BEGIN
    RETURN QUERY
    SELECT 
        COALESCE(AVG(response_value)::DECIMAL(3,2), 0) as avg_score,
        COUNT(*)::INTEGER as total_q,
        COUNT(response_value)::INTEGER as answered_q,
        COUNT(*) FILTER (WHERE response_value <= 2)::INTEGER as low_count
    FROM survey_responses
    WHERE session_id = p_session_id 
      AND section_id = p_section_id 
      AND question_type = 'likert';
END;
$$ LANGUAGE plpgsql;

-- ============================================
-- VISTAS para reportes
-- ============================================

-- Vista de resumen por sesión
CREATE OR REPLACE VIEW v_session_summary AS
SELECT 
    ss.id as session_id,
    c.name as clinic_name,
    ss.respondent_role,
    ss.status,
    ss.overall_score,
    ss.started_at,
    ss.completed_at,
    COUNT(DISTINCT sr.question_id) as questions_answered,
    COUNT(DISTINCT i.id) as insights_count,
    COUNT(DISTINCT i.id) FILTER (WHERE i.insight_type = 'danger') as danger_insights
FROM survey_sessions ss
LEFT JOIN clinics c ON ss.clinic_id = c.id
LEFT JOIN survey_responses sr ON ss.id = sr.session_id
LEFT JOIN insights i ON ss.id = i.session_id
GROUP BY ss.id, c.name;

-- Vista de promedios por sección global
CREATE OR REPLACE VIEW v_section_averages AS
SELECT 
    section_id,
    AVG(average_score) as global_average,
    COUNT(*) as total_sessions,
    AVG(low_score_count) as avg_low_scores
FROM section_scores
GROUP BY section_id;

-- ============================================
-- ROW LEVEL SECURITY (RLS) - Opcional
-- ============================================

-- Habilitar RLS en tablas sensibles
ALTER TABLE survey_sessions ENABLE ROW LEVEL SECURITY;
ALTER TABLE survey_responses ENABLE ROW LEVEL SECURITY;

-- Política: Los usuarios autenticados pueden ver sus propias sesiones
-- (Descomentrar si se implementa autenticación)
-- CREATE POLICY "Users can view own sessions" ON survey_sessions
--     FOR SELECT USING (auth.uid()::text = metadata->>'user_id');

-- ============================================
-- DATOS DE PRUEBA (opcional)
-- ============================================

-- Insertar clínica de ejemplo
INSERT INTO clinics (name, email, city, country) VALUES
('Clínica Veterinaria Demo', 'demo@example.com', 'Madrid', 'España');
```

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
- MarDigital™ - Ecosistema Integrado

---

## 📞 Soporte

**Ecosistema Integrado MarDigital™**
- 📧 Email: lmarotomar@mardigitalhub.com
- 🌐 NexusVet.AI | VetConnect

---

<p align="center">
  <i>Science with Soul. Strategy with Purpose. Intelligence with Humanity.</i>
</p>

<p align="center">
  © 2024 MarDigital™ - Todos los derechos reservados
</p>
