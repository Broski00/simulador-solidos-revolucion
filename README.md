# Simulador Computacional de Sólidos de Revolución

Aplicación web interactiva desarrollada para la asignatura de **Cálculo Integral** en **Ingeniería de Sistemas** (Unidades Tecnológicas de Santander - UTS). Permite modelar, visualizar en 2D y 3D, y resolver analítica y numéricamente volúmenes de sólidos de revolución mediante los métodos de **discos**, **arandelas** y **cascarones cilíndricos** con funciones 100 % personalizables.

🔗 **Demo en vivo:** [https://simulador-solidos-uts.netlify.app](https://simulador-solidos-uts.netlify.app)

---

## Características principales

### 1. Métodos de integración y ejes de rotación
* **Método de Discos:** Calcula $V = \pi \int_a^b [R(x)]^2\,dx$ (o en $y$) cuando la región plana hace contacto directo con el eje de giro.
* **Método de Arandelas:** Calcula $V = \pi \int_a^b \left([R(x)]^2 - [r(x)]^2\right)dx$ para regiones huecas entre dos curvas $f$ y $g$, identificando automáticamente el radio exterior $R$ y el radio interior $r$ (con manejo especial en tramos donde la región cruza el eje de rotación).
* **Método de Cascarones Cilíndricos:** Calcula $V = 2\pi \int_a^b \rho(x)\,h(x)\,dx$ mediante tubos concéntricos paralelos al eje de giro, ideal cuando despejar la variable inversa no es viable.
* **Ejes de giro configurables:** Soporta rotación alrededor de ejes **horizontales ($y = c$)** y **verticales ($x = c$)**, incluyendo ejes desplazados ($c \neq 0$) con expresiones matemáticas.

### 2. Banco de 12 casos de estudio y modo libre
Incluye un catálogo aplicado al contexto de Ingeniería de Sistemas y validación geométrica:
* **3 ejemplos guiados interactivos (uno por método)** con explicación pedagógica en 6 pasos (interpretación de la región, justificación del método, elemento representativo, planteamiento, resolución y verificación):
  * *Discos:* Perfil de fibra óptica $f(x) = \sqrt{x}$ en $[0, 4]$ alrededor del eje $X$ ($V = 8\pi$).
  * *Arandelas:* Disipador térmico hueco entre $f(x) = \sqrt{x}$ y $g(x) = x^2$ en $[0, 1]$ alrededor del eje $X$ ($V = \frac{3}{10}\pi$).
  * *Cascarones:* Depósito de refrigeración líquida $y = 2x^2 - x^3$ en $[0, 2]$ alrededor del eje $Y$ ($V = \frac{16}{5}\pi$).
* **9 ejemplos adicionales precargados:** Cavidad parabólica $x = y^2$ (eje $Y$), lente/antena $f(x) = \text{sen}\,x$, cono y esfera de verificación geométrica, aspa de ventilador con eje desplazado $y = 2$, carcasa esférica perforada, carcasa $y = \text{sen}(x^2)$, cápsula IoT $y = -x^2 + 4x - 3$ y soporte con eje desplazado $x = 3$.
* **Modo desde cero:** Permite vaciar los campos y construir cualquier sólido personalizado.

### 3. Editor matemático inteligente y buscador de cortes
* **Sintaxis flexible sin restricciones:** Admite multiplicación implícita (`2x`, `3(x+1)`), potencias (`^`), funciones en español e inglés (`sen`/`sin`, `cos`, `tan`/`tg`, `arcsen`, `arccos`, `arctan`, `sinh`, `cosh`, `sqrt`, `cbrt`, `abs`, `exp`, `ln`, `log10`), constantes (`pi`/`π`, `e`), expresiones en los límites $a, b$ y $c$ (como `sqrt(8)` o `pi/2`), funciones por tramos con operador ternario (`x < 1 ? x : 2 - x`) y detección automática de la variable independiente (`x` o `y`).
* **Previsualización LaTeX en tiempo real:** Renderiza las expresiones escritas al instante mediante **KaTeX**, acompañado de un teclado rápido de símbolos.
* **Buscador automático de intersecciones (`⌖ Hallar cortes`):** Realiza un barrido numérico en $[-30, 30]$ refinado mediante **bisección** (70 iteraciones) para encontrar cortes entre curvas ($f = g$) o con el eje, convirtiéndolos en valores simbólicos exactos (enteros, fracciones, radicales $\sqrt{n}$ y múltiplos de $\pi$) y asignando los límites $[a, b]$.
* **Herramientas rápidas:** Intercambio directo de curvas (`⇄ Intercambiar f ↔ g`) y limpieza del entorno.

### 4. Doble motor de cálculo: simbólico y numérico
* **Motor simbólico en segundo plano (Web Worker):** Utiliza **Nerdamer** ejecutándose en un hilo independiente (con límite de tiempo de 7 segundos para no bloquear la interfaz) para expandir el integrando, obtener la antiderivada elemental, aplicar el Teorema Fundamental del Cálculo paso a paso en LaTeX y verificar el resultado contra el cálculo numérico.
* **Integración numérica de referencia:** Implementa **cuadratura de Simpson adaptativa** con subdivisión inicial en 16 tramos y tolerancia $\varepsilon = 10^{-12}$, además de un reconocedor de formas cerradas exactas (múltiplos racionales de $\pi$, $\pi^2$, $\sqrt{2}\pi$ y $\sqrt{3}\pi$).
* **Análisis de convergencia (Suma de Riemann por punto medio):** Calcula el volumen aproximado usando exactamente las $n$ secciones dibujadas ($1 \le n \le 200$), reporta los errores absoluto y relativo en vivo, y genera una tabla comparativa para $n = 2, 4, 8, 16, 32, 64, 128, 256$.

### 5. Visualización dual 2D y 3D interactiva
* **Plano cartesiano 2D (HTML5 Canvas):** Dibuja la región sombreada, las curvas $f$ y $g$, el eje de rotación, el rectángulo generador de la sección $k$ y los vectores anotados del elemento representativo ($R$, $r$, $\rho$ y $h$).
* **Modelo 3D en tiempo real (Three.js):** Genera por revolución (`LatheGeometry`) tanto la superficie translúcida del sólido como cada una de las $n$ secciones individuales con codificación de color HSL y resaltado de la sección actual $k$.
* **Herramientas de exploración didáctica:**
  * Control orbital completo (rotar, acercar con rueda y desplazar con clic derecho) más rotación automática.
  * Deslizadores dinámicos para el número de secciones ($n$), sección activa ($k$) y opacidad de la superficie.
  * **Vista en corte transversal ($270^\circ$)** para inspeccionar cavidades internas y modo para aislar una única sección.
  * **Animación de construcción paso a paso (`▶ Construir`)** que ensambla el sólido sección por sección y exportación directa de imagen (`Capturar PNG`).

---

## Tecnologías utilizadas

El proyecto está construido como una aplicación web autocontenida (*client-side*, sin necesidad de servidor ni proceso de compilación):

* **HTML5, CSS3 (Grid & Variables) y JavaScript (ES Modules)**
* **[Three.js (v0.160.0)](https://threejs.org/):** Renderizado 3D WebGL, iluminación, geometrías de revolución y `OrbitControls`.
* **[Math.js (v12.4.2)](https://mathjs.org/):** Análisis sintáctico, compilación y evaluación numérica de funciones.
* **[Nerdamer (v1.1.13)](https://nerdamer.com/) + Web Workers:** Cálculo algebraico e integración simbólica asíncrona.
* **[KaTeX (v0.16.9)](https://katex.org/):** Renderizado tipográfico de ecuaciones matemáticas en LaTeX.
* **HTML5 Canvas 2D API:** Graficación dinámica de la región plana y elementos representativos.
