**Trabalho 2 (T2): Matching de Produtos**
Aprendizado de Máquina
Profª.Drª. Daniela Oliveira Ferreira do Amaral

No primeiro trabalho, o foco foi explorar algoritmos de aprendizado de máquina supervisionado
aplicados a dados estruturados. Neste T2, o desafio é diferente: trabalharemos com dados
textuais não estruturados em um problema real trazido por uma empresa parceira — a Nubo
Desenvolvimento de Software LTDA.

O problema consiste em identificar automaticamente, a partir de um texto de consulta escrito
de forma heterogênea e sem padronização, qual é o produto correspondente em um catálogo
normalizado. Trata-se de um problema clássico de recuperação de informação, com alta
relevância prática em sistemas de compras, gestão de estoque e integração entre bases de
dados de fornecedores distintos.

**Contexto**
Distribuidoras e varejistas do setor de bebidas e alimentos frequentemente recebem pedidos
de compra escritos de formas completamente diferentes: abreviações, erros ortográficos,
variações de embalagem, ausência de unidades de medida padronizadas. O mesmo produto
pode aparecer, como por exemplo:

**Texto da Consulta (query) Produto no Catálogo**
**COCA COLA 1L** C/6 Refrigerante coca-cola pet 1 litro
**SKOL LATA 350ml** C/12 Cerveja skol lata 350ml
**FANTA LARANJA 2L** C/6 Refrigerante fanta laranja 2 litros
**MONSTER BRANCO S/ACUCAR C/6** UNID Energético monster energy zero açúcar lata 473ml

Fazer esse mapeamento manualmente, para milhares de pedidos por dia, é inviável. O objetivo
deste trabalho **é construir e avaliar pipelines automáticos de matching utilizando duas
abordagens distintas de processamento de linguagem natural.**

**Dados Fornecidos**
Você receberá quatro arquivos CSV:
catalog.csv — Catálogo Normalizado de Produtos
matched_id
queries_val.csv
queries_test.csv
matched_id
Contém as seguintes colunas:
Coluna Descrição Exemplo
product_id Código EAN/GTIN único do
produto
07891991014915
product_name Nome padronizado do produto Refrigerante guaraná antarctica
200ml
brand_name Marca do produto Antarctica

**— Consultas de Produtos**
Contém as seguintes colunas:
Coluna Descrição Exemplo
text Texto heterogêneo da consulta FANTA LARANJA 2L C/6
matched_id product_id correto (inicialmente vazio) (a ser preenchido pelo grupo)
A coluna estará vazia para todas as linhas. Este arquivo pode ser utilizado para
exploração, análise dos dados e modelagem. Idealmente, utilize este conjunto para
desenvolver e ajustar seus pipelines, juntamente ao .
e — Conjuntos de Validação e Teste
Ambos contêm 250 queries anotadas pelo professor, com preenchido. O
conjunto de validação deve ser utilizado durante o desenvolvimento para ajustar parâmetros e
comparar variações. O conjunto de teste deve ser utilizado somente ao final, para reportar as
métricas definitivas.
Coluna Descrição
text Texto heterogêneo da consulta
matched_id product_id correto (preenchido)
**Etapas Esperadas**
Ao final do trabalho, espera-se que o grupo elabore e reporte os seguintes passos:
queries.csv
queries_val.csv
queries_val.csv
queries_val.csv
queries_test.csv
litro
**1. Exploração dos dados:** carregue e . Analise a distribuição
dos dados, identifique padrões nas queries (abreviações frequentes, categorias de
produtos) e casos que pareçam desafiadores.
**2. Abordagem 1 — NLP Clássico:** implemente o pipeline com TF-IDF ou BM25, calcule as
métricas P@1, MRR e R@5 sobre e registre os resultados.
Observações: O símbolo @ em métricas como Precision@1 (P@1) significa: “na posição” ou
“considerando os primeiros resultados”.
P@1 significa precisão considerando apenas o primeiro item retornado pelo sistema. Já R@5
= Recall ou Revogação nos 5 primeiros resultados. Nessa métrica, o sistema retorna uma lista
ordenada de resultados; Consideram-se apenas os 5 primeiros itens; Mede-se quantos itens
relevantes foram recuperados dentro desses 5.
**3. Abordagem 2 — NLP com Deep Learning:** implemente o pipeline com um modelo de
rede neural, calcule as mesmas métricas sobre e registre os resultados.
**4. Comparação das abordagens:** construa a tabela comparativa (métricas, tempo de
execução, custo, complexidade) e discuta: em quais tipos de query cada abordagem se
saiu melhor? Qual é o tradeoff entre simplicidade e desempenho?
**5. Avaliação final:** após finalizar o desenvolvimento, reporte as métricas finais de ambas as
abordagens sobre .
**6. Análise qualitativa:** apresente exemplos de acertos, erros e casos ambíguos para cada
abordagem.
**7. Relatório e apresentação:** documente toda a metodologia, os resultados e as conclusões
conforme descrito na seção de Entrega.

