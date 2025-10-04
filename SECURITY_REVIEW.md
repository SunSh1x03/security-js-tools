# Security Review

## Escopo
Análise de segurança e revisão de código do projeto **Security Dashboard + Web Attack Simulator** (single-page HTML + CSS + JavaScript). Foram avaliados apenas os artefatos presentes no repositório atual.

## Visão geral
O projeto tem natureza didática, expondo intencionalmente cenários de vulnerabilidade (ex.: uso de `innerHTML` e consulta SQL concatenada) para fins educativos. Não há backend ou persistência; todas as interações ocorrem no navegador. Ainda assim, alguns cuidados adicionais ajudam a proteger usuários em ambientes reais de demonstração.

## Pontos positivos
- Demonstra claramente boas práticas (uso de `textContent`, prepared statements simulados, alerta sobre proxies para chaves de API).
- Utiliza o modelo k-anon da API *Have I Been Pwned*, evitando envio direto da senha.
- Iframes destinados à simulação XSS usam atributo `sandbox`, restringindo impacto fora do contexto demonstrativo.

## Achados e recomendações
### 1. Exposição de chaves de API no frontend (risco alto)
Os campos para VirusTotal, Shodan e HIBP são exibidos como `input` padrão. Mesmo com a orientação textual de usar um proxy, o código tenta consumir a API diretamente do navegador, o que:
- expõe a chave a qualquer pessoa com acesso ao DOM ou DevTools;
- pode falhar por CORS e incentivar más práticas (inserir chave real no frontend).

**Mitigação sugerida:** transformar os campos em `type="password"`, desabilitar a chamada direta por padrão e reforçar a orientação no README para usar um backend/proxy obrigatório. Oferecer apenas o modo *mock* quando o proxy não estiver configurado.

### 2. Ausência de política de segurança de conteúdo (CSP) (risco médio)
Como o arquivo contém CSS e JavaScript inline, um eventual *host* poderia permitir injeções se o servidor aceitar uploads adicionais ou modificar o arquivo. Uma CSP básica reduz o risco de execução de scripts externos maliciosos.

**Mitigação sugerida:** documentar uma configuração de cabeçalho `Content-Security-Policy` que restrinja a execução a `'self'` e `https://api.ipify.org https://ipapi.co https://api.pwnedpasswords.com https://www.virustotal.com` (ajustar conforme proxies utilizados).

### 3. Falta de *rate limiting* e tratamento de abuso em chamadas externas (risco baixo)
Os botões disparam chamadas diretas às APIs sem limitar frequência ou validar formatos (ex.: URL para VirusTotal). Usuários maliciosos podem acionar loops e consumir cotas rapidamente.

**Mitigação sugerida:** implementar debounce/throttle simples no frontend e, preferencialmente, mover chamadas para backend com autenticação e quotas.

### 4. Sanitização no modo sandbox (risco baixo)
No modo sandbox de XSS, o payload é escrito dentro de `iframe` sandboxado sem sanitização, o que é intencional. Entretanto, se o arquivo for publicado sem entender a finalidade, scripts maliciosos ainda podem ser executados dentro do iframe (embora confinados).

**Mitigação sugerida:** adicionar aviso visível/tooltip de que o modo sandbox executa scripts apenas para demonstração e garantir que o atributo `sandbox` mantenha restrições (sem `allow-same-origin`/`allow-scripts` externos).

## Próximos passos recomendados
1. Atualizar o README com diretrizes operacionais seguras (uso obrigatório de proxy, CSP sugerida, evitar expor chaves).
2. Implementar as mitigações listadas, priorizando a proteção das chaves de API.
3. Considerar adicionar testes de segurança automatizados (linting, scanners estáticos) ao fluxo de CI para manter boas práticas.

---
Relatório elaborado em 2024-05-21.
