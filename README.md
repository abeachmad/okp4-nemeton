# How to run node validator of okp4 nemeton testnet using docker
# okp4 official docker documentaion: https://github.com/okp4/okp4d
# source of state sync, seed and peers: https://nodejumper.io/okp4-testnet/sync

# pull the docker image from official docs.
docker pull okp4/okp4d:latest

# you can check how to use the commands by run:
docker run okp4/okp4d:latest --help

# before start, lets define variables:
CHAIN_ID=okp4-nemeton
MONIKER="replace with your name"

# then initialize the node by run:
docker run -v $(pwd)/.okp4d:/.okp4d okp4/okp4d:latest init $MONIKER --chain-id $CHAIN_ID --home /.okp4d

# This will generate, in the $HOME/.okp4d folder, the following files:
# config/app.toml: Application-related configuration file.
# config/client.toml: Client-oriented configuration file (not used when running a node).
# config/config.toml: Tendermint-related configuration file.
# config/genesis.json: The network's genesis file.
# config/node_key.json: Private key to use for node authentication in the p2p protocol.
# config/priv_validator_key: Private key to use as a validator in the consensus protocol.
# data: The node's database.

# Make sure to backup the generated config/priv_validator_key and config/node_key.json because both are very sensible, if stolen the entire stake delegated to the validator is at risk.

# Download the official genesis.json:
curl https://raw.githubusercontent.com/okp4/networks/main/chains/nemeton/genesis.json > $HOME/.okp4d/config/genesis.json

# Check if you have the correct genesis.json by run:
sha256sum $HOME/.okp4d/config/genesis.json
# the result should be: c2e8fff161850e419e1cb1bef3648c0ed0db961b7713151f10f2509e3fc2ff40

