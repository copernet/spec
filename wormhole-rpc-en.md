## Table 1：Data Query


RPC |feature 
---|----
whc_getinfo|	Get Wormhole node basic information
whc_getactivecrowd|	Get active crowdfunding from a specific address
whc_getallbalancesforaddress|	Get all types of token balance from a specific address
whc_getallbalancesforid	|Get all address and amount information of the specified token in the wormhole system
whc_getbalance	|Get Balance information of a specific token from a specific address
whc_getbalanceshash	|Get state hash of specific token at the current height of this node
whc_getcrowdsale|	Get detailed information from crowdfunding token
whc_getcurrentconsensushash	|Get state hash of Wormhole system at the current height in this node
whc_getgrants|	Get additional issuance grants from a specific token, destroy information
whc_getpayload|	Get Wormhole payload data from a specific transaction
whc_getproperty	|Get information from a specific token
whc_getsto	|Get detail information from a specific airdrop transaction
whc_gettransaction	|Get Wormhole protocol information from a specific transaction
whc_listblocktransactions|	Get Wormhole transaction list from a specific block
whc_listpendingtransactions|	Get pending Wormhole transaction list 
whc_listproperties|	List all tokens in Wormhole system
whc_listtransactions	|List Wormhole transactions in node’s wallet
whc_getfrozenbalance  | Get frozen balance information of a specific token from a specific address
whc_getfrozenbalanceforid | Get all frozen address and amount information of the specified token in the wormhole system
whc_getfrozenbalanceforaddress | Get all types of frozen token balance from a specific address
whc_getERC721PropertyNews | Get ERC721 property information 
whc_getERC721TokenNews | Get ERC721 token information 
whc_getERC721AddressTokens | Get the ERC721 token under the specified address and property 
whc_getERC721PropertyDestroyTokens | Get the destroyed ERC721 tokens under the specified property 
whc_ownerOfERC721Token | Query whether the Token's owner is the specified address 
whc_listERC721PropertyTokens | Lists all ERC721Token that specify the ERC721 property issuance 

#### whc_getinfo
Explanation: Get basic information about the current Wormhole node
Call: wormholed-cli whc_getinfo
Return value: basic information of the current Wormhole node
Examples are as follows：

```
wormholed-cli whc_getinfo
{
  "wormholeversion_int": 6000,
  "wormholeversion": "0.0.6",
  "bitcoincoreversion": "0.17.2",
  "block": 543198,
  "blocktime": 1534136847,
  "blocktransactions": 0,
  "totaltransactions": 1099,
  "alerts": [
  ]
}
```
Return value field description

* wormholeversion_int : version of the Wormhole node 
* wormholeversion : version of the Wormhole node 
* bitcoincoreversion : version of bitcoin-abc 
* block : the latest block height of the node 
* blocktime: the latest block timestamp of the node 
* blocktransactions : the number of Wormhole transactions contained in the current most recent block 
* totaltransactions : the number of all Wormhole transactions in the blockchain at the current height 
* alerts : warning message for node rules

#### Whc_getactivecrowd
Explanation: Get active crowdfunding from a specific address 
Call: wormholed-cli whc_getactivecrowd address
Parameters: address : specific query address
R eturn value
* Return information about the active crowdfunding token when there is an active crowdfunding at this address
* There is no active crowdfunding, return {}. The example is as follows:
```
wormholed-cli whc_getactivecrowd  qq893ghdg697e5t5anh5fqpxwxxhw3akyu9l7wej0q
{ 
 "propertyid": 168,  
  "name": "bcext",  
  "category": "bcext development",  
  "subcategory": "gcash cashwallet cashutil neutrino",  
  "data": "contribute to bitcoin-abc",  
  "url": "https://github.com/bcext",  
  "precision": 8,  
  "issuer": "bitcoincash:qq893ghdg697e5t5anh5fqpxwxxhw3akyu9l7wej0q",  
  "creationtxid": "1f2a39a5fce4e2ce877b611925ef4c6eedb805c1337edd8f1d4bab49e2fe2449",  
  "totaltokens": "100000000.12345678"
}
```
Return value field description
* propertyid : tokenID
* name : token name
* category : token category
* subcategory : token subcategory
* data : token information
* url : the URL of the token
* precision : token precision
* issuer : publisher
* creationtxid : create the transaction ID of the token
* totaltokens : total amount of crowdfunding tokens issued


#### whc_getallbalancesforaddress
Explanation: Get non-zero balance information from all types of tokens at the specified address; no display for tokens with a value of 0
Call: wormholed-cli whc_getallbalancesforaddress address
Parameters: address : specify query address
Return value
• Return token balance information if the token balance is not 0; otherwise it returns {}.
Examples are as follows:
```
wormholed-cli whc_getallbalancesforaddress qr3pzyxl33vdhga54rvh80s62pjznl9eyu9k7q9tmv
[
       {
         "propertyid": 1,        
         "balance": "14.00000000",
         "reserved": "0.00000000"
       },
       {
         "propertyid": 3,
         "balance": "500",
         "reserved": "0"
       }
]
```
Return value field description
* propertyid : tokenID
* balance : available balance
* reserved : frozen balance, the current version is not available


#### whc_getallbalancesforid
Explanation: Get all the addresses in the current blockchain that contain the specified token and the amount information of the token.
Call: wormholed-cli whc_getallbalancesforid property
Parameters: property : tokenID
Return value
• List of addresses holding that token
Examples are as follows:
```
wormholed-cli whc_getallbalancesforid 1
[
   {
       "address": "bitcoincash:qrpcu87d3y83jg6pjhxrk7ys2225rp9m25nypfvtvk",
       "balance": "0.00003000",
       "reserved": "0.00000000"
   },
   {
       "address": "bitcoincash:qzf6rh7z40963a9jhh8agmnexw486g205upjjhsfzl",
       "balance": "100.00000000",
       "reserved": "0.00000000"
   }
]
```
Return value field description
* address : address
* balance : available balance
* reserved : Frozen balance, the current version is not available

#### whc_getbalanceshash
Description: Return the state hash of the specified token in the system when the current node is at the highest block.
Call：wormholed-cli whc_getbalanceshash 1
Parameters: specified tokenID
Return value: the existing state hash that the token has in the system
Examples are as follows:
```
wormholed-cli whc_getbalanceshash 1
{
  "block": 542347,
  "blockhash": "000000000000000000f6d63c8adeff1f4bc6fe3618d73f74813c05f08761060a",
  "propertyid": 1,
  "balanceshash": "f4a051549368b79409b25ad5c3dba4a9e8b0434996d88be8281969f204b35dee"
}
```
Return value field description
* block : the number of the highest block in the current node
* blockhash : hash of this block
* propertyid : tokenID
* balanceshash : the existing state hash that the token has in the system

#### whc_getbalance
Description: Return the specific address, and the balance information of a specific token
Call: wormholed-cli whc_getbalance address propertyid
parameter

* address : the specified address
* propertyid : tokenID
Return value: Amount information of the specified token of the specified address
```
wormholed-cli whc_getbalance qqmrktdkuj0qtu0dyef0h2xkn7u6stycuvk70k0ups 1
{
  "balance": "5.00000000",
  "reserved": "0.00000000"
}
```
Return value field description
* balance : available balance
* reserved : frozen balance, the current version is not available


#### whc_getcrowdsale
Description: Get information of the crowdfunding tokens
Call: wormholed-cli whc_getcrowdsale propertyid (verbose)
parameter:

* propertyid : the ID of the crowdfunding token
* verbose : true, which lists information about crowdfunding participation; false, does not list information about crowdfunding participation
Return value: return information of the crowdfunding token
Examples are as follows:
```
wormholed-cli  whc_getcrowdsale  168 true
{
  "propertyid": 168,
  "name": "bcext",
  "active": true,
  "issuer": "bitcoincash:qq893ghdg697e5t5anh5fqpxwxxhw3akyu9l7wej0q",
  "propertyiddesired": 1,
  "precision": "8",
  "tokensperunit": "1000.11111111",
  "earlybonus": 10,
  "starttime": 1533266569,
  "deadline": 1534733523,
  "amountraised": "1.12345000",
  "tokensissued": "100000000.12345678",
  "addedissuertokens": "0.00000000",
  "participanttransactions": [
    {
      "txid": "ebdc302cddb8fad9e8e11a210af7c01e57b4cf3c883b1ff7d504b4a3791177da",
      "amountsent": "1.12345000",
      "participanttokens": "1393.76317766"
    }
  ]
}
```
Return value field description 
* propertyid :tokenID 
* name :token name 
* active: whether crowdfunding is active 
* issuer: the issuer of crowdfunding 
* propertyiddesired : the tokenID collected
* precision: the precision of the crowdfunding token 
* tokensperunit : The single unit price of the crowdfunding token, i.e. 1WHC equals to a certain amount of tokens. 
* earlybonus: early bird reward 
* starttime : the time when the crowdfunding started 
* deadline : the crowdfunding deadline 
* amountraised: funds raised 
* tokensissued : amount of crowdfunding tokens issued 
* addedissuertokens: When crowdfunding is closed, the number of unsold crowdfunding, these amount of tokens will be sent to the issuer's account address 
* participanttransactions: information for crowdfunding participants 
* txid: transaction ID participating in crowdfunding 
* amountsent: the amount of WHC participating in crowdfunding 
* participanttokens: the amount of crowdfunding tokens purchased

