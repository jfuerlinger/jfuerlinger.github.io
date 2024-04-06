# My personal blog

This blog describes my (sometimes very rocky) journey on the way to implementing my Homelab. I don't have any preferences, but rather put it together part by part and draw inspiration from various sources.

I describe the individual steps to reach my goal and also try to document the problems I encountered along the way, including the solution, as best I can. This serves as a reference for me, but also for anyone working on similar projects.

## Recipes

- [Use Docker context command to control a remote docker daemon](./docker/docker-context.md)
- [Add `non root` user on docker host and enable ssh access](./docker/add-nonroot-user.md)
- [Use of scripted docker-compose.yml files disaster safety](./docker/docker-disaster-safety.md)

## Ideas for the future

- Use of scripted docker-compose.yml files to be able to restore the entire Docker container in the event of a disaster
- Automate backup and restore
- Use watchtower to auto update the docker containers
- Use pihole as a DNS server and also as ad blocker
- Use traefik as a reverse proxy
- Use authentik for user authentication
- Host a bitwarden instance
