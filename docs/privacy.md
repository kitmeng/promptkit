# Privacy Policy for prompt.kit

Last updated: 18 November 2026.

prompt.kit is a browser extension maintained by Kit Meng for saving, organising, and inserting reusable prompts. This policy covers the library, insertion, backup, recovery, and optional GitHub sync features. Storage details below distinguish the current v0.10.1 build from older installations where relevant.

## Information stored on your device

prompt.kit stores your prompts, descriptions, categories, shortcuts, library preferences, and recovery copies in your browser profile. Older versions may also retain variable metadata and onboarding state. It also stores local prompt-use counts and last-used times to support the library experience. Prompts may contain personal information you choose to save.

prompt.kit does not operate a developer-hosted service to receive your library. It does not include advertising, third-party analytics, or remote crash reporting. Local prompt-use counts and last-used times are not included in the GitHub sync library.

## Website access and insertion

prompt.kit runs on websites to recognise shortcuts in supported text fields and insert a selected prompt. It processes editable text locally for these features. Supported fields include text and search inputs, text areas, and rich-text editors. Password-type inputs are excluded; do not save passwords or other secrets in prompts.

Surrounding website text and browsing history are not included in GitHub sync uploads. When you insert a prompt into a website, that website can access the inserted text and handles it under its own policies. prompt.kit does not control what the receiving website does with that text.

## Optional GitHub sync

You can use the local library without connecting GitHub. If you enable sync, prompt.kit communicates with GitHub to authenticate you, identify repositories available to the app, and read and write the library in the repository you select. GitHub receives network information such as your IP address when the extension makes these requests.

The synced library contains prompt content and metadata, categories, and deletion records used to reconcile changes across installations. It is stored as `prompt-kit/library.json`. GitHub sign-in tokens and connection state are stored locally in extension storage and are not included in this library file. The v0.10.1 build holds credentials and connection state in session storage, which the browser clears when the browser session ends. Older versions may retain connection state between sessions. Tokens are sent to GitHub as needed for authentication and API access.

The GitHub App, `promptkit-app`, can read and write repository contents in repositories you authorise. Select only repositories you intend to use for sync. Repository access is governed by your GitHub settings.

A public repository makes its library contents and accessible Git history readable by anyone. A private repository restricts access according to GitHub permissions. Sync is not end-to-end encrypted. GitHub processes information under its [privacy statement](https://docs.github.com/en/site-policy/privacy-policies/github-general-privacy-statement).

## Sharing and use

prompt.kit uses data to provide the library, insertion, import/export, recovery, and optional sync features. The maintainer does not sell extension user data, use it for advertising, or use it to determine creditworthiness. If you enable sync, your library is transferred to GitHub; if your repository is public, other people can read and copy it. Exported files are stored wherever you choose to save or share them.

## Retention and deletion

Local data remains in the browser profile until you change or remove it. You can edit and delete prompts and export your library. Deletion records and recovery copies may remain after a prompt is removed from the active library.

Disconnecting sync removes the extension’s saved connection state and credentials on that device. It does not delete your local library, exported files, or GitHub repository and commit history. You can revoke the app’s authorisation in [GitHub settings](https://github.com/settings/applications), manage installation access in [installation settings](https://github.com/settings/installations), and remove extension data using your browser’s extension controls.

Deleting a prompt from the active library does not erase earlier GitHub commits or copies other people have made. To remove remotely stored information, manage the repository and its history on GitHub, following GitHub’s deletion documentation. GitHub’s retention practices are governed by its policies.

## This policy website

This policy is published in the public prompt.kit repository on GitHub. GitHub may process visitor information under its privacy statement. This policy page does not add analytics or advertising scripts.

## Changes

This page will be updated when prompt.kit’s data practices change. The date above identifies the latest revision.

## Contact

For privacy questions, contact Kit Meng at [kidm3ng@gmail.com](mailto:kidm3ng@gmail.com).
