# DMX_UART_MIDI-to-MODBUS-Interface-with-Altivar-ATV12
Usando a interface chinesa DMX UART MIDI para Modbus interface com o inversor Altivar ATV12

O presente relatório oferece uma análise detalhada e orientações para a implementação de uma
interface de conversão DMX para MODBUS específica para controlar um inversor Altivar ATV 12.
O objetivo é criar um sistema de controle que permita manipular a direção, velocidade e
ativação/desativação de um motor através de canais DMX, facilitando a integração deste
dispositivo em instalações artísticas e cenográficas automatizadas. A solução proposta
aproveita a estrutura de canais DMX para enviar comandos específicos ao inversor, respeitando
suas características de operação via protocolo MODBUS.

A interface adquirida funciona como um tradutor entre o protocolo DMX 512 , amplamente
utilizado em iluminação cênica, e o protocolo MODBUS, comum em equipamentos industriais
como inversores de frequência. Conforme documentado no manual fornecido, esta interface
utiliza uma estrutura de comunicação baseada no protocolo LiDBus para facilitar a conversão
entre diferentes sistemas de comunicação, incluindo DMX 512 , UART, e MIDI.

O protocolo DMX 512 permite o controle de até 512 canais, cada um com valores entre 0 e 255
8 bits. A interface organiza esses canais em grupos de 8 intervalos 0 7 , 8 15 , etc.), permitindo
a implementação de controles customizados para cada faixa. Esta organização é
particularmente útil para atribuir diferentes funções a canais específicos, como necessário para
controlar os parâmetros do inversor Altivar ATV 12.

A comunicação é realizada através de pacotes de dados formatados de acordo com o protocolo
LiDBus, com um cabeçalho de sincronização SOP, endereço do dispositivo, código de
comando, flag de controle, comprimento de dados, dados e palavra de verificação. Esta
estrutura garante a integridade da comunicação entre a interface e o inversor, reduzindo erros
de transmissão e permitindo a confirmação de comandos enviados.

O protocolo LiDBus, desenvolvido pela Guangzhou Lidang Electronic Technology, serve como
base para a comunicação na interface. Cada pacote de dados começa com um cabeçalho de
sincronização A 5 AA, seguido pelo endereço do dispositivo, código de comando, flag de
controle, comprimento de dados, dados e palavra de verificação. Esta estrutura permite uma
comunicação confiável e inequívoca entre a interface e o inversor.

A porta serial utilizada é configurada com 8 bits de dados, sem paridade, 1 bit de parada, e uma
taxa de transmissão padrão de 115200 bps, suportando tanto o modo RS 485 quanto RS 232. A

# Entendendo a Interface de Conversão DMX->MODBUS
A interface adquirida funciona como um tradutor entre o protocolo DMX512, amplamente
utilizado em iluminação cênica, e o protocolo MODBUS, comum em equipamentos industriais
como inversores de frequência. Conforme documentado no manual fornecido, esta interface
utiliza uma estrutura de comunicação baseada no protocolo LiDBus para facilitar a conversão
entre diferentes sistemas de comunicação, incluindo DMX512, UART, e MIDI .
O protocolo DMX512 permite o controle de até 512 canais, cada um com valores entre 0 e 255
(8 bits). A interface organiza esses canais em grupos de 8 intervalos (0-7, 8-15, etc.), permitindo
a implementação de controles customizados para cada faixa. Esta organização é
particularmente útil para atribuir diferentes funções a canais específicos, como necessário para
controlar os parâmetros do inversor Altivar ATV12 .
A comunicação é realizada através de pacotes de dados formatados de acordo com o protocolo
LiDBus, com um cabeçalho de sincronização (SOP), endereço do dispositivo, código de
comando, flag de controle, comprimento de dados, dados e palavra de verificação. Esta
estrutura garante a integridade da comunicação entre a interface e o inversor, reduzindo erros
de transmissão e permitindo a confirmação de comandos enviados 

