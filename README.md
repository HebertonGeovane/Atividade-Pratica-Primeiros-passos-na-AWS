# Atividade-Pratica-Primeiros-passos-na-AWS ![AWS Logo](https://d0.awsstatic.com/logos/powered-by-aws.png)

Estamos concluindo as duas primeiras semanas da nossa jornada  Introdução a Nuvem AWS , finalizando os módulos:  Introdução à Computação em Nuvem | Introdução ao Linux 

Nesta atividade prática, vamos utilizar os conhecimentos adquiridos para criar uma instância EC2 , configurá-la como um servidor web e publicar uma página personalizada.

# Publicando sua primeira página web na Nuvem

## 📚 Objetivo

- Criar uma instância EC2 com Amazon Linux 2023 (Free Tier)

- Configurar segurança básica (porta SSH e HTTP)

- Conectar via terminal (Linux ou Windows)

- Instalar e iniciar Apache

- Criar uma página HTML com imagem da AWS

- Acessar sua página na web

## O que é o Amazon EC2?

O **Amazon EC2 (Elastic Compute Cloud)** é um serviço de computação da AWS que permite executar servidores virtuais sob demanda.

Com o EC2, você pode:

- Escolher o sistema operacional (Linux, Windows etc.)
- Controlar recursos como CPU, memória, rede e disco
- Escalar aplicações com flexibilidade
- Acessar remotamente via SSH
- Hospedar sites, APIs, bancos de dados e mais

