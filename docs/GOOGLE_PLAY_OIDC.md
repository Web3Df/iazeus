# ZEUS — Google Play Console com credenciais temporárias (OIDC)

## Estado

Este repositório contém o workflow de publicação para **teste interno**. Ele não publica automaticamente em produção. A publicação só funciona depois que o proprietário configura Google Cloud, Play Console e as variáveis do GitHub.

## Pré-requisitos

1. Criar/selecionar um projeto Google Cloud e ativar a Google Play Android Developer API.
2. Criar uma conta de serviço dedicada, sem chave JSON permanente.
3. No Play Console, conceder à conta de serviço acesso somente ao aplicativo ZEUS e permissões mínimas para gerenciar versões/testes.
4. Configurar Workload Identity Federation para confiar apenas no repositório `Web3Df/iazeus`, na branch `main` e no ambiente `play-internal`.
5. Conceder à conta de serviço `roles/iam.workloadIdentityUser` para o principal federado autorizado.
6. Criar o ambiente GitHub `play-internal`. Recomenda-se exigir aprovação de um mantenedor antes do job.
7. Confirmar o identificador de pacote Android usado pelo app.

## Variáveis do repositório

Em **Settings → Secrets and variables → Actions → Variables**, configurar:

- `GCP_WORKLOAD_IDENTITY_PROVIDER`: caminho completo do provider, por exemplo `projects/PROJECT_NUMBER/locations/global/workloadIdentityPools/POOL_ID/providers/PROVIDER_ID`.
- `GCP_SERVICE_ACCOUNT`: e-mail da conta de serviço dedicada.
- `PLAY_PACKAGE_NAME`: identificador real do pacote Android (applicationId).

Não adicionar senha, chave JSON ou token permanente. O workflow usa OIDC para obter credencial temporária.

## Uso

1. Um workflow de compilação deve produzir um artefato GitHub Actions contendo exatamente um arquivo `.aab`, com nome padrão `zeus-aab` (ou o nome informado manualmente).
2. Abra **Actions → ZEUS - Publicação Play Store (teste interno) → Run workflow**.
3. Escolha o artefato correto e prefira o estado `draft` para a primeira validação.
4. Confira o log, o hash SHA-256 e a faixa de teste interno no Play Console.

## Limites e segurança

- O workflow publica apenas na faixa `internal`; não contém promoção para produção.
- O AAB deve estar assinado e usar o mesmo applicationId e a chave de assinatura de upload esperada pelo app.
- O arquivo de credenciais temporárias não deve ser incluído no artefato nem commitado.
- A configuração OIDC precisa impor condições de atributo para impedir outros repositórios/workflows.
- GitHub Actions não consegue criar a autorização do Play Console por si só; o proprietário deve configurar a confiança e as permissões uma vez.
- O workflow depende de um artefato AAB já produzido. Se o projeto Android não tiver um workflow de compilação funcional, ele deverá ser criado e validado separadamente.

## Referências oficiais

- GitHub OIDC com Google Cloud: https://docs.github.com/en/actions/how-tos/secure-your-work/security-harden-deployments/oidc-in-google-cloud-platform
- Google Cloud Workload Identity Federation: https://cloud.google.com/iam/docs/workload-identity-federation
- Google Play Developer API: https://developers.google.com/android-publisher