# Estrutura do Protocolo LiDBus
O protocolo LiDBus, desenvolvido pela Guangzhou Lidang Electronic Technology, serve como
base para a comunicação na interface. Cada pacote de dados começa com um cabeçalho de
sincronização (A5 AA), seguido pelo endereço do dispositivo, código de comando, flag de
controle, comprimento de dados, dados e palavra de verificação. Esta estrutura permite uma
comunicação confiável e inequívoca entre a interface e o inversor .
A porta serial utilizada é configurada com 8 bits de dados, sem paridade, 1 bit de parada, e uma
taxa de transmissão padrão de 115200bps, suportando tanto o modo RS485 quanto RS232. A
organização dos bytes segue o formato big-endian (byte mais significativo primeiro), garantindo
compatibilidade com diversos equipamentos industriais

# Configuração do Sistema para Controle do Inversor ATV12
Para implementar o controle do inversor Altivar ATV12 via DMX, é necessário configurar três
canais distintos para as funções desejadas: direção do motor, velocidade e
ativação/desativação. Considerando a estrutura de canais da interface, que separa comandos a
cada N intervalos, podemos definir um mapeamento eficiente para estas funções.

# Proposta de Mapeamento dos Canais DMX para comandos no Inversor
Canal 1 DMX 1 -> Controle de direção do motor: 
Valores 0  127  Sentido horário (forward)
Valores 128  255  Sentido anti-horário (reverse)

Canal 2 DMX 2 -> Controle de velocidade do motor:
Valores 0  255  Representando 0  100 % da velocidade máxima configurada no inversor

Canal 3 DMX 3 -> Ativação/Desativação do motor:
Valores 0  127  Motor desativado (stop)
Valores 128  255  Motor ativado (run)

# Lista de comandos com cálculo cd CRC16 Modbus - arquivo AltivarATV12.ini:

A lista abaixo contempla os comandos desejados para o Altivar ATV12. Ainda falta checagem no Inversor (06/03/2025).

O estudo acima foi renderizado no Perplexity.ai, alimentando-o com os manuais da interface, do inversor, e do protocolo Modbus.

[CMD_LIST]

