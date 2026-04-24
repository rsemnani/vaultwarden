# Install Vaultwarden

## Guide: Deploying Vaultwarden with Cloudflare Tunnel

In this guide, we will walk you through the process of deploying Vaultwarden, a self-hosted password manager, using a Cloudflare Tunnel. Cloudflare Tunnels allow you to securely expose your local web server to the internet without the need for port forwarding or exposing your server's IP address (Zero Trust).

### Prerequisites

Before you begin, make sure you have the following:

1. A Cloudflare account: Sign up for a free account at [cloudflare.com](https://www.cloudflare.com).

2. A [cloudflare tunnel token](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/get-started/create-remote-tunnel/).

3. Debian, Ubuntu, or Raspbian with argon2 installed.

    ```bash
    sudo apt update && sudo apt install argon2 -y
    ```

4. Docker and Docker Compose

   ```bash
   sudo apt install docker.io docker-compose -y
   ```

### Installation

1. Copy the `vaultwarden.env.template` file and make a new file named `vaultwarden.env`. Update the configuration values to match your needs.

2. In `vaultwarden.env`, to enable the admin page, you need to set an authentication token. After this, the page will be available at `/admin`. Create the password hash and update the ADMIN_TOKEN value in `vaultwarden.env`.

    ```bash
    echo -n "MySecretPassword" | argon2 "$(openssl rand -base64 32)" -e -id -k 65540 -t 3 -p 4
    ```

3. In `vaultwarden.env`, Both the mysql and mysql root passwords need to be URL encoded. You can run this command to encode the passwords:

    ```bash
    MYSQL_PASSWORD='your_db_password'
    echo "$MYSQL_PASSWORD" | sed -e 's/%/%25/g' -e 's/ /%20/g' -e 's/!/%21/g' -e 's/"/%22/g' -e 's/#/%23/g' -e 's/\$/%24/g' -e 's/&/%26/g' -e "s/'/%27/g" -e 's/(/%28/g' -e 's/)/%29/g' -e 's/\*/%2A/g' -e 's/+/%2B/g' -e 's/,/%2C/g' -e 's/\//%2F/g' -e 's/:/%3A/g' -e 's/;/%3B/g' -e 's/</%3C/g' -e 's/=/%3D/g' -e 's/>/%3E/g' -e 's/?/%3F/g' -e 's/@/%40/g' -e 's/\[/%5B/g' -e 's/\\/%5C/g' -e 's/\]/%5D/g' -e 's/\^/%5E/g' -e 's/_/%5F/g' -e 's/`/%60/g' -e 's/{/%7B/g' -e 's/|/%7C/g' -e 's/}/%7D/g' -e 's/~/%7E/g'
    ```

4. Start the project using docker compose. Run these commands from the project folder.

    ```bash
    docker compose up -d
    ```

    To stop:

    ```bash
    docker compose down
    ```
