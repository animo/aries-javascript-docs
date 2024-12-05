import DocCardList from '@theme/DocCardList';

# Agent Setup

:::info

This guide assumes you have followed the [Prerequisites](./prerequisites), and you have a valid
[Node.js](https://nodejs.org) or [React Native](https://reactnative.dev) project setup.

:::

:::caution

Credo is still in **active development**, and as such some APIs are still experimental. **When using any experimental features, make sure to use an exact version of Credo** (`0.5.0`) instead of a range (`^0.5.0`), to prevent accidental breaking changes. If you're not leveraging any experimental features, you can use a range (`^0.5.0`) to get the latest bugfixes and features.

For Credo `0.5.x`, **the following features are experimental**:

- Using OpenID4VC from the `@credo-ts/openid4vc` module
- SD-JWT Verifiable Credentials
- Using multi-tenancy from the `@credo-ts/tenants` module
- Using BBS+ Signatures from the `@credo-ts/bbs-signatures` module
- Using the cheqd module from the `@credo-ts/cheqd` module
- Using DIF Presentation Exchange
- Using AnonCreds in the W3C Verifiable Credential format

:::

### Installing the required dependencies

First we have to install the minimal amount of dependencies that are required for configuring an Credo agent.

<!--tabs-->

# Node.js

```console
yarn add @credo-ts/core@^0.5.3 @credo-ts/node@^0.5.3
```

# React Native

```console
yarn add @credo-ts/core@^0.5.3 @credo-ts/react-native@^0.5.3 react-native-fs react-native-get-random-values
```

<!--/tabs-->

### Additional setup

<!--tabs-->

# Node.js

No additional setup is required for Node.js

# React Native

Since [React Native](https://reactnative.dev) does not have an implementation
for
[`crypto.getRandomValues()`](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/getRandomValues)
we have to setup a polyfill for this. We have to do this at the entrypoint of
the application. This could is most likely `index.(js|ts|jsx|tsx)` at the root
of your application.

```diff title="index.tsx" showLineNumbers
+ import 'react-native-get-random-values'
```

In addition you need add support for resolving modules with the `.cjs` extension, as this is used by some of Credo's dependencies and not automatically supported by React Native.

```js title="metro.config.js" showLineNumbers
module.exports = {
  // ... other Metro config options ...
  resolver: {
    // make sure this includes `cjs` (and other extensions you need)
    sourceExts: ['js', 'json', 'ts', 'tsx', 'cjs'],
  },
}
```

Finally, if you're using [Expo](https://expo.dev) you need to add a custom resolution 'hack' that removes support for the legacy unimodules.

**Yarn**

For yarn we can replace the `@unimodules/react-native-adapter` and `@unimodules/core` dependencies with an empty directory. Make sure to create the `noop` directory in the root of your project and create a `.gitkeep` file in the directory so that the directory is committed to your repository.

```json title="package.json" showLineNumbers
{
  // ... other package.json options ...
  "resolutions": {
    "@unimodules/react-native-adapter": "./noop",
    "@unimodules/core": "./noop"
  }
}
```

**NPM**

Not supported at the moment. NPM overrides work different than Yarn resolutions, and thus we can't use the same trick. If you're using NPM, feel free to open a PR with a working solution.

<!--/tabs-->

### Setting up the agent

> this section does not assume any knowledge of the agent configuration.
> In the [Agent Config](../tutorials/agent-config) tutorial we will discuss in-depth what every
> field in the configuration does and when to set it.

In order to use the agent in the application we have to configure and
initialize it. This following configuration is quite generic and possibly not
enough for your specific use cases. Please refer to the
[tutorials](../tutorials) for a more use-case-specific agent setup.

<!--tabs-->

# Node.js

```typescript showLineNumbers set-up.ts section-1

```

# React Native

```typescript showLineNumbers set-up-rn.ts section-1

```

<!--/tabs-->

### Adding a wallet and storage implementation

After creating the `Agent` instance, we need to provide the agent with a wallet and storage implementation. Credo provides one implementations out of the box, but you can also implement your own. Currently a Wallet and Storage implementation using Aries Askar is supported out of the box.

- [Aries Askar](./set-up/aries-askar)

<DocCardList items={[{
type: 'link',
label: 'Aries Askar',
href: './set-up/aries-askar',
docId: 'getting-started/set-up/aries-askar'
}]} />

### Setting up the transports

Finally, we have to set an outbound transport that
will handle traffic from the agent. It is also possible to set an inbound
transport in the same way as the outbound transport.

<!--tabs-->

# Node.js

Sets up an WS outbound and HTTP inbound and outbound transport.

```typescript showLineNumbers set-up.ts section-2

```

# React Native

For mobile agents the WebSocket transport is often required. We will go into
more depth about the reasons for this in the [mediation](./../../tutorials/mediation.md) section.

```typescript showLineNumbers set-up-rn.ts section-2

```

<!--/tabs-->

### Initializing the agent

Finally, we can initialize the agent and it's ready for use.

<!--tabs-->

# Node.js

```typescript showLineNumbers set-up.ts section-3

```

# React Native

```typescript showLineNumbers set-up-rn.ts section-3

```

<!--/tabs-->

### Next Steps

Now that you have your agent setup, it's time to start building your application. Head over to the tutorials page to get started.

<DocCardList items={[
{ type: 'link', label: 'Tutorials', href: '../tutorials', docId: 'tutorials/index' },
{ type: 'link', label: 'Create a Connection', href: '../tutorials/create-a-connection', docId: 'tutorials/create-a-connection' }
]} />

<!--/tabs-->

### Useful resources

- [Hyperledger Aries RFC - 004:
  Agents](https://github.com/hyperledger/aries-rfcs/blob/main/concepts/0004-agents/README.md)