CmdLine1=A5;AA;01;11;81;00;08;00;00;01;00;01;02;39;F1
Check1=1
CmdTime1=10
Note1=FORWARD
CmdLine2=A5;AA;01;11;81;00;08;00;00;01;00;01;02;39;F1
Check2=1
CmdTime2=10
Note2=FORWARD
CmdLine3=A5;AA;01;11;81;00;08;00;00;01;00;01;02;39;F1
Check3=1
CmdTime3=10
Note3=FORWARD
CmdLine4=A5;AA;01;11;81;00;08;00;00;01;00;01;02;39;F1
Check4=1
CmdTime4=10
Note4=FORWARD
CmdLine5=A5;AA;01;11;81;00;08;00;00;01;00;01;04;3B;31
Check5=1
CmdTime5=10
Note5=REVERSE
CmdLine6=A5;AA;01;11;81;00;08;00;00;01;00;01;04;3B;31
Check6=1
CmdTime6=10
Note6=REVERSE
CmdLine7=A5;AA;01;11;81;00;08;00;00;01;00;01;04;3B;31
Check7=1
CmdTime7=10
Note7=REVERSE
CmdLine8=A5;AA;01;11;81;00;08;00;00;01;00;01;04;3B;31
Check8=1
CmdTime8=10
Note8=REVERSE
CmdLine9=A5;AA;01;11;81;00;08;00;00;02;00;01;03;2B;30
Check9=1
CmdTime9=10
Note9=SPEED_0.3
CmdLine10=A5;AA;01;11;81;00;08;00;00;02;00;01;53;79;21
Check10=1
CmdTime10=10
Note10=SPEED_8.25
CmdLine11=A5;AA;01;11;81;00;08;00;00;02;00;01;A1;B6;42
Check11=1
CmdTime11=10
Note11=SPEED_16.5
CmdLine12=A5;AA;01;11;81;00;08;00;00;02;00;01;F0;85;A3
Check12=1
CmdTime12=10
Note12=SPEED_24.16
CmdLine13=A5;AA;01;11;81;00;08;00;00;02;00;02;3F;C5;A5
Check13=1
CmdTime13=10
Note13=SPEED_32.12
CmdLine14=A5;AA;01;11;81;00;08;00;00;02;00;02;8D;FA;46
Check14=1
CmdTime14=10
Note14=SPEED_40.07
CmdLine15=A5;AA;01;11;81;00;08;00;00;02;00;02;DC;AB;97
Check15=1
CmdTime15=10
Note15=SPEED_48.03
CmdLine16=A5;AA;01;11;81;00;08;00;00;02;00;03;64;23;69
Check16=1
CmdTime16=10
Note16=SPEED_MAX
CmdLine17=A5;AA;01;11;81;00;08;00;00;03;00;01;01;7A;31
Check17=1
CmdTime17=10
Note17=STOP
CmdLine18=A5;AA;01;11;81;00;08;00;00;03;00;01;01;7A;31
Check18=1
CmdTime18=10
Note18=STOP
CmdLine19=A5;AA;01;11;81;00;08;00;00;03;00;01;01;7A;31
Check19=1
CmdTime19=10
Note19=STOP
CmdLine20=A5;AA;01;11;81;00;08;00;00;03;00;01;01;7A;31
Check20=1
CmdTime20=10
Note20=STOP
CmdLine21=A5;AA;01;11;81;00;08;00;00;03;00;01;02;7B;71
Check21=1
CmdTime21=10
Note21=RUN
CmdLine22=A5;AA;01;11;81;00;08;00;00;03;00;01;02;7B;71
Check22=1
CmdTime22=10
Note22=RUN
CmdLine23=A5;AA;01;11;81;00;08;00;00;03;00;01;02;7B;71
Check23=1
CmdTime23=10
Note23=RUN
CmdLine24=A5;AA;01;11;81;00;08;00;00;03;00;01;02;7B;71
Check24=1
CmdTime24=10
Note24=RUN
CmdLine25=A5 ;AA ;01 ;11 ;81 ;00 ;08 ;00 ;00 ;04 ;00 ;01 ;00 ;78 ;30 
Check25=1
CmdTime25=10
Note25=READ_HMIS
CmdLine26=A5 ;AA ;01 ;11 ;81 ;00 ;08 ;00 ;00 ;04 ;00 ;01 ;00 ;78 ;30 
Check26=1
CmdTime26=10
Note26=READ_HMIS
CmdLine27=A5 ;AA ;01 ;11 ;81 ;00 ;08 ;00 ;00 ;04 ;00 ;01 ;00 ;78 ;30
Check27=1
CmdTime27=10
Note27=READ_HMIS
CmdLine28=A5 ;AA ;01 ;11 ;81 ;00 ;08 ;00 ;00 ;04 ;00 ;01 ;00 ;78 ;30
Check28=1
CmdTime28=10
Note28=READ_HMIS
CmdLine29=A5 ;AA ;01 ;11 ;81 ;00 ;08 ;00 ;00 ;04 ;00 ;01 ;00 ;78 ;30
Check29=1
CmdTime29=10
Note29=READ_HMIS
CmdLine30=A5 ;AA ;01 ;11 ;81 ;00 ;08 ;00 ;00 ;04 ;00 ;01 ;00 ;78 ;30 
Check30=1
CmdTime30=10
Note30=READ_HMIS
CmdLine31=A5 ;AA ;01 ;11 ;81 ;00 ;08 ;00 ;00 ;04 ;00 ;01 ;00 ;78 ;30 
Check31=1
CmdTime31=10
Note31=READ_HMIS
CmdLine32=A5 ;AA ;01 ;11 ;81 ;00 ;08 ;00 ;00 ;04 ;00 ;01 ;00 ;78 ;30 
Check32=1
CmdTime32=10
Note32=READ_HMIS

