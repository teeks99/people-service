People Service (new name?)
==========================

The idea of the people service is to provide a distributed, surveillance resistant (from: Google/Facebook, ISP, Government, Hacker), social network. This has support for realtime messaging (text, voice, video), non-realtime messaging (i.e. email), and social network style posting (status, pictures, video, etc).

Core to this idea is that each user has a point of access on a publically accessible server on the internet (and/or Tor hidden service). This server will generally only contain encrypted content, except for content that the user explicitly chooses to make publically available (a profile picture, school attended, blog posts, etc or none at all). 

Each server would listen on a port (TBD, listed below as ABCD), similar to the e-mail system. This allows people to use familiar joe@example.com addresses, but instead of messages going to 25, they will go to ABCD. All connections to ABCD will be wrapped in TLS, for an additional but unnecessary layer of security.

A user can have multiple devices (e.g. phone, tablet, laptop, desktop, etc) that all keep a live connection to their personal server, similar to the methods iMessage/Hangouts/etc keep connected. This will allow for real-time messages that are come through the server to be handled. 

Each device the user has keeps a private key pair, which can be rotated (hourly/daily/weekly?) while the device is connected to the server. Each time it is rotated, the new key is signed with the old one, and the public portion is pushed out to the server and out to the desired social network contacts. The old one is kept for some amount of time to access messages already encrypted with it.

When a second user, Jane, wishes to send the first user who is a "friend", Joe, a message. She will sign the message with her private key and encrypt the message with a symmetric key. This symmetric key will then be encryped with each of Joe's device keys (i.e. four encryptions of the symmetric key for the phone, tablet, laptop, and desktop). Then she will wrap the entire message with the public key for Joe's server. Ideally her client would check with Joe's public server immediately before sending the message to retrieve the latest device and server keys, however if she is forced into a store-and-forward mode, she will used the saved public keys she had from the last time she was online. At this point she will send the message to Joe's public server.

When Joe next logs in to his laptop, and starts his people service client, he will see that there is a new message on the server. 