#### whc_getcurrentconsensushash
Description: Get the state hash of the current Wormhole system
Call: wormholed-cli whc_getcurrentconsensushash
Return value: hash of system state
Examples are as follows:

```
wormholed-cli whc_getcurrentconsensushash
{
  "block": 542354,
  "blockhash": "00000000000000000104a3002e3ddf7ccc835751e09ef3335a22078b129c2c71",
  "consensushash": "95e4e7fa0d92f84b18ec45ad9b39fdafcc5200ba8175227d7d571864aa5948c9"
}
```
Return value field description 
* block : the number of the highest block in the current node 
* blockhash : hash of this block
* consensushash : hash of system state

whc_getgrants
Description: Return the additional issuance of the managable token or destroy information 
Call: wormholed-cli whc_getgrants propertyid
Parameters: tokenID
Return value: Manage the additional or destroy information of the token
Examples are as follows:
```
wormholed-cli whc_getgrants 166
{
  "propertyid": 166,
  "name": "blockchain extension development",
  "issuer": "bitcoincash:qpadl79yr4hh0fym4gw73mxp3rm4325knqe7fj6a9s",
  "creationtxid": "a82b29d69538268fb67df384b9be7128456724e6fa69e4eb387944cb78ed9d66",
  "totaltokens": "0.00000",
  "issuances": [
    {
        "txid":
        "grant":
    }
    {
        "txid":
        "revoke":
    }
  ]
}
```
Return value field description

* propertyid : tokenID
* name :token name
* issuer :token issuer
* creationtxid : transaction ID of the creation of the token
* totaltokens : current token circulation supply
* issuances :token management information
* txid : the transaction ID of the additional issuance or destruction
* grant: the amount of additional issued tokens
* revoke: the amount of tokens destroyed


#### whc_getpayload
Description: Get the Wormhole payload data of the specific transaction
Call: wormholed-cli whc_getpayload txid
Parameters: transaction ID
Return value: specific data of the specific transaction in Wormhole protocol.
Examples are as follows:
```
wormholed-cli whc_getpayload a82b29d69538268fb67df384b9be7128456724e6fa69e4eb387944cb78ed9d66
{
  "payload": "00000036010005000000006263657874207265706f7369746f7279006763617368206361736877616c6c657420636173687574696c206e65757472696e6f00626c6f636b636861696e20657874656e73696f6e20646576656c6f706d656e740068747470733a2f2f6769746875622e636f6d2f626365787400666f72207468652066757475726500",
  "payloadsize": 136
}
```
Return value field description
* payload: payload data
* payloadsize : the length of the payload data



#### whc_getproperty

Description: Get information of the specific token  
Call: wormholed-cli whc_getproperty propertyid 
Parameters: tokenID 
Return value: detailed information of the token 
Examples are as follows:
```
wormholed-cli whc_getproperty 168
{
  "propertyid": 168,
  "name": "bcext",
  "category": "bcext development",
  "subcategory": "gcash cashwallet cashutil neutrino",
  "data": "contribute to bitcoin-abc",
  "url": "https://github.com/bcext",
  "precision": 8,
  "issuer": "bitcoincash:qq893ghdg697e5t5anh5fqpxwxxhw3akyu9l7wej0q",
  "creationtxid": "1f2a39a5fce4e2ce877b611925ef4c6eedb805c1337edd8f1d4bab49e2fe2449",
  "fixedissuance": false,
  "managedissuance": false,
  "totaltokens": "100000000.12345678"
}
```
Return value field description
* propertyid :tokenID
* name :token name
* category : token category
* subcategory : token subcategory
* data : token information
* url : the URL of the token
* precision : token precision
* issuer : publisher
* fixedissuance : whether it is a fixed attribute token
* managedissuance : whether it is a manageable token
* creationtxid : transaction ID of the creation of the token
* totaltokens : total amount of crowdfunding tokens issued


#### whc_getsto
Description: Get information about airdrops
Call: wormholed-cli whc_getsto txid recipientfilter
parameter
* txid : the airdrop transaction ID
* recipientfilter: Filter; when the address is specified, only the received airdrop amount information is displayed; when it shows *, the airdrop amount information received by all accounts will be displayed by default. Return value: Airdrop detailed information

Examples are as follows:
```
wormholed-cli whc_getsto 403ec9b6f8b142485ea514d52bc4c782f008021a261f637028a28e1a64681d1b
{
  "txid": "403ec9b6f8b142485ea514d52bc4c782f008021a261f637028a28e1a64681d1b",
  "fee": "268",
  "sendingaddress": "bchtest:qz04wg2jj75x34tge2v8w0l6r0repfcvcygv3t7sg5",
  "ismine": false,
  "version": 0,
  "type_int": 3,
  "type": "Send To Owners",
  "propertyid": 12,
  "precision": "1",
  "amount": "50.0",
  "totalstofee": "0",
  "recipients": [
  ],
  "valid": false,
  "invalidreason": "Unknown error",
  "blockhash": "000000000000037252bf77bba30e5599b20239eb8f9f68c8b18c238688a27f6b",
  "blocktime": 1531904498,
  "positioninblock": 999999,
  "block": 1247269,
  "confirmations": 3368
}
```
Return value field description
* txid : the airdrop transaction ID
* fee : BCH transaction fee for the transaction
* sendingaddress : the sender of the airdrop transaction
* ismine : Whether the sending address belongs to the address in the node wallet
* version : the version number of the airdrop type
* type_int : type of airdrop transaction
* type : airdrop transaction type
* propertyid : the tokenID of the airdrop
* precision: the token precision of the airdrop
* amount : the amount of token of the airdrop
* totalstofee: the transaction fee for this airdrop
* valid : whether it is a valid airdrop transaction
* invalidreason : invalid reason
* blockhash : the block hash where the transaction is located
* blocktime : the timestamp of the block in which the transaction is located
* positioninblock : the index of the transaction in the block
* block : the block number of the transaction
* confirmations : the number of times the transaction was confirmed
* recipients: information about this airdrop receiver
* address : receiving address of this airdrop
* amount : the amount of airdrop received

#### whc_gettransaction

Description: Get information about Wormhole transactions 
Call: wormholed-cli whc_gettransaction txid 
Parameters: transaction ID 
Return value: if it is a Wormhole transaction, return its details 
Examples are as follows:
```
wormholed-cli whc_gettransaction a82b29d69538268fb67df384b9be7128456724e6fa69e4eb387944cb78ed9d66
{
  "txid": "a82b29d69538268fb67df384b9be7128456724e6fa69e4eb387944cb78ed9d66",
  "fee": "390",
  "sendingaddress": "bitcoincash:qpadl79yr4hh0fym4gw73mxp3rm4325knqe7fj6a9s",
  "ismine": false,
  "version": 0,
  "type_int": 54,
  "type": "Create Property - Manual",
  "propertyid": 166,
  "precision": "5",
  "ecosystem": "main",
  "category": "bcext repository",
  "subcategory": "gcash cashwallet cashutil neutrino",
  "propertyname": "blockchain extension development",
  "data": "for the future",
  "url": "https://github.com/bcext",
  "amount": "0.00000",
  "valid": true,
  "blockhash": "0000000000000000009179a89e5ef71fabac5404c61a11cc60a21c1d6caefb7e",
  "blocktime": 1533194387,
  "positioninblock": 1553,
  "block": 541634,
  "confirmations": 729
}
```
Return value field description
* txid : transaction ID
* fee : BCH transaction fee for the transaction
* sendingaddress : the sender of the transaction
* ismine : Whether the sending address belongs to the address in the node wallet
* version : version number of the Wormhole type
* type_int : transaction type of Wormhole
* wormholed-cli : Wormhole transaction type
* propertyid : if the transaction is a token issuance type, identify the tokenID
* precision : precision
* ecosystem : the ecosystem in which the token is located
* category : created token category
* amount : the amount of tokens created
* valid : whether the Wormhole transaction is valid
* blockhash : the block hash where the transaction is located
* blocktime : the timestamp of the block in which the transaction is located
* positioninblock : the index of the transaction in the block
* block : the block number of the transaction
* confirmations : the number of times the transaction was confirmed


