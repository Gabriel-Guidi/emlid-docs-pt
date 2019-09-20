## Correction output (Saída de correção)

<p style="text-align:center"><img src="../img/reachview/base-mode/output.png" style="width: 800px;"/></p>

A saída de correção do Reach é no formato RTCM3, pradrão da industria. Os dados de correção podem ser enviados via Serial, TCP, NTRIP ou LoRa.

### Serial
A conexão da porta serial está disponível através de várias opções de conexão de hardware. Todos eles suportam as seguintes taxas de transmissão: 4800, 9600, 14400, 19200, 28800, 38400, 56000, 57600, 115200, 128000, 153600, 230400, 256000, 460800.

#### UART
Corresponde à porta RS232 no Reach RS2. Maneira comum de conectar ao rádio para enviar dados de correção.

#### USB para PC
Quando conectado via USB a um PC, Reach será exibido como vários dispositivos, um deles será uma porta serial. Você pode usar esta porta serial para enviar dados de correção para o PC.

#### USB OTG
Use um cabo OTG micro-USB para conectar acessórios USB. Nesse modo, apenas os dispositivos USB que emulam uma porta serial podem ser usados. Exemplo de chips populares suportados: FT232, CP2102. Existem vários dispositivos criados nesses chips que fornecerão uma porta TTL UART ou RS232. 

### NTRIP
O NTRIP é a maneira padrão da industria de tranferir correções do GNSS pela internet. Com o ReachView, você pode usar qualquer serviço público ou seu próprio caster. O NTRIP não suporta comunicação ponto a ponto, por exemplo você não pode usá-lo para transferir correções de um Reach para outro diretamente. Na terminologia NTRIP, existem servidores, clientes e casters. O servidor envia a correção para um caster e os clientes podem recebê-los conectando-se a esse caster.

Para enviar a correção para um caster NTRIP, você precisa saber:

- Endereço IP ou nome do domínio do caster
- Port (Porta)
- Username (Usuário)
- Password (Senha)
- Mount point (Nome do base)

Ao conectar via NTRIP no modo base, o Reach atua como um servidor NTRIP.

### TCP
O cenário típico para o uso do TCP é o envio de dados de correção para um aplicativo na mesma rede ou para um servidor com IP público.

O TCP suporta duas funções:

#### Servidor
Você precisa especificar a porta e, depois disso, os clientes poderão se conectar a este dispositivo no endereço IP. Muitos clientes podem estar conectados ao mesmo servidor.
#### Cliente
Você precisa especificar o endereço IP do servidor e o número da porta.

Se o ReachView não permitir definir um determinado número de porta, significa que ele está reservado para uso interno.

### Radio LoRa
O Reach RS2 possui um rádio LoRa interno que é usado para receber ou enviar correções. O rádio funciona apenas de uma maneira: ele pode ser configurado para enviar correções (na base) ou recebê-las (em rover). Usando a modulação LoRa, é possível atingir até 8 Km na linha de visão ou alguns Km em áreas urbanas com apenas 20 dBm de potência. Contanto que as configurações de frequência e o air rate correspondam a um número ilimitado de rovers, é possível receber a correção da mesma base.

Quanto menor o air rate, maior será a distância de trabalho. Dependendo da sua seleção de mensagens RTCM3, o ReachView bloqueará automaticamente as taxas de air insuficientes. Desative as mensagens de correção ou reduza a taxa para desbloquear taxas de air mais baixas. A taxa de air na transmissão do Reach e na recepção devem se corresponder.

Certifique-se de selecionar a potência e a frequência de saída apropriadas, de acordo com os regulamentos locais.

### Bluetooth
Você pode usar o Bluetooth para a saída de correção. Observe que você não pode definir a saída de posição e a saída de correção básica para Bluetooth ao mesmo tempo.

## Mensagens RTCM3

<p style="text-align:center"><img src="../img/reachview/base-mode/messages.png" style="width: 800px;"/></p>
O subconjunto mínimo de mensagens necessário para o RTK funcionar são as mensagens 1074 em 1 Hz com observações GPS e a mensagens 1006 em 0,1 Hz com a posição da antena da base. A ativação de mais mensagens ou taxas mais altas requer maior largura de banda de conexão.

