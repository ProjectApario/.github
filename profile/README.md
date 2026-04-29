# Welcome to Project Apario

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


```go
var (
	config = configurable.New()
	// Command Line Flags
	flag_s_product_name                            = config.NewString("product-name", "apario-reader", "name of the product that is running, used as the prefix to the log file and throughout the runtime of the app; this is its self label")
	flag_s_environment                             = config.NewString("environment", "development", "environment label")
	flag_s_production_environment_label            = config.NewString("production-environment-label", "production", "default is production but useful when --environment value must be treated like production without using the label production. if you dont know how to use this flag, dont use it.")
	flag_s_database                                = config.NewString("database", "", "apario-contribution rendered database directory path")
	flag_i_sem_limiter                             = config.NewInt("limit", channel_buffer_size, "general purpose semaphore limiter")
	flag_i_sem_directories                         = config.NewInt("directories-limiter", channel_buffer_size, "concurrent directories to process out of the database (example: 369)")
	flag_i_sem_pages                               = config.NewInt("pages-limiter", channel_buffer_size, "concurrent pages to process out of the database")
	flag_i_directory_buffer                        = config.NewInt("directory-buffer", channel_buffer_size, "buffered channel size for pending directories from the database (3x --directories, example: 1107)")
	flag_i_buffer                                  = config.NewInt("buffer", reader_buffer_bytes, "Memory allocation for CSV buffer (min 168 * 1024 = 168KB)")
	flag_s_log_file                                = config.NewString("info-log", filepath.Join(".", "logs", fmt.Sprintf("%v-info-%04d-%02d-%02d-%02d-%02d-%02d.log", *flag_s_product_name, startedAt.Year(), startedAt.Month(), startedAt.Day(), startedAt.Hour(), startedAt.Minute(), startedAt.Second())), "File to save logs to. Default is logs/engine-YYYY-MM-DD-HH-MM-SS.log")
	flag_b_enable_cors                             = config.NewBool("enable-cors", true, "Enable/Disable CORS")
	flag_b_enable_csp                              = config.NewBool("enable-csp", true, "Enable/Disable CSP")
	flag_s_cors_allow_origin                       = config.NewString("cors-allow-origin", "*", "Define the header value for Access-Control-Allow-Origin")
	flag_s_cors_allow_methods                      = config.NewString("cors-allow-methods", "GET, POST, PUT, DELETE, OPTIONS", "Define the header value for Access-Control-Allow-Methods")
	flag_s_cors_allow_headers                      = config.NewString("cors-allow-headers", "Origin, Content-Type, Content-Length, Accept-Encoding, X-CSRF-Token, Authorization", "Define the header value for Access-Control-Allow-Headers")
	flag_s_cors_allow_credentials                  = config.NewBool("cors-allow-credentials", false, "Define the header value for Access-Control-Allow-Credentials")
	flag_s_csp_domains_csv                         = config.NewString("csp-domains-csv", "", "List of CSP domains in CSV format")
	flag_s_csp_thirdparty_csv                      = config.NewString("csp-thirdparty-csv", "", "List of third party domains in CSV format")
	flag_s_csp_thirdparty_styles_csv               = config.NewString("csp-thirdparty-styles-csv", "", "List of third party domains in CSV format")
	flag_s_csp_websocket_domains_csv               = config.NewString("csp-ws-domains-csv", "", "List of Web Socket domains in CSV format")
	flag_b_csp_script_enable_unsafe_inline         = config.NewBool("csp-script-unsafe-inline", true, "Enable/Disable Unsafe Inline script execution via CSP")
	flag_b_csp_script_enable_unsafe_eval           = config.NewBool("csp-script-unsafe-eval", false, "Enable/Disable Unsafe Eval script execution via CSP")
	flag_b_csp_child_src_enable_unsafe_inline      = config.NewBool("csp-child-unsafe-inline", true, "Enable/Disable Child SRC Unsafe Inline script execution via CSP")
	flag_b_csp_style_src_enable_unsafe_inline      = config.NewBool("csp-style-unsafe-inline", true, "Enable/Disable Style SRC Unsafe Inline script execution via CSP")
	flag_b_csp_upgrade_unsecure_requests           = config.NewBool("csp-upgrade-insecure", true, "Enable/Disable automagically upgrading HTTP to HTTPS for requests via CSP")
	flag_b_csp_block_mixed_content                 = config.NewBool("csp-block-mixed-content", true, "Enable/Disable automatically blocking mixed HTTP and HTTPS content for requests via CSP")
	flag_s_csp_report_uri                          = config.NewString("csp-report-uri", "/security/csp-report", "Path for content security policy violation reports to get logged")
	flag_s_config_file                             = config.NewString("config", filepath.Join(".", "config.yaml"), "Configuration file")
	flag_i_concurrent_searches                     = config.NewInt("concurrent-searches", 30, "maximum number of allowed concurrent searches before a waiting room appears")
	flag_s_search_algorithm                        = config.NewString("search-algorithm", "jaro_winkler", "values are wagner_fisher, ukkonen, jaro, jaro_winkler, soundex, hamming ; default is jaro_winkler")
	flag_i_search_concurrency_buffer               = config.NewInt("search-concurrency-buffer", 369, "buffer channel size for search results ; default = 369")
	flag_i_search_concurrency_limiter              = config.NewInt("search-concurrency-limiter", 9, "concurrent keyword processing per search query ; default = 9")
	flag_i_search_timeout_seconds                  = config.NewInt("search-timeout-seconds", 30, "maximum seconds to spend on a search")
	flag_f_search_jaro_threshold                   = config.NewFloat64("search-threshold-jaro", 0.71, "1.0 means exact match 0.0 means no match; default is 0.71")
	flag_f_search_jaro_winkler_threshold           = config.NewFloat64("search-threshold-jaro-winkler", 0.71, "using the JaroWinkler method, define the threshold that is tolerated; default is 0.71")
	flag_f_search_jaro_winkler_boost_threshold     = config.NewFloat64("search-jaro-winkler-boost-threshold", 0.7, "weight applied to common prefixes in matched strings comparing dictionary terms, page word data, and search query params")
	flag_i_search_jaro_winkler_prefix_size         = config.NewInt("search-jaro-winkler-prefix-size", 3, "length of a jarrow weighted prefix string")
	flag_i_search_ukkonen_icost                    = config.NewInt("search-ukkonen-icost", 1, "insert cost ; when adding a char to find a match ; increase the score by this number ; default = 1")
	flag_i_search_ukkonen_scost                    = config.NewInt("search-ukkonen-scost", 2, "substitution cost ; when replacing a char increase the score by this number ; default = 2")
	flag_i_search_ukkonen_dcost                    = config.NewInt("search-ukkonen-dcost", 1, "delete cost ; when removing a char to find a match ; increase the score by this number ; default = 1")
	flag_i_search_ukkonen_max_substitutions        = config.NewInt("search-ukkonen-max-substitutions", 2, "maximum number of substitutions allowed for a word to be considered a match ; higher value = lower accurate ; lower value = higher accuracy ; min = 0; default = 2")
	flag_i_search_wagner_fischer_icost             = config.NewInt("search-wagner-fischer-icost", 1, "insert cost ; when adding a char to find a match ; increase the score by this number ; default = 1")
	flag_i_search_wagner_fischer_scost             = config.NewInt("search-wagner-fischer-scost", 2, "substitution cost ; when replacing a char increase the score by this number ; default = 2")
	flag_i_search_wagner_fischer_dcost             = config.NewInt("search-wagner-fischer-dcost", 1, "delete cost ; when removing a char to find a match ; increase the score by this number ; default = 1")
	flag_i_search_wagner_fischer_max_substitutions = config.NewInt("search-wagner-fischer-max-substitutions", 2, "maximum number of substitutions allowed for a word to be considered a match ; higher value = lower accurate ; lower value = higher accuracy ; min = 0; default = 2")
	flag_i_search_hamming_max_substitutions        = config.NewInt("search-hamming-max-substitutions", 2, "maximum number of substitutions allowed for a word to be considered a match ; higher value = lower accuracy ; min = 1 ; default = 2")
	flag_s_gin_log_file                            = config.NewString("access-log", filepath.Join(".", "logs", fmt.Sprintf("%v-gin-%04d-%02d-%02d-%02d-%02d-%02d.log", *flag_s_product_name, startedAt.Year(), startedAt.Month(), startedAt.Day(), startedAt.Hour(), startedAt.Minute(), startedAt.Second())), "Default log file for GIN access logs.")
	flag_b_gin_log_to_stdout                       = config.NewBool("gin-log-stdout", true, "send gin logs to stdout")
	flag_i_webserver_default_port                  = config.NewInt("unsecure-port", 8080, "Port to start non-SSL version of application.")
	flag_i_webserver_secure_port                   = config.NewInt("secure-port", 8443, "Port to start the SSL version of the application.")
	flag_s_ssl_public_key                          = config.NewString("tls-public-key", "", "Path to the SSL certificate's public key. It expects any CA chain certificates to be concatenated at the end of this PEM formatted file.")
	flag_s_ssl_private_key                         = config.NewString("tls-private-key", "", "Path to the PEM formatted SSL certificate's private key.")
	flag_s_ssl_private_key_password                = config.NewString("tls-private-key-password", "", "If the PEM private key is encrypted with a password, provide it here.")
	flag_b_redirect_http_to_https                  = config.NewBool("force-https", false, "force-https when true will redirect any request into --unsecure-port to --secure-port using middleware")
	flag_b_auto_ssl                                = config.NewBool("auto-tls", false, "Create a self-signed certificate on the fly and use it for serving the application over SSL.")
	flag_i_reload_cert_every_minutes               = config.NewInt("tls-life-min", 72, "Lifespan of the auto generated self signed TLS certificate in minutes.")
	flag_i_auto_ssl_default_expires                = config.NewInt("tls-expires-in", 365*24, "Auto generated TLS/SSL certificates will automatically expire in hours.")
	flag_s_auto_ssl_company                        = config.NewString("tls-company", "ACME Inc.", "Auto generated TLS/SSL certificates are configured with the company name.")
	flag_s_auto_ssl_domain_name                    = config.NewString("tls-domain-name", "", "Auto generated TLS/SSL certificates will have this common name and run on this domain name.")
	flag_s_auto_ssl_san_ip                         = config.NewString("tls-san-ip", "", "Auto generated TLS/SSL certificates will have this SAN IP address attached to it in addition to its common name.")
	flag_s_auto_ssl_additional_domains             = config.NewString("tls-additional-domains", "", "Auto generated TLS/SSL certificates will be issued with these additional domains (CSV formatted).")
	flag_b_enable_tls_handshake_error_check        = config.NewBool("enable-middleware-tls-handshake-check", true, "Toggle whether to return an error on misconfigured TLS requests")
	flag_b_enable_rate_limiting                    = config.NewBool("enable-middleware-rate-limiting", true, "Toggle the tollbooth rate limiter for normal routes")
	flag_f_rate_limit                              = config.NewFloat64("rate-limit", 12.0, "Requests per second (0.5 = 1 request every 2 seconds).")
	flag_i_rate_limit_cleanup_delay                = config.NewInt("rate-limit-cleanup", 3, "Seconds between rate limit cleanups.")
	flag_i_rate_limit_entry_ttl                    = config.NewInt("rate-limit-ttl", 3, "Seconds a rate limit entry exists for before cleanup is triggered.")
	flag_b_enable_asset_rate_limiting              = config.NewBool("enable-middleware-asset-rate-limiting", true, "Toggle the tollbooth rate limiter for asset routes.")
	flag_f_asset_rate_limit                        = config.NewFloat64("rate-limit-asset", 36.0, "Requests per second (0.5 = 1 request every 2 seconds).")
	flag_i_asset_rate_limit_cleanup_delay          = config.NewInt("rate-limit-asset-cleanup", 17, "Seconds between rate limit cleanups.")
	flag_i_asset_rate_limit_entry_ttl              = config.NewInt("rate-limit-asset-ttl", 17, "Seconds a rate limit entry exists for before cleanup is triggered.")
	flag_b_enable_downloads_rate_limiting          = config.NewBool("enable-middleware-download-rate-limiting", true, "Toggle the tollbooth rate limiter for downloads routes.")
	flag_f_downloads_rate_limit                    = config.NewFloat64("rate-limit-download", 36.0, "Requests per second (0.5 = 1 request every 2 seconds).")
	flag_i_downloads_rate_limit_cleanup_delay      = config.NewInt("rate-limit-download-cleanup", 17, "Seconds between rate limit cleanups.")
	flag_i_downloads_rate_limit_entry_ttl          = config.NewInt("rate-limit-download-ttl", 17, "Seconds a rate limit entry exists for before cleanup is triggered.")
	flag_s_trusted_proxies                         = config.NewString("trusted-proxies", "", "Configure the web server to forward client IP addresses to the application if a proxy is used such as Nginx; set that proxy's IP here.")
	flag_s_robots_txt_path                         = config.NewString("robots-txt-path", "", "Relative path to override the /robots.txt entry that denies all crawlers.")
	flag_b_enable_ads_txt                          = config.NewBool("enable-ads-txt", false, "Enable the endpoint for /ads.txt to be served. Required to use --ads-txt-path.")
	flag_s_ads_txt_path                            = config.NewString("ads-txt-path", "", "Relative path to override the /ads.txt entry that helps fight fraud.")
	flag_b_enable_security_txt                     = config.NewBool("enable-security-txt", false, "Enable the endpoint for /security.txt to be served. Required to use --security-txt-path.")
	flag_s_security_txt_path                       = config.NewString("security-txt-path", "", "Relative path to override the /security.txt entry that helps fight fraud.")
	flag_b_enable_ip_ban_list                      = config.NewBool("enable-middleware-ip-ban-list", true, "Enable the middleware for ip ban list.")
	flag_i_ip_ban_list_synchronization             = config.NewInt("ip-ban-list-sync-delay", 3600, "seconds between synchronizing the ip ban list to disk")
	flag_s_ip_ban_file                             = config.NewString("ip-ban-file", filepath.Join(".", "database", "ip.db"), "File that contains JSON encoded values for the IP Ban list.")
	flag_s_hits_file                               = config.NewString("hits-file", filepath.Join(".", "database", "hits.db"), "File that contains a JSON encoded value of the hits on the site.")
	flag_i_online_refresh_delay_minutes            = config.NewInt("online-refresh-delay-minutes", 17, "seconds to count active online users before offline cut-off. 369 seconds = 6 minutes 9 seconds = default")
	flag_s_no_route_path_watch_list                = config.NewString("no-route-path-watch-list", "", "Pipe separated string of routes that should trigger an IP ban if too many are received.")
	flag_s_no_route_path_contains_watch_list       = config.NewString("no-route-path-contains-watch-list", "", "Pipe separated string of partial routes that should trigger an IP ban if too many are received.")
	flag_b_enable_ping                             = config.NewBool("enable-ping", true, "Enable the /ping endpoint of your application to return PONG.")
	flag_s_site_title                              = config.NewString("site-title", "Project Apario", "title of the application that appears on the web gui")
	flag_s_site_company                            = config.NewString("company-name", "Project Apario LLC", "name of the company that operates the service")
	flag_s_primary_domain                          = config.NewString("primary-domain", "projectapario.com", "primary domain name used to access the service")
	flag_s_number_decimal_place                    = config.NewString("decimal-symbol", ",", "symbol for decimals, default is .")
	flag_s_dark_mode_cookie                        = config.NewString("dark-mode-cookie-name", "dark-mode", "set the name of the cookie for dark mode")
	flag_b_use_cookies                             = config.NewBool("use-cookies", true, "toggle using cookies or not - cookies and sessions can be true but both cannot be false")
	flag_s_cookie_domain                           = config.NewString("cookie-domain", "localhost:8080", "domain to use for cookies")
	flag_b_use_sessions                            = config.NewBool("use-sessions", false, "toggle using sessions or not - cookies and sessions can be true but both cannot be false")
	flag_s_sessions_directory                      = config.NewString("sessions-directory", "", "absolute path of a directory that sessions can be stored")
	flag_s_session_secret                          = config.NewString("session-secret", "", "secret key used for securing sessions")
	flag_i_concurrent_image_views                  = config.NewInt("concurrent-image-views", 369, "concurrent hits to /covers/<doc-id>/<pg-id>/<size>.jpg permitted")
	flag_i_concurrent_asset_requests               = config.NewInt("concurrent-asset-requests", 369, "concurrent hits to /assets/* permitted")
	flag_i_concurrent_pdf_downloads                = config.NewInt("concurrent-pdf-downloads", 369, "concurrent pdf downloads permitted")
	flag_b_persist_runtime_database                = config.NewBool("persist-runtime-database", false, "boolean to persist the runtime database to disk once loaded")
	flag_b_load_persistent_runtime_database        = config.NewBool("load-persistent-database", false, "boolean to load a persisted database from disk to memory")
	flag_s_persistent_database_file                = config.NewString("persistent-database-path", filepath.Join(".", "database", "app.db"), "path to runtime database file")
	flag_s_flush_db_cache_watch_file               = config.NewString("flush-database-watch-file", filepath.Join(".", "flush-db.next-boot"), "name of a file to touch in the root directory to force the app to delete the database cache and regenerate at boot")
	flag_i_cache_control_database_seconds          = config.NewInt("cache-control-database-seconds", 1209600, "seconds for http header Cache-Control max-age=1209600 [default 14 days]")
	flag_i_cache_control_assets_seconds            = config.NewInt("cache-control-assets-seconds", 3600, "seconds for http header Cache-Control max-age=3600 [default 1 hour]")
	flag_s_users_database_directory                = config.NewString("users-database-path", filepath.Join(".", "database", "user.db"), "absolute path to store user directory information")
	flag_s_session_authenticity_token_secret       = config.NewString("session-authenticity-token-secret", "", "secret for session-authenticity-token that is used for aes-gcm encryption")
	flag_s_session_concurrent_crypt_actions        = config.NewInt("session-concurrent-crypt-actions-limit", 1776, "concurrent encrypt/decrypt calls permitted. change this value if performance is being impacted by excessive authentication requests.")
	flag_i_auth_max_failed_logins                  = config.NewInt("auth-max-failed-logins", 17, "maximum failed logins before the account is locked")
	flag_i_identifier_year_offset                  = config.NewInt("identifier-year-offset", 17, "+/- years from current year to look for documents in the database from when they were generated")
	flag_i_identifier_year_start_min               = config.NewInt("identifier-year-start-min", 0, "if > 0 then time.Now().Year() will be compared against this as a minimum cutoff to accept a document record")
	flag_i_identifier_year_end_max                 = config.NewInt("identifier-year-end-max", 0, "if > 0 then time.Now().Year() will be compared against this as a maximum cutoff year to accept a document record")
	flag_s_snippets_database                       = config.NewString("snippets-database-path", filepath.Join(".", "database", "snippets.db"), "absolute path to the snippets.db file for persistent snippets")
	flag_s_tag_database_path                       = config.NewString("tag-database-path", filepath.Join(".", "database", "tags.db"), "absolute path to the tags.db file for persistent storage")
	flag_i_database_concurrent_write_semaphore     = config.NewInt("database-concurrent-write-semaphore", 17000, "concurrent disk write operations permitted to acquire a lock")
	flag_i_user_identifier_length                  = config.NewInt("user_identifier_length", 6, "char length of the user identifier ; default is 6 which gives you 6^36 = 1.50094635e17 possibilities ; thats a lot!")
	flag_s_textee_database_path                    = config.NewString("textee-database", filepath.Join(".", "database", "textee.db"), "absolute path to textee database directory")
)
```

