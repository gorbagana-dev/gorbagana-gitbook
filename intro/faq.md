# FAQ

### Is $GOR the only official token?

Yes, $GOR is the only official token related to the Gorbagana chain. No other tokens are endorsed by the development team. Beware of fake tokens and scams claiming official status.

&#x20;        CA: 71Jvq4Epe2FCJ7JFSF7jLXdNk1Wy4Bhqd9iL6bEFELvg

### Is there a $GOR token on Gorbagana?

While we are in the development stage, the $GOR token on Testnet/Devnet have any value as these networks are for development purposes only.

### What is the plan for the $GOR token?

Upon Mainnet release, there will be an available bridge contract to move $GOR from Solana Network to Gorbagana Network. As $GOR will be the native gas token of Gorbagana, you will be required to have $GOR on Gorbagana to send transactions and interact with the chain, similarly to how you must have $SOL on Solana to interact with the chain directly.

When interacting with the Bridging Contract, your tokens will be locked in escrow on the origin chain and an equivalent amount is minted/released on the destination chain - tokens aren't destroyed, just moved between ecosystems.

### I am working on a project, how do I obtain more $GOR on testnet?

While you are welcome to utilize our [Faucet](https://faucet.gorbagana.wtf/), if you need a larger amount for deploying and testing, please create a twitter post tagging [@gorbagana_chain](https://x.com/Gorbagana_chain) referencing the application you are building with your Github link as well as a wallet address for us to send more testnet tokens!

### What are the Gorbagio NFTs for and what utility do they have?

This NFT collection will be your pass to the elite ranks of the Gorbagana ecosystem, strongly encouraged to be used by Gorchain apps for airdrops and perks. These NFT's are expected to be the first bridgeable collection from Solana to Gorbagana and have the potential to allow our community to test out some features before public releases.

### How do I purchase a Gorbagio NFT?

<div class="marketplace-links">
  <a href="https://magiceden.us/marketplace/gorbagio" class="marketplace-card" target="_blank">
    <div class="card-content">
      <div class="card-icon" style="background: linear-gradient(45deg, #e439ff, #ff6b9d);">
        ME
      </div>
      <div class="card-text">
        <h4>Magic Eden - Gorbagio Collection</h4>
        <p>Browse and purchase Gorbagio NFTs on Magic Eden</p>
      </div>
    </div>
  </a>

  <a href="https://www.tensor.trade/trade/gorbagio" class="marketplace-card" target="_blank">
    <div class="card-content">
      <div class="card-icon" style="background: linear-gradient(45deg, #00d4ff, #4a9eff);">
        T
      </div>
      <div class="card-text">
        <h4>Tensor - Gorbagio Trading</h4>
        <p>Advanced trading and analytics for Gorbagio NFTs</p>
      </div>
    </div>
  </a>
</div>

<style>
.marketplace-links {
  margin: 20px 0;
}

.marketplace-card {
  display: block;
  background: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 100%);
  border: 1px solid #404040;
  border-radius: 12px;
  padding: 20px;
  margin-bottom: 16px;
  text-decoration: none;
  color: white;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.marketplace-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 25px rgba(0,0,0,0.4);
  border-color: #606060;
}

.marketplace-card:before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: linear-gradient(90deg, #e439ff, #00d4ff);
  opacity: 0;
  transition: opacity 0.3s ease;
}

.marketplace-card:hover:before {
  opacity: 1;
}

.card-content {
  display: flex;
  align-items: center;
}

.card-icon {
  width: 56px;
  height: 56px;
  margin-right: 20px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  font-weight: bold;
  color: white;
  text-shadow: 0 2px 4px rgba(0,0,0,0.3);
}

.card-text h4 {
  margin: 0 0 6px 0;
  font-size: 18px;
  font-weight: 600;
  color: white;
}

.card-text p {
  margin: 0;
  font-size: 14px;
  color: #b0b0b0;
}
</style>

### Which wallets support Gorbagana?

Currently, [Backpack](https://backpack.app/) is the primary supported wallet. Major Solana wallets like Phantom, Solflare, and others don't support custom chains yet. You'll need to use Backpack with the custom RPC feature to connect to the network. For help accessing the network, please see '[Connecting to Testnet](../network-access/connecting-to-testnet.md)'.