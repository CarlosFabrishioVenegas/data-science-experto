# Regresión logística

## La idea en una frase

La regresión logística estima la probabilidad de que ocurra un evento binario (paga o no paga, falla o no falla) pasando una combinación lineal de las variables por una curva en forma de S.

## Cómo funciona

Primero se calcula un puntaje lineal: z = β₀ + β₁x₁ + … + βₖxₖ. Ese puntaje puede valer cualquier número, así que se comprime al intervalo entre 0 y 1 con la función sigmoide:

p = 1 / (1 + e^(−z))

Si se despeja z se obtiene la forma que da nombre al modelo: ln(p / (1 − p)) = z. El lado izquierdo es el logit, el logaritmo de las chances (odds). El modelo es lineal en el logit, no en la probabilidad.

De ahí sale la interpretación de los coeficientes: cuando x₁ sube una unidad, las chances se multiplican por e^β₁. Si β₁ = 0,7, las chances se duplican aproximadamente, porque e^0,7 ≈ 2. El efecto sobre la probabilidad no es constante: es grande cerca de p = 0,5 y casi nulo en los extremos.

## Cómo se ajusta

No se minimizan errores al cuadrado. Se buscan los coeficientes que hacen más verosímiles los datos observados, maximizando la log-verosimilitud:

LL = Σ [ y·ln(p) + (1 − y)·ln(1 − p) ]

No existe una fórmula cerrada como en mínimos cuadrados. Se resuelve de forma iterativa, con Newton-Raphson (también llamado IRLS) o con descenso de gradiente. El gradiente tiene una forma muy simple: Xᵀ(y − p), el error de probabilidad ponderado por cada variable. La función es convexa, así que si existe un óptimo finito, es único.

El negativo de la log-verosimilitud promedio es la log-loss o entropía cruzada, la misma función de pérdida que usan las redes neuronales para clasificar.

## Supuestos

1. **Respuesta binaria.** Hay extensiones para varias clases (multinomial) y para clases ordenadas.
2. **Observaciones independientes.** Datos repetidos del mismo cliente o del mismo equipo rompen este supuesto.
3. **Linealidad en el logit.** Cada variable continua debe relacionarse de forma lineal con el logaritmo de las chances. Es el supuesto que más se incumple y el que menos se revisa.
4. **Sin multicolinealidad fuerte.** Variables muy correlacionadas inflan la varianza de los coeficientes y los vuelven inestables.
5. **Sin separación perfecta.** Si una variable separa por completo las dos clases, la verosimilitud no tiene máximo finito y los coeficientes crecen sin límite.
6. **Muestra suficiente.** Importa el número de eventos de la clase minoritaria, no el total de filas. La guía habitual de unos diez eventos por variable es una referencia, no una ley.

Lo que **no** supone: normalidad de las variables, homocedasticidad, ni una relación lineal entre x y la probabilidad.

## Por qué no tiene R²

El R² clásico mide qué fracción de la varianza explica el modelo, y eso exige residuos continuos. Aquí la respuesta es 0 o 1. Por eso se usan sustitutos:

- **Pseudo-R² de McFadden:** 1 − LL / LL₀, donde LL₀ es la log-verosimilitud de un modelo que solo predice la tasa promedio. Sus valores son mucho más bajos que los de un R² lineal: McFadden señaló que entre 0,2 y 0,4 ya indica un ajuste muy bueno.
- **Log-loss:** cuanto más baja, mejor; castiga con dureza las predicciones seguras y equivocadas.
- **AUC, KS y Gini:** miden capacidad de ordenar, no calibración.
- **Curva de calibración:** compara la probabilidad predicha con la frecuencia observada.

## Curiosidades

- Se llama regresión, pero casi siempre se usa para clasificar. Lo que "regresa" es una probabilidad.
- Una neurona con activación sigmoide es exactamente una regresión logística. Una red neuronal para clasificación binaria termina en una.
- La curva logística nació en demografía: Pierre-François Verhulst la propuso en el siglo XIX para describir poblaciones que crecen hasta un tope.
- La frontera de decisión es una recta (o un hiperplano), aunque la curva de probabilidad sea una S.
- El umbral de 0,5 no es sagrado. Si un falso negativo cuesta diez veces más que un falso positivo, el umbral debe bajar.
- Con clases perfectamente separables, el modelo "más seguro de sí mismo" es también el que no converge.

## En minería y cemento

Probabilidad de falla de un equipo en el próximo turno a partir de vibración y temperatura; probabilidad de que un lote salga fuera de especificación según variables del proceso. La ventaja frente a modelos más complejos es que el signo y el tamaño de cada coeficiente se pueden discutir con el ingeniero de planta.

## En banca

Es la base histórica del credit scoring: estima la probabilidad de incumplimiento y sus coeficientes se traducen en puntos de una scorecard. Su interpretabilidad y su estabilidad la mantienen vigente como modelo de referencia frente al cual se comparan los modelos de boosting.

## Errores comunes

- Leer β como "cambio en la probabilidad". Es cambio en el logit.
- Evaluar con exactitud cuando las clases están desbalanceadas: un modelo que siempre dice "no" acierta el 95 % si el evento ocurre el 5 % de las veces.
- Regularizar sin escalar las variables: la penalización castiga más a las de escala pequeña.
- Confundir buena discriminación con buena calibración. Un modelo puede ordenar bien y dar probabilidades infladas.

## Pruébalo

En el Laboratorio, mueve β₀ y β₁ y observa cómo cambian la curva, la log-loss, el pseudo-R² y la matriz de confusión. Luego pulsa el ajuste por máxima verosimilitud y compara con tu intento manual.
