# Especificação de Produto — Weather App

## Overview

Aplicação de previsão do tempo para que uma pessoa identifique uma cidade, consulte as condições atuais e veja a previsão de cinco dias. Deve permitir alternância entre Celsius e Fahrenheit e oferecer uso em dispositivos móveis.

O fluxo deve tentar identificar a localização automaticamente. Se a pessoa não conceder permissão, a aplicação deve solicitar que informe a cidade. Celsius será a unidade inicial. O idioma deve acompanhar a localização; quando ela não puder ser obtida, o fallback será pt-BR.

A fonte meteorológica será a Open-Meteo para geocoding e forecast, sem chave de API. Os campos detalhados a exibir ainda não foram confirmados. O escopo abaixo distingue decisões confirmadas, requisitos derivados do briefing e propostas que precisam de validação; não representa uma aprovação implícita destas últimas.

## Functional Requirements

- **RF1 — Buscar cidade (confirmado):** permitir que a pessoa informe o nome de uma cidade e inicie uma busca usando o serviço de geocoding da Open-Meteo.
- **RF2 — Consultar clima atual (confirmado):** após a seleção de uma cidade, apresentar as condições atuais disponíveis pela Open-Meteo, incluindo temperatura expressa na unidade ativa. Os demais campos meteorológicos dependem de decisão em aberto.
- **RF3 — Consultar previsão de cinco dias (confirmado):** apresentar a previsão obtida pela Open-Meteo para hoje e os quatro dias seguintes. A granularidade e os campos por dia ainda precisam ser definidos.
- **RF4 — Alternar unidade de temperatura (confirmado):** permitir alternar entre Celsius e Fahrenheit e atualizar todos os valores de temperatura exibidos, tanto atuais quanto previstos.
- **RF5 — Identificar localização (confirmado):** tentar obter a localização da pessoa automaticamente. Se não houver permissão, solicitar que ela informe a cidade manualmente.
- **RF6 — Unidade inicial (confirmado):** usar Celsius como unidade padrão na primeira apresentação.
- **RF7 — Idioma inicial (confirmado, regra incompleta):** escolher o idioma conforme a localização obtida; usar pt-BR quando não for possível obter a localização. A lista de idiomas e a regra de mapeamento precisam ser definidas.
- **RF8 — Selecionar resultado de busca (derivado; validar):** permitir selecionar a cidade correta entre os resultados, inclusive quando houver homônimos. Os dados usados para diferenciá-los ainda não foram definidos.
- **RF9 — Comunicar estados da interface (proposto; validar):** comunicar carregamento, ausência de resultados e falhas ao buscar dados; oferecer nova tentativa quando aplicável.

## User Stories

As personas abaixo são as personas hipotéticas do discovery, não validadas por pesquisa.

- **US1 — Busca e seleção:** Como pessoa que decide o que fazer hoje, quero buscar e selecionar minha cidade para consultar as condições do lugar correto. (RF1, RF8)
- **US2 — Condições atuais:** Como pessoa que decide o que fazer hoje, quero consultar o clima atual da cidade selecionada para orientar minhas decisões imediatas. (RF2)
- **US3 — Planejamento:** Como pessoa que planeja os próximos dias, quero ver a previsão de hoje e dos quatro dias seguintes para organizar minhas atividades. (RF3)
- **US4 — Preferência de temperatura:** Como pessoa que decide o que fazer hoje, quero alternar entre Celsius e Fahrenheit para interpretar a temperatura na unidade que prefiro. (RF4, RF6)
- **US5 — Acesso por localização:** Como pessoa que decide o que fazer hoje, quero usar minha localização automaticamente ou informar uma cidade se negar a permissão para chegar às condições locais sem ficar bloqueada. (RF5)
- **US6 — Idioma compreensível:** Como pessoa usuária, quero ver a interface em um idioma associado à localização, com pt-BR quando a localização não puder ser obtida, para compreender as informações apresentadas. (RF7)
- **US7 — Uso móvel:** Como pessoa que decide o que fazer hoje, quero usar as funções principais em um dispositivo móvel para consultar o clima durante minha rotina. (RNF1)
- **US8 — Recuperação de falhas:** Como pessoa que planeja os próximos dias, quero receber uma indicação quando a busca ou a consulta falhar e saber quando posso tentar novamente. (RF9; proposta a validar)

## Acceptance Criteria

Os critérios usam o formato **Dado / Quando / Então**. Para comportamentos ainda não decididos, o critério limita-se ao que foi confirmado e aponta a decisão pendente.

### RF1 — Buscar cidade

