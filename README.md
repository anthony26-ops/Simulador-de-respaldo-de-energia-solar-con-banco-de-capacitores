# Simulador RC Solar

**Simulador web de un sistema de respaldo de energía solar con banco de capacitores (circuito RC).**
Proyecto final de Física II — Universidad Mariano Gálvez de Guatemala — Inga. Diana Ruiz Cifuentes.

> **Demo en línea:** `https://USUARIO.github.io/NOMBRE-DEL-REPOSITORIO/`
> (reemplace `USUARIO` y `NOMBRE-DEL-REPOSITORIO` por los suyos una vez activado GitHub Pages; ver la sección *Publicar en GitHub Pages*).

---

## 1. ¿Qué es?

En muchas comunidades rurales de Guatemala los cortes de energía eléctrica son frecuentes y cortos (de 5 a 45 minutos). Este proyecto estudia una alternativa de bajo costo a las baterías químicas: **un panel solar pequeño que carga un banco de capacitores durante el día y, cuando ocurre un corte, entrega esa energía a una carga crítica** (por ejemplo, un LED de emergencia).

El simulador permite modelar ese circuito y ver, segundo a segundo, cómo cambian:

- el **voltaje** del banco de capacitores,
- la **corriente** que circula por el circuito,
- la **energía** almacenada,

y calcular la **autonomía**: cuánto tiempo puede mantener encendida la carga durante un corte.

Es una **página web de un solo archivo** (`index.html`). No requiere instalación, servidor, base de datos ni conexión a internet: todo el cálculo ocurre en el navegador de quien la abre.

## 2. Cómo usarlo

**Opción A — desde el navegador (con el enlace de GitHub Pages).** Abrir el enlace de la demo. No hay que instalar nada.

**Opción B — desde el archivo.** Descargar `index.html` y abrirlo con doble clic (Chrome, Edge, Firefox, Safari o Brave). Funciona sin internet.

Una vez abierta la página:

1. Elegir un **escenario de ejemplo** (botones de la izquierda) o cambiar los valores del circuito. El botón activo se resalta en azul y la línea «Escenario actual» describe lo que se está viendo. Esto solo **prepara** el escenario.
2. Pulsar **«Iniciar simulación»** (botón azul, siempre visible arriba del panel izquierdo). El escenario **corre segundo a segundo**: las curvas se dibujan de izquierda a derecha.
3. Leer los **cuatro indicadores** de la parte superior: capacitancia equivalente, constante de tiempo, energía al iniciar el corte y autonomía útil.
4. Observar las **tres gráficas**: voltaje, corriente y energía en el tiempo. La franja azul clara es el corte eléctrico.
5. *(Opcional)* Si se construyó el circuito real, escribir las mediciones de laboratorio en el cuadro «Sus mediciones» para compararlas con la simulación.

**Barra de reproducción.** Sobre los indicadores hay una barra para controlar la animación: *Pausar / Continuar*, *Ver resultado final* (salta al final), *Velocidad* (lenta, normal, rápida o muy rápida) y un reloj que indica el segundo simulado y la fase (carga o corte). Si se cambia algún valor después de ejecutar, aparece un aviso amarillo y los resultados se atenúan hasta volver a pulsar «Iniciar simulación».

**Vista del circuito (el foco).** Entre la barra de reproducción y los indicadores hay un dibujo animado del circuito: el panel solar, el banco de capacitores (una celda por capacitor, con un nivel que sube y baja con el voltaje) y un **foco** que representa la luz de emergencia. El foco solo enciende durante el corte y mientras el voltaje del banco supere el mínimo; brilla más cuanto más voltaje tiene y se apaga cuando baja del mínimo. Los puntos que se mueven por los cables representan la corriente. Sirve para *ver* el efecto de cada cambio: por ejemplo, con el escenario «Banco de 5 F» el foco nunca enciende, y con «Carga completa» brilla y dura mucho más.

**Ayuda con «?».** Junto a cada control, cada grupo y cada cuadro de resultados hay un pequeño botón «?». Al pulsarlo se despliega una explicación de **qué es ese dato y qué modifica si se sube o si se baja** (por ejemplo, qué pasa al subir o bajar el voltaje del panel, o al aumentar o disminuir la cantidad de capacitores).

**Panel con desplazamiento propio.** El panel izquierdo tiene su propia barra de desplazamiento: al recorrer los controles, los resultados de la derecha no se mueven.

## 3. Qué controla cada dato

