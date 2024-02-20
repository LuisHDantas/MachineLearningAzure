# Instruções para criação do projeto

## Treinar modelo

1. No [Microsoft Azure](https://ml.azure.com/home?tid=6fd12631-a171-4640-8518-72b574476054), acessar **ML Automatizado**.

2. Crie um novo ML Automatizado usando as seguintes configurações:
    - Configurações básicas:
        - Nome do trabalho: mslearn-bike-automl
        - Novo nome do experimento: mslearn-bike-rental
        - Descrição: Aprendizado automatizado de máquina para previsão de aluguel de bicicletas
        - Tags: nenhum

    - Tipo de tarefa e dados:
        - Selecione o tipo de tarefa: Regressão
        - Selecione o conjunto de dados: Crie um novo conjunto de dados com as seguintes configurações :
          - Tipo de dados:
            - Nome: aluguel de bicicletas
            - Descrição: Dados históricos de aluguel de bicicletas
            - Tipo: Tabular
          - Fonte de dados:
            - Selecionar De arquivos da web
          - URL da Web:
            - URL da Web: ```https://aka.ms/bike-rentals```
            - Ignore a validação de dados: não selecione
          - Configurações:
            - Formato de arquivo: Delimitado
            - Delimitador: Vírgula
            - Codificação: UTF-8
            - Cabeçalhos de colunas: Somente o primeiro arquivo possui cabeçalhos
            - Ignore linhas: Nenhum
            - O conjunto de dados contém dados de várias linhas: não selecione
          - Esquema:
            - Inclua todas as colunas que não sejam Caminho
            - Revise os tipos detectados automaticamente  
            
            Selecionar Criar. Após a criação do conjunto de dados, selecione o conjunto de dados **aluguéis de bicicleta** para continuar enviando o trabalho de ML automatizado.

    - Configurações de tarefas:
        - Tipo de tarefa: Regressão
        - Conjunto de dados: aluguel de bicicletas
        - Coluna alvo: Aluguéis (inteiro)
        - Configurações de configuração adicionais:
            - Métrica primária: Erro quadrado médio da raiz normalizado
            - Explique o melhor modelo: Não selecionado
            - Use todos os modelos suportados: Não selecionado.
            - Modelos permitidos: Selecione apenas RandomForest e LightGBM
        - Limites:
            - Testes máximos: 3
            - Max ensaios simultâneos: 3
            - Nós máximos: 3
            - Limite de pontuação métrica: 0,085
            - Tempo esgotado: 15
            - Tempo limite de iteração: 15
            - Habilite a rescisão antecipada: Selecionado
        - Validação e teste:
            - Tipo de validação: Divisão de validação de trem
            - Porcentagem de dados de validação: 10
            - Teste o conjunto de dados: Nenhum
    - Computar:
        - Selecione o tipo de computação: Sem servidor
        - Tipo de máquina virtual: CPU
        - Camada de máquina virtual: Dedicado
        - Tamanho da máquina virtual: Standard_DS3_V2 *
        - Número de instâncias: 1
    
3. Envie o trabalho de treinamento. Começa automaticamente.

4. Aguarde o trabalho terminar.

## Análise do melhor modelo

1. No Visão geral guia do trabalho automatizado de aprendizado de máquina, observe o melhor resumo do modelo.

2. Selecione o texto em __Nome do algoritmo__ para o melhor modelo para ver seus detalhes.

3. Selecione __Métricas__  e selecione o **residuals** e **predicted_true** gráficos se ainda não estiverem selecionados.

## Implante e teste o modelo

1. Na aba __Modelo__ do melhor modelo treinado pelo seu trabalho automatizado de aprendizado de máquina, selecione **Implantar** e use o **Serviço da Web** para implantar o modelo com as seguintes configurações:

    - Nome: previsões de aluguel
    - Descrição: Prever aluguel de ciclo
    - Tipo de computação: Instância do contêiner do Azure
    - Habilite a autenticação: Selecionado

2. Aguarde o início da implantação

3. Aguarde o status __Implante__  mudar para **Sucesso**. Isso pode levar de 5 a 10 minutos.

## Teste o serviço implantado

1. No estúdio Azure Machine Learning, no menu à esquerda, selecione __Endpoints__ e abra o __predicted-rentals__ ponto de extremidade em tempo real.

2. Visualize a aba __Teste__.

3. No **Dados de entrada para testar o ponto final painel**, substitua o modelo JSON pelos seguintes dados de entrada:

```  
{
   "Entradas": { "dados": [{ "dia": 1,
         "mnth": 1,   
         "ano": 2022,
         "estação": 2,
         "feriado": 0,
         "dia da semana": 1,
         "dia de trabalho": 1,
         "weathersit": 2, 
         "temp": 0.3, 
         "atempo": 0.3,
         "hum": 0.3,
         "velocidade do vento": 0.3 }]} "Parameters globais": 1.0
}
```

4. Clique **Teste**.

5. Revise os resultados do teste, que incluem um número previsto de aluguéis com base nos recursos de entrada - semelhante a este:

```
{
   "Resultados": [ 356.6528307218089]
}

```
