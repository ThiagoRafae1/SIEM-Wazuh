# 🖧 Servidor DHCP + NAT - Projeto SIEM

## 📌 Objetivo
Este projeto tem como objetivo configurar um servidor **DHCP em ambiente Linux (Ubuntu Server)** para fornecer endereçamento IP automático em uma **rede interna isolada**, utilizada para testes de um ambiente **SIEM (Wazuh)**.

---

## 🧱 Ambiente

- **Sistema Operacional:** Ubuntu Server
- **Interface WAN:** `enp0s3` (DHCP - acesso à internet)
- **Interface LAN:** `enp0s8` (IP fixo - rede interna)

---

## 🌐 Topologia de Rede

- Rede externa (internet) via DHCP
- Rede interna isolada: `192.168.10.0/24`
- Servidor atua como:
  - DHCP Server
  - Gateway (NAT)

---

## ⚙️ Configuração de Rede (Netplan)

Arquivo:
```bash
/etc/netplan/50-cloud-init.yaml
```

Configuração:
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: true
    enp0s8:
      addresses:
        - 192.168.10.1/24
```

Aplicar configuração:
```bash
sudo netplan apply
```

---

## 📦 Instalação do DHCP

```bash
sudo apt update
sudo apt install isc-dhcp-server -y
```

---

## 🔧 Definição da Interface DHCP

Arquivo:
```bash
/etc/default/isc-dhcp-server
```

Configuração:
```bash
INTERFACESv4="enp0s8"
```

---

## 🗂️ Configuração do Escopo DHCP

Arquivo:
```bash
/etc/dhcp/dhcpd.conf
```

Configuração:
```bash
default-lease-time 600;
max-lease-time 7200;
authoritative;

subnet 192.168.10.0 netmask 255.255.255.0 {
  range 192.168.10.10 192.168.10.50;
  option routers 192.168.10.1;
  option domain-name-servers 8.8.8.8, 8.8.4.4;
}
```

---

## 🔄 Reiniciando e Validando o Serviço

```bash
sudo systemctl restart isc-dhcp-server
sudo systemctl status isc-dhcp-server
```

✔️ O serviço deve estar como:
- `enabled`
- `active (running)`

---

## 🔀 Ativação do IP Forwarding

Editar:
```bash
/etc/sysctl.conf
```

Descomentar:
```bash
net.ipv4.ip_forward=1
```

Aplicar:
```bash
sudo sysctl -p
```

---

## 🔐 Configuração de NAT com iptables

Instalar persistência:
```bash
sudo apt install iptables-persistent -y
```

Regra de mascaramento:
```bash
sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
```

Salvar regras:
```bash
sudo netfilter-persistent save
```

Verificar:
```bash
cat /etc/iptables/rules.v4
```

---

## 🧪 Testes

### 🔹 Renovar IP no cliente (Windows)

```bash
ipconfig /release
ipconfig /renew
```

### 🔹 Testar conectividade

```bash
ping 8.8.8.8
ping google.com
```

✔️ Esperado:
- Cliente recebe IP automaticamente
- Consegue acessar internet
- DNS funcionando corretamente

---

## 🚀 Resultado Final

- DHCP funcional distribuindo IPs
- Rede interna isolada operando
- NAT permitindo acesso à internet
- Ambiente pronto para integração com SIEM (Wazuh)

---

## 📎 Possíveis Melhorias

- Implementar firewall com regras mais restritivas
- Integrar com proxy transparente (Squid + e2guardian)
- Monitoramento com Wazuh mais detalhado
- Logs centralizados do DHCP

---

## 👨‍💻 Autor
Projeto desenvolvido para fins de estudo e demonstração prática em ambiente de segurança da informação.

