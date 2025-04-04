# Aula sobre Comandos DDL para Tabelas em MySQL 8.0

## Introdução aos Comandos DDL

Olá! Vamos aprender hoje sobre comandos DDL (Data Definition Language) para tabelas em bancos de dados MySQL 8.0. Os comandos DDL são fundamentais para criar e gerenciar a estrutura do banco de dados.

DDL significa "Linguagem de Definição de Dados" e serve para definir as estruturas que armazenarão os dados. Imagine que você está construindo uma casa: os comandos DDL seriam como o projeto arquitetônico que define onde ficarão as paredes, portas e janelas.

## Principais Comandos DDL para Tabelas

Vamos conhecer os principais comandos DDL:

1. CREATE TABLE - Cria uma nova tabela
2. ALTER TABLE - Modifica a estrutura de uma tabela existente
3. DROP TABLE - Remove uma tabela
4. TRUNCATE TABLE - Remove todos os dados de uma tabela
5. RENAME TABLE - Renomeia uma tabela

Agora vamos aprender cada um deles em detalhes.

## CREATE TABLE - Criando Tabelas

O comando CREATE TABLE permite criar uma nova tabela especificando seu nome, colunas, tipos de dados e restrições.

### Sintaxe básica:

```sql
CREATE TABLE nome_tabela (
    coluna1 tipo_dados [restrições],
    coluna2 tipo_dados [restrições],
    ...
    [restrições_da_tabela]
);
```

### Tipos de dados comuns no MySQL 8.0:

- **INT/INTEGER**: Números inteiros
- **DECIMAL/NUMERIC**: Números decimais
- **VARCHAR/CHAR**: Textos
- **DATE/DATETIME**: Datas
- **BOOLEAN/TINYINT(1)**: Valores lógicos (verdadeiro/falso)
- **TEXT**: Textos longos
- **BLOB**: Dados binários

### Restrições (Constraints):

- **NOT NULL**: A coluna não pode ter valores nulos
- **PRIMARY KEY**: Identifica unicamente cada registro
- **FOREIGN KEY**: Estabelece relacionamento entre tabelas
- **UNIQUE**: Garante que não há valores duplicados
- **CHECK**: Valida os dados conforme uma condição (suportado no MySQL 8.0+)
- **DEFAULT**: Define um valor padrão
- **AUTO_INCREMENT**: Incrementa automaticamente o valor (específico do MySQL)

### Exemplo prático:

Vamos criar um sistema de locadora de veículos com clientes, veículos e locações:

```sql
-- Criando a tabela de clientes
CREATE TABLE clientes (
    id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    telefone VARCHAR(20),
    cpf VARCHAR(14) UNIQUE,
    cnh VARCHAR(20) UNIQUE,
    data_cadastro DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Criando a tabela de categorias de veículos
CREATE TABLE categorias_veiculos (
    id_categoria INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL UNIQUE,
    descricao VARCHAR(200),
    valor_diaria DECIMAL(10,2) NOT NULL CHECK (valor_diaria > 0)
);

-- Criando a tabela de veículos
CREATE TABLE veiculos (
    id_veiculo INT AUTO_INCREMENT PRIMARY KEY,
    placa VARCHAR(10) NOT NULL UNIQUE,
    modelo VARCHAR(100) NOT NULL,
    marca VARCHAR(50) NOT NULL,
    ano_fabricacao INT NOT NULL CHECK (ano_fabricacao > 1900),
    cor VARCHAR(30),
    quilometragem INT NOT NULL DEFAULT 0 CHECK (quilometragem >= 0),
    status VARCHAR(20) DEFAULT 'Disponível' CHECK (status IN ('Disponível', 'Locado', 'Manutenção', 'Inativo')),
    categoria_id INT,
    FOREIGN KEY (categoria_id) REFERENCES categorias_veiculos(id_categoria)
);

-- Criando a tabela de locações
CREATE TABLE locacoes (
    id_locacao INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT NOT NULL,
    veiculo_id INT NOT NULL,
    data_retirada DATETIME NOT NULL,
    data_devolucao_prevista DATETIME NOT NULL,
    data_devolucao_efetiva DATETIME,
    valor_diaria DECIMAL(10,2) NOT NULL CHECK (valor_diaria > 0),
    valor_total DECIMAL(10,2),
    status VARCHAR(20) DEFAULT 'Reservada' CHECK (status IN ('Reservada', 'Em Andamento', 'Concluída', 'Cancelada')),
    FOREIGN KEY (cliente_id) REFERENCES clientes(id_cliente),
    FOREIGN KEY (veiculo_id) REFERENCES veiculos(id_veiculo)
);

-- Criando a tabela de itens adicionais da locação
CREATE TABLE itens_adicionais_locacao (
    id_item INT AUTO_INCREMENT PRIMARY KEY,
    locacao_id INT,
    descricao VARCHAR(100) NOT NULL,
    valor DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (locacao_id) REFERENCES locacoes(id_locacao)
);
```