#### whc_listblocktransactions
Description: return all Wormhole transactions in the specific block
Call: wormholed-cli whc_listblocktransactions blockHeight
Parameter: block number
Return value: Wormhole transaction hash list
Examples are as follows:
```
wormholed-cli whc_listblocktransactions 541634
[
  "599e5126759a98c977fce4056b14628f4e175d757d090ed11809c9f0474e6d55",
  "fa072ca373c6a38248207eab6a4e85933792628188f6ba6eb99a0fa719d8e808",
  "ec451d67689ed2d990652a013b1af87edce694f5675c11b5063c89889f5fa8ed",
  "548a10ca6c0b36bed39a78e4d47636fa16ccd3b9ba0dadb5882deb4933f83336",
  "72d266f60e6c7ddb64b5009b59bc263da4ee1be89257b1fd1625a3c674b23795",
  "a82b29d69538268fb67df384b9be7128456724e6fa69e4eb387944cb78ed9d66",
  "0956f48b0b097df6d3aa2d34acfe75a362a185fdfe32f90b5683f178558d5569"
]
```

#### whc_listpendingtransactions
Description: Query the unconfirmed Wormhole transaction in the node transaction pool
Call: wormholed-cli whc_listpendingtransactions address
Parameter: Filter address
Return value: Unconfirmed transaction information list of the specific address
Examples are as follows:
```
wormholed-cli whc_listpendingtransactions qpadl79yr4hh0fym4gw73mxp3rm4325knqe7fj6a9s
[
]
```


#### whc_listproperties

Description: List all token information in the entire system 
Call: wormholed-cli whc_listproperties 
Return value: list of all tokens in the system 
Examples are as follows:
```
wormholed-cli whc_listproperties
[
  {
    "propertyid": 1,
    "name": "WHC",
    "category": "N/A",
    "subcategory": "N/A",
    "data": "WHC serve as the binding between Bitcoin cash, smart properties and contracts created on the Wormhole.",
    "url": "http://www.wormhole.cash",
    "precision": 0
  },
  {
    "propertyid": 3,
    "name": "BFT",
    "category": "BitApp",
    "subcategory": "Blockchain",
    "data": "BitApp Founder Token",
    "url": "ht\ntps://www.bitapp.pro",
    "precision": 0
  },
  {
    "propertyid": 4,
    "name": "WHD",
    "category": "group",
    "subcategory": "coprenet",
    "data": "the mainnet token issued",
    "url": "www.wormhole.cash",
    "precision": 8
  }
]
```



#### whc_listtransactions
Description: List all transaction information on the blockchain related to the current node wallet address
Call: wormholed-cli whc_listtransactions (address, count, skip, startblock, endblock)
Parameters: all parameters of the RPC are available

* address : filtered address

* count : the number of transactions acquired

* skip : skip the first n transactions

* startblock: the block number at the beginning of the transaction

* endblock: the block number at the end of the transaction

Return value: List of transaction information
Examples are as follows:

```
wormholed-cli  whc_listtransactions qz04wg2jj75x34tge2v8w0l6r0repfcvcygv3t7sg5
[
]
```

#### whc_getfrozenbalance

Description: Return the frozen balance for the specific address and token
Call: wormholed-cli whc_getfrozenbalance address propertyid
parameter

- address : the specified address

- propertyid : tokenID

Return value: Frozen amount information of the specified token of the specified address

Examples are as follows:

```
wormholed-cli whc_getfrozenbalance qqmrktdkuj0qtu0dyef0h2xkn7u6stycuvk70k0ups 1
{
  "frozen": true,
  "balance": "1900.00000000"
}
```

Return value field description

- frozen : Shows whether the asset is frozen
- balance : frozen balance

#### whc_getfrozenbalanceforid

Description: Return the frozen balance for the specific token
Call: wormholed-cli whc_getfrozenbalanceforid propertyid
parameter

- propertyid : tokenID
Return value: Frozen amount information of the specified token
Examples are as follows:

```java
wormholed-cli whc_getfrozenbalanceforid 320
[
  {
    "address": "bchtest:qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu",
    "balance": "1900.00000000"
  }
]
```

Return value field description

- address : frozen address
- balance : frozen balance

#### whc_getfrozenbalanceforaddress

Description: Return the frozen balance for the specific address
Call: wormholed-cli whc_getfrozenbalanceforaddress address
parameter

- Address : address in CashAddr format
Return value: Frozen amount information of the specified address
Examples are as follows:

```java
wormholed-cli whc_getfrozenbalanceforaddress qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu
[
  {
    "propertyid": 320,
    "balance": "1900.00000000"
  }
]
```

Return value field description

- propertyid : frozen property id
- balance : frozen balance

#### whc_getERC721PropertyNews

Description：Get the specific ERC721 property information

Call：`wormholed-cli  whc_getERC721PropertyNews 1`

parameter：

- propertyid ：ERC721 property id

Return value: property information

Examples are as follows:

```
wormholed-cli whc_getERC721PropertyNews 1
{
  "propertyid": "1",
  "owner": "bchreg:qz5jxq9nxqj54ux2afdv670xyzdcpx75ty926hvelx",
  "creationtxid": "9e6691eb08a6e903be567b9209591b50f88d38a872fb0b9e08a3ab5285fbb1af",
  "creationblock": "63a7ff1de8d5e72c720a325681b3475fd2c0d4450c60c65d8886f48fb6ae5c2b",
  "name": "copernet",
  "symbol": "ERC",
  "data": "wormhole",
  "propertyurl": "www.wormhole.cash",
  "totalTokenNumber": 1000,
  "haveIssuedNumber": 4,
  "currentValidIssuedNumer": 2
}
```

#### whc_getERC721TokenNews

Description: Get ERC721 token information

Call：`wormholed-cli whc_getERC721TokenNews 1 1 `

parameter：

* propertyid : ERC721 property id
* tokenid : ERC721 Token id

Return value: token information

Examples are as follows:

```
wormholed-cli whc_getERC721TokenNews 1 1
{
  "propertyid": "1",
  "tokenid": "1",
  "owner": "bchreg:qrvq6ddx42a0e7mmm6ry2uqpdf2s8eg8zcp2sfjesv",
  "creationtxid": "e99b1b419d7cf827d84b9aef99a9aea53420cd54babe25a440c2a1beb382d241",
  "creationblock": "4c472fd84343ff63da24d8e34e19bba5c3d891ec2cebff9b1fe25a242f7fa584",
  "attribute": "0000000000000000000000000000000000000000000000000000000000023567",
  "tokenurl": "www.wormhole.cash"
}
```

#### whc_getERC721AddressTokens

Description：Get the ERC721 Token under the specified address and property

Call：`wormholed-cli whc_getERC721AddressTokens address propertyid `

parameter：

* address: user address
* propertyid : property id

Return value：token list

Examples are as follows:

```
wormholed-cli whc_getERC721AddressTokens bchreg:qrvq6ddx42a0e7mmm6ry2uqpdf2s8eg8zcp2sfjesv 1
[
  {
    "tokenid": "12",
    "attribute": "0000000000000000000000000000000000000000000000000000000000023567",
    "tokenurl": "www.wormhole.cash",
    "creationtxid": "e99b1b419d7cf827d84b9aef99a9aea53420cd54babe25a440c2a1beb382d241"
  }
]
```

####whc_getERC721PropertyDestroyTokens

Description：Get destroyed ERC721 tokens under the specific property 

Call：`wormholed-cli whc_getERC721PropertyDestroyTokens 1`

parameter： 

* propertyid : ERC721 property id

Return value：destroyed token list

Examples are as follows:

```
wormholed-cli whc_getERC721PropertyDestroyTokens 1
[
  {
    "tokenid": "2",
    "attribute": "0000000000000000000000000000000000000000000000000000000000023567",
    "tokenurl": "www.wormhole.cash",
    "creationtxid": "20f114826983e2b3216020e503115bf345fcdee4596e0b0a1079bfd84ff7c927"
  },
  {
    "tokenid": "3",
    "attribute": "0000000000000000000000000000000000000000000000000000000000000323",
    "tokenurl": "www.copernet.com",
    "creationtxid": "f65670326132ee343d0fd6b2ed1fe1d097dd72b33cbe041837ded9f939487cd9"
  }
]
```



### whc_ownerOfERC721Token  

Description：Query whether the Token's owner is the specified address

Call：`wormholed-cli whc_ownerOfERC721Token 1 1 address`

parameter：

- propertyid : ERC721 property id
- tokenid : ERC721 Token id
- address : query address

Return value：Whether the special ERC721 Token is owned or not by the address；true : own; false: don't own

Examples are as follows:

```
wormholed-cli  whc_ownerOfERC721Token 1 1 qpekwx8g5n4xjgrkpnw5d4lhrgywr9jrngn8r8cvh2
{
  "own": false
}
```



