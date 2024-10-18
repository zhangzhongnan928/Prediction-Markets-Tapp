
# Prediction Markets Tapp Concept

## Overview:
A Twitter-based prediction market where users can bet on outcomes using a Tlink-integrated tapp. 
When User A tweets a prediction, tBot replies with a clickable Tlink that renders a card for others to join and bet. Each bet costs $1, 
and the distribution of the betting pool includes rewards for the original poster and subsequent sharers of the prediction.

### Prediction NFT Tapp Ownership:
- Each prediction is a **Prediction NFT tapp**, and the owner of the tapp is the user who originally mentions @tBot.
- The NFT represents the prediction itself and tracks the bets, shares, and final outcomes associated with that prediction.

### Tweet to Initiate Prediction:
1. **User Flow:** 
    - User A tweets a prediction, for example, "BTC will pass $70K today @tBot."
    - tBot listens for mentions like this and automatically generates a Tlink to that specific prediction, replying to the tweet with a card and bet options.
    - The card has two main buttons: “Bet Yes” and “Bet No.” Each bet costs $1, and users can bet multiple times by clicking more than once.
   
2. **Technical:**
    - Use Twitter API to listen for tweets mentioning `@tBot`.
    - Upon detecting a mention, a Prediction NFT tapp is created, and the Tlink is generated using the Tapp engine with a unique ID for the prediction event.
    - The owner of the tapp (the user who @tBot) is granted the NFT representing the prediction.
    - The Tapp card uses ERC-5169/7738 to handle the bet collection and state management of the pool.
    - The Tlink URL could look like: `https://viewer.tokenscript.org/?contract=contractAddress&tokenId=predictionID`

### Betting System:
1. **User Flow:**
    - Other users click on the link and bet $1 for their choice (Yes or No). The more clicks, the more they bet.
    - The system allows for the same user to bet multiple times, with each bet incrementing their stake.
   
2. **Technical:**
    - Smart contracts hold the $1 bet from each user. Each choice (Yes/No) is a separate pool.
    - A simple balance tracking system is implemented where each user's bet (total count) is stored.

### Revenue Distribution:
1. **User Flow:**
    - After the result is finalized, the winning side splits the pool according to their contribution.
    - 10% of the total pool goes to the original poster (User A, who owns the Prediction NFT tapp), and 1% goes to the platform.
    - The system also allows for viral sharing: if User B shares User A's Tlink, 50% of User A's 10% cut gets shared with User B and any subsequent sharers (50% of 50% for further chains).
   
2. **Technical:**
    - A reward calculation is set in the smart contract:
        - **10% to Prediction Owner (User A)**.
        - **1% to platform**.
        - **50% of 10% shared in a chain** for sharers (User B, C, etc.).
    - The splitting logic can be handled by a recursive formula or using a fixed max number of steps for simplicity.

### NFT for each prediction:
1. **User Flow:**
    - Each prediction is an NFT tapp, owner is the people who @tBot.
    - The NFT tapp cards for public: Yes $1, No $1, share to earn, check result
    - The NFT tapp cards for owner: Yes $1, No $1, share to earn, withdraw money
   
2. **Technical:**
    - An ERC-721 (NFT) contract is issued upon the prediction is created by tBot.

### Finalizing and Settling Bets:
1. **User Flow:**
    - Once the prediction period ends, the result is determined (either manually by an admin or via an oracle).
    - The winning side receives their portion of the pool, proportional to their bet.
   
2. **Technical:**
    - The result can be verified either manually or using a decentralized oracle (Chainlink or a custom-built oracle to fetch BTC prices).
    - The smart contract distributes funds to the winning side once the final outcome is known.

### Viral Sharing System:
1. **User Flow:**
    - When a user shares a prediction via the “Share” button, a unique Tlink is generated. Anyone who places a bet via that shared link contributes to the revenue share chain.
    - The shared Tlink also earns a portion of the 10% reward, as described in the revenue distribution logic.

2. **Technical:**
    - Each shared link creates a unique identifier tied to the original prediction, but with a record of the sharer’s ID to handle revenue splitting in the smart contract.
    - The chain continues as new users share, but the percentage shrinks based on the hierarchy.

## Smart Contract Design:
The smart contract would need to cover:
1. **Prediction NFT Tapp** issuance and ownership.
2. **Betting pools** for both Yes/No outcomes.
3. **Revenue distribution** logic for creator and sharers.
4. **NFT issuance** for user account management.
5. **Oracle integration** for prediction result verification.

