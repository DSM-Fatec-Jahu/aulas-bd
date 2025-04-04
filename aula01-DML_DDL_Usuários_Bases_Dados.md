# Introdução ao MySQL: Conexão e Gerenciamento de Usuários e Bancos de Dados

## Sumário
1. [Introdução ao MySQL](#introdução-ao-mysql)
2. [Instalando o MySQL](#instalando-o-mysql)
   - [No Windows](#instalação-no-windows)
   - [No Linux](#instalação-no-linux)
3. [Conectando ao MySQL](#conectando-ao-mysql)
   - [No Windows](#conexão-no-windows)
   - [No Linux](#conexão-no-linux)
4. [Gerenciamento de Usuários](#gerenciamento-de-usuários)
   - [Criando Usuários](#criando-usuários)
   - [Modificando Usuários](#modificando-usuários)
   - [Permissões](#gerenciando-permissões)
   - [Removendo Usuários](#removendo-usuários)
5. [Gerenciamento de Bancos de Dados](#gerenciamento-de-bancos-de-dados)
   - [Criando Bancos](#criando-bancos-de-dados)
   - [Modificando Bancos](#modificando-bancos-de-dados)
   - [Backups](#realizando-backups)
   - [Restauração](#restaurando-backups)
6. [Exemplos Práticos](#exemplos-práticos)
7. [Solução de Problemas Comuns](#solução-de-problemas-comuns)

## Introdução ao MySQL

O MySQL é um sistema de gerenciamento de banco de dados relacional (SGBDR) de código aberto, amplamente utilizado em aplicações web e empresariais. Ele usa a linguagem SQL (Structured Query Language) para gerenciar e manipular dados.

Principais características:
- Open-source (código aberto)
- Alta performance
- Confiabilidade
- Facilidade de uso
- Compatibilidade com diversas plataformas

Nesta aula, aprenderemos como conectar ao MySQL via terminal e como gerenciar usuários e bancos de dados, tanto no Windows quanto no Linux.

## Instalando o MySQL

Antes de começarmos, precisamos ter o MySQL instalado em nosso sistema.

### Instalação no Windows

1. Baixe o instalador do MySQL Community Server em: https://dev.mysql.com/downloads/mysql/

2. Execute o instalador baixado e siga as instruções:
   - Escolha a opção "Custom" para personalizar a instalação
   - Selecione os componentes (pelo menos o MySQL Server e MySQL Shell)
   - Defina uma senha forte para o usuário root
   - Complete a instalação

3. Verifique se o serviço do MySQL está rodando:
   - Abra o "Gerenciador de Serviços" do Windows (digite "services.msc" no menu Iniciar)
   - Procure por "MySQL" na lista e verifique se está como "Em execução"

### Instalação no Linux

#### Para distribuições baseadas em Debian/Ubuntu:

```bash
# Atualize os repositórios
sudo apt update

# Instale o MySQL Server
sudo apt install mysql-server

# Inicie o serviço MySQL
sudo systemctl start mysql

# Configure o MySQL para iniciar automaticamente
sudo systemctl enable mysql

# Execute o script de segurança para configurar o MySQL
sudo mysql_secure_installation
```

Durante o `mysql_secure_installation`, você será guiado para:
- Definir uma senha para o usuário root
- Remover usuários anônimos
- Desabilitar login root remoto
- Remover banco de dados de teste

#### Para distribuições baseadas em Red Hat/Fedora/CentOS:

```bash
# Instale o MySQL Server
sudo dnf install mysql-server

# Inicie o serviço MySQL
sudo systemctl start mysqld

# Configure o MySQL para iniciar automaticamente
sudo systemctl enable mysqld

# Execute o script de segurança para configurar o MySQL
sudo mysql_secure_installation
```

## Conectando ao MySQL

### Conexão no Windows

1. **Usando o Prompt de Comando**:
   - Abra o Prompt de Comando (CMD)
   - Navegue até o diretório de instalação do MySQL (geralmente `C:\Program Files\MySQL\MySQL Server 8.0\bin`)
   - Execute o comando:

```cmd
mysql -u root -p
```

2. **Usando o MySQL Shell**:
   - Abra o MySQL Shell a partir do menu Iniciar
   - O comando para conectar no modo SQL é:

```
\sql
\connect root@localhost
```

3. **Usando o Path do Windows**:
   - Se você adicionou o diretório bin do MySQL ao PATH do Windows, pode simplesmente abrir o CMD e digitar:

```cmd
mysql -u root -p
```

### Conexão no Linux

No terminal, execute:

```bash
# Conectar como root
sudo mysql -u root -p

# Em algumas instalações no Ubuntu, o root usa autenticação por socket
sudo mysql
```

**Parâmetros comuns de conexão**:
- `-u`: especifica o nome do usuário
- `-p`: solicita a senha (você pode incluir a senha imediatamente após o -p, sem espaço, mas isso não é recomendado por questões de segurança)
- `-h`: especifica o host (por padrão é 'localhost')
- `-P`: especifica a porta (por padrão é 3306)

**Exemplo completo**:
```bash
mysql -u root -p -h 127.0.0.1 -P 3306
```

## Gerenciamento de Usuários

### Verificando Usuários Existentes

Uma vez conectado ao MySQL, você pode verificar os usuários existentes:

```sql
-- Mostrar todos os usuários
SELECT user, host FROM mysql.user;
```

### Criando Usuários

Para criar um novo usuário:

```sql
-- Sintaxe básica
CREATE USER 'nome_usuario'@'host' IDENTIFIED BY 'senha';

-- Exemplos práticos
-- Criar usuário que pode conectar de qualquer lugar
CREATE USER 'developer'@'%' IDENTIFIED BY 'senha_segura123';

-- Criar usuário que só pode conectar da máquina local
CREATE USER 'local_app'@'localhost' IDENTIFIED BY 'outra_senha_segura';

-- Criar usuário que só pode conectar de um IP específico
CREATE USER 'remote_app'@'192.168.1.10' IDENTIFIED BY 'senha_remota';
```

### Modificando Usuários

Para alterar a senha de um usuário:

```sql
-- Alterar senha (MySQL 5.7+)
ALTER USER 'nome_usuario'@'host' IDENTIFIED BY 'nova_senha';

-- Método alternativo para versões mais antigas
SET PASSWORD FOR 'nome_usuario'@'host' = PASSWORD('nova_senha');
```

Para renomear um usuário:

```sql
-- Renomear usuário
RENAME USER 'nome_antigo'@'host' TO 'nome_novo'@'host';
```

### Gerenciando Permissões

Os privilégios no MySQL podem ser concedidos em diferentes níveis:
- Global (todos os bancos de dados)
- Banco de dados específico
- Tabela específica
- Coluna específica

#### Concedendo Permissões

```sql
-- Conceder todos os privilégios em todos os bancos
GRANT ALL PRIVILEGES ON *.* TO 'nome_usuario'@'host';

-- Conceder privilégios específicos em um banco
GRANT SELECT, INSERT, UPDATE ON banco_de_dados.* TO 'nome_usuario'@'host';

-- Conceder privilégios em uma tabela específica
GRANT SELECT, UPDATE ON banco_de_dados.tabela TO 'nome_usuario'@'host';

-- Conceder privilégio apenas para criar bancos de dados
GRANT CREATE ON *.* TO 'nome_usuario'@'host';
```

#### Verificando Permissões

```sql
-- Verificar permissões de um usuário
SHOW GRANTS FOR 'nome_usuario'@'host';
```

#### Revogando Permissões

```sql
-- Revogar todos os privilégios
REVOKE ALL PRIVILEGES ON *.* FROM 'nome_usuario'@'host';

-- Revogar privilégios específicos
REVOKE INSERT, UPDATE ON banco_de_dados.* FROM 'nome_usuario'@'host';
```

### Removendo Usuários

```sql
-- Remover um usuário
DROP USER 'nome_usuario'@'host';
```

## Gerenciamento de Bancos de Dados

### Verificando Bancos Existentes

```sql
-- Listar todos os bancos de dados
SHOW DATABASES;
```

### Criando Bancos de Dados

```sql
-- Criar um novo banco de dados
CREATE DATABASE nome_do_banco;

-- Criar um banco se ele não existir (evita erros)
CREATE DATABASE IF NOT EXISTS nome_do_banco;

-- Criar um banco com conjunto de caracteres específico
CREATE DATABASE nome_do_banco CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### Selecionando um Banco de Dados

```sql
-- Selecionar (usar) um banco de dados
USE nome_do_banco;

-- Verificar qual banco está selecionado
SELECT DATABASE();
```

### Modificando Bancos de Dados

```sql
-- Alterar o conjunto de caracteres de um banco
ALTER DATABASE nome_do_banco CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### Removendo Bancos de Dados

```sql
-- Excluir um banco de dados (CUIDADO! Isso exclui todos os dados)
DROP DATABASE nome_do_banco;

-- Excluir apenas se existir (evita erros)
DROP DATABASE IF EXISTS nome_do_banco;
```

### Realizando Backups

#### No Windows

1. **Usando mysqldump a partir do CMD**:

```cmd
mysqldump -u root -p --databases nome_do_banco > C:\caminho\para\backup.sql
```

2. **Backup de todos os bancos de dados**:

```cmd
mysqldump -u root -p --all-databases > C:\caminho\para\backup_completo.sql
```

#### No Linux

1. **Backup de um banco de dados específico**:

```bash
mysqldump -u root -p --databases nome_do_banco > /caminho/para/backup.sql
```

2. **Backup de múltiplos bancos**:

```bash
mysqldump -u root -p --databases banco1 banco2 banco3 > /caminho/para/multi_backup.sql
```

3. **Backup com compressão**:

```bash
mysqldump -u root -p --databases nome_do_banco | gzip > /caminho/para/backup.sql.gz
```

### Restaurando Backups

#### No Windows

```cmd
mysql -u root -p < C:\caminho\para\backup.sql
```

#### No Linux

```bash
mysql -u root -p < /caminho/para/backup.sql

# Para backups comprimidos
gunzip < /caminho/para/backup.sql.gz | mysql -u root -p
```

## Exemplos Práticos

### Cenário 1: Configurando um Ambiente de Desenvolvimento

Vamos criar um banco de dados para desenvolvimento, um usuário específico e conceder as permissões necessárias:

```sql
-- Criar o banco de dados
CREATE DATABASE app_development;

-- Criar um usuário desenvolvedor
CREATE USER 'dev_user'@'localhost' IDENTIFIED BY 'senha_dev123';

-- Conceder permissões ao usuário no banco criado
GRANT ALL PRIVILEGES ON app_development.* TO 'dev_user'@'localhost';

-- Aplicar as permissões
FLUSH PRIVILEGES;

-- Verificar as permissões do usuário
SHOW GRANTS FOR 'dev_user'@'localhost';
```

### Cenário 2: Configurando um Ambiente de Produção

Vamos criar um banco para produção e um usuário com permissões restritas:

```sql
-- Criar o banco de dados de produção
CREATE DATABASE app_production;

-- Criar usuário da aplicação com permissões limitadas
CREATE USER 'app_user'@'%' IDENTIFIED BY 'senha_prod_segura';

-- Conceder apenas as permissões necessárias (sem DROP ou ALTER)
GRANT SELECT, INSERT, UPDATE, DELETE ON app_production.* TO 'app_user'@'%';

-- Aplicar as permissões
FLUSH PRIVILEGES;
```

### Cenário 3: Migração e Backup

Preparando um banco de dados para migração:

```sql
-- No servidor de origem
-- Criar um backup completo
mysqldump -u root -p --databases app_production --routines --triggers > producao_backup.sql

-- No servidor de destino
-- Criar o banco (se não existir)
CREATE DATABASE app_production;

-- Restaurar o backup
mysql -u root -p app_production < producao_backup.sql

-- Verificar se tudo foi restaurado corretamente
USE app_production;
SHOW TABLES;
```

## Solução de Problemas Comuns

### 1. Erro de Conexão

**Problema**: "ERROR 1045 (28000): Access denied for user 'root'@'localhost'"

**Soluções**:
- Verifique se a senha está correta
- No Windows, tente:
  ```cmd
  mysql -u root -p
  ```
- No Linux (Ubuntu), se o root usar autenticação por socket:
  ```bash
  sudo mysql
  ```

### 2. Esqueceu a Senha do Root

**No Windows**:
1. Pare o serviço MySQL no Gerenciador de Serviços
2. Inicie o MySQL em modo seguro:
   ```cmd
   mysqld --skip-grant-tables --user=mysql
   ```
3. Em outra janela CMD:
   ```cmd
   mysql
   ```
4. Redefina a senha:
   ```sql
   UPDATE mysql.user SET authentication_string='' WHERE user='root';
   FLUSH PRIVILEGES;
   ALTER USER 'root'@'localhost' IDENTIFIED BY 'nova_senha';
   ```

**No Linux**:
1. Pare o serviço MySQL:
   ```bash
   sudo systemctl stop mysql
   ```
2. Inicie o MySQL em modo seguro:
   ```bash
   sudo mysqld_safe --skip-grant-tables &
   ```
3. Conecte ao MySQL:
   ```bash
   mysql -u root
   ```
4. Redefina a senha:
   ```sql
   UPDATE mysql.user SET authentication_string='' WHERE user='root';
   FLUSH PRIVILEGES;
   ALTER USER 'root'@'localhost' IDENTIFIED BY 'nova_senha';
   ```

### 3. Erro ao Criar Usuário

**Problema**: "ERROR 1396 (HY000): Operation CREATE USER failed for 'user'@'host'"

**Solução**: O usuário pode já existir. Tente:
```sql
DROP USER IF EXISTS 'user'@'host';
CREATE USER 'user'@'host' IDENTIFIED BY 'senha';
```

## Conclusão

Nesta aula, aprendemos:
- Como conectar ao MySQL via terminal no Windows e Linux
- Como gerenciar usuários (criar, modificar, conceder permissões, remover)
- Como gerenciar bancos de dados (criar, selecionar, modificar, remover)
- Como realizar backups e restaurações
- Como solucionar problemas comuns

O MySQL é uma ferramenta poderosa, e dominar esses comandos básicos é essencial para administrar eficientemente seus bancos de dados. Com a prática, você se tornará cada vez mais confortável com essas operações.

Lembre-se de sempre:
- Manter senhas seguras
- Conceder apenas as permissões necessárias aos usuários
- Realizar backups regularmente
- Documentar as configurações do seu ambiente

---

### Referências e Recursos Adicionais

- [Documentação Oficial do MySQL](https://dev.mysql.com/doc/)
- [Guia de Referência MySQL](https://dev.mysql.com/doc/refman/8.0/en/)
- [MySQL Workbench](https://www.mysql.com/products/workbench/) - Ferramenta gráfica para gerenciamento