- **AC1.1:** Dado que a pessoa informou um nome de cidade não vazio, quando iniciar a busca, então a aplicação deve consultar resultados correspondentes na fonte de localização disponível.
- **AC1.2:** Dado que a pessoa enviou a busca, quando a consulta estiver em andamento, então a aplicação deve manter a interface utilizável e indicar que a busca está em andamento. (Proposto; validar junto de RF9.)

### RF2 — Consultar clima atual

- **AC2.1:** Dada uma cidade selecionada e uma resposta atual válida, quando os dados forem apresentados, então a aplicação deve exibir as condições atuais dessa cidade e sua temperatura na unidade ativa.
- **AC2.2:** Dada uma cidade selecionada e uma falha ao obter as condições atuais, quando a resposta de erro for recebida, então a aplicação deve informar que não foi possível carregar os dados, sem apresentar o erro como condição meteorológica. (Proposto; validar junto de RF9.)
- **Pendente:** os campos meteorológicos obrigatórios além da temperatura ainda precisam ser definidos.

### RF3 — Consultar previsão de cinco dias

- **AC3.1:** Dada uma cidade selecionada e uma resposta válida de previsão, quando a previsão for apresentada, então devem ser exibidos cinco dias: hoje e os quatro dias seguintes.
- **AC3.2:** Dada a previsão organizada em dias, quando a pessoa consultar cada dia, então a data deve estar associada às informações daquele dia, sem duplicar ou omitir uma das cinco datas.
- **Pendente:** campos por dia, granularidade e regra de fuso horário ainda precisam ser definidos.

### RF4 — Alternar unidade

- **AC4.1:** Dado que temperaturas estão sendo exibidas em Celsius, quando a pessoa selecionar Fahrenheit, então todas as temperaturas atuais e previstas visíveis devem ser apresentadas em Fahrenheit.
- **AC4.2:** Dado que temperaturas estão sendo exibidas em Fahrenheit, quando a pessoa selecionar Celsius, então todas as temperaturas atuais e previstas visíveis devem ser apresentadas em Celsius.
- **AC4.3:** Dado que a unidade foi alterada, quando a aplicação atualizar os valores, então os rótulos de unidade devem corresponder aos valores apresentados e nenhuma temperatura deve permanecer identificada incorretamente.

### RF5 — Identificar localização

- **AC5.1:** Dado que a pessoa concede permissão para localização, quando a aplicação obtiver uma posição, então deve usá-la para identificar o local da consulta meteorológica.
- **AC5.2:** Dado que a pessoa não concede permissão, quando a tentativa de localização for recusada, então a aplicação deve solicitar que ela informe a cidade e permitir seguir pela busca manual.
- **Pendente:** momento da solicitação e tratamento de localização indisponível ou imprecisa ainda precisam ser definidos.

### RF6 — Unidade inicial

- **AC6.1:** Dada uma primeira visita sem preferência de unidade previamente salva, quando a aplicação exibir temperaturas, então elas devem estar em Celsius.
- **AC6.2:** Dada uma preferência Fahrenheit já escolhida durante a visita atual, quando a pessoa consultar outra cidade, então a unidade selecionada deve ser aplicada aos valores apresentados. (A persistência entre visitas permanece em aberto.)

### RF7 — Idioma inicial

- **AC7.1:** Dada uma localização obtida e uma regra de idioma definida para ela, quando a interface for inicializada, então os textos da interface devem usar o idioma correspondente.
- **AC7.2:** Dado que a localização não pôde ser obtida, quando a interface for inicializada, então o idioma deve ser pt-BR.
- **Pendente:** idiomas suportados, mapeamento entre localização e idioma e possibilidade de troca manual ainda precisam ser definidos.

### RF8 — Selecionar resultado de busca

- **AC8.1:** Dada uma busca com um ou mais resultados, quando a pessoa selecionar um deles, então a aplicação deve usar a cidade selecionada para carregar o clima atual e a previsão.
- **AC8.2:** Dada uma busca que retorna cidades homônimas, quando os resultados forem apresentados, então deve ser possível distinguir e selecionar a cidade desejada. Os atributos de desambiguação ainda precisam ser definidos.

### RF9 — Estados da interface (proposto; validar)

- **AC9.1:** Dada uma busca sem correspondências, quando a consulta terminar, então a aplicação deve comunicar que não encontrou resultados e permitir uma nova busca.
- **AC9.2:** Dada uma falha recuperável ao consultar localização ou dados meteorológicos, quando a falha ocorrer, então a aplicação deve apresentar uma mensagem compreensível e uma ação de nova tentativa quando aplicável.
- **AC9.3:** Dada uma consulta em andamento, quando os dados ainda não estiverem disponíveis, então a interface deve indicar o estado de carregamento sem apresentar dados novos como se já estivessem atualizados.
- **Nota:** estados e ações de erro são propostas do discovery e precisam de validação de produto.

