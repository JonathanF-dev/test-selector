# AGENTS.md

## Project context
This repository contains a Java/Spring Boot system for a bachelor thesis on regression test selection.
Static analysis is not applicable because the generated integration tests and the Java application run in separate environments.
Mappings between tests and invoked Java classes/methods are derived from runtime logs from Grafana/Loki.

## Architecture rules
- The system is a modular monolith.
- Follow hexagonal architecture.
- Keep business logic out of controllers, schedulers, and infrastructure adapters.
- External systems must be accessed via ports and adapters.
- Keep selection and prioritization strictly separated.
- Start with class-level selection. Method-level selection is a later extension.

## Module boundaries
- `mapping` builds and maintains test-to-code mappings.
- `commitanalysis` determines changed Java artifacts from commits.
- `selection` selects relevant tests for a change set.
- `prioritization` orders already selected tests.
- `domain` contains core domain models and domain services.
- `application` contains use cases and orchestration.
- `adapter` contains infrastructure and entry points.

## Coding rules
- Use constructor injection.
- Prefer small, focused classes.
- Use domain-oriented names.
- Keep domain classes framework-independent where possible.
- Do not introduce unnecessary abstractions.
- Do not add new dependencies without justification.

## Build and test
- Build with: `mvn clean verify`
- Run tests with: `mvn test`
- A task is not done unless the project builds and relevant tests pass.

## Documentation rules
- Add JavaDoc to all public classes and public methods.
- Update `docs/architecture.md` for architecture-relevant changes.
- Keep naming consistent with `docs/domain-glossary.md`.
- Document non-trivial modules or use cases in `docs/`.

## Review guidelines
- Flag architecture boundary violations.
- Flag missing tests for business logic.
- Flag unclear naming.
- Flag direct controller-to-repository access.
- Flag missing documentation updates.

## Definition of done
A task is only done if:
- implementation is complete,
- tests were added or updated where needed,
- public APIs are documented,
- architecture rules are respected,
- relevant documentation is updated.

## Do not
- Do not put business logic into adapters or controllers.
- Do not mix test selection and prioritization logic.
- Do not bypass ports for external systems.
- Do not rename core domain terms without updating the glossary and documentation.