Em taxa de dados, você pode encontrar uma estimativa de bytes/s para 1 satélite quando as mensagens são configuradas em 1 Hz.

|Mensagens RTCM3|Tipo de mensagem|Taxa de dados, bytes/sec (1 satélite, 1Hz)|
|:---:|:---:|:---:|
||**Mensagens necessárias**||
|1006|ARP station coordinate   | 27|
|1074|GPS MSM4      |19.38|
||**Mensagens opcionais para outros GNSS **||
|1084|GLONASS MSM4|19.38|
|1094|Galileo MSM4|9.58|
|1124|BeiDou MSM4|18.9|


Aqui estão algumas informações sobre cada mensagem do padrão RTCM 10403.3¹<sup>[1](#myfootnote1)</sup>:

- **Mensagem 1006** fornece as coordenadas (ECEF) do ponto de referência da antena (ARP) para uma estação de referência estacionária e a altura do ARP acima de um monumento. É a mensagem obrigatória para ativar.

- **Mensagens 1074 (GPS), 1084(GLONASS), 1094 (Galileo), 1124 (BeiDou)** são MSM4 (Multiple Signal Messages). MSM4 são mensagens de alta precisão que contêm um conjunto completo de observações RINEX com resolução estendida. Isso significa que você deve ativar apenas uma mensagem do sistema escolhido para obter todos os dados sobre ele. Recomenda-se manter ativada pelo menos a mensagem GPS 1074.

## Base position (Posição da base)


!!! tip ""
    [Check the Placing the base guide](https://docs.emlid.com/reachrs2/common/tutorials/placing-the-base/) to learn about different ways to set up your base.  

<p style="text-align:center"><img src="../img/reachview/base-mode/position.jpg" style="width: 800px;"/></p>

There are two main options how to specify base station position. Note that RTK positioning is relative to the base station, so any inaccuracy in it’s position will result in a constant shift of rover coordinates. For many applications it is not critical and averaged single coordinate of the base could be used. If your application requires absolute accuracy for rover position an accurate  base coordinate must be entered.

### Manual
In this mode you supply an a priori known coordinate by locating the unit above surveyed point. Coordinate has to be supplied in ECEF XYZ or in WGS84 Latitude and Longitude and WGS84 ellipsoid height. Antenna height offset is entered at this stage as well, offset is limited to 6.5535 m by the RTCM message.

You can change position format in the top right corner of Base coordinates frame.

<p style="text-align:center"><img src="../img/reachview/base-mode/manual.jpg" style="width: 800px;"/></p>

### Average 
By default Reach will average base position every time it starts. This feature significantly simplifies initial setup in a new location, however it will not provide an accurate absolute coordinate.

ReachView has a unique feature that allows it to determine base station position while working as a rover from another base. This is done by obtaining RTK Fixed solution, averaging it over a period of time and this way obtaining an accurate position for the base. A typical scenario would involve setting up a local base station by determining its coordinate from NTRIP and then broadcasting correction locally, thus reducing the baseline for rovers and improving positioning performance.

If the reference station is too far away it is possible to average float and still improve the accuracy of the position.

In case no correction is available when setting up base or absolute accuracy is not required averaged single coordinates could be used.

**Save averaged position to manual**  
After you have successfully obtained averaged position you might want to save it for future use. Click on the “save coordinates” icon and position will be saved as if it was entered in manual mode. Now every time Reach starts it will broadcast this position in correction messages.

**Repeat averaging**  
If you would like to restart base position averaging process you can click on “repeat averaging” icon. This is especially useful in a situation when you accidentally moved Reach during averaging.

<p style="font-size:70%;"><a name="myfootnote1">1</a>: Radio Technical Commission for Maritime Services. 2016. RTCM STANDARD 10403.3 DIFFERENTIAL GNSS (GLOBAL NAVIGATION SATELLITE SYSTEMS) SERVICES – VERSION 3. Virginia: Radio Technical Commission for Maritime Services, pp. 108-262</p>
