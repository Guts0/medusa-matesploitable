# 🛡️ Projeto de Laboratório de Cibersegurança: Força Bruta e Mitigação

## Visão Geral do Desafio

Este projeto documenta a configuração de um laboratório de testes de penetração (pentest) em um ambiente isolado, focando na **simulação de ataques de força bruta** utilizando o **Kali Linux** e a ferramenta **Medusa** contra alvos intencionalmente vulneráveis (**Metasploitable 2** e **DVWA**).

O objetivo principal não é a execução do ataque, mas a **compreensão das vulnerabilidades** e o **exercício prático de medidas de prevenção (Hardening)** para mitigar esses riscos.

## 🎯 Objetivos de Aprendizado

1.  **Configurar um ambiente de laboratório seguro e isolado** (Host-Only Network).
2.  **Compreender a lógica operacional** de ataques de força bruta e *password spraying*.
3.  **Analisar a vulnerabilidade** de serviços comuns (FTP, SMB, Web Login).
4.  **Propor e documentar medidas de mitigação** detalhadas para cada vetor de ataque.

---

## 💻 1. Configuração do Ambiente

O laboratório foi construído usando **VirtualBox** para garantir um ambiente **isolado** e seguro.

### Componentes:

| Máquina Virtual | Sistema Operacional | Função no Teste | Endereço IP (Exemplo) |
| :--- | :--- | :--- | :--- |
| **Kali Linux** | Debian (Rolling) | Atacante (Medusa) | `192.168.56.10` |
| **Metasploitable 2** | Ubuntu (Vulnerável) | Alvo (FTP, SMB, Shell) | `192.168.56.20` |
| **DVWA** (Opcional: em uma VM separada ou no Metasploitable 2) | Aplicação Web Vulnerável | Alvo (Formulário Web) | `192.168.56.20:80/dvwa` |

### Topologia de Rede:

A comunicação é feita estritamente através de uma placa de rede configurada como **"Rede Somente ao Host (Host-Only)"**. Isso impede que o tráfego de teste, incluindo as simulações de ataque, saia para a rede local ou para a Internet.

---

## 🔎 2. Cenários de Ataque Simulados e Vulnerabilidades

Esta seção resume os vetores de ataque estudados e suas vulnerabilidades inerentes.

### A. Força Bruta em FTP (Protocolos de Rede)

* **Vulnerabilidade:** Ausência de controle de taxa (rate limiting) ou bloqueio de conta após múltiplas tentativas falhas.
* **Ferramenta Utilizada:** Medusa (Módulo FTP).
* **Wordlist Exemplo:** Lista simples de 10 senhas comuns (`user.txt` para usuários, `pass.txt` para senhas).

### B. Automação em Formulário Web (DVWA)

* **Vulnerabilidade:** Falta de mecanismos de proteção contra bots, como **CAPTCHA** ou **Honeypots**, permitindo a repetição automática de requisições HTTP POST.
* **Ferramenta Utilizada:** Medusa (Módulo HTTP GET/POST).

### C. Password Spraying em SMB

* **Vulnerabilidade:** Contas de usuário com senhas fracas combinadas com uma política de bloqueio de conta que é fácil de contornar.
* **Técnica:** Tentar uma única senha comum (ex: `Senha123!`) contra uma lista de usuários para evitar o bloqueio de contas individuais.

---

## 💡 3. Medidas de Mitigação (Hardening)

A parte mais crítica do projeto é o **Hardening** dos serviços, transformando as vulnerabilidades em defesas robustas.

### ✅ Mitigação para Serviços de Rede (FTP/SMB)

| Medida de Defesa | Descrição da Implementação |
| :--- | :--- |
| **Fail2Ban** | Instalar e configurar o **Fail2Ban** para monitorar logs e adicionar regras de **iptables** automaticamente, banindo o IP de origem após 3-5 falhas de login em um período curto. |
| **Políticas de Senha** | Impor senhas com **mínimo de 12 caracteres**, complexidade (letras, números, símbolos) e exigir rotação regular. |
| **MFA/SFTP** | Desabilitar FTP por ser inseguro e migrar para **SFTP** ou **SSH/SCP**, utilizando **Autenticação de Dois Fatores (MFA)** para acesso. |

### ✅ Mitigação para Aplicações Web (DVWA)

| Medida de Defesa | Descrição da Implementação |
| :--- | :--- |
| **CAPTCHA / reCAPTCHA** | Implementar um desafio humanizado na página de login para frustrar scripts de automação. |
| **Rate Limiting (WAF)** | Configurar o **Web Application Firewall (WAF)** ou o servidor web (Nginx/Apache) para limitar o número de requisições de login por IP (`429 Too Many Requests`). |
| **Mensagem de Erro Genérica** | Retornar uma única mensagem de erro (`Usuário ou senha inválidos`) para impedir a enumeração de usuários. |

---

## 🚀 Lições Aprendidas e Conclusão

Este desafio reforçou que a **segurança de um sistema é tão forte quanto seu elo mais fraco**.

1.  **Isolamento é Fundamental:** Testes de segurança devem sempre ocorrer em um ambiente isolado para evitar qualquer impacto real.
2.  **Defesa em Profundidade:** Nenhuma mitigação é suficiente sozinha. A combinação de **Fail2Ban (camada de rede)**, **Políticas de Senha (camada de usuário)** e **MFA (camada de autenticação)** é essencial.
3.  **Ferramentas como o Medusa** são ferramentas de testes legítimos (para Pentest), mas exigem uma **compreensão ética e legal** de seu poder.

---

## 🛠️ Comandos (Exemplos para Documentação)

*Inclua aqui os comandos reais utilizados, as wordlists criadas, e os logs de sucesso/falha do seu laboratório. Sugestão: Use blockquotes para os comandos.*

> `Exemplo de comando de teste de conectividade: ping 192.168.56.20`
>
> `Exemplo de sintaxe (conceitual) do Medusa (Não é o comando real de ataque): medusa -h [IP_ALVO] -u [USUARIO] -P [WORDLIST] -M [SERVICO]`
