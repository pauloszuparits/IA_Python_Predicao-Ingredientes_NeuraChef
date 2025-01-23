# **NeuraChef**

Essa é uma rede neural de previsão de gasto de ingredientes por dia em um restaurante utilizando as seguintes **variaveis**
* Estação do ano
* Clima
* Dia da semana
* Dia do mês
* Valor do ingrediente
* Consumo nas receitas

---

## **Classe Ingredientes**

A classe Ingredientes herda da classe Dataset, uma classe do pytorch para criação de datasets.

Temos 3 métodos nessa classe, sendo o método __ init __ o construtor, que recebe um caminho para o csv que será criado o data set e com a classe do pandas lê o arquivo no caminho especificado.

O método __ getitem __ que separa o dataset em sample e label que seria o dado e o rótulo respectivamente, neste caso, os dados estão da coluna 1 até a coluna 9 do csv, enquanto o rótulo, está na ultima coluna do csv.

E por ultimo temos o método __ len __ que retorna o tamanho do conjunto de dados.

## **Classe MLP ( Multilayer Perceptron )**

A classe MLP herda da classe nn.Module que é uma classe do pytorch que auxilia na criação das redes neruais.

Temos 2 métodos nessa classe, sendo o método __ init __ o construtor que recebe o input_size (tamanho da entrada), o hidden_size (tamanho da camada escondida) e o out_size(tamanho da saida). Neste método é definida a arquitetura que será utilizada na rede neural. Primeiramente, é utilizado o nn.Sequential pra criar uma sequencia de camadas, sendo essas camadas:

### **Extração de caracteristicas**

**Camada de entrada**

*   nn.Linear -> uma camada linear que recebe a entrada e passa para a camada escondida
*   nn.ReLU -> uma função de ativação que faz com que a rede possa prever de forma não linear

**Camada escondida**

*   nn.Linear -> uma camada linear que recebe o tamanho da camada escondida e tem uma saida de mesmo tamanho para a camada de saída
*   nn.ReLU -> Outra função de ativação ReLU com a mesma funcionalidade

### **Classificação da rede**

**Camada de saída**

*   nn.Linear -> uma camada linear que recebe o tamanho da camada escondida e tem uma saida de 1 que é o tamanho da saida

*  nn.ReLU -> Outra função de ativação ReLU

O método forward ele serve para indicar como os dados devem fluir pela rede, neste caso, os dados de entrada passam pela camada escondida e são classificados, gerando assim um resultado na camada de saída.

# **Funções de treinamento, validação e medição de precisão**


### Função train()

A função train recebe um train_loader, com os dados de treino, uma rede e a época.
Essa função tem como objetivo treinar a rede.
Primeiramente a função coloca a rede em modo de treinamento através do net.train().
É utilizado um loop dentro da funcao que itera sobre o train_loader, dentro deste loop é feito o **forward**, que é basicamente usar a rede para prever um dado a partir de uma entrada, após essa predição é utilizado uma função de perda, que utiliza a predição + o rotulo para calcular o loss, e por fim essa loss é adicionada a uma lista com a loss da epoch.
Após o forward, temos o **backpropagation**, que utiliza de calculos de derivada para calcular os parametros mais adequados ao modelo que estamos tentando prever.
Por fim temos a atualizacao dos parametros calculados anteriormente utilizando o otimizador.


### Função validate()

A função validate recebe um test_loader com dados de teste do modelo, uma rede e uma época.
Essa função tem como objetivo validar o desempenho da rede e do treinamento do modelo.
Primeiramente a função coloca a rede em modo de validação a partir do net.eval().
É utilizado um loop dentro da função que itera sobre o test_loader e assim como a função train() é realizado o **forward** que utiliza a rede para prever o dado e calcula o loss a partir de uma função. Na funcao validation, diferente da função train, não temos o backpropagation.


### Função calculate_precision()

A função calculate_precision recebe um test_loader com dados de teste do modelo, uma rede.
O objetivo desta função é calcular a precisão do modelo através do calculo do coeficiente de determinação (R²), esse coeficiente varia de 0 a 1 podendo ter resultados negativos caso a rede esteja com resultados muito distantes do que foi testado.
Assim como na função validate, a rede é colocada em modo de validação. Após isso, é feito um iterando em test_loader e armazenando os dados de predição e teste.
Após isso, é utilizado a função r2_score do scikit-learn para calcular o coeficiente R2.


# **Hiperparâmetros, e leitura do dataset**

Hiperparametros são parametros definidos para treinar o modelo, sendo eles


*   **epoch_num** -> Número de vezes que o conjunto de função de treinamento irá ocorrer
*   **lr** -> Taxa de aprendizado é a taxa da qual o modelo irá ajustar os parametros no treinamento

*   **weight_decay** -> Regularização do modelo, ou Penalidade L2, evita que os pesos do modelo sejam ajustados com valores muito altos

*   **num_workers** -> Numero de threads do DataLoader

*   **batch_size** -> Quantidade de exemplos, ou no caso linhas, que serão utilizadas em cada iteração do treinamento


Após a definição dos hiperparametros, o código verifica se a GPU está disponível para uso, será importante usar a GPU para o treinamento do modelo.
E por fim, o código solicita o upload do DataSet e mostra uma pequena amostra do arquivo que foi feito o upload.


# **Instanciação da rede e criação dos DataLoaders**

Primeiramente, pra criação dos DataLoaders, é feito um embaralhamento dos dados, para que a rede seja treinada da forma mais adequada possível. Depois do embaralhamento dos dados, esses dados embaralhados são transformados em um CSV que será lido pela classe Ingrediente.

Assim, é definido o tamanho da entrada, o tamanho da camada escondida, e o tamanho da saída, e é criada a rede.

Após isso, é definido o **critério**, que no caso é o **L1Loss**, e o **otimizador**, que no caso é o **Adam**.


# Fase de treinamentos

Nesta parte do código se inicia os treinamentos, onde há um laço que irá iterar sobre o numero de épocas, chamando as funções de treino, validacao e calculo de precisão.
Os retornos são armazenados para composição de gráficos


# Gráfico de convergência

Foi utilizada a classe **matplotlib** para fazer um gráfico de convergencia, que compara a loss ao longo das épocas.


![image](https://github.com/user-attachments/assets/95469803-72ac-47dd-b77e-afb5f5edfe15)

# Gráfico Precisão R² x Epoch

Aqui temos um gráfico parecido com o anterior, porém, é mostrado a precisão R² ao longo das épocas.

![image](https://github.com/user-attachments/assets/6a7df4dd-b33d-4eeb-b802-a2455aa83473)


# Testes e predições

Após o upload do arquivo de testes, os dados são embaralhados e é feito um gráfico de disperção separado por ingrediente e uma tabela com o valor real x valor predito.

![image](https://github.com/user-attachments/assets/df5aaa94-6766-4e5a-874b-bda08e05baba)


# Documentação completa disponível em:
https://drive.google.com/file/d/1xMOqGyVcU8jc0zITuC3ey1dykuBtBIYG/view?usp=sharing