Observe como criamos relacionamentos entre as tabelas usando FOREIGN KEY. Por exemplo, a tabela `veiculos` tem uma coluna `categoria_id` que referencia `categorias_veiculos(id_categoria)`, estabelecendo que cada veículo pertence a uma categoria.

## ALTER TABLE - Modificando Tabelas

O comando ALTER TABLE permite modificar a estrutura de uma tabela existente. Podemos adicionar, modificar ou remover colunas e restrições.

### Principais operações:

#### 1. Adicionar uma coluna:

```sql
ALTER TABLE nome_tabela
ADD COLUMN nome_coluna tipo_dados [restrições];
```

#### 2. Modificar uma coluna:

```sql
ALTER TABLE nome_tabela
MODIFY COLUMN nome_coluna novo_tipo_dados [novas_restrições];
```

#### 3. Remover uma coluna:

```sql
ALTER TABLE nome_tabela
DROP COLUMN nome_coluna;
```

#### 4. Adicionar uma restrição:

```sql
ALTER TABLE nome_tabela
ADD CONSTRAINT nome_restricao tipo_restricao (coluna);
```

#### 5. Remover uma restrição:

```sql
ALTER TABLE nome_tabela
DROP CONSTRAINT nome_restricao;
```

### Exemplos práticos:

```sql
-- Adicionando uma coluna de endereço à tabela de clientes
ALTER TABLE clientes
ADD COLUMN endereco VARCHAR(200);

-- Modificando o tamanho da coluna modelo de veículos
ALTER TABLE veiculos
MODIFY COLUMN modelo VARCHAR(150) NOT NULL;

-- Adicionando uma coluna para observações em veículos
ALTER TABLE veiculos
ADD COLUMN observacoes TEXT;

-- Adicionando uma restrição CHECK para o ano de fabricação
ALTER TABLE veiculos
ADD CONSTRAINT chk_ano CHECK (ano_fabricacao <= YEAR(CURRENT_DATE));

-- Adicionando uma coluna de cartão de crédito à tabela de clientes
ALTER TABLE clientes
ADD COLUMN cartao_credito VARCHAR(20);

-- Adicionando uma chave estrangeira à tabela de locações para filial
ALTER TABLE locacoes
ADD COLUMN filial_id INT;

ALTER TABLE locacoes
ADD CONSTRAINT fk_filial FOREIGN KEY (filial_id) REFERENCES filiais(id_filial);
```

## DROP TABLE - Removendo Tabelas

O comando DROP TABLE remove completamente uma tabela do banco de dados, incluindo todos os dados, índices e restrições.

### Sintaxe:

```sql
DROP TABLE [IF EXISTS] nome_tabela;
```

A cláusula opcional `IF EXISTS` evita um erro caso a tabela não exista.

### Exemplos:

```sql
-- Removendo uma tabela temporária
DROP TABLE relatorio_temp;

-- Removendo uma tabela se ela existir
DROP TABLE IF EXISTS veiculos_vendidos;
```

**ATENÇÃO**: O DROP TABLE é irreversível! Os dados serão perdidos permanentemente, a menos que você tenha um backup. Também é importante considerar as restrições de integridade referencial antes de remover uma tabela.

## TRUNCATE TABLE - Limpando Tabelas

