# Memorando de Decisão — Fonte de Dados do Projeto


| Campo | Informação |
|---|---|
| Curso / Disciplina | `[Ciencia da computação]` |
| Projeto integrador | `[]` |
| Orientador(a) | `[Andrea Ono Sakai]` |
| Data de entrega desta etapa | `[08/09/2026]` |
| Integrantes do grupo | `[Wendell Rodrigues Macedo, Vinicius Firmino Solto, Fernando Paiva Coelho, Joao Gustavo Sanches, Daniel Dias Paranhos]` |

---

> Preencha cada seção com o que você encontrou na pesquisa. Não deixe nenhum campo com o texto entre colchetes — substitua pelo seu conteúdo. Toda informação levantada nas Opções A e B precisa indicar a fonte de onde veio.

## 1. Situação

<!-- Em uma frase: qual decisão precisa ser tomada e por quê. 
O pipeline do projeto já está definido: qualquer fonte de dados precisa produzir registros que se transformem em janelas e, por fim, em X = [latência, perda, jitter]. Falta decidir de onde virão esses dados na próxima fase. A equipe do projeto precisa recomendar, com base em pesquisa e não em preferência pessoal, se a próxima etapa deve usar um dataset real já publicado ou a API do RIPE Atlas. O grupo deve produzir um memorando de decisão com a recomendação da tomada de decisão. A recomendação só tem valor se for sustentada por pesquisa real — não existe resposta pronta para copiar; ela precisa ser construída a partir do que vocês encontraram.
-->

A equipe precisa decidir se a próxima fase do projeto utilizará um dataset real já publicado ou a API do RIPE Atlas como fonte de dados, garantindo que sejam obtidas as métricas de latência, perda de pacotes e jitter necessárias para treinar, testar e validar o modelo de predição de falhas da rede.

## 2. Opção A — Dataset real

<!-- O que foi encontrado sobre um dataset real de ICMP. Cite a fonte de cada informação. -->

- **Origem / link:** Base de dados do SIMET — Sistema de Medição de Tráfego Internet, desenvolvido e mantido pelo NIC.br (Núcleo de Informação e Coordenação do Ponto BR). O portal disponibiliza bases de medições Web e Mobile para download.
- **Formato:** Os dados são disponibilizados em arquivos CSV, incluindo uma base das medições Web, uma base Mobile e um arquivo de dicionário dos dados.
- **Período coberto:** O portal do NIC.br utiliza, em suas visualizações públicas, informações dos últimos seis meses, reunindo mais de 5 milhões de medições realizadas pelo SIMET. A página de download não informa explicitamente o período histórico total existente dentro dos arquivos CSV
- **Campos disponíveis:** As medições armazenam informações como horário da medição, operadora, tipo de dispositivo, localização aproximada, velocidade de download, latência, perda de pacotes e jitter. Essas três últimas métricas são diretamente compatíveis com os atributos necessários pelo projeto.
- **Licença de uso:** A página de disponibilização dos dados informa que o conteúdo do portal está sob licença Creative Commons Atribuição-CompartilhaIgual 4.0 Internacional (CC BY-SA 4.0), salvo quando houver restrições adicionais indicadas.

**Resumo do que foi encontrado:**

O SIMET é uma alternativa brasileira para obtenção de dados reais de qualidade de Internet. Desenvolvido pelo NIC.br

## 3. Opção B — API do RIPE Atlas

<!-- O que foi encontrado sobre a API: autenticação, criação e consulta de medições. Cite a fonte de cada informação. -->

- **Documentação consultada (link):** ocumentação oficial da RIPE Atlas REST API v2.
- **Autenticação exigida:** Para criar novas medições, é necessário utilizar uma API Key com permissão para criação de medições, normalmente enviada no cabeçalho Authorization. Já a consulta de medições e resultados públicos pode ser feita sem autenticação
- **Como se cria uma medição:** Uma medição é criada enviando uma requisição POST para https://atlas.ripe.net/api/v2/measurements/. No corpo da requisição são informados, em JSON, o tipo da medição — por exemplo, ping —, o destino, a versão de IP, os probes que realizarão a medição e, opcionalmente, período e frequência da coleta
- **Como se consultam os resultados:** Os resultados podem ser consultados por meio de uma requisição GET para https://atlas.ripe.net/api/v2/measurements/{id}/results/, substituindo {id} pelo identificador da medição. Também é possível filtrar os resultados por probes e por período de tempo. Medições públicas podem ser consultadas sem autenticação.
**Resumo do que foi encontrado: A API do RIPE Atlas permite criar medições de rede sob demanda usando probes distribuídos em diferentes regiões do mundo. Para o projeto, pode ser criada uma medição do tipo ping, permitindo coletar dados reais de latência e perda de pacotes diretamente pela API

https://atlas.ripe.net/docs/

## 4. Comparação

<!-- Preencha a tabela com base no que você levantou nas seções 2 e 3. -->

| Critério | Opção A — Dataset real | Opção B — API RIPE Atlas |
|---|---|---|
| Controle sobre a coleta | Baixo — os dados já foram coletados e não é possível definir exatamente como cada medição foi realizada. | Alto — permite escolher o destino, os probes, o tipo de medição e a frequência da coleta. |
| Diversidade geográfica | Boa dentro do Brasil — o SIMET possui medições realizadas em diferentes regiões brasileiras. | Muito alta — possui probes distribuídos em diversos países e regiões do mundo. |
| Custo / complexidade de implementação | Baixo — basta baixar os dados em CSV, tratar e adaptar ao formato do projeto. | Médio — exige integração com a API, autenticação, configuração das medições e tratamento das respostas. |
| Tempo até os primeiros dados estarem disponíveis | Imediato — os dados já estão disponíveis para download e análise. | Maior — é necessário configurar a medição e aguardar a coleta e disponibilização dos resultados. |

