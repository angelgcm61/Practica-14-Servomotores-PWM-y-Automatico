# Practica-14-Servomotores-PWM-y-Automatico
Control de un servomotor SG90 con el microcontrolador, utilizando un potenciómetro para variar su ángulo de 0° a 180° mediante PWM por hardware y automaticamente.

## Control de un servomotor con potenciómetro usando PIC16F887

En esta práctica se trabajó con el uso de servomotores, específicamente con el servomotor SG90, utilizando el microcontrolador PIC16F887. El objetivo principal fue comprender cómo se puede controlar la posición angular de un servomotor mediante una señal PWM, además de aplicar una lectura analógica proveniente de un potenciómetro para modificar su ángulo de manera variable.

Los servomotores son actuadores muy utilizados en sistemas de control, robótica y automatización debido a que permiten posicionar un eje en un ángulo específico. A diferencia de un motor de corriente directa convencional, un servomotor no gira libremente de forma continua, sino que se mueve dentro de un rango limitado, normalmente de 0° a 180°. Para lograr este movimiento, el servo recibe una señal PWM, donde el ancho del pulso determina la posición del eje.

En esta práctica, el servomotor se controla desde el pin RC2 del PIC16F887, utilizando el módulo CCP1 en modo PWM por hardware. Esto permite generar una señal más estable y precisa en comparación con un PWM hecho completamente por software. El potenciómetro se conecta al pin RA0, configurado como entrada analógica AN0, y funciona como el elemento de control para variar el ángulo del servo.

El funcionamiento del programa consiste en leer constantemente el valor analógico entregado por el potenciómetro. Esta lectura puede variar entre 0 y 1023, ya que el convertidor analógico-digital del PIC trabaja con una resolución de 10 bits. Posteriormente, este valor se convierte a un rango adecuado para el servomotor, tomando como referencia los valores de PWM definidos para 0° y 180°.

Para este caso, se establecieron los siguientes valores:

- `SERVO_0 = 31`, correspondiente aproximadamente a 0°.
- `SERVO_180 = 156`, correspondiente aproximadamente a 180°.

Estos valores representan el ancho del pulso necesario para mover el servomotor dentro de su rango completo. De esta manera, cuando el potenciómetro se encuentra en su posición mínima, el servo se aproxima a 0°, y cuando el potenciómetro llega a su posición máxima, el servo se mueve hacia 180°. Los valores intermedios permiten posicionar el servo en ángulos proporcionales.

El programa inicia configurando el ADC para leer la señal analógica del potenciómetro conectado en RA0. Después, se configura el módulo PWM utilizando el Timer2, con un prescaler de 1:16 y un valor de `PR2 = 249`, generando un periodo aproximado de 16 ms. Aunque los servomotores suelen trabajar con periodos cercanos a 20 ms, este periodo permite controlar correctamente el SG90 dentro de la simulación y el montaje físico.

Dentro del ciclo principal, el microcontrolador realiza continuamente la lectura del ADC, calcula el valor correspondiente de PWM y actualiza el ciclo de trabajo enviado al servo. Esto permite que el movimiento sea continuo y responda directamente al giro del potenciómetro.

## Conexiones utilizadas

| Componente | Pin del PIC16F887 | Descripción |
|----------|-------------------|-------------|
| Potenciómetro | RA0 / AN0 | Entrada analógica para controlar el ángulo |
| Servo SG90 | RC2 / CCP1 | Salida PWM hacia la señal del servo |
| VCC del servo | 5V externo o regulado | Alimentación del servomotor |
| GND del servo | GND común | Tierra compartida con el PIC |

Es importante que el GND del servomotor y el GND del PIC estén conectados en común, ya que de lo contrario la señal PWM puede no ser interpretada correctamente por el servo.
