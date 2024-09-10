## 1. Check `​.env` file that BUILDER_API has been enabled  
 ​`BUILDER_API_ENABLED=true`  
## 2. Check BN is configured to use external builder  
You BN should be configured with a mwv-boost URL, see obol or specific BN guide  
If you are running a RP smartnode and have MEV-boost enabled, this should have been set by the smartnode.  
## 3. Check MEV-boost is configured correctly  
3.1 Check mev-boost relays list, see below for an example list  
3.2 Check the logs and see if you are seeing the relays  
 ​`docker logs <mev-boost-container-name> |& less`​   
For example
 `​time="2024-07-24T05:50:10.308Z" level=info msg="using 5 relays" version=v1.8​` 
## 4. Check Charon logs
 `​docker logs charon-charon-1 |& grep "submitted validator registration"`  
You should see submitted validator registration in the logs
## 5. Check VC (lodestar)
5.1 Your `lodestar/run.sh` should be set to `useProduceBlockV3`
 `nano lodestar/run.sh`  
 Check this line has been set to:  
 ​`--useProduceBlockV3=true`​  
5.2 Check /lodestar/run.sh has been change to `builderonly`  
`  BUILDER_SELECTION="builderonly"`

----------
- Obol's guide can be found here: https://docs.obol.org/docs/advanced/quickstart-builder-api#getting-started-with-charon--the-builder-api
- Relays list (mainnet)
	`https://0xac6e77dfe25ecd6110b8e780608cce0dab71fdd5ebea22a16c0205200f2f8e2e3ad3b71d3499c54ad14d6c21b41a37ae@boost-relay.flashbots.net/,https://0x8b5d2e73e2a3a55c6c87b8b6eb92e0149a125c852751db1422fa951e42a09b82c142c3ea98d0d9930b056a3bc9896b8f@bloxroute.max-profit.blxrbdn.com/,https://0xb0b07cd0abef743db4260b0ed50619cf6ad4d82064cb4fbec9d3ec530f7c5e6793d9f286c4e082c0244ffb9f2658fe88@bloxroute.regulated.blxrbdn.com/,https://0xa1559ace749633b997cb3fdacffb890aeebdb0f5a3b6aaa7eeeaf1a38af0a8fe88b9e4b1f61f236d2e64d95733327a62@relay.ultrasound.money/,https://0xa15b52576bcbf1072f4a011c0f99f9fb6c66f3e1ff321f11f461d15e31b1cb359caa092c71bbded0bae5b5ea401aab7e@aestus.live/,https://0x8c4ed5e24fe5c6ae21018437bde147693f68cda427cd1122cf20819c30eda7ed74f72dece09bb313f2a1855595ab677d@global.titanrelay.xyz,https://0x8c4ed5e24fe5c6ae21018437bde147693f68cda427cd1122cf20819c30eda7ed74f72dece09bb313f2a1855595ab677d@regional.titanrelay.xyz`
- Relays list (Holesky)
  `​https://0xb1559beef7b5ba3127485bbbb090362d9f497ba64e177ee2c8e7db74746306efad687f2cf8574e38d70067d40ef136dc@relay-stag.ultrasound.money,https://0xab78bf8c781c58078c3beb5710c57940874dd96aef2835e7742c866b4c7c0406754376c2c8285a36c630346aa5c5f833@holesky.aestus.live,https://0x821f2a65afb70e7f2e820a925a9b4c80a159620582c1766b1b09729fec178b11ea22abb3a51f07b288be815a1a2ff516@bloxroute.holesky.blxrbdn.com,https://0xaa58208899c6105603b74396734a6263cc7d947f444f396a90f7b7d3e65d102aec7e5e5291b27e08d02c50a050825c2f@holesky.titanrelay.xyz/,https://0xafa4c6985aa049fb79dd37010438cfebeb0f2bd42b115b89dd678dab0670c1de38da0c4e9138c9290a398ecd9a0b3110@boost-relay-holesky.flashbots.net​`