### RNF1 — Uso em dispositivos móveis

- **AC-NF1:** Dado um dispositivo móvel suportado, quando a pessoa usar busca, seleção de cidade, consulta de clima e alternância de unidade, então essas funções devem permanecer utilizáveis. As dimensões mínimas, dispositivos e navegadores de referência ainda precisam ser definidos.

## Non-Functional Requirements

- **RNF1 — Responsividade (confirmado em princípio):** as funções principais devem ser utilizáveis em dispositivos móveis. Larguras, orientação, tablets, desktop e navegadores suportados ainda precisam de definição.
- **RNF2 — Acessibilidade (proposto; validar):** considerar operação por teclado, estrutura semântica, rótulos acessíveis, foco visível e contraste adequado. O nível de conformidade e critérios verificáveis estão em aberto.
- **RNF3 — Desempenho (proposto; validar):** busca e consulta devem ter tempos de resposta aceitáveis. Não há metas numéricas, dispositivo ou conexão de referência acordados.
- **RNF4 — Resiliência (proposto; validar):** falhas de rede ou do provedor devem ser comunicadas sem bloquear permanentemente o uso; avaliar nova tentativa e cache. Não há requisito offline confirmado.
- **RNF5 — Fonte de dados (decisão confirmada):** usar Open-Meteo para geocoding e forecast, sem chave de API. Confirmar cobertura, disponibilidade, limites e termos de uso para as localidades atendidas.
- **RNF6 — Privacidade da localização (a detalhar):** a localização automática exige uma experiência de permissão compreensível e deve respeitar a alternativa de informar a cidade. Momento, texto e tratamento de outros estados de permissão ainda estão em aberto.
- **RNF7 — Consistência de unidades:** todos os valores de temperatura apresentados devem corresponder à unidade selecionada e ao respectivo rótulo. Limites de precisão e arredondamento ainda precisam ser definidos.

## Edge Cases

| Caso | Comportamento esperado |
| --- | --- |
| Campo de busca vazio ou contendo apenas espaços | Não iniciar uma consulta de cidade; manter a possibilidade de informar um nome válido. O texto exato de orientação é decisão de UX. |
| Cidade inexistente ou busca sem resultados | Não selecionar uma cidade arbitrária; comunicar ausência de resultados e permitir nova busca. |
| Nome com acentos, espaços ou caracteres especiais válidos | Enviar o texto à busca sem corromper ou descartar caracteres; apresentar resultados retornados pela fonte. Regras de normalização precisam ser definidas. |
| Cidades homônimas | Não escolher automaticamente um resultado ambíguo; permitir que a pessoa selecione o local desejado. Os campos de desambiguação estão em aberto. |
| Permissão de localização negada | Solicitar que a pessoa informe uma cidade manualmente, conforme requisito confirmado. |
| Localização indisponível, timeout ou posição imprecisa | Não presumir uma cidade; oferecer entrada manual como comportamento de contingência proposto, sujeito à validação do fluxo. |
| Falha, indisponibilidade ou limite do provedor meteorológico | Informar que os dados não puderam ser carregados e oferecer nova tentativa quando possível; não mostrar a falha como previsão válida. |
| Timeout ou conexão interrompida durante busca/consulta | Encerrar ou permitir recuperação da operação sem congelar a interface; indicar que a consulta falhou. Tempo limite e política de repetição estão em aberto. |
| Resposta sem resultados de geocodificação | Tratar como busca sem resultados, sem iniciar consulta meteorológica para uma cidade não selecionada. |
| Resposta parcial ou campo meteorológico ausente | Preservar os dados válidos e não inventar o campo ausente; a representação visual do campo e os campos obrigatórios precisam ser definidos. |
| Resposta com menos de cinco dias ou datas duplicadas | Não afirmar que a previsão completa de cinco dias foi carregada; comunicar indisponibilidade parcial. O tratamento visual detalhado precisa ser definido. |
| Mudança entre Celsius e Fahrenheit durante carregamento | Garantir que a unidade final apresentada e os respectivos rótulos sejam consistentes; não misturar valores de unidades diferentes. |
| Cidade próxima a uma mudança de data ou em fuso horário distinto | Apresentar as datas associadas à previsão sem deslocamento acidental; a regra de fuso horário precisa ser decidida antes de validar esse comportamento. |
| Localização concedida, mas idioma/localidade não suportado | Não há regra confirmada para idioma não suportado; definir a lista de idiomas e fallback antes da implementação. |

## Assumptions

