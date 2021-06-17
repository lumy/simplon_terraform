
# Client

You just got a presentation of a new product, your boss want you to test if it's a viable solution.
You will have to design and setup your solution.


## Infrastructure

  - a `load_balancer` that will serve two webserver.
  - `webserver`: two WebServer that run a simple blog.
  - `jenkins`: a jenkins server.

## Configuration

  - All server should have the same user.
  - All server should have the same authorized_keys.
  - all server should have a fail2ban installed.
  - protected the domain name with let'sencrypt (certbot for debian based) you can use any method you which.
  - install a cron to renew, shutting webserver(nginx/apache) is authorized during this operations.
  - all server should respond to a sub domain name.
  - `/blog` should load-balance between both `webserver` and `/jenkins` should proxy `jenkins`.
  - `jenkins` should have two jobs, one that successfully compile your blog, the other that fail
    (doing whatever you wanna him to fail).

You already know ansible, and you just discovered terraform, but not totally.
Hint from your boss: https://www.terraform.io/docs/language/resources/provisioners/syntax.html


# Expected

You should do 1 private repository on github.

## Part 1: Terraform

In the repository a folder named `terraform` containing all terraform file and tfstate.

## Part 2: configuration

in the repository a folder named `ansible` containing all yaml, templates, inventory.

## Part 3: Jenkins

in the repository a folder named `jenkins` containing all pipeline (can be at root directory or in
`ansible` folder).