#add seed and persisten peers:
sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.0001uknow"|g' $HOME/.okp4d/config/app.toml
seeds="8e1590558d8fede2f8c9405b7ef550ff455ce842@51.79.30.9:26656,bfffaf3b2c38292bd0aa2a3efe59f210f49b5793@51.91.208.71:26656,106c6974096ca8224f20a85396155979dbd2fb09@198.244.141.176:26656,a7f1dcf7441761b0e0e1f8c6fdc79d3904c22c01@38.242.150.63:36656"
peers="994c9398e55947b2f1f45f33fbdbffcbcad655db@okp4-testnet.nodejumper.io:29656,958c383f1328ee949cf13a1a6f1ad2303e819eea@121.4.78.19:26656,83a5bec69a78e63f18e665b59a6f162363d8a518@65.108.100.53:34656,9b5d4a98d3732160e82921bb80b79eb608cb1587@161.35.72.166:26656,be7578dbdf52a350b88f29f15d6c6ab90947c3fc@45.81.224.126:26656,7e7e8a0ec3a20a0b9ce49fdfadc6c89b38f464c9@88.198.242.163:20656,5c91cebbea0ac1d1c4eb2c53dad169b34b23aa78@95.217.161.160:26656,2c49a31c4c763225f65c89aea076f198d58aa379@217.79.187.22:26656,3b383a769e0958b3bd44b21b4bc140088207e8dd@45.128.188.221:26656,5ce331c27ba867e11f56b933a51ae67289debfa2@23.88.10.3:26656,9ae02970da0f5a5fade69a268fbefdac13b69810@109.226.233.125:29656,10e463b4f649eb0c42a58d0048713349b0589464@161.35.172.245:26656,c383a523cd6559374bbd1f5d81e61bd11cda0f6f@195.201.197.4:36656,0e2cde3e1ddffbb1ac6ddab2cb27749fd9c2576c@5.161.159.23:36656,b0ad869604deb641d71b5c671fb50037ecba31fc@195.2.85.152:26656,2182373d3ffba08d67a54b50a78102bd1ec4b037@95.216.14.72:33656,a11f430485e1b4f06bf6be5172288214a67e674c@194.126.173.150:29656,8f778c5ba3e593c12debbb4840750e63dfd9b494@161.97.119.89:36656,807378b8ad2657637ffa7c051f1665a5a54567a6@176.120.177.123:26656,a9225a5c513bdc4a2421d0ca4b03b84c9e76ebe5@38.242.245.157:26656,e76cf197504f3390c64e6ac9ba33e1a585230d5a@84.21.171.170:26656,4d8406189309d6afb008e87f893d35dd10a9a2ec@45.88.223.161:26656,26418a5e3d2389441bb66c47f3bd20587cb59717@164.92.155.25:26656,ae373a3758864115c2e0693553cc0c3d0ed60c90@135.181.43.140:26656,b1df4e799904dcae2c85116218d409318c450356@46.101.174.26:26656,8cb8a8034471e78137d11589eb8e0bae768e3e8a@134.209.231.201:26656,cc15ceec925e511f9f660deb3671770341abee18@86.48.20.122:26656,663a2f57306cd3819a31079ec79989cfe7a11680@134.122.62.157:26656,de245278be4c3540f0a6a867c4bac83155b4ebac@178.62.30.239:46656,c140a0e42f4a1b01a698852b3a54b254bcec87f1@185.245.182.58:36656,b08698202eeb334b14057801ecbf25b49bc9dc36@94.131.107.38:26656,890906dd6b0f7cd884c4511751431601f5f194e9@176.124.217.166:36656,7ca0f76a967666f3f264b96b55f97eb421e2791e@34.170.76.169:26656,a1d19b4f6fcb4bca8afb42405a90ce33d592ff15@209.145.56.41:36656,73e347d597dad6c1d8d5d6ad2e3e59499993bb81@116.203.179.11:36656,5e79cb533fc6d3b2020aaadc1f877869bbd205a4@185.197.251.39:26656,e90452c94bce24a2e5c80d3fcaf8e10194414d50@185.188.249.179:26656,33d39b4365068fe4e9e26601d074cecba6b61da8@161.35.72.154:26656,eab52f5e01d4a5c6e214a50e9b87760098b2e64d@188.234.160.105:36656,1dee57be85b0d6103c27ef5be786b59895e9c42a@65.108.52.191:26656,2f6bcad4268381e0dc119eae8f58880d1128628a@216.250.122.228:36656,0543b0658b3ba6692e46ee1af4126e04205d6745@81.16.237.150:29656,4e7167487ede6b2f77acb28de6482b5b5baa2ae7@5.180.180.137:26656,c92f99f6c802b91867c9450b72e62ec4c347ca79@65.109.62.115:33656,6238fe1b28838ca0b3878dcabd72556b411918e8@185.209.28.231:26656,53f3e3abee5be9aef0de9adc663c94ae62c911df@135.181.158.205:26656,33aa29630d56f067580f19995ad87ef5e330764e@135.181.25.155:26656,a8bdf781811b36ba225b712ddef3cea5231271bc@65.108.42.105:30656,876d76951c87e1f89d1040daab009dcbc228d4f2@38.242.149.134:26656,8f04856e3491b97f267bd5716f43d8e11fc0ed42@194.233.67.92:36656,7a61906dbc3e8ab1d267817acb47efc623ea172a@143.198.208.228:26656,5f55d5638a1e8aee0de2fd0ec75d1f755402650d@178.208.228.249:26656,211e33d7feb058579c3a3b94d8a386e70e18f7a1@95.217.62.150:26656,12a395f190cb0b3752e5f3d079bef8080da9d22f@74.208.118.114:36656,ae3c1ef9af47a72dc1240d6d3f6359a8456fb9ea@167.71.10.180:26656,739dd95225d7cad290ca9a964137c32b17314fec@142.132.132.200:34656,fc439e25599888615190e3d8b9cde0d799334758@38.242.237.118:36656,8b20ffd20362d7e14acb79c2ca99a643aea437c1@159.223.86.212:26656,860d80fdc7286725fe09ca20f54291706f180654@104.248.113.179:26656,2f6d5a319ebee0201dff4a0e3b7526d0863a4d32@65.109.85.225:6070,d0de33ccff58afc208a7dac5e7b640c1103570e6@65.108.232.174:36656,486c6b1c85630f60d2382beec540ed8e21caed86@128.199.180.8:36656,a06c4d0c536cb8abf8e3f630932960de319f8d96@167.99.68.145:36656,9f8098b9e6ec296b9b1dc53ada701886e90aa6be@45.61.161.62:36656,0ace47c9e9037ef34e26bfaa6c1d24b791499771@46.101.43.11:26656,4eec9a8d098037688f9e7c8ca70fb8ca056f5ca5@143.198.222.19:26656,05582a1692ebcce17bbdd1565e03bb290074034f@20.38.37.130:36656,09b481ac726f6b9ebea93d8e87dbd414ccb133fa@157.245.147.88:36656,4d2d9257724834875f9848f096d264248c1bf2e2@159.65.22.188:26656,7f086ff89e53d9d2b2933ea677333b18132d069d@65.108.241.107:26656,0bd6a4a185f95c20e707b327e924797b5af11b02@95.214.53.178:36656,dd37382a7cd72d141013e4c77610d519208da12c@185.135.137.160:36656,a56a6a13555b8effadafede915d5532f5e9fa838@46.4.121.72:36656,ae73dca41dc810d6fba9ac4aa0ec4e5494e88826@142.132.199.236:30656,de024b927aedb7fc8887c123d12d61598ff6d385@81.5.117.14:36656,9d53d896fe92947f8955356191ec0d8c48ce5c98@143.198.179.16:26656,a900056a5a573403840cdd1f7e2fac7c6bd42821@144.91.127.111:36656,dacb91d59a7ad70d939ada1f4926dfb8e74d63f5@138.201.51.114:32656,5dcce174c9c34f42ce8bcdc979955e451e087311@159.223.90.179:26656,2f8a78d91ba2cce7fd17f29f781a1807a23658ec@178.18.247.240:36656,8b2b38ac2fff96cefd50a25581b6c5eecffcdfbf@80.7.115.136:36656,c5c8f6d5847bc7cc73685fcd9030d8d76fc6861d@45.89.54.87:26656,912f0a4c513b1aa9abb1f5653c4f6c9c6988700d@135.181.222.106:36656,d9c745f73a1ca685198b55c3e7dc691e8ce21c3f@125.167.94.112:26656,01528c9e79ffa059e4720e2ba1d4d85263f600e2@109.123.245.225:26656,1d9053cdf997044c07e0952b1ca90f3a1e4b17c4@75.119.146.75:36656,b6d9d2d20e346213abee3e8864cb77b90f8d0225@95.142.45.197:26656,02037773a328f6246bfbe436624d5582ad83df22@190.102.106.50:29656,488538301bb305f60de2e206c4e63bfdbaa4e72a@65.108.155.121:26656,670aa91f19ce47c640ccaa7b21a653e1212eebd1@45.84.0.251:26656,c030413e39be95c397c6681639f5d48675554c0c@51.79.78.121:26646,b8fddd530b2d8347212615b6a68c447aba0aed64@161.35.37.194:26656,24802e52cd833cddbb3af50e840e9eeb89905982@167.86.115.26:26656,6fb047e647333b3efe9e536d1825f52b571a6cf4@107.155.91.166:29656,25d94a05b59bc381b25976d6e24a6373a5010cd2@109.123.241.76:36656,952d50142665e95f8f2e2cdfdaea6022db10f6b8@45.150.67.251:26656,1113ce45fd8f4e943dbcc4ded7a0a66f395e2318@135.181.81.99:26656,88e7fc9274459d88d28a7748c5253cb62fcf7285@45.14.194.24:26656,77d804b57b3086e99b288163fdea6ac5a8b9b0ea@95.216.252.42:26656,1cbe918d4cd8eac6b25ef484459286e53ce8acbb@65.109.31.7:36656"
sed -i -e 's|^seeds *=.*|seeds = "'$seeds'"|; s|^persistent_peers *=.*|persistent_peers = "'$peers'"|' $HOME/.okp4d/config/config.toml

