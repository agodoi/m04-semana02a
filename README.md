# Sensores e Atuadores Básicos

## Objetivos

Nesse encontro, teremos a programação básica com circuito RC, entradas por meio de botão e saídas informacionais simples (Leds). 

Os alunos devem realizar a coleta de dados de um experimento montado pelo professor sobre um circuito RC para filtragem e preencher um gráfico. 

O relatório deve ser produzido em sala e entregue ao professor no final da instrução.

## Assuntos relacionados

- Fundamentos de controle e automação
- Introdução, histórico e aplicações de sistemas embarcados
- Programação de microcontroladores
- Simulação de sistemas embarcados
- Uso de ferramentas computacionais para projeto, simulação e análise

## Vantagens dessa instrução no seu projeto

Seu projeto pode precisar de botões de pressão como entrada de dados. E ao pressionar o botão, microscopicamente há várias pressionadas devido às imnperfeições das chapinhas metálicas de contato, gerando o sinal que se nota na imagem a seguir.

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m4-semana2a/blob/main/imgs/boucing.png">
   <img alt="Boucing" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m4-semana2a/blob/main/imgs/boucing.png)">
</picture>


Nota-se que o **nível alto** do sinal foi gerado mais de uma vez, e isso confunde o programa que possui um loop de leitura muito rápido, resultado na detecção involutária de mais de uma pressionada.

O prejuízo é que o seu programa vai pensar que você pressionou mais de uma vez e vai avançar o processo sem você querer. Veja a imagem a seguir que mostrada pela tela de um osciloscópio que comprova o comportamento de um botão sendo pressionado.

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m4-semana2a/blob/main/imgs/boucing_oscilas.png">
   <img alt="Boucing" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m4-semana2a/blob/main/imgs/boucing_oscilas.png)">
</picture>


### Pergunta: Como evitar esse problema? 

### Resposta: Usando um componente eletrônico chamado Capacitor

O capacitor é um componente eletrônico bem antigo e super importante. A figura a seguir mostra como é sua carinha:

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m4-semana2a/blob/main/imgs/capacitores.png">
   <img alt="Boucing" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m4-semana2a/blob/main/imgs/capacitores.png)">
</picture>


O capacitor possui 2 placas condutores internamente separadas por um dielétrico.

Um dielétrico é um material isolante.

A função do capacitor é basicamente transferir uma variação de cargas elétricas de uma placa para a outra. Precisa haver a variação da carga ao longo de um intervalo de tempo qualquer para que o corrente elétrica circule dentro do capacitor.

Portanto, a quantidade de carga elétricas numa dada área é o mesmo que tensão elétrica e a corrente elétrica é a movimentação dessas cargas num dado instante de tempo.

O capacitor possui basicamente 2 funções:

- Armazenamento de cargas elétricas que se traduz em volts armazenados;
- Filtragem de sinais alternados ou pulsantes
