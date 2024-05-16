# RadiumDexFork
This is Radium Dex fork on Solana.\
This is writed using Rust, Anchor, Typescript, Javascript.

## Radium AMM fork
### description
deposit: X% --> treasury wallet, Y% --> liquidity.\
token sale set: manually set, time deadline, or hardcap hit\
create a Raydium liquidity pool

### main function
1. Create token listings
2. A deposit / invest function, such that for each token listing people can buy in the presale.
3. Create vesting schedule in a separate vesting contract (e.g. github.com/Bonfida/token-vesting - but can also be any different vesting contract, just a quick example I found)
4. Automatic creation of Raydium AMM pool + OpenBook ID after the token sale is over.

### Requirements
High quality, maintainable and production ready Solana Program (Smart Contract)\
Unit / Integration tests to make sure everything works accordingly\
Typescript SDK / lib file to interact with the contract

### Contract Functions
#### Create Token listing - ADMIN ONLY
**Description:**\
This function should be used by admins to create new token listings on the launchpad. Each new token will have an active presale, and will be listed on Raydium after the presale.\
**Params:**
* Token Symbol (Used as identifier)
* Token Address
* Treasury address
* % of funds to be used for liquidity, rest gets send to treasury
* hard cap
* Start time (if blank current time)
* End time (if blank, no end time)
* Min Ticket Size - Minimum number of SOL to purchase, by default 0.01
* Max Ticket Size - Maximum number of SOL to purchase, by default 10
* Amount of tokens to be sold
> Token price has to be calculated based on this,
> These tokens should be sent over in the Create Token Listing Tx, and stored
* Cliff and Vesting period for purchased tokens (to be used when creating the vesting contract)
* Time delay between end time and pool creation, 0 if empty - E.g. wait 3 days after pre-sale end to create pool
* + any input needed for the pool creation

#### Update Listing - ADMIN ONLY:
**Description:**\
This function should be used to update a token listing\
**Params:**
* New Treasury Address
* New % of funds to be used for liquidity
* New hard cap
* New Start time
* New End time
* New Time delay between end time and pool creation

#### Get Token listing
**Params:**\
* Token Symbol\
**Returns:**
* Amount raised so far
* Amount tokens / USD still available in presale
* Number of deposits
* hardcap
* start time / end time
* % of funds to be used for liquidity
* Amount of tokens to be sold
* Amount sold so far

#### List tokens
**Description:**\
* This function should be used to list all tokens that are currently available for presale\
**Returns:**
* List of Get Token Listing for all tokens

#### Purchase Tokens
**Description:**\
This function should be used to buy tokens in the presale.\
Part of the funds get send to the treasury wallet, part of the funds get locked for liquidity.\
This function should call a deployment of github.com/Bonfida/token-vesting to create a vesting schedule for the purchased tokens.\
**Params:**
* Token Symbol
* Amount
**Returns:**
* Amount of tokens bought

#### Update Vesting Contract Address - ADMIN ONLY
**Description:**\
Update the contract address of the github.com/Bonfida/token-vesting deployment.\
**Params:**
* Contract Address

#### Add Admin - ADMIN ONLY
**Description:**\
Add admin users.\
**Params:**
* Contract Address

#### Remove Admin - ADMIN ONLY
**Description:**\
Remove admin users.\
**Params:**
* Contract Address
