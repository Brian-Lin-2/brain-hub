# Security

## Keyring

A keyring is an intermediary layer that lets you fetch passwords from any storage system (ie. local, remote, etc) using an API. This allows your code to not have direct access to the backend storage.

**Architecture:**
`Backend` - This is the actual secure storage (ie. macOS Keychain, Windows Credential Manager)
`Keyring` - This is an API that apps use to talk to the backend. When the app fetches a password, it asks the keyring to retrieve it
`Frontend` - This is the code that is using the keyring to fetch the secrets

## Certificates

A certificate is a applications version of an id card. It allows the app to prove that they are exactly who they claim to be.

In a corporate setting, a certificate installed on your computer allows the corporate's firewall to validate that your device belongs with the company. This allows you to access company applications.

## VPN

A private networking tunnel that allows you to create a secure connection between your internet and another network (typically a corporate network). This allows you to access applications that can normally only be accessible through the corporate internet.

## ZTNA

ZTNA (Zero Trust Network Access) is the modern successor to VPN. While VPN creates an encrypted tunnel which gives you access to all resources inside a network, ZTNA never trusts your device. When you want to connect to a specific application, ZTNA looks at a combination of "signals" before it lets you access the application.

These signals usually consist of `Identity, Device Health, Context, Least Privilege`.

The main difference between VPN and ZTNA is full network access vs app-specific access. When using ZTNA, you always verify before accessing each specific application. Other applications in the network are not visible to you until those checks are complete.
