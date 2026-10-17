---
nome: validar dados
descrição: Controle de qualidade e análise antes do compartilhamento — metodologia, precisão e verificações de viés. Use ao revisar uma análise antes de uma apresentação das partes interessadas, verificar cálculos e lógica de agregação, verificar se os resultados de uma consulta SQL parecem corretos ou avaliar se as conclusões são realmente apoiadas pelos dados.
dica de argumento: "<análise para revisar>"
---

# /validate-data - Validar análise antes de compartilhar

> Se você vir espaços reservados desconhecidos ou precisar verificar quais ferramentas estão conectadas, consulte [CONNECTORS.md](../../CONNECTORS.md).

Revise uma análise quanto à precisão, metodologia e possíveis vieses antes de compartilhar com as partes interessadas. Gera uma avaliação de confiança e sugestões de melhoria.

## Uso

```
/validar-dados <análise para revisar>
```

A análise pode ser:
- Um documento ou relatório na conversa
- Um arquivo (markdown, caderno, planilha)
- Consultas SQL e seus resultados
- Gráficos e seus dados subjacentes
- Uma descrição da metodologia e das conclusões

## Fluxo de trabalho

### 1. Metodologia de revisão e suposições

Examinar:

- **Enquadramento da pergunta**: A análise está respondendo à pergunta certa? A pergunta poderia ser interpretada de forma diferente?
- **Seleção de dados**: As tabelas/conjuntos de dados corretos estão sendo usados? O intervalo de tempo é apropriado?
- **Definição de população**: A população de análise está definida corretamente? Existem exclusões não intencionais?
- **Definições métricas**: As métricas são definidas de forma clara e consistente? Elas correspondem à forma como as partes interessadas as entendem?
- **Linha de base e comparação**: A comparação é justa? Os períodos de tempo, os tamanhos das coortes e os contextos são comparáveis?

### 2. Execute a lista de verificação de controle de qualidade pré-entrega

Trabalhe na lista de verificação abaixo — qualidade dos dados, cálculo, razoabilidade e verificações de apresentação.

### 3. Verifique se há armadilhas analíticas comuns

Revise sistematicamente o catálogo detalhado de armadilhas abaixo (explosão de junções, viés de sobrevivência, comparação de períodos incompletos, mudança de denominador, média de médias, incompatibilidades de fuso horário, viés de seleção).

### 4. Verificar Cálculos e Agregações

Sempre que possível, verifique:

- Recalcular alguns números-chave de forma independente
- Verifique se os subtotais somam os totais
- Verifique se as porcentagens somam 100% (ou próximo disso) onde esperado
- Confirme se as comparações YoY/MoM usam os períodos base corretos
- Validar se os filtros são aplicados de forma consistente em todas as métricas

Aplique as técnicas de verificação de sanidade de resultados abaixo (verificações de magnitude, validação cruzada, detecção de bandeira vermelha).

### 5. Avaliar visualizações

Se a análise incluir gráficos:

- Os eixos começam em valores apropriados (zero para gráficos de barras)?
- As escalas são consistentes em todos os gráficos de comparação?
- Os títulos dos gráficos descrevem com precisão o que é mostrado?
- A visualização pode enganar um leitor rápido?
- Existem eixos truncados, intervalos inconsistentes ou efeitos 3D que distorcem a percepção?

### 6. Avaliar Narrativa e Conclusões

Revise se:

- As conclusões são apoiadas pelos dados apresentados
- Explicações alternativas são reconhecidas
- A incerteza é comunicada adequadamente
- As recomendações decorrem logicamente das conclusões
- O nível de confiança corresponde à força da evidência

### 7. Sugira melhorias

Forneça sugestões específicas e acionáveis:

- Análises adicionais que fortaleceriam as conclusões
- Advertências ou limitações que devem ser observadas
- Melhores visualizações ou enquadramentos para pontos-chave
- Falta de contexto que as partes interessadas desejariam

### 8. Gerar Avaliação de Confiança

Classifique a análise em uma escala de 3 níveis:

**Pronto para compartilhar** -- A análise é metodologicamente sólida, cálculos verificados, ressalvas anotadas. Pequenas sugestões de melhoria, mas nada de bloqueio.

**Compartilhe com as ressalvas indicadas** — A análise está amplamente correta, mas tem limitações ou suposições específicas que devem ser comunicadas às partes interessadas. Liste as ressalvas necessárias.

