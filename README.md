# Eleições 2026
Dashboard de resultados oficiais: presidente nacional e por UF, governador e senador em todas as 27 UFs. Node 22+, sem dependências externas.

Execute `npm start` e abra http://localhost:3000. Docker: `docker compose up -d --build`. EasyPanel: build via Dockerfile, porta 3000, volume persistente em /app/data.

Uma rodada de coleta consulta os arquivos oficiais com até 6 leituras independentes simultâneas. A próxima rodada busca respeitar o intervalo de 5 segundos entre inícios, sem sobreposição. A latência de rede e o tempo de publicação da CDN do TSE podem aumentar o atraso: não existe garantia de defasagem máxima de 5 segundos. SSE envia as novas versões ao navegador. ETag evita retransmissões; erros acionam recuo, inclusive 11 minutos em HTTP 403/429. O painel conserva o último resultado e mostra falhas e horários.

Cada versão distinta (IDG) é persistida em data/history.jsonl antes da publicação. O arquivo permite retomar o histórico após reinícios. O gráfico usa horário de coleta; cada ponto informa também o horário do TSE. Não há reconstrução do período anterior à primeira coleta, nem pontos simulados. Para registrar toda a apuração, mantenha o serviço ativo desde o início da divulgação. Interrupções de coleta podem perder versões intermediárias. Não há previsão de vitória ou margem de erro estatística; diferenças são em votos e pontos percentuais. Situações e destinação dos votos são exibidas conforme TSE.

Fonte e documentação: https://www.tse.jus.br/eleicoes/informacoes-tecnicas-sobre-a-divulgacao-de-resultados
Códigos confirmados no arquivo oficial ele-c.json: ciclo ele2026, federal 6257, estadual 6259, 1º turno em 04/10/2026. Cargo 1 presidente, 3 governador, 5 senador. Para outro turno, adaptar códigos e separar o histórico.

Não foram executados testes automatizados, conforme instrução do usuário. Validação operacional consiste em iniciar o serviço e conferir os dados oficiais recebidos. Ferramenta work_state_write indisponível nesta sessão; estado registrado em HANDOFF.md.