🔗 [Acesse a documentação oficial do Amazon EC2](https://docs.aws.amazon.com/pt_br/ec2/)

## 🛠️ Passo a Passo
### 1. Criar instância EC2 (Web Server)
Acesse o Console da AWS

1️⃣ Vá em EC2 > Launch Instance

2️⃣ Nome da instância: `web-server`

3️⃣ Escolher AMI: Amazon Linux 2023 (x86, Free Tier)

4️⃣ Tipo de instância: t2.micro (Free Tier)

5️⃣ Par de chaves (.pem):
Selecione existente ou crie um novo.
Baixe o arquivo .pem e copie para uma pasta de fácil acesso

📸 print dessa etapa!

![image - 2025-07-04T101146 263](https://github.com/user-attachments/assets/e90f2230-0e03-401c-be81-fe5f1383fa8a)

![image - 2025-07-04T101533 607](https://github.com/user-attachments/assets/c5287fcb-f85e-4677-a5e6-16f94619ad1f)
--
### 2. Configurar grupo de segurança (Security Group)

em Network settings editar  > mantenha VPC Default ou escolha VPC Lab 

1️⃣ Criar novo grupo chamado: `security-group-web-server`
    Description: `security-group-for-web-server`
    
2️⃣ Permitir portas:

SSH (porta 22) – para acessar a instância

HTTP (porta 80) – para acessar via navegador

📸 Tire print tela de regras de entrada (inbound rules).

![image - 2025-07-04T102739 475](https://github.com/user-attachments/assets/f6a74c81-a5de-4e99-bf42-82b95c9af485)

![image - 2025-07-04T103618 064](https://github.com/user-attachments/assets/99ce16b7-8c05-44f8-aeb3-a0855364f1b5)

Confirmar configurações e lançar instância
Verifique o resumo
Clique em Launch Instance

![image - 2025-07-04T115259 296](https://github.com/user-attachments/assets/8821b05d-7af1-4a15-9f79-ecb8d13e7254)

### 3. Verificar status da instância
Acesse EC2 > Instances

Confirme:

Name > `webserver`

Instance State: `Running`

Status Check: `2/2 passed`

Verifique aba Security:

Porta `22` (SSH) e `80` (HTTP) habilitadas

📸 print mostrando os status e as portas abertas.

![image - 2025-07-04T120441 418](https://github.com/user-attachments/assets/f2efbdbe-6ca6-47fe-a868-dfad2b297170)

### 4. Acesso via SSH
> vá em Connect 
> SSH client
> 
🔸 Para usuários Linux/macOS (Terminal):

> Vá para o diretório onde salvou  o arquivo.pem abir Terminal
>

```
cd ~/chaves
chmod 400 sua-chave.pem
ssh -i "sua-chave.pem" ec2-user@<seu-endereço-público> ou <Public DNS>
```

🔸 Para usuários Windows (PowerShell):
> Vá para o diretório onde salvou  o arquivo.pem abir Terminal
>

```
ssh -i "sua-chave.pem" ec2-user@<seu-endereço-público>
```
📸 print da conexão bem-sucedida.

![image - 2025-07-04T121755 768](https://github.com/user-attachments/assets/cddda76c-fa0e-42cf-95fc-d4f1039b3e3b)

![image - 2025-07-04T121947 945](https://github.com/user-attachments/assets/798a78d3-0524-4776-8094-5c6ad47d0a3c)

![image - 2025-07-04T122634 519](https://github.com/user-attachments/assets/a54d6ddc-a4ea-4853-a856-1f9434d01a7f)

### 5.  Instalar e configurar Apache

```
sudo yum update -y
sudo yum install -y httpd
```
![image - 2025-07-04T123033 864](https://github.com/user-attachments/assets/5cc23ce7-2d5c-4519-83e0-953fe41ce6c6)


### 6. Criar diretório e permissões

```
sudo mkdir -p /var/www/html
sudo chown ec2-user /var/www/html
```
### 7. Criar página HTML com imagem da AWS

Obs* Poderá mudar o título em *< title >* a descrição em *< h1 >*  há vontade.

```
cat <<EOF > /var/www/html/index.html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>BRGOV1 - AWS re/Start</title>
  <style>
    body {
      background-color: #121212;
      font-family: Arial, sans-serif;
      color: #FFFFFF;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
    }
    h1 {
      font-size: 28px;
      margin-top: 20px;
    }
    img {
      width: 220px;
    }
  </style>
</head>
<body>
  <a href="https://aws.amazon.com/what-is-cloud-computing" target="_blank">
    <img src="https://d0.awsstatic.com/logos/powered-by-aws-white.png" alt="Powered by AWS Cloud Computing">
  </a>
  <h1>BRGOV1 melhor turma de AWS re/Start de 2025</h1>
</body>
</html>
EOF

```
![image - 2025-07-04T124442 736](https://github.com/user-attachments/assets/0eeab6e0-2a42-4a3d-b234-6098b699a84a)

### 8. Iniciar o Apache 

```
sudo systemctl start httpd
sudo systemctl enable httpd
```
![image - 2025-07-04T124711 672](https://github.com/user-attachments/assets/21db55a9-2bc0-456f-96c2-a1f46544581c)

### 9. Verificar no navegador

Acesse:
```
http://<seu-endereço-público> ou <Public DNS>
```
📸 Tire print da sua página publicada na web!

![image - 2025-07-04T124933 025](https://github.com/user-attachments/assets/4de94952-ee54-4bf7-93a7-1f9c47ec3206)

### O que aprendemos nesta atividade?

- Criar e configurar uma instância EC2

- Entender conceitos de AMI, tipo de instância, chave .pem e grupo de segurança

- Utilizar comandos Linux reais (CLI)

- Instalar e configurar Apache (servidor web)

- Publicar uma página HTML estática na nuvem

### Por que isso é importante?

Essa atividade representa o primeiro contato prático com a AWS, um passo essencial para:

> Desenvolver habilidades reais em cloud computing
> 
> Avançar na trilha rumo à certificação AWS Cloud Practitioner
> 
> Construir sua autonomia e confiança no uso da nuvem

# Tecnologias Utilizadas

<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg" alt="AWS" width="80" height="60"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/linux/linux-original.svg" alt="Linux" width="60" height="60"/>
  <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/bash/bash-original.svg" alt="Bash" width="60" height="60"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/apache/apache-original.svg" alt="Apache" width="60" height="60"/>
</div>

---

### 👨‍🏫 Criado por [Heberton Geovane](https://www.linkedin.com/in/heberton-geovane/)
