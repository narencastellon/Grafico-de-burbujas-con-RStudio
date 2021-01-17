# Grafico-de-burbujas-con-RStudio

---
title: "Gráfico de burbujas animado"
author: "Naren Castellón"
date: "01/17/2021"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## **Gráfico de burbujas más básico con geom_point()**

Un diagrama de burbujas es un diagrama de dispersión donde se agrega una tercera dimensión: el valor de una variable numérica adicional se representa mediante el tamaño de los puntos. (fuente: data-to-viz ).

Con `ggplot2` , los gráficos de burbujas se construyen gracias a la función  `geom_point()`. Se deben proporcionar al menos tres variables para `aes()`: x, y y tamaño. La leyenda la construirá automáticamente ggplot2.

Aquí se representa la relación entre la esperanza de vida ( y) y el pib per cápita ( x) de los países del mundo. La población de cada país se representa mediante el tamaño del círculo.

```{r message=FALSE}
# Libraries
library(ggplot2)
library(dplyr)

# El conjunto de datos se proporciona en la biblioteca gapminder
library(gapminder)
data <- gapminder %>% filter(year=="2007") %>% dplyr::select(-year)

# Gráfico de Burbuja Básico
ggplot(data, aes(x=gdpPercap, y=lifeExp, size = pop)) +
    geom_point(alpha=0.7)
```

## **Controle el tamaño del círculo con scale_size()**

Lo primero que debemos mejorar en el gráfico anterior es el tamaño de la burbuja. `scale_size()` permite establecer el tamaño de los círculos más pequeños y más grandes usando el rangeargumento. Tenga en cuenta que puede personalizar el nombre de la leyenda con name.

**Nota :** los círculos a menudo se superponen. Para evitar tener círculos grandes en la parte superior del gráfico, primero debe reordenar su conjunto de datos, como en el código a continuación.

Tareas pendientes : brinde más detalles sobre cómo asignar una variable numérica al tamaño del círculo. Utilice de `scale_radius, scale_sizey scale_size_area`. 

```{r}
# Libraries
library(ggplot2)
library(dplyr)

# El conjunto de datos se proporciona en la biblioteca gapminder
library(gapminder)
data <- gapminder %>% filter(year=="2007") %>% dplyr::select(-year)

# Gráfico de Burbuja Básico
data %>%
  arrange(desc(pop)) %>%
  mutate(country = factor(country, country)) %>%
  ggplot(aes(x=gdpPercap, y=lifeExp, size = pop)) +
    geom_point(alpha=0.5) +
    scale_size(range = c(.1, 20), name="Población (M)")
```

## **Agrega una cuarta dimensión: color**

Si tiene una variable más en su conjunto de datos, ¿por qué no mostrarla usando el color del círculo? Aquí, el continente de cada país se usa para controlar el color del círculo:


```{r}
# Libraries
library(ggplot2)
library(dplyr)

# El conjunto de datos se proporciona en la biblioteca gapminder
library(gapminder)
data <- gapminder %>% filter(year=="2007") %>% dplyr::select(-year)

# Gráfico de Burbuja Básico
data %>%
  arrange(desc(pop)) %>%
  mutate(country = factor(country, country)) %>%
  ggplot(aes(x=gdpPercap, y=lifeExp, size=pop, color=continent)) +
    geom_point(alpha=0.5) +
    scale_size(range = c(.1, 15), name="Población (M)")
```


## **Hazlo bonito**

Algunas mejoras clásicas:

* uso del paquete `viridis` para una bonita paleta de colores
* uso de `theme_ipsum()`del paquete `hrbrthemes`
* títulos de eje personalizados con `xlab` y `ylab`
* añadir trazo al círculo: cambie shapea 21 y especifique color(trazo) y fill.

```{r message=FALSE, warning=FALSE}
# Libraries
library(ggplot2)
library(dplyr)
library(hrbrthemes)
library(viridis)

# Data viene de la libreria gapminder
library(gapminder)
data <- gapminder %>% filter(year=="2007") %>% dplyr::select(-year)

# Gráfico de Burbuja Básico
data %>%
  arrange(desc(pop)) %>%
  mutate(country = factor(country, country)) %>%
  ggplot(aes(x=gdpPercap, y=lifeExp, size=pop, fill=continent)) +
    geom_point(alpha=0.5, shape=21, color="black") +
    scale_size(range = c(.1, 20), name="Population (M)") +
    scale_fill_viridis(discrete=TRUE, guide=FALSE, option="A") +
    theme_ipsum() +
    theme(legend.position="bottom") +
    ylab("Expectativa de Vida") +
    xlab("Gdp per Capita") +
    theme(legend.position = "none")+ggtitle("Gráfico de Burbuja")
```
