
## Go Currency Exchange Library [WIP]

[![Build Status](https://travis-ci.org/me-io/memcached-util.svg?branch=master)](https://travis-ci.org/me-io/memcached-util)
[![Go Report Card](https://goreportcard.com/badge/github.com/me-io/memcached-util)](https://goreportcard.com/report/github.com/me-io/memcached-util)
[![Coverage Status](https://coveralls.io/repos/github/me-io/memcached-util/badge.svg?branch=master)](https://coveralls.io/github/me-io/memcached-util?branch=master)
[![GoDoc](https://godoc.org/github.com/me-io/memcached-util?status.svg)](https://godoc.org/github.com/me-io/memcached-util)
[![GitHub release](https://img.shields.io/github/release/me-io/memcached-util.svg)](https://github.com/me-io/memcached-util/releases)


[![](https://images.microbadger.com/badges/version/meio/memcached-util.svg)](https://microbadger.com/images/meio/memcached-util)
[![COMMIT](https://images.microbadger.com/badges/commit/meio/memcached-util.svg)](https://microbadger.com/images/meio/memcached-util)
[![SIZE-LAYERS](https://images.microbadger.com/badges/image/meio/memcached-util.svg)](https://microbadger.com/images/meio/memcached-util)
[![Pulls](https://shields.beevelop.com/docker/pulls/meio/memcached-util.svg?style=flat-square)](https://hub.docker.com/r/meio/memcached-util)

Swap allows you to retrieve currency exchange rates from various services such as **[Google](https://google.com)**, **[Yahoo](https://yahoo.com)**, **[Fixer](https://fixer.io)**, **[CurrencyLayer](https://currencylayer.com)** or **[1Forge](https://1forge.com)** 
and optionally cache the results. 

## Playground
<a href="https://memcached-util.herokuapp.com/swagger" target="_blank">
  <img height="64" src="https://image.ibb.co/ehsqGp/swagger_ui.jpg" alt="Swagger UI">
</a> 
<a href="https://memcached-util.herokuapp.com" target="_blank">
    <img height="64" src="https://image.ibb.co/hvWT2U/go_swap_server_heroku.png" alt="heroku test instance @ https://memcached-util.herokuapp.com">
</a>

#### /GET Examples for single exchanger:
- [GET /convert?from=USD&to=AED&amount=2&exchanger=yahoo](https://memcached-util.herokuapp.com/convert?from=USD&to=AED&amount=100&exchanger=yahoo) 
- [GET /convert?from=EUR&to=GBP&amount=1&exchanger=google](https://memcached-util.herokuapp.com/convert?from=EUR&to=GBP&amount=100&exchanger=google)
- [GET /convert?from=USD&to=SAR&amount=1&exchanger=themoneyconverter](https://memcached-util.herokuapp.com/convert?from=USD&to=SAR&amount=100&exchanger=themoneyconverter)

#### /POST Examples for single or multi exchanger:
- CURL examples:
    ```bash
    curl -X POST \
      https://memcached-util.herokuapp.com/convert \
      -H 'Content-Type: application/json' \
      -d '{
      "amount": 2.5,
      "from": "USD",
      "to": "AED",
      "decimalPoints": 4,
      "cacheTime": "120s",
      "exchanger": [
        {
          "name": "yahoo"
        },
        {
          "name": "google"
        },
        {
          "name": "themoneyconverter",
          "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.8; rv:21.0) Gecko/20100101 Firefox/21.0"
        }
      ]
    }'
  
     # Response example
     # {
     #    "to": "AED",
     #    "from": "USD",
     #    "exchangerName": "yahoo",
     #    "exchangeValue": 3.6721,
     #    "originalAmount": 2.5,
     #    "convertedAmount": 9.1802,
     #    "convertedText": "2.5 USD is worth 9.1802 AED",
     #    "rateDateTime": "2018-09-30T07:45:45Z",
     #    "rateFromCache": false
     # }  
    ```
- [Run in SwaggerUI](https://memcached-util.herokuapp.com/swagger)
- [![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/e721b5007e2df03f1cac)

## QuickStart 

<a href="https://heroku.com/deploy?template=https://github.com/me-io/memcached-util" target="_blank">
  <img src="https://www.herokucdn.com/deploy/button.svg" alt="Deploy">
</a>
<br>



```bash
# Or using docker  
$ docker pull meio/memcached-util:latest && \
  docker run --rm --name memcached-util -p 5000:5000 -it meio/memcached-util:latest
```

### Programmatically
```bash
$ go get github.com/me-io/memcached-util
```

```go
package main

import (
	"fmt"
	ex "github.com/me-io/memcached-util/pkg/exchanger"
	"github.com/me-io/memcached-util/pkg/swap"
)

func main() {
	SwapTest := swap.NewSwap()

	SwapTest.
		AddExchanger(ex.NewGoogleApi(nil)).
		Build()

	euroToUsdRate := SwapTest.Latest("EUR/USD")
	fmt.Println(euroToUsdRate.GetRateValue())
}

```

## Features
- Convert with Single exchange source `/GET` 
- Convert with Multi exchange sources with fallback mechanism `/POST`
    - Google
    - Yahoo
    - CurrencyLayer
    - Fixer.io
    - themoneyconverter.com
    - openexchangerates.org
    - 1forge.com
- Rate Caching - `120s Default` 
    - Memory - `Default`
    - Redis
- Rate decimal points rounding `4 Default`
- Swagger UI
- Clear API Request and Response
- Docker image, Binary release and Heroku Demo
- Clear documentation and 90%+ code coverage
- Unit tested on live and mock data

## Documentation
The documentation for the current branch can be found [here](#documentation).


## Services
|Exchanger                  |type           |#                  |$|
|:---                       |:----          |:---               |:---|
|[Google][1]                |HTML / Regex   |:heavy_check_mark: |Free|
|[Yahoo][2]                 |JSON / API     |:heavy_check_mark: |Free|
|[Currency Layer][3]        |JSON / API     |:heavy_check_mark: |Paid - ApiKey|
|[Fixer.io][4]              |JSON / API     |:heavy_check_mark: |Paid - ApiKey|
|[1forge][7]                |API            |:heavy_check_mark: |Freemium / Paid - ApiKey|
|[The Money Converter][5]   |HTML / Regex   |:heavy_check_mark: |Free|
|[Open Exchange Rates][6]   |API            |:heavy_check_mark: |Freemium / Paid - ApiKey|

[1]: //google.com
[2]: //yahoo.com
[3]: //currencylayer.com
[4]: //fixer.io
[5]: //themoneyconverter.com
[6]: //openexchangerates.org
[7]: //1forge.com

## TODO LIST
- [ ] error structure incase of json return "" or regex not matched
- [ ] convert panic to api json error
- [ ] increase tests
- [ ] goreleaser
- [ ] verbose logging
- [ ] godoc 
- [ ] static bundle public folder `./cmd/server/public`
- [ ] v 1.0.0 release ( docker / binary github / homebrew mac )
- [ ] support historical rates if possible
- [ ] benchmark & performance optimization ` memory leak`
- [ ] contributors list 

## Contributing

Anyone is welcome to [contribute](CONTRIBUTING.md), however, if you decide to get involved, please take a moment to review the guidelines:

* [Only one feature or change per pull request](CONTRIBUTING.md#only-one-feature-or-change-per-pull-request)
* [Write meaningful commit messages](CONTRIBUTING.md#write-meaningful-commit-messages)
* [Follow the existing coding standards](CONTRIBUTING.md#follow-the-existing-coding-standards)

#### Credits
> Inspired by [florianv/swap](https://github.com/florianv/swap) 

## License

The code is available under the [MIT license](LICENSE.md).
