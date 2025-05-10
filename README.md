# Stack Utils

Uma coleção de templates Docker Compose e Portainer para deploy fácil de mais de 10 aplicativos open-source.

## Visão Geral
- Deploy via Portainer ou Docker Swarm com um único comando.
- Suporte a Docker Secrets e arquivos `.env` opcionais.
- Compatibilidade com GlusterFS, Ceph e NFS (use `VOLUME_PATH=/mnt/`).

## Destaques
- **Implantação Simplificada**: comandos únicos, sem digitar credenciais toda vez.
- **Pronto para Produção**: semi-configurado para Swarm e Portainer.
- **Documentação em Português**: detalhes de cada variável e instruções passo a passo.
- **Open Source**: contribuições e pull requests são bem-vindos.

---

## Variáveis de Ambiente
Defina variáveis para automatizar configurações sem precisar editar arquivos de compose:

| Variável       | Descrição                                                         | Exemplo                         |
| -------------- | ----------------------------------------------------------------- | ------------------------------- |
| `DOMINIO`      | Domínio principal                                                 | `seudominio.com.br`             |
| `ACME_EMAIL`   | Email para certificados Let's Encrypt                              | `email@dominio.com`             |
| `SMTP_SENDER`  | Email remetente de notificações                                   | `nao-responda@dominio.com`      |
| `SMTP_SERVER`  | Endereço do servidor SMTP                                         | `smtp.gmail.com`                |
| `SMTP_USER`    | Usuário de autenticação SMTP                                      | `usuario@gmail.com`             |
| `SMTP_PASSWORD`| Senha ou senha de app SMTP                                        | `S3nh@Segura`                   |
| `NUMBER`       | Sufixo de instância (–1, –2, etc.); padrão: `-1`                 | `-1`                            |
| `VERSION`      | Versão do container; padrão: última estável                       | `latest`                        |
| `TRUSTED_IPS`  | IPs permitidos para conexões (padrão: localhost)                  | `192.168.0.0/24`                |

> **Dica:** em produção, prefira Docker Secrets para variáveis sensíveis.

---

## Primeiros Passos

### 1. Instalar Docker e Dependências
```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg lsb-release
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### 2. Inicializar Swarm e Rede Traefik
```bash
docker swarm init
docker network create --driver overlay traefik-public
```

### 3. Clonar o Repositório
```bash
git clone https://github.com/bruno-growthsales/stack-utils.git
cd stack-utils
```

### 4. Deploy do Portainer
```bash
SUBDOMINIO=portainer \
DOMINIO=seudominio.com.br \
  docker stack deploy -c chatwoot/portainer.yml superstack
```

### 5. Verificar Exposição de Portas
```bash
curl https://ipv4.am.i.mullvad.net/port/80
curl https://ipv4.am.i.mullvad.net/port/443
```

### 6. Acessar Portainer
- Acesse `http://<VPS_IP>:9000` para configurar Traefik e gerenciar stacks.

### 7. Deploy das Stacks
Para cada stack:
```bash
DOMAIN=subdominio.dominio.com.br \
  docker stack deploy -c stacks/<nome_da_stack>.yml <nome_da_stack>
```
Exemplo:
```bash
DOMAIN=n8n.seudominio.com.br docker stack deploy -c n8n/n8n.yml n8n
```

---

## Templates no Portainer
1. No Portainer, vá em **Settings > App Templates**.
2. Adicione a URL:
   `https://raw.githubusercontent.com/matheusmaiberg/one-click-stacks/main/templates.json`
3. Crie templates personalizados para facilitar o deploy via interface.

---

## Contribuições
- Envie pull requests para melhorias, correções e atualizações de versão.
- Abra issues para reportar problemas ou sugerir recursos.

---

*Licença MIT*