O TRUNCATE TABLE remove todos os dados de uma tabela, mas mantém sua estrutura. É mais rápido que o DELETE para remover todos os registros, pois não registra cada exclusão no log de transações.

### Sintaxe:

```sql
TRUNCATE TABLE nome_tabela;
```

### Exemplo:

```sql
-- Removendo todos os dados da tabela de relatórios mensais
TRUNCATE TABLE relatorios_mensais;
```

**Diferenças entre TRUNCATE e DELETE**:
- TRUNCATE é mais rápido para limpar uma tabela inteira
- TRUNCATE reinicia os contadores de AUTO_INCREMENT
- TRUNCATE não pode ser usado com WHERE (remove tudo ou nada)
- TRUNCATE não dispara triggers

## RENAME TABLE - Renomeando Tabelas

No MySQL 8.0, você pode renomear tabelas usando dois métodos:

### Sintaxe 1:

```sql
RENAME TABLE nome_antigo TO nome_novo;
```

### Sintaxe 2:

```sql
ALTER TABLE nome_antigo RENAME TO nome_novo;
```

### Exemplo:

```sql
-- Método 1
RENAME TABLE clientes TO clientes_ativos;

-- Método 2
ALTER TABLE veiculos RENAME TO frota_veiculos;
```

## Modelagem com Relacionamentos

Vamos explorar diferentes tipos de relacionamentos entre tabelas:

### 1. Relacionamento Um-para-Um (1:1)

Cada registro em uma tabela está relacionado a no máximo um registro em outra tabela, e vice-versa.

```sql
-- Criando um relacionamento 1:1 entre veículo e documento do veículo
CREATE TABLE veiculos (
    id_veiculo INT AUTO_INCREMENT PRIMARY KEY,
    placa VARCHAR(10) NOT NULL UNIQUE,
    modelo VARCHAR(100) NOT NULL,
    marca VARCHAR(50) NOT NULL,
    ano_fabricacao INT NOT NULL
);

CREATE TABLE documentos_veiculo (
    id_documento INT AUTO_INCREMENT PRIMARY KEY,
    veiculo_id INT UNIQUE,
    renavam VARCHAR(11) UNIQUE NOT NULL,
    data_licenciamento DATE NOT NULL,
    valor_ipva DECIMAL(10,2),
    FOREIGN KEY (veiculo_id) REFERENCES veiculos(id_veiculo)
);
```

No MySQL 8.0, é uma prática comum incluir um ID próprio para a tabela de documentos, mesmo em relacionamentos 1:1.

### 2. Relacionamento Um-para-Muitos (1:N)

Um registro em uma tabela pode estar relacionado a vários registros em outra tabela, mas cada registro na segunda tabela está relacionado a apenas um registro na primeira.

```sql
-- Relacionamento 1:N entre categorias e veículos
CREATE TABLE categorias_veiculos (
    id_categoria INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL,
    descricao VARCHAR(100)
);

-- Adicionando categoria_id à tabela de veículos
ALTER TABLE veiculos
ADD COLUMN categoria_id INT;

ALTER TABLE veiculos
ADD CONSTRAINT fk_categoria FOREIGN KEY (categoria_id) 
REFERENCES categorias_veiculos(id_categoria);
```

Cada categoria pode ter muitos veículos, mas cada veículo pertence a apenas uma categoria.

### 3. Relacionamento Muitos-para-Muitos (N:M)

Vários registros em uma tabela podem estar relacionados a vários registros em outra tabela. Este tipo de relacionamento é implementado usando uma tabela de junção.

```sql
-- Exemplo: Veículos podem ter várias manutenções e 
-- cada serviço de manutenção pode ser aplicado a vários veículos

CREATE TABLE veiculos (
    id_veiculo INT AUTO_INCREMENT PRIMARY KEY,
    placa VARCHAR(10) NOT NULL UNIQUE,
    modelo VARCHAR(100) NOT NULL
);

CREATE TABLE servicos_manutencao (
    id_servico INT AUTO_INCREMENT PRIMARY KEY,
    descricao VARCHAR(100) NOT NULL,
    valor_padrao DECIMAL(10,2) NOT NULL
);

-- Tabela de junção para relacionamento N:M
CREATE TABLE manutencoes_veiculos (
    id_manutencao INT AUTO_INCREMENT PRIMARY KEY,
    veiculo_id INT,
    servico_id INT,
    data_servico DATE NOT NULL DEFAULT CURRENT_DATE,
    valor_cobrado DECIMAL(10,2) NOT NULL,
    observacoes TEXT,
    UNIQUE KEY (veiculo_id, servico_id, data_servico),
    FOREIGN KEY (veiculo_id) REFERENCES veiculos(id_veiculo),
    FOREIGN KEY (servico_id) REFERENCES servicos_manutencao(id_servico)
);
```

