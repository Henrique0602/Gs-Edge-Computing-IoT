# Monitor de Nível com LEDs e Buzzer

##  Descrição do Problema

Durante períodos de chuva intensa, o acúmulo de água em bueiros pode causar alagamentos e riscos para a população. Monitorar o nível de água  é essencial para emitir alertas em tempo real e prevenir acidentes.

Este projeto simula um sistema de monitoramento de nível de água usando um potenciômetro como sensor de entrada (representando a variação de altura da água), com saídas visuais e sonoras para alertar sobre o nível atingido.

---
## Visão Geral da Solução

Utilizando um Arduino UNO, o sistema interpreta os dados de um potenciômetro (como sensor de nível) e aciona LEDs e um buzzer conforme três faixas de alerta:

- **Nível Baixo (0-33%)**: LED verde aceso  
- **Nível Médio (34-66%)**: LED amarelo aceso  
- **Nível Alto (67-100%)**: LED vermelho aceso + buzzer sonoro  

As mensagens também são exibidas no monitor serial.

###  Componentes utilizados

- 1 Arduino UNO  
- 1 Potenciômetro (simulando o sensor de nível)  
- 3 LEDs (verde, amarelo e vermelho)  
- 3 resistores (220Ω)  
- 1 Buzzer  
- Jumpers e protoboard  

###  Ilustração do circuito

![image](https://github.com/user-attachments/assets/603f5860-427c-4050-8314-49ef29701fa6)


---

###  Simular no Tinkercad

1. Acesse o link abaixo para abrir o projeto:  
   🔗 [[Acessar no Tinkercad](https://www.tinkercad.com/things/5sN9M0tuMXQ-gs-1semestre)]

2. Cole o código que esta no fim da documentação. Para isso, clique no botão código e copie e cole o código na área.

3. Clique em **Start Simulation** e interaja com o potenciômetro.

---

##  Vídeo demonstrativo

Confira o vídeo explicando o funcionamento do sistema, incluindo a simulação:  
📺 [Assistir no YouTube](https://youtu.be/HOKtDr3tVdQ?si=AZICM33jBDFCj005)

---

##  Conclusão

Este projeto demonstra como é possível criar um sistema simples e eficiente para monitoramento de níveis usando sensores analógicos e atuadores visuais/sonoros com Arduino.
---

## 📄 Código Fonte

```cpp
// Definição dos pinos
const int PIN_POTENCIOMETRO = A0;
const int PIN_LED_VERDE     = 10;
const int PIN_LED_AMARELO   = 11;
const int PIN_LED_VERMELHO  = 12;
const int PIN_BUZZER        = 5;

void setup() {
  // Configura os pinos dos LEDs e do buzzer como saída
  pinMode(PIN_LED_VERDE, OUTPUT);
  pinMode(PIN_LED_AMARELO, OUTPUT);
  pinMode(PIN_LED_VERMELHO, OUTPUT);
  pinMode(PIN_BUZZER, OUTPUT);

  // Inicializa a comunicação serial para monitoramento
  Serial.begin(9600);
}

void loop() {
  // Lê o valor analógico do potenciômetro (0 a 1023)
  int leituraBruta = analogRead(PIN_POTENCIOMETRO);

  // Mapeia o valor da leitura para uma escala percentual (0 a 100)
  int nivel = map(leituraBruta, 0, 1023, 0, 100);

  // Exibe o nível no monitor serial
  Serial.print("Nível: ");
  Serial.println(nivel);

  // Avalia o nível e aciona os atuadores conforme a faixa
  if (nivel > 66) {
    // Nível alto - alerta vermelho com buzzer
    digitalWrite(PIN_LED_VERDE, LOW);
    digitalWrite(PIN_LED_AMARELO, LOW);
    digitalWrite(PIN_LED_VERMELHO, HIGH);
    digitalWrite(PIN_BUZZER, HIGH);
    Serial.println("ALERTA! Nível Alto.");

  } else if (nivel > 33) {
    // Nível médio - luz amarela
    digitalWrite(PIN_LED_VERDE, LOW);
    digitalWrite(PIN_LED_AMARELO, HIGH);
    digitalWrite(PIN_LED_VERMELHO, LOW);
    digitalWrite(PIN_BUZZER, LOW);
    Serial.println("Atenção! Nível Médio.");

  } else {
    // Nível baixo - luz verde
    digitalWrite(PIN_LED_VERDE, HIGH);
    digitalWrite(PIN_LED_AMARELO, LOW);
    digitalWrite(PIN_LED_VERMELHO, LOW);
    digitalWrite(PIN_BUZZER, LOW);
    Serial.println("Seguro. Nível Baixo.");
  }

  // Pequeno atraso para estabilidade da leitura
  delay(200);
}

