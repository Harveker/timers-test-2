# STM32 Blackpill: Controle Dinâmico de Timer com LEDs

Este projeto demonstra como controlar dinamicamente o período de um timer de hardware (TIM4) em uma placa STM32 Blackpill (STM32F4xx). A velocidade de um contador regressivo, exibido em 5 LEDs, é ajustada em tempo real através de dois botões de pressão.

## 🚀 Funcionalidades

* **Controle de Velocidade em Tempo Real:** Aumente ou diminua a frequência de uma tarefa periódica usando botões.
* **Uso de Interrupção de Timer:** O TIM4 é configurado para gerar uma interrupção periódica, garantindo que a lógica do contador seja executada em intervalos de tempo precisos.
* **Entrada de Usuário:** Leitura de dois botões para interação com o microcontrolador.
* **Feedback Visual:**
    * Um conjunto de 5 LEDs exibe um padrão de contagem regressiva.
    * O LED integrado da placa (PC13) pisca para indicar que o sistema está em execução ("heartbeat").
* **Implementação com HAL:** O código utiliza a biblioteca STM32 HAL, tornando-o mais portável e fácil de entender.

## 🔧 Hardware Necessário

* Placa de desenvolvimento STM32 Blackpill (STM32F401, STM32F411, ou similar).
* 5x LEDs (da cor de sua preferência).
* 5x Resistores de limitação de corrente para os LEDs (ex: 220Ω ou 330Ω).
* 2x Botões de pressão (push buttons).
* 1x Protoboard (placa de ensaio).
* Fios Jumper para as conexões.
* Programador/Depurador ST-Link V2.

## 🔌 Montagem e Pinagem

Conecte os componentes à sua placa Blackpill conforme a tabela abaixo.

| Componente              | Pino na Blackpill | Descrição                                        |
| ----------------------- | ----------------- | -------------------------------------------------- |
| **LEDs** |                   | Conectar o catodo (-) ao GND                       |
| LED 1 (LSB)             | `PA3`             | Conectar o anodo (+) ao resistor e ao pino PA3     |
| LED 2                   | `PA4`             | Conectar o anodo (+) ao resistor e ao pino PA4     |
| LED 3                   | `PA5`             | Conectar o anodo (+) ao resistor e ao pino PA5     |
| LED 4                   | `PA6`             | Conectar o anodo (+) ao resistor e ao pino PA6     |
| LED 5 (MSB)             | `PA7`             | Conectar o anodo (+) ao resistor e ao pino PA7     |
| **Botões** |                   | Conectar um terminal ao GND                        |
| Botão 1 (Deixar lento)  | `PB12`            | Conectar o outro terminal ao pino PB12             |
| Botão 2 (Deixar rápido) | `PB13`            | Conectar o outro terminal ao pino PB13             |
| **LED da Placa** | `PC13`            | Já está integrado na placa (usado como heartbeat)  |

***Observação:** Os botões são configurados com pull-down interno por software, mas para maior estabilidade, recomenda-se o uso de resistores de pull-up ou pull-down externos.*

## 💡 Como Funciona o Software

### 1. Timer (TIM4)
O timer 4 é a peça central do projeto. Ele é configurado no modo "Base Interrupt", o que significa que ele conta até um valor definido e então gera uma interrupção.
* **Prescaler:** Divide o clock principal do sistema para criar um "tick" de contagem mais lento. No código, está configurado para `9600-1`.
* **Período (ARR - Auto-Reload Register):** É o valor até o qual o timer conta. Quando o contador atinge este valor, ele é reiniciado para 0 e a interrupção `HAL_TIM_PeriodElapsedCallback` é acionada. **É este valor que alteramos dinamicamente.**

### 2. Controle Dinâmico do Período
No loop principal (`while(1)`), o código verifica continuamente se um dos botões foi pressionado.
* Ao pressionar o **Botão 1 (PB12)**, o valor da variável `addCounter` é incrementado.
* Ao pressionar o **Botão 2 (PB13)**, o valor da variável `addCounter` é decrementado.

A linha `TIM4->ARR = addCounter;` escreve o novo valor diretamente no registrador de período do timer, alterando a frequência da interrupção em tempo real.
A linha `TIM4->EGR = TIM_EGR_UG;` força manualmente a ocorrência de um "Evento de Atualização" (Update Event) no Timer 4.

### 3. Contador Regressivo (Lógica dos LEDs)
Dentro da função de interrupção `HAL_TIM_PeriodElapsedCallback`, a seguinte lógica é executada a cada "estouro" do timer:
1.  A variável `leds` é decrementada.
2.  Uma máscara de bits (`& 0x1F`) garante que o valor permaneça entre 0 e 31 (para 5 LEDs).
3.  O valor da variável `leds` é então usado para acender/apagar os 5 LEDs conectados nas portas PA3 a PA7, criando o efeito visual do contador.

## 🛠️ Como Compilar e Usar

1.  **Clone ou baixe** este projeto.
2.  **Abra** o projeto no STM32CubeIDE.
3.  **Compile** o código (o atalho geralmente é `Ctrl + B`).
4.  **Conecte** a Blackpill ao computador através do programador ST-Link.
5.  **Faça o upload** do firmware para a placa (atalho `F11` para depurar/executar).
6.  **Pressione os botões** para ver a velocidade da contagem nos LEDs mudar. O botão em PB12 deixará a contagem mais lenta, e o botão em PB13 a deixará mais rápida.
