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
![Rplot01](https://github.com/user-attachments/assets/e9f78f9d-44a8-40e9-a96d-df781a646058)

A figura representa um gráfico do dataset *mtcars*.

A figura mostra a quantidade de carros plotada de acordo com sua eficiência de combustível em milhas por galão. Esse relatório não nos dá nenhum poder preditivo.

Ver como a eficiência dos carros está distribuída é legal, mas como podemos relacionar isso com outros aspectos dos dados e, além disso, fazer previsões a partir disso?

<table><tr><td>Um modelo é qualquer tipo de função que possui algum poder preditivo.</td></tr></table>

Então, como transformamos este relatório em algo mais útil? Como podemos preencher a lacuna entre relatórios e aprendizado de máquina?

Muitas das vezes, a resposta correta para isso é "mais dados!"  
Isso pode vir na forma de mais observações dos mesmos dados ou pela coleta de novos *tipos* de dados que podemos usar para comparação.

Vamos olhar para o dataset mtcars em mais detalhes:
```R
> head(mtcars)
                   mpg cyl disp  hp drat    wt  qsec vs am gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
```
Ao simplesmente chamar o objeto mtcars no R, podemos ver todos os tipos de colunas nos dados para escolher e construir um modelo de machine learning (ML).

No mundo do ML, as colunas de dados às vezes também são chamadas de características (*features*), como é mostrado abaixo:
```R
> pairs(mtcars[1:7], lower.panel=NULL)
```
![Rplot02](https://github.com/user-attachments/assets/8581c352-0cf2-4acb-9ee2-bda4ad294d95)

Um gráfico de pares (*pairs plot*) do conjunto de dados mtcars, focando nas primeiras sete linhas.

Cada caixa é um gráfico separado, em que a variável dependente é o texto na parte inferior da coluna e a variável independente é o texto no início da linha.

Alguns desses gráficos são mais interessantes para fins de tendência do que outros.  
Nenhum dos gráficos na linha *cyl*, por exemplo, parece se adaptar facilmente a modelagens de regressão simples.

Neste exemplo, estamos plotando algumas dessas features em relação a outras. As *colunas*, ou *features*, ou ainda, *variáveis* desses dados são definidas da seguinte maneira:

* mpg (miles per US gallon): milhas por galão;

* cyl (cylinders): número de cilindros no motor do carro;

* disp (displacement): a cilindrada (ou volume) do motor em polegadas cúbicas;

* hp (horsepower): a potência do motor em cavalos;

* drat: a relação do eixo traseiro do veículo;

* wt (weight): o peso do veículo em milhares de libras;

* qsec: o tempo do veículo em uma corrida de um quarto de milha;

* vs: a configuração dos cilindros do motor do veículo, onde "V" é para um motor em formato de V e "S" é para um design em linha reta.

* am: a transmissão do veículo, onde 0 representa uma transmissão automática e 1 representa uma transmissão manual.

* gear: o número de marchas na transmissão do veículo.

* carb: o número de carburadores utilizados pelo motor do veículo.

Você pode ler o gráfico no canto superior direito como "mpg em função do tempo de um quarto de milha", por exemplo.

Aqui, estamos principalmente interessados em algo que pareça ter algum tipo de relação quantificável.

Note que "mpg em função de cyl" parece muito diferente de "mpg em função de wt (peso)". Neste caso, focamos no último, como mostrado abaixo:

```R
> plot(y=mtcars$mpg, x=mtcars$wt, xlab="Peso do Veículo",
       ylab="Eficiência de Combustível em mpg")
```

![Rplot03](https://github.com/user-attachments/assets/c8a4457f-8166-414a-80f5-68833e27cd0c)

Esse gráfico é a base para traçar um linha de regressão através dos dados.

Agora temos um tipo de conjunto de dados mais interessante. Ainda temos nossa eficiência de combustível (primeiro gráfico), mas agora está plotada em relação ao peso dos respectivos carros em toneladas.

A partir desse formato de dados, podemos extrair uma linha de melhor ajuste para todos os pontos de dados e transformar esse gráfico em uma equação.

Usamos uma função no R para modelar o valor que nos interessa, chamado de *response* (resposta), em relação a outras features em nosso conjunto de dados:
```R
> mt.model <- lm(formula=mpg~wt, data=mtcars)
> coef(mt.model)[2]
       wt 
-5.344472
> coef(mt.model)[1]
(Intercept) 
   37.28513
```
Neste trecho de código, modelamos a eficiência de combustível do veículo (mpg) como uma função do peso do veículo (wt) e extraímos valores desse objeto de modelo para usar em uma equação que podemos escrever da seguinte forma:

<table><tr><td>Eficiência de Combustível = 5.344 * Peso do Veículo + 37.285</td></tr></table>

Agora, se quisermos saber qual é a eficiência de combustível de qualquer carro, não apenas daqueles no conjunto de dados, tudo o que precisaríamos fazer é inserir o peso dele, e obteremos um resultado.

*Esse é o benefício de um modelo.* Temos poder preditivo, dados algum tipo de entrada (ex. peso), que pode nos fornecer um valor para qualquer número que inserimos.

O modelo pode ter suas limitações, mas essa é uma maneira de expandir os dados além de um relatório estático, transformando-os em algo mais flexível e mais informativo.

Esse tipo específico de modelo de machine learning é chamado de *regressão linear*.

---
### Algoritmos vs Modelos: Qual a Diferença?

*Um algoritmo é um conjunto de etapas executadas em ordem*.

O algoritmo mais simples para regressão linear envolve colocar dois pontos em um gráfico e depois traçar uma linha entre eles. Você obtém as partes importantes da equação (inclinação e intercepto) ao tomar a diferença nas coordenadas desses pontos em relação a uma origem.

O algoritmo se torna mais complicado quando você tenta fazer o mesmo procedimento para mais de dois pontos, naturalmente.

Um modelo de ML, como regressão, clusterização ou redes neurais, depende do funcionamento de algoritmos para ajudar a operá-los antes de qualquer coisa. Eles fazem todo o trabalho pesado de multiplicar matrizes, otimizar resultados e gerar um número para usarmos.

Existem muitos tipos de modelos no R, que abrangem um ecossistema de ML de forma mais geral. Há três tipos principais de modelos: modelos de regressão, modelos de classificação e um mix de ambos.

A chamada de função para uma regressão linear simples no R pode ser escrita como: *lm(x ~ y), que significa, me dê o modelo linear para a variável y como uma função da característica x.

*lm()* é, por si só, uma função, mas também é um modelo linear. Ele chama uma série de algoritmos para encontrar os melhores valores que são então fornecidos como uma inclinação e um intercepto. Em seguida, usamos essas inclinações e interceptos para construir uma equação, que podemos usar para análises futuras.

### Limitações da Modelagem

* "All models are wrong, but some are useful." - Statistician George Box

Um modelo é uma representação simplificada da realidade.

Voltando ao nosso exemplo com o mtcars, as limitações do nosso modelo vêm dos dados, especificamente do tempo em que foram coletados e do número de pontos de dados.

Estamos fazendo suposições bastante ousadas sobre as eficiências de combustível dos carros hoje em dia se tentarmos inserir apenas o peso de um veículo moderno em um modelo que foi construído inteiramente a partir de carros fabricados na década de 1970.

Da mesma forma, há muitos poucos pontos de dados no conjunto para começar. trinta e dois pontos pontos de dados é bastante baixo para fazer afirmações definitivas sobre aeficiência de combustível de *qualquer* carro.

Quando George Box diz que todos os modelos estão errados, ele quer dizer que todos os modelos tèm algum erro atribuído a eles.

Modelos de regressão têm uma maneira específica de medir o erro chamada *coeficiente de determinação*, frequentemente referido como o valor "R-square". Esta é uma medida de quão bem os dados se ajustam à linha do modelo ajustado, com valores variando de 0 a 1.

### Estatística e Computação na Modelagem








