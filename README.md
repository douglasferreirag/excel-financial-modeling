# excel-financial-modeling
# 📊 Douglas Invest - Simulador de Planejamento de Investimentos em FIIs

![Excel](https://shields.io)
![Status](https://shields.io)

Um simulador profissional e automatizado de investimentos em **Fundos Imobiliários (FIIs)** desenvolvido no Microsoft Excel. O objetivo principal deste projeto é transformar lógicas de juros compostos e matrizes de dados complexas em uma **ferramenta com interface intuitiva (estilo aplicativo/SaaS)**, voltada para tomadas de decisão estratégica e planejamento financeiro de longo prazo.

---

## 🎯 Perguntas de Negócio Respondidas

A interface do simulador foi projetada para que qualquer usuário final consiga, através de apenas **3 inputs**, obter as respostas para as **5 principais métricas de planejamento**:

1. **Quanto investir por mês?** *(Entrada do usuário ou baseada na regra de 30% do salário)*
2. **Por quantos anos?** *(Horizonte temporal desejado)*
3. **Qual a taxa de rendimento mensal?** *(Previsão de crescimento real dos juros compostos)*
4. **Quanto de patrimônio vai acumular?** *(Capital acumulado ao fim do período)*
5. **Quanto vai receber de dividendos por mês?** *(Renda passiva estimada recorrente baseada no Yield médio da carteira)*

---

## ⚙️ Arquitetura Técnica e Engenharia de Fórmulas

Para garantir que o modelo siga padrões rígidos de governança corporativa, escalabilidade e performance, a arquitetura foi dividida estritamente em **Front-end (Aba `Simulador`)** e **Back-end (Aba `Apoio_Dados`)**.

### 1. Projeção de Cenários Avançada (Função `VF` com Travamentos Absolutos)
O cálculo do patrimônio futuro em múltiplos horizontes temporais (2, 5, 10, 20 e 30 anos) foi construído utilizando a função de **Valor Futuro (`VF`)**.
* **Fórmula Aplicada:** `=VF($B$13; D7*12; -$B$11; 0)`
* **Diferencial Técnico:** O uso rigoroso de referências absolutas (travamento com cifrões `$`) nas variáveis de taxa e aporte garante a integridade estrutural do arquivo. Isso permite que novas linhas ou cenários sejam arrastados sem quebrar o fluxo matemático.

### 2. Alocação Dinâmica por Perfil (PROCV com Chave Composta)
Em vez de utilizar dezenas de condições `SE` aninhadas (o que polui o arquivo e prejudica a performance), a inteligência de distribuição de ativos por perfil (Conservador, Moderado e Agressivo) foi modelada usando **Chave Composta**.
* No back-end, foi criada uma coluna unificando o perfil ao ativo (ex: `Moderado-PAPEL`).
* No front-end, a fórmula reconstrói essa chave de maneira dinâmica para ler a matriz:
  ```excel
  =PROCV($B$19&"-"&A22; Apoio_Dados!A:D; 4; FALSO)
  ```
Isso garante uma manutenção simples caso novos perfis ou categorias de fundos imobiliários precisem ser adicionados no futuro.

---

## 🏷️ Intervalos Nomeados (Governança de Dados)

Substituindo referências brutas de células (como `B7` ou `B12`) por rótulos de negócios inteligíveis, o modelo tornou-se auditável e limpo através dos seguintes intervalos nomeados:
* `salario_base` → Salário cadastrado nas configurações.
* `rendimento_carteira` → O *Dividend Yield* esperado da carteira de FIIs.
* `sugestao_aporte` → Cálculo automático da meta de aporte (30% do salário base).
* `aporte_mensal` → Valor real definido para a simulação atual.
* `taxa_mensal` → Taxa de juros compostos para a projeção de tempo.

---

## 📊 Design de Interface e Elementos Visuais (UI/UX)

O projeto foi construído pensando na experiência de uso do recrutador ou gestor:
* **Visual Clean (Sem Linhas de Grade):** Todas as linhas de grade nativas do Excel foram desativadas na aba de exibição, criando uma estética de software dedicado.
* **Hierarquia de Cores para Proteção de Dados:** 
  * **Células Brancas:** Áreas livres para input do usuário.
  * **Células Cinza Claro (`#F2F2F2`):** Áreas calculadas por fórmulas automáticas (bloqueadas para evitar erros de digitação).
* **Gráfico de Rosca Interativo:** Adicionado um gráfico dinâmico de distribuição de portfólio. Ao alterar a seleção na Lista Suspensa de Perfil de Investimento, o gráfico redesenha as fatias de ativos em tempo real de forma totalmente automatizada.
* **Banner Customizado:** Cabeçalho de identificação visual sob a marca corporativa `FULANO INVEST`.

---

## 📋 Distribuição da Matriz de Alocação

Os percentuais por perfil foram calibrados na tabela de apoio garantindo conciliação contábil contínua (soma exata de **100%** em qualquer escolha):

| Perfil de Risco | PAPEL | TIJOLO | HÍBRIDOS | FOFs | DESENV. | HOTELARIA |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Conservador** | 30% | 50% | 10% | 10% | 0% | 0% |
| **Moderado** | 32% | 35% | 8% | 5% | 10% | 10% |
| **Agressivo** | 50% | 10% | 5% | 5% | 20% | 10% |

---
💻 *Desenvolvido como projeto de portfólio para demonstração de competências avançadas em Business Intelligence, Modelagem Financeira e Estruturação de Dados no Microsoft Excel.*
