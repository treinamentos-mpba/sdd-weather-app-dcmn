# Discovery — Aplicação de previsão do tempo

## Contexto

O briefing solicita uma aplicação de previsão do tempo na qual usuários possam buscar cidades, consultar o clima atual, ver a previsão de cinco dias e alternar entre Celsius e Fahrenheit. O uso em dispositivos móveis é um requisito explícito.

Durante o discovery, foram esclarecidos estes pontos: a fonte de dados deve funcionar sem chave de API; a janela de cinco dias inclui hoje e os quatro dias seguintes; a aplicação deve tentar obter a localização automaticamente e, se não houver permissão, solicitar que a pessoa informe a cidade; Celsius é a unidade padrão; e o idioma deve acompanhar a localização, com pt-BR como fallback quando ela não puder ser obtida. O provedor, os dados a exibir e outros detalhes da experiência ainda precisam de decisão.

## Requisitos Funcionais

- **RF1 — Buscar cidades (explícito):** permitir que o usuário procure uma cidade.
- **RF2 — Consultar o clima atual (explícito):** apresentar informações do clima atual para a cidade escolhida. As informações exatas ainda precisam ser definidas.
- **RF3 — Consultar a previsão (explícito):** apresentar a previsão de hoje e dos quatro dias seguintes. Os campos e a granularidade ainda precisam ser definidos.
- **RF4 — Alternar unidades (explícito):** permitir alternar entre Celsius e Fahrenheit e refletir a unidade escolhida nos valores exibidos.
- **RF5 — Identificar a localização (esclarecido):** tentar obter a localização automaticamente; se não houver permissão, solicitar que a pessoa informe a cidade.
- **RF6 — Definir unidade inicial (esclarecido):** usar Celsius como unidade padrão.
- **RF7 — Definir idioma inicial (esclarecido):** escolher o idioma conforme a localização automática; se ela não puder ser obtida, usar pt-BR.
- **RF8 — Escolher resultado da busca (derivado; validar):** permitir que o usuário selecione a cidade desejada entre os resultados, especialmente quando houver cidades homônimas.
- **RF9 — Informar estados da experiência (proposto; validar):** comunicar carregamento, busca sem resultados e falha ao consultar dados; oferecer nova tentativa quando apropriado.

## Requisitos Não-Funcionais

O suporte a dispositivos móveis é explícito. Os requisitos abaixo são propostas de qualidade para validação, não metas já acordadas.

- **RNF1 — Responsividade (explícito):** a experiência deve ser utilizável em dispositivos móveis. Larguras de tela, orientação e suporte a tablets/desktop precisam ser acordados.
- **RNF2 — Acessibilidade (proposto):** buscar uso por teclado, estrutura semântica, rótulos acessíveis e contraste adequado. O nível de conformidade esperado está em aberto.
- **RNF3 — Desempenho (proposto):** busca e carregamento das condições devem responder em tempo aceitável. Os limites e a conexão de referência ainda precisam ser definidos.
- **RNF4 — Resiliência (proposto):** falhas de rede ou do provedor não devem deixar a interface sem explicação; comportamento de nova tentativa e eventual uso de dados em cache precisam ser definidos.
- **RNF5 — Fonte sem chave de API (restrição esclarecida):** a solução deve usar uma fonte de dados que não exija chave de API. Isso não define qual provedor, seus termos, cobertura ou limites de uso.
- **RNF6 — Privacidade (a detalhar):** a localização automática foi solicitada; definir quando e como explicar seu uso e solicitar permissão, respeitando a alternativa de entrada manual já definida.

## Riscos

As probabilidades e os impactos são avaliações iniciais para orientar discovery, não medições. Devem ser revistos quando as decisões de produto e o provedor forem conhecidos.

| Risco | Probabilidade | Impacto | Mitigação proposta |
| --- | --- | --- | --- |
| A fonte sem chave escolhida ter indisponibilidade, limites de uso ou cobertura insuficiente | Média | Alto | Avaliar termos, cobertura e limites antes da escolha; prever mensagens de erro e nova tentativa. |
| Busca retornar cidades homônimas ou localização imprecisa | Alta | Médio | Exibir contexto geográfico nos resultados (por exemplo, região e país); validar com usuários. |
| Falha ou lentidão na rede ou no serviço impedir a consulta | Média | Alto | Definir estados de carregamento e erro, permitir nova tentativa e avaliar cache após decidir requisitos offline. |
| Dados diários serem associados ao dia errado por diferenças de fuso horário | Média | Médio | Definir fuso horário e limites do dia usados pela previsão; validar datas em localidades diferentes. |
| Valores ou rótulos ficarem inconsistentes ao alternar Celsius/Fahrenheit | Baixa | Alto | Definir unidade da interface de forma consistente e cobrir a conversão com testes. |
| Layout ou controles não funcionarem em alguns dispositivos móveis | Média | Médio | Definir larguras e navegadores suportados; validar em telas e dispositivos representativos. |
| Localização automática ser negada, indisponível ou imprecisa, ou gerar preocupação de privacidade | Média | Médio | Manter entrada manual quando não houver permissão; explicar o uso da localização e validar o fluxo para localização indisponível ou imprecisa. |

