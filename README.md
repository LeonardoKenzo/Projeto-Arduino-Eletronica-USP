# Projeto: Controle de Videogame com Arduino usando Bananas como Botões 🍌🎮
## 📋 Instruções
Neste projeto, iremos criar um controle de videogame utilizando bananas como botões condutivos. A montagem será feita com Arduino e usaremos o princípio da capacitância corporal para detectar toques nos frutos.

O sistema irá simular pressionamento de teclas via comunicação serial ou interface USB, permitindo controlar jogos simples no computador.

## 🧰 Tabela de Componentes Utilizados
|Quantidade|Nome do Componente  |Valor  |Modelo	/ Especificações         |
|----------|--------------------|-------|--------------------------------|
|1         |Arduino Uno         |R$30,00|                                |
|5         |Bananas             |R$20,00|Condutor natural de eletricidade|
|14        |Cabos               |R$10,00|Tipo jumper Macho x jacaré      |
|5         |Resistores          |R$1,00 |10MΩ                            |
|1         |Cabo USB            |R$2,00 |Comunicação Arduino ↔ PC        |

Software Arduino IDE	-	Para programação do Arduino

## 📝 Descrição dos Componentes
* **Arduino Uno:** Responsável por ler os toques nas bananas e enviar os sinais ao computador.

* **Bananas:** Funcionam como sensores táteis condutivos, detectando quando são tocadas pelos jogadores.

* **Cabos Jumper:** Fazem a ligação entre os pinos digitais do Arduino e as bananas.

* **Protoboard:** Para organizar o circuito de forma limpa.

* **Cabo USB:** Faz a alimentação e a comunicação serial entre o Arduino e o computador.

* **Arduino IDE:** Software usado para carregar o código no Arduino.

## 📈 Circuito do Projeto
#### 📷 Esquema de Ligação:
<img src="">

