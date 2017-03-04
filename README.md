People Service (new name?)
==========================

The idea of the people service is to provide a distributed, surveillance resistant (from: Google/Facebook, ISP, Government, Hacker), social network. This has support for realtime messaging (text, voice, video), non-realtime messaging (i.e. email), and social network style posting (status, pictures, video, etc).

* User has a public server (like e-mail)
* Public server has no access to content
* Per-user-device keys
* Realtime connections through server
* Store-and-forward messages through server
* Post data packets to server (posts, pictures, videos), only send decryption key to social network groups

Core to this idea is that each user has a point of access on a publically accessible server on the internet (and/or Tor hidden service). This server will generally only contain encrypted content, except for content that the user explicitly chooses to make publically available (a profile picture, school attended, blog posts, etc or none at all). 

Each server would listen on a port (TBD, listed below as ABCD), similar to the e-mail system. This allows people to use familiar joe@example.com addresses, but instead of messages going to 25, they will go to ABCD. All connections to ABCD will be wrapped in TLS, for an additional but unnecessary layer of security.

A user can have multiple devices (e.g. phone, tablet, laptop, desktop, etc) that all keep a live connection to their personal server, similar to the methods iMessage/Hangouts/etc keep connected. This will allow for real-time messages that are come through the server to be handled. 

Each device the user has keeps a private key pair, which can be rotated (hourly/daily/weekly?) while the device is connected to the server. Each time it is rotated, the new key is signed with the old one, and the public portion is pushed out to the server and out to the desired social network contacts. The old one is kept for some amount of time to access messages already encrypted with it.

Store-and-forward example
-------------------------

When a second user, Jane, wishes to send the first user who is a "friend", Joe, a message. She will sign the message with her private key and encrypt the message with a symmetric key (need a signature outside the packet? how to do this while maintaining privacy?). This symmetric key will then be encryped with each of Joe's device keys (i.e. four encryptions of the symmetric key for the phone, tablet, laptop, and desktop). Then she will wrap the entire message with the public key for Joe's server. Ideally her client would check with Joe's public server immediately before sending the message to retrieve the latest device and server keys, however if she is forced into a store-and-forward mode, she will used the saved public keys she had from the last time she was online. At this point she will send the message to Joe's public server. Joe's public server's private key will decrypt the outer wrapper and store it for one of Joe's devices to download when it next logs in.

When Joe next logs in to his laptop, and starts his people service client, he will see that there is a new message on the server. His client will then download the message and use the private key on his laptop to decrypt the symmetric key. Once he has the symmetric key he decrypts the message and views the plain text of the message.

Realtime Example
----------------

If both Jane and Joe are online, a similar process will occour, except the message exchanged will be simply metadata for a realtime session to open. When this session opens, it will be encrypted with a diffie-hellman key exchange, to provide forward secrecy. In the open session, they can exchange text, voice, video, or data. Ideally this session will flow diectly from Jane's device to Joe's device, however if one or both of the users is behind a firewall that doesn't allow random connections in they can simply route the session through their public server(s). 

Social Media Push Example
-------------------------

Social Media Pull Example
-------------------------


