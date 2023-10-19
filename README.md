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

É provável que seu projeto usará botões de pressão como entrada de dados. 

Por exemplo, pressione botão A para mover as opções do menu, pressione B para selecionar uma opção do menu, etc.

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m4-semana2a/blob/main/imgs/botoes.png">
   <img alt="Boucing" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m4-semana2a/blob/main/imgs/botoes.png)">
</picture>

E ao pressionar o botão, microscopicamente há várias pressionadas devido às imperfeições das chapinhas metálicas de contatos internos, gerando o sinal que se nota na imagem a seguir.

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m4-semana2a/blob/main/imgs/boucing.png">
   <img alt="Boucing" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m4-semana2a/blob/main/imgs/boucing.png)">
</picture>

**ESSE EFEITO CHAMA-SE BOUNCING (REPIQUE)**


Nota-se que o **nível alto** do sinal foi gerado mais de uma vez, e isso confunde o programa que possui um loop rápido de leitura única, resultando na detecção involutária de várias pressionada.

O prejuízo é que o seu programa vai assumir que você pressionou mais de uma vez e vai avançar o processo sem você querer. Veja a imagem a seguir que mostra, através de uma tela de um osciloscópio, o comportamento de um botão sendo pressionado com bouncing.

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m4-semana2a/blob/main/imgs/boucing_oscilas.png">
   <img alt="Bouncing Osciloscópio" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m4-semana2a/blob/main/imgs/boucing_oscilas.png)">
</picture>


### Pergunta: Como evitar esse problema? Por meio de 2 formas.

### (1) Usando hardware, por meio de um componente eletrônico chamado Capacitor

### (2) Usando software, por meio de uma rotina usando a função ```milis()```



### (1) Usando hardware, por meio de um componente eletrônico chamado Capacitor

O capacitor é um componente eletrônico bem antigo e super importante. A função dele será **alisar** os repiques microscópicos do botão.

A figura a seguir mostra a carinha do capacitor.

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m4-semana2a/blob/main/imgs/capacitores.png">
   <img alt="Capacitores" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m4-semana2a/blob/main/imgs/capacitores.png)">
</picture>

O capacitor possui 2 placas internas condutoras e separadas por um **dielétrico**, conforme mostra a figura a seguir.

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m4-semana2a/blob/main/imgs/capacitores_interna.jpg">
   <img alt="Capacitor Interno" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m4-semana2a/blob/main/imgs/capacitores_interna.jpg)">
</picture>

Um **dielétrico** é um material isolante. Mas devido à alta quantidade de cargas numa mesma área, esse material isolante não segura a pressão das cargas, e o resultado é a fluidez de tais cargas. Esse fluxo de cargas dá-se o nome de **corrente elétrica**.

Portanto, a função do capacitor é basicamente transferir a variação de cargas elétricas de uma placa para a outra, e no processo dessa transferência ocorre um alisamento da variação. 

Reforçando: **precisa haver a variação da carga ao longo de um intervalo de tempo para que a corrente elétrica circule dentro do capacitor**.

Portanto, só repetindo: a quantidade de carga elétricas numa dada área é o mesmo que **tensão elétrica** e a **corrente elétrica** é a movimentação dessas cargas num dado instante de tempo.

O capacitor possui basicamente 2 aplicações:

1) Filtragem de sinais alternados ou pulsantes.
2) Armazenamento de cargas elétricas.
 
## Voltando ao seu projeto novamente

Recomenda-se adotar a aplicação (1) no seu projeto para o seu código-fonte ser mais simples e ao mesmo tempo, contornar o efeito indesejado do bouncing. 

A aplicação (2) pode ser uma alternativa quando você deseja simplificar seu hardware, já que seu código-fonte também pode resolver o bouncing.

Hoje veremos as duas aplicações.


# Anti-bouncing usando Hardware

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

# Anti-bouncing usando Software

```
int led = 4;          //declara a variável led igual a 4
int entrada = 7;      //declara a variável entrada igual a 7
int aux = 1;          //declara a variável aux igual a 1
int cont = 0;         //declara a variável cont igual a 0
bool estado = LOW;    //variável de estado do botão inicia com nível baixo
unsigned long time;   //variável time tipo long para a função millis()

void setup() {      //estrutura básica de qualquer arduino, chamada void setup()
   pinMode(led,OUTPUT);      //especifica que o pino led é do tipo saída
   pinMode(entrada,INPUT);   //especifica que a variável entrada é do tipo entrada
   Serial.begin(9600);      //inicia a comunicação serial para vc imprimir dados nela usando Serial.print()
   time = millis();         //grava o valor do contador interno chamado milis na variável time
}

void loop() {            //estrutura básica de qualquer arduino, chamada void loop()
  
   do{                  //faça ... enquanto ...
      estado_anterior = digitalRead(entrada);   //faz uma leitura digital no pino entrada e guarda em estado_anterior
      delay(50);                                 //aguarda 50 milisegundos
      estado = digitalRead(entrada);            // novamente faz uma leitura do pino entrada
   }while(estado_anterior != estado);  //aqui definimos um intervalo de tempo de segurança usando a função delay()
                                       //para sair do loop é necessário identificar uma constancia no estado do botão, ele precisa ser o mesmo durante 50 ms
   
   if (estado==HIGH && aux==1){    //testa se estado é nível alto E aux é igual a 1
      aux = 0;                     //grava 0 em aux
      cont++;                      //incrementa em 1 a variável cont
      digitalWrite(led,HIGH);      //aciona-se um LED com nível alto
   }
   
   if (estado==LOW && aux==0){     //testa se estado é nível baixo e aux é igual a 0
      aux = 1;                     // grava 1 em aux
      cont++;                      //incrementa em 1 a variável cont
      digitalWrite(led,LOW);       //desliga o LED com nível baixo
   }
   
      if (millis()-time>=500){     //imprime o número de variações a cada 0,5 segundo
         time=millis();            //guarda o novo ponto de início para a próxima análise
         Serial.print("variações = ");   //imprime o texto entre aspas duplcas
         Serial.print(cont);             //imprime conteúdo da variável cont
         Serial.print("\n");             //puxa uma linha
         cont = 0;                       //grava 0 na variável cont
      }
   }
   ```

# Comportamento Gráfico do C


colocar dados do ppt já pronto

## Quando fecha a chave

## Quando abre a chave

## Atuadores




# Prática com circuito RC

## Circuito TinkerCad

## Ambiente WokWi

Como compartilhar o projeto da forma correta?
Como fazer simulações



