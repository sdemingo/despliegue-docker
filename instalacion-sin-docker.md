# Installation from binary

All downloads come with SQLite, MySQL and PostgreSQL support, and are built with
embedded assets. This can be different from Gogs.

## Download

You can find the file matching your platform from the [downloads page](https://dl.gitea.com/gitea/) after navigating to the version you want to download.

### Choosing the right file

**For Linux**, you will likely want `linux-amd64`. It's for 64-bit Intel/AMD platforms, but there are other platforms available, including `arm64` (e.g. Raspberry PI 4), `386` (i.e. 32-bit), `arm-5`, and `arm-6`.

**For Windows**, you will likely want `windows-4.0-amd64`. It's for all modern versions of Windows, but there is also a `386` platform available designed for older, 32-bit versions of Windows.

*Note: there is also a `gogit-windows` file available that was created to help with some [performance problems](https://github.com/go-gitea/gitea/pull/15482) reported by some Windows users on older systems/versions. You should consider using this file if you're experiencing performance issues, and let us know if it improves performance.*

**For macOS**, you should choose `darwin-arm64` if your hardware uses Apple Silicon, or `darwin-amd64` for Intel.

**For FreeBSD**, you should choose `freebsd12-amd64` for 64-bit Intel/AMD platforms.

### Downloading with wget

Copy the commands below and replace the URL within the one you wish to download.

```sh
wget -O gitea https://dl.gitea.com/gitea/@version@/gitea-@version@-linux-amd64
chmod +x gitea
```

Note that the above command will download Gitea @version@ for 64-bit Linux.

## Verify GPG signature

Gitea signs all binaries with a [GPG key](https://keys.openpgp.org/search?q=teabot%40gitea.io) to prevent against unwanted modification of binaries.
To validate the binary, download the signature file which ends in `.asc` for the binary you downloaded and use the GPG command line tool.

```sh
gpg --keyserver keys.openpgp.org --recv 7C9E68152594688862D62AF62D9AE806EC1592E2
gpg --verify gitea-@version@-linux-amd64.asc gitea-@version@-linux-amd64
```

Look for the text `Good signature from "Teabot <teabot@gitea.io>"` to assert a good binary,
despite warnings like `This key is not certified with a trusted signature!`.

## Recommended server configuration

**NOTE:** Many of the following directories can be configured using [Environment Variables](administration/environment-variables.md) as well!
Of note, configuring `GITEA_WORK_DIR` will tell Gitea where to base its working directory, as well as ease installation.

### Prepare environment

Check that Git is installed on the server. If it is not, install it first. Gitea requires Git version >= 2.0.

```sh
git --version
```

Create a user to run Gitea (e.g. `git`)

```sh
# On Ubuntu/Debian:
adduser \
   --system \
   --shell /bin/bash \
   --gecos 'Git Version Control' \
   --group \
   --disabled-password \
   --home /home/git \
   git

# On Fedora/RHEL/CentOS:
groupadd --system git
adduser \
   --system \
   --shell /bin/bash \
   --comment 'Git Version Control' \
   --gid git \
   --home-dir /home/git \
   --create-home \
   git
```

### Create required directory structure

```sh
mkdir -p /var/lib/gitea/{custom,data,log}
chown -R git:git /var/lib/gitea/
chmod -R 750 /var/lib/gitea/
mkdir /etc/gitea
chown root:git /etc/gitea
chmod 770 /etc/gitea
```

> **NOTE:** `/etc/gitea` is temporarily set with write permissions for user `git` so that the web installer can write the configuration file. After the installation is finished, it is recommended to set permissions to read-only using:
>
> ```sh
> chmod 750 /etc/gitea
> chmod 640 /etc/gitea/app.ini
> ```

If you don't want the web installer to be able to write to the config file, it is possible to make the config file read-only for the Gitea user (owner/group `root:git`, mode `0640`) however you will need to edit your config file manually to:

* Set `INSTALL_LOCK= true`,
* Ensure all database configuration details are set correctly
* Ensure that the `SECRET_KEY` and `INTERNAL_TOKEN` values are set. (You may want to use the `gitea generate secret` to generate these secret keys.)
* Ensure that any other secret keys you need are set.

See the [command line documentation](administration/command-line.md) for information on using `gitea generate secret`.

### Configure Gitea's working directory

**NOTE:** If you plan on running Gitea as a Linux service, you can skip this step, as the service file allows you to set `WorkingDirectory`. Otherwise, consider setting this environment variable (semi-)permanently so that Gitea consistently uses the correct working directory.

```sh
export GITEA_WORK_DIR=/var/lib/gitea/
```

### Copy the Gitea binary to a global location

```sh
cp gitea /usr/local/bin/gitea
```

### Adding bash/zsh autocompletion (from 1.19)

A script to enable bash-completion can be found at [`contrib/autocompletion/bash_autocomplete`](https://raw.githubusercontent.com/go-gitea/gitea/main/contrib/autocompletion/bash_autocomplete). This can be copied to `/usr/share/bash-completion/completions/gitea`
or sourced within your `.bashrc`.

Similarly a script for zsh-completion can be found at [`contrib/autocompletion/zsh_autocomplete`](https://raw.githubusercontent.com/go-gitea/gitea/main/contrib/autocompletion/zsh_autocomplete). This can be copied to `/usr/share/zsh/_gitea` or sourced within your
`.zshrc`.

YMMV and these scripts may need further improvement.

## Running Gitea

After you complete the above steps, you can run Gitea two ways:

### 1. Creating a service file to start Gitea automatically (recommended)

See how to create [Linux service](installation/run-as-service-in-ubuntu.md)

### 2. Running from command-line/terminal

```sh
GITEA_WORK_DIR=/var/lib/gitea/ /usr/local/bin/gitea web -c /etc/gitea/app.ini
```