| Dato | ¿A qué se refiere? | ¿Qué función tiene en el programa? |
|---|---|---|
| **Voltaje del panel (Vs)** | El voltaje que entrega el panel solar mientras hay sol | Es el voltaje al que tiende el banco al cargarse. Aparece en la ecuación de carga. |
| **Resistencia de carga (R)** | La resistencia entre el panel y el banco | Limita la corriente al cargar; define la constante de tiempo de carga τ = R·Ceq. |
| **Resistencia de la carga (RL)** | La resistencia equivalente del LED que se alimenta | Define cuánto consume el LED durante el corte; determina la velocidad de descarga τ = RL·Ceq. |
| **Voltaje mínimo útil** | El voltaje debajo del cual el LED deja de funcionar | Es el umbral con el que se calcula la autonomía. |
| **Capacitancia de cada uno (C)** | La capacidad de cada capacitor, en faradios | Junto con la cantidad y la conexión define la capacitancia equivalente. |
| **Cantidad de capacitores (n)** | Cuántos capacitores iguales forman el banco (1 a 10) | Define la capacitancia equivalente Ceq. |
| **Conexión** | Cómo se conectan entre sí: paralelo, serie o mixto | Paralelo: Ceq = n·C. Serie: Ceq = C/n. Mixto: Ceq = 4·C/n (n par). |
| **Duración simulada** | Hasta qué segundo se calcula | Define el eje horizontal de las gráficas. |
| **Inicio del corte** | El segundo en que se va la luz | Equivale al tiempo que el panel tuvo para cargar el banco. |
| **Fin del corte** | El segundo en que vuelve la luz | Si es menor que la duración, el panel vuelve a cargar el banco. |
| **Método numérico** | Cómo se resuelve la ecuación: Runge-Kutta 4 o Euler | No cambia la física; RK4 es más preciso. |
| **Paso de cálculo** | Cada cuántos segundos se calcula un punto | Menor paso = más precisión y más lentitud. |

## 4. Los cuatro indicadores

| Indicador | Qué indica | Cómo se calcula |
|---|---|---|
| **Capacitancia equivalente** | La capacidad total del banco | Según cantidad y conexión |
| **Constante de tiempo (τ)** | La «velocidad» del circuito; el banco se llena en unas 5τ | τ = R·Ceq (carga) y RL·Ceq (descarga) |
| **Energía al iniciar el corte** | Cuánta energía tiene guardada el banco cuando se va la luz | E = ½·Ceq·Vc² |
| **Autonomía útil** | Cuánto tiempo alimenta la carga durante el corte | Tiempo con Vc mayor o igual al voltaje mínimo |

La autonomía puede mostrar etiquetas: *«Limitada por el fin del corte simulado»* (el voltaje nunca bajó del mínimo dentro de lo simulado; la autonomía real es mayor y se muestra como «teórica») o *«No alcanza el voltaje mínimo»* (el banco no llegó al mínimo antes del corte: autonomía 0 s).

## 5. Cómo leer las gráficas

| Forma de la curva | Qué significa |
|---|---|
| **Voltaje creciente** | El banco se está cargando |
| **Voltaje creciente que se aplana** | El banco se acerca a Vs: casi está lleno |
| **Voltaje decreciente** | El banco se descarga alimentando la carga |
| **Corriente decreciente** | Cada vez hay menos diferencia de voltaje que empuje la corriente |
| **Salto brusco de la corriente** | Cambio de fase (inicio o fin del corte): cambia la resistencia del circuito |
| **Energía creciente (forma de S suave)** | El banco acumula energía; crece más lento al principio porque E depende de Vc² |
| **Energía decreciente** | La carga consume la energía guardada |
| **Curva plana** | Estado casi estable: banco lleno o descargado |

El manual de usuario (PDF) explica cada gráfica con más detalle.

## 6. Escenarios de ejemplo incluidos

Once botones en el panel izquierdo cargan situaciones ya preparadas (después se pulsa «Iniciar simulación»):

| Botón | Qué cambia respecto al Base | Qué se obtiene |
|---|---|---|
| **Base** | Panel de 5 V, R = 220 Ω, RL = 1000 Ω, 3 capacitores de 1 F en paralelo, corte de 400 s a 900 s | Ceq = 3 F; 2.27 V y 7.75 J al corte; autonomía teórica 699 s |
| **Un capacitor** | Un solo capacitor de 1 F | Se carga más rápido: 4.19 V y 8.77 J; autonomía teórica 845 s |
| **Banco de 5 F** | 5 capacitores en paralelo | Con solo 400 s de carga llega a 1.52 V: autonomía 0 s (el foco no enciende) |
| **Carga completa** | Duración 7000 s; corte de 3300 s a 7000 s | El banco se llena (4.97 V, 37 J); autonomía 3045 s (50.7 min) |
| **Conexión en serie** | 3 capacitores en serie | Ceq = 0.333 F: se llena (4.98 V) pero guarda solo 4.13 J; autonomía 339 s |
| **Conexión mixta** | 4 capacitores: 2 ramas en paralelo de 2 en serie | Ceq = 1 F: se comporta como un solo capacitor de 1 F |
| **Panel de 3 V** | Voltaje del panel de 3 V | Llega a 1.36 V, por debajo del mínimo de 1.8 V: el foco nunca enciende |
| **Panel de 9 V** | Voltaje del panel de 9 V | Llega a 4.09 V y guarda 25.10 J: el foco brilla mucho más; autonomía teórica 2463 s (verifique el voltaje máximo de cada capacitor real) |
| **Carga lenta** | Resistencia de carga de 1000 Ω | Llega a solo 0.62 V (0.58 J): el foco no enciende |
| **Luz de bajo consumo** | RL = 5000 Ω; duración 4000 s | Con la misma carga previa la luz dura 3497 s (58.3 min) en lugar de 699 s |
| **Corte corto** | Corte de 400 s a 600 s; duración 1200 s | El foco enciende, se apaga al volver la energía y el panel recarga el banco |

