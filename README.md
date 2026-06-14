Experimento SSH com Kathara
**1. Descrição do projeto**

Este projeto tem como objetivo simular uma rede computacional utilizando o ambiente Kathara, permitindo a criação de máquinas virtuais interconectadas para análise do funcionamento do protocolo SSH (Secure Shell).

A prática envolve a configuração de um cliente e um servidor Linux, estabelecimento de comunicação via rede local simulada, autenticação remota e captura de tráfego de rede para análise posterior.

O experimento permite observar na prática conceitos de redes de computadores como comunicação cliente-servidor, funcionamento do protocolo TCP, segurança em comunicação remota e uso de ferramentas de captura de pacotes.

**2. Estrutura do ambiente**

O laboratório é composto por dois nós virtuais:

client: máquina cliente responsável por iniciar conexões na rede
server: máquina servidor onde o serviço SSH é executado

Ambos os dispositivos estão conectados em uma rede virtual no segmento 192.168.1.0/24, permitindo comunicação direta entre eles.

**3. Pré-requisitos**

Para executar o experimento, é necessário ter instalado:

Docker (para execução dos containers)
Kathara Framework (para simulação da rede)
Sistema operacional compatível (Linux ou Windows com WSL)
Permissões de administrador para execução dos serviços de rede
4. Arquivos do laboratório

O ambiente é composto pelos seguintes arquivos:

lab.conf: define a topologia da rede e os dispositivos
client.startup: script de inicialização do cliente
server.startup: script de inicialização do servidor
captura_ssh.pcap: arquivo gerado com a captura de tráfego

**5. Configuração da rede**

Durante a inicialização do laboratório, são executadas configurações automáticas nos nós:

No cliente:

configuração do endereço IP 192.168.1.10
ativação da interface de rede

No servidor:

configuração do endereço IP 192.168.1.20
instalação do serviço OpenSSH Server
inicialização do daemon SSH

Essas configurações permitem que os dois dispositivos se comuniquem diretamente dentro da rede simulada.

**6. Execução do experimento**
**6.1 Inicialização do ambiente**

O laboratório é iniciado com o comando:

kathara lstart

Este comando cria os containers, configura a topologia de rede e inicia os dispositivos virtuais.

**6.2 Teste de conectividade**

No cliente, é realizado um teste de comunicação com o servidor:

ping 192.168.1.20

Este teste verifica se há comunicação entre os nós e se a rede está corretamente configurada.

**6.3 Acesso remoto via SSH**

Após a confirmação da conectividade, o cliente realiza acesso remoto ao servidor utilizando o protocolo SSH:

ssh aluno@192.168.1.20

Durante esse processo ocorre autenticação por senha e, após sucesso, é estabelecida uma sessão remota segura, permitindo a execução de comandos no servidor.

**6.4 Captura de tráfego de rede**

No servidor, é utilizada a ferramenta tcpdump para capturar os pacotes de rede:

tcpdump -i eth0 -w captura_ssh.pcap

A captura registra toda a comunicação entre cliente e servidor durante o uso do SSH, gerando um arquivo no formato .pcap para análise posterior.

**6.5 Verificação do arquivo de captura**

Após a execução do experimento, o arquivo gerado é verificado com:

ls
find / -name "*.pcap"

Confirmando a existência do arquivo captura_ssh.pcap, que contém todo o tráfego capturado.

**7. Análise dos resultados**

O arquivo de captura pode ser analisado em ferramentas como Wireshark, permitindo observar:

Estabelecimento da conexão TCP (Three-Way Handshake)
Troca inicial de mensagens do SSH
Negociação de chaves criptográficas
Tráfego criptografado durante a sessão
Comunicação segura entre cliente e servidor

A análise demonstra que, após o estabelecimento da conexão, os dados trafegam de forma criptografada, impedindo a leitura direta do conteúdo.

O experimento demonstra o funcionamento prático do protocolo SSH em um ambiente simulado. A utilização do Kathara permite a criação de uma rede controlada, possibilitando a análise detalhada da comunicação entre dispositivos.

Além disso, o uso do tcpdump permite capturar o tráfego gerado, reforçando a compreensão sobre segurança, criptografia e comunicação em redes de computadores.