**Precisa de revisão** — Foram encontrados erros específicos, problemas metodológicos ou análises ausentes que devem ser abordados antes do compartilhamento. Liste as alterações necessárias com ordem de prioridade.

## Formato de saída

```
## Relatório de Validação

### Avaliação geral: [Pronto para compartilhar | Compartilhar com ressalvas | Precisa de revisão]

### Revisão da Metodologia
[Descobertas sobre abordagem, seleção de dados, definições]

### Problemas encontrados
1. [Severidade: Alta/Média/Baixa] [Descrição e impacto do problema]
2. ...

### Cálculo de verificações pontuais
- [Métrica]: [Verificado / Discrepância encontrada]
- ...

### Revisão de visualização
[Quaisquer problemas com gráficos ou apresentação visual]

### Melhorias sugeridas
1. [Melhoria e por que isso é importante]
2. ...

### Advertências necessárias para as partes interessadas
- [Advertência que deve ser comunicada]
- ...
```

---

## Lista de verificação de controle de qualidade pré-entrega

Execute esta lista de verificação antes de compartilhar qualquer análise com as partes interessadas.

### Verificações de qualidade de dados

- [ ] **Verificação de fonte**: Confirmadas quais tabelas/fontes de dados foram usadas. Eles são os certos para esta pergunta?
- [ ] **Frescor**: Os dados são atuais o suficiente para a análise. Anotei a data "a partir de".
- [ ] **Completude**: Sem lacunas inesperadas em séries temporais ou segmentos ausentes.
- [ ] **Manipulação de nulos**: Taxas de nulos verificadas em colunas-chave. Os nulos são tratados adequadamente (excluídos, imputados ou sinalizados).
- [ ] **Desduplicação**: Não foi confirmada contagem dupla de junções incorretas ou registros de origem duplicados.
- [ ] **Verificação do filtro**: Todas as cláusulas WHERE e filtros estão corretos. Sem exclusões não intencionais.

### Verificações de cálculo

- [ ] **Lógica de agregação**: GROUP BY inclui todas as colunas não agregadas. O nível de agregação corresponde ao grão de análise.
- [ ] **Correção do denominador**: Os cálculos de taxa e porcentagem usam o denominador correto. Os denominadores são diferentes de zero.
- [ ] **Alinhamento de data**: As comparações usam o mesmo período de tempo. Períodos parciais são excluídos ou anotados.
- [ ] **Correção de junção**: os tipos de JUNÇÃO são apropriados (INTERNO vs ESQUERDO). As junções muitos-para-muitos não inflaram as contagens.
- [ ] **Definições métricas**: As métricas correspondem à forma como as partes interessadas as definem. Quaisquer desvios são anotados.
- [ ] **Soma dos subtotais**: As partes somam o todo quando esperado. Se não o fizerem, explique o porquê (por exemplo, sobreposição).

### Verificações de razoabilidade

- [ ] **Magnitude**: Os números estão em uma faixa plausível. A receita não é negativa. As porcentagens estão entre 0-100%.
- [ ] **Continuidade da tendência**: Sem saltos ou quedas inexplicáveis em séries temporais.
- [ ] **Referência cruzada**: Os números das chaves correspondem a outras fontes conhecidas (painéis, relatórios anteriores, dados financeiros).
- [ ] **Ordem de grandeza**: A receita total está no estádio certo. As contagens de usuários correspondem aos números conhecidos.
- [ ] **Casos extremos**: O que acontece nas fronteiras? Segmentos vazios, períodos de atividade zero, novas entidades.

### Verificações de apresentação

- [ ] **Precisão do gráfico**: Os gráficos de barras começam em zero. Os eixos são rotulados. As escalas são consistentes entre os painéis.
- [ ] **Formatação numérica**: Precisão apropriada. Formatação consistente de moeda/porcentagem. Milhares de separadores quando necessário.
- [ ] **Clareza do título**: Os títulos indicam o insight, não apenas a métrica. Os intervalos de datas são especificados.
- [ ] **Advertência de transparência**: Limitações e suposições conhecidas são declaradas explicitamente.
- [ ] **Reprodutibilidade**: Outra pessoa poderia recriar esta análise a partir da documentação fornecida.

## Armadilhas comuns de análise de dados

### Junte-se à Explosão

**O problema**: Uma junção muitos-para-muitos multiplica silenciosamente as linhas, inflando contagens e somas.

**Como detectar**:
```sql
-- Verifique a contagem de linhas antes e depois da junção
SELECIONE CONTAGEM(*) DA tabela_a; -- 1.000
SELECIONE COUNT(*) DE table_a a JOIN table_b b ON a.id = ba_id; -- 3.500 (uh oh)
```