**Abordagens**
**Abordagem 1 — NLP Clássico**
A primeira abordagem utiliza técnicas tradicionais de recuperação de informação baseadas em similaridade textual. O pipeline consiste em:

**1.1 Pré-processamento Textual**
Antes de calcular qualquer similaridade, ambos os textos (query e nome do produto no
catálogo) devem ser normalizados. Recomenda-se aplicar ao menos:
Conversão para minúsculas
Remoção de pontuação e caracteres especiais
Remoção de stopwords em português (ex: "de", "do", "com", "sem", "por")
Normalização de abreviações comuns (ex: → , → )
catalog.csv queries.csv
ML ml L
C/12 UND
product_id
Remoção de termos irrelevantes para o matching (ex: quantidades de embalagem como
, , )
**1.2 Representação Vetorial e Busca por Similaridade**
Após o pré-processamento, represente os textos como vetores e calcule a similaridade entre a
query e cada produto do catálogo. Duas opções são sugeridas (implemente ao menos uma):
TF-IDF com Similaridade de Cosseno: nesta abordagem, montam-se matrizes baseadas em
frequência de termos, ponderados por TF-IDF, e calcula-se a similaridade do cosseno entre a
query e os produtos do catálogo.
BM25: BM25 é um algoritmo de ranqueamento probabilístico amplamente utilizado em motores
de busca, geralmente superior ao TF-IDF para textos curtos.
Para cada query no conjunto avaliado, o pipeline deve:
1. Pré-processar o texto da query
2. Calcular a similaridade com todos os produtos do catálogo
3. Retornar os top-5 produtos mais similares com suas pontuações
4. Registrar o do produto ranqueado em 1º lugar como predição principal

**Abordagem 2 — Deep Learning**
A segunda abordagem deve utilizar um modelo de rede neural. A escolha do modelo e, se
necessário, do provedor fica a critério do grupo — o que importa é que seja uma abordagem
baseada em redes neurais. Caso optem por rodar localmente os modelos, podem recorrer ao uso
do Hugging Face Hub, Sentence-Transformer, ou também pelo Ollama). Caso usem API, sugiro a
API do Google Gemini por ser gratuita e ter limites generosos de uso, mas outras opções
(OpenAI, Anthropic, etc.) também são válidas. Para trabalhar com LLMs duas estratégias são
sugeridas:
**Embeddings Semânticos:** Podemos gerar embeddings para as queries e para os produtos do
catálogo usando um modelo de embedding, depois calcular a similaridade de cosseno entre
eles. O pipeline seria semelhante ao da abordagem clássica, mas utilizando vetores densos
gerados por um modelo de linguagem em vez de vetores esparsos baseados em frequência.

**Zero-Shot ou Few-Shot Prompting:** Alternativamente, podemos pedir diretamente a um
modelo LLM que identifique o produto mais provável dado um contexto. Por exemplo,
fornecendo a query e uma lista dos top-10 candidatos (filtrados previamente por BM25), o
modelo pode escolher o produto mais adequado. Ao não fornecer nenhum exemplo, temos o
cenário zero-shot; ao fornecer alguns exemplos de correspondências corretas, temos o cenário
few-shot.
Para os dois cenários com Deep Learning, recomendo que usem em combinação com a
C/6
Abordagem 1. Primeiro filtre os top-10 candidatos via BM25 ou TF-IDF, depois passe esses
candidatos a rede para a decisão final. Isso reduz drasticamente o consumo de tokens e custo
computacional e melhora a qualidade das respostas.

