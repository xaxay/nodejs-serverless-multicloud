# Course 1 — The Minimum Path to Multi-Cloud

**Outcome:** The minimum foundation to start migrating existing Python Lambda work to Node.js and developing for AWS, Azure, and GCP from day one. Developers write and verify TypeScript handlers for all three clouds using GitHub Copilot — without needing a cloud account.

**Format:** 26 self-contained lab topics. Each topic has its own folder under `~/lab/nodejs-training/` that persists after the session. A developer who misses a session can run any topic independently — no replay required.

**Recording priority:** 🟢 topics are the minimum — record these first. 🟡 topics fit naturally in the flow but can be deferred — record them if there is enough time, otherwise move to the next course.

**No cloud account required.**

---

### Development Environment

🟢 **1. Running a production-equivalent Linux environment on Windows**
WSL2 Ubuntu — servers are Linux, CI is Linux, Lambda is Linux. Eliminates "works on my machine" by developing in the same OS as production.

🟢 **2. Installing the correct Node.js runtime version**
Node.js 22 LTS — active across AWS, Azure, and GCP Lambda runtimes through April 2027.

🟢 **3. Installing Node.js using a version manager**
Volta, nvm, or fnm — switch runtime versions per project without reinstalling; matches how CI and cloud pipelines pin versions.

🟢 **4. Configuring the IDE for Linux-based development**
VS Code + WSL extension — edit Linux files from Windows with full IDE support, no file copying, no path translation issues.

### Project Initialization

🟢 **5. Initializing a new Lambda project**
Set up `package.json` from scratch — the manifest that controls everything downstream: dependencies, scripts, and engine constraints.

🟡 **6. Connecting to a corporate package registry**
Route all package traffic through Artifactory instead of the public registry — required in any enterprise environment.

🟡 **7. Authenticating with a corporate registry**
Manage auth tokens for local development and CI pipelines without committing credentials to git.

### Managing Dependencies

🟢 **8. Adding a production dependency**
Install a runtime package — what ends up inside the Lambda deployment zip.

🟢 **9. Adding a build-time-only dependency**
Install dev tools that never reach production — TypeScript, type definitions, test runners.

### Lambda Project Configuration

🟢 **10. Configuring TypeScript for Lambda**
Compiler settings that match the Lambda Node.js runtime — `commonjs` module format, source maps for CloudWatch, strict mode.

🟢 **11. Catching bugs before deployment**
TypeScript finds null dereferences and wrong types at compile time — bugs that would only surface in production with Python.

🟢 **12. Compiling TypeScript to deployable JavaScript**
Build the `dist/` output Lambda actually runs, and verify the handler returns the correct response shape locally.

🟢 **13. Standardizing build commands across the team**
`package.json` scripts so every developer and CI pipeline runs the same commands — no tribal knowledge about which flags to pass.

### Lambda Handler Patterns

🟢 **14. Decoupling business logic from cloud platform code**
The handler is a thin adapter — parse the event, call pure business logic, format the response. Business logic imports no cloud SDKs. This is what makes the same core logic deployable to any cloud without rewriting it.

🟢 **15. Parsing API Gateway request body correctly**
`event.body` is always a raw string, never an object — the most common bug Python developers introduce when moving to Node.js Lambda. Knowing this pattern before using Copilot is what makes the generated code verifiable.

### AI-Assisted Development and Multi-Cloud Verification

🟢 **16. Installing Copilot in a corporate environment**
VS Code extension setup through corporate proxy and SSL inspection — the practical blockers that prevent most enterprise developers from getting started.

🟢 **17. Converting a Python Lambda handler to TypeScript with Copilot**
Copilot generates the TypeScript handler and thin adapters for AWS, Azure, and GCP from the existing Python Lambda. TypeScript compilation catches type errors immediately.

🟢 **18. Verifying multi-cloud handler compatibility with official cloud testing frameworks**
The three adapters generated in topic 21 are verified using vendor-official tooling: the Azure adapter runs via `@azure/functions` testing utilities; the GCP adapter runs via `@google-cloud/functions-framework/testing`. When the vendor's own library accepts the handler, the interface is proven — not by developer judgment, by cloud vendor authority. No cloud account needed.

