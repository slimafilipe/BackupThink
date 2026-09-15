# BackupThink

Serviço automatizado desenvolvido em **Spring Boot** e **React** responsável por realizar o backup de configurações de gerência de OLTs da fabricante Think, centralizando e versionando os arquivos binários (`.bin`) de forma segura em um repositório privado do GitHub.

Este projeto surgiu como uma iniciativa para automatizar a infraestrutura de rede, garantindo resiliência e facilidade na gestão diária do ISP.

---

## 🛠️ Tecnologias Utilizadas

### **Backend**
* **Java 25** (Assynchronous Connection Pools & Virtual Threads)
* **Spring Boot 3.x** (Web, Data JPA, Scheduling)
* **H2 Database** (Desenvolvimento/Testes) & **PostgreSQL** (Produção)
* **JGit** (Integração nativa com o protocolo Git via Java)
* **Lombok** (Produtividade e código limpo)

### **Frontend**
* **React** + **Vite** (Interface reativa e modular)
* **Tailwind CSS** (Estilização moderna e escurecida)

---

## 🏗️ Como a Aplicação Funciona

1. **Autenticação na OLT:** A API dispara um `POST /api/login` para a OLT enviando as credenciais de acesso.
2. **Download do Binário:** Com o Token retornado, o sistema executa um `GET /api/management/config` via streaming direto (`InputStream`) para otimizar o uso de memória.
3. **Validação do Arquivo:** O fluxo verifica se a OLT devolveu algum erro em JSON antes de realizar o commit, prevenindo arquivos corrompidos.
4. **Versionamento e Backup:** O JGit organiza a estrutura local (`olts/{IP}/{NOME_OLT}/backup_config.bin`), realiza o commit com timestamp e faz o push para o GitHub.
5. **Automação (Scheduler):** Rotina diária/periódica agendada via `@Scheduled` para realizar o backup de toda a planta ativa.

---

## 🚀 Como Configurar e Rodar

### 1. Clonar o Repositório
```bash
git clone [https://github.com/slimafilipe/cronjob-oltThink_backup.git](https://github.com/slimafilipe/cronjob-oltThink_backup.git)
cd cronjob-oltThink_backup
```
### 2. Configurar o Backend

Copie o arquivo de exemplo e preencha suas variáveis:
```bash
cp src/main/resources/application.yml.example src/main/resources/application.yml
```
Execute o Spring Boot:
```bash
./mvnw spring-boot:run
```
### 3. Configurar o Frontend

Em outro terminal, acesse a pasta do frontend e inicie a interface:
```bash
cd frontend
npm install
npm run dev
```
## 🔌 Endpoints Principais da API

    POST /api/v1/olts/{id}/manual-backup — Dispara backup e commit imediato de uma OLT específica.

    POST /api/v1/olts/backup-all — Executa a varredura e backup de todas as OLTs ativas (enabled = true).

    GET /api/v1/olts/{id}/download-git — Faz o download do último backup salvo no repositório.

📄 Licença

Este projeto está sob a licença MIT.
