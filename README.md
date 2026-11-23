# EndocrinIA

Sistema de aprendizaje para la asignatura **Patología Médico Quirúrgica del Sistema Endocrino y de la Nutrición** (UMH, código 1957, Grado en Medicina, plan 2024; 5.º curso, 1.er semestre, 6 ECTS). Se aprende haciendo: laboratorio, cálculo clínico, simulación fisiológica, imagen, urgencias, cirugía y casos, con práctica espaciada y adaptativa. Es una aplicación web de un solo fichero.

**Usar la app:** https://fborrasumh.github.io/endocrinia/

**Idiomas:** español (por defecto), inglés y portugués, también en los contenidos clínicos. Selector en la barra superior, o `?lang=en` / `?lang=pt` en la URL.

## Qué hace

- **30 temas, 9 unidades y 111 resultados de aprendizaje (RA)**, con el texto literal de la Guía Docente, en `src/curriculum.js`.
- **86 actividades de 8 tipos.** Cada una lleva su RA y explica por qué te toca.
  - **Laboratorio** (12 generadores): eje tiroideo, calcio y PTH, Cushing, Addison, aldosteronismo, diabetes insípida, diagnóstico de diabetes, acromegalia, hiponatremia, metanefrinas, hipoglucemia y realimentación. Cada caso es una analítica nueva, y el diagnóstico lo comprueba un clasificador independiente del generador.
  - **Cálculo clínico** (19): osmolalidad, Na corregido, anion gap, dosis diaria total y bolo de insulina, Ca corregido, LDL y objetivo, HOMA, IMC, gasto energético, nutrición, dosis de corticoide y de levotiroxina, déficit de agua, Adrogué-Madias, BWS, ARR y washout suprarrenal. La solución la calcula el código, y además detecta errores típicos (unidades, fórmula equivocada…) y los explica.
  - **Ejes hormonales:** predice el sentido de cada hormona del eje antes de ver el resultado.
  - **Imagen esquemática:** TI-RADS, gammagrafía, TC suprarrenal, RM selar, retinografía y pie diabético (Wagner).
  - **Simuladores hora a hora:** CAD y EHH, siguiendo el consenso de 2024 (K⁺ < 3,5 sin insulina; dextrosa por debajo de 250 mg/dL; bicarbonato solo si pH < 7,0). También hiponatremia sintomática según Adrogué-Madias, con límite de corrección.
  - **15 casos clínicos por pasos** (casos, urgencias y cirugía). Hay pasos de elegir una respuesta, de elegir varias y de ordenar.
  - **Banco de 185 viñetas trilingües**, al menos 6 por tema, con nivel de confianza.
- **Motor adaptativo y explicable** (lógica pura, en `src/motor.js`).
  - Mide el dominio de cada tema con un modelo tipo Elo y estima el olvido con una semivida (R = 2^(−Δt/h)).
  - La prioridad intercala unidades y tipos de actividad, y sigue el calendario del curso si se activa.
  - Con los mismos eventos sale siempre la misma ruta (semillas deterministas).
- **Diagnóstico inicial, simulacro de examen y progreso.**
  - El simulacro tiene 30 preguntas y se puntúa con −0,33 por error, como el examen de la guía.
  - El progreso muestra el dominio y la retención por tema, la calibración de la confianza (Brier) y una estimación de la nota.
  - Se puede exportar un informe en Word, sin red.
- **Tutor de IA opcional:** pistas durante la actividad y un tutor oral que pregunta y corrige.
- **Panel del profesorado.**
  - Validar contenidos, con firma.
  - Analítica de ítems: dificultad, discriminación (r punto-biserial) y distractores.
  - Preguntas propias con imagen y paquete de contenidos en JSON.
  - Exportación a Moodle XML (−33,33 % por error) y GIFT.

### Técnicas de aprendizaje con evidencia (DOI comprobados en Crossref)

| Técnica | Dónde | Referencia |
|---|---|---|
| Práctica de recuperación | todas las actividades | Roediger y Karpicke 2006 · 10.1111/j.1467-9280.2006.01693.x; Larsen et al. 2009 · 10.1111/j.1365-2923.2009.03518.x |
| Espaciado | motor (semivida) | Cepeda et al. 2006 · 10.1037/0033-2909.132.3.354; Settles y Meeder 2016 · 10.18653/v1/P16-1174 |
| Intercalado | plan de sesión | Rohrer y Taylor 2007 · 10.1007/s11251-007-9015-8 |
| Ejemplos resueltos con retirada gradual | cálculos | Renkl y Atkinson 2003 · 10.1207/S15326985EP3801_3 |
| Feedback elaborado inmediato | todas | Hattie y Timperley 2007 · 10.3102/003465430298487 |
| Predecir antes de observar | ejes, simuladores | Brod et al. 2018 · 10.1016/j.learninstruc.2018.01.013 |
| Calibración de la confianza | viñetas, casos | Butler et al. 2008 · 10.1037/0278-7393.34.4.918 |
| Autoexplicación | casos, tutor | Chi et al. 1994 · 10.1207/s15516709cog1803_3 |
| Simulación | CAD/EHH, hiponatremia | Cook et al. 2011 · 10.1001/jama.2011.1234 |
| Razonamiento clínico | casos por pasos | Schmidt y Mamede 2015 · 10.1111/medu.12775 |
| Modelo Elo adaptativo | motor | Pelánek 2016 · 10.1016/j.compedu.2016.03.017 |