### whc_listERC721PropertyTokens 

 Description:  Lists all ERC721Token that specify the ERC721 property issuance

Call:  `wormholed-cli  whc_listERC721PropertyTokens 1`

parameter:

- propertyid : ERC721 property id

Return value：Lists all ERC721Token that specify the ERC721 property issuance

Examples are as follows:

```
wormholed-cli whc_listERC721PropertyTokens  1
[
  {
    "tokenid": "1",
    "owner": "bchtest:qz04wg2jj75x34tge2v8w0l6r0repfcvcygv3t7sg5"
  },
  {
    "tokenid": "2",
    "owner": "bchtest:qz04wg2jj75x34tge2v8w0l6r0repfcvcygv3t7sg5"
  },
  {
    "tokenid": "3",
    "owner": "bchtest:qqqqqqqqqqqqqqqqqqqqqqqqqqqqqdmwgvnjkt8whc"
  }
]
```

​	

#### Create transaction

The following two solutions are used to create a transaction.
Solution 1: Call the RPC in Table 2 to create a Wormhole transaction directly; this series of RPC calls have the following requirements for the node:

* Require the Wormhole node to have a wallet that can be used
* There must be enough BCH and WHC in the wallet for creating transactions
* The advantage: the calling API is simple and can be used directly by users in the node
* The disadvantage: there must be a wallet that has BCH and WHC in the current node.

Solution 2: Create Wormhole transactions through the combined calling of Table 3 and Table 4 of RPC

* This calling scheme can be used to develop applications such as wallets, by calling the server-side RPC service, it will generate unsigned transactions; then the wallet will sign and send the signed transaction.
* The advantage: do not require the calling node to have a wallet, the loss of the token is not a problem; allow offline signature, and broadcasting of transactions
* The disadvantage: the calling process is a complicated

### Table 2：Create Wormhole transaction

RPC	|feature
---|----
whc_burnbchgetwhc|	Burn BCH，get WHC|
whc_sendissuancefixed|	Issue fixed token|
whc_sendissuancemanaged	|Issue manageable token|
whc_sendissuancecrowdsale|	Issue crowdfunding token|
whc_particrowsale|	Participate in crowdfunding|
whc_sendclosecrowdsale|	Close crowdfunding|
whc_sendgrant|	Issue additional amount of manageable token|
whc_sendrevoke|	Destroy amount of manageable token|
whc_send|	transfer|
whc_sendsto|	airdrop|
whc_sendall	|Send all tokens in a specific address to another address|
whc_sendchangeissuer|	Change token issuer|
whc_sendfreeze | Freeze specific token in a specific address|
whc_sendunfreeze |  Unfreeze specific token in a specific address|
whc_issuanceERC721property | Issue ERC721 property |
whc_issuanceERC721Token | Issue ERC721 token under specific property |
whc_transferERC721Token | Transfer specific ERC721 token |
whc_destroyERC721Token | Revoke specific ERC721 token |

#### whc_burnbchgetwhc

Description: Burn BCH, get WHC 
Call: wormholed-cli whc_burnbchgetwhc "amount" (redeemaddress) 
parameter 
* amount : the amount of BCH burned 
* redeemaddress: redundant BCH redemption address; optional: the default wallet generates a new address. 

Return value: generate transaction hash 
Examples are as follows:
```
wormholed-cli whc_burnbchgetwhc 1.5

153438e063d2533f6337d86b9ab7494cf907c4e927c3cef7a9358504cb049cf6
```

#### whc_sendissuancefixed
Description: Issue a fixed attribute token
Call: wormholed-cli whc_sendissuancefixed "fromaddress" ecosystem precision previousid "category" "subcategory" "name" "url" "data" "totalNumber"

Parameter:
* fromaddress : the address that issued the fixed attribute token 
* ecosystem :token ecosystem; currently must be 1
* precision : the precision of the token
* previousid : the tokenID that being added value; currently must be 0
* category :token category
* subcategory : subcategory of token
* name : the name of the token
* url : token URL
* data: custom description data for token
* totalNumber : the amount of issued tokens

Return value: generated transaction hash 

Examples are as follows:
```
wormholed-cli whc_sendissuancefixed  qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 1 3 0 "company" "compute" "luzhiyao" "www.ludete.com" "hello world" 10082936279.232

1d059313c873018a2f0dfe855ba1dde0ce19ec50db51eca42236baa1b8c8d6f4
```

#### whc_sendissuancemanaged

Description: Issue manageable tokens

Call: wormholed-cli whc_sendissuancemanaged "fromaddress" ecosystem precision previousid "category" "subcategory" "name" "url" "data"

parameter

* fromaddress : the address from which the manageable token is issued
* ecosystem :token ecosystem; currently must be 1
* precision : the precision of the token
* previousid : the tokenID that being added value; currently must be 0
* category :token category
* subcategory : subcategory of token
* name : the name of the token
* url :token URL
* data: custom description data for token

Return value: generated transaction hash

Examples are as follows:
```
wormholed-cli whc_sendissuancemanaged  qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 1 3 0 "company" "compute" "luzhiyao" "www.ludete.com" "hello world"

ae878af640344f3c8fae85ed4d37eb0f2a77a2553a0cb7645ff7c92d23d89768
```
#### whc_sendissuancecrowdsale

Description: Issue a crowdfunding token

Call: wormholed-cli whc_sendissuancecrowdsale "fromaddress" ecosystem precision previousid "category" "subcategory" "name" "url" "data" propertyiddesired tokensperunit deadline earlybonus undefine totalNumber

parameter
* fromaddress : the address from which the manageable token is issued
* ecosystem :token ecosystem; currently must be 1
* precision : the precision of the token
* previousid : the tokenID that being added value; currently must be 0
* category :token category
* subcategory : subcategory of token
* name : the name of the token
* url :token URL
* custom description data for data :token
* propertyiddesired : the tokenID attending the fundraising
* tokensperunit: the single unit price of the crowdfunding token; 1WHC is equal to a certain ratio of tokens, the range of unit price [10-8, 108]
* deadline : deadline for crowdfunding
* earlybonus: early bird reward
* undefine : undefined field, must be 0
* totalNumber : the amount of crowdfunding issuance

Return value: generated transaction hash 

Examples are as follows:
```
wormholed-cli whc_sendissuancecrowdsale  qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 1 3 0 "company" "compute" "luzhiyao" "www.ludete.com" "hello world" 1 1002 1536565622 1 0 21231242131

e7d2c232a91a3c1855cbfed05bf75e31041676898b37fa93420308cb3ff7a666
```
#### whc_particrowsale
Description: Participate in crowdfunding

Call: wormholed-cli whc_particrowsale "fromaddress" "toaddress" "amount" ( "redeemaddress" "referenceamount" )

parameter
* fromaddress : the address to participate in crowdfunding
* toaddress : the address of the crowdfunding issuer
* amount : the amount of WHC participating in crowdfunding
* redeemaddress : the address for redeeming the extra BCH; optional, the default is the address that participates in crowdfunding
* referenceamount : the amount of BCH given to the crowdfunding issuer; optional, the default is the minimum amount of the system

Return value: generated transaction hash

Examples are as follows:
```
wormholed-cli whc_particrowsale qpalmy832fp9ytdlx444sehajljnm554dulckcvjl5  qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 100

d0a21fd05ed0e2f23f594d77b8ecec96a94ad1dec2785b658e4f705504766cda
```
#### whc_sendclosecrowdsale

Description: Turn off crowdfunding

Call: wormholed-cli whc_sendclosecrowdsale "fromaddress" propertyid

parameter:
* fromaddress : the address of the crowdfunding issuer
* propertyid : the tokenID of the crowdfunding

Return value: generated transaction hash

Examples are as follows:
```
wormholed-cli whc_sendclosecrowdsale qpalmy832fp9ytdlx444sehajljnm554dulckcvjl5 11

967328ba4b60876e0b1039e7e4ac77c2d7678ac98e968599786a68df18f353cf
```
#### whc_sendgrant

Description: The amount of additional issued manageable tokens

Call: wormholed-cli whc_sendgrant "fromaddress" "toaddress" propertyid "amount" ( "memo" )

parameter:

* fromaddress :token issuer address
* toaddress : add specific amount of tokens to this address
* propertyid : manageable tokenID
* amount : the amount of additional issued tokens
* memo: custom information for additional issued tokens

Return value: generated transaction hash

Examples are as follows：
```
wormholed-cli whc_sendgrant qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq qpalmy832fp9ytdlx444sehajljnm554dulckcvjl5 115 1242

ed76f90ef3950cac5198045a009483dc90d3ce8a4c8d491d86127b1b3f55a555
```


#### whc_sendrevoke