Para ver el efecto de **subir o bajar el voltaje del panel**, compare «Panel de 3 V», «Base» y «Panel de 9 V»; para ver el efecto de **la cantidad de capacitores**, compare «Un capacitor», «Base» y «Banco de 5 F».

Conclusión de diseño que muestran los escenarios: **un banco más grande guarda más energía, pero tarda más en cargarse**; con poco tiempo de sol conviene un banco pequeño, con mucho tiempo uno grande.

## 7. Modelo físico

Durante el día el panel carga el banco a través de R; durante el corte el banco se descarga a través de RL. Cada fase se describe con una ecuación diferencial de primer orden:

```
Carga:      R·Ceq · dVc/dt + Vc = Vs        →   Vc(t) = Vs · (1 − e^(−t/(R·Ceq)))
Descarga:   RL·Ceq · dVc/dt + Vc = 0        →   Vc(t) = V0 · e^(−t/(RL·Ceq))
Energía:    E = ½ · Ceq · Vc²
Autonomía:  t = RL · Ceq · ln(V0 / Vmin)
```

El programa resuelve las ecuaciones numéricamente (Runge-Kutta de 4.º orden o Euler) y dibuja, sobre la curva simulada, la **solución exacta** en línea discontinua; si coinciden, el método numérico es correcto.

Leyes de Física II aplicadas: Ley de Ohm (V = I·R), Leyes de Kirchhoff (ley de voltajes en la malla), relación de un capacitor (Q = C·V, I = C·dV/dt), capacitores en serie y en paralelo, y energía almacenada en un capacitor.

## 8. Comparar con mediciones de laboratorio (opcional)

Si el grupo construyó el circuito, puede escribir en el cuadro **«Sus mediciones»** una medición por línea: **tiempo en segundos**, un espacio o coma, y **voltaje en voltios** (con punto decimal):

```
0, 0.02
20, 0.55
40, 0.98
```

Los puntos aparecen en la gráfica de voltaje y se calcula el error relativo medio, el máximo y el error absoluto medio. **No se sube ningún archivo**; también se pueden pegar dos columnas copiadas de Excel. El botón «Ver un ejemplo» escribe datos **sintéticos** solo para mostrar cómo funciona.

## 9. Publicar en GitHub Pages

Solo se necesita un archivo: **`index.html`** (este repositorio ya lo incluye).

1. Crear un repositorio nuevo en GitHub (público) y subir `index.html` y `README.md` a la raíz.
2. Ir a **Settings → Pages**.
3. En *Build and deployment → Source* elegir **Deploy from a branch**; en *Branch*, **main** y la carpeta **/ (root)**; pulsar **Save**.
4. Esperar uno o dos minutos: GitHub muestra el enlace del sitio (`https://USUARIO.github.io/NOMBRE-DEL-REPOSITORIO/`).

El archivo debe llamarse exactamente `index.html` y estar en la raíz del repositorio.

## 10. Estructura del repositorio

```
├── index.html    ← el simulador completo (HTML + CSS + JavaScript en un solo archivo)
└── README.md     ← este documento
```

Si además se desea guardar todo el proyecto del curso, pueden agregarse (no son necesarios para que funcione): el informe técnico (`.docx`), el manual de usuario (`.pdf`), las diapositivas y la carpeta `simulador/` con una versión equivalente en Python (motor de cálculo, línea de comandos e interfaz Streamlit).

## 11. Limitaciones del modelo

- El panel solar se modela como una **fuente de voltaje constante**; no se considera la curva real del panel ni la variación de la luz durante el día.
- No se incluyen la resistencia interna del capacitor (ESR), las corrientes de fuga ni la temperatura.
- El LED se representa con una **resistencia constante** (un LED real no es lineal).
- Los capacitores del banco se suponen **idénticos y balanceados**.
- Se simula **un único corte** por ejecución.
- Con la energía que almacenan los capacitores (unos 12.5 J por faradio a 5 V) el sistema sirve para **cargas de muy baja potencia** (iluminación LED); no alcanza para cargar un teléfono.

## 12. Compatibilidad

Navegadores recientes: Chrome, Edge, Firefox, Safari y Brave, en Windows, macOS, Linux, Android e iOS. Se adapta a pantallas de celular y tiene tema claro y oscuro automático.

## 13. Integrantes

- [Nombre 1] — [Carné]
- [Nombre 2] — [Carné]
- [Nombre 3] — [Carné]
- [Nombre 4] — [Carné]
- [Nombre 5] — [Carné]

Catedrática: Inga. Diana Ruiz Cifuentes — Física II, 2026.
