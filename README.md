<p align="center">
  <img src="https://raw.githubusercontent.com/home-server-project/.github/main/logo/banner-navy-mid.png" alt="Home Server Project banner">
</p>

# Home Server Gina Builder

Home Server Gina Builder is a GitHub template for creating a personalized, bootable Home Server Installer ISO with your SSH public key.

The ISO is not limited to Gina. After booting it, Home Server Installer can install Home Server Gina, Home Server Gina HCI, or selected upstream Universal Blue uCore LTS images. See [Installer choices](#installer-choices).

## How it fits

- **Gina Builder** creates the personalized bootable ISO.
- **Home Server Installer** runs from that ISO and handles disk selection and installation.
- **Home Server Gina or upstream uCore** is the operating-system image you choose during installation.

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

## Installer choices

The personalized ISO uses the current published Home Server Installer release, which supports **11 signed LTS installation targets across four families**.

<details>
<summary><strong>Show all 11 installation targets</strong></summary>

### Home Server Gina LTS
- Home Server Gina LTS
- Home Server Gina HCI LTS

### Universal Blue uCore LTS
- uCore Minimal LTS
- uCore LTS
- uCore HCI LTS

### Universal Blue uCore LTS / NVIDIA Open
- uCore Minimal LTS NVIDIA Open
- uCore LTS NVIDIA Open
- uCore HCI LTS NVIDIA Open

### Universal Blue uCore LTS / NVIDIA LTS
- uCore Minimal LTS NVIDIA LTS
- uCore LTS NVIDIA LTS
- uCore HCI LTS NVIDIA LTS

</details>

> [!IMPORTANT]
> The selected Gina/uCore image is downloaded during installation. **An internet connection is required during the normal installation path.** The Builder does not embed a selected operating-system image into the ISO.

## What to expect when booting and installing

> [!NOTE]
> **The installer may take a few minutes to appear after booting.** Fedora CoreOS is starting in the background before the Home Server Installer UI launches, so a short wait is normal.
>
> During installation, the progress bar may remain around **20% for several minutes** while the selected image is downloaded, verified, and deployed.
>
> **This is expected. Do not power off or reboot the machine while installation is in progress.**
>
> Once that stage completes, installation normally advances quickly to completion.

## SSH key behavior

`SSH_PUBLIC_KEY` must contain one valid OpenSSH public key. The workflow validates the key before starting the ISO build and refuses to continue if the secret is missing, malformed, contains multiple lines, or appears to contain private-key material.

The public key is safe to distribute inside your personalized installer ISO. Your **private key stays on your own computer** and is never required by this Builder.

### SSH after installation

- `ssh user@IP` works when the matching private key is available through `ssh-agent`, a normal default SSH identity, or SSH client configuration.
- If the private key has a custom filename and is not loaded into an agent, use `ssh -i /path/to/private-key user@IP`.
- After reinstalling a machine at the same IP, the client may need `ssh-keygen -R IP` because a fresh installation generates a new SSH host identity.

## Safety and current scope

Home Server Installer V1 is designed for UEFI systems. Secure Boot must be disabled during installation. VM testing is recommended first. For bare-metal testing, use a dedicated test drive or hardware where the selected installation disk can be safely erased, and keep backups of anything important.

The installer erases and repartitions only the disk selected in the installer UI.

## How the Builder works

When you run the build workflow, the Builder:

1. Resolves the **latest published release** of [Home Server Installer](https://github.com/home-server-project/home-server-installer).
2. Checks out that exact released tag.
3. Builds the released installer binary.
4. Builds a Fedora CoreOS-based Home Server Installer ISO.
5. Embeds the public key from your `SSH_PUBLIC_KEY` secret into the live installer handoff.
6. Produces the ISO, SHA256 checksum and build-information file.
7. Uploads the finished files as a GitHub Actions artifact retained for **1 day**.

The Builder deliberately follows the latest published Installer release rather than the Installer development branch. A new Installer version is used by the Builder only after it has been published as a GitHub Release.

## Upstream and references

<details>
<summary><strong>Project and upstream links</strong></summary>

- [Home Server Gina](https://github.com/home-server-project/home-server-gina)
- [Home Server Installer](https://github.com/home-server-project/home-server-installer)
- [Home Server Project](https://github.com/home-server-project)
- [Universal Blue uCore](https://github.com/ublue-os/ucore)
- [Fedora CoreOS](https://fedoraproject.org/coreos/)

</details>

## License

Apache-2.0. Software and tooling used by the build process retain their own upstream licenses.
