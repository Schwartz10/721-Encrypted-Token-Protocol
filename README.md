Recently I started working on a project that involved smart contracting an exchange of non-fungible tokens representing a variable amount of encrypted data. The requirement is that the encrypted data represented by each token should only be decryptable by the token owner’s private key. The project quickly uncovered a limitation with current implementations of the ERC721 protocol that I have yet to find a sound solution for. This article is meant to be a thought provoker, yet also a small cry for help.

First, I cover the basics of a non-fungible token and why I think non-fungible tokens representing a variable amount of encrypted data is a big part of the blockchain’s future. Next, I dive deeper into the technical challenge associated with creating encrypted, non-fungible tokens, and finally, I’ll offer a theoretical technical solution to the problem (with an associated smart contract coming soon).

*Fungibility*

Fungibility refers to an asset’s interchangeability with other individual assets of the same type https://www.investopedia.com/terms/f/fungibility.asp . For example, we’d consider a United States $10 Bill a fungible asset - since it can be replaced by any other $10 bill, two $5 bills, 10 $1 bills…etc. Conversely, a non-fungible asset is not as easily replaceable or exchangeable due to its unique valuation. Pokemon cards can be considered non-fungible assets - buy a pack of cards for $10 might yield you 9 cards worth $0.10, and 1 card worth $100. Yet go to the bank and attempt to exchange your limited edition, high value card for its proper fiat value and quickly get rejected.

*Use Cases*

Intuitively, one might question the need for a non-fungible, encrypted asset - why would a buyer buy an asset that he/she has to pay for before unscrambling the data and seeing its true form? How would such an asset be valued?

The answer lies in the fact that only a _part_ of the non-fungible token contains encrypted data and the rest is public. The amount of data that’s actually encrypted is completely up to the token’s owner. Intelligent token owners can strategically market aspects of their token in hopes to appeal to potential buyers, without revealing its core.

A first example is the project I am working on - which implements a non-fungible token with a flexible amount of encrypted data that represents medical data. Owners can choose to publicize the fact that they are female, blood-type O Positive, 32 years old with two children, but encrypt their last visit to the doctors which revealed their true health status. Medical researchers may be interested in this patient’s history, and choose to buy the last 4 years of her medical data tokens.

Additionally, this exact model can be applied to many applications that we as everyday consumers would benefit from. What if you could sell your own browsing data to advertisers? Your purchasing behavior? Location data?

Blockchain advocates preach the potential disruption capabilities the blockchain can have on our everyday lives - yet I don’t think the impact will truly be felt unless consumers see direct benefits. I believe that implementing several flexible, encryption token systems with the proper underlying economies and exchanges is a not-so-distant path to getting consumer adoption and admiration of the blockchain’s capabilities.

*Technical Challenge*

The blockchain is a public ledger representing all of our transactions - be it monetary exchanges or requests for computational power. The keyword here is _public_. If we exchange an encrypted asset over the blockchain, how can we ensure the asset remains encrypted? More importantly, how can we *trust* token owners to properly encrypt the data upon sale of their token to a buyer?

RSA encryption (AKA asymmetric cryptography) has two main applications - signing transactions and encrypting messages. Encrypting a message means that the sender uses the receiver’s public key to encrypt the message, so only the receiver can decrypt this message with his/her private key. Bringing this down to the encrypted token level - upon sale of a token, the seller/owner must:

~~~Off-chain~~~
1. Decrypt the encrypted data associated with the token for sale
2. Obtain the public key of the buyer
3. Re-encrypt the data with the buyer’s public key
~~~On-chain~~~
4. Successfully transfer the token

If the seller acts immorally during the off-chain steps in an encrypted token sale (imagine re-encrypting the data with a public key from his/her own other accounts), the seller can essentially hold the token hostage and the buyer will have paid for a useless, unencryptable token. So what solutions do we have available to us to ensure the seller is honest and reliable in following these off-chain steps?

The first solution to immediately rule out is to code this logic in the smart contract itself. On top of RSA encryption being computationally expensive (and thus costing lots of gas), the smart contract function call to actually encrypt this data would require the un-encrypted data, meaning anyone could query the blockchain, find relevant transaction hashes, and capture un-encrypted data from these tokens. This completely defeats the purpose.

A second solution to rule out is to rely on the buyer to verify the transaction. Why can’t this work? Well what happens when the buyer is acting immorally and upon receiving a properly encrypted token, reports it as being wrongly encrypted? With this solution, we fall directly into a recursive byzantine generals’ problem and are left solution-less.

A third solution to rule out is to use the owner of the smart contract to process transactions for us. However, I think this is clearly an unsatisfactory solution given its centralized nature. What happens if the third party starts acting immorally or his/her key pair gets compromised?

## Solution Coming Soon
