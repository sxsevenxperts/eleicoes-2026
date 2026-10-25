# Eleições 2026 — 2º turno

Dashboard com apuração oficial do TSE para presidente e governador nas UFs com segundo turno. Usa Node.js 22+, sem dependências externas.

Execute `npm start` e abra http://localhost:3000. Docker: `docker compose up -d --build`. EasyPanel: build pelo Dockerfile, porta 3000 e volume persistente em `/app/data`.

O TSE publica os arquivos do segundo turno no ciclo `ele2026`: código 6258 para presidente (eleição federal) e 6260 para governador (eleição estadual), ambos identificados como turno `2`. O painel acompanha presidente no Brasil e governador em AC, AM, DF, ES, RN e TO.

Uma rodada consulta os arquivos oficiais com até 6 leituras simultâneas e sem sobreposição. A próxima rodada busca respeitar 5 segundos entre inícios; a latência de rede e publicação da CDN pode aumentar o atraso. SSE envia as novas versões ao navegador e ETag evita retransmissões. Erros acionam recuo, incluindo 11 minutos para HTTP 403/429. O painel conserva o último resultado e informa erros e horários da fonte.

Cada versão distinta (IDG) é gravada em `data/history-turn2.jsonl`, separada do histórico do primeiro turno para não misturar as apurações. O histórico é retomado após reinícios. O gráfico usa horário de coleta e informa também o horário publicado pelo TSE. Não reconstrói o período anterior à primeira coleta e não apresenta pontos simulados. Interrupções podem perder versões intermediárias. Margem de erro não se aplica à totalização; diferenças são mostradas em votos e pontos percentuais.

Fonte oficial: https://www.tse.jus.br/eleicoes/informacoes-tecnicas-sobre-a-divulgacao-de-resultados
