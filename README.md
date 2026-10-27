# Conversor de Cálculo NFD (Devolução de NF-e)

Aplicação web em **HTML + JavaScript puro** para cálculo proporcional de devolução (NFD) com múltiplos itens, a partir de um arquivo **XML de NF-e**.

A ferramenta permite:
- importar XML da NF-e original;
- localizar produtos por `cProd`;
- informar quantidade a devolver;
- aplicar regras opcionais de conversão de unidade;
- calcular rateio proporcional (valor bruto, descontos, despesas, ST);
- demonstrar bases e impostos (ICMS, CBS, IBS);
- exibir totalizador geral da devolução.

---

## ✅ Funcionalidades

- **Upload de XML da NF-e**
  - Leitura do arquivo local via `FileReader`;
  - Parse XML com `DOMParser`;
  - Extração de itens (`det`), produto (`prod`), imposto (`imposto`) e dados da nota (`ide`, `emit`).

- **Conferência e devolução por produto**
  - Busca por código do produto (`cProd`);
  - Cálculo proporcional com base na quantidade devolvida;
  - Inclusão de múltiplos itens no relatório.

- **Regras de conversão (opcional)**
  - **CX_TO_UN**: converte caixas para unidades (multiplica pela quantidade por caixa);
  - **KG_TO_UN**: usa quantidade total de unidades informada externamente (ex.: MB51), desconsiderando proporcionalidade por KG do XML para o rateio unitário.

- **Cálculo tributário e financeiro**
  - Valor bruto proporcional;
  - Descontos (comercial + desoneração);
  - Despesas acessórias (vOutro + vFrete + vSeg);
  - ICMS-ST proporcional;
  - Base e valor de ICMS próprio;
  - Base e valores de CBS e IBS.

- **Interface de relatório**
  - Cards por item com flags fiscais (NF, série, UF, CFOP, CST, etc.);
  - Tabela de valores unitários;
  - Demonstrativo de fórmulas;
  - Totalizador geral (ICMS próprio, ICMS-ST, CBS+IBS, valor líquido).

---

## 🧮 Lógica de cálculo (resumo)

Para cada item selecionado:

1. Define o total de unidades para rateio (`totalUnits`):
   - sem conversão: `qCom` do XML;
   - **CX_TO_UN**: `qCom * fator`;
   - **KG_TO_UN**: `fator` (quantidade total UN informada).

2. Calcula valores unitários de trabalho:
   - `workingUnCom = vProdTotal / totalUnits`
   - `workingUDesc = vDesc / totalUnits`
   - `workingUDeson = vDeson / totalUnits`
   - `workingUDespAces = vDespAces / totalUnits`
   - `workingUST = vICMSST / totalUnits`
   - `workingUBC_ICMS = vBC_ICMS / totalUnits`
   - `workingUBC_IBSCBS = vBC_IBSCBS / totalUnits`

3. Aplica quantidade devolvida (`qty`) para obter proporcionais:
   - `propBruto = workingUnCom * qty`
   - `propAbatimento = (workingUDesc + workingUDeson) * qty`
   - `propDespAces = workingUDespAces * qty`
   - `propST = workingUST * qty`
   - `propLiq = propBruto + propDespAces + propST - propAbatimento`

4. Tributos:
   - `propBC_ICMS = workingUBC_ICMS * qty`
   - `propValICMS = propBC_ICMS * (pICMS/100)`
   - `propBC_IBSCBS = workingUBC_IBSCBS * qty`
   - `vCBS = propBC_IBSCBS * (pCBS/100)`
   - `vIBS = propBC_IBSCBS * (pIBS/100)`

---

## 📁 Estrutura do projeto

- `index.html`  
  Redireciona automaticamente para a aplicação principal.
- `conversorXML_CALCULO_NFD.html`  
  Arquivo principal com interface, estilos e lógica JavaScript.
- `README.md`  
  Documentação do projeto.

---

## ▶️ Como usar

1. Abra a aplicação no navegador.
2. Selecione o arquivo XML da NF-e.
3. Informe:
   - `Código do Produto (cProd)`;
   - `Qtd. a Devolver`.
4. (Opcional) Habilite conversão e escolha a regra:
   - Caixa para unidade;
   - Quilo para unidade (MB51).
5. Clique em **Adicionar ao Relatório**.
6. Repita para outros produtos.
7. Consulte o **totalizador final** ao final da página.

---

## 🌐 Publicação no GitHub Pages

Este projeto pode ser publicado com GitHub Pages (via Actions).

1. Envie os arquivos para a branch `main`;
2. No GitHub: **Settings > Pages**;
3. Em **Build and deployment**, selecione **Source: GitHub Actions**;
4. Garanta que o workflow de deploy esteja em `.github/workflows/deploy-pages.yml`.

URL esperada:

`https://Nexte92.github.io/conversor_calculo_nfd/`

---

## ⚠️ Observações importantes

- Esta ferramenta é de **apoio operacional** para conferência e memória de cálculo.
- A interpretação fiscal/tributária pode variar por cenário, UF, regime e legislação vigente.
- Recomenda-se validação com o time fiscal/contábil antes da emissão definitiva dos documentos.

---

## 🛠️ Tecnologias

- HTML5
- CSS3
- JavaScript (Vanilla)
- DOMParser / FileReader API

---

## 📄 Licença

Defina aqui a licença do projeto (ex.: MIT), se aplicável.
