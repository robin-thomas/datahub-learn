In the following tutorials, we're going to interact with the Solana blockchain (and in particular its Devnet network) using the `@solana/web3.js` library. It's a convenient way to interface with the RPC API when building a Javascript application. Under the hood it implements Solana's RPC methods and exposes them as Javascript objects. We will explore it together as we add features to our app.

----------------------------------

# The challenge

{% hint style="tip" %}
In `pages/api/solana/connect.ts`, implement `connect` by creating a `Connection` instance and getting the API version. You must replace the instances of `undefined` with working code to accomplish this.
{% endhint %}

**Take a few minutes to figure this out**

```typescript
//...
  try {
    const {network} = req.body;
    const url = getNodeURL(network);
    const connection = undefined;
    const version = undefined;
    res.status(200).json(version.["solana-core"]);
  }
 //...
```

**Need some help?** Check out those two links
* [Creating a `Connection` instance](https://solana-labs.github.io/solana-web3.js/classes/Connection.html#constructor)  
* [Getting the API `version`](https://solana-labs.github.io/solana-web3.js/classes/Connection.html#getversion)

{% hint style="info" %}
You can [**join us on Discord**](https://discord.gg/fszyM7K), if you have questions or want help completing the tutorial.
{% endhint %}

Still not sure how to do this? No problem! The solution is below so you don't get stuck.

----------------------------------

# The solution

```typescript
//...
  try {
    const {network} = req.body;
    const url = getNodeURL(network);
    const connection = new Connection(url, 'confirmed');
    const version = await connection.getVersion();
    res.status(200).json(version.['solana-core']);
  }
//...
```

**What happened in the code above?**

* We created a `connection` instance of the `Connection` class using the `new` constructor
* We then call `getVersion` on that `connection` instance. The docs state that `connection.getVersion` returns a Promise so we need to handle this with either `.then`, or in a more compact manner with the nullish coalescing operator and optional chaining operator: `?.` In this way, `version?.["solana-core"]` will provide a safe value other than `undefined` when getting `version`!

-----------------------------

# Make sure it works

Once the code above is saved, refresh the page to see it update & display the current version of the Solana software!

![](../../../.gitbook/assets/pathways/solana/solana-connect.gif)

-----------------------------

# Conclusion

We're going to use this `connection` instance every time we need to connect to Solana. Now we're going to want to move some tokens around, but first we need an account to hold tokens! That's what we'll do in the next tutorial, create a keypair on Solana so that we have an account.