- A aplicação depende de um serviço meteorológico externo e de conectividade para obter dados atualizados; cache ou funcionamento offline não estão confirmados.
- A pessoa poderá identificar uma cidade por busca manual quando não conceder permissão de localização.
- As personas do discovery são hipóteses iniciais e não representam segmentos validados por pesquisa.
- A unidade inicial será Celsius; não se presume que a escolha posterior seja persistida entre visitas.
- Open-Meteo será usada para geocoding e forecast sem chave de API; suas condições de uso, cobertura e limites ainda precisam ser verificados.
- A previsão é para hoje e os quatro dias seguintes; a regra de fuso horário e o significado exato de “hoje” ainda precisam de decisão.

## Risks

As estimativas são iniciais e qualitativas, conforme o discovery; devem ser revistas quando as condições de uso da Open-Meteo e os critérios de produto forem confirmados.

| Risco | Probabilidade | Impacto | Mitigação |
| --- | --- | --- | --- |
| Open-Meteo apresentar indisponibilidade, limite de uso ou cobertura insuficiente | Média | Alto | Verificar cobertura, termos e limites de uso; desenhar estados de falha e recuperação. |
| Resultado de busca corresponder à cidade homônima errada | Alta | Médio | Permitir escolha explícita e definir dados de contexto suficientes para desambiguar. |
| Rede ou serviço falhar durante a consulta | Média | Alto | Comunicar erro, preservar a possibilidade de nova tentativa e decidir se haverá cache. |
| Previsão ser mostrada para datas incorretas devido ao fuso horário | Média | Médio | Definir o fuso de referência e validar datas em localidades diferentes. |
| Conversão ou rótulo de Celsius/Fahrenheit ficar incorreto | Baixa | Alto | Validar consistência de valores e rótulos em todas as telas e estados de carregamento. |
| Layout móvel dificultar o uso de funções principais | Média | Médio | Definir dispositivos de referência e validar o fluxo principal em tamanhos suportados. |
| Localização gerar preocupação de privacidade, ser negada ou resultar em posição inadequada | Média | Médio | Explicar e solicitar permissão de forma clara; manter busca manual e definir tratamento de posição indisponível/imprecisa. |

## Out of Scope

Não fazem parte do escopo base confirmado pelo briefing. Só devem ser incluídos se forem aprovados em discovery ou em uma revisão desta especificação:

- Contas, autenticação e sincronização entre dispositivos.
- Favoritos, histórico persistente de buscas ou preferências salvas entre visitas.
- Alertas meteorológicos e notificações push.
- Histórico meteorológico de datas passadas.
- Garantia de funcionamento offline ou consulta a dados em cache.
- Suporte a idiomas além daqueles que vierem a ser definidos. A lista de idiomas permanece uma pergunta aberta, portanto este limite ainda não pode ser fechado.

## Open Questions

1. **Quais requisitos de cobertura, atualização, termos de uso e limites de uso da Open-Meteo são aceitáveis?**
2. **Quais campos devem compor o clima atual e cada dia da previsão?** Definir também se a previsão será exclusivamente diária ou terá períodos/horas.
3. **Quais dados devem distinguir cidades homônimas?** Por exemplo, região ou país; esses campos são candidatos, não decisão confirmada.
4. **Quando solicitar localização e qual o fluxo se o recurso estiver indisponível, for bloqueado pelo navegador ou retornar uma posição imprecisa?** A entrada manual foi definida para ausência de permissão.
5. **A unidade escolhida deve persistir entre visitas?** Celsius está definido como padrão inicial.
6. **Quais idiomas serão suportados e como a localização será mapeada para o idioma?** Definir também se a pessoa pode alterar o idioma manualmente e qual será o fallback quando a posição for conhecida, mas o idioma não tiver tradução.
7. **A aplicação deve funcionar offline ou apresentar dados em cache?** Se sim, por quanto tempo os dados meteorológicos podem ser considerados válidos?
8. **Qual fuso horário define “hoje” na previsão de hoje + quatro dias?** O fuso da cidade selecionada, o dispositivo ou outra referência?
9. **Quais dispositivos, navegadores, larguras de tela e orientações devem ser suportados?** O requisito confirma uso móvel, mas não especifica limites nem suporte a desktop/tablet.
10. **Quais metas verificáveis de desempenho e acessibilidade devem ser adotadas?** Definir valores, condições de rede/dispositivo e nível de conformidade.
11. **Favoritos, histórico, autenticação ou sincronização são necessários?** Esses recursos estão fora do escopo base até confirmação.
12. **Qual será a precisão e regra de arredondamento das temperaturas e quais unidades além da temperatura, se houver, devem ser mostradas?**