No MySQL 8.0, é comum adicionar um ID próprio para a tabela de junção, além da chave composta.

## Exemplo Prático Completo: Sistema de Locadora de Veículos

Vamos criar um sistema de locadora de veículos completo com várias tabelas relacionadas:

```sql
-- Tabela de filiais
CREATE TABLE filiais (
    id_filial INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    endereco VARCHAR(200) NOT NULL,
    telefone VARCHAR(20) NOT NULL,
    email VARCHAR(100)
);

-- Tabela de funcionários
CREATE TABLE funcionarios (
    id_funcionario INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    cpf VARCHAR(14) UNIQUE NOT NULL,
    cargo VARCHAR(50) NOT NULL,
    filial_id INT,
    data_contratacao DATE NOT NULL,
    salario DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (filial_id) REFERENCES filiais(id_filial)
);

-- Tabela de clientes
CREATE TABLE clientes (
    id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    cpf VARCHAR(14) UNIQUE NOT NULL,
    cnh VARCHAR(20) UNIQUE NOT NULL,
    data_nascimento DATE NOT NULL,
    endereco VARCHAR(200) NOT NULL,
    telefone VARCHAR(20) NOT NULL,
    email VARCHAR(100) UNIQUE,
    data_cadastro DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Tabela de categorias de veículos
CREATE TABLE categorias_veiculos (
    id_categoria INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL UNIQUE,
    descricao VARCHAR(200),
    valor_diaria DECIMAL(10,2) NOT NULL CHECK (valor_diaria > 0)
);

-- Tabela de marcas
CREATE TABLE marcas (
    id_marca INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL UNIQUE
);

-- Tabela de modelos
CREATE TABLE modelos (
    id_modelo INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    marca_id INT NOT NULL,
    categoria_id INT NOT NULL,
    UNIQUE KEY (nome, marca_id),
    FOREIGN KEY (marca_id) REFERENCES marcas(id_marca),
    FOREIGN KEY (categoria_id) REFERENCES categorias_veiculos(id_categoria)
);

-- Tabela de veículos
CREATE TABLE veiculos (
    id_veiculo INT AUTO_INCREMENT PRIMARY KEY,
    placa VARCHAR(10) NOT NULL UNIQUE,
    modelo_id INT NOT NULL,
    ano_fabricacao INT NOT NULL CHECK (ano_fabricacao > 1900),
    cor VARCHAR(30) NOT NULL,
    quilometragem INT NOT NULL DEFAULT 0 CHECK (quilometragem >= 0),
    renavam VARCHAR(11) UNIQUE NOT NULL,
    chassis VARCHAR(17) UNIQUE NOT NULL,
    data_aquisicao DATE NOT NULL,
    filial_id INT NOT NULL,
    status VARCHAR(20) DEFAULT 'Disponível' CHECK (status IN ('Disponível', 'Locado', 'Manutenção', 'Inativo')),
    FOREIGN KEY (modelo_id) REFERENCES modelos(id_modelo),
    FOREIGN KEY (filial_id) REFERENCES filiais(id_filial)
);

-- Tabela de opcionais
CREATE TABLE opcionais (
    id_opcional INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL UNIQUE,
    descricao VARCHAR(200)
);

-- Tabela de relação entre veículos e opcionais (N:M)
CREATE TABLE veiculos_opcionais (
    id_veiculo_opcional INT AUTO_INCREMENT PRIMARY KEY,
    veiculo_id INT,
    opcional_id INT,
    UNIQUE KEY (veiculo_id, opcional_id),
    FOREIGN KEY (veiculo_id) REFERENCES veiculos(id_veiculo),
    FOREIGN KEY (opcional_id) REFERENCES opcionais(id_opcional)
);

-- Tabela de serviços adicionais
CREATE TABLE servicos_adicionais (
    id_servico INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL UNIQUE,
    descricao VARCHAR(200),
    valor DECIMAL(10,2) NOT NULL CHECK (valor >= 0)
);

-- Tabela de locações
CREATE TABLE locacoes (
    id_locacao INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT NOT NULL,
    veiculo_id INT NOT NULL,
    funcionario_id INT NOT NULL,
    filial_retirada_id INT NOT NULL,
    filial_devolucao_id INT NOT NULL,
    data_locacao DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    data_retirada DATETIME NOT NULL,
    data_devolucao_prevista DATETIME NOT NULL,
    data_devolucao_efetiva DATETIME,
    quilometragem_inicial INT NOT NULL,
    quilometragem_final INT,
    valor_diaria DECIMAL(10,2) NOT NULL,
    dias_previstos INT NOT NULL,
    valor_total DECIMAL(10,2),
    status VARCHAR(20) DEFAULT 'Reservada' CHECK (status IN ('Reservada', 'Em Andamento', 'Concluída', 'Cancelada')),
    observacoes TEXT,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id_cliente),
    FOREIGN KEY (veiculo_id) REFERENCES veiculos(id_veiculo),
    FOREIGN KEY (funcionario_id) REFERENCES funcionarios(id_funcionario),
    FOREIGN KEY (filial_retirada_id) REFERENCES filiais(id_filial),
    FOREIGN KEY (filial_devolucao_id) REFERENCES filiais(id_filial)
);

-- Tabela de serviços adicionais incluídos na locação (N:M)
CREATE TABLE locacoes_servicos (
    id_locacao_servico INT AUTO_INCREMENT PRIMARY KEY,
    locacao_id INT,
    servico_id INT,
    quantidade INT DEFAULT 1 CHECK (quantidade > 0),
    valor_unitario DECIMAL(10,2) NOT NULL,
    UNIQUE KEY (locacao_id, servico_id),
    FOREIGN KEY (locacao_id) REFERENCES locacoes(id_locacao),
    FOREIGN KEY (servico_id) REFERENCES servicos_adicionais(id_servico)
);

-- Tabela de pagamentos
CREATE TABLE pagamentos (
    id_pagamento INT AUTO_INCREMENT PRIMARY KEY,
    locacao_id INT NOT NULL,
    tipo_pagamento VARCHAR(30) NOT NULL CHECK (tipo_pagamento IN ('Dinheiro', 'Cartão de Crédito', 'Cartão de Débito', 'Transferência', 'Pix')),
    valor DECIMAL(10,2) NOT NULL,
    data_pagamento DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    status VARCHAR(20) DEFAULT 'Concluído' CHECK (status IN ('Pendente', 'Concluído', 'Cancelado')),
    FOREIGN KEY (locacao_id) REFERENCES locacoes(id_locacao)
);

-- Tabela de manutenções
CREATE TABLE manutencoes (
    id_manutencao INT AUTO_INCREMENT PRIMARY KEY,
    veiculo_id INT NOT NULL,
    tipo VARCHAR(50) NOT NULL,
    descricao TEXT NOT NULL,
    data_inicio DATE NOT NULL,
    data_fim DATE,
    custo DECIMAL(10,2) DEFAULT 0,
    status VARCHAR(20) DEFAULT 'Agendada' CHECK (status IN ('Agendada', 'Em Andamento', 'Concluída', 'Cancelada')),
    observacoes TEXT,
    FOREIGN KEY (veiculo_id) REFERENCES veiculos(id_veiculo)
);
```