**Avaliação**
**Conjuntos de Avaliação**
Os arquivos queries_val.csv e
matched_id já preenchida pela professora.
contêm 250 queries cada, com a coluna
(validação): use durante o desenvolvimento para ajustar parâmetros,
comparar variações e decidir entre abordagens.
(teste): use somente ao final, para reportar as métricas definitivas.
Não o utilize para tomar decisões de desenvolvimento!

**Métricas de Avaliação**
Calcule as seguintes métricas para cada abordagem sobre os conjuntos de avaliação.
Precision@1 (P@1)
Mede se o produto retornado em primeiro lugar está correto. É a métrica mais intuitiva para um
sistema de matching com uma única predição principal.
P@1 =
número de queries em que o produto correto foi ranqueado em 1º lugar
número total de queries avaliadas
Exemplo: se de 50 queries anotadas, 32 tiveram o produto correto em 1º lugar → P@1 = 32/50
= 0,64.

**MRR— Mean Reciprocal Rank**
Considera não apenas se o produto correto foi retornado, mas em qual posição ele aparece na
lista de resultados. É especialmente útil quando o sistema retorna uma lista ranqueada (top-5),
pois dá crédito parcial a acertos nas posições 2, 3, 4 e 5.
onde ranki é a posição do produto correto na lista de resultados para a query i (se não
aparecer no top-5, considera-se = 0). Neste trabalho, adotamos K = 5, ou seja,
calculamos o MRR@5.
Exemplo: para 3 queries com o produto correto nas posições 1, 3 e 2:
queries_test.csv
queries_val.csv
queries_test.csv
NO_MATCH

**Recall@5 (R@5)**
Mede se o produto correto aparece em alguma das 5 primeiras posições da lista de
resultados, independentemente da posição exata.
Nota: você pode ter instruído o modelo a retornar uma única sugestão ao invés de uma lista
ranqueada. Nesse caso, P@1 é a métrica principal e MRR e R@5 coincidem com P@1. Isso
deve ser mencionado na análise comparativa.

**Análise Qualitativa**
Além das métricas numéricas, seu relatório deve apresentar:
Exemplos de acertos: queries que foram mapeadas corretamente, com o texto da query e
o produto encontrado
Exemplos de erros: queries que falharam, com análise do motivo (ex: "o modelo
confundiu Coca-Cola 1L com Coca-Cola 2L porque a quantidade de litros não foi tratada no
pré-processamento")
Casos ambíguos: queries onde mais de um produto do catálogo seria uma resposta válida
Casos : queries para as quais o produto não existe no catálogo — como cada
abordagem se comporta nesses casos?

**Comparação das Abordagens**
Apresente uma tabela comparativa como a seguinte:
Abordagem P@1 MRR R@5 Tempo (250 queries) Custo Complexidade
TF-IDF / BM25 ? ? ? ? Gratuito Baixa
Deep Learning ? ? ? ? Varia Média

**Entrega**
Número de Integrantes: de 2 a 5.
Data de Entrega: 30/06 (terça-feira)
Formato da Entrega: Arquivos/link no Moodle + Apresentação em sala de aula
A apresentação será feita diretamente à professora, sem necessidade de slides
formais. O foco é a discussão dos resultados e metodologia, não a qualidade visual da
apresentação.

**Entregáveis:** README.txt contendo: nome e matrícula de todos os integrantes; instruções passo a passo para instalar as dependências e reproduzir os resultados (incluindo como configurar a variável de ambiente da API key utilizada).
Código-fonte completo (ou link para repositório GitHub/GitLab), contendo os dois pipelines
implementados e o script de avaliação.
Relatório em PDF contemplando:
**1. Referencial Teórico:** descrição dos conceitos utilizados (TF-IDF, BM25, embeddings,
similaridade de cosseno, LLMs) com referências bibliográficas.

**2. Delimitação do Desenvolvimento:** o que foi reaproveitado de código de repositórios
públicos e o que foi implementado pelo grupo. Deixe essa distinção clara.

**3. Metodologia:** descrição do pipeline de cada abordagem, decisões de préprocessamento e estratégia de avaliação (validação × teste).

**4. Resultados e Análise:** tabela comparativa das métricas, exemplos qualitativos de
acertos/erros, discussão sobre os tradeoffs das abordagens.

**5. Conclusão, Melhorias e Dificuldades:** principais aprendizados e possíveis melhorias
futuras (ex: fine-tuning, modelos multilíngues, uso de dados de treinamentos adicionais).

**6. Referência**
README.txt
