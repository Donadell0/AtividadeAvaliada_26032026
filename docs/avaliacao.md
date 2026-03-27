# Avaliação — Engenharia de Software
**Sistema Integrado de Gestão de Farmácia — MVP Definido pelo Estudante**

Aluno: Gustavo de Moraes Donadello    
RA: 26001336  
Data: 26/03/2026  

---

# 1. Definição do MVP
Meu MVP cobre o processo de venda desde a identificação/cadastro do cliente até a emissão do comprovante, incluindo o tratamento de estoque insuficiente, e se estende ao registro automático de contas a receber quando a venda for realizada a prazo.

O nucleo do MVP é o fluxo completo de venda ao balcão, sendo eles, registrar venda, verificar estoque, emitir comprovante, atualizar estoque cadastrar e consultar cliente, realizar venda a prazo, registrar contas a receber, contas de fornecedor, atualizar estoque via compra e registrar contas a pagar 

Está de fora coisas como validar receita medica, emitir alerta de estoque minimo e gerar relatorio 

A seleção do MVP foi guiada pela seguinte lógica: o sistema precisa, antes de tudo, substituir o processo manual de vendas e garantir que o estoque seja confiável. Esses dois problemas são os que mais impactam a operação diária e os que mais geram prejuízo à rede atualmente.

---

# 2. Regras de Negócio 

**RN01 —**  Um produto somente pode ser vendido se houver quantidade disponível no estoque da unidade em que a venda está sendo realizada.
**RN02 —**  Vendas a prazo geram automaticamente um lançamento em Contas a Receber
**RN03 —**  Toda compra de fornecedor gera automaticamente um lançamento em Contas a Pagar
**RN04 —**  O estoque de cada unidade deve ser atualizado automaticamente após qualquer operação de venda, devolução, perda, transferência ou recebimento de compra.
**RN05 —**  O sistema deve emitir um alerta quando a quantidade de um produto em estoque atingir ou ficar abaixo do nível mínimo cadastrado.
**RN06 —**  Vendas de medicamentos controlados só podem ser finalizadas com a validação de receita médica pelo farmacêutico responsável.
**RN07 —**  Cada usuário possui um perfil com permissões específicas
**RN08 —**  Um cliente pode ser cadastrado durante o atendimento, permitindo vincular a compra ao seu histórico imediatamente.

---

# 3. Requisitos Funcionais 

**RF01 —**  O sistema deve permitir registrar vendas de produtos, informando cliente, itens, quantidades e forma de pagamento.
**RF02 —**  O sistema deve consultar produtos por nome, código de barras ou fabricante.
**RF03 —**  O sistema deve verificar a disponibilidade de estoque antes de confirmar uma venda.
**RF04 —**  O sistema deve emitir comprovante ao término de cada venda.
**RF05 —**  O sistema deve permitir o cadastro e a consulta de clientes.
**RF06 —**  O sistema deve registrar e controlar as contas a receber, com status
**RF07 —**  O sistema deve registrar e controlar as contas a pagar, com status
**RF08 —**  O sistema deve registrar compras de fornecedores e atualizar o estoque da unidade correspondente.
**RF09 —**  O sistema deve permitir transferências de produtos entre unidades da rede.
**RF10 —**  O sistema deve gerar relatórios de vendas, estoque, contas a pagar/receber e produtos mais vendidos.
**RF11 —**  O sistema deve emitir alertas de estoque mínimo para os gerentes
**RF12 —**  O sistema deve permitir o gerenciamento de usuários e seus perfis de acesso.

---

# 🛡 4. Requisitos Não Funcionais 

**RNF01 —**  Desempenho: consultas a produtos e estoque devem retornar resultados em até 2 segundos.
**RNF02 —**  Segurança: o acesso ao sistema deve ser realizado mediante autenticação com login e senha; dados sensíveis devem ser armazenados de forma criptografada.
**RNF03 —**  Disponibilidade: o sistema deve estar disponível 99,5% do tempo em horário comercial, com tolerância a falhas e recuperação automática.
**RNF04 —**  Usabilidade: a interface deve ser intuitiva, permitindo que atendentes realizem uma venda completa sem treinamento superior a 4 horas.
**RNF05 —**  Escalabilidade: o sistema deve suportar a adição de novas unidades da rede sem necessidade de refatoração da arquitetura.

