# Convenções e Boas Práticas para Estruturação de Bancos de Dados

## 1. Nomenclatura de Tabelas

### 1.1 Regras Gerais
- Use nomes no plural para tabelas que representam coleções de entidades.
- Utilize snake_case para nomes de tabelas.
- Evite prefixos ou sufixos desnecessários.
- Para nomes compostos, apenas o último termo deve estar no plural.

### 1.2 Exemplos em Inglês
- `users`
- `product_categories`
- `order_items`

### 1.3 Exemplos em Português
- `usuarios`
- `categoria_produtos` ou `produto_categorias`
- `item_pedidos`

## 2. Nomenclatura de Colunas

### 2.1 Regras Gerais
- Use snake_case para nomes de colunas.
- Prefixe chaves estrangeiras com o nome da tabela referenciada no singular.
- Use `id` como nome padrão para chaves primárias.
- Sempre inclua as colunas `created_at` e `updated_at` em todas as tabelas.

### 2.2 Exemplos em Inglês
- `id`
- `first_name`
- `user_id` (chave estrangeira para a tabela `users`)
- `created_at`
- `updated_at`

### 2.3 Exemplos em Português
- `id`
- `nome`
- `usuario_id` (chave estrangeira para a tabela `usuarios`)
- `created_at`
- `updated_at`

### 2.4 Importância das colunas created_at e updated_at
Estas colunas são cruciais para:
- Rastrear quando um registro foi criado e modificado pela última vez.
- Facilitar a depuração e auditoria de dados.
- Permitir ordenação e filtragem baseadas em tempo.
- Possibilitar a implementação de cache e sincronização eficientes.

### 2.5 Coluna deleted_at e Soft Deletes

Quando se deseja implementar a funcionalidade de soft delete, a coluna deleted_at é adicionada. Essa coluna armazena a data em que o registro foi "deletado", mas sem removê-lo do banco de dados. Sua importância inclui:

- Recuperação de dados: Permite que o registro seja restaurado em vez de apagado permanentemente.
- Manutenção de histórico: O registro continua existindo no banco para auditorias ou consultas futuras, mesmo após ser marcado como deletado.
- Filtros automáticos: Alguns frameworks podem excluir automaticamente registros "deletados" das consultas, garantindo a segurança e integridade sem remover dados de forma definitiva.

## 3. Índices e Chaves Estrangeiras

### 3.1 Índices
- Nomeie índices usando o padrão: `idx_{table_name}_{column_name}`
- Para índices compostos: `idx_{table_name}_{column1_name}_{column2_name}`

### 3.2 Chaves Estrangeiras
- Nomeie chaves estrangeiras usando o padrão: `fk_{table_name}_{referenced_table_name}`

### 3.3 Exemplos
- `idx_users_email`
- `idx_products_name_category`
- `fk_order_items_orders`

## 4. Tipos de Dados

### 4.1 Regras Gerais
- Use tipos de dados apropriados para cada coluna (e.g., `INT` para números inteiros, `VARCHAR` para strings de tamanho variável).
- Defina tamanhos adequados para campos de texto.
- Use `DECIMAL` para valores monetários.

### 4.2 Chaves Primárias
- Use `BIGINT` como padrão para colunas de ID.
- Considere `UUID` apenas em casos específicos, como:
  - Quando a unicidade global é necessária (por exemplo, em sistemas distribuídos).
  - Para melhorar a segurança, evitando IDs sequenciais previsíveis.
  - Em cenários de sincronização de dados entre diferentes sistemas.

### 4.3 Vantagens do BIGINT para IDs
- Melhor performance em índices e joins comparado a UUIDs.
- Menor consumo de espaço em disco.
- Compatibilidade mais ampla com ferramentas e frameworks.
- Facilidade de leitura e depuração para humanos.

## 5. Particionamento de Tabelas

### 5.1 Quando Particionar
- Considere particionar tabelas quando uma partição atingir ou ultrapassar 10GB.
- Para tabelas menores que 10GB, geralmente não é necessário particionar.

### 5.2 Estratégias de Particionamento
- Por intervalo de datas (e.g., mensal, trimestral)
- Por hash de uma coluna chave
- Por lista de valores

### 5.3 Boas Práticas
- Escolha a coluna de particionamento com base nos padrões de consulta mais comuns.
- Mantenha um número gerenciável de partições.
- Considere o impacto no desempenho das consultas que atravessam múltiplas partições.

## 6. Outras Boas Práticas

- Use transações para manter a integridade dos dados.
- Implemente validações tanto no nível do aplicativo quanto no banco de dados.
- Documente o esquema do banco de dados e mantenha a documentação atualizada.
- Realize backups regulares e teste os procedimentos de restauração.
- Monitore o desempenho do banco de dados e otimize conforme necessário.

## 7. Considerações Finais

Estas convenções e boas práticas visam melhorar a legibilidade, manutenção e desempenho do banco de dados. É importante adaptar essas diretrizes às necessidades específicas do projeto e da equipe, mantendo a consistência ao longo do tempo.

## 8. Uso de Plural e Singular

### 8.1 Tabelas
- Use plural para nomes de tabelas que representam coleções (ex: `usuarios`, `produtos`).
- Em nomes compostos, apenas o último termo deve estar no plural (ex: `categoria_produtos`, `status_pedidos`).
- Para nomes com três ou mais palavras, mantenha a mesma regra: apenas o último termo no plural.

Exemplos com três ou mais palavras:
- `relatorio_venda_mensais`
- `log_acesso_sistema_externos`
- `historico_alteracao_preco_produto`

### 8.2 Colunas
- Use singular para nomes de colunas (ex: `nome`, `endereco`).
- Para chaves estrangeiras, use o nome da tabela referenciada no singular, seguido de "_id" (ex: `usuario_id`, `produto_id`).
- Em colunas com nomes compostos por três ou mais palavras, mantenha todas no singular.

Exemplos com três ou mais palavras:
- `data_ultimo_acesso`
- `numero_identificacao_fiscal`
- `url_imagem_perfil_usuario`

### 8.3 Exceções
- Tabelas de junção muitos-para-muitos podem usar ambos os termos no singular (ex: `usuario_papel` para relacionar usuários e papéis).
- Algumas entidades naturalmente plurais podem manter essa forma (ex: `dados_demograficos`).

Exemplos de exceções com três ou mais palavras:
- `usuario_grupo_permissao` (tabela de junção)
- `metricas_desempenho_vendas` (naturalmente plural)

### 8.4 Regras Adicionais para Nomes Longos
- Abrevie somente quando necessário
- Mantenha os nomes concisos, mas descritivos.
- Se um nome ficar muito longo, considere abreviações comumente entendidas no contexto do seu domínio.
- Seja consistente com abreviações em todo o esquema do banco de dados.

----

Lembre-se de que a consistência é mais importante que a regra em si. As regras acima são apenas sugestões e podem ser alteradas conforme necessidade do projeto. Uma vez escolhido um padrão, mantenha-o em todo o banco de dados. Documente quaisquer abreviações ou convenções específicas do projeto para garantir que toda a equipe esteja alinhada.
