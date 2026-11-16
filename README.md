# Fútbol femenino: rendimiento deportivo y valoraciones de FC24

¿Las futbolistas con mejores puntuaciones en EA Sports FC24 son también las que destacan en el terreno de juego? Este análisis exploratorio de datos (EDA) estudia esa relación teniendo en cuenta la posición de las jugadoras, la liga y su participación en los partidos.

El objetivo es buscar patrones y diferencias que ayuden a entender qué aspectos del rendimiento se relacionan con las valoraciones del videojuego. Los resultados se interpretan dentro del contexto de cada posición: marcar goles, crear ocasiones, recuperar balones y detener tiros representan tareas diferentes.

## Los datos

Se combinan dos fuentes:

- **Rendimiento deportivo de StatsBomb Open Data:** estadísticas de tiros, goles, goles esperados (xG), pases, asistencias, acciones defensivas y paradas. Se utiliza una tabla preparada previamente a partir de estos datos.
- **Valoraciones de EA Sports FC24:** puntuación general (`overall`) y atributos como tiro, pase, defensa y reflejos de las porteras. La copia utilizada corresponde al 22 de septiembre de 2023 y procede del dataset de Stefano Leone en Kaggle.

El estudio incluye Liga F, FA Women's Super League, Frauen Bundesliga y Serie A Women de la temporada 2023/2024, además de NWSL 2023.

La base deportiva contiene **1.739 identificadores de jugadoras y 1.791 registros**. Cada registro corresponde a una jugadora en un equipo, liga y temporada. Por eso, una futbolista que cambia de equipo puede aparecer en más de una fila.

Fuentes: [StatsBomb Open Data](https://github.com/statsbomb/open-data) y [dataset de FC24 en Kaggle](https://www.kaggle.com/datasets/stefanoleone992/ea-sports-fc-24-complete-player-dataset).

## Cómo se desarrolla el EDA

El trabajo completo está en **`EDA_V2.ipynb`**, organizado en siete apartados:

1. **Pregunta de investigación.** Se plantea qué relación puede existir entre las valoraciones de FC24 y el rendimiento observado.
2. **Fuentes y alcance.** Se describen las ligas, temporadas y características de los datos.
3. **Exploración inicial.** Se revisan tamaños, tipos de variables, valores ausentes y registros duplicados.
4. **Preparación del rendimiento.** Se seleccionan las jugadoras con participación y se calculan estadísticas por aparición, precisión de pase y conversión de tiros en goles.
5. **Unión con FC24.** Se buscan coincidencias de nombres y se añade evidencia de nacionalidad y liga para reducir errores al relacionar jugadoras de ambas fuentes.
6. **Construcción de la base de análisis.** Se comprueba cuántas jugadoras se han podido relacionar y cómo cambia esa proporción por liga y posición.
7. **Análisis y resultados.** Se representan las puntuaciones, se comparan con métricas deportivas y se estudia cómo cambian las relaciones según la liga o el mínimo de partidos jugados.

Para las comparaciones principales se utiliza un mínimo de **10 apariciones**. También se estudian mínimos de 1 y 5 para observar cuánto dependen los resultados de esta elección. 

### Qué se compara en cada posición

| Posición | Indicadores deportivos analizados |
| --- | --- |
| Delanteras | xG y tiros por aparición; porcentaje de tiros que terminan en gol, con un mínimo de 20 tiros |
| Centrocampistas | Pases que generan tiros y asistencias por aparición; precisión de pase |
| Defensas | Entradas e intercepciones favorables por aparición |
| Porteras | Paradas por aparición, interpretadas como volumen de trabajo |

El **xG**, o goles esperados, resume la probabilidad de gol de los tiros realizados. Las estadísticas **por aparición** dividen las acciones entre los partidos en los que se registra participación; no tienen en cuenta cuántos minutos jugó la futbolista en cada partido.

## Qué muestran los resultados

El procedimiento de unión admite **933 correspondencias provisionales** entre ambas fuentes. La base de análisis contiene 903 registros con participación y puntuación FC24; al exigir al menos 10 apariciones quedan **682 registros**.

Las asociaciones principales entre puntuación general y rendimiento son:

- **Delanteras:** la relación con el xG por aparición es positiva y más clara que la relación con el porcentaje de conversión. Los coeficientes de Spearman son 0,516 y 0,123, respectivamente.
- **Centrocampistas:** aparece una relación positiva con los pases que generan tiros por aparición (0,446).
- **Defensas:** la relación es muy débil con las entradas favorables por aparición (0,034) y algo mayor con las intercepciones favorables (0,203). Estas acciones solo recogen una parte de su trabajo defensivo.
- **Porteras:** la relación con las paradas por aparición es negativa (−0,340). Una portera puede realizar más paradas porque su equipo recibe más tiros; este resultado no permite concluir que tenga peor rendimiento.

La correlación de **Spearman** mide si las jugadoras mejor situadas en una variable también tienden a estarlo en la otra. Sus valores van de −1 a 1: cerca de 1 indica una relación en el mismo sentido; cerca de −1, en sentido contrario; y cerca de 0, poca relación entre las ordenaciones. Una correlación no demuestra que una variable cause la otra.

Además, se señalan **43 casos para estudiar con más detalle**, en los que una puntuación alta coincide con un valor bajo de una métrica deportiva, o al contrario. Son puntos de partida para investigar el contexto de las jugadoras, no una clasificación de futbolistas sobrevaloradas o infravaloradas.

Los gráficos, tamaños de cada grupo y resultados por liga se encuentran en el apartado 7 del notebook.

## Qué contiene la carpeta

```text
EDA_FC24_entrega/
├── EDA_V2.ipynb        # Desarrollo del análisis, tablas y gráficos
├── README.md           # Guía del proyecto
├── data/
│   ├── originales/     # Cinco CSV de entrada
│   └── procesados/     # Doce tablas generadas por el análisis
└── figuras/            # Once gráficos en formato PNG
```

En `data/originales/` se incluyen las estadísticas deportivas, los nombres de las jugadoras, el informe de posiciones, los datos FC24 y una tabla previa de posibles coincidencias que se utiliza como comprobación.

En `data/procesados/` se guardan la base completa y la base de análisis, las tablas de coincidencias y casos pendientes, la proporción de registros enlazados por liga y posición, las correlaciones y los resultados al cambiar el mínimo de apariciones. Estas tablas permiten seguir las decisiones del análisis y explorar sus resultados.