**Como prevenir**:
- Sempre verifique a contagem de linhas após as junções
- Se as contagens aumentarem, investigue a relação de junção (é realmente 1:1 ou 1:muitos?)
- Use `COUNT(DISTINCT a.id)` em vez de `COUNT(*)` ao contar entidades por meio de junções

### Viés de sobrevivência

**O problema**: Analisar apenas entidades que existem hoje, ignorando aquelas que foram excluídas, canceladas ou falharam.

**Exemplos**:
- Analisar o comportamento do usuário de "usuários atuais" ignora usuários perdidos
- Olhar para “empresas que usam nosso produto” ignora aqueles que avaliaram e saíram
- Estudar propriedades de resultados "bem-sucedidos" sem resultados "malsucedidos"

**Como prevenir**: Pergunte "quem NÃO está neste conjunto de dados?" antes de tirar conclusões.

### Comparação incompleta de períodos

**O problema**: Comparar um período parcial com um período completo.

**Exemplos**:
- "A receita de janeiro é de US$ 500 mil contra. $800 K de dezembro - mas janeiro ainda não acabou
- "As inscrições desta semana caíram" - verificado na quarta-feira, em comparação com uma semana anterior completa

**Como prevenir**: Sempre filtre para completar os períodos ou compare o mesmo dia do mês / mesmo número de dias.

### Mudança de denominador

**O problema**: O denominador muda entre os períodos, tornando as taxas incomparáveis.

**Exemplos**:
- A taxa de conversão melhora porque você mudou a forma como conta os usuários "elegíveis"
- A taxa de rotatividade muda porque a definição de "ativo" foi atualizada

**Como prevenir**: Use definições consistentes em todos os períodos comparados. Observe quaisquer alterações de definição.

### Média das Médias

**O problema**: A média de médias pré-calculadas fornece resultados errados quando os tamanhos dos grupos são diferentes.

**Exemplo**:
- Grupo A: 100 usuários, receita média de US$ 50
- Grupo B: 10 usuários, receita média de US$ 200
- Errado: Média das médias = ($ 50 + $ 200) / 2 = $ 125
- Direita: Média ponderada = (100*$50 + 10*$200) /110 = $63,64

**Como prevenir**: Sempre agregue a partir de dados brutos. Nunca calcule médias pré-agregadas.

### Incompatibilidades de fuso horário

**O problema**: Diferentes fontes de dados usam fusos horários diferentes, causando desalinhamento.

**Exemplos**:
- Carimbos de data/hora do evento em UTC vs. datas voltadas para o usuário na hora local
- Rollups diários que usam diferentes tempos de corte

**Como prevenir**: Padronize todos os registros de data e hora em um único fuso horário (UTC recomendado) antes da análise. Documente o fuso horário utilizado.

### Viés de seleção na segmentação

**O problema**: Os segmentos são definidos pelo resultado que você está medindo, criando uma lógica circular.

**Exemplos**:
- "Os usuários que concluíram a integração têm maior retenção" - obviamente, eles se auto-selecionaram
- "Os usuários de energia geram mais receita" - eles se tornaram usuários de energia GERANDO receita

**Como prevenir**: Defina segmentos com base nas características pré-tratamento, não nos resultados.

### Outras armadilhas estatísticas

- **Paradoxo de Simpson**: A tendência se inverte quando os dados são agregados vs. segmentado
- **Correlação apresentada como causalidade** sem evidências de apoio
- **Pequenos tamanhos de amostra** levando a conclusões não confiáveis
- **Outliers que afetam desproporcionalmente as médias** (medianas devem ser usadas em vez disso?)
- **Testes múltiplos / seleção seletiva** resultados significativos
- **Viés de antecipação**: Usando informações futuras para explicar eventos passados
- **Intervalos de tempo escolhidos a dedo** que favorecem uma narrativa específica

## Verificação de Sanidade do Resultado

### Verificações de magnitude

Para qualquer número-chave em sua análise, verifique se ele passa no "teste de cheiro":

| Tipo de métrica | Verificação de sanidade |
|---|---|
| Contagens de usuários | Isso corresponde aos números conhecidos de MAU/DAU? |
| Receita | Isso está na ordem de grandeza correta vs. ARR conhecido? |
| Taxas de conversão | Isso está entre 0% e 100%? Corresponde aos números do painel? |
| Taxas de crescimento | O crescimento de 50%+ MoM é realista ou há algum problema de dados? |
| Médias | A média é razoável dado o que você sabe sobre a distribuição? |
| Porcentagens | As porcentagens dos segmentos somam ~100%? |

