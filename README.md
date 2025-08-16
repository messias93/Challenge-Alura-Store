# Challenge-Alura-Store
Faz parte do curso de Ciência de dados na plataforma Alura em parceria com a Oracle. O Challenge é uma forma de aprendizado com desafios práticos. 
# Análise de Varejo – 4 Lojas (Google Colab)

Projeto simples para responder às perguntas do exercício com **pandas** e **matplotlib** usando dados de 4 lojas.

## Objetivo

* Calcular **faturamento** (apenas **Preço**, sem frete).
* Ver **faturamento por loja** e **por categoria** (separado por loja).
* Medir **média de avaliação por loja**.
* Listar **produtos mais e menos vendidos**.
* Ver **frete médio por loja**.
* Registrar uma **conclusão**: qual loja vender.

## Requisitos

```
pip install pandas matplotlib babel
```

## Como usar (Colab)

1. Carregue os 4 DataFrames do GitHub: `loja`, `loja2`, `loja3`, `loja4`.
2. Execute o bloco de preparação abaixo.
3. Rode os blocos curtos de cada análise.

## Preparação (mínima)

```python
import pandas as pd
import matplotlib.pyplot as plt
from babel.numbers import format_currency

# Identifica origem e consolida
loja["Loja"], loja2["Loja"], loja3["Loja"], loja4["Loja"] = "Loja 1", "Loja 2", "Loja 3", "Loja 4"
lojas = pd.concat([loja, loja2, loja3, loja4], ignore_index=True)

# Tipos essenciais
lojas["Preço"] = pd.to_numeric(lojas["Preço"], errors="coerce")
lojas["Frete"] = pd.to_numeric(lojas["Frete"], errors="coerce")
lojas["Avaliação da compra"] = pd.to_numeric(lojas["Avaliação da compra"], errors="coerce")

# Métrica base: faturamento bruto = apenas Preço
lojas["Faturamento"] = lojas["Preço"]
```

## Blocos essenciais

**Faturamento por loja**

```python
lojas.groupby("Loja", as_index=False)["Faturamento"].sum()
```

**Faturamento por categoria × loja**

```python
lojas.pivot_table(values="Faturamento", index="Categoria do Produto", columns="Loja", aggfunc="sum", fill_value=0)
```

**Média de avaliação por loja**

```python
lojas.groupby("Loja", as_index=False)["Avaliação da compra"].mean()
```

**Top produtos (mais vendidos)**

```python
v = lojas.groupby("Produto", as_index=False).agg(Qtd=("Produto","size"), Faturamento=("Faturamento","sum"))
v.sort_values(["Qtd","Faturamento"], ascending=[False,False]).head(10)
```

**Frete médio por loja**

```python
lojas.groupby("Loja", as_index=False)["Frete"].mean()
```

**Formatação em R\$ (opcional, apenas na exibição)**

```python
format_currency(1234.56, "BRL", locale="pt_BR")  # => R$ 1.234,56
```

## Gráficos rápidos (exemplos)

**Faturamento por loja (barras)**

```python
fat = lojas.groupby("Loja", as_index=False)["Faturamento"].sum()
plt.bar(fat["Loja"], fat["Faturamento"])
plt.title("Faturamento por Loja"); plt.ylabel("R$")
plt.show()
```

**Avaliação média por loja (linha)**

```python
m = lojas.groupby("Loja", as_index=False)["Avaliação da compra"].mean()
plt.plot(m["Loja"], m["Avaliação da compra"], marker="o")
plt.ylim(0,5); plt.title("Média de Avaliação"); plt.ylabel("Média")
plt.show()
```

## Decisão (resumo)

* Comparar: menor **faturamento**, menor **participação nas top categorias**, **avaliação** sem vantagem, **mix** fraco e **frete médio** alto.
* No conjunto analisado, a **Loja 4** foi indicada para venda (confirme com seus resultados no notebook).

