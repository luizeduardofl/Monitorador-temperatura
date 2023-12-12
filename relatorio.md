# Monitorador de temperatura com LED RGB e Buzzer 

## Introdução

* Membros: Gustavo Quezado, Luiz Eduardo e Sergio Alfredo.
* Monitorar a temperatura de um sistema é uma tarefa importante em várias áreas da engenharia.
Além de um termômetro digital, é necessário que haja outros indicadores para facilitar a compreensão da informação,
pois nem sempre um _display_ estará disponível. Por isso, criou-se um monitorador de temperatura com dois atuadores
principais, um sonoro (buzzer) e outro visual (LED RGB). O sensor de temperatura usado foi o DHT22. Em suma, O LED RGB
muda de cor com o aumento da temperatura e o buzzer emite um alerta sonoro caso a temperatura esteja muito alta.
Um botão foi implementado no projeto para finalizar o alerta sonoro.
* Podemos dizer que a principal parte do projeto, além do sensor DHT22, foi o uso de atuadores, bastante abordados no curso.
Eles recebem comandos vindos do código e retornam com uma ação que pode ser percebida por um observador.

## Componentes

* ESP32 da Espressif;
* Sensor de temperatura DHT22;
* Protoboard;
* LED RGB;
* Buzzer;
* Botão.

## Metodologia / Solução Proposta  

* Para indicar a temperatura de uma forma intuitiva, usou-se um LED RGB com o seguinte comportamento: 
  * Se a temperatura estiver fria, ilumine azul;
  * Se estiver morna, ilumine verde;
  * Se estiver quente, ilumine vermelho.  
* Quanto ao buzzer, seu funcionamento é simples: emite um alarme sonoro enquanto a temperatura estiver alta.
Perceba que o LED RGB estará vermelho quando o buzzer estiver neste estado.
* Para o alarme ser desligado, basta apertar o botão.
* Devido à quantidade de componentes usados, uma _protoboard_ foi necessária.

## Imagem 
![Imagem do projeto final](/ProjetoFinal.jpg)

## Código
* Primeiro importamos as bibliotecas e inicializamos os atuadores e o sensor:
```
import machine, neopixel
import time, dht
sensor = dht.DHT22(machine.Pin(14))
button = machine.Pin(12, machine.Pin.IN, machine.Pin.PULL_UP)
buzzer = machine.PWM(machine.Pin(22, machine.Pin.OUT))
buzzer.init(freq=1, duty=0)
n = 1
p = 23
np = neopixel.NeoPixel(machine.Pin(p), n)
```
* Em seguinda temos o laço `while`, a parte principal do código. O sensor está medindo a temperatura e exibindo-a
com uma casa decimal:
```
while True:
    sensor.measure()
    temp = sensor.temperature()
    print('Temperatura: %3.1f C' %temp)
```
* Se a temperatura estiver menor do que 20°C, o LED RGB acenderá na cor azul:
```
if temp < 20:   
        np[0] = (0, 0, 255)
        np.write()
```
* De forma análoga, o LED RGB acenderá na cor verde se a temperatura estiver entre 20 e 23°C:
```
 if temp >= 20 and temp <= 23:
        np[0] = (0,255,0)
        np.write()
        buzzer.duty(0)
```

* Por fim, no caso em que a temperatura está alta, o LED RGB acende na cor vemelha e o buzzer emite
o barulho por meio de `buzzer.freq(1500)` e `buzzer.duty(10)`.
```
 if temp > 23:
        np[0] = (255,0,0)
        np.write()
        buzzer.freq(1500)
        buzzer.duty(10)
```
* Perceba que, no código da cor verde, há `buzzer.duty(0)`. Isso foi feito para silenciar o buzzer
caso a temperatura deixe de ser quente, isto é, transite para o estado anterior do laço. Não é necessário implementar isso no código
da cor azul, pois uma mudança brusca de temperatura não ocorre no contexto desse projeto.

* Para o usuário encerrar o barulho e o programa, basta apertar o botão:
```
if not button.value():
        np[0] = (0,0,0)
        np.write()
        break;
    
buzzer.deinit()
```
* Veja o código completo [aqui](https://github.com/luizeduardofl/IoT-2023/blob/main/ProjetoFinal.py) 

## Experimentos 

* Para testar os três casos do laço `while`, realizamos três ações:

  * Colocamos o monitorador no ar-condicionado;
  * Colocamos o monitorador na temperatura da sala;
  * Sopramos ar quente da boca no monitorador.
   
* Ao colocar o monitorador diretamente no ar-condicionado, ele rapidamente esfria e emite a cor azul.
Na sala, a temperatura permanece na faixa de 20 a 23°C geralmente, o que faz o LED acender na cor verde.
Ao soprar o ar da nossa boca no monitorador, ele rapidamente emite o vermelho e apita o buzzer indefinidamente.
Só é possível interromper o barulho se o programa for encerrado ou se a temperatura detectada diminuir.