---

# 5. Casos de Uso 
![DiagramaGeral](./imgs/diagramaGeral)

---

# 6. Documentação dos Casos de Uso
Para **cada caso de uso**, utilize o template abaixo:
---

## **UC01 — Registrar Venda**
**Ator(es):**  Atendente
**Descrição:**  O atendendo irá registrar uma venda no sistema.
**Pré-condições:**  Atendente autenticado; produto cadastrado no sistema
**Pós-condições:**  Venda registrada; estoque atualizado; comprovante emitido

### Fluxo Principal
1.  Atendente inicia nova venda
2.  Pesquisa produto por nome, código ou fabricante
3.  Informa quantidade desejada
4.  Sistema verifica estoque
5.  Atendente identifica ou cadastra cliente
6.  Confirma forma de pagamento
7.  Sistema registra a venda e emite comprovante

### Fluxos Alternativos / Exceções
- FA01 —  Produto sem estoque: sistema alerta e bloqueia o item

### Relacionamentos
- **Include:** Sistema registra a venda e emite comprovante <<include>>.
- **Include:** Sistema verifica estoque <<include>>
-  
- **Extend:** Se pagamento a prazo: <<extend>> UC02
- **Extend:** Se produto controlado: <<extend>> UC10.

![DiagramaUC01](./imgs/UC01)

---

## **UC02 — Realizar Venda a Prazo**
**Ator(es):**  Atendente
**Descrição:**  O atendendo irá realizar uma venda a prazo no sistema.
**Pré-condições:**  UC01 em execução; cliente cadastrado
**Pós-condições:**  Lançamento gerado em Contas a Receber

### Fluxo Principal
1.  Atendente seleciona pagamento a prazo
2.  Informa data de vencimento ou número de parcelas
3.  Sistema gera lançamento em Contas a Receber com status Aberta.


### Relacionamentos
- **Regras de Negócio** RN02

![DiagramaUC02](./imgs/UC02)

---

## **UC03 — Verificar Estoque**
**Ator(es):**  Sistema (automático)
**Descrição:**  O atendendo irá realizar uma verificação de estoque no sistema.
**Pré-condições:**  Produto e unidade identificados
**Pós-condições:**  Confirmação ou rejeição da disponibilidade

### Fluxo Principal
1.  Sistema consulta estoque da unidade
2.  Compara quantidade solicitada com disponível
3.  Retorna resultado para o caso de uso chamador


### Relacionamentos
- **Regras de Negócio** RN01, RN05

![DiagramaUC03](./imgs/UC03)

---

## **UC04 — Emitir Comprovante**
**Ator(es):**  Sistema (automático)
**Descrição:**  Após uma venda realizada o sistema irá emitir um comprovante da venda
**Pré-condições:**  Venda confirmada
**Pós-condições:**  Comprovante gerado com detalhes da operação

### Fluxo Principal
1.  Sistema coleta dados da venda.
2.  Gera documento com itens, valores, data e forma de pagamento
3.  Disponibiliza para impressão ou envio digital.


### Relacionamentos
- **Regras de Negócio** RN01

![DiagramaUC04](./imgs/UC04)

---

## **UC05 — Registrar Compra de Fornecedor**
**Ator(es):**  Gerente
**Descrição:**  O gerente irá registrar uma compra feita com um fornecedor.
**Pré-condições:**  Fornecedor e produtos cadastrados
**Pós-condições:**  Estoque atualizado; lançamento em Contas a Pagar

### Fluxo Principal
1.  Gerente informa fornecedor, produto, quantidade, valor e data.
2.  Sistema registra a compra
3.  Disponibiliza para impressão ou envio digital.
4.  Sistema atualiza o estoque
5.  Sistema lança em Contas a Pagar


### Relacionamentos
- **Include:** Sistema atualiza o estoque <<include>>
- **Include:** Sistema lança em Contas a Pagar <<include>>.
- **Regras de Negócio** RN03, RN04

