# basic-dns-setup

this is a solution for the project located at https://roadmap.sh/projects/basic-dns

it references the previously completed projects located here

https://github.com/its-mr-gray/gh-deployment-workflow
https://github.com/its-mr-gray/static-site-server

i purchased a domain from cloudflare. the domain is www.barbaricbarbell.com and if you navigate to this site, you will see the previously mentioned project's HTML output.

this was accomplished by going to the gh-deployment-workflow repo, opening settings -> pages, and setting a custom domain to barbaricbarbell.com and saving. then, in the backend of my cloudflare domain, i added multiple server records to the DNS. 

| Type  | Name | Content / Target      | Notes                |
| ----- | ---- | --------------------- | -------------------- |
| A     | @    | 185.199.108.153       | GitHub Pages IP      |
| A     | @    | 185.199.109.153       | GitHub Pages IP      |
| A     | @    | 185.199.110.153       | GitHub Pages IP      |
| A     | @    | 185.199.111.153       | GitHub Pages IP      |
| CNAME | www  | its-mr-gray.github.io | Points www to GitHub |


this caused the domain to redirect to the github pages site.


i then needed to go to my AWS EC2 instance that was created for the static-site-server project to update some of the security/IP settings.

i had to add an elastic IP to the instance. this allows the IP to remain stable and unchanged so that the domain can reference it without the redirect erroring. i then added another record to the domain's DNS

| Type | Name   | Content              | Notes                   |
| ---- | ------ | -------------------- | ----------------------- |
| A    | server | <Elastic-IP-address> | Points subdomain to EC2 |

i then had to update the security group on my EC2 instance. i needed to add an inbound rule to allow HTTP traffic on port 80 from `0.0.0.0/0` (not recommended for production, but for the purposes of this project, it is fine).

the result of this was that server.barbaricbarbell.com serves the previously created static site. 
