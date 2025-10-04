🧠 Security Dashboard + Web Attack Simulator



Projeto didático e interativo desenvolvido por Filipe Penido, combinando segurança ofensiva e defensiva em um ambiente visual e seguro, acessível diretamente pelo navegador.
O projeto é single-file (HTML, CSS e JavaScript puro), sem dependências externas — ideal para ser publicado no GitHub Pages, usado em workshops, portfólios e treinamentos técnicos.

<img width="1101" height="869" alt="image" src="https://github.com/user-attachments/assets/e8597653-cf54-4120-aed2-c5f302907d2f" />

🔗 Demo online: https://sunsh1x03.github.io/security-js-tools

⚙️ Recursos principais

---

📊 Painel de Segurança

Consulta IP público, geolocalização e verificação de senhas vazadas (modelo k-anon da API Have I Been Pwned).

Inclui integrações opcionais com VirusTotal, Shodan e HIBP (modo mock disponível).

---

🧪 Simulador de Ataques Web

Demonstra XSS (modos seguro, inseguro e sandboxed) e SQL Injection (vulnerável vs. parametrizado).

Mostra, em tempo real, como boas práticas mitigam vulnerabilidades.

---

📘 Modo tutorial

Tour visual interativo que explica cada função passo a passo.

Ideal para treinamentos, workshops e demonstrações de segurança.

---

📄 Automação

Gera automaticamente um arquivo README.md com instruções, APIs e créditos.

---

🛡️ Configuração segura de APIs

- Configure proxies/serverless functions que armazenem as chaves de serviços como VirusTotal, Shodan e HIBP em variáveis de ambiente.
- Defina `window.VIRUSTOTAL_PROXY_URL` antes do script principal apontando para seu endpoint seguro para habilitar o botão "Checar". Exemplo:

```html
<script>
  window.VIRUSTOTAL_PROXY_URL = 'https://sua-funcao.exemplo.com/virustotal';
</script>
```

- Sem proxy configurado, utilize apenas o botão **Mockar resposta**.

---

💡 Filosofia

Ensinar como as falhas funcionam e como preveni-las, equilibrando:



🛡️ Visão ofensiva: como o atacante pensa.

🔐 Visão defensiva: como proteger e validar código seguro.

---

🔒 Segurança e boas práticas

- **Proxies obrigatórios para chaves:** O frontend não faz chamadas diretas para APIs que exigem segredo. Configure variáveis globais como `VIRUSTOTAL_PROXY_URL` apontando para um backend/serverless que mantenha as chaves em variáveis de ambiente e implemente autenticação, rate limiting e logs. No frontend, utilize apenas o modo *mock* até que o proxy esteja ativo.
- **Proteja as chaves no navegador:** Campos de API agora usam `type="password"` e não exibem valores digitados. Recomendamos deixar os inputs vazios e armazenar os segredos apenas no backend.
- **Política de Segurança de Conteúdo (CSP):** Publique o arquivo com um cabeçalho semelhante a `Content-Security-Policy: default-src 'self'; img-src 'self' data:; connect-src 'self' https://api.ipify.org https://ipapi.co https://api.pwnedpasswords.com; frame-src 'self';` e inclua os domínios dos seus proxies/API necessárias.
- **Sandbox XSS didático:** O modo "Iframe sandbox" executa o payload em um contêiner isolado apenas para demonstração. Não reutilize esse modo para processar entrada de usuários em produção sem sanitização.
- **Boas práticas adicionais:** Aplique debounce/rate limiting nas ações de consulta no backend para evitar abuso e consumo indevido de quotas de API.

Nenhum dado sensível é transmitido em texto claro.

As APIs externas são opcionais e devem ser executadas via proxy seguro.

Código simples, claro e totalmente didático.

---

👤 Autor & Créditos

Projeto concebido, customizado e documentado por Filipe Penido.
Partes do código foram geradas com apoio de ferramentas de IA para acelerar a implementação; todo o conteúdo final foi revisado e adaptado pelo autor.

Se utilizar ou referenciar este projeto, mantenha os créditos originais.