Neste exemplo, temos várias tabelas relacionadas formando um sistema de locadora de veículos completo:

1. Relacionamento 1:N entre filiais e funcionários (uma filial tem muitos funcionários)
2. Relacionamento 1:N entre filiais e veículos (uma filial gerencia muitos veículos)
3. Relacionamento 1:N entre marcas e modelos (uma marca tem muitos modelos)
4. Relacionamento 1:N entre categorias e modelos (uma categoria tem muitos modelos)
5. Relacionamento 1:N entre modelos e veículos (um modelo tem muitos veículos)
6. Relacionamento N:M entre veículos e opcionais (um veículo pode ter muitos opcionais e um opcional pode estar em muitos veículos)
7. Relacionamento 1:N entre clientes e locações (um cliente pode fazer muitas locações)
8. Relacionamento 1:N entre veículos e locações (um veículo pode ser locado muitas vezes, em momentos diferentes)
9. Relacionamento 1:N entre funcionários e locações (um funcionário pode registrar muitas locações)
10. Relacionamento N:M entre locações e serviços adicionais (uma locação pode incluir muitos serviços e um serviço pode ser incluído em muitas locações)
11. Relacionamento 1:N entre locações e pagamentos (uma locação pode ter vários pagamentos)
12. Relacionamento 1:N entre veículos e manutenções (um veículo pode passar por muitas manutenções)

