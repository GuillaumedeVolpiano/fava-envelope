# fava-envelope

A beancount fava extension to add a envelope budgeting capability to fava and beancount. It is developed as an fava plugin and CLI.

![PyPI](https://img.shields.io/pypi/v/fava-envelope?color=success&label=pypi%20package)
![GitHub last commit](https://img.shields.io/github/last-commit/bryall/fava-envelope)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Run on Repl.it](https://repl.it/badge/github/bryall/fava-envelope)](https://repl.it/github/bryall/fava-envelope)

## Repl.it
Click the repl.it link to be able to see the plugin in action
1. Click the link
2. Click Run
3. When the web pane opens, click the open in new tab ( have not figurec ouf why it is not showing in the initial window )
4. Click the "Fava Envelope" link in fava to see the plugin

## Installation via pip
```
python -m pip install fava-envelope
```

## TODO

* add example file for screenshots and testing
* Add testing
* add charts

## Running fava-envelope

## Load the Extension
Add this to your beancount journal, and start fava as normal
```
2000-01-01 custom "fava-extension" "fava_envelope" "{}"
```

You should now see 'Envelope' in your fava window. You must set up a budget (see below), or else Fava will report a 404 error.

## Setting up budget

### Set the budget start date
start date in the format <4 digit year>-<2 digit month>
```
2020-01-01 custom "envelope" "start date" "2020-01"
```

## Set up Budget Accounts
You will need to specify the Assets and Liabilities you want included in your budget (For example ignoring Investment accounts). you can use regular expression in these statements
```
2020-01-01 custom "envelope" "budget account" "Assets:Checking"
2020-01-01 custom "envelope" "budget account" "Liabilities:Credit-Cards:*"
```

### Set up mappings
By default fava-envelope will use the Assets/Liabilities/Income/Expenses buckets that are not listed in the budget accounts. this directive allows you to map them to another bucket
```
2020-01-01 custom "envelope" "mapping" "Expenses:Food:*" "Expenses:Food"
```

### Allocate money to a bucket
```
2020-01-31 custom "envelope" "allocate" "Expenses:Food" 100.00
```

### Set up operating currency
The envelopes will read the operating currency from the core beancount option (using the first one if multiple operating currencies are set.
```
option "operating_currency" "EUR"
```
It will default to USD if this option is not set. Only a single currency is supported for a given budget. 

The operating currency of the budget can now also be set as an option.
```
2020-01-31 custom "envelope" "currency" "EUR"
```

### Multiple budgets in multiple currency
To set multiple budgets in multiple currencies, define the currencies as a list in the extension options.
```
2000-01-01 custom "fava-extension" "fava_envelope" "{'currencies':['GBP', 'USD']}"
```
Then each envelope directive has to be postponed by the currency to identify which budget it refers to.
```
2020-01-01 custom "envelopeGBP" "budget account" "Assets:Checking"
2020-01-31 custom "envelopeUSD" "allocate" "Expenses:Food" 100.00
```


For multiple budgets, an account can be set to be treated as an income account, so that operations from this account will be treated as income for envelope budgeting purpose (only).
```
2020-01-31 "envelopeGBP" "income account" "Assets:USD:MyBank"
```