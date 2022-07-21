# Ferramenta-Aireplay-ng-para-raspberry-pi
Descrição da aula


Vídeo Ataque à estação com Aireplay-ng
O objetivo desta apresentação é apresentar um estudo de caso de uso das ferramentas do Aircrack-NG. A nossa rede aqui é formada por essa máquina de trabalho (professor está com foco no prompt de comando do computador mencionado por ele) com endereço 192.168.70.73 conectada na rede “RedeISP070”. 
Nesta máquina .70.73 temos uma unidade de monitoramento, isto é, um RaspBerry PI com uma câmera com endereço 192.168.70.70 e aqui a câmera está disponibilizando nossos dois servidores da rede doméstica 2 RaspBerry PI4.
Temos uma terceira máquina, está aqui (professor aponta a câmera para um Laptop) que vai fazer o papel da máquina atacante. 
Ela está na mesma rede “RedeISP070” mas poderia estar em outra rede e ela será a máquina do intruso, do atacante.
A máquina de trabalho está monitorando o ambiente através da unidade de monitoramento do .70.70 com uma câmera.
Vamos ver se está pingando .70.70 (professor digita “ping 192.168.70.70” no prompt de comando para verificar se a máquina está respondendo) está ok e a máquina do intruso também está ok (professor executa um ping na máquina do intruso no endereço 192.168.70.71).
O nosso cenário está ok mas na mesma rede mas repito, a máquina do intruso poderia ser uma máquina em uma outra rede.
Então vamos verificar quem está conectado nessa rede “RedeISP070”. 
Aqui no resultado do comando “iwconfig” ele me passa o endereço MAC do access point onde podemos utilizar no Airodump para verificar quais estações estão conectadas a este access point.
Então, primeiro utilizamos o Airmon para matar alguns processos que podem prejudicar a execução do Airodump com o comando “sudo airmon-ng check kill”. 
Preciso informar a senha de super usuário pois utilizei o sudo.
Ele matou esse processo “wpa_supliccant” que tem a ver com a rede sem fio.
E agora vamos executar o Airodump e já fazemos isso informando nossa interface wifi, ficando assim o comando “sudo airodump-ng wlp3s0” e aí apareceriam todas as redes no raio de ação e as estações de cada rede. 
Como não queremos ser invasivos vamos dizer ao Airodump acrescentando essa opção aqui onde queremos apenas as estações ligadas ao nosso ponto de acesso. Então o comando ficará assim “sudo airodump-ng --bssid 00:25:9c:85:31:c6 wlp3s0”.
Aparece apenas o nosso ponto de acesso e agora apareceu o primeiro endereço MAC “00:21:5c:4a:5a:07” que é esta máquina aqui (professor aponta a câmera para o Laptop citado anteriormente).
Não está mostrando nossa unidade de monitoramento pois ela acabou de cair. 
Vamos fazer uma limpeza aqui para poder startar nosso serviço direito utilizando o comando “sudo ifconfig wlp3s0 down”, vamos retomar o modo management com o comando “sudo ifconfig wlp3s0 mode managed”, vamos ligar a interface “sudo ifconfig wlp3s0 up” e vamos reiniciar o network-manager “sudo service network-manager restart”.
Agora tenho de volta às minhas redes e vamos conectar novamente na rede “.70”.
Vamos dar um ping no endereço “192.168.70.70”. Está no ar sim.
Vamos ver se ela retomou aqui o monitoramento, está funcionando.
Então vamos rodar novamente aqueles dois comandos “sudo airmon-ng check kill” e o “sudo airodump-ng --bssid 00:25:9c:85:31:c6 wlp3s0”.
Os comandos “ifconfig e wconfig” não são conteúdos para esta apresentação.
Está aqui “00:21:5c:4a:5a:07” é a máquina do intruso e a “b8:27:eb:7e:4d:62” é a unidade de monitoramento que não respondeu novamente.
Vamos fazer o seguinte, vou tentar dar um ping nela não respondeu. 
Claro, tudo bem. Não está respondendo pois estou com o Airodump rodando. 
No momento que eu matei os processos para rodar o Airodump eu fiquei sem conexão com a rede por isso que, excelente ter acontecido isso expor essa situação, no momento que estou rodando o Airodump estou no modo monitor e por isso não consigo acessar o computador de monitoramento. 
Já sabemos o endereço MEC da unidade de monitoramento (professor copia o endereço MAC da listagem) então vamos parar o Airodump e fazer a limpeza que fizemos antes que é desligar a interface de rede wifi “sudo ifconfig wlp3s0 down”, colocar ela em modo manager “sudo ifconfig wlp3s0 mode managed”, ligá-la “sudo ifconfig wlp3s0 up” e reiniciar o network-manager “sudo service network-manager restart”. 
Ele acaba reiniciando a rede cabeada também, vamos desligar a rede cabeada ficando apenas na rede wifi.
Retornou ao monitoramento de vídeo.
Excelente!
Falta agora fazermos o ataque nesta máquina aqui (professor mostra o Laptop citado anteriormente) mas vou apresentar os comandos na máquina de trabalho.
O comando é o seguinte, “sudo airplay-ng -0 0 -c b8:27:eb:7e:4d:62 -a 00:25:9c:85:31:c6 wlp3s0” onde o primeiro endereço MAC é o da máquina a ser atacada e o segundo é do ponto de acesso a ser utilizado para chegar na máquina atacada e o “-0 0” é onde eu digo o numero de pacotes de desautenticação que serão enviados para a rede. Colocando “0” o script ficará infinitamente enviando pacotes. 
Pronto, executei este comando na máquina intrusa e ela já está fazendo o ataque na máquina de monitoramento (professor aponta a câmera para o Laptop mostrando que o comando foi executado e o ataque está sendo feito).
Vejam que a unidade de monitoramento congelou (professor mostra a imagem vinda da câmera da unidade de monitoramento com a imagem congelada) mas veja que lá na máquina do intruso ele mandou alguns pacotes e parou e a máquina de monitoramento voltou a se comunicar pois foi interrompido o ataque. 
Porque foi interrompido o ataque?
Foi interrompido pois faltou um “e” no comando que executamos o ataque ficando o comando assim “sudo aireplay-ng -0 0 -c b8:27:eb:7e:4d:62 -a 00:25:9c:85:31:c6 wlp3s0” e faltou matar os processos auxiliares que podem estar interferindo na execução do Aireplay. 
É isso que eu vou fazer agora na máquina do intruso (professor irá executar o comando “sudo airmon-ng check kill”).
Pronto!
Feito isso, vai dar para ver a máquina do intruso efetuando o ataque na máquina de monitoramento (professor aponta a câmera para o Laptop que está fazendo o papel de intruso com os pacotes visivelmente sendo enviados pelo terminal). 
A máquina de monitoramento já está congelada novamente (professor aponta a câmera para o computador que faz o videomonitoramento mostrando a imagem congelada).
A máquina intrusa continua mandando pacotes onde antes ela tinha mandado uns 30 pacotes de ataques de desautenticação e agora ela permanece enviando os pacotes e a unidade de monitoramento permanece congelada (professor tenta atualizar a página do videomonitoramento mas sem sucesso).
Vamos fazer o seguinte, vamos interromper o ataque.
Ataque interrompido (professor mostra o Laptop com o envio de pacotes cancelado) e agora vamos ver como está nosso computador de videomonitoramento (professor retoma o acesso via navegador ao computador de videomonitoramento e atualiza a página) as vezes demora um pouquinho para ela conseguir voltar mas está aqui, voltou a funcionar.
Então, o estudo de caso do uso das ferramentas do Aircrack-ng.
Vimos também mas não fez parte do conteúdo desta apresentação o uso do ifconfig e iwconfig e ilustramos uma possibilidade de ataque.
Se eventualmente uma das câmeras do sistema de monitoramento não estiver mandando imagem, pode acontecer de alguém por perto estar atacando pois na nossa máquina do intruso ela está na mesma rede mas não necessariamente onde poderia estar em qualquer outra rede aqui no raio de ação. 
Bastaria ela rodar o Airodump e descobrir o endereço da minha máquina e do meu ponto de acesso e iniciar um ataque da mesma maneira.
É isso então. 