## Boas Práticas ao Usar Comandos DDL no MySQL 8.0

1. **Planeje antes de criar**: Modele seu banco de dados cuidadosamente antes de começar a criar tabelas.

2. **Use nomes significativos**: Escolha nomes claros para tabelas e colunas que representem bem o que elas armazenam.

3. **Documente seu schema**: Adicione comentários ao seu esquema de banco de dados.
   ```sql
   -- Adicionando comentário a uma tabela no MySQL
   ALTER TABLE locacoes COMMENT = 'Registros de locações de veículos aos clientes';
   
   -- Adicionando comentário a uma coluna no MySQL
   ALTER TABLE locacoes MODIFY COLUMN valor_total DECIMAL(10,2) COMMENT 'Valor total da locação incluindo serviços adicionais';
   ```

4. **Use CREATE IF NOT EXISTS e DROP IF EXISTS**: Para evitar erros quando executar scripts múltiplas vezes.
   ```sql
   CREATE TABLE IF NOT EXISTS marcas (
       id_marca INT AUTO_INCREMENT PRIMARY KEY,
       nome VARCHAR(50) NOT NULL UNIQUE
   );
   ```

5. **Scripts idempotentes**: Crie scripts que possam ser executados várias vezes sem causar problemas.
   ```sql
   -- Exemplo de script idempotente para criar uma coluna no MySQL
   SET @exist := (SELECT COUNT(*) FROM information_schema.columns 
                 WHERE table_name = 'clientes' 
                 AND column_name = 'email'
                 AND table_schema = DATABASE());

   SET @query = IF(@exist = 0, 
                  'ALTER TABLE clientes ADD COLUMN email VARCHAR(100)', 
                  'SELECT "Coluna já existe" AS message');

   PREPARE stmt FROM @query;
   EXECUTE stmt;
   DEALLOCATE PREPARE stmt;
   ```

6. **Backup antes de alterações**: Sempre faça backup antes de executar comandos DDL em produção.
   ```sql
   -- Backup de tabela no MySQL
   CREATE TABLE backup_veiculos SELECT * FROM veiculos;
   ```

7. **Mantenha a integridade referencial**: Use sempre chaves estrangeiras para manter a integridade dos dados.

8. **Use transações para operações complexas**: Agrupe várias operações DDL em transações quando possível (nota: nem todas operações DDL podem ser revertidas em MySQL).
   ```sql
   START TRANSACTION;
   
   CREATE TABLE nova_tabela (id INT PRIMARY KEY);
   ALTER TABLE outra_tabela ADD COLUMN nova_coluna INT;
   
   COMMIT;
   ```

9. **Use ENGINE=InnoDB**: O MySQL 8.0 usa InnoDB por padrão, mas é bom especificar para garantir.
   ```sql
   CREATE TABLE veiculos (
       id_veiculo INT AUTO_INCREMENT PRIMARY KEY,
       placa VARCHAR(10) NOT NULL UNIQUE
   ) ENGINE=InnoDB;
   ```

10. **Defina CHARACTER SET e COLLATION**: Para suporte adequado a diferentes idiomas.
    ```sql
    CREATE TABLE clientes (
        id_cliente INT AUTO_INCREMENT PRIMARY KEY,
        nome VARCHAR(100) NOT NULL
    ) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ```

