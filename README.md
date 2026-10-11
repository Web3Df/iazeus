# ZEUS IA Autônomo

Repositório para desenvolver o agente heurístico local ZEUS e preparar sua distribuição Android.

## Arquitetura planejada

- Código Python e interface Android Kivy
- Modelo heurístico local
- Biblioteca ZEUS versionada no dispositivo
- Memória SQLite local
- Event Log e hashes SHA-256
- Compilação Android por GitHub Actions
- Atualizações opcionais via GitHub Releases
- Publicação Play Console por OIDC, limitada à faixa de teste interno

## Estado verificado neste repositório

- Repositório GitHub criado: sim
- Workflow de validação estrutural: adicionado
- Workflow de publicação Play Console por OIDC: adicionado como configuração
- Aplicativo Android completo e compilável: ainda precisa ser incorporado/validado neste repositório
- Publicação real na Play Store: não realizada
- Configuração Google Cloud/Play Console: depende do proprietário da conta

## Publicação segura no Play Console

Consulte [docs/GOOGLE_PLAY_OIDC.md](docs/GOOGLE_PLAY_OIDC.md). O workflow de publicação espera um artefato AAB já compilado em outra execução do GitHub Actions. Não contém credenciais permanentes e não publica em produção.

## Limites

O workflow de publicação não cria permissões no Google Cloud ou Play Console. O proprietário precisa configurar Workload Identity Federation, o ambiente protegido `play-internal`, e as variáveis do repositório. Não coloque senhas, tokens ou chaves privadas nos arquivos.
