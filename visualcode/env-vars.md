LLM Environment Variables (Contract)

Services may use one LLM, embeddings, both, or neither.
No variable is mandatory unless the service actually needs it.

Base variables

LLM_BASE_URL, LLM_API_KEY → main LLM calls

LLM_EMBEDDING_BASE_URL, LLM_EMBEDDING_API_KEY → embedding calls

If a service doesn’t use LLMs or embeddings, these are simply not required.

Options convention

Any variable prefixed with LLM_OPTION_* or LLM_EMBEDDING_OPTION_* is collected into an options object.

The prefix is removed and the remainder is lowercased
(LLM_EMBEDDING_OPTION_MODEL → model).

This keeps provider options deterministic and avoids service-specific parsing.

Agent-specific overrides (optional)

Agents may define AGENT_<NAME>_LLM_OPTION_* to override options per agent.

If not defined, agents fall back to the main LLM options.

No defaults

The code must not define defaults.

Missing or incorrect variables are allowed and will fail at runtime if required by the request.