## Resumo dos Comandos DDL no MySQL 8.0

| Comando | Função | Exemplo |
|---------|--------|---------|
| CREATE TABLE | Criar uma nova tabela | `CREATE TABLE veiculos (id_veiculo INT AUTO_INCREMENT PRIMARY KEY, placa VARCHAR(10) NOT NULL UNIQUE);` |
| ALTER TABLE | Modificar uma tabela existente | `ALTER TABLE veiculos ADD COLUMN cor VARCHAR(30);` |
| DROP TABLE | Remover uma tabela | `DROP TABLE veiculos_antigos;` |
| TRUNCATE TABLE | Remover todos os dados | `TRUNCATE TABLE log_acessos;` |
| RENAME TABLE | Renomear uma tabela | `RENAME TABLE veiculos TO frota_veiculos;` |

## Exercícios Práticos

1. Crie uma tabela de clientes com pelo menos 5 colunas, incluindo uma chave primária.
2. Adicione uma coluna de histórico de pontos na CNH à tabela de clientes.
3. Crie uma tabela de seguros de veículos e estabeleça um relacionamento com a tabela de veículos.
4. Renomeie a tabela de veículos para "frota_ativa".
5. Crie uma tabela para registrar as multas dos veículos, estabelecendo relacionamentos adequados.

## Solução dos Exercícios

### 1. Criando uma tabela de clientes

```sql
CREATE TABLE clientes (
    id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    cpf VARCHAR(14) UNIQUE NOT NULL,
    cnh VARCHAR(20) UNIQUE NOT NULL,
    telefone VARCHAR(20) NOT NULL,
    email VARCHAR(100) UNIQUE,
    data_cadastro DATETIME DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### 2. Adicionando coluna de histórico de pontos na CNH

```sql
ALTER TABLE clientes
ADD COLUMN pontos_cnh INT DEFAULT 0 CHECK (pontos_cnh >= 0 AND pontos_cnh <= 40);
```

### 3. Criando tabela de seguros e relacionamento

```sql
CREATE TABLE seguros_veiculos (
    id_seguro INT AUTO_INCREMENT PRIMARY KEY,
    veiculo_id INT NOT NULL,
    seguradora VARCHAR(100) NOT NULL,
    numero_apolice VARCHAR(50) NOT NULL UNIQUE,
    data_contratacao DATE NOT NULL,
    data_vencimento DATE NOT NULL,
    valor_cobertura DECIMAL(12,2) NOT NULL,
    valor_franquia DECIMAL(10,2) NOT NULL,
    status VARCHAR(20) DEFAULT 'Ativo' CHECK (status IN ('Ativo', 'Cancelado', 'Vencido')),
    FOREIGN KEY (veiculo_id) REFERENCES veiculos(id_veiculo)
) ENGINE=InnoDB CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### 4. Renomeando a tabela

```sql
RENAME TABLE veiculos TO frota_ativa;
```

### 5. Criando tabela de multas

```sql
CREATE TABLE multas (
    id_multa INT AUTO_INCREMENT PRIMARY KEY,
    veiculo_id INT NOT NULL,
    data_infracao DATETIME NOT NULL,
    local_infracao VARCHAR(200) NOT NULL,
    descricao TEXT NOT NULL,
    valor DECIMAL(10,2) NOT NULL,
    pontos INT NOT NULL CHECK (pontos >= 0),
    status VARCHAR(20) DEFAULT 'Pendente' CHECK (status IN ('Pendente', 'Paga', 'Contestada', 'Cancelada')),
    responsavel_id INT,
    FOREIGN KEY (veiculo_id) REFERENCES frota_ativa(id_veiculo),
    FOREIGN KEY (responsavel_id) REFERENCES clientes(id_cliente)
) ENGINE=InnoDB CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

---

Com estes conhecimentos, você já pode começar a criar a estrutura de bancos de dados relacionais utilizando comandos DDL para tabelas no MySQL 8.0. Lembre-se que o design cuidadoso do banco de dados é fundamental para garantir um sistema eficiente e fácil de manter.