No usamos «estilos de aprendizaje» (Pashler et al. 2008 · 10.1111/j.1539-6053.2009.01038.x).

## Qué comprueba el código y qué hace la IA

- **El código** genera los casos, calcula las soluciones, clasifica las analíticas, hace avanzar los simuladores, corrige, puntúa y decide la ruta. Todo esto funciona **sin clave y sin conexión**.
- **La IA** (opcional, con la clave de cada persona: OpenAI, Gemini o Claude) da pistas y hace de tutor oral. El código la vigila:
  - quita de la pista cualquier frase que revele la respuesta;
  - comprueba literalmente las citas que la IA atribuye al estudiante;
  - descarta los RA que no pertenecen al tema.
- **La IA nunca pone notas.**

## Privacidad

- Todo se guarda en el navegador (IndexedDB). No hay servidor.
- La clave de IA se queda en el navegador.
- Antes de enviar algo al proveedor, la app enmascara los datos personales y muestra una vista previa de lo que va a salir.
- La copia JSON y la exportación para investigación no incluyen claves.

## Límites

- **Contenido pendiente de validación por el profesorado.** Las viñetas, los casos y los parámetros de los simuladores son material docente de apoyo, **no una herramienta clínica**.
- Las **imágenes son esquemas** en SVG, no imágenes médicas reales.
- Los **simuladores están simplificados**. Reproducen las decisiones clave de las guías, no la fisiología completa.
- La estimación de nota es orientativa.
- **No se ha probado con una clave real de IA.** Las pruebas usan IA simulada de los tres proveedores.
- No hay integración LTI ni SSO (necesitarían servidor). El banco se lleva a Moodle exportándolo.

## Pruebas realizadas

- Lógica en Node: actividades, coherencia entre generadores y clasificadores, simuladores, motor determinista.
- Navegador (Playwright): idiomas, CSP, ejemplo sin clave, filtro anti-fuga, verificación de citas, errores 429, simulacro, Word sin red, copia JSON, exportación anónima, panel del profesorado, Moodle XML, teclado, funcionamiento sin conexión.
- Accesibilidad: axe-core WCAG 2.2 AA sin infracciones.
- Móvil a 390 px sin desborde.
- Rendimiento: primera pantalla en 0,43 s con la CPU ×4. El HTML ocupa 0,89 MB.

## Investigación y diccionario de datos

La exportación para investigación es voluntaria. Lleva un **seudónimo aleatorio** y eventos codificados, **sin texto libre**:

| Campo | Significado |
|---|---|
| `ts` | fecha (día) |
| `act` | actividad |
| `tipo` | eje, lab, calc, img, sim, urg, qx, caso, test |
| `tema` | T01–T30 |
| `item` | id de la viñeta |
| `score` | 0–1 |
| `ok` | acierto |
| `conf` | seg, prob, adiv |
| `resp` | opción elegida (0–3) |
| `ms` | tiempo de respuesta |
| `modo` | normal, diag, examen |
| `etapa` | retirada del ejemplo resuelto (0, 1, 2) |

Además, la exportación incluye el seudónimo, la versión y la fecha.

La app produce los datos; **no produce por sí sola evidencia publicable**. Publicar resultados requiere:

- aprobación del comité de ética (OEP en la UMH);
- consentimiento informado y cumplimiento del RGPD;
- idealmente, un diseño preregistrado (OSF) con grupo de comparación.

## Adoptarla en otra asignatura o universidad

1. Edita `src/curriculum.js`: ficha, evaluación, unidades, temas, RA y calendario.
2. Ajusta o amplía los bancos (`src/banco_u*.js`, `src/casos.js`) y los generadores (`src/gen.js`).
3. Ejecuta `python3 build.py`, luego `node tests/logic_test.js` y `python3 tests/smoke_test.py`.
4. Publica el `index.html`.

## Autoría (CRediT)

- **Fernando Borrás Rocher** (Universidad Miguel Hernández de Elche) · ORCID [0000-0002-5519-4573](https://orcid.org/0000-0002-5519-4573). Conceptualización, software, metodología, redacción.
- **Antonio F. Compañ Rosique** (Universidad Miguel Hernández de Elche) · ORCID [0000-0002-8166-396X](https://orcid.org/0000-0002-8166-396X). Conceptualización, recursos (asignatura y Guía Docente), validación del contenido clínico.

## Cómo citar

Borrás Rocher, F. y Compañ Rosique, A. F. (2026). *EndocrinIA* (v1.0.0) [Software]. DOI en trámite.

## Licencia

Software bajo licencia MIT; véase [LICENSE](LICENSE). MIT es una licencia de software. Para los contenidos docentes (viñetas, casos) se sugiere CC BY 4.0; lo deciden los autores.