Description: The number of destroyed management tokens

Call: wormholed-cli whc_sendrevoke "fromaddress" propertyid "amount" ( "memo" )

parameter:
* fromaddress :token issuer address
* propertyid : manageable tokenID
* amount : the number of tokens destroyed
* memo: custom information of the destroyed token

Return value: generated transaction hash

Examples are as follows:
```
wormholed-cli whc_sendrevoke qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 115 100

91c56172524fe11fabb7f954cf893a7c42be31e79f549031d11b89a5ea7d4581
```

#### whc_send

Description: Transfer

Call: wormholed-cli whc_send "fromaddress" "toaddress" propertyid "amount" ( "redeemaddress" "referenceamount" )

parameter:
* fromaddress : the sender address of the token
* toaddress : the recipient address of the token
* propertyid : the tokenID of the transfer
* amount : the amount of tokens transferred
* redeemaddress : the BCH redeem address; optional, the default is the address of the token sender
* referenceamount :token the amount of BCH output by the receiver; optional, the default is the minimum amount of the system

Return value: generated transaction hash

Examples are as follows:
```
wormholed-cli whc_send qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq qpalmy832fp9ytdlx444sehajljnm554dulckcvjl5 115 100

d87ae34ed64e23087228eba458af1ebaf94f0db04912c59f6531f2b8c5c72f91
```

#### whc_sendsto

Description: Airdrop

Call: wormholed-cli whc_sendsto "fromaddress" propertyid "amount" ( "redeemaddress" distributionproperty )

parameter:
* fromaddress : the sender address of the airdrop
* propertyid : the tokenID of the airdrop
* amount : the amount of tokens being airdropped
* redeemaddress : the BCH redeem  address; optional, the default is the address of the token sender
* distributionproperty : the target tokenID that receives the airdrop; optional, the default is the ID that airdrop tokens

Return value: generated transaction hash

Examples are as follows：
```
wormholed-cli whc_sendsto  qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 115 1000 qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 1

bf3d30fc9c9424bdc6e38fc55320bad6cda9488e74296fc8dfb06cb2d9ee0fd9
```

#### whc_sendall

Description: Send all tokens of the specific address to another address

Call: wormholed-cli whc_sendall "fromaddress" "toaddress" ecosystem ( "redeemaddress" "referenceamount" )

parameter:
* fromaddress :token sender address
* toaddress :token recipient address
* ecosystem: ecosystem; currently must be 1
* redeemaddress : the BCH redeem address; optional, the default is the address of the token sender
* referenceamount : the amount of BCH output by the token receiver; optional, the default is the minimum amount of the system

Return value: generated transaction hash
Examples are as follows:
```
wormholed-cli whc_sendall qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq qpalmy832fp9ytdlx444sehajljnm554dulckcvjl5 1

cb93fbe852955201b757a790a73bb964728dd4309a449b2e46e67c9f69292909
```
#### whc_sendchangeissuer

Description: Change the issuer of the token

Call: wormholed-cli whc_sendchangeissuer "fromaddress" "toaddress" propertyid

parameter:
* fromaddress :token issuer
* toaddress : the modified issuer
* propertyid : tokenID

Return value: generated transaction hash

Examples are as follows:
```
wormholed-cli whc_sendchangeissuer qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq qpalmy832fp9ytdlx444sehajljnm554dulckcvjl5 115

d1fb2ee670e3489e80f9fbfbd9e001dfb4ed64d5107354e7b74ceb0398625fb1

```

#### whc_sendfreeze

Description: Freeze the token of a address

Call: wormholed-cli  whc_sendfreeze  "fromaddress"  propertyid  "amount" "frozenaddress"

parameter:

- fromaddress :token issuer
- propertyid : tokenID
- amount：token amount, not available for now
- frozenaddress : the  address to be frozen

Return value: generated transaction hash

Examples are as follows:

```java
wormholed-cli whc_sendfreeze qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q 320 "100.0" qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu
45a9e5702c7d5c9836d77a5f571059a2b50fe32e39bf7bf0c0d7951392cb4e0b
```

#### whc_sendunfreeze

Description: Unfreeze the token of a address

Call: wormholed-cli  whc_sendunfreeze  "fromaddress"  propertyid  "amount" "frozenaddress"

parameter:

- fromaddress :token issuer
- propertyid : tokenID
- amount：token amount, not available for now
- frozenaddress : the  address to be unfrozen

Return value: generated transaction hash

Examples are as follows:

```java
wormholed-cli whc_sendunfreeze qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q 320 "100.0" qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu
4d7e239fbc1a71ce7b27ae7b6bc4c557131973505f0d1701377d0302177390f9
```

#### whc_issuanceERC721property  

Description: Issue ERC721 property

Call：`wormholed-cli whc_issuanceERC721property "issueAddress" "name" "symbol" "data" "url" totalNumber`

Parameter：

* issueAddress: issuer of property
* name: propertye name
* symbol: property symbol
* data: property information
* url:  property url

Return value：transaction hash

Examples are as follows:

```
wormholed-cli whc_issuanceERC721property qz5jxq9nxqj54ux2afdv670xyzdcpx75ty926hvelx s s s s 888
8fa8ce4b2c0d45ffa3c280311d4e1fb1dfde061ef5272e73fa0366e79615f532
```

#### whc_issuanceERC721Token  

Description: Issue ERC721 token under the specific property

Call：`wormholed-cli whc_issuanceERC721Token "issueAddress" "receiveaddress" "propertyID" "tokenID" "tokenAttributes" "tokenURL"`

Parameter:

* issueAddress: issuer address
* receiveaddress: receiver address
* propertyID: ERC721 property ID (Note: This field be a decimal string)
* tokenID: ERC721 Token ID , optional. (Note: This field be a decimal string)
* tokenAttributes: Token attributes (Note: This field must be a hexadecimal string)
* tokenURL: Token url 

Return value: transaction hash

Examples are as follows:

```
wormholed-cli whc_issuanceERC721Token bchreg:qz5jxq9nxqj54ux2afdv670xyzdcpx75ty926hvelx bchreg:qrvq6ddx42a0e7mmm6ry2uqpdf2s8eg8zcp2sfjesv 1 1 0x023567 www.wormhole.cash
e99b1b419d7cf827d84b9aef99a9aea53420cd54babe25a440c2a1beb382d241
```

#### whc_transferERC721Token  

Description: transfer specific ERC721 token

Call：`wormholed-cli whc_transferERC721Token "ownerAddress" "receiveaddress" 1 1 `

Parameter:

* ownerAddress: Token owner
* receiveaddress: Token receiver
* propertyID: ERC721 property id
* tokenID: ERC721 Token id

Return value: transaction hash

Examples are as follows:

```
wormholed-cli whc_transferERC721Token bchreg:qz5jxq9nxqj54ux2afdv670xyzdcpx75ty926hvelx  bchreg:qraufn9sah7jecdv2xfcmjjrdj8u8l5f3ghvjmsncc 1 1 
e0bdaf619da70d23ec042428b797448bd9fd3674b0a5db926ee008956711c161
```

#### whc_destroyERC721Token  

Description: revoke specific ERC721 token 

Call: `wormholed-cli whc_destroyERC721Token "ownerAddress" 1 1`

Parametor: 

* ownerAddress: ERC721 Token owner
* propertyID: ERC721property id
* tokenID: ERC721 token id

Return value: transaction hash

Examples are as follows:

```
 wormholed-cli whc_destroyERC721Token "qqzy3s0ueaxkf8hcffhtgkgew8c7f7g85um9a2g74r" "1" "2"
 33d27bca6703c0f837e8f51c8ebb3985d36f899349e84bccb78bff852a308c2b
```

### Table 3 ：Create Wormhole protocol payload data

RPC	|feature
---|----
whc_createpayload_burnbch|	Burn BCH, get WHC|
whc_createpayload_issuancefixed	|Issue fixed token|
whc_createpayload_issuancemanaged|	Issue manageable token|
whc_createpayload_issuancecrowdsale|	Issue crowdfunding token|
whc_createpayload_particrowdsale|	Participate crowdfunding|
whc_createpayload_closecrowdsale|	Close crowdfunding|
whc_createpayload_grant|	Additionally issued amount of manageable token|
whc_createpayload_revoke|	Destroyed amount of manageable token|
whc_createpayload_simplesend|	transfer|
whc_createpayload_sto|	airdrop|
whc_createpayload_sendall|	Send all tokens from a specific address to another address|
whc_createpayload_changeissuer|	Change the issuer of token|
whc_createpayload_freeze | Freeze the token of a address|
whc_createpayload_unfreeze | Unfreeze the token of a address|
whc_createpayload_issueERC721property | Create ERC721 property |
whc_createpayload_issueERC721token | Create ERC721 token |
whc_createpayload_transferERC721token | Transfer ERC721 token |
whc_createpayload_destroyERC721token | Revoke ERC721 token |

