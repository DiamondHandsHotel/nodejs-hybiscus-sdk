<div align="center">
    <a href="https://hybiscus.dev">
    <img width="40%" src="https://hybiscus.dev/public/img/Wordmark.svg" alt="Hybiscus logo"/>
    </a>
</div>

<div align="center">
    Hybiscus is a cloud based service for building PDF reports using pre-designed components that look impressive without any effort. Simply define the content using a simple JSON API and get a PDF in return in a matter of seconds.
</div>

---

# 🌺 Hybiscus SDK (NodeJS)
> NodeJS SDK for interacting with the Hybiscus API

## 🪛 Requirements
- NodeJS 12.X or newer

## 🛠 Installation
The library can be installed via `npm` as follows:

```shell
$   npm install @hybiscus/web-api
```

## 🚀 Usage
The NodeJS SDK provides a declarative API for building up the report and the components inside it. Below is a simple example to get you started:

> **Note** To use the Hybiscus API, you require an API key which you can get by signing up at [https://hybiscus.dev/signup](https://hybiscus.dev/signup) with a **Forever Free** plan. For more details on plans, see [here](https://hybiscus.dev/plans).

### Quick start

```js
const { HybiscusClient, Report, Components } = require("@hybiscus/web-api");
const { Row, Section, Table } = Components;

const section = new Section({ sectionTitle: "title" }).addComponents([
    new Row({ width: "2/3" }),
    new Row({ width: "2/3" }).addComponents([
        new Table({
            title: "Table title",
            headings: ["URL", "Page views"],
            rows: [
                ["google.com", "500"],
                ["bing.com", "50"],
            ]
        }),
    ]),
]);
const report = new Report({
    reportTitle: "Report title",
    reportByline: "The byline" 
}).addComponent(section);

const client = new HybiscusClient(process.env.HYBISCUS_API_KEY);
client
    .buildReport({report })
    .then((response) => {
        console.log(response);
    })
    .catch((error) => console.error(error));
```

The Promise returned by `client.buildReport` resolves to an object, which contains the URL for the generated PDF. The object is defined by the following interface:

```ts
interface IPDFReport {
    url: string | null;
    taskID: string | null;
    status: "SUCCESS" | "FAILED" | "RUNNING" | "QUEUED";
    errorMessage: string | null;
}

```

### Components
Classes are available for each of the components in the Hybiscus API. All component classes follow the same basic principle, initialise the component class using the options that are specified in the [API docs](https://hybiscus.dev/docs/components/section). The only difference with the TypeScript / NodeJS library is that instead of using `snake_case` formatting for the names, the names are changed to `camelCase`

Components which are specified as extendable in the API docs, have the optional method `.addComponents` or `.addComponent`, which you can use to add components within them. Components can be deeply nested through this way, giving a lot flexibility.

```js
const { Components } = require("@hybiscus/web-api");
const { Section, Text } = Components;

const section = new Section({ sectionTitle: "title" })
    .addComponents([
        new Section({ sectionTitle: "Sub-section" })
            .addComponents([
                new Text({ text: "Example text" }),
                new Text({ text: "More example text" }),
            ]);
        new Section({ sectionTitle: "Sub-section" })
            .addComponents([
                new Text({ text: "Example text" }),
                new Text({ text: "More example text" }),
            ]);
    ])
```

This forms part of the declarative API, which lets you define the report contents without worrying about layout and design, and focusing on content.

---
&copy; 2021, Hybiscus