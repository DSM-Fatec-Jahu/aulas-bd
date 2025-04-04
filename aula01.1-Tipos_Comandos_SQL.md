# Tabela de Comandos SQL por Categoria

## 1. DDL (Data Definition Language)

| Tipo | Descrição | Comandos |
|------|-----------|----------|
| **DDL** | Linguagem de Definição de Dados - Usada para definir e modificar a estrutura do banco de dados e seus objetos | • **CREATE**: Cria objetos no banco de dados (tabelas, views, índices, etc.)<br>• **ALTER**: Modifica a estrutura de objetos existentes<br>• **DROP**: Remove objetos do banco de dados<br>• **TRUNCATE**: Remove todos os registros de uma tabela, mas mantém sua estrutura<br>• **COMMENT**: Adiciona comentários ao dicionário de dados<br>• **RENAME**: Renomeia objetos existentes<br>• **GRANT**: Concede privilégios a usuários<br>• **REVOKE**: Remove privilégios concedidos a usuários |

## 2. DML (Data Manipulation Language)

| Tipo | Descrição | Comandos |
|------|-----------|----------|
| **DML** | Linguagem de Manipulação de Dados - Usada para gerenciar dados dentro dos objetos do banco de dados | • **SELECT**: Recupera dados de uma ou mais tabelas<br>• **INSERT**: Adiciona novos registros a uma tabela<br>• **UPDATE**: Modifica registros existentes<br>• **DELETE**: Remove registros de uma tabela<br>• **MERGE**: Combina operações de INSERT e UPDATE (UPSERT)<br>• **CALL**: Chama um procedimento armazenado<br>• **EXPLAIN PLAN**: Mostra o plano de execução de uma consulta<br>• **LOCK TABLE**: Controla concorrência |

## 3. DCL (Data Control Language)

| Tipo | Descrição | Comandos |
|------|-----------|----------|
| **DCL** | Linguagem de Controle de Dados - Usada para controlar acesso aos dados no banco de dados | • **GRANT**: Concede privilégios específicos a usuários<br>• **REVOKE**: Remove privilégios concedidos anteriormente<br>• **DENY**: Nega explicitamente privilégios a usuários (em alguns SGBDs como SQL Server) |

## 4. TCL (Transaction Control Language)

| Tipo | Descrição | Comandos |
|------|-----------|----------|
| **TCL** | Linguagem de Controle de Transações - Usada para gerenciar as mudanças feitas por instruções DML | • **COMMIT**: Salva permanentemente as transações<br>• **ROLLBACK**: Desfaz transações até o último COMMIT<br>• **SAVEPOINT**: Define pontos dentro de uma transação para os quais você pode fazer ROLLBACK<br>• **SET TRANSACTION**: Especifica características da transação |

## 5. DQL (Data Query Language)

| Tipo | Descrição | Comandos |
|------|-----------|----------|
| **DQL** | Linguagem de Consulta de Dados - Alguns consideram uma categoria separada do DML, focada especificamente em consultas | • **SELECT**: Recupera dados do banco de dados<br>• Cláusulas relacionadas: **FROM**, **WHERE**, **GROUP BY**, **HAVING**, **ORDER BY**<br>• Operadores: **UNION**, **INTERSECT**, **EXCEPT**, **JOIN** (INNER, LEFT, RIGHT, FULL) |

## 6. SCL (Session Control Language)

| Tipo | Descrição | Comandos |
|------|-----------|----------|
| **SCL** | Linguagem de Controle de Sessão - Usada para controlar as características dinâmicas da sessão | • **ALTER SESSION**: Modifica as configurações da sessão atual<br>• **SET ROLE**: Especifica o papel ativo para a sessão atual |

## 7. Comandos Auxiliares/Utilitários

| Tipo | Descrição | Comandos |
|------|-----------|----------|
| **Utilitários** | Comandos que auxiliam na administração e operações do banco de dados | • **DESCRIBE** (ou **DESC**): Mostra a estrutura de uma tabela<br>• **USE**: Seleciona um banco de dados para uso<br>• **SHOW**: Exibe informações sobre bancos de dados, tabelas, etc.<br>• **HELP**: Fornece ajuda sobre comandos SQL |

## 8. Extensões Procedurais (PL/SQL, T-SQL, etc.)

| Tipo | Descrição | Comandos |
|------|-----------|----------|
| **Extensões Procedurais** | Extensões de linguagem específicas de cada SGBD para programação procedural | • **CREATE PROCEDURE**: Cria procedimentos armazenados<br>• **CREATE FUNCTION**: Cria funções definidas pelo usuário<br>• **CREATE TRIGGER**: Cria gatilhos que são executados automaticamente<br>• **CREATE PACKAGE**: (Oracle) Agrupa procedimentos e funções relacionados<br>• **DECLARE**: Declara variáveis em blocos de código<br>• **BEGIN/END**: Define blocos de código<br>• **IF/ELSE/ELSEIF**: Estruturas condicionais<br>• **LOOP/WHILE/FOR**: Estruturas de iteração<br>• **EXCEPTION**: Tratamento de exceções |

## 9. Comandos para Índices e Otimização

| Tipo | Descrição | Comandos |
|------|-----------|----------|
| **Índices e Otimização** | Comandos relacionados à criação e gerenciamento de índices e otimização de desempenho | • **CREATE INDEX**: Cria índices para otimização de consultas<br>• **DROP INDEX**: Remove índices existentes<br>• **ALTER INDEX**: Modifica a estrutura de índices<br>• **ANALYZE**: Coleta estatísticas sobre tabelas e índices<br>• **VACUUM**: (PostgreSQL) Recupera espaço e opcionalmente atualiza estatísticas<br>• **OPTIMIZE TABLE**: (MySQL) Reorganiza o armazenamento físico de dados da tabela |
