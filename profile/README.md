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

```bash
mkdir -p ~/work/projectapario
cd ~/work/projectapario
git clone git@github.com:ProjectApario/writer.git
cd writer
```

Then install it: 

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
