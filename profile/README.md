# Welcome to Project Apario

> **TL;DR** I regret building this piece of software when I did. I should have never built it. I was wrong to build it. I wish I could take it back.

In 2020 I built the **PhoenixVault** and then went through a _personal transformation_ that essentially _undercut_ myself from my own project so that I could recompnse that happened to me in Romania with what I was doing in America. The pandemic, really introduced those challenges for me as an Ceaușescu orphanage survivor that I overcame and built in service to others regardless of any naysayers or clout-deniers might try to proclaim.

![PhoenixVault](https://github.com/ProjectApario/reader/blob/1f0fa7247062a6e483f63629f9736ffd00e8d87e/bundled/assets/images/phoenixvault_logo-light.png)

by [Andrei Merlescu @andreimerlescu](https://github.com/andreimerlescu)  

## The PhoenixVault

There are **four (4)** components to the **PhoenixVault** that are: 

| Component | Year |Role | &nbsp; |
|-----------|------|-----|--------|
| `writer` | 2022 | Compiles the _Apario Database_ | [Explore &rarr;](https://github.com/ProjectApario/writer) | 
| `reader` | 2023 | Web Application for _Apario Database_ | [Explore &rarr;](https://github.com/ProjectApario/reader) | 
| `search` | 2024 | Gematria search index for _Apario Database_ | [Explore &rarr;](https://github.com/ProjectApario/search) |
| `merkel` | 2025 | Merkle trees for _Apario Database_` | [Explore &rarr;](https://github.com/ProjectApario/merkel) |

### The Writer 

This component compiles the _Apario Database_ that the [reader](https://github.com/ProjectApario/reader) consumes and displays an interactive web application that is self contained with Go powered SSL capabilities. To compile this database, you'll need a **Collection of Records** in the form of PDF files that you want to gether into a single _collection of data_ that will have **each component** part of it. 

```bash
mkdir -p ~/work/projectapario
cd ~/work/projectapario
git clone git@github.com:ProjectApario/writer.git
cd writer
```

The [writer](https://github.com/ProjectApario/writer) component is 3344 lines of code across 15 `.go` files. The [install.sh](https://github.com/ProjectApario/writer/blob/main/install.sh) script loads the dependencies onto your system so you can compile your own _Apario Database_. 

```bash
chmod +x install.sh && sudo ./install.sh
```

I use the `config.yml` strategy with the [writer](https://github.com/ProjectApario/writer). For example, if I am working on `teslafiles.info` then I'd have a `writer.yml` file that I'd use for my config file. To begin, lets ensure that some directories exist: 

```bash
mkdir -p "~/apario/teslafiles.info/{logs,config,workspace,data,import,app,search,ssl}"
```

This will create: 

```log
~/apario
~/apario/teslafiles.info
~/apario/teslafiles.info/logs
~/apario/teslafiles.info/config
~/apario/teslafiles.info/workspace
~/apario/teslafiles.info/data
~/apario/teslafiles.info/import
~/apario/teslafiles.info/search
~/apario/teslafiles.info/app
~/apario/teslafiles.info/ssl
```

| Directory | Purpose |
|-----------|---------|
| `~/apario/<collection-domain>/logs` | Contains log files |
| `~/apario/<collection-domain>/config` | Contains `[reader\|writer\|search\|merkel].yml` files |
| `~/apario/<collection-domain>/workspace` | Binary will use this as a temporary workspace |
| `~/apario/<collection-domain>/data` | When importing data into the _Apario Database_, you place the **originals** here. |
| `~/apario/<collection-domain>/import` | When you're importing data, your structured data goes in here such as your `.csv` or `.xlsx` files. |
| `~/apario/<collection-domain>/app` | The database(s) live inside of here. |
| `~/apario/<collection-domain>/search` | The index files for the _Apario Database_ [gematria](https://github.com/andreimerlescu/gematria) included binary data. |
| `~/apario/<collection-domain>/ssl` | The TLS | SSL files for the [reader](https://github.com/ProjectApario/reader). |

The `writer.yml` file in `~/apario/teslafiles.info/config` for **teslafiles.info**: 

```yml
---
log: ~/apario/teslafiles.info/logs/writer.log
database-directory: ~/apario/teslafiles.info/app
no-clam: true
language: eng
```

When using a `.csv` file, you'd add: 

```yml
import-csv: ~/apario/teslafiles.info/import/teslafiles.csv
csv-column-url: URL
csv-column-path: PATH
csv-column-record-number: ID
csv-column-title: TITLE
```

Your `.csv` file would have: 

```csv
ID,TITLE,URL,PATH
1,Document One,https://example.com/document1.pdf,~/apario/teslafiles.info/data/document1.pdf
2,Document Two,https://example.com/document2.pdf,~/apario/teslafiles.info/data/document2.pdf
3,Document Three,https://example.com/document3.pdf,~/apario/teslafiles.info/data/document3.pdf
```

Then running the **writer** is a matter of: 

```bash
go build -o writer .
chmod +x writer
./writer -config ~/apario/teslafiles.info/config/writer.yml
```

Then, the application will begin streaming detailed logging information into the `logs:` directory choice from the `writer.yml` setting destination and minimal information to the STDOUT of the `./writer ...` invocation. 

### The Reader

This **component** is responsible for rendering an **interactive web application** that is _embedded into the GPL-3 open source project_ so the **PhoenixVault** can be launched in an _appliance mode_ with zero external runtime dependencies needed for operating it. Before, when I had built the **SaaS** model with _DJ Nicke_, the OPEX for running the dozen dependencies was over $7,000 per month at the scale of 500 online users every hour. This **GPL-3 Go Rewrite** is _completely free_ and can run on [OVH](https://us.ovhcloud.com) for as low as $33/month for up to 10,000 pages of records that your _Apario Database_ contains that you used the `writer` to compile. To get started using that compiled database, run this:

```bash
cd ~/work/projectapario
git clone git@github.com:ProjectApario/reader.git
cd reader
```

Next, you'll need to compile an `~/apario/teslafiles.info/config/reader.yml` config file that is config to look like: 

```yml
---
product-name: PhoenixVault
environment: production
production-environment-label: production
database: ~/apario/teslafiles.info/app
site-title: PhoenixVault - Nikola Tesla Files
unsecure-port: 8080
secure-port: 8443
auto-tls: true
tls-company: Tesla Files
tls-domain-name: teslafiles.info
tls-san-ip: 127.0.0.1,12.34.56.78
tls-additional-domains: www.teslafiles.info
csp-domains-csv: teslafiles.info:8080,www.teslafiles.info:8080,teslafiles.info:8443,www.teslafiles.info:8443,localhost:8080,localhost:8443
company-name: Tesla Files
primary-domain: teslafiles.info
cookie-domain: teslafiles.info
```

Now, lets say that you do not want to use `auto-tls: true` and you're using `auto-tls: false`, then you'll need to use these additional properties: 

```yml
tls-public-key: ~/apario/teslafiles.info/ssl/public.key
tls-private-key: ~/apario/teslafiles.info/ssl/private.key
tls-private-key-password: Not!A_Passw0rd!Don'tUse
force-https: true
```

> **NOTE:** The `tls-public-key` expects the **Certificate Authority** to be included at the _end of the file_ so that it reads as `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` must be for the **primary domain**, then the _trust root then intermediate certificates_ in the _same file_. The permissions should be `0644` and can be set with `chmod 0644 ~/apario/teslafiles.info/ssl/public.key`.

Given this baseline configuration, you'll effectively be able to run: 

```bash
go build -o reader .
chmod +x reader
./reader -config ~/apario/teslafiles.info/config/reader.yml
```

Then the application will _compile its index_ and this can take roughly **3 to 6 seconds per page** depending on the size of the _Apario Database_. This will compile a runtime index that will make restarts of the appliance faster. The index is invalidated when the checksum of the database mismatches the cached index. When new files are added, the database index is automatically refreshed on next boot.

The binary includes a **bunch of functionality** for things like **CORS** and **Rate Limiting** and **Search Preferences** and many _many_ other functionalities. To best understand it, the first value after the `config.New<Type>(` below, in the Go code, from the reader's [config.go](https://github.com/ProjectApario/reader/blob/main/config.go) file. In your `reader.yml` file, each quoted value, like `"product-name", ` becomes in your YML file `product-name: ` where the value follows the space after the colon. This applies to each of these properties and what you can do with the [reader](https://github.com/ProjectApario/reader) is extend it beyond its default functionality that provides **out of the box majestic OSINT research &amp; search capabilities powered by [gematria](https://github.com/andreimerlescu/gematria) and [texteee](https://github.com/andreimerlescu/texteee).

Now, why is this application valuable in 2026? It's valuable because limiting a data set to 10 or 100 or even 1,000 documents and allowing the **coincidences** of what _Gematria_ actually _IS_ become _realized_ in _some form or another_ through the virtue of using the functionality it offers. On much larger data sets, its easy to get lost in the nose and see how the notion of Gematria doesn't offer much more than StumbleInto; but its still another form of StumbleInto that is isolated to much larger data sets such as the JFK Files. Both use cases, this application trio provides a novel solution for search that is **free and open source** _as promised_ and _as delivered_ and _as needed since **The Michael Trimm Show** in 2016 when I was a strong _Bernie Bro_ breaking down the numbers of how the old guy still had a chance against _the machine_. Given that we're on the brink of disclosure, knowing how this product was built can help anybody who wants to improve this product by using AI to enhance it by simply knowing how its deployed, how its run, and how it can be enhanced.

### Search &amp; Merkel

Both of these components are not published as part of the [reader](https://github.com/ProjectApario/reader) | [writer](https://github.com/ProjectApario/writer) components of Project Apario, but they are being developed and worked on to provide an _out of memory index_ of the [reader](https://github.com/ProjectApario/reader) that has less runtime requirements associated with its larger footprint caused by the textee/gematria _inefficiencies_. I can't describe them anything other than _that_, so _it is what it is and I'll just let it be._ 

My goal with the [search](https://github.com/ProjectApario/search) component was to reduce the memory footprint of the [reader](https://github.com/ProjectApario/reader) by offering an index of the data. For 100K pages, the results were kind of slow on my fast hardware. I'm on a 256GB Mac Pro 28 Core both Intel and Silicon M3 Ultra capable of AI and virtualized workloads. That's what GitHub user 91,485 has in 2026. But, do we need the **search** or the **merkel** tools at all? If the _reader_ and _writer_ are not heavily used when they have been **available for years for free** in a manner that I believed the _public wanted_ - **me out of the conversation about what I invented** but I learned that instead, if you need it, you'll use it. I built it to last for a long time. It's a self-contained appliance that has been forged over a 21+ year professional career. If these two components are needed, they'll be integrated into the **reader** and **writer** components. 

When [merkel](https://github.com/ProjectApario/merkel) is needed is when the [$APARIO DAO NFTs](https://xrp.cafe/collection/apario-dao) have been acquired and the [reader](https://github.com/ProjectApario/reader) begins distributing $APARIO tokens after completing reading &amp; quiz games on **verified OSINT that benefits society and public discourse in a healthy and authentic manner.** 