### Search &amp; Merkel

Both of these components are not published as part of the [reader](https://github.com/ProjectApario/reader) | [writer](https://github.com/ProjectApario/writer) components of Project Apario, but they are being developed and worked on to provide an _out of memory index_ of the [reader](https://github.com/ProjectApario/reader) that has less runtime requirements associated with its larger footprint caused by the textee/gematria _inefficiencies_. I can't describe them anything other than _that_, so _it is what it is and I'll just let it be._ 

My goal with the [search](https://github.com/ProjectApario/search) component was to reduce the memory footprint of the [reader](https://github.com/ProjectApario/reader) by offering an index of the data. For 100K pages, the results were kind of slow on my fast hardware. I'm on a 256GB Mac Pro 28 Core both Intel and Silicon M3 Ultra capable of AI and virtualized workloads. That's what GitHub user 91,485 has in 2026. But, do we need the **search** or the **merkel** tools at all? If the _reader_ and _writer_ are not heavily used when they have been **available for years for free** in a manner that I believed the _public wanted_ - **me out of the conversation about what I invented** but I learned that instead, if you need it, you'll use it. I built it to last for a long time. It's a self-contained appliance that has been forged over a 21+ year professional career. If these two components are needed, they'll be integrated into the **reader** and **writer** components. 

When [merkel](https://github.com/ProjectApario/merkel) is needed is when the [$APARIO DAO NFTs](https://xrp.cafe/collection/apario-dao) have been acquired and the [reader](https://github.com/ProjectApario/reader) begins distributing $APARIO tokens after completing reading &amp; quiz games on **verified OSINT that benefits society and public discourse in a healthy and authentic manner.** 

### Background Story

The whole story begins publicly when [Austin Steinbart](https://x.com/AustinSteinbart) was closely following [Operation Yokohama](https://web.archive.org/web/20220123095516/http://operation.yokohama/) that was [@TS_SCI_MAJIC12](https://x.com/TS_SCI_MAJIC12) on [X.com](https://x.com/projectapario). Many confused **Operation Yokohama** with [Q 3614](https://qalerts.app/?n=3614) thinking that I had any involvement with that _conspiracy_ - just because **Q liked my video** doesn't mean that _I am involved with Q_ in any capacity. Rather, what **Operation Yokohama** was truly about, was using a LARP and Science Fiction to undo a multi-decade conspiracy waged against me by the Sovereign known as the United States Government. A _live action role play_ was a theatre performance on Twitter acting out for people in entertainment. 

I acknowledged that what I was doing was a performance art and that its intended to be inspirational and fun to engage with, but is not financial advice or insider information. I taught a syntax of communication that looked like English that was not English in grammar. This is due to my _English As A Second Language_ phenomenon that I experienced as a result of my adoption to America. You see, December 28th 1990 I was acquired after laywers were paid $13,000 and overnight my language went from _listening to Romanian_ and **never speaking it** _AND_ being called **Andrei** to _overnight_ _listening to English_ and being called **Michael**. Not only did I have to figure out _what was being said_, but I also needed to figure out _why everybody was calling me **Michael** when that wasn't my name_. Rather, what happened to me, was my adoptee identity became culturally erased by the Americans who sought to erase Romania and the heritage that I belong to. A branch was cut from the vine and then grafted into America, and then the branch realized that it belonged to a tree at one point; but what happened to _that tree_? Truly, **The Dreaming Tree** _has died_ [🎶](https://music.apple.com/us/album/the-dreaming-tree/299546495?i=299546574). 

Truly when _Boyd Tinsley_ followed me on Twitter, before I lost my account, my heart was jumping for joy doing laps. I also received likes from [@JK_Rowling](https://x.com/jk_rowling?s=21) that were lost when the **PhoenixVault** was _suspended 17 days after its launch_. What is hilarious is **Gary Lake-Schaal** at [WB Games](https://github.com/andreimerlescu/andreimerlescu/blob/main/FULLTIME.md#video-game-credits) never knew that I built **The PhoenixVault** in front of _DJ Nicke_ and that I survived Ceaușescu's orphanages **and I live with a neurological motor disorder inflicted by the orphanage.** What I survived were the [Gregory Keck](https://github.com/andreimerlescu/andreimerlescu/blob/709ad35d53ea9a7a7e96b9b97e1921b70421de05/004-michael-joins-road-to-evergreen.md) identity splitting training materials and I rocked my head because I knew that _it was going to be used against me_. How, it was used against me, was by harming others and distributing the tapes to students under the guise of **this is educational** and _this is good for **him to experience** when in reality **it was harmful**_. 

While I single-handedly built a **Software As A Service** solution built in **Ruby on Rails** called **The PhoenixVault**, it cost me over $7,000 per month to operate and had around a dozen dependencies that involved significant DevOps code in order to automate builds and stability into the pipeline itself. While I was working full time for **WB Games**, during the holiday break in 2022, I built the [writer](https://github.com/PhoenixVault/writer) over 11 days while I was given time off for Christmas, _the only vacation days of the year granted to me aside from Federal Holidays_.  

This rough draft took what I built in `ruby` and enhanced it with Go using concurrency. I reduced the _operational load_ from **90-180 seconds per page** to under **9 seconds per page** by using the native Go concurrency model of the channels as the project demonstrates. When aggregrating collections that are 666K pages of records released by the National Archives and Records Administration (NARA) that were flattened PDF/Images that were unsearchable because the OCR (optical character recognition) had been stripped out of it prior to release. Now, it appears that properly redact documents anymore is to difficult for their agency. I was being denied health insurance from **WB Games** despite my disability and I was _asking for reasonable accommodations_ while my manager was calling me a _dirty contractor_ because I was refusing a $30K/year pay-cut so I could call myself full time. However, when my first contract ended and I was rewned for a second time, they offered me employment once again, and I turned the lowball number down again. In between contracts, I was unpaid for an entire month and during that time I built the [reader](https://github.com/andreimerlescu/reader). 

This, of course, all caused me to take the Project Apario and PhoenixVault offline in order to release the **GPL-3** Oen Source [reader](https://github.com/andreimerlescu/reader) [writer](https://github.com/andreimerlescu/writer) [search](https://github.com/andreimerlescu/search) and [merkel](https://github.com/andreimerlescu/merkel). Ultimately, my cashflow stopped and the project became free and open source. The incentive to _paying me anything_ stopped for the _OSINT community_ and it also _stopped with WB Games_ because the attitude of _dirty contractor_ wasn't exclusive to just that one manager, which is why I didn't make a stink about it. All the management was doing it. Because I was a contractor. It felt like _hazing_ in college. Behavior that I **did not tolerate** when I was running the IEEE Student Branch at [Wentworth](https://wit.edu) from 2007-2010. I joined [GitHub](https://github.com/miketrimm) user 91,485 in 2009 when I joined **Cisco Systems** and worked there for 7 years in-and-our of full time positions as H-1B replacements were being systmatically installed and the diversity that I witnessed that had orphanage survivors like myself and other wunderkind miracles of engineering gathered in one space now has become a **monoculture that is relying on AI to diversify its output and throughput that true diversity offers natively**. 

So, the story began then, and there, and _what I was doing then_ has concluded and this repository **is the result** of those seeds sown into me. You see, in 2016, a decade ago, just 7 years after I joined GitHub as user 91,485 while running the IEEE Student Branch at Wentworth, running laps against classmates who would later one day be my boss and use their position of power to inflict bodily injury onto me, like what happened to me at [Beamable](https://github.com/andreimerlescu/andreimerlescu/blob/main/DISABILITY.md). 

So, to get to **The PhoenixVault**, this name was inspired by **Austin Steinbart** and **DJ Nicke** and I rebranded by 2019 project called _Crowd Sorucing Declass_ and _rebuilt it from the ground up using `rails new phoenixvault` with DJ on the Zoom call with me_.

The original thing that DJ saw me build over 17 days in 2020 during the lockdowns of the pandemic were re-engineered by me from the ground up so that the operational expenses could be managed by DJ himself if he wanted to run PhoenixVault for his own work and rebrand it with the help of AI. Fork the repository, change the images, and update the templates and voila you have your own PhoenixVault. AI can even do a lot of heavy lifting for you if you know how to ask it for help. More than likely though, it'll end up breaking things in the application because AI doesn't _understand anything_. It's not **alive** and it doesn't have feelings. It's a text analysis tool that is attempting to autocomplete the next character for you without knowing _anything_. It has _only what it was trained on and what its tools can hand it_. It's a tool. What I built, took a human to forge. I forged it. By hand. And yes, now Austin can run a copy of it himself without the OPEX overhead and trade secrets that he wasn't privy to knowing.

The rewrite was done in two stages. Both taking 11 days each. The first was the [writer](https://github.com/ProjectApario/writer) and the second was the [reader](https://github.com/ProjectApario/reader). Together, these took me 22 days to build from start to finish without AI. These bursts of inspiration come in between employment opportunities and other life commitments that I have when I am not serving others.

### Giving to Project Apario

https://ko-fi.com/ProjectApario can accept donations if you're interested in Giving to Project Apario, or you can Sponsor [@andreimerlescu](https://github.com/andreimerlescu). Either option allows you to continue sowing seeds into the _survivor_, _visionary_, _inventor_, _father_ and _builder_ that you called _Michael_ for many years until I said **enough is enough** and I restored myself as **Andrei** the man who God created me, not the man who the _fake news media said to you that's what I called myself_. I _learned that_ over years of being trained _like a dog_ until I was performing _bird shows_ for approval. Those days have been handed off to memory lane and as _Andrei_, I felt that my audience only knew me as Michael Trimm, before my transition, and now, as Andrei, I have very limited feedback given how isolated everything has become given the Twitter days being dismantled by God knows how many bad faith actors who weren't trying to pursue **truth** and **knowledge** but instead were trying to _dominate_ and _control_ and use _blackmail_ on people that they _couldn't control_. Not only am I by the book, but I am by the book, and I love writing books as you can see. Streams of consciousness are easier for me to write down, but this piece opened as a tutorial for you to run **Project Apario**'s trio yourself. The [search](https://github.com/ProjectApario/search) is the same way in that it needs a `search.yml` provided to it and the [keys.go](https://github.com/ProjectApario/search/blob/master/keys.go) file has all of the `property-name: ` that you'd define in your `search.yml` config file in `~/apario/teslafiles.info/config`.

As you can tell, I am not about the money money money and I build without expecting anything in return. The **seeds sown into me** have been in the **tens of thousands of dollars** over the last **decade** and I am eternally grateful to every single one of you for the _faith that you extended to me!_

### Project Apario in 2027

This summer, a lot of things can happen for Project Apario that prepares it for a massive 2027 re-launch as PhoenixVault, and ultimately, I am inviting you to begin participating and sowing seeds back into the project. It's time that this goes mainstream now that the foundation has been set and the product has been battle tested for **many years now in production** with _more stability than Claude_ and _its just been me for years doing this by myself._ When the [$APARIO DAO NFTs](https://xrp.cafe/collection/apario-dao) sell, that will provide me some capital to legitimately work with in order to reboot the decentralized crypto-backed idea which is the logical next step that **The PhoenixVault** _must take_. This summer is the 250 year birthday celebration of America and the **Phoenix** rises from [the ashes](https://github.com/andreimerlescu/andreimerlescu/blob/main/DISABILITY.md).


