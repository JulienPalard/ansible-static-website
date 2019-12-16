# Nginx static website

This role sets up nginx with HTTPS using letsencrypt.

It tries to be minimal.


## Role Variables


- `domain`: Domain name of your website, like: `example.com`.
- `extra_certificates (default: `[]`)`: A list of extra certificate to generate, like `[www.example.com]`.
- `owner` (optional): Unix username for this website, like `example_com`.
- `path` (optional): Path for your static files, like: `/var/www/example.com/`.
- `public_deploy_key` (optional): A public SSH key, so you can push your static files via rsync.
- `nginx_extra` (default: empty string): Added at the end of the `server` block in nginx configuration, so you can add some locations or whatever.

## Example Playbook

```yaml
- hosts: static_websites
  tasks:
    - name: Setup mdk.fr
      include_role: name=static_website
      tags: always
      vars:
        domain: mdk.fr
        extra_certificates: [www.mdk.fr]
        owner: mdk_fr
        path: /var/www/mdk.fr/
        public_deploy_key: 'ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIC/8I1ecV8EutLc+Qx6Q8b2RhzXMl9n23LznNlw+MQtM deploy'
```

Or to simply create a 301:

```yaml
- name: Setup palard.fr
  include_role: name=static_website
  tags: always
  vars:
    domain: palard.fr
    extra_certificates: [julien.palard.fr, www.palard.fr]
    nginx_extra: "location / {return 301 https://mdk.fr;}"
```

Beware of using `include_role` and not `roles` so variables from a
website does not *leak* to others. It would not be nice if a random
public_deploy_key is given to all your site not having a deploy key.


### License

MIT


### Author Information

Julien Palard — https://mdk.fr
