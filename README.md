# DICAS_do_USO_de_SQL
Aquei separei todos os problemas que vejo no dia-a-dia do meu aprendizado e que consegui solucionar, segue abaixo os exemplos.
**Dicas SQL: Sintaxe e Exemplos**

1. **COUNT com GROUP BY:**
   - **Regra:** Ao usar a função COUNT com outras colunas, é necessário agrupar pelos campos não agregados.
   - **Exemplo:**
     ```sql
     SELECT department, COUNT(employee_id)
     FROM employees
     GROUP BY department;
     ```

2. **WHERE vs HAVING:**
   - **Regra:** Use WHERE para filtrar linhas antes da agregação e HAVING para filtrar após a agregação.
   - **Exemplo:**
     ```sql
     SELECT department, AVG(salary)
     FROM employees
     WHERE salary > 50000
     GROUP BY department
     HAVING AVG(salary) > 60000;
     ```

3. **JOIN:**
   - **Regra:** Utilize JOIN para combinar dados de duas ou mais tabelas com base em uma condição.
   - **Exemplo:**
     ```sql
     SELECT employees.employee_id, employees.employee_name, departments.department_name
     FROM employees
     JOIN departments ON employees.department_id = departments.department_id;
     ```

4. **ORDER BY:**
   - **Regra:** Utilize ORDER BY para classificar os resultados em ordem ascendente (ASC) ou descendente (DESC).
   - **Exemplo:**
     ```sql
     SELECT product_name, unit_price
     FROM products
     ORDER BY unit_price DESC;
     ```

5. **Subconsultas (Subqueries):**
   - **Regra:** Subconsultas podem ser usadas em cláusulas WHERE, FROM ou SELECT para realizar consultas mais complexas.
   - **Exemplo:**
     ```sql
     SELECT employee_name
     FROM employees
     WHERE department_id IN (SELECT department_id FROM departments WHERE department_name = 'IT');
     ```

6. **Alias de Tabela e Coluna:**
   - **Regra:** Use aliases para fornecer nomes mais legíveis às tabelas e colunas.
   - **Exemplo:**
     ```sql
     SELECT e.employee_id AS ID, e.employee_name AS Name, d.department_name AS Department
     FROM employees e
     JOIN departments d ON e.department_id = d.department_id;
     ```

7. **Erro de Aspas Simples:**
   - **Erro:** Falta ou excesso de aspas simples ao redor de valores de texto.
   - **Solução:** Certifique-se de usar aspas simples corretamente.
     ```sql
     -- Errado
     SELECT column_name FROM table_name WHERE name = John;

     -- Correto
     SELECT column_name FROM table_name WHERE name = 'John';
     ```

8. **Erro de Ponto e Vírgula:**
   - **Erro:** Esquecimento do ponto e vírgula no final da consulta.
   - **Solução:** Adicione o ponto e vírgula no final da consulta.
     ```sql
     -- Errado
     SELECT column_name FROM table_name WHERE condition

     -- Correto
     SELECT column_name FROM table_name WHERE condition;
     ```

9. **Erro de Nome de Coluna Ambíguo:**
   - **Erro:** Uso de um nome de coluna ambíguo sem alias.
   - **Solução:** Utilize aliases ou especifique a tabela ao qualificar a coluna.
     ```sql
     -- Errado
     SELECT id FROM table1, table2 WHERE id = 1;

     -- Correto
     SELECT table1.id FROM table1, table2 WHERE table1.id = 1;
     ```

10. **Erro de Comentário Não Fechado:**
   - **Erro:** Comentário SQL não fechado corretamente.
   - **Solução:** Certifique-se de fechar corretamente os comentários.
     ```sql
     -- Errado
     /* Este é um comentário SQL não fechado

     -- Correto
     /* Este é um comentário SQL corretamente fechado */
     ```

11. **Erro de Uso Incorreto de JOIN:**
   - **Erro:** Uso incorreto da cláusula JOIN.
   - **Solução:** Especifique corretamente a condição de junção.
     ```sql
     -- Errado
     SELECT column_name FROM table1 JOIN table2;

     -- Correto
     SELECT column_name FROM table1 JOIN table2 ON table1.id = table2.id;
     ```

12. **Erro de Uso de Palavras-Chave Reservadas:**
   - **Erro:** Uso de palavras-chave reservadas sem aspas ou escapes.
   - **Solução:** Coloque as palavras-chave entre aspas ou utilize escapes, se necessário.
     ```sql
     -- Errado
     SELECT order, count FROM orders;

     -- Correto
     SELECT "order", "count" FROM orders;
     ```
13. **Erro ao utilizar alguns comandos DLL**
    - **Erro:** utilizou o comando ALTER TABLE sem a Palavra-chave
    - **solução:** ordenou o que fosse feito usando o comando e a palavra-chave
   ```sql
   -- Errado
   ALTER TABLE TBVEN FOREIGN KEY (CDCLI) REFERENCES TBCLI(CDCLI)
   -- Correto
   ALTER TABLE TBVEN ADD FOREIGN KEY (CDCLI) REFERENCES TBCLI(CDCLI)
```

14. **Erro de Agrupamento e Ordenação:**
   - **Erro:** Tentativa de selecionar colunas não agregadas sem agrupamento em uma consulta GROUP BY.
   - **Solução:** Inclua as colunas não agregadas na cláusula GROUP BY ou use funções de agregação.
     ```sql
     -- Errado
     SELECT department, employee_name FROM employees GROUP BY department;

     -- Correto
     SELECT department, COUNT(employee_name) FROM employees GROUP BY department;
     ```

