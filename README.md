lz-script
Practically an AIO script for LayerZero.

The script has been prepared for informational purposes only.
This script was developed for educational purposes only.

Changes v1.1:

I added the MIN_STABLE_FOUND variable to the main_config.py file so that the script does not pick up all sorts of small things from wallets, but works only with the number of stablecoins that you need. Example: There are 2 USDT and 0.11 USDC in the wallet. If you set MIN_STABLE_FOUND = 1.0, the script will only work with USDT.
We removed single-nets, made everything basic in stargate so that people don't get confused. Network settings are made in the main_config.py file and in stargate_._settings.py
Optimized some inaccuracies.
I removed USDT from the Arbitrum network, as there were several problems with it during swaps.
As soon as they add support for stablecoins in the zkSync and zkEVM networks, I immediately add them to the script.
What the script can do:
Bridge ALL(!!) available USDT/USDC from any network to any network (except ETH, FTM (due to deactivation of pools), Metis)
Buy and stake STG in POLYGON
Buy BTC in AVALANCHE, bridge it via btcbridge to POLYGON, bridge back and sell it
Swap ETH to ARBITRUM or OPTIMISM to GoerliETH via https://testnetbridge.com/, one-way
Bridge USDT to Aptos via https://theaptosbridge.com/bridge, one-way
Bridge USDT to Harmony through https://bridge.harmony.one/erc20, one-way
Dilute transaction activity with approvals on different networks
Dilute transaction activity with swaps on different networks
Work in a multi-thread
Choose random wallets and perform activities in a random order
Record general logs of all actions and separately by wallets
Preparation for use
Networks in the script are divided into single and non-single. Single networks - ARBITRUM, BSC, OPTIMISM - networks from which you can bridge, and B (to) cannot. It is done to save money, because the bridge to these networks is ~2 times more expensive than from these networks. Non-single networks - POLYGON, AVALANCHE - main networks.

If you have no experience in programming, re-read the instructions - do the initial setup, and do not change other code.
We also strongly recommend that you first run 2-3 purses to understand the logic of the script

main_config.py
WAIT_TIME = range(5, 10) - delay between actions, in minutes
- the percentage of the USDT/USDC balance that will be used for the bridge from single networks (1 = 100%, 0.5 = 50%)
- list of non-single networks. You can add your own (which are presented on Stargate, of course). If you don't want to dig into the code - skip, because you would have to write a separate guide to describe the process of adding.RATIO_STARGATE_SINGLE = 0.5STARGATE_CHAIN_LIST

max_setting.csv
Table with commission settings (in dollars).
- Activity
- Maximum Transaction Fee (Gas) - Maximum Stargate Value (see screenshot below)
ActivityMAX_GASMAX_VALUE



RPCs.py
Fill in your own or the public of the Russian Orthodox Church.

Free private ROCs:

https://www.alchemy.com/ (no problems with them)
https://www.quicknode.com/
https://accounts.lavanet.xyz/ (sometimes fell off)
stargate_settings.py
Optional!
Here you can change the routes of the bridges.

Let's take ARBITRUM as an example:
POLYGON and AVALANCHE are specified. If we want to add the ability to bridge from ARBITRUM to OPTIMISM, we add it on a new line.chain_to'OPTIMISM',

data.csv
The heart of the script! Pay special attention to filling out this table. All decimal numbers (17.89) have a period (".") instead of a comma.

