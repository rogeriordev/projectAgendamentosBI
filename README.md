# Dashboard Ford - Agenda e Serviços (Power BI)

## 📊 Visão Geral

Este repositório contém a documentação do dashboard desenvolvido em Power BI para monitoramento e análise de indicadores operacionais, abrangendo gestão de agendamentos de serviços automotivos e análise de experiência do cliente através do sistema.

## 🎯 Objetivo

O dashboard foi desenvolvido para fornecer uma visão consolidada e em tempo real dos principais indicadores de desempenho relacionados a:
- Gestão de agendamentos e atendimentos
- Controle de qualidade e processos
- Satisfação do cliente e NPS
- Performance individual de consultores

## 📑 Estrutura do Dashboard

### **Página 1: Agenda Ford - Visão Geral**

Esta página apresenta um panorama completo dos agendamentos e processos operacionais:

#### Indicadores Principais:
- **Agendamentos (171)**: Total de agendamentos realizados no período
- **Agendado (7)**: Agendamentos confirmados aguardando atendimento
- **Andamento (21)**: Serviços em execução
- **Concluído (140)**: Serviços finalizados
- **Concluído S/ OS (3)**: Serviços concluídos sem ordem de serviço

#### Indicadores de Processo:
- **Checklist Veículo (164)**: Total de checklists realizados nos veículos
- **Controle Qualidade (152)**: Verificações de qualidade executadas
- **Teste de Rodagem (31)**: Testes realizados após serviços
- **Checklist Entrega (134)**: Verificações pré-entrega ao cliente
- **Unidade Parada (18)**: Veículos aguardando peças ou aprovação

#### Filtros Disponíveis:
- Semestre
- Mês
- Semana
- Consultor
- Motivo

---

### **Página 2: Agenda Ford - Performance por Consultor**

Apresenta análise detalhada da performance individual de cada consultor:

#### Métricas Individuais:
- **Agendamentos totais** por consultor
- **Agendado vs Andamento**: Acompanhamento do fluxo de trabalho
- **% Revisão Dia Anterior**: Percentual de revisões do dia anterior
- **% Checkin**: Taxa de check-in realizado
- **% Checklist Veículo**: Completude dos checklists
- **% Agend. Entrega**: Agendamentos de entrega realizados
- **% Checklist Entrega (C)**: Checklist de entrega completo
- **% Fecham. Automático**: Fechamentos automáticos do sistema
- **% Motivo Preenchido**: Taxa de preenchimento de motivos

#### Indicadores Agregados:
- **Agendamento Online**: Total vs Online (158 total, 85 online - 53.80%)
- **Progr. Oficina Não Realizada**: Monitoramento de não conformidades
- **Leva e Traz**: Objetivo vs Realizado (7,9 objetivo, status "Em branco")

#### Recursos Visuais:
- Cards com fotos dos consultores para fácil identificação
- Medidores (gauges) com código de cores:
  - 🔵 Azul: Performance ótima (≥95%)
  - 🟠 Laranja: Performance mediana (70-94%)
  - 🔴 Vermelho: Performance abaixo do esperado (<70%)

---

### **Página 3: OneCX Serviço - Experiência do Cliente**

Dashboard focado em métricas de satisfação e experiência do cliente:

#### Indicadores Principais:

**Métricas Agregadas:**
- **Índice de Experiência (CVP)**: 4,86 (escala 0-5)
- **Serviço/Reparo Correto 1ª Vez**: 91,30%
- **Satisfação Concessionária (OER)**: 84,78%
- **NPS da Concessionária**: 80,43
- **NPS da Marca**: 82,61

**Análises Comparativas por Consultor:**
- Índice de Experiência (CVP) comparativo
- Satisfação da Concessionária por atendente
- NPS da Concessionária individual
- NPS da Marca por consultor

#### Destaques:
- Marlon apresenta as melhores avaliações em todos os indicadores (100% em satisfação)
- Análise permite identificar oportunidades de melhoria por consultor
- Correlação entre performance operacional e satisfação do cliente

---

## 🛠️ Recursos Técnicos do Power BI Utilizados

### **Modelagem de Dados**

