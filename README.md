This is a container that does Wordpress and Docker _the right way._

## What's wrong with Wordpress's Docker image?

The container shipped by Wordpress copies over the contents of `/usr/src/wordpress` to `/var/www/html` when the container is first created, but only if no content already exists in `/var/www/html`. This means that if you've already deployed the container and have a persistent volume mounted at that location, you can upgrade your container from `4.7.4` to `4.8.1`, and although it claims to be `4.8.1`, nothing happens. You're still running `4.7.4`. If you want to upgrade, you have to do it manually, and if you're doing it manually, what's the point of upgrading the container?

## What's different about this image?

1. Wordpress core and Wordpress content are separate
2. Wordpress is installed as a submodule at build time, so we're always pulling from their repository (and not copying it needlessly)
3. Wordpress knows how to operate with content in a different directory
4. Wordpress knows how to operate as a submodule

This means that you can initialize your container, build your site, deploy it, and when Wordpress releases a new version, simply _upgrade the image_ of your container and re-launch it. 

### Additional Rancher Benefits

If you run this with [Rancher](https://www.rancher.com), you get extra benefits:

- If you mount an NFS volume as the shared content, you can scale your container and easily gain fault tolerance
- When you upgrade, you will be given the ability to test your upgrade and optionally rollback if something goes wrong.
- The image supports Rancher Secrets through `WORDPRESS_DB_PASSWORD_FILE`

## Deployment Instructions

See [INSTALL.md](INSTALL.md) for installation instructions.
