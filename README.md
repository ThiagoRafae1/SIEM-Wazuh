# 🔐 Projeto SIEM com Wazuh

## 📖 Visão Geral

Este projeto demonstra a implementação de um ambiente **SIEM (Security Information and Event Management)** utilizando o Wazuh para monitoramento de eventos de segurança, detecção de ameaças e análise de logs em tempo real.

O laboratório foi desenvolvido em ambiente virtual com o objetivo de simular ataques reais e validar a capacidade de detecção do sistema.

---

## 🎯 Objetivos

* Monitorar logs de sistemas Linux
* Detectar ataques de força bruta (SSH)
* Gerar alertas em tempo real
* Analisar eventos de segurança através de dashboards

---

## 🧰 Tecnologias Utilizadas

* Wazuh
* Ubuntu Server
* VirtualBox

---

## 🏗️ Arquitetura do Projeto

O ambiente foi estruturado da seguinte forma:

* 1 Servidor SIEM (Wazuh + Elasticsearch + Kibana)
* 1 Máquina alvo (Linux com SSH ativo)
* Rede interna para simulação de ataques

📌 *Imagem da arquitetura disponível em:*
`docs/arquitetura.png`

---

## 🖧 Infraestrutura de Rede (DHCP + NAT)

O ambiente do laboratório SIEM foi projetado com segmentação de rede para simular cenários reais.

### 🔧 Componentes

* Servidor Ubuntu atuando como:

  * DHCP Server
  * Gateway (NAT)
* Rede interna isolada para testes
* Distribuição automática de IP para clientes

### 🌐 Topologia

* `enp0s3` → WAN (Internet)
* `enp0s8` → LAN (192.168.10.0/24)

### 📡 DHCP

* Range: 192.168.10.10 - 192.168.10.50
* Gateway: 192.168.10.1
* DNS: 8.8.8.8 / 8.8.4.4

### 🔁 NAT

* Masquerade com iptables
* Permite acesso à internet para clientes da LAN

### 📊 Integração com SIEM

Logs monitorados pelo Wazuh:

* DHCP requests (DHCPDISCOVER)
* DHCP leases (DHCPACK)
* Possíveis comportamentos suspeitos na rede

📄 Documentação completa: `docs/dhcp-server.md`

---

## ⚙️ Configuração do Ambiente

### 1. Instalação do Wazuh

* Instalação do servidor Wazuh
* Configuração do Elasticsearch e Kibana

### 2. Configuração do Agente

* Instalação do agente Wazuh na máquina alvo
* Conexão com o servidor SIEM

### 3. Coleta de Logs

* Monitoramento do arquivo `/var/log/auth.log`
* Integração com Filebeat

---

## 🚨 Simulação de Ataques

### 🔹 Ataque: SSH Brute Force

Ferramenta utilizada:

* Hydra

Execução do ataque:

```bash
hydra -l usuario -P senha.txt ssh://IP-ALVO
```

Objetivo:

* Gerar múltiplas tentativas de login falhadas
* Validar detecção pelo SIEM

---

## 📊 Resultados e Evidências

Durante a simulação, o SIEM foi capaz de:

* Detectar tentativas de login inválidas
* Gerar alertas automáticos
* Identificar o IP atacante

📌 Evidências disponíveis em:

* `docs/evidencias/ataque-bruteforce.png`
* `docs/evidencias/alerta-siem.png`

---

## 🔍 Regras de Detecção

Foi utilizada regra padrão do Wazuh para detecção de brute force, com possibilidade de customização.

Exemplo de melhoria:

* Criação de regras personalizadas em XML

---

## 📄 Relatório Completo

O relatório detalhado do projeto pode ser acessado em:

📎 `docs/relatorio.pdf`

---

## 🔐 Segurança das Informações

Para garantir boas práticas de segurança, todos os dados sensíveis foram anonimizados neste projeto.

Não são expostos:

* Endereços IP públicos
* Credenciais ou senhas
* Informações pessoais do ambiente real

Os dados apresentados são simulados ou pertencem a ambientes controlados de laboratório.

---

## 🛡️ Possíveis Melhorias

* Implementação de resposta automática (bloqueio de IP)
* Integração com Threat Intelligence
* Monitoramento de outros serviços (HTTP, FTP)
* Dashboard personalizado no Kibana

---

## 👨‍💻 Autor

Projeto desenvolvido por [Thiago Rafael Belmaia]

---

## 📌 Observações

Este projeto tem fins educacionais e foi desenvolvido em ambiente controlado para estudo de segurança da informação.