DO - whether it is necessary to chase away this wallet. In order for the script to start running it, you need to write an English "X". Without the "X", it doesn't matter what activities are chosen next. After the end of the run of all selected wallet activities, "X" will change to "DONE".
Name - Wallet name (required for logs).
Wallet - wallet address in the EVM network.
Private_key - Private key to the wallet.
Proxy - Proxy field. As long as it doesn't work, leave it empty.
Stargate - log - the total number of bridges via Stargate after running the script, you don't need to change anything, you need it for reporting. Initially, = 0.
Stargate_range - amount from and to - how much to buy USDT/USDC in non-single networks. If you don't have USDT/USDC in non-single networks in your wallets, the script will purchase a random amount from the specified range in the random network (!!).
STARGATE_FIRST_SWAP - specify "DONE" if you do not need to purchase USDT/USDC in non-single networks (it is assumed that one of the networks already has stablecoins for the run). If the purchase of stablecoins is necessary, leave them empty.
STARGATE_POLYGON/AVALANCHE - how many bridges from this network need to be made through StarGate. Initially 0. IMPORTANTLY! Must be 0, not an empty string, otherwise it will throw an error.
STARGATE_BSC - similar to , only for BSC.STARGATE_POLYGON/AVALANCHE
STARGATE_BSC_RANGE - the value of how much to buy USDT/USDC in BSC, similar to .STARGATE_RANGE
STARGATE_BSC_FIRST_SWAP - similar to , only for BSC.STARGATE_FIRST_SWAP
STARGATE_ARBITRUM - by analogy with , only for ARBITRUM.STARGATE_POLYGON/AVALANCHE
STARGATE_ARBITRUM_RANGE - by analogy with , only for ARBITRUM.STARGATE_POLYGON/AVALANCHE
STARGATE_ARBITRUM_FIRST_SWAP - by analogy with , only for ARBITRUM.STARGATE_FIRST_SWAP
STARGATE_LIQ - Adding liquidity doesn't work yet.
STARGATE_LIQ_VALUE - how much liquidity to add to Stargate does not work yet.
STARGATE_STG - whether you need to buy and stake STG. The module runs on POLYGON.
STARGATE_STG_RANGE - the amount from and to $, the range how much to buy STG for a steak.
BTC_BRIDGE - how many bridges to make from AVALANCHE to POLYGON and back with the purchase and sale of BTC.b. IMPORTANT! Must be 0 and not an empty string, otherwise it will give an error.
BTC_BRIDGE_RANGE - in what range to buy BTC.b in $ equivalent to work.
BTC_BRIDGE_STEP - stages, no need to touch. Initially empty. With each stage, an "X" will appear, indicating which stage the script is in: One X - bought BTC.b. Two X - bridged BTC.b into avalanche from the polygon. Three X - BTC.b bridged into the Avalanche polygon. Empty line - the script sold BTC.b and subtracted (one) from column 1.BTC_BRIDGEBTC_BRIDGE
TESTNET_BRIDGE - the number of times to swap ETH to GETH via testnetbridge.
TESTNET_BRIDGE_RANGE - the range of buying GETH in $ equivalent.
TESTNET_BRIDGE_CHAINS - Which networks to use in TestnetBridge. Chooses a random one. Write separated by commas, with a capital letter.
APTOS_BRIDGE - how many times to bridge USDT from BSC to APTOS.
APTOS_BRIDGE_RANGE - in what range to buy USDT/USDC.
APTOS_BRIDGE_WALLET - A unique Aptos wallet to which USDT will be bridged. (Generate on Cointool for convenience)
HARMONY_BRIDGE - how many times to bridge USDT from BSC to HARMONY.
HARMONY_BRIDGE_RANGE - In what range to buy USDT/USDC for bridge.
SWAP_TOKEN - random swaps to confuse the algorithm of activities to identify shave patterns. (Optional)
SWAP_TOKEN_RANGE - in what range to swap random tokens. (Optional)
SWAP_TOKEN_CHAINS - In which networks to make random swaps. (Optional)
APPROVE_DO - whether to make random token approvals for your wallet. The meaning is the same as token swaps, but cheaper. If you do, write the English "X".
APPROVE - log - the number of approvals, you don't need to change anything, you need it for reporting. Initially, = 0.
APPROVE_CHAINS - In which networks to approve.
APPROVE_TIMES - what is the random number of approvals from 0 to X to make between activities.
Logs
Text files with logs for specific wallets will appear in the folder. This is necessary for the convenience of tracking possible bugs and errors.
The file contains the entire log of activities. If it says "True" at the end of the line, it means that the activity was completed successfully. If it says "False", it means that the activity ended with an error. Also, in the cell with "False", the encountered error will be indicated.
The log from Cmd/IDE is duplicated into the file. In the same way, but broken down by wallets.log_walletlog.csvmain.txtlog_wallet

Notes
In case an error occurs while the script is running, the script will move on until it completes all the activities and there are no activities that it fails to perform (lack of funds, pool problem, etc.). All errors can be viewed in the logs.

Try not to stop the script at runtime. If you still can't do without stopping, wait for the activity to be completed and the Start in... line appears, and then stop the script with the Ctrl+Pause Break key combination. Otherwise, you will have to figure out at what stage you interrupted the script and edit data.csv
