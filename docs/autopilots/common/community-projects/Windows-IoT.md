#Windows IoT

----------

**Aviso legal**

O Windows IoT SDK é um projeto pessoal do membro da comunidade Emlid [Tony Wall](http://community.emlid.com/users/codechief).

Para qualquer suporte, por favor, refira-se a ele no [Tópico do projeto](http://community.emlid.com/t/windows-10-iot-image-for-navio/381/last).

Todo o texto a seguir é fornecido pelo autor do projeto.

**Fim do aviso**

----------

## Projeto

Em 2015, a Microsoft anunciou o suporte oficial para o Raspberry Pi 2 executando o  Windows 10 em sua nova plataforma "IoT" (Internet of Things). Logo após o trabalho começou em um SDK de código aberto para Navio com os seguintes objetivos:

 - Permitir o acesso a todos os grandes sensores dos projetos Navio HAT para Windows DIY.
 - Prover o conceito de um piloto automático executando um sistema operacional Windows (quadricoptero no modo de vôo estabilizado).
 - Forneçer as ferramentas necessárias para a comunidade ampliar os pilotos automáticos existentes para o Windows e/ou criar um sistema totalmente novo.

Este documento descreve como desenvolver seu próprio código para o Navio em execução no Windows 10 IoT, ou apenas experimentar o teste e os aplicativos de amostra para você mesmo. Você pode acompanhar o progresso no [Tópico do fórum Emlid](http://community.emlid.com/t/windows-10-iot-image-for-navio/381/last) ou no [Projeto Hackster.io](https://www.hackster.io/team-code-for-robots/navio-sdk-for-windows-iot). Se você gosta, por favor "respeite" o projeto Hackster para envie alguns elogios ao criador ;-)


## Introdução

Esta seção descreve como iniciar o desenvolvimento instalando o Windows IoT em seu dispositivo, o Visual Studio em seu PC e, finalmente, como clonar, criar e executar o código.


### Ferramentas

Links para baixar as ferramentas de desenvolvimento e instruções básicas sobre o que instalar são fornecidos pela Microsoft aqui:

[Introdução ao Windows IoT](http://ms-iot.github.io/content/en-US/GetStarted.htm)

A ferramente  "IoT Dashboard" facilita o processo de download da imagem do SO para o Raspberry Pi 2, ajudando você a se conectar e configurar dispositivos pela rede. Os usuários finais precisam apenas instalar o painel, ou você pode fornecer um cartão SD pré-carregado.

Alternativamente, você pode baixar a imagem ISO e as notas de lançamento aqui:

[Outros Downloads](http://ms-iot.github.io/content/en-US/Downloads.htm)

Como o código fonte do driver do dispositivo foi adicionado à solução, você também precisará instalar o WDK (Windows Driver Kit) mais recente disponível no link  "Instalar o WDK 10" nesta página:

[Downloads do kit de drivers](https://msdn.microsoft.com/en-US/windows/hardware/dn913721%28v=vs.8.5%29.aspx?f=255&MSPPError=-2147217396)


## Desenvolvimento

A "estrutura" atual do Navio SDK é uma biblioteca do Windows Universal, portanto, pode ser consumido por qualquer aplicativo do Windows Universal escrito em qualquer linguagem .NET suportada pelo IoT. Basta usar o gerenciador de pacotes NuGet do Visual Studio para adicionar o pacote [Emlid.WindowsIot.Hardware](https://www.nuget.org/packages/Emlid.WindowsIoT.Hardware) e, em seguida, escrever um código com várias classes de dispositivos Navio para utilizar o hardware.

A implantação é fácil com os pacotes do Windows Universal produzidos pelo Visual Studio. Eles são pré-compilados e carregados diretamente no dispositivo IoT. Você pode implantá-los via Visual Studio durante o desenvolvimento, por meio da interface da Web para usuários finais, roteirá-los com o PowerShell ou incluí-los em uma imagem de cartão SD pré-carregada.


### Preparação

É altamente recomendável que você conclua algum treinamento básico antes de tentar criar e implantar seu primeiro projeto de IoT. Até mesmo desenvolvedores experientes do Windows terão de se familiarizar com alguns dos requisitos especiais de contrução e implantação para o processador ARM e conexões remotas de dispositivos IoT. Além disso, o sistema IoT ainda está em desenvolvimento inicial, mesmo que as compilações de desktop do Windows 10 estejam concluídas.

 1. Siga as instruções para criar, implantar e debug o exemplo [Amostra IoT "Olá Mundo](http://ms-iot.github.io/content/en-US/win10/samples/HelloWorld.htm).
 2. No Visual Studio, abra a caixa de diálogo "Tools - Extensions and Updates...".
 3. Selecione a guia "Updates (Atualizações)" e atualize a extensão do GitHub  e "Modelos de projeto do Windows IoT Core".
 4. Opcionalmente, atualize todos os outros itens (várias atualizações serão necessárias).
Atualize a extensão GitHub (menu "Tools - Extensions and Updates...").
 5. Selecione a guia "Online", pesquise e instale a extensão "Productivity Power Tools 2015" extension.


### Construindo o SDK a partir da fonte

1. Clique em "Team Explorer" e depois no ícone "Plug" icon (gerenciar conexões).
2. Em "Local Git Repositories", clique no link "Clone" e, em seguida, insira o URL do projeto  GitHub"[https://github.com/emlid/Navio-SDK-Windows-IoT](https://github.com/emlid/Navio-SDK-Windows-IoT)", escolha um diretório no qual os arquivos serão salvos, em seguida, clique no botão "Clone".
3. Abra a solução selecionando a opção de menu "File - Open - Project/Solution..." e, em seguida, o arquivo de solução (.sln) do diretório que você acabou de clonar.
4. Na janela Solution Explorer, clique com o botão direito do mouse na raiz da solução e selecione "Power Commands - Open Command Prompt" e insira os comandos:
  ```
  cd Tools\Scripts
  PowerShell.exe -File "Setup Local.ps1"
  exit
  ```
5. Na barra de ferramentas de construção, verifique se a consfiguração "Debug" e a plataforma "ARM" estão selecionadas.
6. No menu "Build", selecione "Rebuild Solution". 
7. Alguns pacotes do NuGet podem ser carregados, então a compilação será iniciada e deverá ser concluída sem erros.


### Crie e execute o aplicativo de teste de hardware

Atualmente, o aplicativo Hardware Test é distribuído apenas como fonte. Mais tarde, também estará disponível para uso independente (sem ferramentas de desenvolvimento).

1. Certifique-se de que as ferramentas remotas de debugging estejam em execução no dispositivo. Use a página "Debugging" da web (facilmente acessível através da ferramenta do painel) e a porta remota, que devem corresponder em sua configuração de implantação.
2. Na barra de ferramentas de contrução, verifique se o processador "ARM" e o projeto de inicialização "Navio Hardware Test" estão selecionados.
3. Clique na seta suspensa ao lado do ícone verde "Play (reproduzir)" e depois em "Remote Machine (Máquina remota)".
4. Se está é a priemeira vez que você implementa em uma máquina remota, a caixa de diálogo de configuração aparecerá, então você deverá digitar o  "nome:porta" do dispositivo de destino e escolher nenhuma autenticação.
5. Com a máquina remota ainda selecionada, clique no ícone "Play" para construir, implantar e executar a amostra.
6. A solução deve compilar, implantar e executar no Navio/RasPi. Use o mouse para selecionar "LED & PWM" (o restante ainda não está implementado). Verifique a frequência, defina alguns controles deslizantes (melhor PWM próximo ao fundo) e clique no botão Output ON.
7. Jogue com os controles deslizantes para testar seu LED e servos/ESCs.
8. Monitore a janela de saída para quaisquer mensagens e observe o dispositivo físico em busca de qualquer feedback, por exemplo, cores do LED ou servo movimento.
9. Clique em "Close (Fechar)" e depois em then "Exit (sair)" para finalizar.


### Construa e execute as amostras

Atualmente as aplicações do projeto são "Aplicativo de Plano de Fundo", o que significa que eles são executados diretamente do Visual Studio sem interface com usuário. Toda a saída é enviada de volta via mensagens de debug para a janela "Output (Saída)" do Visual Studio.

1. Certifique-se de que as ferramentas remotas de debugging estejam em execução no dispositivo. Use a página "Debugging" da web (facilmente acessível através da ferramenta do painel) e a porta remota, que devem corresponder em sua configuração de implantação.
2. Na barra de ferramentas de contrução, verifique se o processador "ARM" e o projeto de inicialização "Navio Hardware Test" estão selecionados.
3. Clique na seta suspensa ao lado do ícone verde "Play (reproduzir" e, em seguida, em "Remote Machine (Máquina remota)".
4. Se está é a primeira vez que você implementa em uma máquina remota, a caixa de diálogo de configuração aparecerá, então você deve digitar "nome:porta" do dispositivo de destino e escolher nenhuma autenticação.
5. Com a máquina remota ainda selecionada, clique no ícone "Play" para construir, implantar e executar a amostra.

Monitore a janela de saída para quaisquer mensagens e observe o dispositivo físico em busca de qualquer feedback, por exemplo, cores dos LEDS ou servo movimento.


## Problemas conhecidos

1. Desde a atualização 1 e a IoT 10586, é necessário iniciar manualmente as ferramentas remotas de debugging por meio da interface da web, antes que a implantação seja bem-sucedida. Além disso, o nome da máquino remota deve ser editada para incluir a porta remota do debugger, que aparece na caixa de diálogo de confirmação ao iniciá-los através da interface da web, por exemplo MyDeviceName:8116.
2. A saída RC é super lenta porque estamos no meio da conversão para C++ e drivers. A decodificação de software é muito lenta e não confiável no modo de usuário no Raspberry Pi, por isso é adiada até que todos os outros componentes sejam implementados. O Navio2 pode ser suportado priemiro porque possui hardware de decodificação.

## Roteiro

A versão atual da estrutura é conhecida como "estrutura modo do usuário". Posteriormente, ela será convertido em um componente de tempo de execução do Windows para que possa ser consumido por outros idiomas além do .NET, como o C++ nativo e o NodeJS. A linguagem de desenvolvimento também mudará para componentes de tempo de execução C++ de nível inferior, que trabalharão em consjunto com os novos drivers de dispositivo específico do Navio para desempenho e confiabilidade. Quando os drivers de dispositivo estiverem disponíveis, uma imagem personalizada de IoT também será produzida para facilitar a instalação. As instruções de criação de OEM também serão incluídas para que você possa integrar os drivers individuais à sua própria imagem de IoT.

The framework will remain, but the Navio*Device classes will be redirected to call the new lower level components. However it is inevitable that some breaking changes will occur. At least the user mode framework will continue to run on standard IoT images (without drivers installed) so you have to possibility to stay with the older NuGet package versions and upgrade when it suits you.

It remains to be seen what Microsoft will provide for provisioning and deployment of end user applications, and if there will be any other editions than "core". There could even be Windows Store support, some other kind of one-click installation. We can already provide manual download links to "side-load" standard packages onto the device via the web interface. The goal is to achieve "plug and play" installation and configuration of Navio and Navio enabled apps running Windows IoT.


### Milestone 1: "Core SDK"

Mission statement: "Full hardware support for Navio on Windows IoT."

1. LED & PWM (PCA9685) device, sample and test app (100% complete).
2. RC Input (PPM over GPIO) device, sample and test app (60% complete).
3. IMU (MPU9250) device, sample and test app.
4. Barometer (MS5611) device, sample and test app (100% complete).
5. GPS (U-Blox NEO-MN8, possibly NEO7M, NEO-6T too) device, sample and test app.
6. ADC (ADS1115) device, sample and test app.
7. FRAM (MB85RC04) device, sample and test app (100% complete).
 

### Milestone 2: "Windows Drone PoC"

Mission statement: "Answer the question; can you really fly a drone with Windows IoT?"

1. Motor control and PID logic for a quadcopter.
2. FMU test app, enable raw flight with RC input in stabilize mode.
3. Manual (acrobatic) mode and switch capability.
3. RTL failsafe mode and switch.
4. GPS fence failsafe with auto-correction.
5. MAVLink proxy, including Ground Control Station support.


### Future Milestones:

Mission statement: "Expand support and add new devices."

1. Support other multi-rotor types in the PoC application.
2. FrSky telemetry.
3. FrSky SPort (sensors) device(s) and test app(s).
4. U-Center GPS proxy for easy configuration / upgrade of GPS.
5. RTKLib integration for highly accurate GPS.
6. Emlid REACH integration, similar to Windows Remote Arduino or as close to full IoT Intel Galileo (Intel Edison) support as possible (depends on how good Microsoft's support is in this area).
7. Expand the Windows drone sample, possibly spinning-off a new autopilot (includes support for other vehicle types, e.g. planes and rovers).
8. Experiment with Windows HAL for ArduPilot (requires a good Linux code bridge like Cygwin, but for IoT).
9. Expand samples for other (than drone) projects, e.g. general robotics, trackers and DIY solutions.


## Change Log

*2016.01.02* v1.0.7

1. FRAM memory device implemented, both the MB85RC256V of Navio+ and MB85RC04V of Navio.
2. General test app GUI display, usability and multi-threading improvements.

Continued development of the core SDK. FRAM complete and fully tested on Navio+ device. Support for the original Navio's FRAM chip had been coded but has not been fully tested.


*2015.12.28* v1.0.6

1. Barometer (pressure and temperature sensor) implemented.
2. LED cycle feature added to LED & PWM test app.

Continued development of the existing C# sensor library. Note the barometer temperature is always about hotter that then environment because of heating by surrounding components, e.g. the Raspberry Pi processor is sitting underneath it. For that reason you will also see the temperature rise in the hardware test app, because the processor is being taxed with the GUI functionality.


*2015.12.22* v1.0.5
 
1. Device driver "capability" proof of concept. Working skeleton RC input kernel mode WDF driver which successfully deploys and can be communicated with from a user mode Universal application.
2. Various patches and documentation updates to existing source code and samples.
3. Updated solution to work with latest IoT December 2015 OS build 10586, SDK/WDK 1511, Visual Studio 2015 Update 1.
 
This is an interim check-in which does not add any end-user (consumer developer) functionality, but solves the technical requirement to be able to write a kernel mode driver and communicate from user mode.
Because of the difficulties with the VS2015 tools and lack of improvement in update 1 (which actually got worse in some aspects) focus will temporarily shift back to the user mode framework for all other features than RC Input.
Also with the release of Navio 2 which has RC Input hardware, the device driver can wait a little while, so we can first gain the full chipset functionality in user mode and maybe see if a simple quadcopter can hover with it (and a joystick instead of RC input).
Commitment is still there to produce high performance (real-time) and robust drivers shortly after a successful user mode PoC.


*2015.10.06* v1.0.4

1. RC input C# proof of concept complete (so far as worthwhile). Only accurate to ~1 second due to "floating GPIO" limitation (see notes below). 
2. Completed RC Input in hardware test app and added new RC Input C# sample.
3. Added NuGet package. No need to download source and compile to use the hardware from now on! The package URL is: https://www.nuget.org/packages/Emlid.WindowsIot.Hardware

Due to a Microsoft IoT image limitation, specifically with the GPIO #4 pin hard-wired to RC Input on the Navio, we cannot achieve any kind of acceptable performance in "user mode" code.
Furthermore, the current UWP API has a lack of support for time critical threads in "user mode". It is necessary to bring forwards the conversion to C++ and device drivers.
The next release will include only drivers and provide access via a C++/CX coded Windows Runtime Component. C# code and any other language can then be used as normal for user applications.


*2015.08.25*

1. RC input progress. Test app enables use of RC Input, but no GUI interaction yet. For now use the Output window of Visual Studio to see debug messages reporting decoded PPM values. They appear to be correct, but some tuning of rounding and frame error detection is required.


*2015.08.14*

1. RC Input hardware classes done in their first untested form.
2. RC Input model and preliminary view added to hardware test app, but not enabled yet.
3. Minor clean-up and coding standards in other classes and projects.


*2015.07.31*

1. Updated solution to Visual Studio 2015 RTM.
2. Hardware Test App - Fixed theme/resource errors and applied "Emlid green" instead of default "IoT pink".
3. SetLED - Fixed error in buffer write routine.