### Testing

🟢 **19. Setting up Jest for TypeScript Lambda projects**
Configure Jest with ts-jest — run TypeScript tests without a separate compile step.

🟢 **20. Writing unit tests for Lambda handlers**
Test business logic in isolation — assert on the response shape and verify error paths without deploying.

🟡 **21. Debugging with VS Code and source maps**
Set breakpoints in TypeScript source and step through execution in the VS Code debugger — source maps translate runtime line numbers back to the original code.

### Local Testing

🟢 **22. Running Lambda locally with SAM CLI**
`sam local start-api` starts a local HTTP server running the handler inside the official Lambda Docker image — hit it with curl or a browser exactly as API Gateway would. Requires a container runtime — Podman or Rancher Desktop are the recommended enterprise-friendly options (both free, no licensing restrictions).

### Bonus

⭐ **Using Copilot to learn Node.js as a Python developer**
How to use Copilot as a personal learning companion — asking it to explain Node.js and serverless concepts in Python terms, compare patterns side by side, and answer "why does this work differently than in Python?" in the context of your own code. Personalized to each developer's questions and gaps.

---

## Course 2 — Advanced Node.js Ecosystem and Production Practices

Course 1 covers the Node.js foundation. Course 2 goes deeper into the Node.js ecosystem — areas where the tooling, conventions, and failure modes differ significantly from Python and require dedicated coverage. Combined with production-grade resilience and multi-cloud service integration, this course brings the team to a level where Node.js serverless functions are ready for real environments.

Each area can be delivered first as a cross-cloud survey covering AWS, Azure, and GCP, then expanded into per-cloud depth courses as needed.

---

**Dependency Management Depth**
*Includes lockfile pinning, CVE scanning, and vulnerability fixes — topics intentionally left out of Course 1 so they can be covered here with the depth they deserve.*
*Required foundation for everything else in Course 2. Version range rules, license auditing, phantom dependency protection, peer conflict resolution. Enterprise teams without this ship with hidden risk — the wrong version range installs different packages on different machines, and unlisted transitive packages silently reach production.*

**Local Service Emulation — Basics**
*LocalStack (AWS), Azurite (Azure Storage and Queue), and GCP emulators (Pub/Sub, Firestore) run cloud service APIs locally. Developers test against real API surfaces without touching any cloud or incurring cost. Basic course covers all three clouds; advanced per-cloud depth is a separate follow-on.*

**Reliability and Performance**
*Production-grade serverless behavior across AWS, Azure, and GCP. Covers failure modes and latency problems that only appear at scale — invisible during development but guaranteed in production.*

- Serverless failure resilience — idempotency, dead letter topics (SQS on AWS, Service Bus on Azure, Pub/Sub on GCP), timeout handling.
- Serverless performance and cold start optimization — SDK client initialization, connection reuse across warm invocations. Applies to AWS Lambda, Azure Functions, and GCP Cloud Functions equally.

**Advanced Security and CI/CD Enforcement**
*Blocking the pipeline on high-severity CVEs, scoping audits to production dependencies, enforcing lockfile integrity across every developer and CI run. Compliance requirements in most enterprise environments, not optional improvements.*

---

## Available for Further Courses

The following areas are available after Course 2. Each is self-contained and can be commissioned independently.

**Deep Local Service Emulation — Per-Cloud**
Advanced per-cloud depth after the Course 2 basics: full DynamoDB and SQS workflows on LocalStack, Azure Storage and Service Bus on Azurite, Pub/Sub and Firestore on GCP emulators. Integration tests that cover complete service interactions, not just connectivity.

**Multi-Cloud Deployment**
Each cloud covered as its own topic — deploying to AWS with SAM, Azure with Functions Core Tools, GCP with Functions Framework — then combined into a unified approach where switching provider is a configuration change, not a rewrite. All three targets plus the shared CI/CD pipeline can be delivered as one course built on the decoupling pattern from Course 1.

---

**22 topics · 🟢 19 core · 🟡 3 optional · no cloud account required**