### Técnicas de validação cruzada

1. **Calcule a mesma métrica de duas maneiras diferentes** e verifique se elas correspondem
2. **Verificação pontual de registros individuais** - escolha algumas entidades específicas e rastreie seus dados manualmente
3. **Comparar com benchmarks conhecidos** -- comparar com painéis publicados, relatórios financeiros ou análises anteriores
4. **Engenheiro reverso** -- se a receita total for X, a receita por usuário vezes a contagem de usuários é aproximadamente igual a X?
5. **Verificações de limites** — o que acontece quando você filtra para um único dia, um único usuário ou uma única categoria? Esses micro-resultados são sensatos?

### Sinais de alerta que justificam investigação

- Qualquer métrica que tenha mudado em mais de 50% período a período sem uma causa óbvia
- Contagens ou somas que são números redondos exatos (sugere um problema de filtro ou valor padrão)
- Taxas exatamente de 0% ou 100% (podem indicar dados incompletos)
- Resultados que confirmam perfeitamente a hipótese (a realidade costuma ser mais confusa)
- Valores idênticos em períodos de tempo ou segmentos (sugere que a consulta está ignorando uma dimensão)

## Padrões de documentação para reprodutibilidade

### Modelo de documentação de análise

Toda análise não trivial deve incluir:

```marcação
## Análise: [Título]

### Pergunta
[A pergunta específica que está sendo respondida]

### Fontes de dados
- Tabela: [schema.table_name] (a partir de [data])
- Tabela: [schema.other_table] (a partir de [data])
- Arquivo: [nome do arquivo] (fonte: [de onde veio])

### Definições
- [Métrica A]: [Exatamente como é calculado]
- [Segmento X]: [Exatamente como a adesão é determinada]
- [Período de tempo]: [Data de início] a [data de término], [fuso horário]

### Metodologia
1. [Etapa 1 da abordagem de análise]
2. [Etapa 2]
3. [Etapa 3]

### Suposições e Limitações
- [Suposição 1 e por que é razoável]
- [Limitação 1 e seu potencial impacto nas conclusões]

### Principais descobertas
1. [Encontrando 1 com evidências de apoio]
2. [Encontrando 2 com evidências de apoio]

### Consultas SQL
[Todas as consultas utilizadas, com comentários]

### Advertências
- [Coisas que o leitor deve saber antes de agir sobre isso]
```

### Documentação de código

Para qualquer código (SQL, Python) que possa ser reutilizado:

```pitão
"""
Análise: Retenção Mensal de Coorte
Autor: [Nome]
Data: [Data]
Fonte de dados: tabela de eventos, tabela de usuários
Última validação: [Data] -- os resultados corresponderam ao painel em 2%

Finalidade:
    Calcule coortes mensais de retenção de usuários com base na data da primeira atividade.

Suposições:
    - “Ativo” significa pelo menos um evento no mês
    - Exclui contas de teste/internas (user_type!= 'interno')
    - Usa datas UTC em todo o

Saída:
    Matriz de retenção de coorte com linhas cohort_month e colunas months_since_signup.
    Os valores são taxas de retenção (0-100%).
"""
```

### Controle de versão para análises

- Salve consultas e código no controle de versão (git) ou em um sistema de documentos compartilhados
- Observe a data do instantâneo de dados usado
- Se uma análise for executada novamente com dados atualizados, documente o que mudou e por quê
- Link para versões anteriores de análises recorrentes para comparação de tendências

## Exemplos

```
/validate-data Revise esta análise trimestral de receita antes de enviá-la à equipe executiva: [análise]
```

```
/validate-data Verifique minha análise de rotatividade - estou comparando as taxas de rotatividade do quarto trimestre com o terceiro trimestre, mas o quarto trimestre tem uma janela de medição mais curta
```

```
/validate-data Aqui está uma consulta SQL e seus resultados para nosso funil de conversão. A lógica parece correta? [consulta + resultados]
```

## Dicas

- Execute /validate-data antes de qualquer apresentação ou decisão de alto risco
- Mesmo análises rápidas se beneficiam de uma verificação de sanidade - leva um minuto e pode salvar sua credibilidade
- Se a validação encontrar problemas, corrija-os e revalide
- Compartilhe o resultado da validação junto com sua análise para aumentar a confiança das partes interessadas