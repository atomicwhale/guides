# How to enable mev-boost on Rocketpool Smart Node on Holesky
### Adjust BN settings
Please double check this flag with the BN you are using (I am using Nimbus here)  
`nano .rocketpool/override/eth2.yml`  
Add these lines
```
    environment:
      - MEV_BOOST_URL=http://mev-boost:18550
```
### Add additional mev-boost docker compose settings
`mev-boost.yml` in the override folder will be ignored by smartnode on holesky, so we need to use one of the containers enabled on holesky.  
I am using `eth2.yml` here.   
`nano .rocketpool/override/eth2.yml`  
Add following:
```
services:
  mev-boost:
    image: flashbots/mev-boost:1.7
    container_name: rocketpool_mev-boost
    restart: unless-stopped
    ports: ["18550:18550/tcp"]
    volumes:
      - ~/.rocketpool/scripts:/setup:ro
    networks:
      - net
    environment:
      - NETWORK=devnet
      - MEV_BOOST_PORT=18550
      - MEV_BOOST_RELAYS=<Your relays of choice>
    entrypoint: sh
    command: "/setup/start-mev-boost.sh"
    cap_drop:
      - all
    cap_add:
      - dac_override
    security_opt:
      - no-new-privileges
networks:
  net:
```
### Check VC flag to enable mev-boost
I recommand to use an external VC (with builderblocks flag enabled) to connect to RP smartnode BN. Check specific flags needed for the specific VC.  
The VC in RP smartnode will continue to use local blocks because the `externalbuilder` setting is disabled by default.

**A list of MEV relays on holesky**
```https://0xb1559beef7b5ba3127485bbbb090362d9f497ba64e177ee2c8e7db74746306efad687f2cf8574e38d70067d40ef136dc@relay-stag.ultrasound.money,https://0xab78bf8c781c58078c3beb5710c57940874dd96aef2835e7742c866b4c7c0406754376c2c8285a36c630346aa5c5f833@holesky.aestus.live,https://0x821f2a65afb70e7f2e820a925a9b4c80a159620582c1766b1b09729fec178b11ea22abb3a51f07b288be815a1a2ff516@bloxroute.holesky.blxrbdn.com,https://0xaa58208899c6105603b74396734a6263cc7d947f444f396a90f7b7d3e65d102aec7e5e5291b27e08d02c50a050825c2f@holesky.titanrelay.xyz/,https://0xafa4c6985aa049fb79dd37010438cfebeb0f2bd42b115b89dd678dab0670c1de38da0c4e9138c9290a398ecd9a0b3110@boost-relay-holesky.flashbots.net,http://0x821f2a65afb70e7f2e820a925a9b4c80a159620582c1766b1b09729fec178b11ea22abb3a51f07b288be815a1a2ff516@testnet.relay-proxy.blxrbdn.com:18552/,https://0x833b55e20769a8a99549a28588564468423c77724a0ca96cffd58e65f69a39599d877f02dc77a0f6f9cda2a3a4765e56@relay-holesky.beaverbuild.org,https://0xb1d229d9c21298a87846c7022ebeef277dfc321fe674fa45312e20b5b6c400bfde9383f801848d7837ed5fc449083a12@relay-holesky.edennetwork.io```
