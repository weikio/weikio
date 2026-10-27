![Weik.io](.github/weikio-logo.svg#gh-light-mode-only)
![Weik.io](.github/weikio-logo-dark.svg#gh-dark-mode-only)

Weik.io is an open source GitOps integration platform that manages integrations, APIs, file transfers and events. The first Weik.io installation went into production in January 2018, and today Cross Wrap, Dinolift, Jouka, Junttan, Oscar Software, Steerprop and Tana run their integrations on Weik.io.

- Weik.io: https://weik.io
- Docs: https://weik.io/docs
- University: https://weik.io/university
- Weik.io Cloud: https://weikio.cloud
- Live demo: https://demo.weik.io
- GitHub: https://github.com/weikio/weikio

## Since 2018

| When | What |
|---|---|
| January 2018 | The first Weik.io installation goes into production. |
| 2019 | The [weikio organisation](https://github.com/weikio) on GitHub publishes its first open source .NET libraries. |
| 2022 | Release 2022.1, with agents, integration flows and event-driven integrations. |
| 2023 to July 2026 | Releases 2023.1 to 2026.1 added managed file transfer, the entity store, API management, event channels and change tracking. |
| October 2026 | Release 2026.2.0, with integrations as folders of Camel files in Git, environments and promotion, and installations on one server or on as many as you need. |

Every release since 2022.1 has built on the same agents, CLI and admin UI. This repository starts at release 2026.2.0, which is why its commit history is short. [CHANGELOG.md](CHANGELOG.md) lists the highlights of each release from 2022.1 on, and [The company behind Weik.io](https://weik.io/company) has the longer history.

## Call the live demo

The live demo at https://demo.weik.io is a running Weik.io with a finance project, its integrations and an API gateway. This call goes through the demo's gateway to the API `echo` with the public demo key, which has a rate limit:

```sh
curl -i https://api.demo.weik.io/echo/v1/ -H "X-Api-Key: 654ztmyazm0set339ykfznrgbp03nx4pa1znq9sqy2pj8zengcm0"
```

```
HTTP/2 200 
content-type: application/json
date: Sun, 01 Nov 2026 15:39:40 GMT
content-length: 153
strict-transport-security: max-age=31536000
traceresponse: 00-9f2475370a2521d26c210b8665f1306c-fdaa762f37fdc6cd-01

{"api":"echo","method":"GET","path":"/echo/","time":"2026-11-01T15:39:40+00:00","traceparent":"00-9f2475370a2521d26c210b8665f1306c-f327ae4b444f95dc-01"}
```

Every answer carries a `traceresponse` header with the ID of your call's trace. To see the call in the admin UI, sign in to the live demo as the viewer with `viewer@demo.weik.io` and the password `try-weikio-live-demo`, open APIs, then `echo` and Requests. The newest row is your call, and its trace opens from that row.

## Run it on your machine

[![Open in Dev Containers](https://img.shields.io/static/v1?label=Dev%20Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/weikio/try-weikio)

The Try Weik.io dev container runs the whole platform on your machine, with the `weikio` CLI signed in and the admin UI at http://localhost:8580. It needs Docker with 8 GB of memory, 4 CPUs and 20 GB of free disk, and the steps are in [weikio/try-weikio](https://github.com/weikio/try-weikio).

## Install the CLI

On Linux x64 this script checks the release's `SHA256SUMS` and installs the CLI as `~/.local/bin/weikio`:

```sh
curl -fsSL https://weik.io/install.sh | sh
```

The command needs `curl` and `ca-certificates`, which minimal images such as `debian:12` don't have, so install them there first with `apt-get update && apt-get install -y curl ca-certificates`.

On Windows x64 this script runs in PowerShell without administrator rights, and the CLI is signed by Adafy Oy:

```powershell
irm https://weik.io/install.ps1 | iex
```

## Your first integration

`weikio init` writes an integration folder with a Camel route and two settings files, and `weikio run` publishes it, deploys it to devtest and follows its log lines:

```
$ weikio init hello
Created hello/
  hello.camel.yaml        timer -> log, runs every 5 s
  application.properties  message=hello from weikio
  integration.yaml        name hello, other fields as commented examples

Next, to run it in project sandbox and environment devtest:
  weikio run ./hello

$ weikio run hello
Target: http://localhost:8580, project sandbox (context), environment devtest (context)
integration sandbox/hello created (version 0.1.0, built in 16.2 s)
deployment devtest/sandbox/hello created (follows latest)
Following the log lines of sandbox/hello. Ctrl-C stops following, and the integration keeps running.
09:14:07.113 UTC INFO  hello  dev  Exchange[ExchangePattern: InOnly, BodyType: String, Body: hello from weikio]
09:14:12.114 UTC INFO  hello  dev  Exchange[ExchangePattern: InOnly, BodyType: String, Body: hello from weikio]
```

The first build takes about 16 seconds because Weik.io resolves the Camel libraries the integration uses. A later change to configuration alone builds in a second or two. The guide with every step is [Your first integration](https://weik.io/docs/start/first-integration).

## Examples

The [examples](examples) folder has the integrations that the docs, the University videos and the live demo use, and its README shows how to apply them to your Weik.io installation:

- [`hello`](examples/hello): the integration `weikio init hello` writes, a timer that logs a line every 5 seconds
- [`finance/erp-invoice-export`](examples/finance/erp-invoice-export): a Camel integration that sends each new invoice to the ERP over HTTP, with a variable, a secret and the custom metric `invoices_processed`
- [`finance/transfers/supplier-invoices.yaml`](examples/finance/transfers/supplier-invoices.yaml): a transfer that moves supplier invoice PDFs from an FTP server to Azure Blob Storage every morning at 05:00 UTC
- [`finance/apis/invoices`](examples/finance/apis/invoices): the API `finance/invoices` with version `v1` and the subscription `partner-x`
- [`finance/invoice-created`](examples/finance/invoice-created): the channel that `erp-invoice-export` publishes to and `invoice-archiver` receives from
- [`.github/workflows`](examples/.github/workflows): `plan.yaml`, `devtest.yaml` and `production.yaml`, the GitHub Actions workflows that show the plan on each pull request and apply the folder to devtest and production after a merge

## Install Weik.io

- [Install Weik.io on one server](https://weik.io/docs/operate/install-single-server)
- [Install Weik.io on several servers](https://weik.io/docs/operate/install-cluster)
- [Install an agent](https://weik.io/docs/operate/sites-and-agents)
- [Install the CLI](https://weik.io/docs/start/install-the-cli)

The images are on Docker Hub, tagged with the exact version and with no `latest` tag: [`weikio/weikio-backend`](https://hub.docker.com/r/weikio/weikio-backend), [`weikio/weikio-agent`](https://hub.docker.com/r/weikio/weikio-agent), [`weikio/weikio-apim`](https://hub.docker.com/r/weikio/weikio-apim) and [`weikio/weikio-forwarder`](https://hub.docker.com/r/weikio/weikio-forwarder).

## Current release

| Release | Published | Notes | Files |
|---|---|---|---|
| 2026.2.1 | 28 October 2026 | [Release notes](https://weik.io/releases/2026.2.1) | [The CLI, `SHA256SUMS` and `images.env`](https://github.com/weikio/weikio/releases/tag/2026.2.1) |

[CHANGELOG.md](CHANGELOG.md) lists every release with its notes. To hear about each new release, watch this repository with Custom and Releases.

## Licence

Weik.io is open source under the [Apache License 2.0](LICENSE), copyright Adafy Oy.

## Ideas

Post your ideas in the [Ideas category of Discussions](https://github.com/weikio/weikio/discussions/categories/ideas), where the Weik.io team answers them. [Where Weik.io is going](https://weik.io/roadmap) says what's coming.

## Getting help

- Ask questions in [Discussions](https://github.com/weikio/weikio/discussions).
- Report a bug with the [bug form](https://github.com/weikio/weikio/issues/new?template=bug.yml), and a problem in the docs with the [docs form](https://github.com/weikio/weikio/issues/new?template=docs.yml).
- Support subscriptions for the installations you run yourself are on https://weik.io/support, and the team that develops Weik.io answers every case.

## Security

Report a vulnerability to security@weik.io or through GitHub's private vulnerability reporting on this repository. [SECURITY.md](SECURITY.md) says what to include and which releases receive fixes.
