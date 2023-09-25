
# osmosis-cl-bot
Bot to manage supercharged positions on Osmosis.

Features: (Work in progress)

Here's one of my bots for proof:  (Work in progress)

## Table of Contents
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Setup](#setup)
- [Usage](#usage)
- [Disclaimer](#disclaimer)
- [TODO](#todo)

## Request code access
- This bot took me a while to create and therefore the code is private. [DM me for a price if you are serious.](https://linktr.ee/jasbanza)
- It has an MIT license, so you're free to do what you like with the code once you have it.
- I might not continue development. See the [disclaimer](#disclaimer)


## Prerequisites:



1. A secure Linux environment. WSL will also work.
2. Install nodejs and npm package manager:


    ```bash
    # Update the package list
    sudo apt update

    # Install Node.js and npm
    sudo apt install nodejs npm

    ```

    *After the installation is complete, you can verify that Node.js and npm were installed correctly by checking their versions:*

    ```bash
    node -v
    npm -v
    ```

3. [Install the Osmosis command line](https://get.osmosis.zone/)
  

## Installation:

1. ~~Fork the repo:~~
    This repo is now private. If you want the code, [DM me for a price if you are serious.](https://linktr.ee/jasbanza)
    ```bash
    git clone https://github.com/jasbanza/async-retry-handler.git
    ```

2. Install necessary dependancies by running the following in the forked directory:

    ```bash
    npm install
    ```


## Setup:
1. Create a new Osmosis account in Keplr
2. Fund the wallet initially & set up an initial Supercharged position if you want to test. Make sure there is > 1 $OSMO for gas.
3. Set up the bot account in osmosisd with the following command. Enter the wallet's seed phrase when prompted.
    ```bash
    osmosisd keys add bot --recover --keyring-backend "test"
    ```
    ⚠️ *We use `--keyring-backend "test"` to eliminate the need to use a passphrase when doing transactions.*

4. Copy `./config/config.template.js` to `./config/config.js` and update the following settings:

   ```js
   export default {
    OSMOSISD_ACCOUNT_NAME: "bot", // bot's name in osmosisd
    OSMOSISD_ACCOUNT_ADDRESS: "osmo1YourBotWalletAddress69420", // bot's osmosis wallet
    HOST: "localhost", // for remote monitoring.
    PORT: 9009, // for remote monitoring. 
   };

   ```

## Usage:

- In your console, run the following command:
    
    ```bash
    npm run start
    ```
- You can monitor from the console or via your deployment's *`port 9009`* in your web browser.

- It's recommended to set this up as a systemd service using `osmosis-cl-bot.service` but that is beyond the scope of this guide...
*(Just remember your nodejs path should be absolute not relative.)*
<br><br>

# Disclaimer:

**Important: Please read this disclaimer carefully before using the osmosis-cl-bot (the "Bot").**

The osmosis-cl-bot is an open-source software application developed by [jasbanza](https://linktr.ee/jasbanza) ("the Developer"). By using this Bot, you agree to the following terms:

1. **No Financial Advice:** The Bot is intended for informational and educational purposes only and should not be considered financial, investment, or trading advice. The Developer is not a financial advisor, and any information provided by the Bot should not be construed as financial advice.

2. **Use at Your Own Risk:** Use of the Bot is at your own risk. The Developer makes no representations or warranties regarding the accuracy, reliability, or completeness of the information provided by the Bot. You acknowledge that cryptocurrency and blockchain-related activities involve significant risks, and you should exercise caution and conduct your own research before making any decisions.

3. **No Guarantees:** The Developer does not guarantee the performance or results of the Bot. The Bot may not always operate as expected, and there may be bugs or errors. The Developer is not responsible for any losses, damages, or expenses that may result from the use of the Bot.

4. **No Liability:** To the fullest extent permitted by law, the Developer disclaims any liability for any direct, indirect, incidental, consequential, or special damages, including but not limited to, loss of funds, profits, or data, arising out of or in connection with the use of the Bot.

5. **Third-Party Services:** The Bot may interact with third-party services, including but not limited to cryptocurrency exchanges and blockchain networks. The Developer is not responsible for the actions or performance of these third-party services. You should review and comply with the terms and policies of any third-party services you use in conjunction with the Bot.

6. **Updates and Changes:** The Developer may update, modify, or discontinue the Bot at any time without prior notice. You are responsible for ensuring that you have the latest version of the Bot.

7. **Compliance:** You are responsible for complying with all applicable laws and regulations in your jurisdiction when using the Bot. The Developer is not responsible for any legal consequences resulting from your use of the Bot.

By using the Bot, you acknowledge and accept these terms and disclaimers. If you do not agree with these terms, do not use the Bot.

Please use the Bot responsibly and consider seeking advice from qualified professionals before making any financial or investment decisions.

[jasbanza](https://linktr.ee/jasbanza) DISCLAIMS ALL LIABILITY FOR ANY DAMAGES OR LOSSES ARISING OUT OF OR IN CONNECTION WITH THE USE OF THE BOT.

[jasbanza](https://linktr.ee/jasbanza)<br>
26 September 2023


<br><br><br>

# TODO:

## Common functions needed

- [x] execute shell scripts (osmosisd)
- [x] handle stdin/stdout
- [x] handle errors

## core functionality for bot

#### CONTINUOUS POSITION POLLING:
- [x] get current tick from pool
- [x] get positions ranges
- [x] check if position is within range
- [ ] if out of range, REBALANCE

#### CONTINUOUS REWARD POLLING:
- [ ] check rewards. if past threshold, **ADD TO POSITION**
- [ ] if multiple positions exist, CHECK APRS add to the more lucritive one.

#### CHECK APRS
- [ ] return APRs.

#### REBALANCE:
- [ ] **REMOVE POSITION**
- [ ] **DETERMINE BEST POSITION** and **CREATE NEW POSITION** 
- [ ] **AUTO SWAP** in order to add to position
- [ ] **ADD TO POSITION** 

#### DETERMINE BEST POSITION
- [ ] what is the current tightest tick range for current price...
- [ ] OR do we want this to be 2 positions, e.g. 10% of position total should be a few ticks wider than the tightest tick to account for highly volatile pools? (in case the polling isn't quick enough)

#### CREATE NEW POSITION
- [ ] **check wallet balance**
- [ ] create position.

#### COMPOUND:
- [ ] **CLAIM REWARDS**
- [ ] **AUTO SWAP**
- [ ] **ADD TO POSITION**

#### ADD TO POSITION:
- [ ] **check wallet balance**
- [ ] **get current pool ratio**
- [ ] **auto swap** based on ratio
- [ ] **add to position** (max of both tokens) set min added = 0
- [ ] **repeat** until both tokens are fully in pool.
- [ ] Perhaps set a **reserve limit** so that there's no infinite loops. e.g. 0.1 of each token should not be swapped.

#### AUTO SWAP
- [ ] async function **given an estimated ratio, swaps assets**.

#### GRACEFUL HANDLING OF ALL TRANSACTIONS:
- [ ] Sometimes there's a **timeout**, so **check** balance after a few seconds, to see if there was any change. (tx type dependant)
- [ ] If there wasn't a balance change after 5 checks, look for the latest tx **on chain** for **errors**.
- [ ] If it was **gas related, increase gas**.
- [ ] Perhaps add this newly adjusted **gas setting to the bot config**

#### LOG TO DB
- [ ] Log all actions / position performance to database, so that we quickly determine performance and any issues.
