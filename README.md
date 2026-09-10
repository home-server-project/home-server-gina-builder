<p align="center">
  <img src="https://raw.githubusercontent.com/home-server-project/.github/main/logo/banner-navy-mid.png" alt="Home Server Project banner">
</p>

# Home Server Gina Builder

Home Server Gina Builder is an optional GitHub template for creating a personalized, bootable [Home Server Installer](https://github.com/home-server-project/home-server-installer) ISO with your SSH public key already embedded.

If you do not need a personalized ISO, use the ready-to-use ISO published with the [latest Home Server Installer release](https://github.com/home-server-project/home-server-installer/releases/latest).

> [!IMPORTANT]
> **Before the first build, add a repository Actions secret named `SSH_PUBLIC_KEY`.**
>
> Put your **public SSH key** in that secret, for example `ssh-ed25519 AAAA...`.
> Never put a private SSH key in GitHub.

## Quick start

1. Click **Use this template → Create a new repository**.
2. Add your `SSH_PUBLIC_KEY` under **Settings → Secrets and variables → Actions**.
3. Open **Actions → Build personalized Home Server Installer ISO → Run workflow**.
4. When the workflow succeeds, download the `home-server-gina-installer-amd64` artifact.

The artifact contains:

```text
home-server-gina-installer-amd64.iso
home-server-gina-installer-amd64.iso.sha256
build-info.txt
```

> [!NOTE]
> The download is retained for **1 day**. If it expires, run the workflow again to build a fresh ISO.

<details>
<summary><strong>New to GitHub? Full step-by-step guide</strong></summary>

1. Click **Use this template → Create a new repository**.
2. Choose your GitHub account as the owner, give the repository any name you like, and click **Create repository**.
3. In your new repository, open **Settings**.
4. Go to **Secrets and variables → Actions**.
5. Under **Repository secrets**, click **New repository secret**.
6. Set the name to `SSH_PUBLIC_KEY`.
7. Paste your full public SSH key into the secret value.
8. Click **Add secret**.
9. Open the **Actions** tab at the top of your repository.
10. In the left side, select **Build personalized Home Server Installer ISO**.
11. Click **Run workflow**.
12. Leave the branch set to **main**, then click the green **Run workflow** button.
13. Wait for the workflow to show **Success**.
14. Open the completed workflow run and download the personalized ISO artifact.
15. Before using SSH after installation, read [SSH key behavior](#ssh-key-behavior).

</details>

## Installer choices and requirements

The personalized ISO uses the **latest published Home Server Installer release** and provides the same installation choices as that release. Supported Gina/uCore images, installation behavior, storage layout, testing details and current limitations are documented in the [Home Server Installer README](https://github.com/home-server-project/home-server-installer).

The selected operating-system image is downloaded during installation, so an internet connection is required for the normal installation path. The Builder does not embed a selected Gina/uCore image into the ISO.

> [!IMPORTANT]
> The current Installer V1 path is UEFI-only. **Secure Boot must be disabled during installation**, and the selected target disk is erased and repartitioned.

## SSH key behavior

`SSH_PUBLIC_KEY` must contain one valid OpenSSH public key. The workflow validates the key before starting the ISO build and refuses to continue if the secret is missing, malformed, contains multiple lines, or appears to contain private-key material.

Only the public key is embedded in the personalized ISO. Your **private key stays on your own computer** and is never required by this Builder.

<details>
<summary><strong>SSH after installation</strong></summary>

- `ssh user@IP` works when the matching private key is available through `ssh-agent`, a normal default SSH identity, or SSH client configuration.
- If the private key has a custom filename and is not loaded into an agent, use `ssh -i /path/to/private-key user@IP`.
- After reinstalling a machine at the same IP, the client may need `ssh-keygen -R IP` because a fresh installation generates a new SSH host identity.

</details>

<details>
<summary><strong>How the Builder works</strong></summary>

When you run the build workflow, the Builder:

1. Resolves the **latest published release** of [Home Server Installer](https://github.com/home-server-project/home-server-installer).
2. Checks out that exact released tag.
3. Builds the released installer binary.
4. Builds a fresh Fedora CoreOS-based Home Server Installer ISO using the current Fedora CoreOS stable live image available at build time.
5. Embeds the public key from your `SSH_PUBLIC_KEY` secret into the live installer handoff.
6. Produces the ISO, SHA256 checksum and build-information file.
7. Uploads the finished files as a GitHub Actions artifact retained for **1 day**.

The Builder deliberately follows the latest published Installer release rather than the Installer development branch. A new Installer version is used by the Builder only after it has been published as a GitHub Release.

</details>

## Upstream and references

<details>
<summary><strong>Project and upstream links</strong></summary>

- [Home Server Installer](https://github.com/home-server-project/home-server-installer)
- [Home Server Gina](https://github.com/home-server-project/home-server-gina)
- [Home Server Project](https://github.com/home-server-project)
- [Universal Blue uCore](https://github.com/ublue-os/ucore)
- [Fedora CoreOS](https://fedoraproject.org/coreos/)

</details>

## License

Apache-2.0. Software and tooling used by the build process retain their own upstream licenses.