## Perguntas em Aberto (Open Questions)

1. **Qual provedor de dados será usado, além da exigência de não precisar de chave de API?** Quais requisitos de cobertura, atualização, termos de uso e limites são aceitáveis? **Impacto:** determina a viabilidade, cobertura geográfica, confiabilidade e eventuais restrições de distribuição.
2. **Quais informações devem aparecer no clima atual e em cada dia da previsão?** A previsão será um resumo diário ou incluirá períodos/horas? **Impacto:** afeta a utilidade percebida, o contrato com a fonte de dados e a organização da interface.
3. **Como a busca deve tratar cidades com o mesmo nome?** Quais dados de contexto devem aparecer nos resultados, e a busca deve sugerir resultados enquanto a pessoa digita? **Impacto:** sem desambiguação, o usuário pode consultar a localidade errada; sugestões também afetam a experiência e o comportamento da busca.
4. **Em que momento a aplicação deve solicitar acesso à localização? O que deve ocorrer se o recurso de localização estiver indisponível ou retornar uma posição imprecisa?** A entrada manual foi definida como alternativa quando não houver permissão. **Impacto:** afeta o fluxo inicial, a confiança do usuário e a forma de selecionar a cidade correta.
5. **A unidade escolhida pela pessoa deve ser lembrada entre visitas?** Celsius está definido como padrão inicial. **Impacto:** determina se é necessário persistir a preferência no dispositivo.
6. **Quais idiomas serão suportados, como a localização determinará o idioma e a pessoa poderá alterá-lo manualmente?** pt-BR está definido como fallback quando a localização não puder ser obtida. **Impacto:** influencia tradução, formatos de data e número, preferência do usuário e esforço de localização.
7. **A aplicação deve funcionar sem conexão ou exibir dados recentes em cache?** Por quanto tempo os dados poderiam ser considerados úteis? **Impacto:** define armazenamento local, tratamento de desatualização e complexidade de implementação.
8. **A previsão de hoje + quatro dias deve respeitar o fuso horário da cidade selecionada?** **Impacto:** afeta a data apresentada, principalmente perto da meia-noite ou em locais distantes do usuário.
9. **Quais dispositivos, navegadores e larguras de tela são prioritários?** O requisito de uso móvel não define suporte a desktop ou tablets. **Impacto:** orienta validação de responsividade e compatibilidade.
10. **Quais metas de desempenho e acessibilidade devem ser aceitas?** **Impacto:** sem critérios verificáveis, não será possível avaliar objetivamente qualidade e conclusão da entrega.
11. **São necessários favoritos, histórico de buscas, contas ou sincronização entre dispositivos?** **Impacto:** pode adicionar persistência, autenticação e escopo além da consulta pontual; nenhum desses recursos está confirmado no briefing.

## Suposições (Assumptions)

Estas hipóteses não são decisões e precisam ser confirmadas antes de orientar a especificação:

- A aplicação será acessada por uma interface digital; o briefing não define plataforma ou tecnologia.
- A aplicação tentará identificar a localização automaticamente e recorrerá à entrada manual da cidade se não houver permissão.
- Os dados meteorológicos virão de um serviço externo, sujeito a disponibilidade e cobertura; nenhum provedor foi escolhido.
- Não há requisito confirmado de autenticação, favoritos, histórico ou persistência de preferências.

## Personas Hipotéticas

Personas iniciais para orientar perguntas de discovery; não foram validadas com pesquisa de usuários.

- **Pessoa que decide o que fazer hoje:** quer consultar rapidamente o clima atual antes de sair. O uso em celular é plausível, dado o requisito de suporte móvel, mas não foi confirmado como exclusivo.
- **Pessoa que planeja os próximos dias:** quer comparar a previsão de hoje e dos quatro dias seguintes para uma cidade. O contexto, a frequência de uso e os critérios de sucesso ainda precisam de validação.

## Decisões Confirmadas no Discovery

- **Fonte de dados:** há a restrição de não exigir chave de API. O provedor específico continua em aberto.
- **Janela da previsão:** hoje mais os quatro dias seguintes.
- **Localização:** tentar obter a localização automaticamente; se não houver permissão, solicitar a cidade manualmente.
- **Idioma:** selecionar conforme a localização; usar pt-BR se ela não puder ser obtida. Idiomas suportados e regra de seleção continuam em aberto.
- **Unidade padrão:** Celsius. Persistência da escolha do usuário continua em aberto.
