> [!IMPORTANT]  
> **RF-042 - Autenticação em Duas Etapas (2FA)**

> [!NOTE]  
> **Prioridade:** 🔴 Alta | **Sprint:** 3 | **Responsável:** @maria.pereira

O sistema deve obrigar o usuário a inserir um código TOTP (via Google Authenticator ou SMS) após a validação de senha.

**✅ Critérios de Aceitação:**
1. O código deve expirar em 60 segundos.
2. O usuário pode solicitar reenvio de SMS até 3 vezes.
3. Em caso de falha 5 vezes consecutivas, a conta é bloqueada por 15 minutos.

> [!WARNING]  
> **Impacto em Segurança:** Este requisito substitui a antiga pergunta de segurança (RF-018). Remover o RF-018 ao implementar este.