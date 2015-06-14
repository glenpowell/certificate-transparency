# Introduction #

We welcome contributions from the community, be they bugfixes or new features.

# Contacting other developers #
There's a [Google group](https://groups.google.com/forum/#!forum/certificate-transparency) where general discussion on Certificate Transparency is taking place. It's also a good place to discuss modifications to the code base prior to submitting patches (new patches are posted to the list automatically).

# Submitting a patch #
By patch we mean an actual diff file or a pull request on GitHub.
All code committed goes through a code review: One of the committers will pick up your patch, review it and submit it once ready.

## General code guidelines ##
Following these guidelines will help us review your patch quickly and efficiently:
  * Separate clean-up changes from functional changes. Indentation changes, sorting import order, etc, should be separate patches.
  * Break large changes into small, functional chunks. Each patch should stand by itself. This will help shorten review time (by not blocking submission of one change on discussion regarding another change).
  * If using GitHub, make sure each commit in the pull request is substantial (to avoid cluttering the repository log with intermediate development steps).
  * Add tests for new functionality.

## Coding style ##
We aim to follow Google coding style guides, so if your patch is more than a few lines long, you may find these resources helpful in getting it accepted faster:

C++ style guide:
http://google-styleguide.googlecode.com/svn/trunk/cppguide.xml

Python style guide:
http://google-styleguide.googlecode.com/svn/trunk/pyguide.html

Java style guide:
http://google-styleguide.googlecode.com/svn/trunk/javaguide.html

## Sending the patch ##
The most straightforward way is to send a GitHub pull request with your changes. The code review will take place on the GitHub pull request.

For large changes, it is recommended to use Rietveld (https://codereview.appspot.com/) for code reviews. Sure, it looks a bit dated, but it's more convenient for large code reviews than GitHub's current review model (as of May 2014).