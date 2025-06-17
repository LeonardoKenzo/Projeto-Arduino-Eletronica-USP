# Projeto: Controle de Videogame com Arduino usando Bananas como Botões 🍌🎮
## 📋 Instruções
Neste projeto, iremos criar um controle de videogame utilizando bananas como botões condutivos. A montagem será feita com Arduino e usaremos o princípio da capacitância corporal para detectar toques nos frutos.

O sistema irá simular pressionamento de teclas via comunicação serial ou interface USB, permitindo controlar jogos simples no computador.

## 🧰 Tabela de Componentes Utilizados
Quantidade	Nome do Componente	Valor / Modelo	Especificações
1	Arduino Uno (ou similar)	-	Microcontrolador ATmega328
6	Bananas	-	Condutor natural de eletricidade
6	Cabos tipo jumper	Macho x jacaré	Para ligação Arduino → bananas
1	Resistores pull-down	10kΩ (opcional)	Para melhorar leitura digital
1	Protoboard (opcional)	-	Para melhor organização das ligações
1	Cabo USB	-	Comunicação Arduino ↔ PC
Software	Arduino IDE	-	Para programação do Arduino

## 📝 Descrição dos Componentes
* **Arduino Uno:** Responsável por ler os toques nas bananas e enviar os sinais ao computador.

* **Bananas:** Funcionam como sensores táteis condutivos, detectando quando são tocadas pelos jogadores.

* **Cabos Jumper:** Fazem a ligação entre os pinos digitais do Arduino e as bananas.

* **Resistores de 10kΩ (opcional):** Usados como pull-down para garantir leitura correta nos pinos digitais.

* **Protoboard (opcional):** Para organizar o circuito de forma limpa.

* **Cabo USB:** Faz a alimentação e a comunicação serial entre o Arduino e o computador.

* **Arduino IDE:** Software usado para carregar o código no Arduino.

## 📈 Circuito do Projeto
#### 📷 Esquema de Ligação:

#### 📷 Montagem Física:

## 🧮 Lógica de Funcionamento / Código
O Arduino ficará monitorando o estado dos pinos digitais. Quando um toque for detectado (por meio da variação de tensão causada pelo contato humano com a banana), o Arduino enviará um comando ao computador (via comunicação Serial ou, se estiver usando um Arduino Leonardo, via emulação de teclado).

#### 📷 Trecho de Código ou Fluxograma:
(Inclua aqui uma imagem ou trecho do código Arduino usado)

#### ✏️ Cálculos / Considerações Técnicas
(Se houver, inclua cálculos sobre limiar de detecção, divisão de tensão, ou esquemas de pull-down/pull-up)

Exemplo de configuração simples de leitura:

## 👨‍🎓 Alunos Responsáveis
Leonardo Kenzo Tanaka [Github: [LeonardoKenzo](https://github.com/LeonardoKenzo)]

Gustavo de Faria Fernandes [Github: [Gustavo-Fernandes04](https://github.com/Gustavo-Fernandes04)]

Pedro Teidi de Sá Yamacita [Github: [pedroYamacita](https://github.com/pedroYamacita)]

Enzo Ferreira de Castro Lima
