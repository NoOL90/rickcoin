// RickCoin NFT Oracle - Dynamic Metadata Script
// Updates metadata daily for NFT holders via IPFS pinning or external URI logic

const fs = require("fs");
const path = require("path");
const { create } = require("ipfs-http-client");
const crypto = require("crypto");

const ipfs = create({ url: "https://ipfs.infura.io:5001/api/v0" });

const traits = ["Mutant Pig", "Brain on Legs", "Quantum Waffle", "Exploding Clock", "Rick's Flask"];

function generateMetadata(tokenId) {
  const seed = crypto.createHash("sha256").update(tokenId + Date.now().toString()).digest("hex");
  const traitIndex = parseInt(seed.substring(0, 2), 16) % traits.length;
  const trait = traits[traitIndex];

  const metadata = {
    name: `RickCoin Relic #${tokenId}`,
    description: `A chaotic tokenized object from the RickCoin Multiverse. Current state: ${trait}.`,
    image: `https://rickcoin.nft/assets/${trait.replace(/\s+/g, "_").toLowerCase()}.png`,
    attributes: [{ trait_type: "Current Form", value: trait }]
  };

  return metadata;
}

async function updateNFTMetadata(tokenId) {
  const metadata = generateMetadata(tokenId);
  const filePath = path.join(__dirname, `metadata-${tokenId}.json`);
  fs.writeFileSync(filePath, JSON.stringify(metadata, null, 2));

  const file = fs.readFileSync(filePath);
  const { cid } = await ipfs.add(file);
  console.log(`Updated metadata for token #${tokenId}: https://ipfs.io/ipfs/${cid}`);
}

// Example update
updateNFTMetadata("42");
