# 🚀 Laboratório: Monitoramento e Auditoria com CloudWatch e CloudTrail na AWS

![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/monitoringDev.jpg)

**Autor:** Halley Veras  
**Curso: Developer – Escola da Nuvem** 
**Objetivo:** Criar um ambiente de monitoramento e auditoria usando Amazon CloudWatch, CloudTrail, SNS e S3 na AWS.

---

## 🎯 Objetivos do Laboratório

- 📊 Criar alarme CloudWatch para monitorar uso de CPU em instância EC2.
- 🔔 Configurar Amazon SNS para notificações por e-mail.
- 🔍 Habilitar o CloudTrail para rastreamento de atividades na conta.
- 🪵 Visualizar logs via S3 e CloudWatch Logs.
- 💻 Gerar carga com a ferramenta `stress`.

---

## 🧰 Pré-requisitos

- Conta AWS com permissões para EC2, CloudWatch, CloudTrail, SNS e S3.
- Permissões IAM:
  - `CloudWatchFullAccess`
  - `AWSCloudTrail_FullAccess`
  - `AmazonSNSFullAccess`
  - `AmazonS3FullAccess`
  - `AmazonEC2FullAccess`
- Navegador Web
- Par de chaves SSH (.pem)

---

## 🪜 Etapas do Laboratório

### ✅ Passo 1 – Criar a Instância EC2

1. Acesse o **console do EC2**.
2. Clique em **Launch Instance** e configure:
   - **Nome:** `Instancia-Teste-CloudWatch`
   - **AMI:** Amazon Linux 2
   - **Tipo:** `t2.micro`
   - **Key pair:** Selecione ou crie uma
   - **VPC/Subnet:** Use a padrão
   - **Public IP:** Habilitado
   - **Security Group:** `SG-Teste-CloudWatch-SeuNome` com porta SSH aberta para seu IP
3. Anote o **Instance ID**.

![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_13-53.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_13-55.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_13-56.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_13-59.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-00.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-04.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-04_1.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-05.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-08.png)

---

### 📈 Passo 2 – Criar Alarme no CloudWatch

1. Acesse o **console do CloudWatch**.
2. Vá até `Alarms` > `Create alarm`.
3. Em métricas:
   - Selecione `EC2 > Per-Instance Metrics`
   - Pesquise pelo **Instance ID**
   - Selecione `CPUUtilization`
4. Configure:
   - **Threshold:** maior que `70`
   - **Período:** 5 minutos
   - **Alarm name:** `AlarmeCPU-Instancia-SeuNome`
   - Configure ação para enviar e-mail via **SNS topic** (crie um novo: `SNS-SeuNome`)
5. **Confirme a assinatura no seu e-mail.**

![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-09.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-10.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-10_1.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-11.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-14.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-15.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-18.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-18_1.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-22.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-23.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-25.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-25_1.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_14-26.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_16-37.png)

---

### 🕵️‍♂️ Passo 3 – Habilitar CloudTrail

1. Acesse o **console do CloudTrail**.
2. Clique em `Create trail` e configure:
   - **Nome:** `trilha-auditoria-seunome`
   - **S3 bucket:** novo bucket (nome sugerido pelo console)
   - **KMS:** Habilite e crie nova chave `alias/CloudTrailKey-seunome`
3. Mantenha ativado apenas:
   - `Management events`: leitura e escrita
4. Finalize.

![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_15-01.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_15-02.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_15-07.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_15-10.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_15-34.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_15-35.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_15-36.png)

---

### 📦 Passo 4 – Estressar a Instância

1. Conecte via SSH:  
   ```bash
   ssh -i "seu-par.pem" ec2-user@<IP>

2. Execute:
sudo yum update -y
sudo amazon-linux-extras install epel -y
sudo yum install stress -y
stress --cpu 8 --timeout 600

![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_16-16.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_16-17.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_16-18.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_16-19.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_16-20.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_16-21.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_16-23.png)

### 🧪 Passo 6 – Verificar o Alarme
Volte ao CloudWatch.

Veja o estado do alarme (In alarm).

Confirme o e-mail do SNS com alerta de CPU alta.

![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_16-27.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_16-43.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-monitoring-lab-developer-EDN/refs/heads/main/arquivos/2025-06-17_16-42.png)

