# Regresión lineal y sus supuestos

## La idea en una frase

La regresión lineal resume la relación entre una respuesta continua y una o más variables con la recta (o el plano) que deja los errores al cuadrado lo más pequeños posible.

## El modelo

y = β₀ + β₁x + ε. El término ε recoge todo lo que la recta no explica. Mínimos cuadrados ordinarios (OLS) elige β₀ y β₁ para minimizar la suma de residuos al cuadrado, SSE = Σ(yᵢ − ŷᵢ)². En el caso simple hay fórmula cerrada:

- β₁ = cov(x, y) / var(x)
- β₀ = ȳ − β₁·x̄

La recta siempre pasa por el punto de las medias (x̄, ȳ).

## Qué mide el R²

R² = 1 − SSE / SST, donde SST = Σ(yᵢ − ȳ)² es el error que cometería quien predijera siempre la media. R² es entonces la fracción de esa variabilidad que la recta logra explicar.

- R² = 1: todos los puntos caen sobre la recta.
- R² = 0: la recta no mejora a la media.
- R² negativo: solo aparece cuando la recta no fue ajustada por OLS (o se evalúa en datos nuevos) y resulta peor que predecir la media. En el Laboratorio lo verás al mover la recta lejos de los puntos.

## Supuestos

1. **Linealidad.** La media de y cambia de forma lineal con x.
2. **Independencia de los errores.** En series de tiempo suele fallar: los errores vecinos se parecen.
3. **Homocedasticidad.** La dispersión de los errores es la misma en todo el rango de x.
4. **Normalidad de los errores.** No hace falta para estimar los coeficientes; hace falta para que los intervalos y las pruebas sean exactos en muestras pequeñas.
5. **Sin multicolinealidad perfecta** y variables explicativas no correlacionadas con el error.

Con los supuestos 1 a 3 y 5, OLS es el estimador lineal insesgado de menor varianza (teorema de Gauss-Markov).

## Curiosidades

- "Regresión" viene de Francis Galton, que observó que los hijos de padres muy altos tendían a ser menos altos que ellos: regresaban hacia la media.
- El cuarteto de Anscombe son cuatro conjuntos de datos con la misma recta y prácticamente el mismo R², y cuatro gráficos por completo distintos. Por eso se grafica antes de confiar en un número.
- R² nunca baja al agregar una variable, aunque sea ruido. Para comparar modelos se usa el R² ajustado o un criterio de validación.
- Un solo valor atípico con mucha palanca puede girar la recta entera. Actívalo en el Laboratorio.
- Un R² alto no prueba causalidad ni que el modelo sea correcto; un R² bajo no impide que un coeficiente sea útil.

## En minería y cemento

Relacionar el consumo específico de energía de un molino con el tonelaje y la dureza del mineral, o la resistencia del cemento con la finura. Los residuos graficados contra el tiempo delatan derivas del proceso.

## En banca

Modelos de pérdida dado el incumplimiento, proyección de saldos y, sobre todo, como línea base contra la que se justifica cualquier modelo más complejo.

## Pruébalo

En el Laboratorio, mueve el intercepto y la pendiente para acercar la recta a los puntos y mira subir el R². Después sube el ruido, agrega el valor atípico y compara con el ajuste de mínimos cuadrados.
