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

## Vantagens dessa instrução para o seu projeto

É provável que seu projeto usará botões de pressão como entrada de dados. Exemplo, pressione botão A para mover as opções do menu, pressione B para selecionar uma opção do menu, etc.

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m4-semana2a/blob/main/imgs/botoes.png">
   <img alt="Boucing" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m4-semana2a/blob/main/imgs/botoes.png)">
</picture>

E ao pressionar o botão, microscopicamente há várias pressionadas devido às imnperfeições das chapinhas metálicas de contatos internos, gerando o sinal que se nota na imagem a seguir.

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m4-semana2a/blob/main/imgs/boucing.png">
   <img alt="Boucing" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m4-semana2a/blob/main/imgs/boucing.png)">
</picture>

**ESSE EFEITO CHAMA-SE BOUNCING (REPIQUE)**


Nota-se que o **nível alto** do sinal foi gerado mais de uma vez, e isso confunde o programa que possui um loop de leitura única e muito rápido, resultando na detecção involutária de mais de uma pressionada.

O prejuízo é que o seu programa vai assumir que você pressionou mais de uma vez e vai avançar o processo sem você querer. Veja a imagem a seguir que mostra, através de uma tela de um osciloscópio, o comportamento de um botão sendo pressionado.

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m4-semana2a/blob/main/imgs/boucing_oscilas.png">
   <img alt="Boucing Osciloscópio" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m4-semana2a/blob/main/imgs/boucing_oscilas.png)">
</picture>


### Pergunta: Como evitar esse problema? 

### Resposta: Usando um componente eletrônico chamado Capacitor

O capacitor é um componente eletrônico bem antigo e super importante. A função dele será **alisar** os ruídos.

A figura a seguir mostra a sua carinha:

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m4-semana2a/blob/main/imgs/capacitores.png">
   <img alt="Capacitores" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m4-semana2a/blob/main/imgs/capacitores.png)">
</picture>


O capacitor possui 2 placas condutores internamente separadas por um dielétrico, conforme mostra a figura a seguir:


<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m4-semana2a/blob/main/imgs/capacitores_interna.jpg">
   <img alt="Capacitor Interno" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m4-semana2a/blob/main/imgs/capacitores_interna.jpg)">
</picture>

Um dielétrico é um material isolante. Mas devido à quantidade de cargas numa memsa área (e isso dá-se o nom de **Volts**), esse material isolante não segura a pressão das cargas e deixa fluir. Esse fluxo de cargas dá-se o nome de **corrente elétrica**.

Portanto, a função do capacitor é basicamente transferir a variação de cargas elétricas de uma placa para a outra. Precisa haver a variação da carga ao longo de um intervalo de tempo para que o corrente elétrica circule dentro do capacitor.

Portanto, só repetindo: a quantidade de carga elétricas numa dada área é o mesmo que tensão elétrica e a corrente elétrica é a movimentação dessas cargas num dado instante de tempo.

O capacitor possui basicamente 2 funções:

1) Filtragem de sinais alternados ou pulsantes
2) Armazenamento de cargas elétricas que se traduz em volts armazenados, que por sua vez, se traduz em tempo de carga e descaga de cargas, que se traduz em temporizador.
 
## Voltando ao seu projeto novamente

A função (1) é muito provável que você use no seu projeto ou o seu código-fonte pode ser tornar muito complexo para contornar o efeito indesejado do bouing. 

A função (2) é pouco provável que você use no seu projeto, pois seu código-fonte pode resolver isso usando um timer interno do microcontrolador.


# Como montar seu circuito?

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m4-semana2a/blob/main/imgs/anti_boucing.png">
   <img alt="Anti Bouncing" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m4-semana2a/blob/main/imgs/anti_boucing.png)">
</picture>


```
unsigned long time; // váriavel para receber valores da função milli()
int cont = 0; // o contador de variações
int entrada = 7;
bool estado = LOW; // variável de estado do botão
int aux = 1;


void setup() {
  Serial.begin(9600);
  pinMode(led,OUTPUT);
  pinMode(entrada,INPUT);
  time = millis(); //iniciamos a variável com o primeiro valor do "cronometro"
}
void loop() {

  estado = digitalRead(entrada);
  if (estado==HIGH && aux==1){
    cont++;
    aux = 0;
  }
  if (estado==LOW && aux==0){
    cont++;
    aux = 1;
  }
  if (millis()-time>=500){ // imprime o número de variações a cada 0,5 segundo
    time=millis(); // guarda o novo ponto de inicio para a proxima analise
    Serial.print("variações = ");
    Serial.print(cont);
    Serial.print("\n");
    cont = 0;
   }    
}
```

# Comportamento Gráfico do C

## Quando fecha a chave

## Quando abre a chave

## Atuadores




# Prática com circuito RC

## Circuito TinkerCad

## Ambiente WokWi

Como compartilhar o projeto da forma correta?
Como fazer simulações



