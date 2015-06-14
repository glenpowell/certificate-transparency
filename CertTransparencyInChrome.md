# Introduction #
Certificate Transparency support in Chrome is an on-going effort of the team. There are several steps, the first one (already completed) is to verify SCTs that are served together with SSL certificates.
Next ones are fetching inclusion proofs from logs and gossiping.

# Status #
(Dec 18th, 2013): CT support is now available in Chrome's dev (unstable) channel. Additionally, there's a build with an SCT viewer, see details below.

(Dec 9th, 2013): CT support is available in Chromium's tree. The next developer build of Chrome should contain it.

Chrome's dev channel now contains preliminary CT support. For an easier way to view Signed Certificate Timestamps, you can get a build of Chromium with an SCT viewer:
[Linux-64bit](https://drive.google.com/file/d/0B1zVGBrl1bKdZlMzYkEyWmVLOGc/edit?usp=sharing)

## Current features ##
  * Verification of a multiple SCTs, either embedded in the certificate or obtained via CT's TLS extension or OCSP response.
  * Verifies against Google's CT logs, an additional log can be specified on the command line.
  * Indicates one of the following:
    * There are no SCTs for this certificate.
    * All SCTs for this certificate come from unknown logs.
    * At least one SCT for this certificate comes from a known log, but the SCT could not be validated.
    * At least one SCT for this certificate comes from a known log and is valid.
  * Capturing in Chrome's NetLog of the raw SCTLists and the parsed SCTs with their validation status.

## Not yet implemented ##
  * SCT Viewer for Windows/ChromeOS: in-progress
  * SCT Viewer for Linux: will be available once Chromium switches to Aura for Linux.
  * SCT Viewer for Mac: not started

## How to install ##
These instructions pertain to the downloaded tarball. If you've installed Chrome's dev channel, skip to the next section.

After unpacking the tarball, then make sure the `chrome-sandbox` binary is owned by `root.root` and `chmod 4755 chrome-sandbox`.

## How to use ##
### Supplying your own log key ###
By default, Chrome will check SCTs coming from Google's CT logs. The next step is only necessary if you wish to get Chrome to check an SCT issued by your own log.
If so, launch the binary with the following command:
```
./chrome --certificate-transparency-log="Google_Pilot_Log:MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEmXg8sUUzwBYaWrRb+V0IopzQ6o3UyEJ04r5ZrRXGdpYM8K+hB0pXrGRLI0eeWz+3skXrS0IO83AhA3GpRL6s6w=="  --user-data-dir="/tmp/chromeprofile"
```
The value of the `certificate-transparency-log` parameter is colon-delimited, starting with a log description, followed by a base64-encoded log public key. In this example this is Google's pilot log key.
Note that the `user-data-dir` parameter is optional (but would avoid messing your regular Chrome profile).

### UI indicator ###
Use Chrome to access a site which serves a certificate with an embedded SCT or serves SCTs during the TLS handshake:
  * https://embed.ct.digicert.com has a certificate with an embedded SCT.
  * The EmbeddedTestCertificates page explains how to generate your own test enviornment.

The site identity string in the Connection tab has changed to reflect the CT status of the site, as detailed on the [Chrome](https://sites.google.com/site/certificatetransparency/certificate-transparency-in-chrome) page.

### SCTs in the NetLog ###
See the [Chrome](https://sites.google.com/site/certificatetransparency/certificate-transparency-in-chrome) page for details on how to access and parse information from the net log.

### SCT Viewer ###
Note that this UI is a work-in-progress and is bound to change. Comments on how to improve it are welcome, patches even more so.

The SCT Viewer is available in the Connection information dialogue, next to the link to the Certificate Viewer. The link to open it will only appear if the related certificate has any SCTs.
The `--enable-sct-viewer` flag must be provided in Chrome's command line to enable the SCT viewer.