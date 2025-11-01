# 📊 Projeto Power BI – Dashboard Gatito Petshop

<img width="895" height="499" alt="image" src="https://github.com/user-attachments/assets/b0c9ff92-f078-46ed-9d5e-e03107d4a3be" />


## 🐾 1. Apresentação do Projeto

**Nome:** Dashboard Financeiro – *Gatito Petshop*  
**Ferramenta:** Microsoft Power BI  
**Objetivo:** Criar um painel interativo para análise de vendas, faturamento e comportamento dos clientes, consolidando dados de múltiplas fontes (TXT, Excel e Google Sheets).

O projeto tem como foco o acompanhamento de:
- Faturamento total e por categoria (gênero, bairro, período)
- Quantidade de vendas
- Média de pets por cliente
- Produtos e marcas mais vendidos

---

## ⚙️ 2. Metodologia

O processo foi dividido em cinco etapas principais, conforme as aulas do curso:

### **Aula 01 – Importação e Tratamento de Dados**
- Importação do arquivo `Cliente.txt`
- Ajuste de delimitadores e cabeçalhos
- Definição de tipos de dados (texto, número, data)
- Uso da opção **“Usar primeira linha como cabeçalho”**
- Aplicação de transformações e fechamento com **“Fechar e Aplicar”**

**Métrica criada:**  
> Média de Pets = Média da coluna `Pets` (medida implícita)

---

### **Aula 02 – Modelagem e Relacionamentos**
- Importação e combinação de planilhas anuais (pasta de origem)
- Correção de erros de relacionamento N x N (campos vazios)
- Criação manual da relação entre `Clientes[IDConsumidor]` e `Vendas[IDConsumidor]`
- Criação de colunas calculadas e medidas explícitas via DAX

**Colunas calculadas:**
```DAX
Valor_unitario = RELATED(Produtos[Valor])
Faturamento = 'Vendas'[Quantidade] * 'Vendas'[Valor_unitario]
```

**Medida explícita:**
```DAX
Faturamento_Total = SUM('Vendas'[Faturamento])
```

**Conceitos aplicados:**
- Diferença entre medidas **implícitas** (arraste direto) e **explícitas** (via código)
- Criação de medidas virtuais armazenadas em memória

---

### **Aula 03 – Visualizações**

**Gráfico de Pizza:**  
- **Objetivo:** Exibir faturamento total por gênero  
- **Campos:**  
  - Legenda: `Clientes[Gênero]`  
  - Valores: `Vendas[Faturamento_Total]`

**Gráfico de Barras Clusterizado:**  
- **Objetivo:** Faturamento total por bairro  
- **Campos:**  
  - Eixo Y: `Clientes[Bairro]`  
  - Valores: `Vendas[Faturamento_Total]`

**Gráfico de Área (séries temporais):**  
- **Objetivo:** Evolução do faturamento ao longo do tempo  
- **Campos:**  
  - Eixo X: `Vendas[DataCompra]`  
  - Valores: `Vendas[Faturamento_Total]`

**Visuais adicionais:**
- **Image Grid:** Exibição de imagens de produtos (`Produtos[URL Img]`)
- **Text Filter:** Busca por nome do produto (`Produtos[Nome]`)
- **Segmentação de Dados:**  
  - Por **Marca** (`Produtos[Marca]`)  
  - Por **Data** (`Vendas[DataCompra]` com Range Slider)

---

### **Aula 04 – Estilo e Layout**

- Importação de imagem de fundo personalizada (tema *Gatito Petshop*)
- Ajuste de transparência e cores no painel
- Aplicação de **pincel de formatação** para uniformizar visuais
- Organização dos KPIs principais no topo:
  - Faturamento Total
  - Média de Pets
  - Quantidade de Vendas

