---
lastmod: 2019-03-11
date: 2016-07-01
title: Verifying packages (PGP and SHA256)
notoc: true
---
How to make sure what you are downloading is legit.

## PGP
We will check the detached signature [PGP]({{< param download_repo >}}/bin/krakend_{{< product latest_version >}}_amd64.tar.gz.asc) against our package [KrakenD]({{< param download_repo >}}/bin/krakend_{{< product latest_version >}}_amd64.tar.gz).

{{< terminal title="Term" >}}
gpg --verify krakend_{{< product latest_version >}}_amd64.tar.gz.asc krakend_{{< product latest_version >}}_amd64.tar.gz
gpg: Signature made Sun Mar 10 18:17:18 2019 UTC using RSA key ID {{< param pgp_key >}}
gpg: Can't check signature: public key not found

{{< /terminal >}}


We don't have the packager public key (AB39BEA1) in our system. You need to retrieve the public key from a key server.

{{< terminal title="Term" >}}
gpg --keyserver keyserver.ubuntu.com --recv-key {{< param pgp_key >}}
gpg: requesting key {{< param pgp_key >}} from hkp server keyserver.ubuntu.com
gpg: trustdb created
gpg: key {{< param pgp_key >}}: public key "Devops Faith Package Manager <packages@devops.faith>" imported
gpg: Total number processed: 1
gpg: imported: 1	(RSA: 1)
{{< /terminal >}}

Now you can verify the signature of the package:

{{< terminal title="Term" >}}
gpg --verify krakend_{{< product latest_version >}}_amd64.tar.gz.asc krakend_{{< product latest_version >}}_amd64.tar.gz
gpg: Signature made Sun Mar 10 18:17:18 2019 UTC using RSA key ID {{< param pgp_key >}}
gpg: Good signature from "Devops Faith Package Manager <packages@devops.faith>"
gpg: WARNING: This key is not certified with a trusted signature!
gpg:					There is no indication that the signature belongs to the owner.
Primary key fingerprint: {{< param pgp_fingerprint >}}
{{< /terminal >}}


## SHA256

To make sure the binary downloaded matches our SHA256 ensure the next 2 commands produce the same [SHA]({{< param download_repo >}}/bin/krakend_{{< product latest_version >}}_amd64.tar.gz.sha256) output.

{{< terminal title="Term" >}}
shasum -a 256 -b krakend_{{< product latest_version >}}_amd64.tar.gz
{{< /terminal >}}

Compare it to:
{{< terminal title="Term" >}}
curl {{< param download_repo >}}/bin/krakend_{{< product latest_version >}}_amd64.tar.gz.sha256
{{< /terminal >}}