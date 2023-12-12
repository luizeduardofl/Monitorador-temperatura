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

## Códigos 

* Mostrar trechos de códigos mais importantes e explicações.
* Criar um repositório para o código (github) e indicar nesta seção. 

## Experimentos 

* Apresentar os experimentos ou testes executados. 
* Explicar os resultados. 
