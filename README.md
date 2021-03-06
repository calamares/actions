<!--
    SPDX-License-Identifier: BSD-2-Clause
    SPDX-FileCopyrightText: 2021 Adriaan de Groot <groot@kde.org>
-->

# Calamares Actions

> This repository holds GitHub actions used by the **other** Calamares
> repositories (e.g. Calamares itself, and -extensions) for bits
> and pieces of their workflows.


## Generic-Build

GitHub action with a generic build sequence for Calamares repositories.
Once the source is set up, `cmake;make;make install` is the general
approach, so this action does that.

In passing, it sets one output:

- *git-summary*:
  The one-line git summary (first line of commit message plus commit hash)
  that the build starts with.

  
## Matrix-Notify

GitHub action for notifications on Matrix. This action is
a simplified and cut-down version of other notifiers;
it requires `curl` and `jq` in the runner.

Parameters for the action (see also `matrix-notify/action.yml`):

- *room*:
    Matrix Room ID; this is the internal ID, not the room name. Should be secret.
- *token*:
    Matrix Token; this is obtained by logging in. Should be secret.
- *message*:
    Message to send; generally multi-line. This is sent as a plain-text message,
    so no formatting, markdown-handling or anything else is done.
    
An example of use is the following snippet in a workflow step:

```
uses: calamares/actions/matrix-notify@49c1a968ca0fcb46e592743d1a5251326f1a9bc7
with:
    token: ${{ secrets.MATRIX_TOKEN }}
    room: ${{ secrets.MATRIX_ROOM }}
    message: |
    OK ${{ github.workflow }} in ${{ github.repository }}
    .. is done now.
```

Note the use of `${{ secrets. }}` to pass in secrets for the action.


## Prepare-*

The various `prepare-*` actions setup and configure the host for
building Calamares; these actions should run before `generic-build`.
Pick the one that matches the host container.
