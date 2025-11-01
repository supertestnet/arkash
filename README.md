# Arkash
Using Ark as a backend to recreate the experience of ecash, but self-custodial (and not as private)

# What is this?
This is a step closer to a self-custodial ecash-like experience. I've been working on something similar for a while with my Hedgehog project, but a few nights ago I realized that Ark can do the same thing: store funds in such a way that a non-custodial third party can forward it to your recipient or "return to sender," but not take it (Ark servers and Hedgehog servers can both do this).

The protocol can be summarized thusly: create a secret, and ensure that anyone who knows that secret can use it to withdraw money from a server via LN (with the server's cooperation) or on L1 (without its cooperation). Then share three pieces of info with your recipient: the secret, the server they need to talk to to get the money via LN (in the happy path), and whatever they need to get the money via L1 (in the sad path). Voila! This makes the payment asynchronous. The recipient needn't be online when the sender creates the payment, and the sender needn't be online when the recipient redeems it.

# How can I try it?
Just click here: https://supertestnet.github.io/arkash

But be warned: it is only for testing, and you are likely to lose your money. Only put money in if you are willing to lose it for the sake of science.

# Video demo
[![](https://supertestnet.github.io/arkash/arkash-screenshot.png)](https://supertestnet.github.io/arkash/arkash-demo.mp4)

# Explainer

In the world of bitcoin, there are two main "Chaumian Ecash" projects: fedimint and cashu. I really like the interface provided by these two services, namely this feature: to send somone money, you don't need to "get" anything from them, such as a bitcoin address or a lightning invoice. Instead, you *create* a payment instrument for them -- effectively, an IOU from an ecash mint -- and just hand it to them (or email it to them, or dm it to them, etc).

I like that interface because it reminds me of *physical* payment instruments. If I want to physically hand you $5, I don't need to get something from you; I just reach into my pocket and hand a bill to you. Same with a cheque or money order -- I just physically hand you the thing I want you to have (or mail it to you, or slip it under your door, etc).

I want digital money to work like that too, but in bitcoin (and lightning), the paradigm is that the recipient acts first, at the very least by creating a wallet and putting a bitcoin address or a similar payment string in a location where other people can find it. The sender then acts by finding that payment string and encoding a payment to it. This seems backward to me. In my opinion, the sender shouldn't have to *get* something from the recipient, the sender should just "give" something to the recipient. E.g. I should be able to give you bitcoin even if you've never opened a bitcoin wallet before, just like I can give you cash even if you've never had cash before. Chaumian ecash gets us closer to this world by letting the *sender* act first (as I want), but sadly, doing bitcoin-backed Chaumian ecash in a non-custodial way is currently an unsolved problem.

I think Ark moves a step in the right direction: Ark Service Providers are non-custodial and they provide "Send" and "Receive" functions that are similar to the ones provided by ecash mints. Sadly, they don't have the same privacy properties. But I think I can get pretty close by creating single-purpose Ark "accounts."

To that end, in this app, you can create a single-purpose Ark "account." By that term, I mean a cryptographic keypair, a bitcoin address that encodes Ark's onboarding script, and a websocket connection to an Ark Service Provider who understands this protocol. Then you can send money to this account (so it is held by your private key, and the server does not have custody), and then you can send the private key to your recipient. Sending the private key constitutes the payment, and he or she does not have to be online for you to do this!

When your recipient gets online, they can load the private key into this app and "redeem" the payment -- i.e. withdraw the money from the "account" you deposited it *into.* And thus it works a lot like how Chaumian ecash works: the sender and the recipient do not have to be online simultaneously, but now the server does not have custody of user funds, and there is at least one privacy benefit that Chaumian ecash has too: as long as each account is only used twice, to fund it and to redeem it, the server has very little info to build up a profile about the users.

Except that I am not yet sure of a good way to stop the server from learning each user's ip address, so he or she *can* build up a profile with lots of sensitive info: your ip address, the amount of times that ip address has used his service, the amount of money deposited by that ip address each time, the times of day when it did so (and thus, probably, your time zone), and the ip addresses of the people who redeemed that money. That's a lot, but if you use tor or a vpn to guard your ip address, that can help, and anyway, I think it's a step in the right direction. Have fun!
