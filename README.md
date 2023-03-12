# webshim
This tool has grown and changed. It started its like as a hack to allow to you run Metamask from your local C:// directory. But it has turned into a generic tool
that uses Peerjs to create a remote web shim that you can control locally. I will put a version out on IPFS so can run it from there or download to your own website. Of course if you had a website to download to, then you wouldn't need the shim to begin with.

---------------------------------------------------------------
An interesting hack that allows you to run Metamask from your local computer

For some reason, the Metamask tool fails if you call it from your local computer with Javascript. The 'window.ethereum' Object simply does not 
load. This is very frustrating because you are forced to build a remote website to call Metamask for no other good reason.

Insteresting enough, Metamask will work from IPFS if you use the https://ipfs.io/ipfs/$CID gateway. So you could put your code out on IPFS and use it from there.

This presents two problems, however:

   1. Anyone could see all of your code, including an API key.
   2. You still have to port your code back and forth to IPFS each time you update it.

Now, I was able to get around these issues rather easily by simply putting some of my code into an iframe. This allowed me to host my webpage on my local computer and have a special "Metamask Writer" that perfectly fit into the frame which I was given to live in. While this worked ok, it quickly became limited.

Enter WebRTC. By using the peer.js javascript package (hosted without nodejs), I am now able to create a hidden metamask module that is simple enough to host on IPFS and not change. It has a copy of the ethers.js library. While it has some small amount of code devoted to working with my credentialed "Zork ][" ledger, it could work with any ledger that you can add to Metamask.

When the code boots ups, you will see two messages in your console log:

local listening...
remote listening...

Once you see these two, you are good to go. In this version, your local copy is called "left" and the remote one is called "right". This means that if two of us use this at the same time, we might have a collision. I will fix this soon.

There is an coding interface for this. I don't want to entirely use the term "API", but I do give you a few commands:

To start run:

chat()

To send, you then run:

remote.send("VERB NOUN")

The possible "Verbs" are:

"eval" - This evaluates the rest of the string in the remote environment, this is basically the keys to the kingdom (game over). With this, you can build whatever else you want. However (because I am a nice guy), I will give you a few other commands.

  Example:

     remote.send("eval console.log(abc)");
     [ returns the value of "abc" if set ]


"cons" - This is similar to the console.log command. 

  Example:

    remote.send("cons x")
    [ returns the value of "x" if set ]

"var" - Sets a variable in the remote environment. It doesn't actually use the 'var' keyword, the what gets set is always global for the iframe environment. This one can be a bit strange to set:

    remote.send("var x " + xyz)
    [ Sets the remote "x" variable globally to the local value of xyz. Remember, all that the send command userstands is strings.

--------------------------------------------------------------------------------------------------

But what about Metamask? You didn't talk about Metamask! 

The remote instance has a copy of ethers.js in it. You could build your own version and host it out separately on IPFS, btw. Then you could load it up with whatever functionality that you want. The example that I currently use to trigger Metamask is a call to my smart contracts:

remote.send(`eval c2.tell('circles',${circles.toString()},'')`);

This runs a contract which have loaded into the system, it calls a function called "tell" which created a dictionary map between the string 'circles' and a function on the running local system called "circles()". You will have to come up with ways to load you own smart contracts into the system, but every possible command that you can run through javascript can be evaluated. 