📷 Vídeo do Projeto: [Link Vídeo](https://youtube.com/shorts/01CHxVGYxkw?feature=shared)

## 🧮 Lógica de Funcionamento / Código
Este projeto utiliza um Arduino Uno para transformar cinco entradas analógicas (A0 a A4) em botões sensíveis ao toque, conectados a objetos condutores como bananas. Cada pino é configurado com resistor pull-up interno, permitindo detectar quando o usuário toca em uma banana conectada ao pino, fechando o circuito com o GND. O Arduino monitora continuamente o estado de cada entrada e envia, via porta serial, mensagens indicando quando um botão é pressionado ("COMANDO ON") ou liberado ("COMANDO OFF"). Essas mensagens são interpretadas por um programa Python no computador, que simula teclas do teclado (como W, A, S, D e espaço), permitindo controlar jogos ou aplicativos com toques físicos nos objetos conectados.

#### 📷 Código Arduíno:
```c
// Definição dos pinos
const int pinoCima     = A0;
const int pinoBaixo    = A1;
const int pinoEsquerda = A2;
const int pinoDireita  = A3;
const int pinoPulo     = A4;

// Estados atuais e anteriores
bool estadoCima, estadoBaixo, estadoEsquerda, estadoDireita, estadoPulo;
bool anteriorCima, anteriorBaixo, anteriorEsquerda, anteriorDireita, anteriorPulo;

void setup() {
  Serial.begin(9600);

  // Configura os pinos como entrada com pull-up interno
  pinMode(pinoCima,     INPUT);
  pinMode(pinoBaixo,    INPUT);
  pinMode(pinoEsquerda, INPUT);
  pinMode(pinoDireita,  INPUT);
  pinMode(pinoPulo,     INPUT);

  // Inicializa os estados anteriores como HIGH (sem toque)
  anteriorCima     = HIGH;
  anteriorBaixo    = HIGH;
  anteriorEsquerda = HIGH;
  anteriorDireita  = HIGH;
  anteriorPulo     = HIGH;
}

void loop() {
  // Leitura dos estados atuais
  estadoCima     = digitalRead(pinoCima);
  estadoBaixo    = digitalRead(pinoBaixo);
  estadoEsquerda = digitalRead(pinoEsquerda);
  estadoDireita  = digitalRead(pinoDireita);
  estadoPulo     = digitalRead(pinoPulo);

  // Verifica mudanças e envia comandos
  if (estadoCima != anteriorCima) {
    Serial.println(estadoCima == LOW ? "CIMA ON" : "CIMA OFF");
    anteriorCima = estadoCima;
  }

  if (estadoBaixo != anteriorBaixo) {
    Serial.println(estadoBaixo == LOW ? "BAIXO ON" : "BAIXO OFF");
    anteriorBaixo = estadoBaixo;
  }

  if (estadoEsquerda != anteriorEsquerda) {
    Serial.println(estadoEsquerda == LOW ? "ESQUERDA ON" : "ESQUERDA OFF");
    anteriorEsquerda = estadoEsquerda;
  }

  if (estadoDireita != anteriorDireita) {
    Serial.println(estadoDireita == LOW ? "DIREITA ON" : "DIREITA OFF");
    anteriorDireita = estadoDireita;
  }

  if (estadoPulo != anteriorPulo) {
    Serial.println(estadoPulo == LOW ? "PULAR ON" : "PULAR OFF");
    anteriorPulo = estadoPulo;
  }

  delay(10); // Pequeno atraso para estabilidade
}
```
#### 📷 Código Python traduzindo para o teclado:
```py
import serial
import keyboard
import time

# Porta serial (troque COM5 pela sua porta correta)
porta_serial = 'COM5'
baud_rate = 9600

try:
    arduino = serial.Serial(porta_serial, baud_rate, timeout=1)
    time.sleep(2)  # Espera o Arduino reiniciar
    print(f"Conectado na porta {porta_serial}")
except:
    print(f"Não foi possível abrir a porta {porta_serial}")
    exit()

# Mapeia comandos para teclas
comando_para_tecla = {
    'CIMA': 'w',
    'BAIXO': 's',
    'ESQUERDA': 'a',
    'DIREITA': 'd',
    'PULAR': 'space'
}

# Guarda o estado atual de cada tecla (ativa/desativada)
teclas_ativas = {comando: False for comando in comando_para_tecla}

print("Aguardando comandos...")

try:
    while True:
        if arduino.in_waiting:
            linha = arduino.readline().decode(errors='ignore').strip()

            if linha.startswith("CIMA") or linha.startswith("BAIXO") or \
               linha.startswith("ESQUERDA") or linha.startswith("DIREITA") or \
               linha.startswith("PULAR"):

                partes = linha.split()

                if len(partes) == 2:
                    comando, estado = partes[0], partes[1]

                    if comando in comando_para_tecla:
                        tecla = comando_para_tecla[comando]

                        if estado == "ON" and not teclas_ativas[comando]:
                            keyboard.press(tecla)
                            teclas_ativas[comando] = True
                            print(f"Tecla pressionada: {tecla}")

                        elif estado == "OFF" and teclas_ativas[comando]:
                            keyboard.release(tecla)
                            teclas_ativas[comando] = False
                            print(f"Tecla liberada: {tecla}")

except KeyboardInterrupt:
    print("\nEncerrando programa...")
    # Garante que todas as teclas sejam soltas ao sair
    for comando, ativo in teclas_ativas.items():
        if ativo:
            keyboard.release(comando_para_tecla[comando])
    arduino.close()

```

## 👨‍🎓 Alunos Responsáveis
Leonardo Kenzo Tanaka [Github: [LeonardoKenzo](https://github.com/LeonardoKenzo)]

Gustavo de Faria Fernandes [Github: [Gustavo-Fernandes04](https://github.com/Gustavo-Fernandes04)]

Pedro Teidi de Sá Yamacita [Github: [pedroYamacita](https://github.com/pedroYamacita)]

Enzo Ferreira de Castro Lima
