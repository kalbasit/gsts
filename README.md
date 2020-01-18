![gsts](images/logo/cover.png)

`gsts` (short for `Google STS`) is a fork of [aws-google-auth](https://github.com/cevoaustralia/aws-google-auth) (the original project) based on [puppeteer](https://pptr.dev) which aims to obtain and store AWS STS credentials to interact with Amazon services by authenticating against a pre-configured GSuite SAML instance.

In other words, you can configure Amazon AWS to rely on Google to validate your login credentials and to tell it what your account should map to in terms of IAM roles.

The problem is that this flow is tailored for the web which makes command-line usage a lot more difficult. This utility is a hack around that.

## Installation

Install the package via `npm`:

```sh
npm install --global gsts
```

or via `yarn`:

```
yarn global add gsts
```

## Usage

You can launch `gsts` using command-line options or by setting up some environment variables.

```sh
❯ gsts --help
gsts

Options:
  --help                         Show help                             [boolean]
  --version                      Show version number                   [boolean]
  --aws-profile                  AWS Profile                    [default: "sts"]
  --aws-role-arn                 AWS Role ARN
  --idp-id, --google-idp-id      Google IDP ID                        [required]
  --sp-id, --google-sp-id        Google SP ID                         [required]
  --username, --google-username  Google username
```

The first authentication is performed directly on a headful browser instance where all of the authentication challenges generated by Google are natively supported (TOTP, Push, SMS, Security Keys, etc). Subsequent runs use an existing session to obtain fresh STS credentials every time the utility is ran.

As this utility is meant to be used from the command-line, it is imperitive to be fast. Therefor, all unnecessary requests (like static assets) are skipped by default. The complete roundtrip time after the first authentication step is completed is in the order of one second and totally invisible.

For compatibility reasons, most environment variables supported [aws-google-auth](https://github.com/cevoaustralia/aws-google-auth) are also supported by `gsts`:

| Description | Env Variable | Required |
|-------------|-----------|------------|
| AWS Profile | $AWS_PROFILE | No (_default: sts_)
| Google IDP ID | $GOOGLE_IDP_ID | Yes |
| Google SP ID | $GOOGLE_SP_ID | Yes |
| Google Username | $GOOGLE_USERNAME | No |

To debug the utility, run it with `DEBUG=gsts`.

## License

MIT
