# Haven Protocol Gateway for WooCommerce

## Features

* Payment validation done through `haven-wallet-rpc` 
* Validates payments with `cron`, so does not require users to stay on the order confirmation page for their order to validate.
* Order status updates are done through AJAX instead of Javascript page reloads.
* Customers can pay with multiple transactions and are notified as soon as transactions hit the mempool.
* Configurable block confirmations, from `0` for zero confirm to `60` for high ticket purchases.
* Live price updates every minute; total amount due is locked in after the order is placed for a configurable amount of time (default 60 minutes) so the price does not change after order has been made.
* Hooks into emails, order confirmation page, customer order history page, and admin order details page.
* View all payments received to your wallet with links to the blockchain explorer and associated orders.
* Optionally display all prices on your store in terms of Monero.
* Shortcodes! Display exchange rates in numerous currencies.

## Requirements

* Haven wallet to receive payments - Wallets

MacOS Desktop: https://github.com/haven-protocol-org/haven-web-app/releases/download/v1.0.2/Haven-1.0.2.dmg

Windows Desktop: https://github.com/haven-protocol-org/haven-web-app/releases/download/v1.0.2/Haven-1.0.2.Setup.exe

MacOS CLI: https://docs.havenprotocol.org/Haven-1.0.2-CLI_MacOS-10.14-and-later.zip

Windows CLI: https://docs.havenprotocol.org/Haven-1.0.2_CLI_Windows-x64.zip

Linux CLI: https://docs.havenprotocol.org/Haven-1.0.2-CLI-Linux-x64-glibc-2-29.zip

* [BCMath](http://php.net/manual/en/book.bc.php) - A PHP extension used for arbitrary precision maths

## Installing the plugin

* Download the plugin from the [releases page](https://github.com/monero-integrations/monerowp) or clone with `git clone https://github.com/monero-integrations/monerowp`
* Unzip or place the `monero-woocommerce-gateway` folder in the `wp-content/plugins` directory.
* Activate "Monero Woocommerce Gateway" in your WordPress admin dashboard.
* It is highly recommended that you use native cronjobs instead of WordPress's "Poor Man's Cron" by adding `define('DISABLE_WP_CRON', true);` into your `wp-config.php` file and adding `* * * * * wget -q -O - https://yourstore.com/wp-cron.php?doing_wp_cron >/dev/null 2>&1` to your crontab.

The most secure way to accept Haven on your website. You'll need:

* Root access to your webserver
* Latest [daemon] [CLI]

After downloading (or compiling) the Haven binaries on your server, install the [systemd unit files](https://github.com/monero-integrations/monerowp/tree/master/assets/systemd-unit-files) or run `havend` and `haven-wallet-rpc` with `screen` or `tmux`. You can skip running `havend` by using a remote node with `haven-wallet-rpc` by adding `--daemon-address remote.haven.miner.rocks: port 17750 (2)nodes.hashvault.pro: port 17750 (3) Http://xhv-pst.minershive.ca: port 17750` to the `haven-wallet-rpc.service` file.

Note on security: using this option, while the most secure, requires you to run the Monero wallet RPC program on your server. Best practice for this is to use a view-only wallet since otherwise your server would be running a hot-wallet and a security breach could allow hackers to empty your funds.

## Configuration

* `Enable / Disable` - Turn on or off Haven gateway. (Default: Disable)
* `Title` - Name of the payment gateway as displayed to the customer. (Default: Haven Gateway)
* `Discount for using Haven` - Percentage discount applied to orders for paying with Haven. Can also be negative to apply a surcharge. (Default: 0)
* `Order valid time` - Number of seconds after order is placed that the transaction must be seen in the mempool. (Default: 3600 [1 hour])
* `Number of confirmations` - Number of confirmations the transaction must recieve before the order is marked as complete. Use `0` for nearly instant confirmation. (Default: 5)
* `Confirmation Type` - Confirm transactions with either your viewkey, or by using `haven-wallet-rpc`. (Default: viewkey)
* `Haven Address` (if confirmation type is viewkey) - Your public Haven address starting with 4. (No default)
* `Secret Viewkey` (if confirmation type is viewkey) - Your *private* viewkey (No default)
* `Haven wallet RPC Host/IP` (if confirmation type is `monero-wallet-rpc`) - IP address where the wallet rpc is running. It is highly discouraged to run the wallet anywhere other than the local server! (Default: 127.0.0.1)
* `Haven wallet RPC port` (if confirmation type is `haven-wallet-rpc`) - Port the wallet rpc is bound to with the `--rpc-bind-port` argument. (Default 18080)
* `Testnet` - Check this to change the blockchain explorer links to the testnet explorer. (Default: unchecked)
* `SSL warnings` - Check this to silence SSL warnings. (Default: unchecked)
* `Show QR Code` - Show payment QR codes. (Default: unchecked)
* `Show Prices in Haven` - Convert all prices on the frontend to Haven. Experimental feature, only use if you do not accept any other payment option. (Default: unchecked)
* `Display Decimals` (if show prices in Monero is enabled) - Number of decimals to round prices to on the frontend. The final order amount will not be rounded and will be displayed down to the nanoMonero. (Default: 12)

## Shortcodes

This plugin makes available two shortcodes that you can use in your theme.

#### Live price shortcode

This will display the price of Haven in the selected currency. If no currency is provided, the store's default currency will be used.

```
[haven-price]
[haven-price currency="BTC"]
[haven-price currency="USD"]
[haven-price currency="CAD"]
[haven-price currency="EUR"]
[haven-price currency="GBP"]
```
Will display:
```
1 XHV = 123.68000 USD
1 XHV = 0.01827000 BTC
1 XHV = 123.68000 USD
1 XHV = 168.43000 CAD
1 XHV = 105.54000 EUR
1 XHV = 94.84000 GBP
```


#### Haven accepted here badge

This will display a badge showing that you accept Haven-currency.

`[haven-accepted-here]`

![Haven Accepted Here](/assets/images/monero-accepted-here.png?raw=true "Monero Accepted Here") [new image needed]

## Donations

XHV : hvxxvYDegzr27W9WQ7MeGf6sj7xUM8QqujkmWP5zDkcDe1aDrswDFdNTgG1oZNYprqPF1czdixj58a2kaAVZ59Gv2tFpdvubSZ

XLA : Se37n8eHBMuXBasEYTEmqfaNSNZ2EnJV3F9FCwuUm6Efe4y8pCxgLapWCUWxDrxkDJMku1sFXEQafV8QSYkhJqkv1dPCuZPzf
