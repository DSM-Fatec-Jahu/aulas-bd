# TIPOS DE DADOS NO MYSQL 8.0

## TIPOS NUMÉRICOS

| Tipo       | Descrição                         | Faixa (Com Sinal)                                      | Faixa (Sem Sinal)                 | Armazenamento |
|------------|-----------------------------------|--------------------------------------------------------|-----------------------------------|---------------|
| TINYINT    | Inteiro muito pequeno             | -128 a 127                                             | 0 a 255                           | 1 byte        |
| SMALLINT   | Inteiro pequeno                   | -32.768 a 32.767                                       | 0 a 65.535                        | 2 bytes       |
| MEDIUMINT  | Inteiro de tamanho médio          | -8.388.608 a 8.388.607                                 | 0 a 16.777.215                    | 3 bytes       |
| INT        | Inteiro normal                    | -2.147.483.648 a 2.147.483.647                         | 0 a 4.294.967.295                 | 4 bytes       |
| BIGINT     | Inteiro grande                    | -9.223.372.036.854.775.808 a 9.223.372.036.854.775.807| 0 a 18.446.744.073.709.551.615    | 8 bytes       |
| FLOAT      | Ponto flutuante precisão simples  | ±1.175494351E-38 a ±3.402823466E+38                   | (Mesma faixa)                     | 4 bytes       |
| DOUBLE     | Ponto flutuante precisão dupla    | ±2.2250738585072014E-308 a ±1.7976931348623157E+308   | (Mesma faixa)                     | 8 bytes       |
| DECIMAL    | Decimal de precisão fixa          | Depende de M,D (máx M=65, D=30)                        | (Mesma faixa)                     | Variável      |

## TIPOS DE TEMPO (DATA E HORA)

| Tipo       | Descrição                         | Formato                  | Faixa                                           | Armazenamento                          |
|------------|-----------------------------------|--------------------------|------------------------------------------------|----------------------------------------|
| DATE       | Data                              | 'YYYY-MM-DD'             | '1000-01-01' a '9999-12-31'                    | 3 bytes                                |
| TIME       | Hora                              | 'HH:MM:SS'               | '-838:59:59' a '838:59:59'                     | 3 bytes                                |
| DATETIME   | Data e hora combinadas            | 'YYYY-MM-DD HH:MM:SS'    | '1000-01-01 00:00:00' a '9999-12-31 23:59:59'  | 5 bytes + 3 bytes (fração segundos)    |
| TIMESTAMP  | Timestamp baseado no padrão Unix  | 'YYYY-MM-DD HH:MM:SS'    | '1970-01-01 00:00:01' a '2038-01-19 03:14:07'  | 4 bytes + 3 bytes (fração segundos)    |
| YEAR       | Ano                               | YYYY                     | 1901 a 2155                                    | 1 byte                                 |

## TIPOS DE TEXTO (STRING)

| Tipo       | Descrição                      | Tamanho Máximo             | Armazenamento                           | Observações                           |
|------------|--------------------------------|----------------------------|----------------------------------------|---------------------------------------|
| CHAR       | String comprimento fixo        | 0 a 255 caracteres         | O tamanho definido                     | Preenche com espaços se necessário    |
| VARCHAR    | String comprimento variável    | 0 a 65.535 caracteres      | Conteúdo + 1-2 bytes                   | Limitado pelo tamanho máximo da linha |
| TINYTEXT   | String de texto pequena        | 255 caracteres             | Comprimento + 1 byte                   |                                       |
| TEXT       | String de texto normal         | 65.535 caracteres (~64KB)  | Comprimento + 2 bytes                  |                                       |
| MEDIUMTEXT | String de texto médio          | 16.777.215 chars (~16MB)   | Comprimento + 3 bytes                  |                                       |
| LONGTEXT   | String de texto longo          | 4.294.967.295 chars (~4GB) | Comprimento + 4 bytes                  |                                       |

## TIPOS BINÁRIOS (BLOB)

| Tipo       | Descrição                      | Tamanho Máximo         | Armazenamento               |
|------------|--------------------------------|------------------------|------------------------------|
| BINARY     | String binária comprimento fixo| 0 a 255 bytes          | O tamanho definido           |
| VARBINARY  | String binária comprimento var.| 0 a 65.535 bytes       | Conteúdo + 1-2 bytes         |
| TINYBLOB   | Objeto binário pequeno         | 255 bytes              | Comprimento + 1 byte         |
| BLOB       | Objeto binário normal          | 65.535 bytes (~64KB)   | Comprimento + 2 bytes        |
| MEDIUMBLOB | Objeto binário médio           | 16.777.215 bytes (~16MB)| Comprimento + 3 bytes       |
| LONGBLOB   | Objeto binário longo           | 4.294.967.295 bytes (~4GB)| Comprimento + 4 bytes     |

## OUTROS TIPOS

| Tipo               | Descrição                           | Limite                              | Armazenamento                         |
|--------------------|-------------------------------------|-------------------------------------|---------------------------------------|
| ENUM               | Enumeração (lista valores possíveis)| Máximo 65.535 valores               | 1 byte (até 255) ou 2 bytes (até 65.535)|
| SET                | Conjunto de valores (múltipla escolha)| Máximo 64 valores                  | 1 a 8 bytes (depende do número valores)|
| JSON               | Documentos JSON                     | ~4GB (mesmo que LONGTEXT)           | Variável                              |
| GEOMETRY           | Tipo espacial genérico              | -                                   | Variável                              |
| POINT              | Ponto (coordenada x,y)              | -                                   | Variável                              |
| LINESTRING         | Linha ou caminho                    | -                                   | Variável                              |
| POLYGON            | Polígono (área fechada)             | -                                   | Variável                              |
| MULTIPOINT         | Coleção de pontos                   | -                                   | Variável                              |
| MULTILINESTRING    | Coleção de linhas                   | -                                   | Variável                              |
| MULTIPOLYGON       | Coleção de polígonos                | -                                   | Variável                              |
| GEOMETRYCOLLECTION | Coleção de objetos geométricos      | -                                   | Variável                              |

Observações importantes:
- O tamanho máximo de uma linha no MySQL é de 65.535 bytes
- A codificação de caracteres escolhida (UTF-8, etc.) afeta o espaço necessário para strings
- O atributo UNSIGNED pode ser usado com tipos numéricos para duplicar o limite superior
- O tipo TIMESTAMP automaticamente converte para o fuso horário atual
- Tipos espaciais são otimizados para armazenar e consultar dados geográficos