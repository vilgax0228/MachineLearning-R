# Intro to Machine Learning with R: Rigorous Mathematical Analysis

## Capítulo 1. O que são Modelos?
*Modelos* são úteis porque diferentemente de dashboards, que oferecem uma imagem estática doque os dados mostram atualmente (ou em um dado momento), modelos conseguem ir além e ajudam a entender o futuro.

Foram incontáveis os dashboards que já vi ou construí que simplesmente diz "é assim que muitos ativos estão no momento". Ou, "este é o nosso indicador chave para hoje".

Um relatório é uma entidade estática que não oferece uma intuição sobre como as coisas evoluem com o tempo.
```R
# Mostra o que um relatório se parece
> op <- par(mar = c(10, 4, 4, 2) + 0.1)  # margin formatting
> barplot(mtcars$mpg, names.arg = row.names(mtcars), las=2, ylab="Fuel
        Efficiency in Miles per Gallon")
```
![rplot mtcars](https://github.com/user-attachments/assets/e07f09af-94d3-4484-9bcf-f1248bd5c4e2)
A figura representa um gráfico do dataset *mtcars*.

A figura mostra a quantidade de carros plotada de acordo com sua eficiência de combustível em milhas por galão. Esse relatório não nos dá nenhum poder preditivo.

Ver como a eficiência dos carros está distribuída é interessante, mas como podemos relacionar isso com outros aspectos dos dados e, além disso, fazer previsões a partir disso?

<table><tr><td>Um modelo é qualquer tipo de função que possui algum poder preditivo.</td></tr></table>

