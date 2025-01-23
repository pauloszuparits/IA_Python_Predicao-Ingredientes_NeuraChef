# **NeuraChef**

**NeuraChef** é uma rede neural projetada para prever o consumo diário de ingredientes em um restaurante com base em variáveis como:  
- **Estação do ano**  
- **Clima**  
- **Dia da semana**  
- **Dia do mês**  
- **Valor do ingrediente**  
- **Consumo nas receitas**  

---

## **Classe Ingredientes**

A classe `Ingredientes` herda de `Dataset`, uma classe do PyTorch para criação e manipulação de datasets.  
Ela implementa os seguintes métodos:  

1. **`__init__`**:  
   - Construtor que recebe o caminho para um arquivo CSV.  
   - Utiliza o pandas para carregar e processar os dados.  

2. **`__getitem__`**:  
   - Separa o dataset em amostras e rótulos.  
   - As colunas de 1 a 9 são os dados de entrada, enquanto a última coluna é o rótulo.  

3. **`__len__`**:  
   - Retorna o tamanho do conjunto de dados.  

---

## **Classe MLP (Multilayer Perceptron)**

A classe `MLP` herda de `nn.Module` do PyTorch e define a arquitetura da rede neural.  

### **Métodos**  
1. **`__init__`**:  
   - Construtor que recebe:  
     - `input_size`: Tamanho da entrada.  
     - `hidden_size`: Tamanho da camada oculta.  
     - `output_size`: Tamanho da saída.  
   - Define a arquitetura utilizando `nn.Sequential` com as seguintes camadas:  

     **Extração de características**  
     - Camada de entrada:  
       - `nn.Linear`: Camada linear que conecta a entrada à camada oculta.  
       - `nn.ReLU`: Função de ativação que permite aprendizado não linear.  
     - Camada oculta:  
       - `nn.Linear`: Camada linear conectando a entrada e saída da camada oculta.  
       - `nn.ReLU`: Função de ativação.  

     **Classificação da rede**  
     - Camada de saída:  
       - `nn.Linear`: Camada linear conectando a camada oculta à saída.  
       - `nn.ReLU`: Função de ativação.  

2. **`forward`**:  
   - Define como os dados fluem pela rede, passando pelas camadas ocultas e de saída para gerar as previsões.  

---

## **Funções de treinamento, validação e precisão**

### **`train()`**  
Treina a rede utilizando um conjunto de dados.  
1. Coloca a rede em modo de treinamento (`net.train()`).  
2. Executa o **forward** (previsão), calcula o erro (loss) e realiza o **backpropagation** para ajustar os parâmetros.  

### **`validate()`**  
Valida o desempenho da rede.  
1. Coloca a rede em modo de validação (`net.eval()`).  
2. Realiza o **forward** para calcular o erro sem ajustar os parâmetros.  

### **`calculate_precision()`**  
Calcula o coeficiente de determinação (R²) para medir a precisão do modelo.  
1. Coloca a rede em modo de validação.  
2. Utiliza o `r2_score` do scikit-learn para calcular o R².  

---

## **Hiperparâmetros e leitura do dataset**

Os hiperparâmetros definidos são:  
- **`epoch_num`**: Número de épocas.  
- **`lr`**: Taxa de aprendizado.  
- **`weight_decay`**: Regularização para evitar overfitting.  
- **`num_workers`**: Número de threads do `DataLoader`.  
- **`batch_size`**: Número de amostras por iteração.  

O código verifica a disponibilidade de GPU para treinamento e solicita o upload do dataset, exibindo uma amostra dos dados.  

---

## **Instanciação da rede e criação dos DataLoaders**

1. Os dados são embaralhados e convertidos em CSV para uso pela classe `Ingredientes`.  
2. A rede é configurada com os tamanhos de entrada, camada oculta e saída.  
3. São definidos:  
   - **Critério**: `L1Loss`.  
   - **Otimizador**: `Adam`.  

---

## **Fase de treinamento**

Nesta etapa, o treinamento é executado em um laço que:  
1. Itera sobre o número de épocas.  
2. Chama as funções de treino, validação e precisão.  
3. Armazena os resultados para visualização.  

---

## **Visualização dos resultados**

### **Gráfico de Convergência**  
Compara a loss ao longo das épocas utilizando o `matplotlib`.  

![Gráfico de Convergência](https://github.com/user-attachments/assets/95469803-72ac-47dd-b77e-afb5f5edfe15)  

### **Gráfico Precisão R² x Época**  
Mostra a precisão R² ao longo das épocas.  

![Gráfico de Precisão](https://github.com/user-attachments/assets/6a7df4dd-b33d-4eeb-b802-a2455aa83473)  

---

## **Testes e predições**

Após o upload do arquivo de testes:  
1. Os dados são embaralhados.  
2. É gerado um gráfico de dispersão por ingrediente.  
3. Uma tabela exibe os valores reais x preditos.  

![Gráfico de Dispersão](https://github.com/user-attachments/assets/df5aaa94-6766-4e5a-874b-bda08e05baba)  

---

## **Documentação Completa**  
Para mais detalhes, acesse a [documentação completa](https://drive.google.com/file/d/1xMOqGyVcU8jc0zITuC3ey1dykuBtBIYG/view?usp=sharing).  