#### whc_createpayload_burnbch

Description : burn BCH, get WHC

Transfer：wormholed-cli whc_createpayload_burnbch

Return value: created wormhole protocol payload data

Example is as follows:
```
wormholed-cli whc_createpayload_burnbch

00000044
```
#### whc_createpayload_issuancefixed

Description: issue fixed token

Transfer：wormholed-cli whc_createpayload_issuancefixed ecosystem precision previousid "category" "subcategory" "name" "url" "data" "totalNumber"

parameter
* Ecosystem: token ecosystem; currently must be 1 
* precision: precision of token 
* previousid: value attached tokenID; currently must be 0
* category: category of token 
* subcategory: subcategory of token 
* name: name of token 
* url: URL of token URL
* data: self-defined token description data 
* totalNumber: number of token issued

Return value: Created wormhole protocol payload data

Example is as follows
```
wormholed-cli whc_createpayload_issuancefixed  1 3 0 "company" "compute" "luzhiyao" "www.ludete.com" "hello world" 10082936279.232

0000003201000300000000636f6d70616e7900636f6d70757465006c757a686979616f007777772e6c75646574652e636f6d0068656c6c6f20776f726c64000000092b9dd5d0c0
```
#### whc_createpayload_issuancemanaged

Description:  Issue manageable token

Transfer：wormholed-cli whc_createpayload_issuancemanaged ecosystem precision previousid "category" "subcategory" "name" "url" "data"
parameter
* ecosystem: ecosystem of token; currently must be 1
* precision: precision of token 
* previousid : value attached tokenID; currently must be 0
* category: token category 
* subcategory: token subcategory
* name: token name 
* url: token URL
* data: self-defined token memo 
* Return value：the hex-encoded payload

Example is as follows:
```
wormholed-cli whc_createpayload_issuancemanaged   1 3 0 "company" "compute" "luzhiyao" "www.ludete.com" "hello world"

0000003601000300000000636f6d70616e7900636f6d70757465006c757a686979616f007777772e6c75646574652e636f6d0068656c6c6f20776f726c6400
```
#### whc_createpayload_issuancecrowdsale

Description: issue crowdfunding token

Transfer：wormholed-cli whc_createpayload_issuancecrowdsale ecosystem precision previousid "category" "subcategory" "name" "url" "data" propertyiddesired tokensperunit deadline earlybonus undefine totalNumber

parameter
* Ecosystem: token ecosystem; currently must be 1 
* Precision: token precision 
* Previousid: value attached tokenID; currently must be 0
* Category: token category 
* subcategory: token subcategory 
* name: token name
* url: token URL
* data: token self-defined description data 
* propertyiddesired: crowd funded tokenID
* tokensperunit: token price per unit; 1=how many token, price range of per token [10-8, 108]
* deadline: crowdfunding deadline 
* earlybonus: early bird bonus 
* undefined: undefined unit, must be 0 
* totalNumber: total number of token 

Return value：the hex-encoded payload

Example is as follows:
```
wormholed-cli whc_createpayload_issuancecrowdsale   1 3 0 "company" "compute" "luzhiyao" "www.ludete.com" "hello world" 1 1002 1536565622 1 0 21231242131

0000003301000300000000636f6d70616e7900636f6d70757465006c757a686979616f007777772e6c75646574652e636f6d0068656c6c6f20776f726c640000000001000000175462aa00000000005b96217601000000134f48a53638
```
#### whc_createpayload_particrowdsale

Description : participate crowdfunding

Transfer：wormholed-cli whc_createpayload_particrowdsale "amount"

parameter
*	amount: amount of WHC for crowdfunding

Return value：the hex-encoded payload

Example is as follows:
```
wormholed-cli whc_createpayload_particrowdsale    100

000000010000000100000002540be400
```
#### whc_createpayload_closecrowdsale

Description: close crowdfunding

Transfer：wormholed-cli whc_createpayload_closecrowdsale propertyid

parameter：
*	propertyid: crowdfunding tokenID

Return value：the hex-encoded payload

Example is as follows:
```
wormholed-cli whc_createpayload_closecrowdsale  11

000000350000000b
```
#### whc_createpayload_grant

Description: amount of additionally issued manageable token 

Transfer：wormholed-cli whc_createpayload_grant propertyid "amount" ( "memo" )

parameter：
*	propertyid: manageable token ID
*	amount: additionally issued token amount  
*	memo: additionally issued token memo

Return value：the hex-encoded payload

Example is as follows:
```
wormholed-cli whc_createpayload_grant  115 1242

0000003700000073000000000012f39000
```
#### whc_createpayload_revoke

Description: revoked manageable token amount

Transfer：wormholed-cli whc_createpayload_revoke propertyid "amount" ( "memo" )

parameter：
*	propertyid: manageable token ID
*	amount: destroyed token amount 
*	memo: destroyed token memo

Return value：the hex-encoded payload

Example is as follows:
```
wormholed-cli whc_createpayload_revoke  115 100

000000380000007300000000000186a000
```
#### whc_createpayload_simplesend

Description: transfer

Transfer：wormholed-cli whc_createpayload_simplesend propertyid "amount"

parameter：
* fromaddress： token’s sender address
* toaddress：toke’s receiver address 
* propertyid: transfer tokenID
* amount: transfer token amount
* redeemaddress: BCH redeem address; optional, the default is the address of token sender 
* referenceamount: BCH amount of token receiver; optional, the default is the  minimum amount in the system
* Return value：the hex-encoded payload

Example is as follows
```
wormholed-cli whc_createpayload_simplesend   115 100

000000000000007300000000000186a0
```

#### whc_createpayload_sto

Description: airdrop

Transfer：wormholed-cli whc_createpayload_sto propertyid "amount" distributionpropertyß

parameter：

* Fromaddress: airdrop token sender address 
* Propertyid: airdrop property token ID
* Amount: airdrop token amount 
* Redeemaddress: BCH redeem address; optional, the default is the token sender address 
* Distributionproperty: airdrop target token ID; optional, the default is the airdrop token ID
* Return value：the hex-encoded payload

Example is as follows
```
wormholed-cli whc_createpayload_sto   115 1000  1

000000030000007300000000000f424000000001
```
#### whc_createpayload_sendall

Description: send all token from a specific address to another address

Transfer：wormholed-cli whc_createpayload_sendall ecosystem

parameter：
* Ecosystem: ecosystem, currently must be 1
* Redeemaddress: BCH redeem address; optional, the default is the token sender address 
* Referenceamount: token receiver BCH amount; optional, the default is the minimum amount of the system

Return value：the hex-encoded payload

Example is as follows
```
wormholed-cli whc_createpayload_sendall   1

0000000401
```
#### whc_createpayload_changeissuer

Description: change token issuer

Transfer：wormholed-cli whc_createpayload_changeissuer propertyid

parameter：
* Fromaddress: token sender 
* Toaddress: sender after change 
* Propertyid:tokenID

Return value：the hex-encoded payload

Example is as follows
```
wormholed-cli  whc_createpayload_changeissuer   115

0000004600000073

```

#### whc_createpayload_freeze

Description: Freeze the token of address

Transfer：wormholed-cli whc_createpayload_freeze "toaddress"  propertyid  "amount" 

parameter：

- toaddress: the address to be frozen
- propertyid: tokenID
- amount: token amount, not available for now

Return value：the hex-encoded payload

Example is as follows

```java
wormholed-cli whc_createpayload_freeze qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu 320 "100"
000000b90000014000000002540be400626368746573743a7171706a307975387739756b6737783468383378783761346e6a3866376d7373683564676e36666c667500
```

#### whc_createpayload_unfreeze

Description: Unfreeze the token of address

Transfer：wormholed-cli whc_createpayload_unfreeze "toaddress"  propertyid  "amount" 

parameter：

- toaddress: the address to be unfrozen
- propertyid: tokenID
- amount: token amount, not available for now

Return value：the hex-encoded payload

Example is as follows

```java
wormholed-cli whc_createpayload_unfreeze qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu 320 "100"
000000ba0000014000000002540be400626368746573743a7171706a307975387739756b6737783468383378783761346e6a3866376d7373683564676e36666c667500
```

#### whc_createpayload_issueERC721property 

Description: create ERC721 property

Call: `wormholed-cli whc_createpayload_issueERC721property "name" "symbol" "data" "www.ludete.com"  89977 `

Parameter: 

* name: property name
* symbol: property symbol
* data: property information 
* url: property url

Return value: the hex-encoded payload

Example is as follows

```
wormholed-cli whc_createpayload_issueERC721property "name" "symbol" "data" "www.ludete.com"  89977
00000009016e616d650073796d626f6c0064617461007777772e6c75646574652e636f6d000000000000015f79
```