**Design adotado:**
- Paleta: Roxo (#7A3E9D), Amarelo (#F9E547), Branco (#FFFFFF)
- Tipografia legível e ícones ilustrativos
- Dashboard centralizado com visual limpo e moderno

---

### **Aula 05 – Publicação e Layout Móvel**

- Configuração do layout móvel em **Exibição → Layout Móvel**
- Ajuste de elementos para visualização responsiva
- Inserção de formas e transparências para guiar o layout
- Publicação em **My Workspace** via Power BI Service
- Testes de exibição Web e Mobile

**Boas práticas aplicadas:**
- Separação de ambiente Desktop e Service
- Configuração de filtros para segurança dos dados
- Organização de medidas e campos em pastas no painel de campos

---

## 🧩 3. Modelagem de Dados

### **Tabelas Utilizadas**
| Tabela       | Origem                      | Descrição |
|---------------|-----------------------------|------------|
| `Clientes`   | Arquivo TXT (`Cliente.txt`)  | Informações cadastrais e demográficas dos clientes |
| `Vendas`     | Pasta com planilhas anuais Excel | Registros de transações, datas e quantidades vendidas |
| `Produtos`   | Google Sheets (importado via Web CSV) | Dados de produtos, preços e URLs das imagens |

### **Relacionamentos**
| Tabela Origem | Tabela Destino | Campo Chave | Tipo de Relacionamento |
|----------------|----------------|--------------|------------------------|
| `Clientes` | `Vendas` | IDConsumidor | 1 : * |
| `Produtos` | `Vendas` | IDProduto | 1 : * |

---

## 🗂️ 4. Dicionário de Dados

### **Tabela: Clientes**
| Campo | Tipo | Descrição |
|--------|------|------------|
| IDConsumidor | Inteiro | Identificador único do cliente |
| Nome | Texto | Nome do cliente |
| Gênero | Texto | M/F |
| Bairro | Texto | Localização do cliente |
| Pets | Número | Quantidade de animais de estimação |

### **Tabela: Vendas**
| Campo | Tipo | Descrição |
|--------|------|------------|
| IDVenda | Inteiro | Identificador da venda |
| IDConsumidor | Inteiro | Chave estrangeira (Cliente) |
| IDProduto | Inteiro | Chave estrangeira (Produto) |
| Quantidade | Número | Total de produtos vendidos |
| DataCompra | Data | Data da venda |
| Faturamento | Moeda | Valor total da transação |

### **Tabela: Produtos**
| Campo | Tipo | Descrição |
|--------|------|------------|
| IDProduto | Inteiro | Identificador único do produto |
| Nome | Texto | Nome do produto |
| Marca | Texto | Marca do produto |
| Valor | Moeda | Preço unitário |
| URL Img | Texto | Caminho da imagem do produto |

---

## 💡 5. Principais Indicadores (KPIs)

| Indicador | Cálculo | Valor (Exemplo) | Descrição |
|------------|----------|----------------|------------|
| **Faturamento Total** | `SUM(Vendas[Faturamento])` | R$ 2,03 Mi | Soma total das vendas |
| **Média de Pets** | `AVERAGE(Clientes[Pets])` | 2,61 | Média de animais por cliente |
| **Quantidade de Vendas** | `SUM(Vendas[Quantidade])` | 57 mil | Total de produtos vendidos |

---

## 🎨 6. Visualizações Criadas

- **KPI Cards:** Exibem Faturamento, Média de Pets e Qtd. de Vendas  
- **Gráfico de Pizza:** Faturamento por Gênero  
- **Gráfico de Barras:** Faturamento por Bairro  
- **Gráfico de Área:** Evolução temporal do faturamento  
- **Text Filter:** Busca de produtos  
- **Image Grid:** Exibição de imagens de produtos  
- **Segmentadores:** Filtro de marca e data

---

## 🌐 7. Publicação e Compartilhamento

- Publicado no Power BI Service → *My Workspace*  
- Layout otimizado para **Web e Mobile**  
- Compartilhamento com usuários autenticados via conta Microsoft  
- Controle de visualização por níveis de acesso e filtros

---

## ✅ 8. Conclusões

O **Dashboard Gatito Petshop** consolidou informações de clientes, produtos e vendas em uma única visão interativa, permitindo:
- Identificar padrões de consumo e preferências dos clientes;
- Acompanhar evolução do faturamento e volumes de vendas;
- Filtrar por marcas, produtos e períodos de forma dinâmica;
- Visualizar métricas-chave em formato amigável para desktop e mobile.

---

## 🧠 9. Aprendizados e Próximos Passos

**Aprendizados principais:**
- Integração de múltiplas fontes de dados (TXT, Excel, Google Sheets)
- Uso de Power Query para tratamento e transformação
- Modelagem relacional e criação de medidas DAX
- Aplicação de boas práticas visuais e publicação

## 🔗 Sobre o Autor
https://github.com/carlosmendesjmc
