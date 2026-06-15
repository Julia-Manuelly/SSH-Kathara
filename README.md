# Experimento SSH com Kathara

## 1. Descrição do Projeto

Este projeto tem como objetivo mostrar o funcionamento do protocolo SSH (Secure Shell) em uma rede virtual criada com o Kathara.

O experimento consiste em criar duas máquinas virtuais, uma atuando como cliente e outra como servidor. A partir disso, é realizada uma conexão remota segura utilizando SSH e uma captura dos pacotes trafegados na rede.

Com essa prática, é possível entender melhor conceitos importantes de redes de computadores, como comunicação cliente-servidor, uso do protocolo TCP, autenticação de usuários e criptografia dos dados transmitidos.

## 2. Estrutura da Rede

A rede utilizada possui dois dispositivos virtuais:

**client**

* Endereço IP: 192.168.1.10
* Função: realizar a conexão SSH

**server**

* Endereço IP: 192.168.1.20
* Função: disponibilizar o serviço SSH

Os dois dispositivos estão conectados à mesma rede virtual e conseguem se comunicar diretamente.

## 3. Pré-requisitos

Antes de executar o experimento é necessário possuir:

* Docker instalado e funcionando;
* Kathara instalado;
* Wireshark para análise das capturas;
* Linux ou Windows com WSL.

Para verificar se o Docker está instalado:

```bash
docker --version
```

Para verificar se o Kathara está instalado:

```bash
kathara --version
```

## 4. Arquivos Utilizados

O projeto é composto pelos seguintes arquivos:

### lab.conf

Define a topologia da rede virtual.

### client.startup

Configura automaticamente o endereço IP do cliente.

### server.startup

Configura o endereço IP do servidor e inicia o serviço SSH.

### captura_ssh.pcap

Arquivo gerado pelo tcpdump contendo os pacotes capturados durante o experimento.

## 5. Configuração do Ambiente

### Arquivo lab.conf

```txt
client[0]="A"
server[0]="A"
```

### Arquivo client.startup

```bash
ip addr add 192.168.1.10/24 dev eth0
ip link set eth0 up
```

### Arquivo server.startup

```bash
ip addr add 192.168.1.20/24 dev eth0
ip link set eth0 up

apt update
apt install -y openssh-server

mkdir -p /run/sshd

/usr/sbin/sshd
```

## 6. Passo a Passo do Experimento

### Passo 1 – Iniciar o laboratório

Abrir o terminal na pasta do projeto e executar:

```bash
kathara lstart
```

Esse comando cria os dispositivos virtuais e configura a rede.

### Passo 2 – Acessar o servidor

Abrir um terminal e executar:

```bash
kathara connect server
```

### Passo 3 – Criar um usuário

Dentro do servidor executar:

```bash
useradd -m aluno
```

Definir uma senha:

```bash
passwd aluno
```

Foi utilizada a senha:

```text
123456
```


### Passo 4 – Acessar o cliente

Abrir outro terminal e executar:

```bash
kathara connect client
```

### Passo 5 – Testar a comunicação

No cliente executar:

```bash
ping 192.168.1.20
```

Se aparecer resposta do servidor, significa que a rede está funcionando corretamente.

### Passo 6 – Iniciar a captura de pacotes

No servidor executar:

```bash
tcpdump -i eth0 -w captura_ssh.pcap
```

O comando ficará executando em segundo plano capturando todo o tráfego da rede.

### Passo 7 – Realizar a conexão SSH

No cliente executar:

```bash
ssh aluno@192.168.1.20
```

Na primeira conexão aparecerá uma mensagem pedindo confirmação da chave do servidor.

Digite:

```text
yes
```

Depois informe a senha criada anteriormente.

Se a autenticação for realizada com sucesso, o usuário terá acesso remoto ao servidor.

### Passo 8 – Executar alguns comandos

Já conectado ao servidor, executar:

```bash
whoami
pwd
ls
```

Esses comandos servem para gerar tráfego na conexão SSH.

Para encerrar a sessão:

```bash
exit
```

### Passo 9 – Encerrar a captura

Voltar ao terminal do servidor e pressionar:

```text
Ctrl + C
```

Será exibida uma mensagem semelhante a:

```text
136 packets captured
```

Isso indica que os pacotes foram capturados com sucesso.


### Passo 10 – Verificar o arquivo de captura

No servidor executar:

```bash
find / -name "*.pcap"
```

Resultado esperado:

```text
/captura_ssh.pcap
```

## 7. Análise dos Resultados

O arquivo de captura pode ser aberto no Wireshark para análise do tráfego.

Utilizando o filtro:

```text
tcp.port == 22
```

é possível visualizar os pacotes relacionados ao SSH.

Durante a análise podem ser observados:

* Three-Way Handshake do TCP;
* Troca de versões do SSH;
* Negociação dos algoritmos de criptografia;
* Troca de chaves;
* Comunicação criptografada entre cliente e servidor.

Essas etapas mostram como o SSH cria uma conexão segura para proteger os dados transmitidos.

## 8. Conclusão

O experimento permitiu verificar na prática o funcionamento do protocolo SSH em uma rede simulada com o Kathara.

Foi possível configurar um servidor SSH, realizar conexões remotas, capturar os pacotes da comunicação e analisar o tráfego utilizando o Wireshark.

Os resultados demonstraram que o SSH oferece uma forma segura de acesso remoto, utilizando criptografia para proteger os dados durante toda a comunicação.
ite capturar o tráfego gerado, reforçando a compreensão sobre segurança, criptografia e comunicação em redes de computadores.
