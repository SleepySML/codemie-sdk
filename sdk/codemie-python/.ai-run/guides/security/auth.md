# Python SDK Authentication

**Project**: codemie-sdk · **Stack**: Python requests ^2.31.0, pydantic ^2.12.2 · **Scope**: authentication modes, localhost handling and SSL verification in the Python SDK; how services receive the token source is in `architecture/architecture.md`.

## Authentication

### Rules
- R1. Change the localhost pattern list in both `CodeMieClient._is_localhost_domain` and `ApiRequestHandler._is_localhost_domain`: the client uses its copy to skip Keycloak, and the handler uses its copy to send `Authorization: dev-codemie-user` when no token is set (the match is a substring test, so a host containing `192.168` counts as local). Evidence: `sdk/codemie-python/src/codemie_sdk/client/client.py::CodeMieClient._is_localhost_domain`
- R2. Pass `verify=self._verify_ssl` (or `self.verify_ssl`) on every `requests` call. The calls in `utils/http.py` and `auth/credentials.py` all do this, and none hardcodes `verify=False`. Evidence: `sdk/codemie-python/src/codemie_sdk/auth/credentials.py::KeycloakCredentials.get_token`

### Read first
| When you are | Exemplar | Why this one | Tests |
|---|---|---|---|
| adding a Keycloak grant or token source | `sdk/codemie-python/src/codemie_sdk/auth/credentials.py::KeycloakCredentials.get_token` | grant order (token exchange, then password, then client credentials) and the `expires_in` minus 30 s cache in one method | `sdk/codemie-python/tests/test_credentials.py::test_external_token_wins_over_password_grant` |
