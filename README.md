# My personal blog

This blog describes my (sometimes very rocky) journey on the way to implementing my Homelab. I don't have any preferences, but rather put it together part by part and draw inspiration from various sources.

I describe the individual steps to reach my goal and also try to document the problems I encountered along the way, including the solution, as best I can. This serves as a reference for me, but also for anyone working on similar projects.

## Recipes

- [Use Docker context command to control a remote docker daemon](./docker/docker-context.md)
- [Add `non root` user on docker host and enable ssh access](./docker/add-nonroot-user.md)
- [Use of scripted docker-compose.yml files disaster safety](./docker/docker-disaster-safety.md)
- [SSH TCP Forwarding to access services from the remote servers network](/ssh/ssh-tcp-forwarding.md)

## Ideas for the future

- Disable sudo password for specific user (https://askubuntu.com/questions/147241/execute-sudo-without-password)
- Share the hosts ssh key with the dev container
  - ssh-add -l
  - https://code.visualstudio.com/remote/advancedcontainers/sharing-git-credentials
  
    ```bash
        # Make sure you're running as an Administrator
        Set-Service ssh-agent -StartupType Automatic
        Start-Service ssh-agent
        Get-Service ssh-agent
    ```
- Use of scripted docker-compose.yml files to be able to restore the entire Docker container in the event of a disaster
- Automate backup and restore
- Use watchtower to auto update the docker containers
- Use pihole as a DNS server and also as ad blocker
- Use traefik as a reverse proxy
- Use authentik for user authentication
- Host a bitwarden instance
