**English** · [Русский](quarkus-for-java-services.md) 

# Quarkus for Java Services: Advantages, Limitations, and a Comparison with Spring Boot

**As of October 20, 2026.** This comparison covers the latest stable releases, **Quarkus 3.40.1.1** (the 3.40 LTS line) and **Spring Boot 4.1.1**. Pre-release versions are excluded. The versions are listed on the [Quarkus 3.40.1.1 release page](https://github.com/quarkusio/quarkus/releases/tag/3.40.1.1) and the [Spring Boot project page](https://spring.io/projects/spring-boot/).

## Why Quarkus

Quarkus combines a broad range of Jakarta EE and MicroProfile APIs with build-time application preparation. These APIs cover web services, dependency injection, data access, transactions, validation, JSON, configuration, security, observability, and messaging. For a team experienced with Jakarta EE and MicroProfile, this preserves a familiar programming model, while moving metadata processing into the build reduces work at startup. [Quarkus CDI 3.40](https://quarkus.io/version/3.40/guides/cdi-reference/), [Quarkus Persistence 3.40](https://quarkus.io/version/3.40/guides/hibernate-orm/), [extension model 3.40](https://quarkus.io/version/3.40/guides/writing-extensions/)

Quarkus prepares applications at build time for both execution modes: JVM and Native Image. Choose the mode based on service requirements and measurements under load. [Quarkus: JVM and Native Image](https://quarkus.io/guides/building-native-image/)

## Growth of Quarkus and Its Ecosystem

Quarkus was publicly introduced as an open-source project in March 2019. It had 185 contributors by November of that year; by the Quarkus 3.40 LTS release in September 2026, the cumulative count had reached 1,250. This shows the growth of the project's developer community. The 2024 Eclipse Foundation survey also indicates adoption: 32% of respondents working on cloud-native Java applications used Quarkus. That figure describes the survey respondents, not the entire Java market. [Project history](https://quarkus.io/blog/quarkus-in-a-foundation/), [contributors in 2019](https://quarkus.io/blog/quarkus-1-0-0-CR2-released/), [Quarkus 3.40 LTS](https://quarkus.io/blog/quarkus-3-40-released/), [Eclipse Foundation survey](https://jakarta.ee/blogs/2024-cloud-native-java-survey-findings/)

Quarkus evolves alongside the Java platform: it supports Java 25 and virtual threads, uses Vert.x for non-blocking I/O, combines imperative and reactive programming models, and can build applications for the JVM or as Native Images. Build-time component preparation integrates these capabilities through the Quarkus extension model. [Java 25](https://quarkus.io/blog/quarkus-3-31-released/), [virtual threads](https://quarkus.io/version/3.40/guides/rest-virtual-threads/), [reactive architecture](https://quarkus.io/version/3.40/guides/quarkus-reactive-architecture/), [extension model](https://quarkus.io/version/3.40/guides/writing-extensions/)

Platform extensions and Quarkiverse projects address common requirements of modern services, including data access, security, messaging, observability, and cloud integration. The catalog continues to grow; check each extension's maturity status and compatibility with the Quarkus version in use. [Extension catalog](https://quarkus.io/extensions/), [extension registry](https://quarkus.io/guides/extension-registry-user/)

## Familiar Jakarta EE and MicroProfile Model

Quarkus supports a broad set of Jakarta EE APIs through extensions. Developers can use familiar annotations and interfaces while adding only the technologies their service needs. The table summarizes the main capabilities.

| Jakarta EE area | Implementation in Quarkus 3.40.1.1 |
| --- | --- |
| Web resources and dependency injection | Jakarta REST through Quarkus REST; CDI 4.1 Lite with additions through ArC. [REST](https://quarkus.io/guides/rest/), [CDI](https://quarkus.io/version/3.40/guides/cdi-reference/) |
| Servlet and WebSocket | Servlet API through `quarkus-undertow`; Jakarta WebSocket through `quarkus-websockets`. WebSockets Next offers a separate API and does not implement Jakarta WebSocket. [Servlet](https://quarkus.io/version/3.40/guides/http-reference/), [Jakarta WebSocket](https://quarkus.io/version/3.40/guides/websockets/) |
| Persistence and transactions | Jakarta Persistence through Hibernate ORM and Jakarta Transactions through Narayana JTA. [Persistence](https://quarkus.io/version/3.40/guides/hibernate-orm/), [Transactions](https://quarkus.io/guides/transaction/) |
| Validation | Jakarta Validation through Hibernate Validator. [Validation](https://quarkus.io/guides/validation/) |
| JSON | Jakarta JSON Binding through JSON-B and Jakarta JSON Processing through JSON-P are available with the corresponding extensions. [JSON REST](https://quarkus.io/guides/rest-json/) |
| Data repositories | Jakarta Data is supported by the Quarkus Data Hibernate extension. The extension is experimental, so consider its maturity when choosing it. [Quarkus Data Hibernate](https://quarkus.io/version/3.40/guides/quarkus-data-hibernate/) |

### Panache for Data Access

The `quarkus-hibernate-orm-panache` extension simplifies entity and query code on top of Hibernate ORM and Jakarta Persistence. Panache supports two styles: Active Record, which places operations on the entity class, and repositories through `PanacheRepository`, which keep entities as ordinary Jakarta Persistence entities. This reduces repetitive code for common data operations. [Hibernate ORM with Panache](https://quarkus.io/version/3.40/guides/hibernate-orm-panache/)

Panache is a Quarkus API, not part of Jakarta EE or MicroProfile. It is useful for new services when the team accepts a Quarkus dependency in the data access layer; if that layer must remain portable, it can use Jakarta Persistence directly. Panache is also distinct from Jakarta Data and the Quarkus Data Hibernate extension in the table above. [Hibernate ORM with Panache](https://quarkus.io/version/3.40/guides/hibernate-orm-panache/), [Quarkus Data Hibernate](https://quarkus.io/version/3.40/guides/quarkus-data-hibernate/)

### MicroProfile APIs

MicroProfile Config underlies Quarkus configuration. Other APIs are implemented by corresponding platform extensions and added as needed:

| MicroProfile API | Implementation in Quarkus |
| --- | --- |
| MicroProfile Config | Quarkus configuration is based on SmallRye Config. [Configuration reference](https://quarkus.io/guides/config-reference/) |
| MicroProfile Health | The `quarkus-smallrye-health` extension implements application health checks. [Health guide](https://quarkus.io/guides/smallrye-health/) |
| MicroProfile REST Client | The `quarkus-rest-client` extension includes the MicroProfile REST Client API. [REST Client guide](https://quarkus.io/guides/rest-client/) |
| MicroProfile Fault Tolerance | The `quarkus-smallrye-fault-tolerance` extension provides retries, timeouts, and fallbacks. By default, there are small differences from the specification: methods returning `CompletionStage` or `Uni` receive asynchronous fault tolerance without `@Asynchronous`, and `@Retry`, `@Fallback`, and `@CircuitBreaker` inspect exception cause chains. For strict compatibility, set `quarkus.fault-tolerance.mp-compatibility=true`. [Fault Tolerance guide: Additional resources](https://quarkus.io/version/3.40/guides/smallrye-fault-tolerance/#additional-resources) |
| MicroProfile OpenAPI | The `quarkus-smallrye-openapi` extension processes MicroProfile OpenAPI metadata and publishes an API description. [OpenAPI](https://quarkus.io/version/3.40/guides/openapi-swaggerui/) |
| MicroProfile JWT RBAC | The `quarkus-smallrye-jwt` extension implements MicroProfile JWT for token verification and role-based access. [JWT](https://quarkus.io/version/3.40/guides/security-jwt/) |
| MicroProfile Context Propagation | The `quarkus-smallrye-context-propagation` extension carries contexts across asynchronous stages. [Context Propagation](https://quarkus.io/guides/context-propagation/) |
| MicroProfile Reactive Messaging | Quarkus Messaging extensions use the MicroProfile Reactive Messaging specification for channels and message handlers. [Messaging](https://quarkus.io/guides/messaging/) |
| MicroProfile GraphQL | The `quarkus-smallrye-graphql` extension implements MicroProfile GraphQL. [GraphQL](https://quarkus.io/guides/smallrye-graphql/) |

A team can develop a service using Jakarta EE and MicroProfile concepts without adopting Spring components. When migrating an existing application, account for differences in configuration, bean discovery, and CDI Full support. For example, CDI Portable Extensions require a different implementation using Quarkus extension mechanisms. [ArC limitations](https://quarkus.io/version/3.40/guides/cdi-reference/)

## What Quarkus Does at Build Time

After compiling an application, a Quarkus build runs an **augmentation** phase. It moves some framework work from startup into the build pipeline. The amount of work depends on the extensions, application classes, and configuration; there is no universal duration for this phase. [Quarkus build and startup phases](https://quarkus.io/version/3.40/guides/writing-extensions/)

| Work performed during augmentation | What is prepared for startup |
| --- | --- |
| Extensions index classes and read metadata from annotations, configuration, and descriptors. | The application does not need to parse all of this metadata again at every startup. [Quarkus extensions](https://quarkus.io/version/3.40/guides/writing-extensions/) |
| ArC discovers, validates, and wires CDI beans, generates required classes, and removes unused beans. | ArC has a prepared structure for container initialization. [Quarkus CDI](https://quarkus.io/version/3.40/guides/cdi-reference/) |
| Quarkus analyzes Jakarta REST resources and routes, including conflicting endpoint paths. | Duplicate route errors are detected during the build. [Quarkus REST](https://quarkus.io/guides/rest/) |
| Extensions prepare integrations by generating bytecode for runtime service initialization and, where needed, metadata for Native Image. | Startup executes prepared code rather than repeating all library analysis. [Quarkus extensions](https://quarkus.io/version/3.40/guides/writing-extensions/), [native tips](https://quarkus.io/guides/writing-native-applications-tips/) |
| When Hibernate ORM is used, Quarkus analyzes entities, enhances their bytecode, and parses XML mappings. | Part of the ORM model is prepared in advance; `SessionFactory` still starts with the application. [Hibernate ORM](https://quarkus.io/version/3.40/guides/hibernate-orm/) |

**Build-time boundary:** On the JVM, Quarkus still performs initialization at startup. Connecting to the database, opening a port, and reading runtime configuration remain startup tasks. A Native Image build may additionally include some static initialization in the binary. Quarkus moves metadata processing and code generation into the build, not the entire application startup. [Quarkus phases](https://quarkus.io/version/3.40/guides/writing-extensions/), [Hibernate ORM](https://quarkus.io/version/3.40/guides/hibernate-orm/)

## Reflection

Reflection remains part of Java and is useful when the set of types is determined dynamically. Quarkus aims to avoid it in framework internals: it discovers and wires CDI beans during the build and prepares required classes and metadata in advance. This is particularly visible with Quarkus extensions, which prepare integrations for startup and Native Image. Reflection remains available where application libraries need it, such as some JSON serialization scenarios. [Quarkus CDI](https://quarkus.io/version/3.40/guides/cdi-reference/), [Quarkus JSON](https://quarkus.io/guides/rest-json/), [Quarkus extensions](https://quarkus.io/version/3.40/guides/writing-extensions/)

It is more accurate to call reflection **a costly tool for certain scenarios** than an obsolete API. Moving class discovery and annotation analysis to the build reduces startup work. With Native Image, dynamic access also needs explicit metadata if the compiler cannot infer it from the code. [Quarkus: native reflection](https://quarkus.io/guides/writing-native-applications-tips/)

| Mode | Spring Boot 4.1.1 | Quarkus 3.40.1.1 |
| --- | --- | --- |
| Standard JVM | With a standard launch, Spring discovers components and parses configuration at startup. It uses reflection to invoke `@Bean` methods and access certain class members; it creates proxies for AOP. [Spring Boot AOT and JVM](https://docs.spring.io/spring-boot/reference/packaging/native-image/introducing-graalvm-native-images.html), [Spring AOP proxies](https://docs.spring.io/spring-framework/reference/core/aop/proxying.html) | ArC discovers, validates, and wires CDI beans during the build; some proxies and supporting code are prepared in advance. Application libraries may still use reflection at runtime. [Quarkus CDI](https://quarkus.io/version/3.40/guides/cdi-reference/), [Quarkus JSON](https://quarkus.io/guides/rest-json/) |
| JVM with AOT processing | Spring Boot can run a JAR with generated initialization code on the JVM. Build the application with AOT processing and launch it with `spring.aot.enabled=true`. [Spring Boot: AOT on the JVM](https://docs.spring.io/spring-boot/reference/packaging/aot.html) | Quarkus uses augmentation results with its standard JVM build and launch; no separate AOT mode is required for this. [Quarkus phases](https://quarkus.io/version/3.40/guides/writing-extensions/) |
| Native Image | Spring AOT generates bean definitions, proxies, and GraalVM hints in advance. Reflective access remains possible for registered types. [Spring Boot Native Image](https://docs.spring.io/spring-boot/reference/packaging/native-image/introducing-graalvm-native-images.html) | Quarkus and its extensions register many required types in advance. Developers register dynamic types that cannot be identified during the build. [Quarkus native tips](https://quarkus.io/guides/writing-native-applications-tips/) |

For JVM applications, the distinction is how build-time preparation is used: Quarkus applies it in its standard process, while Spring Boot offers a separate AOT mode for the JVM. This distinction alone does not establish which service will be faster or use fewer resources; measure both implementations. [Spring Boot: AOT on the JVM](https://docs.spring.io/spring-boot/reference/packaging/aot.html), [Quarkus phases](https://quarkus.io/version/3.40/guides/writing-extensions/)

## Code Removal: Three Different Mechanisms

It is important to distinguish Quarkus mechanisms from analysis performed by GraalVM Native Image. ArC removes unused CDI beans during a Quarkus build, while the experimental JAR tree-shaking feature can exclude unreachable dependency classes. Reachability analysis during native compilation applies to applications built with either Quarkus or Spring Boot.

| Mechanism | Where it applies | When to check it |
| --- | --- | --- |
| **ArC removes unused CDI beans** | During a Quarkus build, **including JVM builds**. ArC may not detect a bean used only through programmatic lookup with `CDI.current()`. Mark such beans with `@io.quarkus.arc.Unremovable` or list them in `quarkus.arc.unremovable-types`. [Quarkus CDI: removing unused beans](https://quarkus.io/version/3.40/guides/cdi-reference/) | Test paths that use programmatic CDI lookup; the risk is lower with ordinary static injection points. |
| **JAR dependency tree-shaking** | A separate **experimental** Quarkus feature, disabled by default. When enabled, it excludes unreachable classes from dependency JARs. Do not attribute this behavior to a standard Quarkus build. [Quarkus JAR tree-shaking](https://quarkus.io/guides/jar-tree-shaking/) | Check it only when enabled, especially if the application loads classes dynamically. |
| **Native Image reachability analysis** | During native compilation, GraalVM can remove code absent from the reachable call graph. For reflective access, Quarkus offers `@io.quarkus.runtime.annotations.RegisterForReflection` or Native Image configuration; extensions register many types automatically. This native-mode constraint also applies to Spring Boot. [Quarkus native tips](https://quarkus.io/guides/writing-native-applications-tips/), [Spring Boot Native Image](https://docs.spring.io/spring-boot/reference/packaging/native-image/introducing-graalvm-native-images.html) | Test real native executable scenarios, especially dynamic serialization and third-party libraries. |

`@Unremovable` and `@RegisterForReflection` solve **different problems**. The former preserves a CDI bean during ArC analysis and may be needed in JVM mode too. The latter preserves reflective access to a class and its members in Native Image; it does not prevent removal of a CDI bean. If Quarkus can infer a type from a REST method signature, manual registration is usually unnecessary. When returning an untyped `Response`, the body type may be unknown at build time, so `@RegisterForReflection` may be needed. [Quarkus REST JSON example](https://quarkus.io/guides/rest-json/)

## Choosing JVM or Native Image for Quarkus

For a long-running service, choose between JVM and Native Image based on its workload and resource constraints. The JVM uses JIT optimization for hot code and may deliver higher peak throughput; the build does not require native compilation. Native Image improves cold startup and generally reduces memory use. It can also benefit a long-running service if the same workload fits within lower memory or CPU limits. [Quarkus: building a native executable](https://quarkus.io/guides/building-native-image/)

Compare the modes under an equivalent functional workload, measuring throughput, latency, memory use, and CPU consumption at the planned limits. Also consider build time and the dynamic-scenario compatibility checks required for Native Image. CDI and other integrations are still prepared at build time in Quarkus JVM mode.

## Limitations That May Change the Decision

| Limitation | Impact for a Jakarta EE team |
| --- | --- |
| The application relies on CDI Full, Portable Extensions, or behavior specific to a Jakarta EE server. | **High** if these are essential: ArC implements CDI Lite with additions, and Portable Extensions are not supported in their usual form. Validate the use case with a prototype. [Quarkus CDI](https://quarkus.io/version/3.40/guides/cdi-reference/) |
| The organization already has many Spring-specific libraries and services. | **High** for migration, lower for a new project. The cost of rewriting and maintenance may outweigh the benefit of familiar Jakarta APIs. |
| The application uses programmatic CDI lookup or dynamic serialization. | **Moderate**: the first needs attention even on the JVM; the second matters especially with Native Image. Both have documented registration mechanisms. [Quarkus CDI](https://quarkus.io/version/3.40/guides/cdi-reference/), [Quarkus REST JSON](https://quarkus.io/guides/rest-json/) |
| Strict portability of MicroProfile Fault Tolerance behavior is required. | **Low**: the behavioral differences are small, and compatibility can be enabled with `quarkus.fault-tolerance.mp-compatibility=true`. During migration, test the asynchronous methods and exception handling actually used. [Quarkus Fault Tolerance: Additional resources](https://quarkus.io/version/3.40/guides/smallrye-fault-tolerance/#additional-resources) |

### Alternatives to Common Spring Components

An organization may already have an established set of Spring technologies for data access, security, and batch processing. Quarkus offers its own solutions for these tasks; the table gives examples. During migration, compare the specific features in use and estimate the cost of changing the code.

| Spring component | Quarkus capabilities | What to check during migration |
| --- | --- | --- |
| Spring Data JPA | Hibernate ORM with Panache for entities and repositories; the `quarkus-spring-data-jpa` compatibility extension supports a subset of the Spring Data JPA API. [Panache](https://quarkus.io/version/3.40/guides/hibernate-orm-panache/), [Spring Data JPA compatibility](https://quarkus.io/guides/spring-data-jpa/) | Panache has its own API; the compatibility extension does not implement every Spring Data JPA feature. |
| Spring Security | Quarkus Security with OIDC and JWT extensions can validate tokens and restrict access to resources. [Quarkus OIDC](https://quarkus.io/guides/security-oidc-bearer-token-authentication/), [Quarkus JWT](https://quarkus.io/version/3.40/guides/security-jwt/) | Compare the authorization rules, authentication mechanisms, and custom integrations in use. |
| Spring Batch | The Quarkiverse JBeret extension provides Jakarta Batch for batch processing; Quarkus Scheduler or Quartz starts jobs on a schedule. [JBeret](https://quarkus.io/extensions/io.quarkiverse.jberet/quarkus-jberet/), [Scheduler and Quartz](https://quarkus.io/version/3.40/guides/scheduler-reference/) | A scheduler does not replace batch step processing. JBeret has a different API from Spring Batch; check state persistence and job restart behavior separately. |

**Note.** The main Keycloak server distribution [moved from WildFly to Quarkus](https://www.keycloak.org/docs/26.0.8/upgrading/index.html). This is an example of Quarkus being used in a large security product. Quarkus services can use Keycloak as an identity provider through [OIDC](https://quarkus.io/guides/security-oidc-bearer-token-authentication/).

Spring Boot may be preferable when a service depends on specific Spring Data, Spring Security, or Spring Batch features whose migration would require substantial changes. For a new service, evaluate the required use cases rather than relying only on familiar library names.

**Recommendation:** Choose Quarkus for a new service built by a team experienced with Jakarta EE and MicroProfile when there is no substantial dependence on the Spring ecosystem. Select JVM or Native Image based on resource requirements and measurements under the target workload. Before migrating an existing application, check its dependencies on CDI Full, Portable Extensions, and libraries tied to a particular server.
