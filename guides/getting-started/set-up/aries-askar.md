# Aries Askar

[Aries Askar](https://github.com/hyperledger/aries-askar) provides secure, encrypted storage and cryptographic support for encrypting, decrypting, signing and verifying data. It also provides both the `Wallet` and `StorageService` implementations for the agent.

:::caution
Although Aries Askar is stable and already used in production in agents such as [Aries Cloud Agent Python](https://github.com/hyperledger/aries-cloudagent-python), support for Aries Askar in Credo is currently experimental. We recommend new projects to use Aries Askar from the start, and also to migrate existing projects to Aries Askar. However, projects may experience some issues. If you encounter any issues, please [open an issue](https://github.com/openwallet-foundation/credo-ts/issues/new).

Currently, there are few limitations to using Aries Askar.

- When running in Node.JS, only Node.JS 18 is supported for now. See [Supported Node.JS versions for Aries Askar](#supported-nodejs-versions-for-aries-askar)
- Install scripts rely on bash command substitution to get the proper binaries for each system architecture and platform. Therefore, if you are under Windows, you must configure `npm` or `yarn` to use a bash-compliant shell (e.g. `npm config set script-shell "C:\\Program Files\\git\\bin\\bash.exe"`)

:::

:::tip

If you're upgrading from the Indy SDK to Aries Askar, see [Migrating from an Indy SDK Wallet to Aries Askar](../updating/update-indy-sdk-to-askar)

:::

### Installing Aries Askar

When using Credo with Aries Askar, there are a few extra dependencies that need to be installed. We need to install the `@aries-framework/askar` package, which implements the needed interfaces for the agent. Secondly, we need to add native bindings for the specific platform `@hyperledger/aries-askar-<platform>`. Currently there are bindings for Node.JS, as `@hyperledger/aries-askar-nodejs`, and React Native as `@hyperlegder/aries-askar-react-native`.

<!--tabs-->

# Node.JS

```console
yarn add @aries-framework/askar@^0.4.0 @hyperledger/aries-askar-nodejs@^0.1.0
```

# React Native

```console
yarn add @aries-framework/askar@^0.4.0 @hyperledger/aries-askar-react-native@^0.1.0
```

<!--/tabs-->

### Adding Aries Askar to the Agent

After installing the dependencies, we can register the Askar Module on the agent.

<!--tabs-->

# Node.JS

```typescript showLineNumbers set-up-askar.ts section-1

```

# React Native

```typescript showLineNumbers set-up-askar-rn.ts section-1

```

<!--/tabs-->

### Supported Node.JS versions for Aries Askar

Due to an issue in `ref-napi` (which is used in the Node.JS bindings for Aries Askar), performance for Aries Askar in Node.JS is not as expected. A patched version for `ref-napi` has been published that fixes this issue, but this only works in Node.JS 18+.

To use Aries Askar in Node.JS, make sure you're using Node.JS 18 and patch the `ref-napi` package to point towards `@2060.io/ref-napi` in your `package.json`:

<!--tabs-->

# NPM

Using [NPM `overrides`](https://docs.npmjs.com/cli/v9/configuring-npm/package-json#overrides) we can point the `ref-napi` package to `@2060.io/ref-napi`.

```json
{
  "overrides": {
    "ref-napi": "npm:@2060.io/ref-napi"
  }
}
```

# Yarn

Using [Yarn `resolutions`](https://classic.yarnpkg.com/lang/en/docs/selective-version-resolutions/) we can point the `ref-napi` package to `@2060.io/ref-napi`.

```json
{
  "resolutions": {
    "ref-napi": "npm:@2060.io/ref-napi"
  }
}
```

<!--/tabs-->

:::info

See the following issues for more information on the performance bottleneck

- https://github.com/hyperledger/aries-askar/issues/76
- https://github.com/node-ffi-napi/ref-napi/issues/72
- https://github.com/node-ffi-napi/ref-napi/pull/73

:::