####whc_createpayload_issueERC721token 

Description: create ERC721 token

Call: `wormholed-cli whc_createpayload_issueERC721token 1 2 0x0234 "www.ludete.com"`

Parameter:

* propertyid : ERC721 property id
* tokenid : ERC721 token id
* tokenAttributes : Token attributes
* tokenURL : Token url

Return value: the hex-encoded payload

Example is as follows

```
wormholed-cli whc_createpayload_issueERC721token 1 2 0x0234 "www.ludete.com"
00000009020100000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000034020000000000000000000000000000000000000000000000000000000000007777772e6c75646574652e636f6d00
```

####whc_createpayload_transferERC721token

Description: Transfer ERC721 token

Call: `wormholed-cli whc_createpayload_transferERC721token 1 1` 

Parameter:

* propertyid : ERC721 property id
* tokenid : ERC721 token id 

Return value: the hex-encoded payload

Example is as follows

```
wormholed-cli whc_createpayload_transferERC721token 1 1
000000090301000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000
```

####whc_createpayload_destroyERC721token

Description: Revoke ERC721 token

Call：`wormholed-cli whc_createpayload_destroyERC721token 1 1 `

Parameter:

* propertyid: ERC721 property id
* tokenid: ERC721 token id 

Return value: the hex-encoded payload

Example is as follows

```
wormholed-cli whc_createpayload_destroyERC721token 1 1
000000090401000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000
```

### Table 4: Create Transaction 

RPC|feature
---|----
whc_createrawtx_input|	Append a transaction input to an unsigned transaction|
whc_createrawtx_opreturn|	Append the payload data of the Wormhole protocol as a new output script to the unsigned transaction|
whc_createrawtx_reference|	Append a transaction output to an unsigned transaction|
whc_createrawtx_change|	Append a transaction output to the specified location of the unsigned transaction output collection|
whc_decodetransaction	|Parse the original transaction of the Wormhole|

#### whc_createrawtx_input

Description: Append a transaction output to an unsigned transaction 

Transfer：wormholed-cli whc_createrawtx_input "rawtx" "txid" c

Parameter：
* Rawtx: unsigned transaction, can be NULL 
* Txid: transaction ID
* Txid: To spend the output index of the transaction (txid) 

Return Value: add after transaction data after transaction 

Example is as follows
```
wormholed-cli  whc_createrawtx_input   "" "d1fb2ee670e3489e80f9fbfbd9e001dfb4ed64d5107354e7b74ceb0398625fb1" 2

0200000001b15f629803eb4cb7e7547310d564edb4df01e0d9fbfbf9809e48e370e62efbd10200000000ffffffff0000000000
```
#### whc_createrawtx_opreturn

Description: Append the payload data of the Wormhole protocol as a new output script to the unsigned transaction

Transfer：wormholed-cli wormhole "rawtx" "payload"

Parameter：
* Rawtx: Unsigned transaction, can be NULL 
* Payload: payload data of the Wormhole protocol

Return Value: add after transaction data after transaction 

Example is as follows
```
wormholed-cli whc_createrawtx_opreturn 0200000001b15f629803eb4cb7e7547310d564edb4df01e0d9fbfbf9809e48e370e62efbd10200000000ffffffff0000000000 0000004600000073

0200000001b15f629803eb4cb7e7547310d564edb4df01e0d9fbfbf9809e48e370e62efbd10200000000ffffffff0100000000000000000e6a0c08776863000000460000007300000000
```
#### whc_createrawtx_reference

Description: Append a transaction output to an unsigned transaction 

Transfer：wormholed-cli whc_createrawtx_reference "rawtx" "destination" ( amount )

Parameter：
* rawtx: unsigned transaction, can be NULL
* destination: The destination address where the output will be added：
* amount: The amount of the output; optional, the default is the minimum amount of the system 

Return Value: add after transaction data after transaction 

Example is as follows
```
wormholed-cli whc_createrawtx_reference 0200000001b15f629803eb4cb7e7547310d564edb4df01e0d9fbfbf9809e48e370e62efbd10200000000ffffffff0100000000000000000e6a0c08776863000000460000007300000000 qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 1.24

0200000001b15f629803eb4cb7e7547310d564edb4df01e0d9fbfbf9809e48e370e62efbd10200000000ffffffff0200000000000000000e6a0c08776863000000460000007300176407000000001976a9149e763b620e844d5e926206ba534d2a4b979fb15188ac00000000
```
#### whc_createrawtx_change

Description: Append a transaction output to the specified location of the unsigned transaction output collection

Transfer：wormholed-cli whc_createrawtx_change "rawtx" "prevtxs" "destination" fee ( position )

Parameter：
* rawtx: unsigned transaction, can be NULL 
* prevtxs: Reference input set for unsigned transactions 
* destination: The destination address where the output will be added 
* fee: Transaction fee 
* position: Append this transaction output to the specified location of the output collection; optionally, the default is appended to the position where the index is 0.

Return Value: add after transaction data after transaction 

Example is as follows
```
wormholed-cli whc_createrawtx_change "0100000001b15ee60431ef57ec682790dec5a3c0d83a0c360633ea8308fbf6d5fc10a779670400000000ffffffff025c0d00000000000047512102f3e471222bb57a7d416c82bf81c627bfcd2bdc47f36e763ae69935bba4601ece21021580b888ff56feb27f17f08802ebed26258c23697d6a462d43fc13b565fda2dd52aeaa0a0000000000001976a914946cb2e08075bcbaf157e47bcb67eb2b2339d24288ac00000000" "[{\"txid\":\"6779a710fcd5f6fb0883ea3306360c3ad8c0a3c5de902768ec57ef3104e65eb1\",\"vout\":4,\"scriptPubKey\":\"76a9147b25205fd98d462880a3e5b0541235831ae959e588ac\",\"value\":0.00068257}]" "qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq" 0.00003500 1

0100000001b15ee60431ef57ec682790dec5a3c0d83a0c360633ea8308fbf6d5fc10a779670400000000ffffffff035c0d00000000000047512102f3e471222bb57a7d416c82bf81c627bfcd2bdc47f36e763ae69935bba4601ece21021580b888ff56feb27f17f08802ebed26258c23697d6a462d43fc13b565fda2dd52aeefe40000000000001976a9149e763b620e844d5e926206ba534d2a4b979fb15188acaa0a0000000000001976a914946cb2e08075bcbaf157e47bcb67eb2b2339d24288ac00000000
```

## RPC combinations of Table 3, 4 

### Get WHC by burning BCH
1. Add input：   `wormholed-cli whc_createrawtx_input `
2. Add output, transfer the WHC to burning address： `wormholed-cli whc_createrawtx_reference`
3. create payload data：  `wormholed-cli whc_createpayload_burnbch`
4. add output，add the created wormhole payload data to the transaction output： `wormholed-cli whc_createrawtx_opreturn`
5. add output to receive changes： `wormholed-cli whc_createrawtx_reference`  (This step can be omitted, the extra amount will be used as transaction fee)
6. Sign the transaction created：`wormholed-cli signrawtransaction`
7. Send：`wormholed-cli sendrawtransaction`

Example is as follows
```
1. wormholed-cli whc_createrawtx_input "" cda3e90e9ad1cd73ef793263d4b38a2ff6b80c149c04b7faf5540aac35d837d4 2
return value：0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0000000000
2. wormholed-cli whc_createrawtx_reference "0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0000000000" "bchtest:qqqqqqqqqqqqqqqqqqqqqqqqqqqqqdmwgvnjkt8whc" 2
return value：0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0100c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000
3. wormholed-cli whc_createpayload_burnbch
return value：00000044
4. wormholed-cli whc_createrawtx_opreturn "0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0100c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000" "00000044"
return value：0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0200c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000000000000a6a08087768630000004400000000
5. wormholed-cli whc_createrawtx_reference "0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0200c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000000000000a6a08087768630000004400000000" "qpnprg0h9y8ts3p9257f3sfe7j040yemqql84kh26q" 2.999
return value：0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0300c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000000000000a6a080877686300000044601ce011000000001976a9146611a1f7290eb84425553c98c139f49f57933b0088ac00000000
6. wormholed-cli signrawtransaction 0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0300c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000000000000a6a080877686300000044601ce011000000001976a9146611a1f7290eb84425553c98c139f49f57933b0088ac00000000
return value：
{
  "hex": "0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd020000006b483045022100a70f30364283c1382d179f85a1f2b5a0bc26e8d11d6ccbd9dce4bde02bb6fc3e02202d3876e4db506de74bc387e7fa6f57e6d8f84188f0e6e3ffa1ee2656d5213104412102cfdb34fee8eb0f17e5fe731094036327e645803050797620f46fc718dc5479d3ffffffff0300c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000000000000a6a080877686300000044601ce011000000001976a9146611a1f7290eb84425553c98c139f49f57933b0088ac00000000",
  "complete": true
}
7. wormholed-cli sendrawtransaction 0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd020000006b483045022100a70f30364283c1382d179f85a1f2b5a0bc26e8d11d6ccbd9dce4bde02bb6fc3e02202d3876e4db506de74bc387e7fa6f57e6d8f84188f0e6e3ffa1ee2656d5213104412102cfdb34fee8eb0f17e5fe731094036327e645803050797620f46fc718dc5479d3ffffffff0300c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000000000000a6a080877686300000044601ce011000000001976a9146611a1f7290eb84425553c98c139f49f57933b0088ac00000000
return value：2b932bf1e73a31e87ce30be3a4f86b9d68beb9e61e9badf399474b95c32180eb

```

