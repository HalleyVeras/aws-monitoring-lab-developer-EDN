# 🚀 Laboratório: Monitoramento e Auditoria com CloudWatch e CloudTrail na AWS

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

📸 *Adicione aqui um print da instância criada*

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

📸 *Adicione aqui um print do alarme configurado e do e-mail de confirmação*

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

📸 *Adicione aqui print da trilha criada e estrutura do bucket S3*

---

### 📦 Passo 4 – Visualizar Logs

#### Logs via S3:
1. Acesse o **console S3**.
2. Navegue até o bucket do CloudTrail.
3. Explore as pastas e visualize os arquivos `.gz`.

#### (Opcional) Enviar logs para CloudWatch Logs:
- Habilite essa opção em `Trails > Edit`.
- Crie um novo **Log Group** e **IAM Role**.

📸 *Adicione aqui print dos logs visualizados no S3 e CloudWatch Logs (opcional)*

---

### 🔥 Passo 5 – Estressar a Instância

1. Conecte via SSH:  
   ```bash
   ssh -i "seu-par.pem" ec2-user@<IP>