## 5. Recomendação

<!-- Uma frase direta: qual opção você recomenda. -->

Opção A — Dataset real do SIMET, por oferecer dados já disponíveis, com menor complexidade de implementação

## 6. Justificativa

<!-- Por que essa opção vence a outra, com base nas evidências das seções 2, 3 e 4 — não em preferência pessoal. -->

Dataset real do SIMET é a mais adequada porque os dados já estão disponíveis para uso, possuem métricas compatíveis com o projeto, como latência, perda de pacotes e jitter, e exigem menor esforço de implementação. Em comparação, a API do RIPE Atlas oferece maior controle e diversidade geográfica, porém exige autenticação, configuração das medições e integração com a API, aumentando a complexidade e o tempo até a obtenção dos primeiros dados. Portanto, para esta etapa do projeto, o dataset do SIMET apresenta a melhor relação entre disponibilidade, facilidade de uso e compatibilidade com o modelo

## 7. Riscos e limitações

<!-- O que pode dar errado com a opção escolhida, e como isso poderia ser mitigado. -->

é que a equipe não possui controle sobre como e quando os dados foram coletados. Também pode ser necessário tratar valores ausentes, filtrar registros e adaptar os campos ao formato exigido pelo projeto. Para reduzir esses riscos, o grupo deve realizar uma etapa de limpeza e validação dos dados, selecionar apenas registros compatíveis com as métricas de latência, perda de pacotes e jitter e converter o dataset para o padrão definido no contrato de dados do projeto.

## 8. Contribuição Individual dos Integrantes

<!-- cada integrante deve descrever, com suas próprias palavras, o que efetivamente fez nesta etapa. Contribuições genéricas como "ajudei em tudo" não serão aceitas. Use verbos de ação e seja específico (ex.: "pesquisei , analisei, testei, ... apresentei prós/contras ao grupo, ...").-->


### Integrante 1 — `Wendell Rodrigues Macedo`
- **O que fez nesta etapa:** `Pesquisei informações sobre o dataset real do SIMET, verificando as métricas disponíveis, como latência, perda de pacotes e jitter. Também analisei a compatibilidade desses dados com as necessidades do projeto e apresentei os principais pontos encontrados ao grupo.`
- **Tempo dedicado (aprox.):** `2h30`
- **Evidência da contribuição** *(print de conversa, rascunho, e-mail, documento compartilhado etc.)*: 

### Integrante 2 — `Vinicius Firmino Solto`
- **O que fez nesta etapa:** `Pesquisei a documentação da API do RIPE Atlas, analisando como funciona a autenticação, a criação de medições e a consulta dos resultados. Também comparei a complexidade da API com a utilização de um dataset já disponível.`
- **Tempo dedicado (aprox.):** `2h30`
- **Evidência da contribuição** *(print de conversa, rascunho, e-mail, documento compartilhado etc.)*: 

### Integrante 3 — `Fernando Paiva Coelho`
- **O que fez nesta etapa:** `Realizei a comparação entre o dataset do SIMET e a API do RIPE Atlas considerando controle sobre a coleta, diversidade geográfica, complexidade de implementação e tempo para obtenção dos dados. Também participei da elaboração da recomendação e da justificativa final do memorando.`
- **Tempo dedicado (aprox.):** `3h00`
- **Evidência da contribuição** *(print de conversa, rascunho, e-mail, documento compartilhado etc.)*: 

### Integrante 4 — `Joao Gustavo Sanches`
- **O que fez nesta etapa:** `Analisei os riscos e limitações relacionados ao uso do dataset real do SIMET, identificando possíveis problemas como falta de controle sobre a coleta, necessidade de limpeza dos dados e adaptação dos campos ao formato definido pelo projeto. Também sugeri formas de reduzir esses riscos.`
- **Tempo dedicado (aprox.):** `2h00`
- **Evidência da contribuição** *(print de conversa, rascunho, e-mail, documento compartilhado etc.)*: 

### Integrante 5 — `Daniel Dias Paranho`
- **O que fez nesta etapa:** `Revisei as informações pesquisadas pelo grupo, conferi o preenchimento das seções do memorando e organizei as fontes utilizadas. Também revisei a recomendação e a justificativa para verificar se estavam de acordo com os resultados obtidos na pesquisa.`
- **Tempo dedicado (aprox.):** `2h00`
- **Evidência da contribuição** *(print de conversa, rascunho, e-mail, documento compartilhado etc.)*: 

### Integrante 6 — `[Escreva nome completo do aluno ]`
- **O que fez nesta etapa:** `[]`
- **Tempo dedicado (aprox.):** `[ex.: 3h30]`
- **Evidência da contribuição** *(print de conversa, rascunho, e-mail, documento compartilhado etc.)*: 
`[]` 
`[]`

---

## Fontes consultadas

1. NIC.br — Dados SIMET e informações sobre as métricas de qualidade da Internet: https://bandalarga.nic.br/

2. NIC.br — Documentação do SIMET sobre medições de latência, jitter e perda de pacotes: https://docs.medicoes.nic.br/simet-integrado/debian/periodicidade/

3. RIPE Atlas — Documentação oficial da API REST: https://atlas.ripe.net/docs/apis/

4. RIPE Atlas — Autenticação e uso de API Key: https://atlas.ripe.net/docs/apis/rest-api-manual/authentication/

5. RIPE Atlas — Criação de medições: https://atlas.ripe.net/docs/apis/rest-api-manual/measurements/creating-measurements/

6. RIPE Atlas — Consulta dos resultados das medições: https://atlas.ripe.net/docs/apis/rest-api-reference/measurements/measurements_results
