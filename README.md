# Home Server uCore Builder

Build a personalized Home Server Installer ISO in GitHub Actions.

> [!IMPORTANT]
> **Before the first build, add a repository Actions secret named `SSH_PUBLIC_KEY`.**
>
> Put your **public SSH key** in that secret, for example `ssh-ed25519 AAAA...`.
> Never put a private SSH key in GitHub.
>
> Go to **Settings → Secrets and variables → Actions → New repository secret**, create `SSH_PUBLIC_KEY`, then run the build workflow.

This repository is intended to be used as a GitHub **template repository**. Click **Use this template** to create your own copy, add your SSH public key secret, and build your own ISO without modifying the installer source.

## New to GitHub? Start here

1. Click **Use this template → Create a new repository**.
2. Choose your GitHub account as the owner, give the repository any name you like, and click **Create repository**.
3. In your new repository, open **Settings**.
4. Go to **Secrets and variables → Actions**.
5. Under **Repository secrets**, click **New repository secret**.
6. Set:
   - **Name:** `SSH_PUBLIC_KEY`
   - **Secret:** paste your full public SSH key, for example `ssh-ed25519 AAAA...`
7. Click **Add secret**.
8. Open the **Actions** tab at the top of your repository.
9. In the left side, select **Build personalized Home Server Installer ISO**.
10. Click **Run workflow**.
11. Leave the branch set to **main**, then click the green **Run workflow** button.
12. Wait for the workflow to show **Success**.
13. Open the completed workflow run.
14. In the summary, click **Download personalized ISO artifact (.zip)**.

After extracting the ZIP, you will have:

```text
home-server-ucore-installer-amd64.iso
home-server-ucore-installer-amd64.iso.sha256
build-info.txt
```

> [!NOTE]
> The download is kept for 1 day. If it expires, run the workflow again to build a fresh ISO.

## What the Builder does

When you run **Actions → Build personalized Home Server Installer ISO → Run workflow**, the workflow:

1. Resolves the **latest published release** of [`home-server-project/home-server-installer`](https://github.com/home-server-project/home-server-installer).
2. Checks out that exact released tag.
3. Builds the released installer binary.
4. Builds a Fedora CoreOS-based Home Server Installer ISO.
5. Embeds the public key from your `SSH_PUBLIC_KEY` secret into the live installer handoff.
6. Produces a SHA256 checksum and a small build-information file.
7. Uploads the finished files as a GitHub Actions artifact retained for **1 day**.

The Builder deliberately follows the latest published installer release rather than the installer's development branch. A new installer version is used by the Builder only after it has been published as a GitHub Release.

## Installer choices

The personalized ISO still presents all five V1 installation targets:

- Home Server uCore LTS
- Home Server uCore HCI LTS
- uCore Minimal LTS
- uCore LTS
- uCore HCI LTS

> [!IMPORTANT]
> The selected uCore image is downloaded during installation. **An internet connection is required during the normal installation path.** The Builder does not embed a selected uCore container image into the ISO.

## Build your ISO

After creating a repository from this template:

1. Add the `SSH_PUBLIC_KEY` Actions secret as described at the top of this README.
2. Open the **Actions** tab.
3. Select **Build personalized Home Server Installer ISO**.
4. Select **Run workflow**.
5. Wait for the workflow to finish successfully.
6. Download the `home-server-ucore-installer-amd64` artifact from the workflow run.

The artifact contains:

```text
home-server-ucore-installer-amd64.iso
home-server-ucore-installer-amd64.iso.sha256
build-info.txt
```

## SSH key behavior

`SSH_PUBLIC_KEY` must contain one valid OpenSSH public key. The workflow validates the key before starting the ISO build and refuses to continue if the secret is missing, malformed, contains multiple lines, or appears to contain private-key material.

The public key is safe to distribute inside your personalized installer ISO. Your **private key stays on your own computer** and is never required by this Builder.

After installation, SSH client key selection still works normally: your local SSH client must have the matching private key available through the SSH agent, a standard identity filename, SSH configuration, or an explicit `ssh -i` option.

## Safety and current scope

Home Server Installer V1 is designed for UEFI systems. Secure Boot must be disabled during installation. VM testing is recommended first. For bare-metal testing, use a dedicated test drive or hardware where the selected installation disk can be safely erased, and keep backups of anything important.

The installer erases and repartitions only the disk selected in the installer UI.

## Projects

- [Home Server Installer](https://github.com/home-server-project/home-server-installer)
- [Home Server uCore](https://github.com/home-server-project/home-server-ucore)
- [Universal Blue uCore](https://github.com/ublue-os/ucore)
- [Fedora CoreOS](https://github.com/coreos/fedora-coreos-tracker)

Home Server Project: **Cloud-native technology, brought home.**