![DiagramaUC05](./imgs/UC05)

---

## **UC06 — Atualizar Estoque**
**Ator(es):**  Sistema (automático)
**Pré-condições:**  Operação de venda, compra, devolução ou transferência concluída
**Pós-condições:**  Quantidade em estoque atualizada

### Fluxo Principal
1.  Sistema identifica operação realizada
2.  Incrementa ou decrementa quantidade no estoque da unidade
3.  Verifica se quantidade ficou abaixo do mínimo. Se sim: <<extend>> UC11.

### Relacionamentos
- **Extend:** Verifica se quantidade ficou abaixo do mínimo. Se sim: <<extend>> UC11.
- **Regras de Negócio** RN04, RN05

![DiagramaUC06](./imgs/UC06)

---

## **UC07 — Registrar Contas a Pagar**
**Ator(es):**  Sistema (automático) / Financeiro
**Pré-condições:**  Compra registrada ou lançamento manual iniciado
**Pós-condições:**  Lançamento de conta a pagar criado

### Fluxo Principal
1.  Sistema ou usuário Financeiro informa descrição, valor e vencimento
2.  Sistema cria lançamento com status Aberta
3.  Financeiro pode consultar e atualizar status (Paga, Atrasada).

### Relacionamentos
- **Regras de Negócio** RN03

![DiagramaUC07](./imgs/UC07)

---

## **UC08 — Registrar Contas a Receber**
**Ator(es):**  Sistema (automático) / Financeiro
**Pré-condições:**  Venda a prazo registrada
**Pós-condições:**  Lançamento de conta a receber criado

### Fluxo Principal
1.  Sistema cria lançamento com valor, vencimento e status Aberta
2.  Financeiro consulta e registra recebimentos
3.  Status é atualizado para Recebida ou Atrasada conforme prazo.

### Relacionamentos
- **Regras de Negócio** RN02

![DiagramaUC08](./imgs/UC08)

---

## **UC09 — Consultar Produto**
**Ator(es):**  Atendente, Gerente
**Pré-condições:**  Usuário autenticado
**Pós-condições:**  Informações do produto exibidas

### Fluxo Principal
1.  Usuário informa nome, código de barras ou fabricante
2.  Sistema retorna lista de produtos correspondentes com preço, estoque e detalhes.

### Relacionamentos
- **Regras de Negócio** RN07

![DiagramaUC09](./imgs/UC09)

---

## **UC10 — Validar Receita Médica**
**Ator(es):**  Farmacêutico
**Pré-condições:**  Venda de produto controlado iniciada
**Pós-condições:**  Receita validada; venda liberada ou bloqueada

### Fluxo Principal
1.  Sistema identifica produto controlado
2.  Solicita validação do farmacêutico
3.  Farmacêutico analisa receita e confirma ou rejeita
4.  Se aprovada, venda prossegue. Se rejeitada, venda é cancelada.

### Relacionamentos
- **Regras de Negócio** RN06

![DiagramaUC10](./imgs/UC10)

---

## **UC11 — Emitir Alerta de Estoque Mínimo**
**Ator(es):**  Sistema (automático)
**Pré-condições:**  Quantidade em estoque ≤ nível mínimo cadastrado
**Pós-condições:**  Alerta enviado ao gerente da unidade

### Fluxo Principal
1.  Sistema detecta quantidade abaixo do mínimo após atualização de estoque
2.  Gera notificação para o gerente da unidade com produto, quantidade atual e mínimo configurado.

### Relacionamentos
- **Regras de Negócio** RN05

![DiagramaUC11](./imgs/UC11)


---

## **UC12 — Gerar Relatório**
**Ator(es):**  Gerente, Financeiro, Administrador
**Pré-condições:**  Usuário com perfil autorizado
**Pós-condições:**  Relatório gerado e disponibilizado

### Fluxo Principal
1.  Usuário seleciona tipo de relatório (vendas, estoque, financeiro, fornecedores)
2.  Informa período e filtros desejados.
3.  Sistema processa e exibe o relatório.

### Relacionamentos
- **Regras de Negócio** RN07

![DiagramaUC12](./imgs/UC12)