- **Tabelas Relacionadas**: O modelo utiliza múltiplas tabelas conectadas através de relacionamentos um-para-muitos (1:N), incluindo:
  - Tabela de agendamentos
  - Tabela de consultores
  - Tabela de pesquisas OneCX
  - Tabela calendário (dimensão temporal)
  - Tabela de status/motivos

- **Relacionamentos**: Implementação de chaves primárias e estrangeiras para garantir integridade referencial

### **Medidas DAX**

O dashboard utiliza diversas medidas calculadas em DAX para os indicadores. Exemplo de fórmula implementada:

```dax
% Retorno Pesquisas Servico = 
VAR qdeconcl = CALCULATE(SUM('mysql tbonecxconvites'[convites_concl]), 'mysql tbonecxconvites'[segmento] = "Serviço")+0
VAR qdeentr = CALCULATE(SUM('mysql tbonecxconvites'[convites_entregues]), 'mysql tbonecxconvites'[segmento] = "Serviço")+0
RETURN DIVIDE(qdeconcl, qdeentr) * 100
```

Outras medidas implementadas incluem:
- Cálculos de percentuais com tratamento de divisão por zero
- Agregações condicionais usando CALCULATE e filtros
- Variáveis (VAR) para otimização de performance
- Medidas de ranking e comparação

### **Visualizações**

- **Cards**: Exibição de KPIs principais com formatação condicional
- **Medidores (Gauge)**: Indicadores visuais com limites mínimo, máximo e metas
- **Gráficos de Barras**: Comparações entre consultores
- **Imagens**: Fotos dos consultores para personalização
- **Segmentações (Slicers)**: Filtros interativos para análise dinâmica

### **Formatação Condicional**

- Cores baseadas em limites de performance
- Ícones dinâmicos conforme status
- Destaque visual para valores fora do padrão

### **Dicas de Ferramentas (Tooltips)**

- Tooltips personalizadas com informações detalhadas
- Drill-through para análise aprofundada
- Contexto adicional ao passar o mouse sobre visualizações

### **Interatividade**

- Filtros cruzados entre visualizações
- Drill-down em hierarquias temporais (Semestre > Mês > Semana)
- Sincronização de segmentações entre páginas
- Bookmarks para navegação rápida

---

## 🔄 Atualização de Dados

### **Conexão com MySQL**

Os dados são atualizados automaticamente a partir de um banco de dados MySQL:

- **Gateway de Dados**: Configurado para comunicação segura entre Power BI Service e o banco MySQL local/on-premises
- **Atualização Programada**: Configurada no Power BI Service para executar em horários específicos
- **Tabelas Principais**:
  - `mysql tbonecxconvites`: Dados de pesquisas de satisfação
  - Tabelas de agendamentos e processos
  - Tabelas de consultores e configurações

### **Processo de Atualização**

1. Power BI Service executa a atualização no horário programado
2. Gateway estabelece conexão com o banco MySQL
3. Consultas SQL são executadas para extrair dados atualizados
4. Dados são carregados no modelo do Power BI
5. Medidas e visualizações são recalculadas automaticamente
6. Dashboard atualizado fica disponível para os usuários

---

## 📈 Benefícios do Dashboard

- ✅ **Visibilidade em tempo real** dos indicadores operacionais
- ✅ **Identificação rápida** de gargalos e oportunidades
- ✅ **Análise comparativa** de performance entre consultores
- ✅ **Monitoramento da satisfação** do cliente
- ✅ **Tomada de decisão baseada em dados**
- ✅ **Otimização de processos** através de insights visuais

---

## 📱 Responsividade

O dashboard foi desenvolvido considerando:
- Layout adaptável para diferentes resoluções
- Compatibilidade com aplicativo mobile do Power BI
- Visualizações otimizadas para tablets e smartphones

---

## 🔐 Segurança e Governança

- Controle de acesso baseado em perfis de usuário
- RLS (Row-Level Security) para garantir visualização apenas de dados autorizados
- Auditoria de atualizações e acessos através do Power BI Service

---

## 📞 Contato e Suporte

Para dúvidas, sugestões ou suporte técnico relacionado a este dashboard, entre em contato com a equipe de BI.

---

**Última atualização**: 28/11/2025  
**Versão**: 1.0  
**Desenvolvido em**: Microsoft Power BI Desktop
