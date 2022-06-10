# HoneypotRescueWithSafeBuy
On-chain honeypot detection with SafeBuy proxy method for secure transfers. Includes funds withdrawal rescue tools from honeypots and MasterChef contracts. 

Cross-chain supported by passing in a router or factory address.

Supported functionality:

- Honeypot detection (smart contract pays for test funds, minimal amount tested)

  `honeypotCheck(address targetTokenAddress, address routerAddr)`
  
   returns:
   
  `struct HoneypotCheckResponse {
        uint256 buyResult; // Result of buying transaction of honeypot token test
        uint256 targetTokenBalance; // Balance of the token we wanted to transfer (Usually set to $minTransactionAmount)
        uint256 sellResult; // Result of sell transaction of honeypot token test (If it's greater than 0 then we were able to sell)
        uint256 buyCost; // Cost of the gas to buy
        uint256 sellCost; // Cost of the gas to sell 
        uint256 expectedAmount; // How much user expected to receive. This will be equal to our $minTransactionAmount.
    }
    `
    
    If sellResult is 0, then it's a honeypot.

- Honeypot detection (user pays for test funds from transfer transaction)

  `honeypotCheckCustomAmount(address targetTokenAddress, address routerAddr) payable`
  
   returns:
   
  `HoneypotCheckResponse`
  
- SafeBuy proxy method for secure transactions

  This method automatically checks for honeypot transactions (contract pays test fees) before allowing a user transfer transaction to go through. 
  Will return swapped tokens if approved transaction or original tokens back to user if a honeypot is found.
  
  `safeBuy(address targetTokenAddress, address routerAddr) payable`

- Honeypot rescue emergency withdraw (emergency withdrawal from MasterChef all pools)

  `emergencyWithdrawFromAllPoolsInMasterChef(address addr)`
  
- Honeypot rescue bypass sell (attempts a transfer using Factory methods in case honeypot contract has disabled transfers)
  User must deposit liquidityToken amount to swap first with `depositToken(address)`. Attempts 
  
  `honeypotBypass(address honeypotToken, address liquidityToken, address factoryAddr)`
  
  If a swap cannot be done through DEX, the user can withdraw their liquidityToken with `withdrawToken(address)`.
  
- Honeypot rescue bypass sell (custom amount of liquidityToken to swap)
  
  `honeypotBypassCustomAmount(address honeypotToken, address liquidityToken, address factoryAddr, uint256 amount) payable`
