# Advanced Workflows in Postman

The afternoon sessions will use [this OpenAPI Specification file](./openapi-books/books-api-afternoon.yaml), and we recommend that you import this as a new API within Postman.

### If you need help

If you have trouble implementing something, [please ask for help](./getting-help.md).

## Importing

We're going to start by importing the YAML file into Postman, and telling Postman to import it as a Test Suite.

---

## Topics and Documentation Links

We're going to talk you through several of Postman's features of API design, development, deployment and monitoring, and more. All of this will be done with the API-First Mindset that we've discussed in our earlier sessions, and we'll take time for questions if time allows.

The list below may not be done in this order, and we will not likely make it through all of this material. Below, we provide links for each of these areas so that you can continue to work within Postman using [this example OpenAPI design](./openapi-books/books-api-afternoon.yaml).


**Testing**
- Documentation Links:
   - https://learning.postman.com/docs/writing-scripts/test-scripts/
   - https://learning.postman.com/docs/running-collections/building-workflows/
   - https://learning.postman.com/docs/designing-and-developing-your-api/validating-elements-against-schema/
- Adding more testing to look at alternate ways of checking Schema validation
- Request chaining, using `setNextRequest` with some branch logic
   - the `setNextRequest` only happens if you use the Collection Runner, won't run as one-off execution
   - queues up the command, and is the last thing executed no matter where we find it in the script
   - either put setNextRequest in every test, or in none of them?
   - if a test has no setNextRequest, the runner will go to the next request in the list

**Mock Servers**
- Documentation Links:
   - https://learning.postman.com/docs/designing-and-developing-your-api/mocking-data/setting-up-mock/
- Using collection examples to build mock servers
- Use the mock servers to execute our tests

**Documentation**
- Documentation Links:
   - https://www.postman.com/api-documentation-tool/
- API-side documentation based on OpenAPI Specification for developers
- Client-side documentation

**Code Generation**
- Documentation Links:
   - https://learning.postman.com/docs/designing-and-developing-your-api/generating-server-code/
   - https://learning.postman.com/docs/developer/code-generators/
- server-side code generation
- client-side code generation

**Deployment**
- Documentation Links:
   - https://learning.postman.com/docs/designing-and-developing-your-api/deploying-an-api/deploying-an-api-overview/
   - https://www.postman.com/api-evangelist/workspace/deployment/overview
- AWS demo, API Gateway

**Monitoring**
- Documentation Links:
   - https://learning.postman.com/docs/monitoring-your-api/intro-monitors/
- create monitoring in Postman
- uptime monitoring

**Git integration**
- Documentation Links:
   - https://learning.postman.com/docs/designing-and-developing-your-api/versioning-an-api/using-external-git-repo/
- add GitHub repo to our API
   - sync our changes to GitHub
   - what does Postman export/sync to GitHub?

**CI/CD Automation**
- Documentation Links:
   - https://learning.postman.com/docs/integrations/ci-integrations/
   - https://www.postman.com/automated-testing/
- Newman
   - using Newman with an exported collection
   - using the Postman API to dynamically fetch collections, environments, etc

