# DIAGRAMA DE REDE 

![alt text](</docs/imagens/diagrama/DiagramaderedePNg.png>)

### **Matriz SP (80 funcionários)**  
- **VLANs:**  
  - 10 (Administrativo): `10.0.10.0/24`  
  - 20 (Financeiro): `10.0.20.0/24`  
  - 30 (TI): `10.0.30.0/24`  
  - 40 (Atendimento): `10.0.40.0/24`  
  - 50 (Visitantes): `10.0.50.0/24`  
- **Servidores:** ERP, arquivos, impressão (`10.8.99.124`).  
- **Switch Core:** Centraliza tráfego, conecta VLANs, firewall e VPNs.  

### **Filial RJ (38 funcionários)**  
- Rede: `10.1.0.0/24`  
- Conectada via VPN site-to-site.  

### **Filial MG (18 funcionários)**  
- Rede: `10.1.10.0/24`  
- Acesso remoto seguro via VPN.  

### **Fluxos de tráfego:**  
- **Interno (VLANs):** Roteamento pelo Switch Core.  
- **Internet:** Firewall aplica NAT e regras de segurança.  
- **Filiais:** Túneis VPN criptografados.  
- **Nuvem:** Firewall protege acesso a Office 365/CRM.  

---

# JUSTIFICATIVAS TÉCNICAS  

- **VLANs:** Isolamento de setores, controle de acessos, redução de riscos.  
- **VPN site-to-site:** Comunicação segura entre unidades.  
- **Firewall com logging:** Monitoramento e bloqueio de ameaças.  
- **Rede para visitantes:** Evita acesso a dados sensíveis.  
- **Nuvem:** Produtividade com segurança (via firewall).  

---


# Explicação do Diagrama de Rede e Rotas

## 1. Núcleo da Rede (Switch Core)

O **Switch Core** centraliza o tráfego de rede na **Matriz São Paulo** e está conectado a:

- **Firewall**, que fornece acesso à Internet;
- **VLANs internas** (TI, Financeiro, etc.);
- **Servidores** (ERP, arquivos, impressão);
- **Conexões VPN site-to-site** com as filiais RJ e MG.

---

## 2. Tráfego Dentro da Matriz SP

### Comunicação entre VLANs (interna)

As VLANs 10, 20, 30, 40 e 50 estão ligadas ao Switch Core, que possui capacidade de roteamento (camada 3). Isso permite:

- Um computador da **VLAN 10** (`10.0.10.10`) se comunicar com um servidor na **VLAN 30** (`10.0.30.10`);
- Um funcionário do **Financeiro** acessar arquivos em `10.0.60.1` na **VLAN ERP**.

O roteamento interno entre as sub-redes VLANs viabiliza essas conexões com segurança e organização.

---

## 3. Tráfego com a Internet

### Saída para a Internet

- Todos os pacotes destinados à internet saem do **Switch Core** e são encaminhados ao **firewall**.
- O **firewall realiza NAT** (Network Address Translation), convertendo os IPs internos para IPs públicos válidos.
- O firewall aplica **regras de segurança**, restringindo o acesso de VLANs sensíveis (como TI e Financeiro).

---

## 4. Tráfego com as Filiais (VPN)

A VPN é configurada no modo **site-to-site**, criando túneis criptografados permanentes entre a matriz e cada filial.

### Comunicação com a Filial RJ

- **Rede**: `10.1.0.0/24`
- Exemplo: Um host da VLAN 30 na matriz (`10.0.30.10`) acessa um host da filial RJ (`10.1.0.10`):
  - O pacote sai do host → Switch Core → túnel VPN → chega à filial → é entregue ao destino.

### Comunicação com a Filial MG

- **Rede**: `10.1.10.0/24`
- O tráfego segue o mesmo fluxo da filial RJ:
  - Host da matriz → Switch Core → túnel VPN → destino na filial MG

---

## 5. Integração com Sistemas em Nuvem (Office 365, CRM)

A rede foi planejada para uso seguro de serviços em nuvem, como:

- **Office 365** (e-mails, documentos, videoconferências)
- **Sistemas de CRM** (gestão de clientes)

### Como funciona:

- O tráfego para a nuvem passa antes pelo **firewall**, que:
  - Aplica **regras de segurança**
  - Protege os dados contra **acessos não autorizados** e **ameaças externas**

### Benefícios:

- Acesso a ferramentas de produtividade e gestão a qualquer momento (in loco ou remoto)
- Aumento da mobilidade, colaboração e eficiência
- Redução da dependência de servidores locais
- Facilidade de atualizações, backups e suporte técnico

---

## Resumo Simplificado

O **Switch Core (núcleo da rede)**:

- Conecta todas as VLANs da **matriz SP**
- Conecta os **servidores** e o **firewall**
- Realiza **roteamento entre VLANs**
- Se comunica com as **filiais RJ e MG via VPN**

Com isso, qualquer máquina autorizada pode acessar recursos internos ou remotos, mantendo a **segurança, desempenho e controle da rede**.