# download statesync to fasten the synconization of your node to the latest block:
cp $HOME/.okp4d/data/priv_validator_state.json $HOME/.okp4d/priv_validator_state.json.backup
okp4d tendermint unsafe-reset-all --home $HOME/.okp4d --keep-addr-book
rm -rf $HOME/.okp4d/data 
rm -rf $HOME/.okp4d/wasm
SNAP_NAME=$(curl -s https://snapshots2-testnet.nodejumper.io/okp4-testnet/ | egrep -o ">okp4-nemeton.*\.tar.lz4" | tr -d ">")
curl https://snapshots2-testnet.nodejumper.io/okp4-testnet/${SNAP_NAME} | lz4 -dc - | tar -xf - -C $HOME/home
mv $HOME/.okp4d/priv_validator_state.json.backup $HOME/.okp4d/data/priv_validator_state.json

# start the node:
docker run -v $(pwd)/.okp4d:/.okp4d okp4/okp4d:latest start --home /.okp4d

# make your wallet if you don't have one:
docker run -v $(pwd)/.okp4d:/.okp4d okp4/okp4d:latest keys add <your wallet name> --keyring-backend test --home /.okp4d
# remember to backup your mnemonic keys, this is the only one time you can see your mnemonic. You will never have an access anymore to view yoour mnemonic.

# or recover your wallet if you already had one:
docker run -v $(pwd)/.okp4d:/.okp4d okp4/okp4d:latest keys add <walletname> --recover --home /.okp4d