15. **Erro de Sintaxe em Subconsulta:**
   - **Erro:** Subconsulta mal formada.
   - **Solução:** Verifique a sintaxe da subconsulta e certifique-se de que ela retorna um único valor ou conjunto de valores.
     ```sql
     -- Errado
     SELECT column_name FROM table_name WHERE column_name = (SELECT column_name FROM another_table WHERE condition);

     -- Correto
     SELECT column_name FROM table_name WHERE column_name IN (SELECT column_name FROM another_table WHERE condition);
     ```

16. **Erro de Nome de Tabela Incorreto:**
   - **Erro:** Referência a uma tabela inexistente ou com nome incorreto.
   - **Solução:** Verifique e corrija o nome da tabela.
     ```sql
     -- Errado
     SELECT column_name FROM non_existing_table;

     -- Correto
     SELECT column_name FROM existing_table;
     ```
17. **Consulta Convertendo para Maiúsculas e Aplicando um Filtro:**
```sql
-- Errado
SELECT employee_id, employee_name
FROM employees
WHERE employee_name = 'John';
  
-- Correto
SELECT employee_id, UPPER(employee_name) AS employee_name_upper
FROM employees
WHERE UPPER(employee_name) = 'JOHN';
```

18. **Agrupamentos e funções de agregação**
    - **Erro:** Uso de duas coluas para agrupamente por data entrega mais de um resultado por datas iguais.
    - **Solução:** Utilizar apenas um agrupamento no group by e usar uma função de agregação(MAX) para entregar o maior valor daquela data

```sql
-- consulta errada
WITH noshow AS (
    SELECT
        mpv.data_venda,
        mpo.data_orcamento,
        mpo.valor_total as orcamento,
        mpv.valor_total as vendas
    FROM
        moda_praia_produtos mpp
    CROSS JOIN
        moda_praia_vendas mpv
    LEFT JOIN
        moda_praia_orcamento mpo ON mpp.ID_produto = mpo.ID_produto AND mpv.data_venda = mpo.data_orcamento
)

SELECT
    ns.data_venda AS data_venda,
    ns.data_orcamento AS data_orcamento,
    SUM(ns.orcamento) AS orcamento_total,
    SUM(ns.vendas) AS vendas_total
FROM
    noshow ns
LEFT JOIN
    moda_praia_vendas v ON ns.data_venda = v.data_venda
GROUP BY
    ns.data_venda, ns.data_orcamento
ORDER BY
    ns.data_venda;

-- Consulta Correta:


WITH noshow AS (
    SELECT
        mpv.data_venda,
        mpo.data_orcamento,
        mpo.valor_total as orcamento,
        mpv.valor_total as vendas
    FROM
        moda_praia_produtos mpp
    CROSS JOIN
        moda_praia_vendas mpv
    LEFT JOIN
        moda_praia_orcamento mpo ON mpp.ID_produto = mpo.ID_produto AND mpv.data_venda = mpo.data_orcamento
)

SELECT
    ns.data_venda AS data,
    MAX(ns.data_orcamento) AS data_orcamento,
    SUM(ns.orcamento) AS orcamento_total,
    SUM(ns.vendas) AS vendas_total
FROM
    noshow ns
LEFT JOIN
    moda_praia_vendas v ON ns.data_venda = v.data_venda
GROUP BY
    ns.data_venda
ORDER BY
    ns.data_venda;
```


**Operadores SQL: Uso e Exemplos**

1. **IS:**
   - **Uso:** Testa se um valor é igual a `NULL`.
   - **Exemplo:**
     ```sql
     -- Errado
     SELECT column_name
     FROM table_name
     WHERE column_name = NULL;

     -- Correto
     SELECT column_name
     FROM table_name
     WHERE column_name IS NULL;
     ```

2. **AND:**
   - **Uso:** Combina condições em uma cláusula WHERE, ambas devem ser verdadeiras.
   - **Exemplo:**
     ```sql
     -- Correto
     SELECT column1, column2
     FROM table_name
     WHERE column1 > 100 AND column2 = 'value2';
     ```

3. **OR:**
   - **Uso:** Combina condições em uma cláusula WHERE, pelo menos uma deve ser verdadeira.
   - **Exemplo:**
     ```sql
     
     -- Correto
     SELECT column1, column2
     FROM table_name
     WHERE column1 > 100 OR column2 = 'value2';
     ```

4. **BETWEEN:**
   - **Uso:** Filtra o resultado dentro de um intervalo específico.
   - **Exemplo:**
     ```sql
     -- Errado
     SELECT column_name
     FROM table_name
     WHERE column_name > 10 AND column_name < 50;

     -- Correto
     SELECT column_name
     FROM table_name
     WHERE column_name BETWEEN 10 AND 50;
     ```

5. **> (maior que), < (menor que):**
   - **Uso:** Compara valores para verificar se um é maior ou menor que o outro.
   - **Exemplo:**
     ```sql
    
     -- Correto
     SELECT column_name
     FROM table_name
     WHERE column_name > 50;
     ```

6. **= (igual a):**
   - **Uso:** Compara se os valores de duas expressões são iguais.
   - **Exemplo:**
     ```sql
   
     -- Correto
     SELECT column1, column2
     FROM table_name
     WHERE column1 = 'value';
     ```
