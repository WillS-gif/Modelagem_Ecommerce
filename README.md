# Modelagem_Ecommerce
Modelo de um projeto referente a um ecommerce.


<img width="959" height="963" alt="modelo ecommerce v2" src="https://github.com/user-attachments/assets/377cf9e9-e88f-40ce-ac55-b4fe42329e2d" />



Nesse projeto foi utilizado o MySQL para criação do modelo.

📌 Regras de Cardinalidade

🔹 Cliente – Cliente_PF / Cliente_PJ

Cliente 1:0..1 Cliente_PF

Cliente 1:0..1 Cliente_PJ

* Cada cliente pode ser apenas PF ou PJ, nunca os dois ao mesmo tempo.
Isso garante a exclusividade: um cliente cadastrado como PF não terá dados de PJ.



🔹 Cliente – Pedido

Cliente 1:N Pedido

* Um cliente pode fazer vários pedidos, mas cada pedido pertence a um único cliente.
  

 🔹 Pedido – Pagamento

Pedido 1:N Pagamento

* Um pedido pode ser pago com uma ou mais formas de pagamento (exemplo: metade no cartão, metade no pix).
Cada pagamento está sempre vinculado a um único pedido.


🔹 Pedido – Entrega

Pedido 1:N Entrega

* Um pedido pode ter várias entregas (caso seja dividido em pacotes), mas cada entrega pertence a um único pedido.
Na entrega temos status e código de rastreio.



🔹 Pedido – Produto

Pedido N:M Produto

* Um pedido pode conter vários produtos e cada produto pode estar em vários pedidos.
Essa relação é resolvida pela tabela associativa (ex.: ItemPedido) com:

(idPedido, idProduto, Quantidade, ValorUnitario)



🔹 Produto – Fornecedor

Produto N:M Fornecedor

*Um produto pode ser fornecido por diversos fornecedores, e um fornecedor pode fornecer diversos produtos.
Isso exige uma tabela associativa (ex.: ProdutoFornecedor).


🔹 Produto – Estoque

Produto N:M Estoque

*Um produto pode estar em vários estoques (ex.: centros de distribuição diferentes), e cada estoque pode conter vários produtos.
Tabela associativa: ProdutoEstoque (com quantidade).


🔹 Produto – Vendedor (Terceiro)

Produto N:M Vendedor

* Um produto pode ser vendido por vários vendedores terceiros (marketplace), e cada vendedor pode ter vários produtos.
Tabela associativa: ProdutoVendedor.

Segue algumas consultas:

1. Listar todos os clientes (PF e PJ)
SELECT c.idCliente,
       c.Nome,
       c.Identificação,
       c.Endereço
FROM Cliente c;


2. Listar todos os produtos disponíveis e seus valores
SELECT p.idProduto,
       p.Categoria,
       p.Descrição,
       p.Valor
FROM Produto p;


3. Consultar pedidos com seus respectivos clientes
SELECT pe.idPedido,
       pe.Status_do_pedido,
       pe.Descrição,
       c.Nome AS Cliente,
       pe.Frete
FROM Pedido pe
JOIN Cliente c ON pe.Cliente_idCliente = c.idCliente;


4. Mostrar os produtos comprados em cada pedido
SELECT pe.idPedido,
       c.Nome AS Cliente,
       p.Descrição AS Produto,
       rp.Quantidade
FROM Pedido pe
JOIN Cliente c ON pe.Cliente_idCliente = c.idCliente
JOIN Relação_de_Produto_pedido rp ON rp.Pedido_idPedido = pe.idPedido
JOIN Produto p ON rp.Produto_idProduto = p.idProduto;


5. Consultar entregas com código de rastreio e status
SELECT e.idEntrega,
       e.CodigoRastreio,
       e.Status,
       e.DataEnvio,
       e.DataEntrega
FROM Entrega e;


6. Mostrar pagamentos realizados por pedido
SELECT pa.idPagamento,
       pa.idPedido,
       pa.TipoPagamento,
       pa.Valor,
       pa.DataPagamento
FROM Pagamento pa;


7. Listar fornecedores e os produtos que eles disponibilizam
SELECT f.idFornecedor,
       f.RazãoSocial,
       p.Descrição AS Produto,
       p.Valor
FROM Fornecedor f
JOIN Disponibiliza_o_produto dp ON dp.Fornecedor_idFornecedor = f.idFornecedor
JOIN Produto p ON dp.Produto_idProduto = p.idProduto;

