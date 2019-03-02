# SMTP to SES Mail Proxy

This is a tiny little proxy that speaks unauthenticated SMTP on the front side
and makes calls to the SES
[SendRawEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendRawEmail.html)
on the back side.

Everything this software does is possible with a more fully-featured mail
server like Postfix but requires setting up Postfix (which is complicated) and,
if following best practices, rotating credentials every 90 days (which is
annoying). Because this integrates with the AWS SDK it can be configured
through the normal SDK configuration channels such as the instance metadata
service which provides dynamic credentials or environment variables, in which
case you should still manually rotate credentials but have one choke-point to
do that.

## Usage
By default the command takes no arguments and will listen on port 2500 on all
interfaces. The listen interfaces and port can be specified as the only
argument separated with a colon like so:

```
./ses-smtpd-proxy 127.0.0.1:2600
```

## Security Warning
This server speaks plain unauthenticated SMTP (no TLS) so it's not suitable for
use in an untrusted environment nor on the public internet. I don't have these
use-cases but I would accept pull requests implementing these features if you
do have the use-case and want to add them.

## Building
To build the binary run `make ses-smtpd-proxy`. 

To build a Docker image, which is based on Alpine Latest, run `make docker` or
`make publish`. The later command will build and push the image. To override
the defaults specify `DOCKER_REGISTRY`, `DOCKER_IMAGE_NAME`, and `DOCKER_TAG`
in the make command like so:

```
make DOCKER_REGISTRY=reg.example.com DOCKER_IMAGE_NAME=ses-proxy DOCKER_TAG=foo docker
```
## Contributing
If you would like to contribute please visit the project's GitHub page and open
a pull request with your changes. To have the best experience contributing,
please:

* Don't break backwards compatibility of public interfaces
* Update the readme, if necessary
* Follow the coding style of the current code-base
* Ensure that your code is formatted by gofmt
* Validate that your changes work with Go 1.11+

All code is reviewed before acceptance and changes may be requested to better
follow the conventions of the existing API.

## Contributors
* Mike Crute (@mcrute)
