# envkey gem

Integrate [EnvKey](https://www.envkey.com) with your Ruby or Ruby On Rails projects to keep api keys, credentials, and other configuration securely and automatically in sync for developers and servers.

## envkey-source

In EnvKey v2, using the [envkey-source](https://docs-v2.envkey.com/docs/envkey-source) executable from the command line offers additional functionality, like automatic reloads, that aren't available to EnvKey's language-specific SDKs. It's also faster and a lot smaller in size. Consider using it instead of this library if it fits your use case.

## Installation

In your Gemfile:

```ruby
gem 'envkey'
```

## Usage

To load an EnvKey environment, follow the [integration quickstart](https://docs-v2.envkey.com/docs/integration-quickstart), but stop before integrating with envkey-source (which you won't be doing).

If you're using Rails, that's all you need to do. In plain Ruby, you need to require envkey at the entry point of your application.

```ruby
require 'envkey'
```

Now all your EnvKey variables will be available on `ENV`.

### Errors

The gem will throw an error if an `ENVKEY` is missing or invalid.

### Overriding Vars

The envkey gem will not overwrite existing environment variables or additional variables set in the `.env` file you loaded your `ENVKEY` from. This can be convenient for customizing environments that otherwise share the same configuration. You can also use [branches or local overrides](https://docs-v2.envkey.com/docs/branches-and-local-overrides) for this purpose.

### Working Offline

The envkey gem caches your encrypted config in development so that you can still use it while offline. Your config will still be available (though possibly not up-to-date) the next time you lose your internet connection. If you do have a connection available, envkey will always load the latest config. Your cached encrypted config is stored in `$HOME/.envkey/cache`

For caching purposes, the gem assumes you're in development mode if either `ENV["RAILS_ENV"]` or `ENV["RACK_ENV"]` is `"development"` or `"test"`.

You can also turn on caching by setting a `ENVKEY_SHOULD_CACHE=1` environment variable when running your app (_not_ in your EnvKey config):

```bash
ENVKEY_SHOULD_CACHE=1 ruby your_app.rb
```

## envkey-source binaries

If you look in the `ext` directory of this gem, you'll find a number of `envkey-source` binaries for various platforms and architectures. These are output by the [envkey-source Go library](https://github.com/envkey/envkey/public/sdks/envkey-source). It contains EnvKey's core cross-platform fetching, decryption, verification, web of trust, redundancy, and caching logic, and can also be used directly.

## x509 error / ca-certificates

On a stripped down OS like Alpine Linux, you may get an `x509: certificate signed by unknown authority` error when attempting to load your config. envkey-source tries to handle this by including its own set of trusted CAs via [gocertifi](https://github.com/certifi/gocertifi), but if you're getting this error anyway, you can fix it by ensuring that the `ca-certificates` dependency is installed. On Alpine you'll want to run:

```
apk add --no-cache ca-certificates
```

## Further Reading

For more on EnvKey in general:

Read the [docs](https://docs-v2.envkey.com).

Read the [integration quickstart](https://docs-v2.envkey.com/docs/integration-quickstart.html).

Read the [security and cryptography overview](https://docs-v2.envkey.com/docs/security).

## Need help? Have questions, feedback, or ideas?

Post an [issue](https://github.com/envkey/envkey/issues), start a [discussion](https://github.com/envkey/envkey/dicussions), or email us: [support@envkey.com](mailto:support@envkey.com).