### Transfer
1. Add input:  `wormholed-cli whc_createrawtx_input `
2. Create payload data：    `wormholed-cli whc_createpayload_simplesend`
3. Add output, add the created wormhole payload data to the transaction output:  `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes: `wormholed-cli whc_createrawtx_reference`
5. Add output to receive the token: `wormholed-cli whc_createrawtx_reference`
6. Sign the transaction：`wormholed-cli signrawtransaction`
7. Send: `wormholed-cli sendrawtransaction`

### Transfer all tokens of the specific address
1. Add input:  `wormholed-cli whc_createrawtx_input `
2. Create payload data:  `wormholed-cli whc_createpayload_sendall`
3. Add output, add the created wormhole payload data to the transaction output:   `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes:`wormholed-cli whc_createrawtx_reference`（这步可以省略）
5. Add output to receive the token:`wormholed-cli whc_createrawtx_reference`
6. Sign the transaction：`wormholed-cli signrawtransaction`
7. Send: `wormholed-cli sendrawtransaction`

### Create property which has a fixed number of token 
1. Add input: `wormholed-cli whc_createrawtx_input `
2. Create payload data:  `wormholed-cli whc_createpayload_issuancefixed`
3. Add output, add the created wormhole payload data to the transaction output:   `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes:`wormholed-cli whc_createrawtx_reference`
  Result: the new token created at this step will be at the address of step 1 
5. Sign：`wormholed-cli signrawtransaction`
6. Send：`wormholed-cli sendrawtransaction`

### Create crowdsale property
1. Add input:  `wormholed-cli whc_createrawtx_input `
2. Create payload data： `wormholed-cli whc_createpayload_issuancecrowdsale`
3. Add output, add the created wormhole payload data to the transaction output:  `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes: `wormholed-cli whc_createrawtx_reference`
5. Sign: `wormholed-cli signrawtransaction`
6. Send: `wormholed-cli sendrawtransaction`

### Participate in crowdsale
1. Add input:  `wormholed-cli whc_createrawtx_input `
2. Create payload data: `wormholed-cli whc_createpayload_particrowdsale`
3. Add output, add the created wormhole payload data to the transaction output:   `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes: `wormholed-cli whc_createrawtx_reference`
5. Create a crowdsale address output: `wormholed-cli whc_createrawtx_reference`
6. Sign: `wormholed-cli signrawtransaction`
7. Send: `wormholed-cli sendrawtransaction`


### Close crowdsale
1. Add input:  `wormholed-cli whc_createrawtx_input `
2. Create payload data： `wormholed-cli whc_createpayload_closecrowdsale`
3. Add output, add the created wormhole payload data to the transaction output:   `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes:`wormholed-cli whc_createrawtx_reference`
5. Sign:`wormholed-cli signrawtransaction`
6. Send: `wormholed-cli sendrawtransaction`

### Create manageale property
1. Add input：   `wormholed-cli whc_createrawtx_input `
2. Create payload data： `wormholed-cli whc_createpayload_issuancemanaged`
3. Add output, add the created wormhole payload data to the transaction output:   `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes: `wormholed-cli whc_createrawtx_reference` 
5. Sign: `wormholed-cli signrawtransaction`
6. Send: `wormholed-cli sendrawtransaction`

### Grant token
1. Add input:  `wormholed-cli whc_createrawtx_input `

2. Create payload data： `wormholed-cli whc_createpayload_grant`

3. Add output and add the created wormhole payload data to the transaction output:    `wormholed-cli whc_createrawtx_opreturn`

4. Add output to receive changes: `wormholed-cli whc_createrawtx_reference`

5. Create output to grant tokens to：`wormholed-cli whc_createrawtx_reference`

   This step can be omitted if the token is issued to the issuer address

6. Sign：`wormholed-cli signrawtransaction`

7. Send：`wormholed-cli sendrawtransaction`

### Revoke token
1. Add input：   `wormholed-cli whc_createrawtx_input `
2. Create payload data： `wormholed-cli whc_createpayload_revoke `
3. Add output and add the created wormhole payload data to the transaction output:    `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes: `wormholed-cli whc_createrawtx_reference`
5. Sign：`wormholed-cli signrawtransaction`
6. Send：`wormholed-cli sendrawtransaction`

### Send tokens to specific token holders 
1. Add input：   `wormholed-cli whc_createrawtx_input `(注意：第一个输入必须含有足够的空投token)
2. Create payload data： `wormholed-cli whc_createpayload_sto`
3. Add output and add the created wormhole payload data to the transaction output:   `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes: `wormholed-cli whc_createrawtx_reference`
5. Sign：`wormholed-cli signrawtransaction`
6. Send：`wormholed-cli sendrawtransaction`

### Change token Issuer
1. Add input:   `wormholed-cli whc_createrawtx_input `(注意：第一个输入必须为token的发行地址)
2. Create payload data： `wormholed-cli whc_createpayload_changeissuer`
3. Add output and add the created wormhole payload data to the transaction output：   `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes: `wormholed-cli whc_createrawtx_reference`
5. Add outpt which token Issuer changed to：`wormholed-cli whc_createrawtx_reference`
6. Sign：`wormholed-cli signrawtransaction`
7. Send：`wormholed-cli sendrawtransaction`

### Freeze token

1. Add input：   `wormholed-cli whc_createrawtx_input `
2. Create payload data： `wormholed-cli whc_createpayload_freeze`
3. Add output and add the created wormhole payload data to the transaction output：   `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes：`wormholed-cli whc_createrawtx_reference` 
5. Sign：`wormholed-cli signrawtransaction`
6. Send：`wormholed-cli sendrawtransaction`

### Unfreeze token

1. Add input：   `wormholed-cli whc_createrawtx_input `
2. Create payload data： `wormholed-cli whc_createpayload_unfreeze`
3. Add output and add the created wormhole payload data to the transaction output：   `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes：`wormholed-cli whc_createrawtx_reference` 
5. Sign：`wormholed-cli signrawtransaction`
6. Send：`wormholed-cli sendrawtransaction`

### Issue ERC721 property

1. Add input: `wormholed-cli whc_createrawtx_input`
2. Create payload data: `wormholed-cli whc_createpayload_issueERC721property`
3. Add output and add the created wormhole payload data to the transaction output:  `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes: `wormholed-cli whc_createrawtx_reference`
5. Sign: `wormholed-cli signrawtransaction`
6. Send: `wormholed-cli sendrawtransaction`

### Issue ERC721 token

1. Add input: `wormholed-cli whc_createrawtx_input`
2. Create payload data: `wormholed-cli whc_createpayload_issueERC721token`
3. Add output and add the created wormhole payload data to the transaction output: `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes: `wormholed-cli whc_createrawtx_reference`
5. Add output,  the created token is on this address: `wormholed-cli whc_createrawtx_reference`
6. Sign: `wormholed-cli signrawtransaction`
7. Send:  `wormholed-cli sendrawtransaction`

### Transfer ERC721 Token

1. Add input: `wormholed-cli whc_createrawtx_input`
2. Create payload data: `wormholed-cli whc_createpayload_transferERC721token`
3. Add output and add the created wormhole payload data to the transaction output:  `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes: `wormholed-cli whc_createrawtx_reference`
5. Add output，transfer the token to this address: `wormholed-cli whc_createrawtx_reference`
6. Sign: `wormholed-cli signrawtransaction`
7. Send: `wormholed-cli sendrawtransaction`

### Revoke ERC721 Token

1. Add input: `wormholed-cli whc_createrawtx_input`
2. Create payload data: `wormholed-cli whc_createpayload_destroyERC721token`
3. Add output and add the created wormhole payload data to the transaction output: `wormholed-cli whc_createrawtx_opreturn`
4. Add output to receive changes: `wormholed-cli whc_createrawtx_reference`
5. Sign: `wormholed-cli signrawtransaction`
6. Send: `wormholed-cli sendrawtransaction`
